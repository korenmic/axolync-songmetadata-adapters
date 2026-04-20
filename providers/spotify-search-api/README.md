# Spotify Search API

## Why this is a SongMetadata candidate

Spotify search can query tracks by free text and by field filters including `artist`, `track`, and `isrc`, so it is a plausible fallback when a Spotify-affine match lacks a direct track id.

## Primary sources checked

- Spotify Search docs: `https://developer.spotify.com/documentation/web-api/reference/search`
- GitHub: `https://github.com/spotify/spotify-web-api-ts-sdk`
- GitHub: `https://github.com/thelinmichael/spotify-web-api-node`

## Key signals

- Official Spotify API
- Search supports structured filters, including ISRC
- Still auth-gated
- Search ambiguity makes this weaker than direct track-id lookup

