# Neon Face Scan

Hand Lab experiment. Your face is tracked and drawn as a low-poly neon
wireframe, while a denser point-cloud copy of it, sampled from MediaPipe's full
face mesh, floats in the upper-left corner. Hold up fingers to change the
colour, and open your palm to blow the scan apart.

**Live:** https://rukiya98.github.io/neon-face-scan/neon-face-scan.html

## How it works

1. MediaPipe Face Landmarker tracks the face every frame. The wireframe is 88
   triangles over 61 of its landmarks, using a fixed topology built once from a
   neutral, frontal face, with the eyes and mouth cut out as holes. It is
   applied by landmark index, so it deforms with the face and stays clean as
   you turn your head.
2. The floating scan samples points along every edge of MediaPipe's full face
   tessellation, which makes it dense around the eyes, nose and lips and sparse
   across the cheeks and forehead. The points go into an intensity buffer that
   is drawn twice: a blurred pass for the glow, then the sharp points on top.
3. MediaPipe Hand Landmarker runs on every other new camera frame, about 15
   times a second on a 30 fps webcam, and the app counts extended fingers from
   the landmarks it returns. A finger counts as extended when its tip is
   clearly further from the wrist than its middle joint (at least 12% further,
   35% for the thumb), which holds up however the hand is rotated. A majority
   vote over the last 7 readings stops the colour flickering. Only one hand is
   read.
4. Face landmarks are smoothed by how fast each one is moving. Slow drift is
   damped, lightly on the wireframe and more heavily on the floating scan, so
   the mesh stays calm, while fast motion like a blink passes through almost
   unfiltered.
5. With darkening on, the camera feed goes through a contrast curve, so a lit
   face stays visible while a dim room drops towards black.

## Gestures

| Fingers | Effect |
|---------|--------|
| 0 (fist, or no hand in view) | cyan, pulls the scan back together |
| 1 | green, pulls the scan back together |
| 2 | yellow, leaves the scan as it is |
| 3 | amber, leaves the scan as it is |
| 4 | red, leaves the scan as it is |
| 5 (open palm) | magenta, scatters the scan |

A gesture acts when you change to it, so holding one still doesn't fight the
`SPACE` key.

## Controls

| Key | Action |
|-----|--------|
| `SPACE` | scatter / reform the scan |
| `B` | toggle darkening (on by default) |
| `F` | fullscreen |
| `H` | show / hide the readouts and key hints |

Keys work once the camera has started.

## Requirements

A webcam, an internet connection and a modern browser, with Chrome or Edge
working best. The MediaPipe library and models are downloaded from jsDelivr
and Google's model storage when you press Start. After that, tracking runs on
your device and no video leaves the browser. Works best with your face evenly
lit.

Built with MediaPipe Face Landmarker, MediaPipe Hand Landmarker and Canvas 2D.
