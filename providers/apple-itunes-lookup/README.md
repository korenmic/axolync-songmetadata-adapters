# Apple iTunes Lookup

## Why this is a SongMetadata candidate

The public iTunes Search / Lookup APIs can return music-track metadata, including track length, without Apple Music developer-token setup.

That makes this the lightest Apple-backed duration enrichment candidate when the accepted match is already Apple-affine and carries enough identifiers or title/artist truth.

## Primary sources checked

- Apple iTunes Search API: `https://performance-partners.apple.com/search-api`
- GitHub: `https://github.com/marcbouchenoire/itunes-store-api`

## Key signals

- Public HTTP surface
- No Apple Music developer token required
- Community TypeScript client exists and is MIT licensed
- Lookup by item id / URL is cleaner than plain keyword search

