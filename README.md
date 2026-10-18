# FlowEdge — AMOLED Light Show

**Versión inicial 0.1.0** · Android nativo · Kotlin · Jetpack Compose

FlowEdge es una base funcional de desarrollo para un espectáculo de luces en los bordes de una pantalla AMOLED. La primera prioridad es el motor **Waterfall**: múltiples filamentos con desplazamiento vertical y deformación lateral, variaciones de fase, capas de color y un campo central negro delimitado.

## Estado real de esta entrega

- Incluye un proyecto Android nativo con Kotlin y Jetpack Compose.
- Incluye motor de animación Canvas con Waterfall y variaciones visuales para ocho estilos seleccionables.
- Incluye demostración manual, botón de detener, interruptor maestro, ajuste de brillo del efecto, velocidad y selección de estilo.
- Incluye activación experimental por acelerómetro cuando el usuario activa «Efectos al poner boca abajo». Exige una posición estable durante aproximadamente un segundo y aplica una ventana antirrebote.
- Persiste interruptor maestro, activación por sensor y brillo mediante DataStore.
- Incluye una sección inicial de recomendaciones de protección AMOLED.
- **No se ha compilado ni probado en un teléfono real en este entorno.** No se entrega APK porque este entorno no dispone de Android SDK ni Gradle instalado. El ZIP contiene el código fuente y configuración, no un binario instalable.
- El proyecto todavía no implementa los 15 efectos completos del diseño original, perfiles personalizados, temporizadores, estadísticas de consumo, batería baja ni Quick Settings. Las ocho opciones actuales son una primera versión y algunas comparten el motor de cintas con variaciones; no deben presentarse como los 15 espectáculos finales.
- No reemplaza el AOD del fabricante ni puede dibujar cuando Android mantiene la pantalla apagada. El espectáculo solo se renderiza dentro de la actividad visible.

## Requisitos

- Android Studio reciente compatible con Android Gradle Plugin 8.7.3.
- JDK 17.
- Android SDK Platform 35 y herramientas de compilación correspondientes.
- Conexión a internet la primera vez para que Gradle descargue dependencias.

## Abrir y ejecutar

1. Descomprime `FlowEdge-AMOLED.zip`.
2. Abre la carpeta `FlowEdge` desde Android Studio (`File > Open`).
3. Acepta la sincronización de Gradle y permite que descargue dependencias.
4. Conecta un dispositivo Android o crea un emulador y pulsa **Run**.
5. En la app, pulsa **Mostrar** para ejecutar el efecto. El selector permite cambiar de estilo.

Si Android Studio informa de una incompatibilidad de Gradle o SDK, revisa que esté seleccionado JDK 17 y que Android SDK Platform 35 esté instalado. El proyecto no incluye `gradle-wrapper.jar`: no fue posible generar un wrapper auténtico porque Gradle no está instalado en el entorno que creó este ZIP. Android Studio puede abrir el proyecto y generar el wrapper al disponer de Gradle; no se incluye un wrapper ficticio.

## Publicar en GitHub

Crea un repositorio vacío llamado `FlowEdge-AMOLED` en tu cuenta y ejecuta en una terminal dentro de la carpeta descomprimida:

```bash
git init
git add .
git commit -m "Initial FlowEdge Android project"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/FlowEdge-AMOLED.git
git push -u origin main
```

Reemplaza `TU_USUARIO` por tu usuario real de GitHub. No se ha creado ni publicado un repositorio remoto desde este entorno.

## Cómo funciona el motor Waterfall

El motor usa un `Canvas` de Compose con un conjunto acotado de trazos. Cada trazo se deforma mediante sumas de funciones sinusoidales con distintas frecuencias, fases y velocidades; se dibuja con gradientes y capas de resplandor. Los dos laterales se dibujan por separado y se recortan a sus respectivas zonas. Finalmente se vuelve a pintar el rectángulo central en `#000000` para imponer un límite negro explícito. La animación se actualiza con `withFrameNanos`, ligado al ciclo de fotogramas de Compose.

Es una primera aproximación matemática, no un shader GPU personalizado. Debe medirse en el dispositivo para validar FPS, temperatura y consumo antes de considerar terminada la optimización.

## Sensores y límites de Android

- El acelerómetro solo se registra mientras la automatización y el interruptor maestro están activos y la actividad está en primer plano.
- El umbral de orientación puede necesitar calibración según el montaje físico y el fabricante.
- El sistema no enciende la pantalla apagada de forma garantizada ni sustituye Always-On Display.
- No se solicita permiso de accesibilidad, administrador del dispositivo, root ni superposición.

## Protección AMOLED

El brillo del dibujo se limita al 80 % de la intensidad de color del motor y el patrón está animado. Esto **no elimina el riesgo de desgaste desigual**; la duración, el brillo físico del teléfono y las características del panel siguen importando. No mantengas el espectáculo activo durante horas sin supervisión. En esta versión el control regula la intensidad del dibujo, no el brillo físico de Android.

## Estructura

```text
FlowEdge/
├── app/
│   ├── build.gradle.kts
│   └── src/
│       ├── main/AndroidManifest.xml
│       ├── main/java/com/flowedge/app/MainActivity.kt
│       ├── main/res/values/styles.xml
│       └── test/java/com/flowedge/app/
├── build.gradle.kts
├── gradle.properties
├── settings.gradle.kts
├── .gitignore
└── README.md
```
