---
name: Control playback on a Sonos group
description: Authenticate, find a household and its groups, then start, pause, or skip playback on a group.
api: https://docs.sonos.com/reference
base_url: https://api.ws.sonos.com/control/api/v1
auth: OAuth 2.0 (Bearer) — scope playback-control-all
operations: [getHouseholds, getGroups, getPlaybackStatus, play, pause, togglePlayPause, skipToNextTrack, setPlayModes]
---

# Control playback on a Sonos group

Use the Sonos Control API to drive playback on a group of speakers.

## Auth
Obtain an OAuth 2.0 access token via the authorization code grant
(authorize: `https://api.sonos.com/login/v3/oauth`, token:
`https://api.sonos.com/login/v3/oauth/access`, scope `playback-control-all`).
Send it on every request as `Authorization: Bearer <token>`.

## Steps
1. **getHouseholds** — `GET /households`. Pick the target `householdId`.
2. **getGroups** — `GET /households/{householdId}/groups`. Choose the `groupId`
   you want to control (the group is the unit playback targets, not the player).
3. **getPlaybackStatus** — `GET /groups/{groupId}/playback` to read the current
   state (PLAYBACK_STATE_PLAYING / PAUSED / IDLE / BUFFERING).
4. Drive playback:
   - **play** — `POST /groups/{groupId}/playback/play`
   - **pause** — `POST /groups/{groupId}/playback/pause`
   - **togglePlayPause** — `POST /groups/{groupId}/playback/togglePlayPause`
   - **skipToNextTrack** — `POST /groups/{groupId}/playback/skipToNextTrack`
   - **setPlayModes** — `POST /groups/{groupId}/playback/playModes` to set
     shuffle / repeat / crossfade.

## Rules
- Address commands by `groupId`, never by a single player, for shared playback.
- Errors return `{ "errorCode": ..., "reason": ... }` — handle `ERROR_NOT_CAPABLE`
  (wrong state) and `ERROR_RESOURCE_GONE` (group changed) by re-fetching groups.
  See `errors/sonos-problem-types.yml`.
- For live updates instead of polling, subscribe to the `playback` namespace
  (see `asyncapi/sonos-events-webhooks.yml`).
