# Apple Music Catalog by ID

## Why this is a SongMetadata candidate

Apple Music catalog song endpoints expose `durationInMillis` directly, so a provider-affine SongMetadata adapter can use a preserved Apple song id to return strong canonical duration truth.

## Primary sources checked

- Apple Music API Songs docs: `https://developer.apple.com/documentation/applemusicapi/songs-api`
- GitHub: `https://github.com/rryam/MusadoraKit`
- GitHub: `https://github.com/KoleMyers/apple-musickit-example`

## Key signals

- Official Apple catalog API
- Direct song-id fetch path
- Duration metadata is first-class
- Requires Apple developer-token / MusicKit setup

