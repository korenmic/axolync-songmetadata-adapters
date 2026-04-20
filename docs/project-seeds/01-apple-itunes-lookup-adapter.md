# Apple iTunes Lookup Adapter

## Summary

Apple's public iTunes Search / Lookup APIs make this one of the lightest real SongMetadata candidates because they can return track duration without Apple Music developer-token setup.

The maintained TypeScript client ecosystem is good enough to prove this is not a theoretical path, and ordinary browser-local JS is plausible here in a way that many credentialed metadata providers are not.

The main limitation is trust tier, not feasibility: this public lookup path is weaker than authenticated Apple Music catalog-id or ISRC fetches, so it should stay a lower-precedence Apple-affine enrichment lane.

## Priority

`P0`

## Research Status

`researched`

## Candidate Class

- primary class: `public catalog metadata adapter`
- secondary traits:
  - `apple-backed`
  - `no-auth public API`
  - `duration-capable`
  - `good early implementation candidate`

## Upstream Snapshot

Primary sources checked:
- Apple iTunes Search API: `https://performance-partners.apple.com/search-api`
- GitHub: itunes-store-api: `https://github.com/marcbouchenoire/itunes-store-api`
- Provider evidence note: `C:/Users/koren/src/Sinq2/axolync-songmetadata-plugin/providers/apple-itunes-lookup/README.md`

Observed signals during research:
- Apple still documents the public iTunes Search / Lookup surface, including track-oriented media/entity parameters.
- marcbouchenoire/itunes-store-api is a small typed MIT client, which lowers implementation friction for a JavaScript-hosted adapter.
- This is a practical first-pass Apple-affine duration enricher, but it should not outrank authenticated Apple Music catalog paths when stronger identifiers exist.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `BRIGHT GREEN` | Public HTTP + lightweight TypeScript client make this one of the cleanest browser-first SongMetadata candidates. |
| 2. WASM port/packaging | `BLACK` | There is no engine to port; this is plain HTTP metadata retrieval. |
| 3. Webview wrapper JS -> native/plugin bridge | `RED` | A native bridge adds weight without solving a real problem for this public API. |
| 4. Browser extension | `YELLOW` | Feasible, but weaker than ordinary browser-local JS for a public metadata call. |
| 5. Userscript | `DARK RED` | Possible for experiments, but too brittle for a product-grade metadata lane. |
| 6. Desktop companion | `RED` | A companion app is unnecessary for a public store lookup. |
| 7. Shared Python adapter host | `YELLOW` | Works, but gives up the strongest browser-local simplicity. |
| 8. Shared Node adapter host | `DARK GREEN` | Very workable if Axolync prefers central HTTP policy and caching outside the browser. |
| 9. CLI wrapper around upstream/runtime | `DARK RED` | Shelling a public lookup API through a CLI adds indirection without value. |
| 10. FFI/native library host lane | `BLACK` | No native library advantage exists here. |
| 11. Forced conversion to local JS | `BLACK` | Nothing needs conversion; direct JS is already available. |
| 12. Remote shared service | `DARK GREEN` | A central metadata broker could own caching and storefront policy if browser-local calls are later restricted. |
| 13. Cloud microservice per request / per tenant | `YELLOW` | Possible, but usually heavier than the problem requires. |
| 14. Platform SDK direct integration | `BLACK` | This candidate is about a public web API, not an Apple platform SDK. |

## Maintainability Read

Machine-readable rating: `high`

- The API shape is straightforward and the JS implementation path is light.
- Most risk is in Apple result precision, storefront policy, and identifier availability rather than in adapter complexity.

## Recommendation

1. `1. Local JS in ordinary browser/webview plugin`
2. `8. Shared Node adapter host`
3. `12. Remote shared service`

### Recommendation Notes

- This is the best first Apple-backed SongMetadata adapter candidate if Axolync wants the smallest possible implementation with real duration value.
- Treat it as Apple-affine and lower-trust than authenticated Apple Music catalog-id or ISRC lookups.

## Open Questions

1. Will accepted SongSense matches preserve enough Apple-affinity identifiers or URLs to let iTunes lookup stay precise instead of broad text search?
2. Should storefront selection be explicit in SongMetadata settings or implicit from the accepted match context?
