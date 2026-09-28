# Merry Functionality Walkthrough

> "Go ahead. Talk. I've got the rest." - Merry

![Merry idle companion](./images/idle-frame-1.png)

Merry is a local-first desktop companion for dictation, notes, meetings and voice-agent workflows. The product target is an independent app shipped separately for Windows, macOS and Linux, such as `.exe`, `.dmg` and Linux packages.

## What Merry Does

Merry listens when the user activates it, transcribes speech, optionally cleans or transforms the transcript, and sends the result where it is needed. Around that core loop it provides a control panel for notes, chat, meetings, model setup, dictionary terms, history and integrations.

Core surfaces:

- **Floating voice pill:** a compact always-on-top overlay for dictation, voice-agent commands and quick menu actions.
- **Assistant panel:** a conversational voice surface that can respond, use selected text, and call tools.
- **Control panel:** the main app window for onboarding, notes, history, chat, meetings, settings and model configuration.
- **Meeting overlay:** a lightweight prompt/recording surface for scheduled or detected calls.

## Companion States

The companion assets are grouped by state and reused across the product experience.

| State | Image | Meaning |
|---|---|---|
| Idle | ![Idle](./images/idle-frame-1.png) | Merry is ready but not actively recording. |
| Listening | ![Listening](./images/listening-frame-1.png) | Audio capture is active. |
| Thinking | ![Thinking](./images/thinking-frame-1.png) | Merry is transcribing, cleaning up, or asking an AI model to respond. |
| Error | ![Error](./images/error-frame-1.png) | A permission, recording, model, or routing step failed and needs user action. |

## Feature Walkthrough

### 1. Onboarding and Setup

On first launch, Merry opens a route-based onboarding flow. The local-only product path starts without requiring an account unless account support is explicitly enabled for a build.

Walkthrough:

1. Grant required platform permissions such as microphone, accessibility, and system audio where supported.
2. Pick languages and use cases.
3. Configure dictation hotkeys and run a dictation demo.
4. Set up notes and, when enabled, meeting/calendar-related flows.
5. Choose a model setup path: local, BYOK/self-hosted, cloud, or enterprise depending on policy and build configuration.

### 2. Dictation

![Merry listening](./images/listening-frame-2.png)

The dictation flow lives in the floating overlay. A hotkey or pill interaction starts recording, the audio pipeline captures microphone PCM, and the selected transcription route returns text.

Main behavior:

- Records microphone audio from the overlay.
- Supports local and self-hosted speech-to-text routes.
- Can show live transcript preview for supported streaming models.
- Runs cleanup/rephrase through the configured cleanup model when enabled.
- Uses native paste helpers to insert final text into the active app.
- Handles selected-text workflows such as rephrase or edit-in-place.

### 3. Voice Assistant

The assistant path uses a dedicated hotkey or command menu action. Spoken commands are routed through the voice-agent scope, and the assistant can use context such as selected text or optional screen context when supported by the chosen model.

Expected use cases:

- Ask Merry to rewrite selected text.
- Turn rough speech into notes, replies, plans or structured output.
- Search local notes through agent tools.
- Use configured tools such as dictionary, clipboard, calendar availability and note creation.

### 4. Notes

The notes workspace provides local capture and organization. Notes can be created manually, by dictation, from meetings, or through background actions.

Main behavior:

- Create and edit notes with the rich text editor.
- Organize notes into folders/spaces.
- Run note actions such as cleanup, formatting or summarization where supported.
- Search notes with keyword search and local semantic search.
- Open embedded chat around notes and meeting transcripts.

### 5. Meetings

![Merry thinking](./images/thinking-frame-1.png)

Meeting features combine detection, reminders, recording and transcript capture.

Main behavior:

- Detects meetings from configured calendar context, known meeting processes, and microphone activity where the platform supports it.
- Records meeting audio into notes.
- Captures microphone and system audio paths for richer transcripts.
- Uses speaker-aware transcript handling and duplicate/echo suppression logic.
- Saves the meeting transcript back into the notes system.

### 6. Chat

The control panel includes typed chat for general conversation and note-aware work. It uses the `chatIntelligence` inference scope, separate from voice-agent and dictation cleanup scopes.

Main behavior:

- Create and continue local chat conversations.
- Stream model responses.
- Use note cards and contextual references where supported.
- Fall back according to configured inference routes.

### 7. Settings and Model Routing

Merry separates model configuration by feature scope so one endpoint does not have to power everything.

Important scopes:

- **Dictation transcription:** local or self-hosted speech-to-text.
- **Dictation cleanup:** OpenAI-compatible chat completion route for polishing dictated text.
- **Voice assistant:** agent-addressed commands, selection edits and optional vision context.
- **Notes/chat intelligence:** note actions, chat and formatting.
- **Translation:** separate dictation translation path.

Settings also cover hotkeys, permissions, dictionary terms, upload behavior, meeting behavior, integrations, enterprise policy and developer/API key setup.

### 8. Local Data, Search and Privacy

Merry is designed around local storage and explicit operator-controlled network use.

Local pieces:

- SQLite-backed app data, notes, history and settings.
- Qdrant plus embeddings for semantic note search.
- Native helper binaries for OS-specific hotkeys, paste, microphone, calendar and system-audio tasks.
- Optional local model servers supplied by the operator.

Privacy rule of thumb: no outbound network path should be assumed safe unless the operator explicitly configured it.

## Pricing and Subscription

Merry's personal subscription service is INR-based and owner-configurable.

Current code-backed defaults:

- **Free trial:** Rs 0 / ₹0 for 30 days.
- **Fallback free allowance:** 2,000 words over 7 days when there is no live entitlement.
- **Referral reward:** 7 days added to the referrer when a valid new account claims their code.
- **Paid personal plans:** owner/admins can create plans with id, name, INR price, duration, active state and optional word limits.
- **Payment status:** Razorpay is the intended payment seam, but paid checkout is intentionally not enabled yet; every personal plan is free today unless the deployment owner wires a gateway.
- **Workspace billing:** separate from personal billing and used for workspace plans, seats, owners, checkout/portal flows and seat increases.

## Benefits

- **Private by design:** Merry prioritizes local storage and operator-controlled inference.
- **Fast daily capture:** Dictation, cleanup and paste are optimized for quick context switches.
- **Meeting memory:** Meeting capture turns conversations into searchable notes.
- **Flexible intelligence:** Different AI scopes can be configured for dictation, chat, agent work and translation.
- **Cross-platform target:** Merry is planned as separate Windows, macOS and Linux desktop releases.

## Upcoming Features and Plans

- Independent installers/packages for Windows, macOS and Linux.
- More polished meeting summaries and follow-up extraction.
- Stronger local-first defaults around model setup and data movement.
- Better multilingual dictation and translation, especially for Indic and mixed-language workflows.
- Smoother note search, assistant tools and voice-first editing flows.
