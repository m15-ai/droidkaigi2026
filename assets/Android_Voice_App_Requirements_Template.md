# Android Voice App Requirements Template

## App Identity

- App name:
- Short app name:
- Package name:
- Primary purpose:
- Target user:
- Primary language:
- Supported languages:
- Future language expansion:

## Project Constraints

- Android Studio on:
- Kotlin only:
- Minimum SDK:
- Target SDK:
- Single Activity:
- MainActivity only:
- No fragments:
- No XML layouts:
- Jetpack Compose only:
- Portrait only:
- Landscape allowed:
- Edge-to-edge UI:
- Material Design 3:
- No BOM characters at start of files:

## Architecture

- Architecture pattern: MVVM
- Main packages:
  - `ui`
  - `viewmodel`
  - `data`
  - `data/local`
  - `data/remote`
  - `domain`
  - `service`
  - `audio`
  - `llm`
  - `stt`
  - `tts`
  - `settings`
- Dependency injection:
- Repository pattern:
- Coroutine / Flow usage:
- Background service usage:
- Foreground service notification:

## GUI

- Main screen layout:
- Top app bar:
- Primary controls:
- Secondary controls:
- Output panel:
- Transcript panel:
- Status indicators:
- Floating Action Button:
- FAB on/off states:
- Dark theme:
- Light theme:
- Settings screen:
- Error display:
- Loading states:
- Empty states:

## Navigation

- Number of screens:
- Start screen:
- Main screen:
- Settings screen:
- History/session screen:
- Navigation library:
- Back behavior:

## Voice Capture

- Microphone input:
- Push-to-talk:
- Toggle listening:
- Always listening:
- Background listening:
- Foreground service required:
- Audio format:
- Sample rate:
- Silence detection:
- VAD:
- Wake word:
- Volume button control:
- Stop capture when app backgrounded:

## STT

- STT provider:
- Local STT:
- Cloud STT:
- Realtime streaming:
- Final chunks only:
- Interim chunks:
- Input language:
- Language detection:
- Confidence threshold:
- Retry behavior:
- Offline behavior:

## LLM

- LLM provider:
- Local LLM:
- Cloud LLM:
- Realtime model:
- Prompt role:
- System prompt:
- User prompt format:
- Structured JSON output:
- Function/tool calling:
- Safety filters:
- Language validation:
- Context window strategy:
- Conversation memory:
- Session summary:

## TTS

- TTS required:
- TTS provider:
- Local TTS:
- Cloud TTS:
- Voice:
- Output language:
- Streaming audio:
- Interruptible speech:
- Mute option:
- Audio focus handling:

## AI Pipeline

- Input flow:
  - Microphone
  - STT
  - Text normalization
  - Language check
  - LLM
  - Optional TTS
  - UI output
  - Session log
- Translate interim text:
- Translate final text:
- Show source language:
- Show translated language:
- Store raw transcript:
- Store cleaned transcript:
- Store LLM response:

## Database

- Room DB:
- KSP:
- Entities:
  - Session
  - TranscriptEntry
  - LlmResponse
  - Settings
  - Attachment
- DAOs:
- Repositories:
- Migrations:
- Seed data:
- Session log from day one:
- Export data:
- Delete data:

## Files and Storage

- App-specific storage:
- Shared storage:
- Attachments:
- Audio recording storage:
- Video/image storage:
- Time-stamped filenames:
- Max recording duration:
- Cleanup policy:
- Export format:

## Network and Cloud

- Internet required:
- Cloud sync:
- Firebase:
- Google Cloud:
- API backend:
- Authentication:
- Retry policy:
- Offline mode:
- Sync queue:
- Conflict resolution:

## Permissions

- Microphone:
- Camera:
- Storage:
- Notifications:
- Foreground service:
- Internet:
- Bluetooth:
- Location:
- Permission rationale screens:

## Security

- API keys stored on device:
- Secure storage:
- Keystore:
- Backend proxy:
- User data encryption:
- Logs redaction:
- PII handling:
- HIPAA/health-data concerns:
- Debug logging allowed in release:

## Settings

- SharedPreferences:
- DataStore:
- Theme setting:
- STT provider setting:
- LLM provider setting:
- TTS provider setting:
- Language setting:
- Background listening setting:
- Clear history:
- Export history:

## Build and Gradle

- Kotlin:
- Compose:
- Material 3:
- Room:
- KSP:
- Lifecycle ViewModel:
- Coroutines:
- Retrofit/OkHttp:
- Firebase:
- CameraX:
- Media3:
- Permissions library:
- Required Gradle notes:

## Testing

- Unit tests:
- ViewModel tests:
- Repository tests:
- Room tests:
- Fake STT provider:
- Fake LLM provider:
- Fake TTS provider:
- UI tests:
- Manual test checklist:

## Logging and Debugging

- On-screen debug panel:
- Logcat tags:
- Session logs:
- STT logs:
- LLM request logs:
- LLM response logs:
- Error logs:
- Release logging policy:
