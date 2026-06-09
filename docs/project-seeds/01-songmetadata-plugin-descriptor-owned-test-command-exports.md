# Seed 01 - SongMetadata Plugin Descriptor-Owned Test Command Exports

## Summary

Remove descriptor fallback warnings for `axolync-songmetadata-adapters` by publishing build, sanity, and full-test command metadata through the repo descriptor.

## Product Context

SongMetadata plugin is a catalog/source repo in the legacy plugin family. Its validation metadata should be descriptor-owned so Builder no longer depends on fallback command fields.

## Technical Constraints

- Do not introduce runtime authority into the repo.
- Preserve any catalog metadata that remains relevant.
- If this repo is catalog-only or zero-test, express that explicitly in descriptor metadata.

## Required Outcome

- Builder resolves SongMetadata plugin command metadata from the descriptor.
- Descriptor fallback warnings disappear for the repo.
- Focused validation proves zero-test or test command handling is descriptor-owned.

## Open Questions

- None.
