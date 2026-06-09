# SongMetadata Plugin Descriptor-Owned Test Command Exports Requirements

## Requirement 1

**User Story:** As Builder, I want SongMetadata plugin command metadata from the descriptor, so catalog/source validation does not rely on fallback config.

### Acceptance Criteria

1. WHEN Builder resolves SongMetadata plugin commands THEN it SHALL use descriptor-owned build, sanity, and full-test exports or explicit zero-test status.
2. WHEN descriptor exports are available THEN SongMetadata plugin fallback warnings SHALL disappear.
3. WHEN the repo is catalog-only THEN no runtime authority SHALL be introduced.

## Requirement 2

**User Story:** As a cleanup reviewer, I want descriptor cleanup to clarify this repo's role, so legacy plugin ambiguity does not remain in reports.

### Acceptance Criteria

1. WHEN validation runs THEN the repo SHALL be classified through descriptor metadata.
2. WHEN command exports are missing THEN validation SHALL fail or emit red warnings.
