# MusicBrainz ISRC Lookup

## Why this is a SongMetadata candidate

MusicBrainz exposes recording lookup/search paths keyed by ISRC, and recording metadata includes duration, making it a legitimate provider-neutral fallback when provider-specific enrichers did not satisfy canonical duration.

## Primary sources checked

- MusicBrainz API docs: `https://musicbrainz.org/doc/MusicBrainz_API`
- MusicBrainz Search docs: `https://musicbrainz.org/doc/MusicBrainz_API/Search`
- GitHub: `https://github.com/Borewit/musicbrainz-api`

## Key signals

- Open metadata API
- Recording search supports ISRC
- Recording metadata includes duration/length
- User-Agent / etiquette compliance matters

