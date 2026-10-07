---
name: backend-node-express-mysql
description: Arquitectura Backend en Node.js (ES Modules), Express y MySQL 8+ con Sequelize ORM transaccional para el Sistema de Facturación ARCA/AFIP. Usar al diseñar o implementar modelos Sequelize, controladores atómicos, servicios de negocio, repositorios con transacciones ACID, bloqueo de secuencia de comprobantes y caché persistente de tickets WSAA.
---

# Backend Node.js + Express + MySQL + Sequelize ORM (Arquitectura Fiscal ARCA)

## Principios y Mejores Prácticas

### 1. Regla de Oro (según `docs/constitution.md` y `backend_SOUL.md`)
- **Arquitectura en Capas Estricta con Sequelize (`workspace/backend/src/`)**:
  - `config/`: Instancia de `Sequelize` conectada a MySQL 8+ (`sequelize` + `mysql2`), validación de variables de entorno y rutas de certificados `.pem`/`.key`.
  - `models/`: Definición de modelos y relaciones de **Sequelize** (`Voucher`, `VoucherItem`, `VoucherVat`, `Client`, `Product`, `ArcaToken`, `PointOfSaleSequence`).
  - `repositories/`: Interacción exclusiva con los modelos de Sequelize y manejo de transacciones ACID (`sequelize.transaction()`).
  - `services/`: Lógica de negocio pura, orquestación de `wsaaService`, `wsfeService`, validaciones fiscales y cálculo de cuadratura de importes.
  - `controllers/`: Controladores **atómicos** (un archivo por endpoint, ej. `createInvoice.js`, `checkArcaStatus.js`) exportados con `export default function`. Prohibido importar modelos de Sequelize o armar XML SOAP directamente dentro de un controlador.
- **Límites de Clean Code**:
  - Máximo **~40 líneas por función** y **100 líneas por archivo**.
  - Sin punto y coma (`;`) al final de las sentencias (ASI).

### 2. Patrones Recomendados con Sequelize

#### A. Transacciones ACID y Bloqueo de Secuencia (`LOCK.UPDATE`) en Sequelize
Para evitar que dos terminales POS envíen el mismo `CbteDesde`/`CbteHasta` a ARCA simultáneamente, utilizar transacciones administradas de Sequelize con bloqueo pesimista (`t.LOCK.UPDATE`):
```javascript
import sequelize from '../config/database.js'
import PointOfSaleSequence from '../models/PointOfSaleSequence.js'

export default async function reserveNextVoucherNumber(ptoVta, cbteTipo, arcaLastNumber) {
  return await sequelize.transaction(async (t) => {
    const sequence = await PointOfSaleSequence.findOne({
      where: { ptoVta, cbteTipo },
      lock: t.LOCK.UPDATE,
      transaction: t
    })

    const localLast = sequence ? sequence.ultimoNro : 0
    const nextNumber = Math.max(localLast, arcaLastNumber) + 1

    if (sequence) {
      await sequence.update({ ultimoNro: nextNumber }, { transaction: t })
    } else {
      await PointOfSaleSequence.create({ ptoVta, cbteTipo, ultimoNro: nextNumber }, { transaction: t })
    }

    return nextNumber
  })
}
```

#### B. Máquina de Estados del Comprobante en Modelo `Voucher`
Toda venta fiscal debe transicionar por estados auditables en el modelo Sequelize `Voucher` (`ENUM`):
1. `PENDIENTE_ENVIO`: Creado en MySQL vía Sequelize antes de llamar a `FECAESolicitar`.
2. `TIMEOUT_PENDIENTE_CONSULTA`: Si la petición HTTP a ARCA sufre timeout o corte de red. Bloquea nuevos envíos hasta ejecutar `FECompConsultar`.
3. `APROBADO`: CAE y fecha de vencimiento recibidos y persistidos de forma inmutable.
4. `RECHAZADO`: Rechazado por ARCA (`Resultado = 'R'`), guardando código y descripción de `<Errors>` / `<Observaciones>`.

#### C. Caché Persistente del Ticket de Acceso (Modelo `ArcaToken`)
Persistir `<token>`, `<sign>` y `<expirationTime>` usando un modelo Sequelize `ArcaToken` (clave única por `cuit`, `service` y `environment`) para que el backend sobreviva a reinicios de Node.js sin volver a llamar a `loginCms` durante las 12 horas de vigencia.

### 3. Lo que NO se debe hacer
- **NUNCA** invocar métodos de modelos Sequelize (`Voucher.findAll`, `Voucher.create`) directamente desde `controllers/`; toda llamada a Sequelize debe pasar por `repositories/`.
- **NUNCA** usar `FLOAT` o `DOUBLE` en definiciones de columnas Sequelize para importes monetarios; usar siempre `DataTypes.DECIMAL(15, 2)` para evitar errores de redondeo de punto flotante en la cuadratura fiscal de ARCA.
- **NUNCA** eliminar físicamente (`destroy`) un comprobante que ya obtuvo CAE en ARCA; las anulaciones fiscales se realizan emitiendo una **Nota de Crédito** vinculada mediante `<CbtesAsoc>`.
