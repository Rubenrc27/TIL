# Control de Pantallas EPD Philips mediante AIDL

Las pantallas de tinta electrónica (EPD) de Philips en entornos profesionales de cartelería digital se gestionan a través de servicios del sistema. Para interactuar con ellas desde nuestra propia App, es necesario utilizar **AIDL** (Android Interface Definition Language).

## Configuración del `AndroidManifest.xml`

Para que nuestra aplicación pueda "ver" el servicio de Philips, debemos declarar la consulta explícita en el manifiesto:

```xml
<queries>
    <package android:name="com.example.epdservice" />
</queries>
```

## Implementación de la Conexión (IPC)

La comunicación se realiza mediante un `ServiceConnection`. Es vital manejar el ciclo de vida para evitar fugas de memoria.

```kotlin
private val connection = object : ServiceConnection {
    override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
        // Obtenemos la interfaz definida en el archivo .aidl
        epdInterface = IEPDAidlInterface.Stub.asInterface(service)
    }

    override fun onServiceDisconnected(name: ComponentName?) {
        epdInterface = null
    }
}

// Lanzar el bind al servicio específico de Philips
private fun bindEpdService() {
    val intent = Intent().apply {
        component = ComponentName("com.example.epdservice", "com.example.epdservice.USBConnectionService")
    }
    bindService(intent, connection, Context.BIND_AUTO_CREATE)
}
```

## Envío de Imágenes a la Pantalla

Una vez establecida la conexión, podemos enviar rutas de archivos locales para que el hardware los renderice:

```kotlin
// Envía una imagen local a las coordenadas 0,0
val responseCode = epdInterface?.sendImage(file.absolutePath, 0, 0, 0, 0, false, 0)
```

## Puntos clave
* **IPC (Inter-Process Communication):** AIDL permite llamar a métodos de otra aplicación (en este caso, el driver de la pantalla) como si fueran locales.
* **TCON (Timing Controller):** En pantallas EPD, a veces es necesario activar el `setTCONKeepOn(true)` antes de enviar múltiples ráfagas de datos para evitar parpadeos innecesarios.
