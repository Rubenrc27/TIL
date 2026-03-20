# Notificaciones Push con Firebase (FCM) en Android

Configurar notificaciones push requiere integrar Firebase Cloud Messaging (FCM) en el proyecto de Android Studio y configurar el servicio para recibir mensajes.

## 1. Configuración Inicial
1. Crea un proyecto en la [Firebase Console](https://console.firebase.google.com/).
2. Registra tu app Android con el nombre del paquete (`applicationId`).
3. Descarga el archivo `google-services.json` y colócalo en el directorio `app/`.

## 2. Dependencias (build.gradle)
Añade el plugin de Google Services en `build.gradle` (Project) y la librería de FCM en `build.gradle` (Module: app):

```kotlin
// build.gradle (Project)
plugins {
    id("com.google.gms.google-services") version "4.4.0" apply false
}

// build.gradle (Module: app)
plugins {
    id("com.android.application")
    id("com.google.gms.google-services")
}

dependencies {
    implementation(platform("com.google.firebase:firebase-bom:32.7.0"))
    implementation("com.google.firebase:firebase-messaging-ktx")
}
```

## 3. Configurar el Manifiesto (AndroidManifest.xml)
Registra el servicio que extenderá de `FirebaseMessagingService`:

```xml
<service
    android:name=".MyFirebaseMessagingService"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>
```

## 4. Implementar el Servicio (Kotlin)
Crea una clase que extienda `FirebaseMessagingService` para manejar los tokens y las notificaciones entrantes:

```kotlin
class MyFirebaseMessagingService : FirebaseMessagingService() {

    // Se llama cuando se genera un nuevo token de registro
    override fun onNewToken(token: String) {
        super.onNewToken(token)
        Log.d("FCM", "Nuevo Token: $token")
        // Envía el token a tu servidor si es necesario
    }

    // Se llama cuando se recibe un mensaje mientras la app está en primer plano
    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        super.onMessageReceived(remoteMessage)

        remoteMessage.notification?.let {
            showNotification(it.title, it.body)
        }
    }

    private fun showNotification(title: String?, message: String?) {
        val channelId = "default_channel"
        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager

        // Crear canal de notificación para Android 8.0+
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(channelId, "Notificaciones", NotificationManager.IMPORTANCE_DEFAULT)
            notificationManager.createNotificationChannel(channel)
        }

        val notification = NotificationCompat.Builder(this, channelId)
            .setSmallIcon(R.drawable.ic_notification)
            .setContentTitle(title)
            .setContentText(message)
            .setAutoCancel(true)
            .build()

        notificationManager.notify(0, notification)
    }
}
```
