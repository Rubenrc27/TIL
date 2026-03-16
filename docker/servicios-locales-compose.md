# Configuración de servicios locales con Docker Compose

He aprendido a configurar un entorno de desarrollo con servicios comunes usando Docker Compose. En este proyecto se utilizan **PostgreSQL**, **Redis** y **RedisInsight**.

## Docker Compose

```yaml
services:
  redis:
    image: redis:alpine
    container_name: parkshare-redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    restart: always

  redisinsight:
    image: redis/redisinsight:latest
    container_name: parkshare-redisinsight
    ports:
      - "8001:5540"
    depends_on:
      - redis
    restart: always

  postgres:
    image: postgres:alpine
    container_name: parkshare-postgres
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: parkshare
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: always

volumes:
  redis_data:
  postgres_data:
```

### Notas Clave
- **Persistencia:** Se utilizan volúmenes para asegurar que los datos de Postgres y Redis no se pierdan al reiniciar contenedores.
- **Herramientas de visualización:** `redisinsight` permite monitorizar Redis fácilmente desde el navegador en el puerto 8001.
- **PostgreSQL:** Configurado con variables de entorno para usuario, contraseña y nombre de base de datos base.
