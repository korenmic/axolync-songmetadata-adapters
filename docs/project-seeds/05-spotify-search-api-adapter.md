# Spotify Search API Adapter

## Summary

Spotify search is a legitimate SongMetadata fallback because the official endpoint supports structured filters including artist, track, and isrc.

It is still clearly weaker than direct Spotify track-id lookup because search ambiguity now becomes part of the adapter's correctness burden.

So this row should remain visible as a real candidate, but it should sit below direct-id enrichment in both product trust and implementation order.

## Priority

`P2`

## Research Status

`researched`

## Candidate Class

- primary class: `authenticated search metadata adapter`
- secondary traits:
  - `spotify-backed`
  - `search-based`
  - `duration-capable`
  - `requires auth`

## Upstream Snapshot

Primary sources checked:
- Spotify Search docs: `https://developer.spotify.com/documentation/web-api/reference/search`
- GitHub: Spotify Web API TS SDK: `https://github.com/spotify/spotify-web-api-ts-sdk`
- GitHub: spotify-web-api-node: `https://github.com/thelinmichael/spotify-web-api-node`
- Provider evidence note: `C:/Users/koren/src/Sinq2/axolync-songmetadata-plugin/providers/spotify-search-api/README.md`

Observed signals during research:
- Spotify search supports field filters including artist, track, and isrc.
- Healthy JS SDK families exist, so implementation mechanics are not the blocker.
- The real product risk is search ambiguity rather than raw API capability.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `YELLOW` | Feasible, but auth plus search-result ambiguity keep this from being a clean browser-first recommendation. |
| 2. WASM port/packaging | `BLACK` | There is no engine to port. |
| 3. Webview wrapper JS -> native/plugin bridge | `RED` | A native bridge is unnecessary for a search-only metadata API unless it already owns credentials. |
| 4. Browser extension | `DARK RED` | Possible, but not a natural fit for a search-auth problem. |
| 5. Userscript | `BLACK` | Too brittle for an authenticated provider search lane. |
| 6. Desktop companion | `YELLOW` | A companion can own credentials, but a shared host is still cleaner. |
| 7. Shared Python adapter host | `YELLOW` | Technically fine, but Node/TypeScript still has the cleaner evidence here. |
| 8. Shared Node adapter host | `DARK GREEN` | Best immediate fit: structured query construction, result filtering, and auth ownership all sit comfortably here. |
| 9. CLI wrapper around upstream/runtime | `DARK RED` | Adds indirection to an already ambiguous search path. |
| 10. FFI/native library host lane | `BLACK` | No native-library advantage exists here. |
| 11. Forced conversion to local JS | `BLACK` | Not a conversion problem. |
| 12. Remote shared service | `DARK GREEN` | A shared broker is a good place to centralize search-policy heuristics and caching. |
| 13. Cloud microservice per request / per tenant | `YELLOW` | Viable, but heavier than the current product need. |
| 14. Platform SDK direct integration | `BLACK` | This is a web API search path, not a platform SDK story. |

## Maintainability Read

Machine-readable rating: `medium`

- The API and JS ecosystem are strong.
- Search-policy correctness and ambiguity handling are the real maintenance cost.

## Recommendation

1. `8. Shared Node adapter host`
2. `12. Remote shared service`
3. `1. Local JS in ordinary browser/webview plugin`

### Recommendation Notes

- This is a legitimate fallback only after direct Spotify id lookup is unavailable.
- Keep it bounded by provider affinity and strict winner selection.

## Open Questions

1. How much search ambiguity is acceptable before SongMetadata should refuse to enrich instead of guessing?
2. Should Spotify search be allowed to use ISRC filters first when that field is present, before trying artist/title text?
