# Relay

A minimal Chrome extension that records full meeting audio — both the
remote participants' voices from the meeting tab and your own
microphone — mixed into a single file you can save to your computer.

## A note on how this works

Relay shows a visible red banner ("● Relay is recording this meeting")
at the top of the meeting tab itself whenever it's recording, in
addition to Chrome's own built-in tab-capture indicator. This is
intentional: recording other people's conversations without making
that obvious is illegal in many places and a bad way to treat people
on a call. Relay is built to make recording *easy*, not to make it
*invisible*. Please don't modify the code to suppress or hide these
indicators, and let everyone on the call know you're recording before
you start.

## Installation

1. Download/clone this folder to your computer.
2. Open Chrome and go to `chrome://extensions`.
3. Turn on **Developer mode** (top-right toggle).
4. Click **Load unpacked** and select the `relay` folder (the one
   containing `manifest.json`).
5. The Relay icon will appear in your toolbar. Pin it for easy access.

## Usage

1. Join a meeting in a supported tab (Google Meet, Zoom, Microsoft
   Teams, Webex, GoToMeeting, BlueJeans, Whereby, Discord, or Slack
   huddles).
2. Click the Relay icon. If the active tab is recognized as a meeting,
   you'll see a single **Start Recording** button.
3. Click it. The first time, Chrome will ask for microphone
   permission — allow it. Recording begins immediately, and a red
   banner appears on the meeting page so everyone sharing that screen
   can see it's happening.
4. The popup shows a running timer. You can close the popup — Relay
   keeps recording in the background. Reopening the popup shows the
   correct elapsed time and a **Stop Recording** button.
5. Click **Stop Recording** (from the popup) when you're done. A
   "Save As" dialog opens immediately with a suggested filename like
   `Relay_Meeting_2026-06-19_14-32-05.webm`. Choose where to save it.
6. If the meeting tab is closed while recording, Relay automatically
   stops, saves whatever was captured up to that point, and shows a
   notification.

## Supported meeting platforms

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

To add another platform, add one line to the `MEETING_HOST_RULES`
array in `config.js`, **and** add a matching URL pattern to
`host_permissions` in `manifest.json` — Relay only has permission to
inject the recording banner into the hostnames listed there, so the
two lists need to stay in sync.

## File format

Recordings are saved as WebM containers with Opus audio at 128 kbps
stereo — small files with very good voice quality, playable in
Chrome, VLC, and most modern media players.

## Files

| File             | Purpose                                                        |
| ---------------- | ---------------------------------------------------------------|
| `manifest.json`  | Extension manifest (Manifest V3)                               |
| `config.js`      | Shared list of supported meeting platforms                     |
| `background.js`  | Service worker: state machine, tab capture, banner injection   |
| `offscreen.html` / `offscreen.js` | Hidden document that captures, mixes, and encodes audio |
| `popup.html` / `popup.css` / `popup.js` | The toolbar popup UI                            |
| `icons/`         | Toolbar and store icons                                        |

## Troubleshooting

- **"Not a meeting tab"** — Relay only activates on the platforms
  listed above. Make sure the meeting tab is the active tab when you
  open the popup.
- **Microphone permission denied** — Click the lock/info icon in
  Chrome's address bar for the meeting tab, allow microphone access,
  and try again.
- **No audio from the meeting in the recording** — Make sure the
  meeting tab was actually playing audio when you clicked Start
  Recording, and that you didn't deny the tab-capture prompt.
