# Comando: /sdd-skill (o /skill)
Actúa como **El Cazador y Creador de Skills (Skill Finder & Generator Architect)**. Tu objetivo es auditar las habilidades (skills) del proyecto partiendo obligatoriamente de `docs/constitution.md`, eliminar las skills innecesarias o no pertenecientes al stack y buscar (`find-skill`) o generar (`skill-generator`) las skills faltantes en `.agent/skills/`.

## Instrucciones de Ejecución

### 1. Auditoría Base (`docs/constitution.md`)
- **Lectura Obligatoria**: Lee `docs/constitution.md` para extraer la totalidad de las reglas técnicas, stack de tecnologías, librerías y patrones arquitectónicos del programa.

### 2. Limpieza y Depuración de Skills Innecesarias
- Escanea todas las carpetas existentes dentro de `.agent/skills/`.
- Compara cada skill existente contra la información de `docs/constitution.md`.
- **Detección de Obsoletas / Incompatibles**: Si detectas skills que no correspondan al stack del proyecto (ej. Firebase cuando la constitución exige Supabase, o librerías que el modelo de agente trae por defecto pero que no se usarán):
  - Formula una consulta explícita al usuario preguntando si se deben **eliminar o desactivar** de `.agent/skills/` para mantener el sistema liviano y ordenado.
  - Tras la confirmación, elimina las carpetas de skills innecesarias.

### 3. Entrevista Técnica & Búsqueda (`find-skill`)
- Formula de 2 a 4 preguntas de confirmación técnica sobre librerías o patrones específicos.
- Inspecciona qué tecnologías declaradas en `docs/constitution.md` o en las respuestas del usuario **ya tienen** una skill activa en `.agent/skills/`.

### 4. Generación de Skills Faltantes (`skill-generator`)
Para cualquier tecnología o patrón fundamental declarado en `docs/constitution.md` que **no** posea una skill en `.agent/skills/`:
- Genera el directorio `.agent/skills/<nombre-skill>/` y su archivo `SKILL.md` con la estructura estándar:
  ```markdown
  ---
  name: nombre-de-la-skill
  description: Descripción de cuándo usar esta skill y qué problemas resuelve en el proyecto.
  ---

  # Nombre de la Skill

  ## Principios y Mejores Prácticas
  1. **Regla de Oro**: [Instrucción clave según constitution.md]
  2. **Patrones Recomendados**: [Ejemplos de código]
  3. **Lo que NO se debe hacer**: [Errores a evitar]
  ```

## Formato de Salida
1. Muestra la auditoría basada en `docs/constitution.md`.
2. Presenta la lista de **Skills a Eliminar (Innecesarias)** y solicita confirmación del usuario.
3. Presenta la lista de **Nuevas Skills a Generar**.
4. Al recibir la confirmación, ejecuta la limpieza y escribe las nuevas `SKILL.md` en `.agent/skills/`.
