# DroidKaigi 2026 — Voice Agents on Android with Pipecat

Conference materials and demo code for our DroidKaigi 2026 session in Tokyo.

> **TODO:** talk title, abstract, speaker bio, session link, date/room.

## The repos

| Repo | What it is |
|---|---|
| **[droidkaigi2026](https://github.com/m15-ai/droidkaigi2026)** | This hub — slides, docs, links (start here) |
| **[droidkaigi2026-homer-server](https://github.com/m15-ai/droidkaigi2026-homer-server)** | ⚾ **Homer**, the baseball voice agent server: a vanilla [NousResearch hermes-agent](https://github.com/NousResearch/hermes-agent) brain behind a Pipecat WebRTC voice stack, with live MLB data skills (Dodgers / Shohei Ohtani superfan). Runs on a Raspberry Pi 5. |
| **[droidkaigi2026-app-gvp](https://github.com/m15-ai/droidkaigi2026-app-gvp)** | 📱 **GVP** — Android demo app |
| **[droidkaigi2026-app-cliff](https://github.com/m15-ai/droidkaigi2026-app-cliff)** | 📱 **Cliff** — Android demo app |
| **[droidkaigi2026-app-pipecat](https://github.com/m15-ai/droidkaigi2026-app-pipecat)** | 📱 **Pipecat** — the Android voice thin client that talks to Homer |

> **TODO:** one-line descriptions for GVP and Cliff.

## The demo in one picture

```
📱 Android Pipecat client ──WebRTC──▶ Homer server (:7864)
                                      [ Deepgram STT → agent brain → Cartesia TTS ]
                                                          │
                                            Nous hermes-agent (gpt-4o-mini)
                                                          │ terminal tool
                                            mlb.py → statsapi.mlb.com (live MLB data)
```

The voice pipeline treats a whole agent as if it were just an LLM — swap the
LLM slot for an agent bridge and the phone app never knows the difference.
Full architecture: [HOMER.md](https://github.com/m15-ai/droidkaigi2026-homer-server/blob/main/HOMER.md) ·
Build-it-yourself: [SERVER.md](https://github.com/m15-ai/droidkaigi2026-homer-server/blob/main/SERVER.md)

## In this repo

- [`slides/`](slides/) — the conference deck (.pptx + PDF export)
- [`docs/demo-runbook.md`](docs/demo-runbook.md) — demo-day checklist and fallback plan
- [`assets/`](assets/) — QR codes, images, logos
