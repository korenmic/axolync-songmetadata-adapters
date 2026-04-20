# Spotify URL Info

## Why this is a SongMetadata candidate

`spotify-url-info` is a practical low-friction fallback when a match preserves a Spotify URL but the product does not want to own Spotify auth immediately.

## Primary sources checked

- GitHub: `https://github.com/microlinkhq/spotify-url-info`
- Spotify Track docs for shape comparison: `https://developer.spotify.com/documentation/web-api/reference/get-track`

## Key signals

- MIT-licensed JavaScript package
- Works from Spotify URLs and a caller-provided fetch agent
- Advertises a response shape close to Spotify Web API output
- More fragile and less contract-clean than the official Spotify developer API

