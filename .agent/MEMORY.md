# MEMORIA SEMÁNTICA Y APRENDIZAJES DEL PROYECTO (ARCATEM POS / ERP)

Este archivo contiene el contexto a largo plazo, decisiones de arquitectura y aprendizajes acumulados del proyecto **ARCATEM — Sistema de Facturación Electrónica (ARCA / ex-AFIP)**. Debe ser leído por todos los agentes al inicio de cualquier tarea.

## HECHOS VALIDADOS (FACTS)
- **Dominio**: Sistema de Facturación Electrónica POS Local / ERP con integración SOAP nativa a los Web Services de ARCA (`WSAA` y `WSFEv1` - RG 4291, RG 5616 y RG 4892) sin librerías aranceladas.
- **Stack Full-Stack**:
  - **Frontend (`workspace/frontend/`)**: React 19 (`.js`/`.jsx`), Vite 8, Tailwind CSS v4, shadcn/ui, `lucide-react`.
  - **Backend (`workspace/backend/`)**: Node.js (ES Modules), Express, **Sequelize ORM** (`sequelize` + `mysql2` sobre MySQL 8+ InnoDB transaccional ACID), firma criptográfica mediante **`openssl cms -sign`** (estándar oficial recomendado por ARCA), XML SOAP (`fast-xml-parser`), QR (`qrcode`) y generación de comprobantes con **prioridad en PDF (A4)** más soporte térmico 80mm.
- **Preferencia del Usuario**: Respuestas concisas, desarrollo incremental guiado por especificaciones (SDD), código auto-documentado sin comentarios redundantes y sin punto y coma (`;`).

## DECISIONES ARQUITECTÓNICAS
- **[2026-10-05 17:08] Constitución del Proyecto (`docs/constitution.md`)**: Se estableció la arquitectura Cliente-Servidor desacoplada donde el backend Node.js/Express custodia las claves privadas (`.key`) y certificados X.509 (`.pem`), gestiona el caché de 12 horas del Ticket de Acceso (`TA`) de WSAA y garantiza la numeración correlativa de comprobantes en MySQL.
- **[2026-10-05 17:08] Protocolo Anti-Duplicación ante Timeouts**: Ante cualquier timeout de red o error 5xx durante `FECAESolicitar`, está prohibido reintentar la emisión directamente; el backend debe ejecutar primero `FECompConsultar` y `FECompUltimoAutorizado` para reconciliar el estado fiscal.
- **[2026-10-05 18:35] Ecosistema de Skills Fiscales (`.agent/skills/`)**:
  - `AFIP_WebServices_Expert_Skill` (`afip-arca-webservices`): Contiene la guía experta de WSAA/WSFEv1 junto con los manuales oficiales en PDF y los WSDLs de Homologación y Producción.
  - `backend-node-express-mysql`: Reglas de arquitectura en capas, controladores atómicos, máquina de estados del comprobante y bloqueo transaccional `FOR UPDATE` en MySQL.
  - `pos-fiscal-qr-print`: Reglas de firma CMS PKCS#7, validación Módulo 11 de CUIT, generación del QR oficial en Base64 JSON (RG 4892) e impresión A4/80mm (incluyendo Ley 27.743 de Transparencia Fiscal).
- **[2026-10-05 18:35] Límites de Clean Code**: Funciones y componentes de máximo ~40 líneas; archivos de máximo 100 líneas; controladores Express atómicos y componentes React declarados con `export default function`.

## APRENDIZAJES Y ERRORES EVITADOS
- **[2026-06-23 19:35] [Dependencias / Testing]**: Evitar versiones antiguas de `lucide-react` y preferir rutas consistentes compatibles con Vitest.
- **[2026-06-30 19:55] [Frontend / Tailwind v4]**: Al inicializar Vite con Tailwind CSS v4, instalar `tailwindcss` y `@tailwindcss/vite`, configurar el plugin en `vite.config.js` y colocar `@import "tailwindcss";` en `index.css`.
- **[2026-07-10 10:35] [shadcn/ui / Vite JavaScript]**: En proyectos React Vite con JavaScript (`.js`/`.jsx`), configurar `jsconfig.json` con `baseUrl` y `paths: { "@/*": ["./src/*"] }` antes de ejecutar el CLI de `shadcn`.
- **[2026-10-05 18:35] [ARCA WSAA / Token Caching]**: Solicitar un nuevo Ticket de Acceso (`loginCms`) antes de que expire el actual devuelve el fault SOAP `coe.alreadyAuthenticated`. El backend siempre debe persistir y reutilizar el TA vigente durante sus 12 horas.
