# Apple Music Catalog by ISRC Adapter

## Summary

Apple explicitly documents catalog song fetch by ISRC, which makes this a strong Apple-backed SongMetadata fallback when accepted matches preserve recording identity but not a direct Apple song id.

The official response includes durationInMillis, so the metadata itself is high-quality once a trustworthy ISRC is available.

The real nuance is arbitration: Apple notes that one ISRC may return more than one song, so this lane still needs bounded provider-aware result selection.

## Priority

`P1`

## Research Status

`researched`

## Candidate Class

- primary class: `authenticated identifier fallback adapter`
- secondary traits:
  - `apple-backed`
  - `ISRC-capable`
  - `high-trust duration`
  - `requires developer token`

## Upstream Snapshot

Primary sources checked:
- Apple Music API by ISRC docs: `https://developer.apple.com/documentation/applemusicapi/get-multiple-catalog-songs-by-isrc`
- GitHub: MusadoraKit: `https://github.com/rryam/MusadoraKit`
- Provider evidence note: `C:/Users/koren/src/Sinq2/axolync-songmetadata-plugin/providers/apple-music-catalog-isrc/README.md`

Observed signals during research:
- Apple explicitly documents filter[isrc] on catalog songs.
- The response includes durationInMillis, so the metadata itself is exactly what SongMetadata stage 1 needs.
- Apple warns that one ISRC may resolve to multiple songs, so this lane is strong but not completely self-arbitrating.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `YELLOW` | Still possible, but auth + multi-result handling keep this less clean than a host-owned implementation. |
| 2. WASM port/packaging | `BLACK` | There is no engine to port. |
| 3. Webview wrapper JS -> native/plugin bridge | `DARK GREEN` | A native wrapper can own Apple auth while returning only normalized duration results upward. |
| 4. Browser extension | `DARK RED` | Adds little value beyond making the auth story awkward in another place. |
| 5. Userscript | `BLACK` | Too brittle for an authenticated Apple metadata flow. |
| 6. Desktop companion | `YELLOW` | Possible, but heavier than a shared host or native bridge. |
| 7. Shared Python adapter host | `YELLOW` | Technically fine, but Node or a native bridge are cleaner first choices. |
| 8. Shared Node adapter host | `DARK GREEN` | Strong path for central token ownership and result arbitration when ISRC returns multiple songs. |
| 9. CLI wrapper around upstream/runtime | `RED` | A CLI shell is possible but not attractive for a multi-result metadata API. |
| 10. FFI/native library host lane | `RED` | Less direct than the stronger host/bridge stories. |
| 11. Forced conversion to local JS | `BLACK` | Not a conversion problem. |
| 12. Remote shared service | `DARK GREEN` | A remote broker can own tokens and resolve multi-result ISRC responses centrally. |
| 13. Cloud microservice per request / per tenant | `YELLOW` | Viable, but operationally heavier than the problem requires today. |
| 14. Platform SDK direct integration | `DARK GREEN` | Strong when Axolync later wants Apple-native adapter ownership. |

## Maintainability Read

Machine-readable rating: `medium`

- The metadata shape is strong, but the auth story and multi-result ISRC arbitration add real policy work.
- This candidate becomes more valuable as provider-affine accepted matches preserve ISRC consistently.

## Recommendation

1. `8. Shared Node adapter host`
2. `12. Remote shared service`
3. `3. Webview wrapper JS -> native/plugin bridge`

### Recommendation Notes

- This is the best Apple fallback once direct Apple song ids are absent but ISRC is preserved.
- It should stay below direct Apple catalog-id lookup when both are available.

## Open Questions

1. How often will accepted SongSense matches actually preserve ISRC in a trustworthy normalized field?
2. Should Apple ISRC responses be allowed to compete internally, or should SongMetadata require a single unambiguous match before accepting duration?
