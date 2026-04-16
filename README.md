# OnDevice Dictate

Browser-based speech dictation that keeps audio processing local to the tab. It loads a Whisper model in the browser, captures microphone input, runs voice activity detection on-device, and appends the transcript to the page.

## What it needs

- A modern Chromium-based browser for the best experience
- Microphone permission
- A secure context: `http://localhost` or `https://`

Opening the HTML file directly from disk will not work reliably because browser microphone APIs require a secure origin.

## Run it

1. Serve the folder locally.
2. Open the page in the browser.
3. Click `Load Model`.
4. Click `Start Dictating` and speak.

If you already have a simple local server, any of these are fine:

- `python3 -m http.server 8000`
- `npx serve .`
- `npx http-server .`

Then open `http://localhost:8000` or the equivalent URL.

## How it works

- Captures audio from `getUserMedia`
- Uses `AudioWorklet` to keep the audio path responsive
- Runs Silero VAD in the browser to detect speech segments
- Sends each speech segment to a local Whisper model loaded through Transformers.js

## Notes

- The first model load can take a while because the weights are downloaded once.
- `whisper-tiny.en` is the best default for a quick MVP.
- If WebGPU is available, the demo can use it for faster inference.

## Troubleshooting

- If the page says to use localhost or HTTPS, start a local server instead of opening the file directly.
- If mic access is blocked, check browser permissions for the site.
- If model loading fails, try the smaller `whisper-tiny.en` option first.
