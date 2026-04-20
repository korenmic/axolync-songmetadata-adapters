# Apple Music Catalog by ISRC

## Why this is a SongMetadata candidate

Apple Music can fetch songs by `filter[isrc]`, which is valuable when SongSense preserves recording identity but not a direct Apple catalog song id.

## Primary sources checked

- Apple Music API by ISRC docs: `https://developer.apple.com/documentation/applemusicapi/get-multiple-catalog-songs-by-isrc`
- GitHub: `https://github.com/rryam/MusadoraKit`

## Key signals

- Official Apple catalog API
- Direct ISRC query path exists
- Response includes `durationInMillis`
- Multiple songs can be returned for one ISRC, so arbitration still matters

