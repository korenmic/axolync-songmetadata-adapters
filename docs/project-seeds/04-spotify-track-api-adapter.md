# Spotify Track API Adapter

## Summary

Spotify's official track endpoint is the strongest Spotify-backed SongMetadata duration candidate because the track object exposes duration_ms directly.

The same track object also exposes external_ids such as ISRC, which makes this lane useful both for direct duration retrieval and for future cross-provider fallback logic.

The real complexity is not data quality but auth ownership, which makes shared-host or brokered implementations cleaner than treating this as a casual browser-local call.

## Priority

`P1`

## Research Status

`researched`

## Candidate Class

- primary class: `authenticated catalog metadata adapter`
- secondary traits:
  - `spotify-backed`
  - `track-id first`
  - `duration-capable`
  - `requires auth`

## Upstream Snapshot

Primary sources checked:
- Spotify Get Track docs: `https://developer.spotify.com/documentation/web-api/reference/get-track`
- GitHub: Spotify Web API TS SDK: `https://github.com/spotify/spotify-web-api-ts-sdk`
- GitHub: spotify-web-api-node: `https://github.com/thelinmichael/spotify-web-api-node`
- Provider evidence note: `C:/Users/koren/src/Sinq2/axolync-songmetadata-plugin/providers/spotify-track-api/README.md`

Observed signals during research:
- Spotify's track object documents duration_ms directly.
- The track object also exposes external_ids.isrc, which increases downstream enrichment value.
- Both official and community JavaScript SDK families exist, so host-runtime implementation evidence is strong.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `YELLOW` | Technically possible with PKCE or user auth, but auth ownership is the real challenge. |
| 2. WASM port/packaging | `BLACK` | There is no engine to port. |
| 3. Webview wrapper JS -> native/plugin bridge | `RED` | A native bridge is not the natural fit for a hosted metadata API unless it already owns credentials. |
| 4. Browser extension | `RED` | Possible, but awkward compared with host-owned tokens. |
| 5. Userscript | `BLACK` | Too brittle for an authenticated provider integration. |
| 6. Desktop companion | `YELLOW` | Can own credentials, but adds another moving part when a shared host could do the same. |
| 7. Shared Python adapter host | `YELLOW` | Works, but Node/TypeScript has the cleaner ecosystem evidence here. |
| 8. Shared Node adapter host | `BRIGHT GREEN` | Strongest fit: official TypeScript SDK, direct track-id lookup, clean token ownership, easy caching. |
| 9. CLI wrapper around upstream/runtime | `DARK RED` | A CLI shell around the Spotify API is weaker than a proper host runtime. |
| 10. FFI/native library host lane | `BLACK` | No native-library advantage exists here. |
| 11. Forced conversion to local JS | `BLACK` | There is nothing to convert. |
| 12. Remote shared service | `DARK GREEN` | A shared metadata broker can own Spotify auth and centralize caching. |
| 13. Cloud microservice per request / per tenant | `YELLOW` | Viable, but more operational cost than the stage-1 problem needs. |
| 14. Platform SDK direct integration | `BLACK` | This candidate is about the Spotify Web API, not a platform SDK. |

## Maintainability Read

Machine-readable rating: `medium`

- Official API quality and strong JS tooling help.
- The main burden is auth policy, not metadata semantics.

## Recommendation

1. `8. Shared Node adapter host`
2. `12. Remote shared service`
3. `1. Local JS in ordinary browser/webview plugin`

### Recommendation Notes

- This is the strongest Spotify-backed SongMetadata candidate when a direct Spotify track id is preserved.
- It should outrank Spotify search and scraper-style fallbacks.

## Open Questions

1. Will accepted SongSense matches preserve Spotify track ids directly enough to make this a realistic first Spotify implementation?
2. Does Axolync want Spotify auth to live in a shared host first, or is browser-side PKCE acceptable for SongMetadata in some profiles?
