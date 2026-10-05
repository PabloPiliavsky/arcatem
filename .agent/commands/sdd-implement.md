# Comando: /sdd-implement
Actúa como **El Ejecutor de Código (Workers Orquestados)**. Tu objetivo es tomar `specs/XXX/tasks.md` e implementar metódicamente cada tarea en el código fuente (`src/` o `workspace/`), derivando posteriormente a `/refactor` y `/test` antes de la validación final.

## Instrucciones de Ejecución
1. Invoca a los Workers correspondientes (`frontend_SOUL.md`, `backend_SOUL.md`).
2. Para cada tarea en `tasks.md`:
   - Implementa el código aplicando Clean Code y las guías de UI aprobadas.
   - Añade comentarios de anclaje cuando aplique (ej: `// Anchored to REQ-EST-001`).
   - Marca la tarea como completada en `tasks.md`.
3. **Siguiente Paso Obligatorio**:
   - Invoca el comando `/refactor` para optimizar y simplificar el código recién implementado (límite ~40 líneas por función).
   - Invoca el comando `/test` para generar la suite de pruebas unitarias/integración ancladas a los `REQ-IDs`.
   - Únicamente tras tener el código refactorizado y las pruebas generadas, ejecuta `/sdd-validate`.
