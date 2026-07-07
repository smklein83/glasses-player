# Glasses Playlist Player

A 600×600 web player for the Meta Ray-Ban Display browser: big focusable
buttons (swipe to choose, pinch to select), status and errors rendered on
the lens. Serve it over HTTPS (e.g. GitHub Pages) and open it in the
glasses browser.

It has two modes, chosen automatically at load:

| Mode | Trigger | Plays |
|------|---------|-------|
| YouTube | no `videos.json` in the repo | the playlist in `PLAYLIST_ID` via the IFrame API |
| Self-hosted | `videos.json` exists | your own video files, same UI |

## YouTube mode — currently walled off on the glasses

YouTube's anti-bot system ("Sign in to confirm you're not a bot") has been
blocking the glasses browser's fingerprint outright: every video errors
with code 150 in the embed, the plain youtube.com pages are walled too, it
follows the device across Wi-Fi/LTE, and Google sign-in does not complete
on the glasses, so the one documented cure is unavailable. Nothing a web
page can change (host, referrer, cookies, tokens, headers) gets around
that — it has to be fixed between Meta and YouTube, or the flag has to
lift on its own. The player detects the pattern (3 refused videos in a
row), stops skip-looping, and says so on the lens.

Config knobs at the top of the script in `index.html`:

- `PLAYLIST_ID` — the YouTube playlist to play
- `SHUFFLE` — `true` for random order (both modes)

## Self-hosted mode — cannot be blocked

Put a `videos.json` at the repo root listing the files to play, in order:

```json
[
  { "title": "First clip",  "src": "videos/first.mp4" },
  { "title": "Second clip", "src": "videos/second.mp4" }
]
```

Then commit the files under `videos/`. On the next load the player switches
to them automatically: same Prev / Play / Next / volume buttons, playlist
loops at the end, unplayable entries are skipped. Delete `videos.json` to
go back to YouTube mode.

Practical notes:

- GitHub Pages serves files up to 100 MB each; encode for the glasses
  (H.264 MP4, ~600px wide, AAC audio) and most clips stay well under it.
- `src` can also be a full URL on another host, but that host must allow
  cross-origin media; same-repo files avoid the issue entirely.
- Only add content you have the right to copy. Your own YouTube uploads
  can be downloaded legitimately from YouTube Studio; ripping other
  people's videos is against YouTube's terms.
