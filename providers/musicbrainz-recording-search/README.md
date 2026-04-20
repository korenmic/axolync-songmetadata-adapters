# MusicBrainz Recording Search

## Why this is a SongMetadata candidate

MusicBrainz recording search is the broadest default fallback path for SongMetadata when the system only has artist/title truth and provider-matched enrichers left `canonicalDurationMs` unresolved.

## Primary sources checked

- MusicBrainz Search docs: `https://musicbrainz.org/doc/MusicBrainz_API/Search`
- MusicBrainz Recording docs: `https://musicbrainz.org/doc/Recording`
- GitHub: `https://github.com/Borewit/musicbrainz-api`

## Key signals

- Open metadata API
- Search fields include artist, artistname, recording, and dur
- Recording length is a real first-class property
- Search ambiguity keeps this below provider-affine id-based enrichers

