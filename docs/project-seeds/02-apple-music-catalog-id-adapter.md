# Apple Music Catalog by ID Adapter

## Summary

Apple's official catalog song endpoints expose durationInMillis directly, which makes this a high-trust SongMetadata duration enricher when the accepted match already preserves Apple song identity.

Unlike public iTunes lookup, this path avoids broad search ambiguity and should become the stronger Apple-backed lane once Axolync can own Apple credentials honestly.

The real cost is operational and architectural rather than conceptual: developer-token handling and runtime placement dominate the implementation decision.

## Priority

`P1`

## Research Status

`researched`

## Candidate Class

- primary class: `authenticated catalog metadata adapter`
- secondary traits:
  - `apple-backed`
  - `high-trust duration`
  - `requires developer token`
  - `identifier-first`

## Upstream Snapshot

Primary sources checked:
- Apple Music API Songs docs: `https://developer.apple.com/documentation/applemusicapi/songs-api`
- GitHub: MusadoraKit: `https://github.com/rryam/MusadoraKit`
- GitHub: Apple MusicKit example: `https://github.com/KoleMyers/apple-musickit-example`
- Provider evidence note: `C:/Users/koren/src/Sinq2/axolync-songmetadata-plugin/providers/apple-music-catalog-id/README.md`

Observed signals during research:
- Apple's official catalog song APIs expose durationInMillis directly.
- Real GitHub integrations exist around MusicKit / Apple Music API flows, proving this is an honest implementation family rather than a hypothetical path.
- This lane is quality-first but auth-heavy, so its real fit depends on whether Axolync wants host-brokered tokens or a native Apple bridge.

## Integration Method Fit Matrix

| Method | Fit | Why |
|---|---|---|
| 1. Local JS in ordinary browser/webview plugin | `YELLOW` | Feasible with browser-capable auth flows, but token handling and Apple configuration make this less clean than public lookup or a host-owned token lane. |
| 2. WASM port/packaging | `BLACK` | There is no engine to port; the problem is auth and HTTP orchestration. |
| 3. Webview wrapper JS -> native/plugin bridge | `DARK GREEN` | A native wrapper can own Apple platform credentials while exposing only the normalized SongMetadata result upward. |
| 4. Browser extension | `DARK RED` | Possible but awkward, and still leaves Apple token handling as the real burden. |
| 5. Userscript | `BLACK` | Too brittle for an authenticated Apple metadata lane. |
| 6. Desktop companion | `YELLOW` | A companion could own credentials, but it is still heavier than a shared host or native bridge. |
| 7. Shared Python adapter host | `YELLOW` | Python can broker the HTTP calls, but there is no particular advantage over Node here. |
| 8. Shared Node adapter host | `DARK GREEN` | Clean server-side token ownership and straightforward HTTP make this one of the strongest implementation shapes. |
| 9. CLI wrapper around upstream/runtime | `RED` | A CLI layer is possible, but weaker than a long-lived host that can cache and reuse tokens. |
| 10. FFI/native library host lane | `RED` | Possible, but usually less direct than using a host runtime or real platform bridge. |
| 11. Forced conversion to local JS | `BLACK` | There is nothing to convert; the hard part is the auth story. |
| 12. Remote shared service | `DARK GREEN` | A shared service is an honest way to centralize Apple credentials and cache catalog responses. |
| 13. Cloud microservice per request / per tenant | `YELLOW` | Feasible, but more operationally expensive than a local shared host for current Axolync scope. |
| 14. Platform SDK direct integration | `DARK GREEN` | Apple-native SDK paths are real here, especially if Axolync eventually leans into native wrapper lanes. |

## Maintainability Read

Machine-readable rating: `medium`

- The API semantics are strong and stable compared with scrape-style alternatives.
- Credential handling, token refresh, and runtime placement are the real long-term maintenance cost.

## Recommendation

1. `8. Shared Node adapter host`
2. `3. Webview wrapper JS -> native/plugin bridge`
3. `14. Platform SDK direct integration`

### Recommendation Notes

- This is a very strong Apple-backed duration enricher once the platform is ready to own Apple credentials honestly.
- It should outrank public iTunes search/lookup when the accepted match already preserves Apple song identity.

## Open Questions

1. Will accepted SongSense matches preserve Apple song ids directly enough to justify this as a first implemented Apple adapter rather than a later hardening lane?
2. Should Axolync prefer a server/host token broker or a wrapper-native Apple Music lane first?
