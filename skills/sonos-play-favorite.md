---
name: Play a favorite or playlist on Sonos
description: Find a household's Sonos favorites or playlists and start one playing on a group.
api: https://docs.sonos.com/reference
base_url: https://api.ws.sonos.com/control/api/v1
auth: OAuth 2.0 (Bearer) — scope playback-control-all
operations: [getHouseholds, getGroups, getFavorites, loadFavorite, getPlaylists, loadPlaylist]
---

# Play a favorite or playlist on Sonos

## Auth
OAuth 2.0 access token (scope `playback-control-all`) sent as
`Authorization: Bearer <token>`.

## Steps
1. **getHouseholds** — `GET /households`; pick `householdId`.
2. **getGroups** — `GET /households/{householdId}/groups`; pick `groupId`.
3. Favorites path:
   - **getFavorites** — `GET /households/{householdId}/favorites`; pick a
     favorite `id`.
   - **loadFavorite** — `POST /groups/{groupId}/favorites` with the favorite id
     and `playOnCompletion: true` to start it playing.
4. Playlists path:
   - **getPlaylists** — `GET /households/{householdId}/playlists`; pick a
     playlist `id`.
   - **loadPlaylist** — `POST /groups/{groupId}/playlists` with the playlist id
     and `playOnCompletion: true`.

## Rules
- Favorites and playlists are household-scoped; playback is group-scoped.
- Handle `ERROR_INVALID_OBJECT_ID` (stale favorite/playlist id) by re-fetching.
- See `conventions/sonos-conventions.yml` for the resource/eventing model.
