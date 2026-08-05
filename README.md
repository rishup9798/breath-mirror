# Breath Mirror

A webcam/mic browser toy: blow into your mic to fog the screen, pinch your
fingers to wipe/draw clear spots in the fog, and hold both hands in an
"L" shape to trigger a photo capture.

**Live demo:** https://breath-mirror-1.vercel.app/breath-mirror.html

Only two files, no build step, no npm install needed. It loads the
MediaPipe Hands library from a CDN at runtime, so you need an internet
connection while using it.

## Hand tracking

This project uses [MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)
(loaded via CDN, `@mediapipe/hands` + `@mediapipe/camera_utils`) to track
21 landmark points per hand, entirely in the browser — no video or
landmark data is ever sent to a server.

Two gestures drive the interaction:

- **Pinch** (thumb tip + index fingertip close together) — wipes/draws
  clear spots in the fog while moving your hand.
- **Both hands in an "L" shape** (thumb + index finger extended, other
  fingers curled) held for ~1.5 seconds — triggers a 3-2-1 countdown
  and takes a photo.

Because tracking runs live at 30fps+, it needs:
- **Decent, even lighting** — hands in shadow or backlit will track poorly.
- **Hands clearly in frame** and not overlapping too much with your face
  or each other.
- A reasonably modern device — hand tracking is CPU/GPU-intensive, so
  older phones or laptops may see reduced frame rates.

Camera and microphone permissions must be granted for the experience to
work at all — if either is denied, the app will show a status message
asking for access instead of the fog effect.

## Why you can't just double-click the file

Browsers block camera/microphone access (`getUserMedia`) on `file://`
pages for security reasons. You need to serve the files over
`http://localhost` instead. Any of the options below take under a minute.

## Option 1 — Python (usually already on your PC)

Open a terminal / Command Prompt in this folder and run:

```
python -m http.server 8000
```

(On some systems the command is `python3` instead of `python`.)

Then open your browser to:

```
http://localhost:8000
```

## Option 2 — Node.js

If you have Node installed:

```
npx serve .
```

It will print a local URL (usually `http://localhost:3000`) — open that.

## Option 3 — VS Code "Live Server" extension

If you use VS Code, install the "Live Server" extension, right-click
`index.html`, and choose "Open with Live Server."

## Using it

1. Allow camera and microphone access when the browser prompts you.
2. Blow toward your mic — fog appears on screen.
3. Pinch your thumb and index finger together and move your hand to
   wipe clear spots in the fog.
4. Hold both hands in an "L" shape (thumb + index finger) for ~1.5s to
   trigger a 3-2-1 countdown and take a photo.
5. Hover a thumbnail in the photo strip at the bottom and click "save"
   to download it.

Works best in Chrome or Edge (good WebRTC/camera support). Give it
decent lighting for hand tracking to work reliably.
