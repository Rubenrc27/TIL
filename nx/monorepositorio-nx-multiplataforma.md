# Gestión de Monorepositorios con NX

He aprendido a utilizar **NX** para gestionar un proyecto multiplataforma que incluye una aplicación móvil (Android/iOS), una web (Next.js) y herramientas compartidas.

## Configuración Clave (`nx.json`)

NX utiliza plugins para automatizar tareas según la tecnología:
- **@nx/react-native/plugin**: Para gestionar el ciclo de vida de la app móvil.
- **@nx/expo/plugin**: Para facilitar el desarrollo con Expo.
- **@nx/next/plugin**: Para aplicaciones Next.js.
- **@nx/vite/plugin**: Para tests y builds rápidos con Vite.

## Scripts de Ejecución Centralizados (`package.json`)

Desde la raíz, se pueden ejecutar comandos para cualquier app:
- `run:android`: `npx nx run @org/movil:android`
- `run:web`: `npx nx run @org/web:dev`
- `lint`: `npx nx run-many -t lint` (Ejecuta el linter en todos los proyectos afectados).

## Ventajas del Monorepo
- **Compartir código:** Permite tener paquetes compartidos (como `packages/data` con Prisma) que usan tanto la web como el móvil.
- **Caché Inteligente:** NX guarda en caché los resultados de tareas anteriores para no repetirlas si no hay cambios.
