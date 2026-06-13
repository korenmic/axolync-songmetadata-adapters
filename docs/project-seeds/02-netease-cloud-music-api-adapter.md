# NetEase Cloud Music API Adapter

## Summary

NetEase Cloud Music API wrappers are a strong SongMetadata candidate for Asian catalog enrichment because they expose search, song detail, album, artist, playlist, and NetEase provider IDs.

The best current upstream is NeteaseCloudMusicApiEnhanced, a maintained MIT revival of the original large Node API wrapper after the original Binaryify GitHub repository was archived.

This row should be used as a provider-specific metadata authority candidate, especially when NetEase SongSense returns only IDs or partial metadata that need enrichment.

## Priority

`P1`

## Research Status

`researched`

## Candidate Class

- primary class: `unofficial hosted metadata API wrapper`
- secondary traits:
  - `netEase-provider`
  - `metadata-enrichment`
  - `search-capable`
  - `network-bound`
  - `unofficial-api`

## Upstream Snapshot

Primary sources checked:
- NeteaseCloudMusicApiEnhanced api-enhanced: `https://github.com/NeteaseCloudMusicApiEnhanced/api-enhanced`
- NeteaseCloudMusicApiEnhanced docs: `https://docs-neteasecloudmusicapi.focalors.ltd/`
- Binaryify NeteaseCloudMusicApi archive: `https://github.com/Binaryify/NeteaseCloudMusicApi`
- simple-netease-cloud-music: `https://github.com/surmon-china/simple-netease-cloud-music`
- chaunsin netease-cloud-music: `https://github.com/chaunsin/netease-cloud-music`
- netease-qq-music-api crate: `https://crates.io/crates/netease-qq-music-api`

Observed signals during research:
- NeteaseCloudMusicApiEnhanced is active, MIT-licensed, and describes itself as a broad NetEase Cloud Music Node.js API service.
- The original Binaryify GitHub repository is archived, but points readers to the package and external continuation context.
- simple-netease-cloud-music documents search, playlist, picture, artist, album, lyric, URL, and song-detail calls with a lightweight Node API.
- The Go and Rust wrapper evidence shows that NetEase metadata access is not limited to one JavaScript package, but Node remains the best initial Axolync fit.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `YELLOW` | JS wrappers exist, but direct browser use may hit CORS and provider request-shape constraints. |
| 2. WASM port/packaging | `BLACK` | This is metadata HTTP/API work, not an engine to port. |
| 3. Webview wrapper JS -> native/plugin bridge | `YELLOW` | A native bridge can proxy calls, but a shared host is usually simpler for metadata. |
| 4. Browser extension | `YELLOW` | An extension can work around browser constraints, but metadata lookup does not inherently need extension-level privileges. |
| 5. Userscript | `RED` | Userscript access would be fragile and not a good long-term metadata authority. |
| 6. Desktop companion | `YELLOW` | A desktop helper could own a local Node or Go wrapper, but deployment overhead is higher than a shared host. |
| 7. Shared Python adapter host | `YELLOW` | Python could wrap the HTTP API, but the strongest maintained upstream is Node. |
| 8. Shared Node adapter host | `BRIGHT GREEN` | Best first lane because the maintained API Enhanced wrapper is Node-oriented and broad. |
| 9. CLI wrapper around upstream/runtime | `YELLOW` | CLI wrapping is possible around Go/Node tools, but less clean for live metadata enrichment. |
| 10. FFI/native library host lane | `YELLOW` | A Rust or Go host is possible, but there is no clear advantage over Node for the first adapter. |
| 11. Forced conversion to local JS | `YELLOW` | Endpoint calls are JS-adjacent, but a local rewrite would inherit service drift and browser constraints. |
| 12. Remote shared service | `DARK GREEN` | A shared service can cache search/detail responses and isolate NetEase endpoint drift. |
| 13. Cloud microservice per request / per tenant | `DARK GREEN` | A per-request service is plausible for metadata normalization, especially around search and detail lookups. |
| 14. Platform SDK direct integration | `BLACK` | No official platform SDK route was found for NetEase metadata. |

## Maintainability Read

Machine-readable rating: `medium`

- API Enhanced is broad and maintained, but it is still a reverse-engineered wrapper rather than an official partner contract.
- SongMetadata normalization should keep NetEase IDs, aliases, translated names, album art, duration, and provider provenance explicit.
- A host-owned integration avoids browser CORS and lets Axolync centralize search normalization, caching, and rate-limit handling.

## Recommendation

1. `8. Shared Node adapter host`
2. `12. Remote shared service`
3. `13. Cloud microservice per request / per tenant`

### Recommendation Notes

- Use NeteaseCloudMusicApiEnhanced as the primary proof source.
- Keep the SongMetadata row focused on provider ID enrichment and normalized metadata, not playback or download behavior.

## Open Questions

1. Should NetEase SongMetadata be an observer-style enrichment adapter or only run when NetEase provider IDs are already present?
2. Which NetEase fields should be normalized into the shared SongMetadata contract versus preserved as provider-specific extras?
