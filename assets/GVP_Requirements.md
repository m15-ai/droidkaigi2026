# Android Voice App Requirements Template

## App Identity

- App name: Google Voice Pipeline
- Short app name: GVP
- Package name: com.m15.gvp
- Primary purpose: Fully on-device voice AI agent — STT, LLM, and TTS all run locally with zero cloud calls during pipeline operation
- Target user: Developer / power user evaluating on-device voice agent architectures
- Primary language: English (US)
- Supported languages: English only for v1
- Future language expansion: Yes — Sherpa-ONNX supports multilingual models; LiteRT models for other languages may appear on Hugging Face

## Project Constraints

- Android Studio on: Windows 11 (Ladybug or later)
- Kotlin only: Yes
- Minimum SDK: 31 (Android 12)
- Target SDK: 35 (Android 15)
- Single Activity: Yes
- MainActivity only: Yes
- No fragments: Yes
- No XML layouts: Yes
- Jetpack Compose only: Yes
- Portrait only: Yes
- Landscape allowed: No
- Edge-to-edge UI: Yes
- Material Design 3: Yes — dynamic color on SDK 31+, static fallback otherwise, System/Light/Dark theme switching
- No BOM characters at start of files: Yes

## Architecture

- Architecture pattern: MVVM
- Main packages:
  - `ui` — Compose screens (Setup, VoiceAgent, Settings) and theme
  - `viewmodel` — VoiceAgentViewModel (at package root)
  - `data` — Room DB, entities, DAOs, repository
  - `data/local` — N/A (db and repo live under `data/db` and `data/repo`)
  - `data/remote` — N/A (no cloud services)
  - `domain` — N/A (no domain layer — ViewModel talks directly to engines)
  - `service` — VoiceAgentService (foreground service for mic)
  - `audio` — AudioCapture (16 kHz mono PCM) + SileroVad (energy-based VAD)
  - `llm` — LlmOrchestrator, MediaPipeLlmEngine, MlKitLlmEngine, StubLlmEngine, model catalog, downloader, prompt builder
  - `stt` — SherpaSttEngine, MlKitGenAiSttEngine, SttRouter, model catalog
  - `tts` — AndroidTtsEngine (offline Android TTS with sentence-level streaming)
  - `settings` — GvpPrefs (DataStore), ThemeMode
- Dependency injection: Manual ServiceLocator (singleton object)
- Repository pattern: Yes — ConversationRepository for session + message persistence
- Coroutine / Flow usage: Yes — StateFlow for UI, SharedFlow for STT events, callbackFlow for LLM streaming, Flow for DataStore
- Background service usage: Yes — VoiceAgentService foreground service while pipeline is active
- Foreground service notification: Yes — "GVP listening" notification with FOREGROUND_SERVICE_TYPE_MICROPHONE

## GUI

- Main screen layout: Top bar + pipeline status chips (VAD/STT/LLM/TTS) + toggleable orb visualizer or chat transcript + FAB column
- Top app bar: CenterAlignedTopAppBar with "GVP" title and settings gear icon
- Primary controls: 3 FABs — end session, speaker toggle, visualizer/chat toggle
- Secondary controls: Settings gear in top bar
- Output panel: LLM responses displayed as chat bubbles (assistant left-aligned, user right-aligned)
- Transcript panel: LazyColumn (reverseLayout) with live partials and "thinking..." indicator
- Status indicators: 4 pipeline status chips (VAD, STT, LLM, TTS) that light up when active; TTFT latency pill overlay
- Floating Action Button: 3 stacked FABs (Close, Speaker, Visualizer toggle)
- FAB on/off states: Speaker and Visualizer FABs change color (primary vs surfaceVariant) based on state
- Dark theme: Yes — dynamicDarkColorScheme + darkColorScheme fallback
- Light theme: Yes — dynamicLightColorScheme + lightColorScheme fallback
- Settings screen: Scrollable column with theme, pipeline mode, LLM model, STT model, ML Kit STT toggle, TTS mute/voice, VAD sliders, barge-in threshold, clear history
- Error display: Snackbar in VoiceAgentScreen
- Loading states: LLM/STT status cards on Setup screen (checking/downloading/needs_download/ready/unavailable)
- Empty states: "Tap the mic to start talking" centered text

## Navigation

- Number of screens: 4 (Setup, CustomPrompt, VoiceAgent, Settings)
- Start screen: Setup — LLM/STT status cards, system message preview, Start button
- Main screen: VoiceAgent — transcript + visualizer + pipeline chips
- Settings screen: App preferences (theme, models, VAD tuning, etc.)
- History/session screen: No — not in v1
- Navigation library: Jetpack Navigation Compose
- Back behavior: VoiceAgent pops to Setup; Settings pops to previous; Prompt pops back

## Voice Capture

- Microphone input: Yes — AudioRecord with VOICE_COMMUNICATION source
- Push-to-talk: No
- Toggle listening: Yes — session start/stop
- Always listening: No
- Background listening: No
- Foreground service required: Yes
- Audio format: PCM 16-bit mono, 16 kHz, 20ms frames
- Sample rate: 16000 Hz
- Silence detection: Yes — configurable silence threshold (default 1.5s)
- VAD: Energy-based RMS heuristic with echo suppression for full-duplex barge-in
- Wake word: No
- Volume button control: No
- Stop capture when app backgrounded: Yes — pipeline stops on onDestroy

## STT

- STT provider: Primary: Sherpa-ONNX (streaming Zipformer). Experimental: ML Kit GenAI Speech Recognition
- Local STT: Yes — fully on-device
- Cloud STT: No
- Realtime streaming: Yes — partial results streamed via VAD-gated recognizer
- Final chunks only: No — both interim and final
- Interim chunks: Yes — live partials shown in UI
- Input language: English (US)
- Language detection: No
- Confidence threshold: No — accept all recognized text
- Retry behavior: Model re-download on failure; STT status card tappable to retry
- Offline behavior: Fully offline after model download

## LLM

- LLM provider: Multi-engine orchestrator — MediaPipe LiteRT (primary), ML Kit Gemini Nano (secondary), Stub (fallback)
- Local LLM: Yes — fully on-device
- Cloud LLM: No
- Realtime model: No — standard request/response with token streaming
- Prompt role: System + user + assistant (rolling 5-pair history)
- System prompt: "You are a helpful voice assistant running entirely on-device. Keep responses concise and conversational..."
- User prompt format: Per-model chat template (Gemma/ChatML/Phi/Generic)
- Structured JSON output: No
- Function/tool calling: No
- Safety filters: No — relies on model's built-in behavior
- Language validation: No
- Context window strategy: Rolling last 5 exchange pairs + system message
- Conversation memory: In-memory list + Room DB persistence
- Session summary: No

## TTS

- TTS required: Yes
- TTS provider: Android TextToSpeech (offline)
- Local TTS: Yes
- Cloud TTS: No
- Voice: System default English (US); user-selectable from installed voices
- Output language: English
- Streaming audio: Yes — sentence-level streaming (sentences enqueued as they complete while LLM is still generating)
- Interruptible speech: Yes — barge-in stops TTS and cancels LLM
- Mute option: Yes — transcript-only mode toggle in settings
- Audio focus handling: Yes — AUDIOFOCUS_GAIN_TRANSIENT_MAY_DUCK, released when all utterances finish

## AI Pipeline

- Input flow:
  - Microphone — 16 kHz mono PCM with AEC/NS/AGC + 8x software gain
  - STT — Sherpa-ONNX or ML Kit GenAI (via SttRouter), VAD-gated
  - Text normalization — duplicate detection via Levenshtein similarity (0.85 threshold)
  - Language check — N/A (English only)
  - LLM — multi-engine orchestrator, streaming tokens with hallucination trimming (LlmStop)
  - Optional TTS — sentence-level streaming with markdown/emoji sanitization
  - UI output — chat bubbles + live streaming partial + TTFT latency overlay
  - Session log — Room DB (ChatSession + MessageItem)
- Translate interim text: No
- Translate final text: No
- Show source language: No
- Show translated language: No
- Store raw transcript: No — only final cleaned text
- Store cleaned transcript: Yes — via ConversationRepository
- Store LLM response: Yes — via ConversationRepository

## Database

- Room DB: Yes
- KSP: Yes
- Entities:
  - Session — ChatSession (id, title, createdAt)
  - TranscriptEntry — TranscriptChunk (id, sessionId, fromMs, toMs, text, isFinal)
  - LlmResponse — MessageItem (messageId, sessionId, role, text, createdAt) — stores both user and assistant messages
  - Settings — N/A (DataStore, not Room)
  - Attachment — N/A
- DAOs: SessionDao, MessageDao, TranscriptDao
- Repositories: ConversationRepository
- Migrations: No — fallbackToDestructiveMigration
- Seed data: No
- Session log from day one: Yes
- Export data: No for v1
- Delete data: Yes — clear all from settings with confirmation dialog

## Files and Storage

- App-specific storage: Yes — LLM .task models and STT model files in filesDir
- Shared storage: No
- Attachments: No
- Audio recording storage: No — audio processed in real-time, not saved
- Video/image storage: No
- Time-stamped filenames: No
- Max recording duration: N/A — continuous listening during session
- Cleanup policy: Manual only (clear history in settings)
- Export format: N/A

## Network and Cloud

- Internet required: No for pipeline — only for on-demand model downloads from Hugging Face
- Cloud sync: No
- Firebase: No
- Google Cloud: No
- API backend: No
- Authentication: No API keys. Optional Hugging Face token for gated model downloads (Gemma)
- Retry policy: Manual retry via status card tap
- Offline mode: Yes — fully offline after initial model download
- Sync queue: No
- Conflict resolution: N/A

## Permissions

- Microphone: Yes — RECORD_AUDIO (runtime)
- Camera: No
- Storage: No
- Notifications: Yes — POST_NOTIFICATIONS (runtime, API 33+)
- Foreground service: Yes — FOREGROUND_SERVICE + FOREGROUND_SERVICE_MICROPHONE (install-time)
- Internet: Yes — INTERNET (install-time, model downloads only)
- Bluetooth: No
- Location: No
- Permission rationale screens: No — system dialog only

## Security

- API keys stored on device: No. Optional HF token in plaintext DataStore (low sensitivity)
- Secure storage: No
- Keystore: No
- Backend proxy: No
- User data encryption: No — Room DB unencrypted
- Logs redaction: Transcript content at DEBUG level only
- PII handling: Transcripts stored locally only, never transmitted. User can delete all data.
- HIPAA/health-data concerns: No
- Debug logging allowed in release: No — INFO and above only

## Settings

- SharedPreferences: No
- DataStore: Yes — Jetpack DataStore (Preferences), 13 keys
- Theme setting: Yes — System/Light/Dark
- STT provider setting: Yes — Sherpa model selector + ML Kit GenAI toggle
- LLM provider setting: Yes — pipeline mode (Auto/MediaPipe/AICore) + LLM model selector (4 models)
- TTS provider setting: Yes — voice selector from installed Android TTS voices + mute toggle
- Language setting: No — English only
- Background listening setting: No
- Clear history: Yes — with confirmation dialog
- Export history: No

## Build and Gradle

- Kotlin: 2.0+ (K2 compiler)
- Compose: Yes — BOM 2024.10.01
- Material 3: Yes
- Room: 2.6.1
- KSP: Yes (for Room)
- Lifecycle ViewModel: 2.8.6
- Coroutines: 1.9.0
- Retrofit/OkHttp: No — HttpURLConnection for model downloads
- Firebase: No
- CameraX: No
- Media3: No
- Permissions library: No — ActivityResultContracts directly
- Required Gradle notes: NDK ABI filter arm64-v8a only; local AAR for sherpa-onnx-1.13.2; MediaPipe tasks-genai 0.10.27; ML Kit genai-prompt 1.0.0-beta2; ML Kit genai-speech-recognition 1.0.0-alpha1

## Testing

- Unit tests: No
- ViewModel tests: No
- Repository tests: No
- Room tests: No
- Fake STT provider: No — StubLlmEngine serves as LLM fallback but no fake STT
- Fake LLM provider: Yes — StubLlmEngine (canned response streamed word-by-word)
- Fake TTS provider: No
- UI tests: No
- Manual test checklist: Pipeline flow, barge-in, echo suppression, model download, settings persistence, offline operation, speaker routing, theme switching

## Logging and Debugging

- On-screen debug panel: No
- Logcat tags: AudioCapture, GVP.VAD, GVP.STT, GVP.STT.MLKit, GVP.LLM, GVP.TTS, GVP.Pipeline, GVP.BargeIn
- Session logs: Yes — all messages persisted to Room
- STT logs: Yes — recognition events, model loading, partials
- LLM request logs: Yes — prompt dispatches, engine selection
- LLM response logs: Yes — generation progress, hallucination marker hits, completions
- Error logs: Yes — engine init failures, download errors, generation errors
- Release logging policy: INFO and above only
