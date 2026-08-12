# Android Gets a Voice: Building Real-Time Agents

Conference materials and demo code for our **DroidKaigi 2026** session in Tokyo.

## The repos

| Repo | What it is |
|---|---|
| **[droidkaigi2026](https://github.com/m15-ai/droidkaigi2026)** | This hub — slides, docs, links (start here) |
| **[droidkaigi2026-app-gvp](https://github.com/m15-ai/droidkaigi2026-app-gvp)** | **GVP** — the fully **on-device** voice assistant: Sherpa-ONNX STT → on-device LLM (MediaPipe/LiteRT or Gemini Nano via AICore) → Android TTS, with VAD and barge-in. No cloud, no API key — works in airplane mode; verified on a Pixel 10 (Tensor G5). |
| **[droidkaigi2026-app-cliff](https://github.com/m15-ai/droidkaigi2026-app-cliff)** | **Cliff** — the **cloud-streaming** voice assistant: Deepgram Flux STT → Claude (SSE streaming) → Deepgram Aura-2 TTS, end-to-end streaming with full barge-in. |
| **[droidkaigi2026-app-pipecat](https://github.com/m15-ai/droidkaigi2026-app-pipecat)** | **Pipecat** — the Android voice **thin client** that talks to Homer over WebRTC, with selectable per-agent visualizers (oscilloscope · a ballpark "Running the Bases" scene · an amber orb). |
| **[droidkaigi2026-homer-openclaw](https://github.com/m15-ai/droidkaigi2026-homer-openclaw)** | **Homer**, the baseball voice agent server: an [OpenClaw](https://openclaw.ai) brain behind a Pipecat WebRTC voice stack, with live MLB data skills (Dodgers / Shohei Ohtani superfan). Runs on a Raspberry Pi 5. |

Homer is the voice agent server; the Pipecat thin client points at it over
WebRTC (`:7865`), so the whole stack runs on a Raspberry Pi 5 and you talk to it
from the Android app.

## In this repo

- [`slides/`](slides/) — the conference deck (.pptx + PDF export)
- [`assets/`](assets/) — the requirements template, plus a completed example (GVP)
