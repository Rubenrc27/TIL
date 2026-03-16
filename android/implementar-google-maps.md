# Implementación de Google Maps en Android (Kotlin)

Integrar Google Maps requiere configurar la API Key en la consola de Google Cloud y realizar ajustes en el proyecto de Android Studio.

## 1. Dependencias
Añade la librería de Google Play Services en tu archivo `build.gradle.kts` (Module: app):

```kotlin
dependencies {
    implementation("com.google.android.gms:play-services-maps:18.2.0")
}
```

## 2. Permisos y API Key (AndroidManifest.xml)
Añade los permisos de internet y localización, y el metadato con tu clave de API:

```xml
<manifest ...>
    <!-- Permisos -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

    <application ...>
        <!-- Tu API Key de Google Maps -->
        <meta-data
            android:name="com.google.android.geo.API_KEY"
            android:value="TU_API_KEY_AQUI" />
    </application>
</manifest>
```

## 3. Layout (XML)
Añade el fragmento del mapa en tu archivo de diseño (ej: `activity_main.xml`):

```xml
<fragment
    android:id="@+id/map"
    android:name="com.google.android.gms.maps.SupportMapFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

## 4. Inicialización en la Activity
Implementa la interfaz `OnMapReadyCallback` para manejar el mapa una vez cargado:

```kotlin
class MainActivity : AppCompatActivity(), OnMapReadyCallback {

    private lateinit var mMap: GoogleMap

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Obtener el SupportMapFragment y notificar cuando el mapa esté listo
        val mapFragment = supportFragmentManager
            .findFragmentById(R.id.map) as SupportMapFragment
        mapFragment.getMapAsync(this)
    }

    override fun onMapReady(googleMap: GoogleMap) {
        mMap = googleMap

        // Añadir un marcador en una ubicación específica (ej: Madrid)
        val madrid = LatLng(40.416775, -3.703790)
        mMap.addMarker(MarkerOptions().position(madrid).title("Marcador en Madrid"))
        mMap.moveCamera(CameraUpdateFactory.newLatLngZoom(madrid, 10f))
    }
}
```
