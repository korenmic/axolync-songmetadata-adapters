# Axolync SongMetadata Adapters

Shared lane-level home for cross-provider `SongMetadata` research that is not owned by a dedicated provider addon repo.

Current scope:

- keep lane-level SongMetadata thinking separate from provider-specific addon repos
- stay narrow around product-relevant normalized fields
- avoid duplicating provider adapter authority that now lives in dedicated addon repos

Provider-specific SongMetadata adapter research has moved into:

- `axolync-addon-itunes`
- `axolync-addon-spotify`
- `axolync-addon-musicbrainz`

This repo does not ship runtime adapters yet.
