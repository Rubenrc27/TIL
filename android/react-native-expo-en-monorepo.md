# Desarrollo Android con React Native y Expo en un Monorepo

He aprendido a integrar una aplicación móvil desarrollada con **React Native** y **Expo** dentro de una arquitectura de monorepositorio con NX.

## Tecnologías Utilizadas
- **Expo Framework:** Para facilitar el acceso a APIs nativas y el desarrollo rápido.
- **Firebase:** Integración de `analytics`, `auth` y `crashlytics` para robustez y analítica.
- **Mapas:** Uso de `react-native-maps` para geolocalización.
- **Navegación:** `@react-navigation/native` para la gestión de pantallas.

## Comandos Útiles
Para arrancar el emulador de Android directamente desde NX:
```bash
npm run run:android
```
O para iniciar Metro Bundler:
```bash
npm run run:metro
```

## Configuración de Dependencias
Las dependencias nativas se gestionan de forma centralizada pero se exponen a través del bundle de Metro para Android, asegurando compatibilidad entre las versiones de Firebase y React Native.
