# Hoja de Ruta del Proyecto (Master Feature Roadmap)

## Constitución de Referencia: [`docs/constitution.md`](file:///c:/Users/Pablo/Desktop/personal/arcatem/docs/constitution.md)

Esta hoja de ruta desglosa el **Sistema de Facturación Electrónica POS / ERP (ARCA / ex-AFIP)** en una secuencia incremental de features correlativas bajo la metodología **Spec-Driven Development (SDD)**.

---

### Secuencia de Features Planificadas

1. **`specs/001-foundation-db-config/`**: Inicialización Full-Stack, Base de Datos MySQL (Sequelize) y Configuración Fiscal del Emisor.
   - **Propósito**:
     - Inicializar `workspace/backend/` (Node.js ES Modules, Express, Sequelize ORM + MySQL 8+) y `workspace/frontend/` (React 19, Vite 8, Tailwind CSS v4, shadcn/ui).
     - Configurar `.gitignore` estricto para proteger claves privadas (`.key`), certificados (`.pem`/`.crt`), `TA.xml` y `.env`.
     - Crear el modelo y pantalla de **Configuración del Emisor Fiscal** (CUIT, Razón Social, Condición frente al IVA, Punto de Venta, IIBB, Inicio de Actividades, selector de entorno `HOMOLOGACIÓN` vs `PRODUCCIÓN` y verificación de certificados).
   - **Dependencias**: Ninguna *(Módulo Base)*.

2. **`specs/002-arca-wsaa-auth/`**: Motor Criptográfico `WSAA` (`TRA`, Firma `openssl cms` y Caché de Ticket de Acceso 12h) + Estado `FEDummy`.
   - **Propósito**:
     - Implementar la generación del Ticket de Requerimiento de Acceso (`LoginTicketRequest.xml`) con margen anti-deriva NTP.
     - Firmar en formato CMS (PKCS#7) invocando `openssl cms -sign` con la clave privada y certificado X.509.
     - Consumir el servicio SOAP `LoginCms` de ARCA y persistir el `Token`, `Sign` y `expirationTime` en el modelo Sequelize `ArcaToken` (reutilizando el token durante sus 12 horas de vigencia).
     - Exponer endpoint y widget en el header del POS conectado a `FEDummy` (`AppServer`, `DbServer`, `AuthServer`).
   - **Dependencias**: `001-foundation-db-config`.

3. **`specs/003-catalog-clients-products/`**: Gestión de Clientes (Validación CUIT Módulo 11 y RG 5616) y Catálogo de Productos/Servicios.
   - **Propósito**:
     - CRUD completo de Clientes con validación de dígito verificador Módulo 11 para CUIT/CUIL, soporte para Consumidor Final (`DocTipo = 99` / `96` DNI) y asignación obligatoria de `CondicionIVAReceptorId` (RG 5616).
     - CRUD de Productos y Servicios con código SKU, precio neto/final y alícuota de IVA (`21%`, `10.5%`, `27%`, `5%`, `2.5%`, `0%`, Exento).
   - **Dependencias*: `001-foundation-db-config`.

4. **`specs/004-arca-wsfe-billing-engine/`**: Motor Transaccional de Facturación `WSFEv1` y Protocolo de Contingencia Anti-Duplicación.
   - **Propósito**:
     - Sincronizar numeración correlativa combinando `FECompUltimoAutorizado` y bloqueo pesimista en Sequelize (`lock: t.LOCK.UPDATE`).
     - Validar cuadratura matemática de importes (`ImpTotal = ImpTotConc + ImpNeto + ImpOpEx + ImpTrib + ImpIVA` en `DECIMAL(15, 2)`).
     - Emitir Facturas A (`1`), B (`6`), C (`11`), Notas de Débito (`2, 7, 12`) y Notas de Crédito (`3, 8, 13`) mediante `FECAESolicitar`.
     - Implementar la máquina de estados (`PENDIENTE_ENVIO`, `TIMEOUT_PENDIENTE_CONSULTA`, `APROBADO`, `RECHAZADO`) y la reconciliación automática obligatoria con `FECompConsultar` ante caídas de red o timeouts.
   - **Dependencias**: `002-arca-wsaa-auth`, `003-catalog-clients-products`.

5. **`specs/005-pos-billing-ui/`**: Terminal Punto de Venta (POS) e Interfaz Operativa de Facturación en React.
   - **Propósito**:
     - Interfaz ágil de facturación en React + shadcn/ui con carga rápida de ítems, búsqueda por teclado, selección de cliente y determinación automática de letra de comprobante (`A`, `B` o `C`).
     - Manejo visual de errores/observaciones de ARCA y botón de recuperación segura (`FECompConsultar`) cuando una venta queda en estado de contingencia por timeout.
   - **Dependencias**: `004-arca-wsfe-billing-engine`.

6. **`specs/006-qr-pdf-printing/`**: Generador de Código QR Oficial de ARCA (RG 4892) y Comprobantes PDF A4 (Prioridad) / Ticket 80mm.
   - **Propósito**:
     - Construir el payload JSON codificado en Base64 para el QR Oficial (`https://www.afip.gob.ar/fe/qr/?p=...`) incorporando CAE y fecha de vencimiento.
     - Generar el comprobante fiscal en **PDF formato Hoja A4 (Prioridad 1)** con diseño reglamentario (discriminación de IVA en Factura A, Régimen de Transparencia Fiscal Ley 27.743 en Factura B y monotributo en Factura C) y dejar lista la vista de impresión para **Ticket Térmico 80mm**.
   - **Dependencias**: `004-arca-wsfe-billing-engine`, `005-pos-billing-ui`.

7. **`specs/007-voucher-history-credit-notes/`**: Historial de Comprobantes, Auditoría Fiscal y Emisión de Notas de Crédito Vinculadas (`CbtesAsoc`).
   - **Propósito**:
     - Panel de consulta de comprobantes emitidos con filtros por fecha, cliente, tipo y punto de venta.
     - Descarga/reimpresión de PDFs, consulta en vivo contra ARCA (`FECompConsultar`) y flujo de anulación fiscal mediante emisión de Nota de Crédito referenciando automáticamente el comprobante original en `<CbtesAsoc>`.
   - **Dependencias**: `005-pos-billing-ui`, `006-qr-pdf-printing`.
