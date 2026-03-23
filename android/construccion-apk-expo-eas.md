# Construcción de un APK con EAS

Generar un archivo APK (Android Package) con Expo Application Services (EAS) permite probar la aplicación en dispositivos físicos sin pasar por la Play Store.

## 1. Requisitos previos
Es necesario tener instalado el CLI de EAS y haber iniciado sesión:

```bash
npm install -g eas-cli
eas login
```

## 2. Configuración del proyecto
Si el proyecto no tiene configurado EAS, se debe inicializar:

```bash
eas build:configure
```

## 3. Modificar `eas.json`
Por defecto, EAS genera archivos `.aab` (Android App Bundle). Para generar un `.apk`, hay que añadir o modificar un perfil en el archivo `eas.json`:

```json
{
  "build": {
    "preview": {
      "android": {
        "buildType": "apk"
      }
    },
    "production": {}
  }
}
```

*   **`buildType: "apk"`**: Indica a EAS que debe generar un instalador directo.
*   **`preview`**: Es el nombre del perfil (puedes usar cualquier otro nombre).

## 4. Ejecutar la construcción
Para iniciar el proceso de compilación en los servidores de Expo, ejecuta:

```bash
eas build -p android --profile preview
```

## 5. Obtener el archivo
Una vez finalizada la construcción, el CLI proporcionará una URL de descarga. También se puede encontrar el archivo en el [dashboard de Expo](https://expo.dev/artifacts).

*Nota: Para instalar el APK en Android, es necesario habilitar la opción de "Instalar aplicaciones de fuentes desconocidas" en los ajustes del dispositivo.*
