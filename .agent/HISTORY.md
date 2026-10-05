# HISTORIAL DE CAMBIOS Y DECISIONES TÉCNICAS (HISTORY.md)

## [2026-10-05 18:40] Alineación del Ecosistema Multi-Agente y Skills Especializadas ARCA/AFIP

### Decisiones de Arquitectura y Gobernanza SDD
- **Constitución Fundacional (`docs/constitution.md`)**: Se redactó la Constitución del Proyecto para el Sistema de Facturación Electrónica POS/ERP con integración nativa a ARCA (ex-AFIP) usando React 19, Vite 8, Tailwind CSS v4, shadcn/ui, Node.js, Express y MySQL 8+.
- **Activación y Enriquecimiento de `AFIP_WebServices_Expert_Skill`**: Se añadió el frontmatter YAML estándar (`name: afip-arca-webservices`) y las estructuras SOAP exactas de `WSAA` (`loginCms`) y `WSFEv1` (`FEDummy`, `FECompUltimoAutorizado`, `FECAESolicitar`, `FECompConsultar`), vinculando los manuales oficiales PDF y archivos WSDL locales.
- **Nuevas Skills Técnicas Generadas**:
  - `backend-node-express-mysql`: Patrones de controladores atómicos, transacciones ACID con `SELECT ... FOR UPDATE` en MySQL para correlatividad de comprobantes y máquina de estados fiscal.
  - `pos-fiscal-qr-print`: Firma criptográfica CMS (PKCS#7) con OpenSSL, validación de CUIT (Módulo 11), especificación oficial del Código QR de ARCA (RG 4892) y requisitos de impresión gráfica (A4 y ticket 80mm).
- **Alineación Integral de `.agent/`**: Se actualizaron `AGENT.md`, `backend_SOUL.md`, `frontend_SOUL.md`, `01_PLANNER.md`, `02_SYNTHESIZER.md` y `MEMORY.md` para que todos los agentes operen como expertos en los Web Services de ARCA/AFIP.
