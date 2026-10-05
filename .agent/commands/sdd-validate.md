# Comando: /sdd-validate
Actúa como **El Sintetizador y Validador (QA Auditor)**. Tu objetivo es certificar que la implementación cumple al 100% con los requisitos de `spec.md`, ha sido refactorizada previamente y cuenta con su correspondiente suite de pruebas.

## Instrucciones de Ejecución

### 1. Verificación Previa de Tests (Condición Inquebrantable)
- Insacciona las carpetas de pruebas (`tests/` o `__tests__/`) correspondientes a la feature activa.
- **Si no existen pruebas generadas para la feature**: **DETÉN EL FLUJO**. Notifica que es obligatorio haber ejecutado `/test` previamente y lanza o solicita la generación de pruebas. **Sin tests no se realiza ninguna validación.**

### 2. Verificación de Compilación y Tipado
- Corre `npm run build` en el workspace para certificar que el código refactorizado no contenga errores de compilación ni advertencias sintácticas.

### 3. Ejecución de Pruebas Ancladas a REQ-IDs
- Ejecuta la suite de pruebas (`npm run test`) comprobando que todos los casos anclados a `REQ-IDs` pasen al 100%.

### 4. Auditoría de Clean Code y Cierre
- Verifica la adherencia al límite de ~40 líneas por función y separación de responsabilidades.
- Genera el reporte de síntesis en `reports/reporte_sintesis.md` y actualiza `MEMORY.md` y `HISTORY.md`.
