# Spotify Track API

## Why this is a SongMetadata candidate

The official Spotify track object exposes `duration_ms` and `external_ids.isrc`, so a provider-affine SongMetadata adapter can retrieve strong duration truth when the accepted match already includes a Spotify track id.

## Primary sources checked

- Spotify Get Track docs: `https://developer.spotify.com/documentation/web-api/reference/get-track`
- GitHub: `https://github.com/spotify/spotify-web-api-ts-sdk`
- GitHub: `https://github.com/thelinmichael/spotify-web-api-node`

## Key signals

- Official Spotify API
- Direct track-id lookup
- Duration is explicit
- Authentication is mandatory

