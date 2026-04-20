# Spotify URL Info Adapter

## Summary

spotify-url-info is a real SongMetadata candidate because it can extract Spotify-like metadata from URLs without first solving Spotify auth.

That makes it valuable as an atlas row: it represents the practical low-friction fallback class, not the official high-trust class.

The tradeoff is contractual and operational trust, so it should remain below Spotify's official Web API lanes in both priority and recommendation strength.

## Priority

`P2`

## Research Status

`researched`

## Candidate Class

- primary class: `scrape-assisted metadata adapter`
- secondary traits:
  - `spotify-backed`
  - `url-driven`
  - `no-auth`
  - `lower contractual trust`

## Upstream Snapshot

Primary sources checked:
- GitHub: spotify-url-info: `https://github.com/microlinkhq/spotify-url-info`
- Spotify Get Track docs for shape comparison: `https://developer.spotify.com/documentation/web-api/reference/get-track`
- Provider evidence note: `C:/Users/koren/src/Sinq2/axolync-songmetadata-plugin/providers/spotify-url-info/README.md`

Observed signals during research:
- spotify-url-info is an MIT JavaScript package that works from a caller-provided fetch agent.
- The README says its output resembles Spotify API data, which is useful for a SongMetadata adapter that only needs canonical duration.
- This remains a scrape-style lane rather than the official Spotify platform contract.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `DARK GREEN` | This is one of the few Spotify candidates that is naturally browser-friendly because it is already JS + fetch based and avoids auth. |
| 2. WASM port/packaging | `BLACK` | No engine to port. |
| 3. Webview wrapper JS -> native/plugin bridge | `BLACK` | A native bridge adds no obvious value to a JS fetch-based metadata helper. |
| 4. Browser extension | `YELLOW` | A browser extension can host scrape-style fallbacks cleanly, but it is still weaker than ordinary local JS here. |
| 5. Userscript | `YELLOW` | Technically plausible because the candidate is URL/web driven, but still too brittle for a first-class product lane. |
| 6. Desktop companion | `RED` | A companion app is usually unnecessary for a URL-driven JS metadata helper. |
| 7. Shared Python adapter host | `BLACK` | Wrong ecosystem fit. |
| 8. Shared Node adapter host | `YELLOW` | Works if Axolync wants centralized caching, but the main value of this candidate is easy local JS usage. |
| 9. CLI wrapper around upstream/runtime | `DARK RED` | A CLI shell adds indirection without improving trust. |
| 10. FFI/native library host lane | `BLACK` | No native-library angle exists here. |
| 11. Forced conversion to local JS | `BLACK` | Already JavaScript. |
| 12. Remote shared service | `YELLOW` | Possible for centralized scrape policy, but then Axolync inherits service ownership for a weaker trust lane. |
| 13. Cloud microservice per request / per tenant | `RED` | Operationally heavy for a scrape-style fallback. |
| 14. Platform SDK direct integration | `BLACK` | No platform SDK story exists here. |

## Maintainability Read

Machine-readable rating: `low`

- Runtime integration is easy because the helper is already JS + fetch based.
- The real risk is that this is not the official Spotify contract and may prove brittle over time.

## Recommendation

1. `1. Local JS in ordinary browser/webview plugin`
2. `4. Browser extension`
3. `8. Shared Node adapter host`

### Recommendation Notes

- This is a useful atlas row because it shows a real no-auth Spotify option, but it should remain below the official Spotify API lanes.
- Use it only when Axolync explicitly accepts the lower trust and brittleness of scrape-style metadata helpers.

## Open Questions

1. Is a scrape-style Spotify fallback acceptable at all under Axolync's product and policy principles?
2. Would this candidate be allowed only in debug / experimental profiles, or is there a real release-grade case for it?
