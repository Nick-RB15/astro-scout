# Astro Scout · Academia Espacial

App web inmersiva de entrenamiento espacial: 5 estaciones con modelos **3D
interactivos**, un **Instructor IA** al que puedes hacerle preguntas por texto
o voz, y **validación de gestos por cámara** (MediaPipe). Funciona 100 % en el
navegador, sin backend ni claves de API.

## Cómo ejecutarla

Debe servirse por HTTP (la cámara y los módulos ES no funcionan con doble clic
sobre el archivo):

```bash
# desde la carpeta del proyecto
python3 -m http.server 8099
# abre http://localhost:8099 en Chrome/Edge
```

## Estructura

```
index.html          Estructura y vistas (hub + estación)
css/styles.css      Estilos e interfaz (starfield, HUD, modales…)
js/data.js          Contenido editable: estaciones, base de conocimiento, gestos
js/app.js           Núcleo: progreso, XP/rangos, insignias, sonido, visor 3D
js/chatbot.js       Instructor IA: preguntas, pistas, quiz, voz, borrar chat
js/tracking.js      Cámara: navegación con la mano + validación por gesto
models/             Modelos .glb (reemplaza por los tuyos, mismos nombres)
qr/                 Códigos QR de cada estación
assets/logo.png     Logo
tools/              Script para regenerar los .glb de marcador de posición
```

## Reemplazar los modelos 3D

Los `.glb` de `models/` son marcadores de posición. Exporta tus modelos desde
Blender y guárdalos con **los mismos nombres**:
`casco.glb`, `brujula.glb`, `botiquin.glb`, `panel_oxigeno.glb`, `guante.glb`.
Para regenerar los de ejemplo: `python3 tools/make_placeholder_models.py`.

## Qué puedes hacer

- **Explorar el 3D**: rotar, reiniciar vista, pantalla completa, AR (móvil) y
  puntos interactivos (hotspots) sobre cada pieza del equipo.
- **Preguntar al Instructor IA**: preguntas libres, pistas progresivas, un
  mini-quiz por estación, datos curiosos, voz (hablar/escuchar) y borrar o
  limpiar mensajes.
- **Navegar con la mano**: activa la cámara desde el panel, mueve el cursor con
  el índice y pellizca (pulgar + índice) para seleccionar una estación.
- **Validar con gestos**: cada estación se completa con su gesto (palma, puño,
  índice, sonrisa). Suma XP, sube de rango y desbloquea insignias.
- **Progreso persistente**: se guarda en el navegador (localStorage).
