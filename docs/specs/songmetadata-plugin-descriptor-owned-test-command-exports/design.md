# SongMetadata Plugin Descriptor-Owned Test Command Exports Design

## Overview

Publish SongMetadata plugin validation metadata through descriptor exports or explicit zero-test catalog metadata.

## Design

- Add descriptor build, sanity, and full-test command exports if tests remain applicable.
- Otherwise encode explicit descriptor-owned zero-test status.
- Validate Builder report discovery without fallback config.

## Non-Goals

- No runtime authority changes.
- No SongMetadata provider implementation work.
