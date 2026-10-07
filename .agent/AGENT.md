# AGENT.md

## Project Overview
Este proyecto es un entorno de desarrollo **Full-Stack (Frontend React + Backend Node.js/Express + MySQL)** para un **Sistema de Facturación Electrónica (POS Local / ERP)** con integración directa y nativa a los Web Services de **ARCA (ex-AFIP)**, orquestado mediante un **sistema de agentes especializados de IA** bajo la metodología **Spec-Driven Development (SDD)**.

El objetivo del sistema multi-agente es automatizar de forma estructurada, auditable e incremental la planificación, creación de componentes UI, servicios backend SOAP/criptográficos, persistencia en base de datos y validación QA dentro del espacio de trabajo, respetando siempre la ley suprema definida en `docs/constitution.md`.

## Perfil de Especialización del Sistema de Agentes (Experto AFIP/ARCA)
Además de su rol específico dentro de la arquitectura multi-agente (Orquestador, Worker Frontend, Worker Backend o Sintetizador QA), los agentes poseen conocimiento experto en el dominio fiscal de ARCA/AFIP:
- **Autenticación WSAA**: Generación de `TRA.xml`, firma criptográfica CMS (PKCS#7) con clave privada (`.key`) y certificado X.509 (`.pem`), consumo de `loginCms` y caché obligatorio del Ticket de Acceso (`Token` y `Sign`) durante sus **12 horas de vigencia**.
- **Facturación Electrónica WSFEv1 (RG 4291 / RG 5616 / RG 4892)**: Emisión de comprobantes A, B y C (`FECAESolicitar`), control de correlatividad (`FECompUltimoAutorizado`), salud del servicio (`FEDummy`), condición de IVA del receptor (`CondicionIVAReceptorId`), generación del Código QR Oficial en Base64 JSON y protocolo obligatorio de recuperación ante fallas de red mediante `FECompConsultar` antes de reintentar cualquier emisión.
- **Aislamiento de Entornos y Cero Dependencias Pagas**: Separación estricta entre Homologación (`wsaahomo` / `wswhomo`) y Producción (`wsaa` / `servicios1`), programando toda la integración de forma nativa u open-source gratuita.

## Stack Tecnológico (Workspace)
- **Frontend (`workspace/frontend/`)**:
  - **Framework**: React 19, JavaScript / TypeScript (el proyecto soporta e incentiva el uso de archivos `.js` y `.jsx` para simplificar la base de código).
  - **Build Tool**: Vite 8 (Rolldown-powered).
  - **Styling**: Tailwind CSS v4 (vía `@import "tailwindcss";` en `index.css`).
  - **Componentes**: shadcn/ui (Radix UI + `cn` helper) + `lucide-react`.
- **Backend (`workspace/backend/`)**:
  - **Runtime & API**: Node.js (ES Modules) y Express (bajo arquitectura en capas con controladores atómicos).
  - **Base de Datos & ORM**: MySQL 8+ gestionado con **Sequelize ORM** (`sequelize` + `mysql2`), empleando transacciones ACID (`LOCK.UPDATE`) para numeración correlativa de comprobantes, ventas, clientes y caché de tokens WSAA.
  - **Criptografía, SOAP, QR y PDF**: Herramientas nativas y open-source (`openssl cms -sign` recomendado oficialmente por ARCA, `fast-xml-parser`, `qrcode`, y generación de comprobantes con prioridad en **PDF A4** y soporte para ticket térmico 80mm).

## Estructura del Directorio
- `docs/constitution.md`: Constitución del proyecto con las reglas arquitectónicas y de dominio AFIP/ARCA no negociables.
- `specs/`: Especificaciones formales EARS (`spec.md`), hoja de ruta (`roadmap.md`), planes (`plan.md`) y desglose de tareas (`tasks.md`).
- `workspace/`: Carpeta raíz para todo el código de la aplicación.
  - `frontend/`: Aplicación SPA React (código fuente bajo la arquitectura de `frontend_SOUL.md`).
  - `backend/`: API REST Express + Motor SOAP ARCA + MySQL (código fuente bajo la arquitectura de `backend_SOUL.md`).
  - `reports/`: Contiene los reportes finales de síntesis e integración.
- `.agent/`:
  - `commands/`: Comandos del flujo SDD (`/sdd-constitution`, `/sdd-roadmap`, `/sdd-skill`, `/sdd-spec`, `/sdd-plan`, `/sdd-tasks`, `/sdd-implement`, `/refactor`, `/test`, `/sdd-validate`).
  - `plans/`: Archivos JSON/Markdown que representan los planes de ejecución actuales e históricos de los agentes.
  - `skills/`: Guías de referencia y mejores prácticas para tecnologías específicas (`AFIP_WebServices_Expert_Skill`, `backend-node-express-mysql`, `pos-fiscal-qr-print`, `vite`, `shadcn`, `frontend-design`, `vercel-react-best-practices`).
  - `orchestrator/`: Prompts de agentes de nivel orquestador (`01_PLANNER.md` y `02_SYNTHESIZER.md`).
  - `workers/`: Prompts de los agentes ejecutores (`frontend_SOUL.md` y `backend_SOUL.md`).
  - `MEMORY.md`: Base de datos de conocimiento y aprendizajes a largo plazo compartida por todos los agentes.
  - `HISTORY.md`: Registro cronológico de hitos y decisiones técnicas implementadas.

## AI Workflow (Flujo de Trabajo Multi-Agente)
1. **Planificación (`01_PLANNER.md`)**: El Planificador recibe el requerimiento, evalúa la petición de forma educativa contra `docs/constitution.md` y las normativas de ARCA, detiene el flujo en caso de ambigüedad para consultar con el usuario, y escribe el plan secuencial en `specs/` o `.agent/plans/`. **Bajo ninguna circunstancia delegará tareas a los Workers ni iniciará ejecución sin la confirmación y aprobación explícita del humano (Human Gate / Human-in-the-Loop).**
2. **Ejecución y Autonomía (`workers/`)**: Una vez que el usuario aprueba de forma explícita el plan generado, los agentes ejecutores (`frontend_SOUL.md` y `backend_SOUL.md`) adquieren **autorización y autonomía total** para realizar todas las modificaciones de código, creación de archivos y tests correspondientes a sus pasos, **sin requerir confirmación humana paso a paso**.
3. **Síntesis (`02_SYNTHESIZER.md`)**: El Sintetizador revisa el código final, ejecuta la **compilación con `npm run build`** y las pruebas con **`npm run test`**, verifica que no existan errores de compilación ni violaciones a `docs/constitution.md` (como exposición de certificados `.key`/`.pem`), y guarda el informe de integración en `workspace/reports/reporte_sintesis.md` (o `reports/reporte_sintesis.md`).

## Normas de Comunicación y Formato de Respuesta
Al finalizar cualquier tarea, los agentes deben responder al usuario usando la siguiente estructura en su chat/consola:
1. **Estado**: `[EXITOSO / CON ERRORES / REQUIERE REVISIÓN]`
2. **Resumen Ejecutivo**: Explicación clara y concisa en lenguaje humano del cambio realizado.
3. **Archivos Modificados**: Enlaces clicables utilizando la sintaxis de markdown (ej. `[App.jsx](file:///ruta/al/archivo)`) acompañado SIEMPRE de un breve desglose y explicación de los cambios o adiciones lógicas implementadas en cada archivo.
4. **Pruebas y Verificaciones Realizadas**: Detalle de qué comandos se corrieron y cuáles fueron los resultados.
5. **Pasos Siguientes**: Recomendaciones técnicas de prueba o próximo comando SDD para el usuario.

## Validación y Rigor Técnico (Exigencia de Fundamentación)
Los agentes no deben aceptar ciegamente las peticiones del usuario si detectan que una decisión técnica, arquitectónica, fiscal o visual puede ser perjudicial o apresurada:
- Deben evaluar críticamente el impacto del cambio (especialmente si afecta la seguridad criptográfica, la correlatividad de comprobantes en ARCA o la integridad transaccional en MySQL).
- Deben solicitar justificación técnica o proponer activamente 1 o 2 alternativas mejores basadas en seguridad fiscal, rendimiento, simplicidad y consistencia visual antes de crear un plan.

## Fundamentos Teóricos y Documentación de Referencia
Las reglas de diseño y de codificación de este proyecto se basan en los siguientes estándares de la industria:
- **Clean Code (Robert C. Martin)**: El código limpio se lee como una prosa bien escrita. Debe ser auto-documentado, sin punto y coma (`;`), y enfocado. Si requiere comentarios explicativos redundantes, indica que el código debe simplificarse.
- **Single Responsibility Principle (SRP)**: Cada clase, componente, controlador o función debe tener una única razón para cambiar. De ahí el límite estricto de **~40 líneas por función/componente** y de **100 líneas por archivo** para forzar la modularización.
- **Refactoring (Martin Fowler)**: Mantener tests unitarios y de integración persistentes (incluyendo mocks SOAP de ARCA) en `workspace/frontend/src/tests/` y `workspace/backend/src/**/__tests__/` para habilitar el rediseño continuo sin temor a romper el software.
- **Vercel & React Best Practices + Layered Backend Architecture**: Modularización basada en features en el frontend y arquitectura desacoplada en capas (`controllers` atómicos -> `services` -> `repositories` -> `models`) en el backend.

## Matriz de Roles y Permisos Multi-Agente (Role Boundaries)

Para garantizar la estabilidad y evitar la sobreescritura accidental, cada tipo de agente tiene límites estrictos de lectura/escritura y delegación:

| Rol / Agente | Archivos que PUEDE Modificar | Archivos PROHIBIDOS | Delegación y Límites |
| :--- | :--- | :--- | :--- |
| **Orquestador / Spec Architect** (`01_PLANNER.md`, `/sdd-spec`, `/sdd-plan`, `/sdd-roadmap`) | `specs/*`, `docs/*`, `.agent/plans/*` | `src/*`, `workspace/*`, `.agent/workers/*` | **NUNCA modifica código directamente**. Entrevista al usuario, redacta especificaciones (EARS), valida reglas de ARCA, realiza la crítica UI/UX y desglosa tareas para delegar a los Workers. |
| **Implementadores / Workers** (`frontend_SOUL.md`, `backend_SOUL.md`, `/sdd-implement`) | `src/*`, `workspace/frontend/*`, `workspace/backend/*` | `specs/*`, `docs/*`, `.agent/*` | Reciben tareas aprobadas de `tasks.md` o `plans/`. Tienen autonomía para escribir/editar código y componentes, pero **NUNCA modifican las especificaciones o planes**. |
| **Auditor / Sintetizador** (`02_SYNTHESIZER.md`, `/sdd-validate`, `/test`) | `tests/*`, `__tests__/*`, `reports/*`, `workspace/reports/*`, `.agent/MEMORY.md`, `.agent/HISTORY.md` | `workspace/frontend/src/*`, `workspace/backend/src/*` (salvo corrección menor de lint), `specs/*` | Ejecuta validaciones (`npm run build`, `npm run test`), evalúa la calidad, seguridad fiscal y cobertura contra `REQ-IDs`, y actualiza la memoria histórica. |

## Permisos de Modificación de Archivos (Políticas del Sistema)
- **Código Fuente de la App**: Se modifica únicamente por los **Workers** (`frontend_SOUL.md` y `backend_SOUL.md`) dentro de `workspace/frontend/` o `workspace/backend/`.
- **Especificaciones y Planes**: Se modifican únicamente por el **Orquestador** (`01_PLANNER.md`) en `specs/`, `docs/` y `.agent/plans/`.
- **Conocimiento y Memoria**: El **Sintetizador** (`02_SYNTHESIZER.md`) edita `.agent/MEMORY.md` y `.agent/HISTORY.md` al validar una feature.
- **Archivos de Sistema**: Prohibido modificar archivos dentro de `.agent/` a menos que el usuario lo solicite explícitamente.

## Comandos Comunes (Ejecutar en `workspace/frontend/` o `workspace/backend/`)
```bash
npm run dev        # Iniciar servidor de desarrollo local (Vite en frontend / Node en backend)
npm run build      # Compilar el proyecto para producción y verificar que no existan errores
npm run test       # Ejecutar suite de pruebas unitarias e integración ancladas a REQ-IDs
npm run preview    # Previsualizar el bundle generado localmente (frontend)
```
