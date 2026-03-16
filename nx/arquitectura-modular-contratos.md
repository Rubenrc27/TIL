# Arquitectura Modular y Contratos de API Seguros

He aprendido cómo un monorepositorio con **NX** permite separar las apps de los contratos de API y clientes, mejorando la seguridad y reutilización.

## Estructura de Paquetes
- **apps/movil** y **apps/web**: Consumen la lógica de negocio.
- **packages/contracts**: Define los tipos (probablemente con Zod) que se usan tanto en el backend como en los frontends.
- **packages/api-clients**: Librería cliente generada automáticamente para no tener que escribir las peticiones HTTP manualmente en cada app.

## Automatización con OpenAPI/Swagger
Mediante el uso de un script de generación (\`tools/scripts/generate-openapi.ts\`), el proyecto mantiene la documentación al día.

### Flujo de Trabajo
1. Se define la lógica en el backend.
2. Se ejecuta el script para generar el \`openapi.json\`.
3. Se actualizan los clientes de API en los frontends.

Esto garantiza que si el backend cambia, los frontends detectarán errores de tipo en tiempo de compilación.
