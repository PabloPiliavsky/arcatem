# Comando: /sdd-constitution
Actúa como **El Guardián de la Constitución del Proyecto**. Tu objetivo es inicializar o actualizar el archivo `docs/constitution.md` definiendo las leyes inquebrantables del proyecto y sugiriendo la invocación posterior de `/sdd-roadmap` (usando `01_PLANNER.md`) para desglosar el mapa maestro de la aplicación.

## Instrucciones de Ejecución
1. Revisa la tecnología del proyecto (React, Vite, Tailwind CSS v4, shadcn/ui, Clean Code, límites de 40 líneas por función).
2. Si `docs/constitution.md` no existe, créalo con las siguientes secciones:
   - **I. Principios de Arquitectura**: Modularización por features, SRP, funciones limpias de ~40 líneas por función.
   - **II. Estándar de UI/UX**: Uso de shadcn/ui, prohibición de diseños predeterminados/genéricos, jerarquía visual limpia.
   - **III. Estrategia de Pruebas**: Cobertura de tests anclada a Requisitos (`REQ-IDs`).
   - **IV. Control de Cambios**: Toda modificación requiere una `spec.md` aprobada previa.
3. Si el archivo ya existe, actualiza únicamente las leyes aprobadas por el usuario.
4. **Siguiente Paso Obligatorio**: Al guardar `docs/constitution.md`, solicita la invocación del comando `/sdd-roadmap` para utilizar `01_PLANNER.md` y armar la hoja de ruta de la aplicación.

## Formato de Salida
Presenta la constitución creada o modificada en `docs/constitution.md` y sugiere proceder de inmediato con `/sdd-roadmap`.
