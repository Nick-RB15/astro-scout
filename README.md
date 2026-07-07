# 🚀 Astro Scout · Academia Espacial

**Astro Scout** es una experiencia web de entrenamiento inmersivo que convierte el aprendizaje sobre exploración espacial en un juego por estaciones. Cada estación combina un **modelo 3D interactivo**, un **instructor con IA** (chat + voz) y la **validación de una habilidad mediante gestos de la mano detectados por la cámara**, todo ejecutándose **100 % en el navegador** (sin servidor ni backend).

El cadete avanza estación por estación, gana XP, sube de rango y desbloquea insignias hasta convertirse en un verdadero *Astro Scout*.

---

## ✨ Características

- **5 estaciones de entrenamiento**, cada una con una habilidad real de exploración espacial.
- **Visor 3D interactivo** (`<model-viewer>`): rotación automática, control de cámara, puntos de interés (*hotspots*), pantalla completa y **Realidad Aumentada (AR)** en dispositivos compatibles.
- **Instructor IA** por estación: responde preguntas por palabras clave, da pistas progresivas, lanza mini-quizzes, cuenta datos curiosos y usa **síntesis y reconocimiento de voz** (Web Speech API).
- **Validación por gestos con MediaPipe**: mini-juegos que se completan haciendo gestos frente a la cámara (puño, mano abierta, índice arriba, victoria, pulgar arriba).
- **Navegación con la mano** en el panel principal: mueve un cursor con el dedo y "pellizca" para seleccionar una estación.
- **Progresión y gamificación**: XP, rangos (Recluta → Cadete → Piloto → Navegante → Comandante), barra de progreso, insignias, confeti y efectos de sonido.
- **Códigos QR** por estación para montar un circuito físico: al escanear un QR se abre directamente esa estación.
- **Progreso persistente** en `localStorage` (no se pierde al recargar).
- **Fondo animado** de estrellas y estrellas fugaces.

---

## 🛰 Estaciones

| # | Estación | Habilidad | Gesto de validación |
|---|----------|-----------|---------------------|
| 01 | 🪖 Casco de vuelo | Sujeción de equipo | Cerrar el puño para agarrar el casco y abrir la mano para colocarlo |
| 02 | 🧭 Brújula de navegación | Orientación | Índice arriba para disparar a 5 estrellas objetivo |
| 03 | 🧰 Botiquín espacial | Primeros auxilios | Índice (pinza) → puño + pulgar arriba → mano abierta (vendar) |
| 04 | 🫧 Panel de oxígeno | Manejo de recursos | Índice (detector) → puño (soldar) → victoria (abrir válvula) |
| 05 | 🧤 Guante de sincronización | Trabajo en equipo | Puño para agarrar cables y mano abierta para conectarlos |

Las estaciones se desbloquean en orden: hay que completar la anterior para acceder a la siguiente.

---

## 📁 Estructura del proyecto

```
.
├── index.html                     # Punto de entrada de la app
├── css/
│   └── styles.css                 # Estilos
├── js/
│   ├── app.js                     # Estado, HUD, ruteo, HUB, visor 3D, progreso
│   ├── data.js                    # Datos de estaciones, base de conocimiento IA, datos curiosos
│   ├── chatbot.js                 # Instructor IA (chat, voz, pistas, quiz)
│   └── tracking.js                # Tracking de gestos y mini-juegos (MediaPipe)
├── models/                        # Modelos 3D .glb de cada estación
│   ├── casco.glb
│   ├── brujula.glb
│   ├── botiquin.glb
│   ├── panel_oxigeno.glb
│   └── guante.glb
├── qr/                            # Códigos QR de cada estación (png)
├── assets/
│   └── logo.png
└── tools/
    └── make_placeholder_models.py # Genera modelos .glb de marcador de posición
```

---

## 🚦 Cómo ejecutarlo

Como el proyecto usa **módulos ES** (`import`/`export`) y accede a la **cámara**, debe servirse por **HTTP/HTTPS** (no funciona abriendo `index.html` con `file://`).

Levanta un servidor estático local desde la raíz del proyecto:

```bash
# Opción 1: Python 3
python3 -m http.server 8000

# Opción 2: Node
npx serve .
```

Luego abre <http://localhost:8000> en **Chrome o Edge** (necesarios para MediaPipe/WebAssembly y el reconocimiento de voz).

> **Nota sobre la cámara y la voz:** los navegadores solo permiten la cámara y el reconocimiento de voz en `localhost` o en sitios `https://`. Al desplegarlo en producción, usa HTTPS.

---

## 🧊 Modelos 3D

Los archivos `.glb` de la carpeta `models/` pueden ser tus modelos reales exportados de Blender. Si aún no los tienes, genera modelos de marcador de posición:

```bash
pip install trimesh numpy
python3 tools/make_placeholder_models.py
```

Esto crea `casco.glb`, `brujula.glb`, `botiquin.glb`, `panel_oxigeno.glb` y `guante.glb` en `models/`. Reemplázalos por tus modelos reales manteniendo los mismos nombres.

---

## 📱 Circuito físico con QR

En el panel principal, el botón **📱 Códigos QR** abre las imágenes de `qr/`. Imprime cada código y colócalo en su estación física; al escanearlo, el navegador abre directamente esa estación de la app (`index.html#<estacion>`).

---

## 🎮 Controles y gestos

- **Navegar con la mano** (panel principal): mueve el cursor con el dedo índice y pellizca (pulgar + índice) para seleccionar.
- **Dentro de una estación**: sigue la misión y realiza el gesto indicado frente a la cámara para validar (se mantiene ~1,2 s para confirmar).
- **Instructor IA**: escribe o usa el micrófono. Prueba con *"pista"*, *"quiz"*, *"¿para qué sirve?"*, *"dato curioso"* o *"listo"*.
- **Sonido**: se puede silenciar desde el HUD. El progreso se reinicia con **↺ Reiniciar**.

---

## 🛠 Tecnologías

- HTML, CSS y JavaScript (módulos ES) — sin frameworks ni build step.
- [`<model-viewer>`](https://modelviewer.dev/) 3.4.0 para los modelos 3D y AR.
- [MediaPipe Tasks Vision](https://developers.google.com/mediapipe) para el reconocimiento de manos y rostro.
- Web Speech API (síntesis y reconocimiento de voz).
- `localStorage` para persistir el progreso.

---

## 🌐 Compatibilidad

- Recomendado: **Chrome** o **Edge** de escritorio o Android.
- Requiere **cámara**, **WebAssembly** y contexto seguro (`localhost` o `https`).
- El reconocimiento de voz puede no estar disponible en todos los navegadores; el resto de la app sigue funcionando.
