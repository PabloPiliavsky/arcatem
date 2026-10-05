---
name: backend-node-express-mysql
description: Arquitectura Backend en Node.js (ES Modules), Express y MySQL 8+ transaccional para el Sistema de Facturación ARCA/AFIP. Usar al diseñar o implementar controladores atómicos, servicios de negocio, repositorios MySQL con transacciones ACID, bloqueo de secuencia de comprobantes y caché persistente de tickets WSAA.
---

# Backend Node.js + Express + MySQL (Arquitectura Fiscal ARCA)

## Principios y Mejores Prácticas

### 1. Regla de Oro (según `docs/constitution.md` y `backend_SOUL.md`)
- **Arquitectura en Capas Estricta (`workspace/backend/src/`)**:
  - `config/`: Pool de conexiones `mysql2/promise`, validación de variables de entorno y rutas de certificados `.pem`/`.key`.
  - `controllers/`: Controladores **atómicos** (un archivo por endpoint, ej. `createInvoice.js`, `checkArcaStatus.js`) exportados con `export default function`. Prohibido escribir SQL o lógica SOAP dentro de un controlador.
  - `services/`: Lógica de negocio, orquestación de `wsaaService`, `wsfeService`, validaciones fiscales y cálculo de cuadratura de importes.
  - `repositories/`: Consultas SQL parametrizadas con `mysql2/promise` y manejo de transacciones ACID.
  - `models/`: Definición de esquemas y migraciones SQL.
- **Límites de Clean Code**:
  - Máximo **~40 líneas por función** y **100 líneas por archivo**.
  - Sin punto y coma (`;`) al final de las sentencias (ASI).

### 2. Patrones Recomendados

#### A. Transacciones ACID y Bloqueo de Secuencia (`FOR UPDATE`) en MySQL
Para evitar que dos terminales POS envíen el mismo `CbteDesde`/`CbteHasta` a ARCA al mismo tiempo, sincronizar con bloqueo de fila en MySQL:
```javascript
export default async function reserveNextVoucherNumber(connection, ptoVta, cbteTipo, arcaLastNumber) {
  const [rows] = await connection.execute(
    'SELECT ultimo_nro FROM puntos_venta_secuencias WHERE pto_vta = ? AND cbte_tipo = ? FOR UPDATE',
    [ptoVta, cbteTipo]
  )
  const localLast = rows[0]?.ultimo_nro ?? 0
  const nextNumber = Math.max(localLast, arcaLastNumber) + 1

  await connection.execute(
    'UPDATE puntos_venta_secuencias SET ultimo_nro = ? WHERE pto_vta = ? AND cbte_tipo = ?',
    [nextNumber, ptoVta, cbteTipo]
  )
  return nextNumber
}
```

#### B. Máquina de Estados del Comprobante en Base de Datos
Toda venta fiscal debe transicionar por estados auditables en la tabla `comprobantes`:
1. `PENDIENTE_ENVIO`: Creado localmente antes de llamar a `FECAESolicitar`.
2. `TIMEOUT_PENDIENTE_CONSULTA`: Si la petición HTTP a ARCA sufre timeout o corte de red. Bloquea nuevos envíos hasta ejecutar `FECompConsultar`.
3. `APROBADO`: CAE y fecha de vencimiento recibidos y persistidos de forma inmutable.
4. `RECHAZADO`: Rechazado por ARCA (`Resultado = 'R'`), guardando código y descripción de `<Errors>` / `<Observaciones>`.

#### C. Caché Persistente del Ticket de Acceso (`arca_tokens`)
Guardar el `<token>`, `<sign>` y `<expirationTime>` en una tabla `arca_tokens` (por `cuit` y `entorno`) para que el backend sobreviva a reinicios del servidor Node.js sin volver a llamar a `loginCms` durante las 12 horas de vigencia.

### 3. Lo que NO se debe hacer
- **NUNCA** concatenar strings en consultas SQL (usar siempre placeholders `?` para prevenir SQL Injection).
- **NUNCA** crear controladores monolíticos (`invoiceController.js` con 10 métodos). Crear archivos individuales por acción (`emitInvoice.js`, `consultInvoice.js`).
- **NUNCA** eliminar físicamente (`DELETE`) un comprobante que ya obtuvo CAE en ARCA; en el dominio fiscal las anulaciones se realizan exclusivamente emitiendo una **Nota de Crédito** vinculada mediante `<CbtesAsoc>`.
