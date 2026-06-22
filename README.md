# Relay · AI Meeting Assistant

Relay is a Chrome extension that captures your online meetings (Google Meet, Zoom, Teams) with a single click. It records mixed meeting audio (remote participants + your microphone) directly from your browser and saves it as a high-quality WebM file. This clean, complete audio forms the essential foundation for future AI processing — transcription, action item extraction, and automated follow-ups.

---

## Core Problem

- Action items are remembered differently by each person in a meeting
- Follow-up emails are written manually — or simply forgotten
- Decisions get buried in chat threads with no clear record
- New team members have no way to catch up on past choices

---

## How the Full System Will Work

- Chrome extension captures mixed audio from the meeting tab and microphone
- Audio is uploaded to a backend and transcribed using Whisper AI
- OpenAI/GPT processes the transcript to extract tasks, owners, deadlines, and decisions
- Automated follow-up emails are sent the moment the meeting ends
- A dashboard tracks completion status and flags unresolved blockers
- Every meeting is stored and searchable — so organisational memory is never lost

---

## What This Repository Does Right Now

This Chrome extension records the mixed stereo audio of:

- Meeting tab audio (remote participants from Google Meet, Zoom, Teams, and other supported platforms)
- Your microphone (your own voice)

It saves everything as a WebM/Opus file directly to your computer via Chrome's download API.

To build a reliable AI meeting assistant, you first need pristine, complete audio. Browser APIs (tabCapture + getUserMedia) are tricky to mix correctly. This extension solves that problem so the future AI pipeline has clean data to work with.

---

## Current Features

- Detects supported meeting platforms (Google Meet, Zoom, Teams, Webex, GoToMeeting, BlueJeans, Whereby, Discord, Slack)
- Records tab audio and microphone simultaneously
- Mixes both into a single stereo track using the Web Audio API
- Saves as WebM/Opus (128 kbps) – playable in any modern player
- Shows a visible recording banner on the meeting page (ethical, non-optional)
- Handles tab closure mid-recording gracefully
- Persists state across service worker evictions
- **Updated UI:** Dark brutalist redesign with Re/ay branding

---

## Installation

1. Download or clone this folder to your computer.
2. Open Chrome and go to `chrome://extensions`.
3. Turn on Developer mode (top-right toggle).
4. Click Load unpacked and select the relay folder (the one containing `manifest.json`).
5. The Relay icon will appear in your toolbar. Pin it for easy access.

---

## Supported Meeting Platforms

Defined in `config.js` as a simple, easy-to-extend list:

- meet.google.com
- *.zoom.us
- teams.microsoft.com
- webex.com
- gotomeeting.com
- bluejeans.com
- whereby.com
- discord.com
- slack.com (huddles)

To add another platform, add one line to the `MEETING_HOST_RULES` array in `config.js`, and add a matching URL pattern to `host_permissions` in `manifest.json` — Relay only has permission to inject the recording banner into the hostnames listed there, so the two lists need to stay in sync.

---

## File Format

Recordings are saved as WebM containers with Opus audio at 128 kbps stereo — small files with very good voice quality, playable in Chrome, VLC, and most modern media players.

---

## Files

| File             | Purpose |
| ---------------- | ------- |
| `manifest.json`  | Extension manifest (Manifest V3) |
| `config.js`      | Shared list of supported meeting platforms |
| `background.js`  | Service worker: state machine, tab capture, banner injection |
| `offscreen.html` / `offscreen.js` | Hidden document that captures, mixes, and encodes audio |
| `popup.html` / `popup.css` / `popup.js` | The toolbar popup UI |
| `icons/`         | Toolbar and store icons |

---

## Troubleshooting

- **"Not a meeting tab"** — Relay only activates on the platforms listed above. Make sure the meeting tab is the active tab when you open the popup.
- **Microphone permission denied** — Click the lock/info icon in Chrome's address bar for the meeting tab, allow microphone access, and try again.
- **No audio from the meeting in the recording** — Make sure the meeting tab was actually playing audio when you clicked Start Recording, and that you did not deny the tab-capture prompt.

---

## Ethical Note

Relay shows a prominent red banner on the meeting page whenever recording is active. This is intentional and non-negotiable. Recording conversations without clear notice is illegal in many jurisdictions and is fundamentally disrespectful to other participants. Do not modify the code to hide this indicator.