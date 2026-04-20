# MusicBrainz Recording Search Adapter

## Summary

MusicBrainz recording search is the broadest provider-neutral SongMetadata fallback because it can search by artist/title and still return recording length.

That also makes it the most ambiguity-prone candidate in this repo, so its value is resilience rather than first-choice authority.

It deserves a real atlas row because the open API and JS tooling are strong, but it should sit behind provider-affine enrichers and behind MusicBrainz ISRC lookup.

## Priority

`P2`

## Research Status

`researched`

## Candidate Class

- primary class: `open metadata search fallback adapter`
- secondary traits:
  - `provider-neutral`
  - `artist-title search`
  - `no auth required`
  - `lower trust than identifier lookups`

## Upstream Snapshot

Primary sources checked:
- MusicBrainz Search docs: `https://musicbrainz.org/doc/MusicBrainz_API/Search`
- MusicBrainz Recording docs: `https://musicbrainz.org/doc/Recording`
- GitHub: musicbrainz-api: `https://github.com/Borewit/musicbrainz-api`
- Provider evidence note: `C:/Users/koren/src/Sinq2/axolync-songmetadata-plugin/providers/musicbrainz-recording-search/README.md`

Observed signals during research:
- MusicBrainz recording search supports artist/artistname/recording fields.
- Recording length is a first-class property in MusicBrainz.
- This is useful for fallback resilience, but clearly weaker than identifier-driven lookups.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `DARK GREEN` | Open HTTP and no auth make this feasible locally, but the adapter still needs strict search-result discipline. |
| 2. WASM port/packaging | `BLACK` | There is no engine to port. |
| 3. Webview wrapper JS -> native/plugin bridge | `BLACK` | A native bridge adds no obvious value to an open search API. |
| 4. Browser extension | `DARK RED` | Possible, but weaker than plain JS or a shared host. |
| 5. Userscript | `DARK RED` | Too brittle for a reusable search fallback lane. |
| 6. Desktop companion | `BLACK` | Usually unnecessary here. |
| 7. Shared Python adapter host | `YELLOW` | Fine, but not the strongest evidence path. |
| 8. Shared Node adapter host | `DARK GREEN` | Strong place to centralize search heuristics, scoring, and cache policy. |
| 9. CLI wrapper around upstream/runtime | `DARK RED` | Adds indirection to an already search-policy-heavy candidate. |
| 10. FFI/native library host lane | `BLACK` | No native-library advantage exists here. |
| 11. Forced conversion to local JS | `BLACK` | Already available through JS tooling and direct HTTP. |
| 12. Remote shared service | `YELLOW` | Possible when Axolync wants centralized search heuristics, but still likely heavier than needed. |
| 13. Cloud microservice per request / per tenant | `RED` | Too heavy for a broad fallback search path. |
| 14. Platform SDK direct integration | `BLACK` | No platform SDK story exists here. |

## Maintainability Read

Machine-readable rating: `medium`

- The API is open and the tooling is good.
- The real burden is fallback correctness and overmatching avoidance, not runtime implementation complexity.

## Recommendation

1. `8. Shared Node adapter host`
2. `1. Local JS in ordinary browser/webview plugin`
3. `12. Remote shared service`

### Recommendation Notes

- Keep this row visible because it is a real resilience option, but do not confuse it with a first-choice canonical-duration source.
- It should run after provider-affine enrichers and after MusicBrainz ISRC lookup.

## Open Questions

1. How strict should artist/title fallback matching be before SongMetadata refuses to return duration?
2. Should this path be disabled entirely in strict profiles that only allow provider-affine or identifier-based metadata enrichers?
