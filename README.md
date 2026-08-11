# Android Gets a Voice: Building Real-Time Agents

Conference materials and demo code for our **DroidKaigi 2026** session in Tokyo.

## The repos

| Repo | What it is |
|---|---|
| **[droidkaigi2026](https://github.com/m15-ai/droidkaigi2026)** | This hub — slides, docs, links (start here) |
| **[droidkaigi2026-homer-hermes](https://github.com/m15-ai/droidkaigi2026-homer-hermes)** | **Homer**, the baseball voice agent server: a vanilla [NousResearch hermes-agent](https://github.com/NousResearch/hermes-agent) brain behind a Pipecat WebRTC voice stack, with live MLB data skills (Dodgers / Shohei Ohtani superfan). Runs on a Raspberry Pi 5. |
| **[droidkaigi2026-homer-openclaw](https://github.com/m15-ai/droidkaigi2026-homer-openclaw)** | The **same Homer** rebuilt on an [OpenClaw](https://openclaw.ai) brain — identical persona, MLB tool, voice, and client contract, different agent framework. Point the app at one port or the other and compare frameworks live. |
| **[droidkaigi2026-app-gvp](https://github.com/m15-ai/droidkaigi2026-app-gvp)** | **GVP** — the fully **on-device** voice assistant: Sherpa-ONNX STT → on-device LLM (MediaPipe/LiteRT or Gemini Nano via AICore) → Android TTS, with VAD and barge-in. No cloud, no API key — works in airplane mode; verified on a Pixel 10 (Tensor G5). |
| **[droidkaigi2026-app-cliff](https://github.com/m15-ai/droidkaigi2026-app-cliff)** | **Cliff** — the **cloud-streaming** voice assistant: Deepgram Flux STT → Claude (SSE streaming) → Deepgram Aura-2 TTS, end-to-end streaming with full barge-in. |
| **[droidkaigi2026-app-pipecat](https://github.com/m15-ai/droidkaigi2026-app-pipecat)** | **Pipecat** — the Android voice **thin client** that talks to Homer over WebRTC, with selectable per-agent visualizers (oscilloscope · a ballpark "Running the Bases" scene · an amber orb). |

The two Homer repos are one agent on two frameworks — same persona, same live
MLB data tool, same TTS voice, same WebRTC contract. The thin client just
changes the server URL (`:7864` hermes-agent, `:7865` OpenClaw), so the demo
doubles as a live agent-framework comparison.

## In this repo

- [`slides/`](slides/) — the conference deck (.pptx + PDF export)
- [`assets/`](assets/) — the requirements template, plus a completed example (GVP)
