# Gestión de Datos en Redis con Node.js

He aprendido a realizar un **seed** de datos para **Redis** utilizando la librería `ioredis` dentro de un script de automatización en TypeScript.

## Limpieza e Inicialización de Caché

Un script de seed debe ser idempotente, lo que significa que debe poder ejecutarse varias veces sin causar duplicados. En este proyecto se utiliza para limpiar y luego poblar datos:

```typescript
import Redis from 'ioredis';

const redis = new Redis(process.env['REDIS_URL'] || 'redis://localhost:6379');

async function seedRedis() {
  console.log('Limpiando registros de Redis...');
  
  // Borrar una lista específica
  await redis.del('spots:active');
  
  // Buscar y borrar todas las claves que coincidan con un patrón (p. ej. "spot:*")
  const keys = await redis.keys('spot:*');
  if (keys.length > 0) {
    await redis.del(...keys);
  }
}
```

## Ventajas del Seed en Redis
- **Datos de prueba rápidos:** Permite poblar la caché con información simulada (usando `faker`) para pruebas de rendimiento sin depender de la base de datos principal.
- **Configuración de volúmenes:** El script permite pasar argumentos como `--volume=medium` para escalar la cantidad de datos que se suben a Redis.
- **Integración con Docker:** Al tener el contenedor de Redis levantado, el script conecta directamente y deja el entorno listo para su uso.

### Notas de Implementación
- Se recomienda usar `dotenv` para cargar la URL de conexión de Redis desde el archivo `.env`.
- La limpieza mediante `keys` y `del` es útil en desarrollo, pero debe usarse con precaución en producción para evitar bloqueos de red si hay millones de claves.
