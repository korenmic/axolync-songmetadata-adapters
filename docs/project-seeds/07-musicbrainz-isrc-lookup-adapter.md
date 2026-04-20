# MusicBrainz ISRC Lookup Adapter

## Summary

MusicBrainz ISRC-capable recording lookup is one of the strongest provider-neutral SongMetadata fallbacks because it can return recording duration without provider-specific credentials.

The open API plus maintained JavaScript client ecosystem make this a practical implementation lane, not just a theoretical fallback.

Its real role should be default fallback, not first-choice enrichment, because provider-matched adapters should still get first shot at canonical duration truth.

## Priority

`P1`

## Research Status

`researched`

## Candidate Class

- primary class: `open metadata fallback adapter`
- secondary traits:
  - `provider-neutral`
  - `ISRC-capable`
  - `no auth required`
  - `useful default fallback`

## Upstream Snapshot

Primary sources checked:
- MusicBrainz API docs: `https://musicbrainz.org/doc/MusicBrainz_API`
- MusicBrainz Search docs: `https://musicbrainz.org/doc/MusicBrainz_API/Search`
- GitHub: musicbrainz-api: `https://github.com/Borewit/musicbrainz-api`
- Provider evidence note: `C:/Users/koren/src/Sinq2/axolync-songmetadata-plugin/providers/musicbrainz-isrc-lookup/README.md`

Observed signals during research:
- MusicBrainz supports recording search by ISRC.
- Recording metadata includes length/duration information.
- musicbrainz-api is a maintained JavaScript client with throttling and browser-noted compatibility.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `DARK GREEN` | Open HTTP + no auth make this a plausible browser-local fallback when Axolync obeys MusicBrainz etiquette and rate limits. |
| 2. WASM port/packaging | `BLACK` | There is no engine to port. |
| 3. Webview wrapper JS -> native/plugin bridge | `BLACK` | A native bridge adds no obvious value to an open metadata API. |
| 4. Browser extension | `DARK RED` | Possible, but weaker than ordinary JS or a shared host for this API. |
| 5. Userscript | `DARK RED` | Too brittle for a reusable metadata fallback. |
| 6. Desktop companion | `BLACK` | Usually unnecessary for an open lookup API. |
| 7. Shared Python adapter host | `YELLOW` | Works, but the JavaScript/browser story is already strong enough. |
| 8. Shared Node adapter host | `DARK GREEN` | Strong fit when Axolync wants centralized User-Agent policy, throttling, and cache ownership. |
| 9. CLI wrapper around upstream/runtime | `DARK RED` | Adds indirection without improving the core lookup quality. |
| 10. FFI/native library host lane | `BLACK` | No native-library benefit exists here. |
| 11. Forced conversion to local JS | `BLACK` | Already available through JS tooling and direct HTTP. |
| 12. Remote shared service | `YELLOW` | Possible, but overkill if the only need is an open fallback lookup. |
| 13. Cloud microservice per request / per tenant | `RED` | Operationally heavier than this open fallback needs. |
| 14. Platform SDK direct integration | `BLACK` | No platform SDK story exists here. |

## Maintainability Read

Machine-readable rating: `high`

- The API is open and the JS tooling is solid.
- The main discipline is etiquette, throttling, and refusing over-broad fallback guesses.

## Recommendation

1. `1. Local JS in ordinary browser/webview plugin`
2. `8. Shared Node adapter host`
3. `12. Remote shared service`

### Recommendation Notes

- This is one of the best provider-neutral SongMetadata fallbacks because it can work without credentials and still return real duration metadata.
- It should only run after provider-matched adapters fail to satisfy canonicalDurationMs.

## Open Questions

1. Should MusicBrainz fallback be disabled by default until provider-affine enrichers fail, or always allowed in best-effort profiles?
2. Do we want browser-local MusicBrainz calls, or should a shared host own User-Agent compliance and throttling from the start?
