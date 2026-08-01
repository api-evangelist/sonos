---
name: Announce an audio clip on a Sonos player
description: Play a short audio clip (notification/announcement) on a Sonos player without disrupting the current playback session.
api: https://docs.sonos.com/reference
base_url: https://api.ws.sonos.com/control/api/v1
auth: OAuth 2.0 (Bearer) — scope playback-control-all
operations: [getHouseholds, getGroups, loadAudioClip]
---

# Announce an audio clip on a Sonos player

The audioClip namespace plays a short sound (doorbell, TTS, notification) on a
player and automatically ducks/restores any current playback.

## Auth
OAuth 2.0 access token (scope `playback-control-all`) as
`Authorization: Bearer <token>`.

## Steps
1. **getHouseholds** — `GET /households`; pick `householdId`.
2. **getGroups** — `GET /households/{householdId}/groups`; the response lists
   players — pick a target `playerId`.
3. **loadAudioClip** — `POST /players/{playerId}/audioClip` with a body such as
   `{ "name": "Doorbell", "appId": "com.example.app", "streamUrl": "https://.../clip.mp3", "clipType": "CUSTOM", "priority": "HIGH" }`.
   Omit `streamUrl` and set `clipType` to a built-in (e.g. `CHIME`) to use a
   default sound.

## Rules
- Audio clips target a **playerId**, not a groupId.
- `appId` must be a valid reverse-DNS identifier for your integration.
- Requires a player that supports audio clips; otherwise `ERROR_NOT_CAPABLE`.
- Subscribe to the `audioClip` namespace for clip status events.
