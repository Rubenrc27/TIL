# Comunicación Serial USB con Sensores Nexmosphere en Android

La integración de sensores físicos (botones, sensores IR, básculas) en Android requiere la comunicación con dispositivos USB de forma directa o mediante drivers seriales (como FTDI o Prolific).

## Integración de Sensores Nexmosphere

Nexmosphere usa comandos ASCII a través de una interfaz serial virtual. Los pasos para una integración nativa son:

### 1. Detección y Permisos
Es necesario buscar el driver compatible y solicitar permisos al usuario mediante un `PendingIntent`.

```kotlin
val usbManager = getSystemService(Context.USB_SERVICE) as UsbManager
val availableDrivers = UsbSerialProber.getDefaultProber().findAllDrivers(usbManager)
if (availableDrivers.isEmpty()) return

// Pedir permiso si no se tiene
val permissionIntent = PendingIntent.getBroadcast(this, 0, Intent(ACTION_USB_PERMISSION), 0)
usbManager.requestPermission(driver.device, permissionIntent)
```

### 2. Configuración del Puerto Serie
Una vez obtenido el permiso, se abre el puerto con la velocidad de baudios adecuada (típicamente 115200 para sensores Nexmosphere).

```kotlin
usbSerialPort = driver.ports[0]
usbSerialPort?.open(connection)
usbSerialPort?.setParameters(115200, 8, UsbSerialPort.STOPBITS_1, UsbSerialPort.PARITY_NONE)
```

### 3. Escuchar Eventos (SerialInputOutputManager)
Para no bloquear el hilo principal, se utiliza un manejador de entrada/salida que corre en un hilo separado.

```kotlin
usbIoManager = SerialInputOutputManager(usbSerialPort, object : SerialInputOutputManager.Listener {
    override fun onNewData(data: ByteArray?) {
        val chunk = String(data!!, Charsets.US_ASCII)
        if (chunk.contains("X001A[17]")) { // Comando específico de activación
            onButtonPressed()
        }
    }
    override fun onRunError(e: Exception?) { /* ... */ }
})
executor.submit(usbIoManager)
```

## Arquitectura Recomendada

Para una aplicación interactiva, es vital:
1. **Foreground Service:** Mantener la conexión USB viva mediante un servicio en primer plano para evitar que el sistema lo cierre.
2. **AtomicBoolean:** Usar bloqueos atómicos (`AtomicBoolean`) para evitar que múltiples eventos de hardware disparen procesos concurrentes no deseados (debouncing por software).
3. **MD5 Hashing:** Si el hardware dispara descargas de red, calcular el hash del contenido para no procesar o guardar archivos idénticos redundantes.
