# Changelog

All notable changes to Showtime by jimme.xyz are listed here. Versions follow [semver](https://semver.org/).

## 1.1.0 — 2026-09-23

Showtime now makes demo videos from more than the current project.

- `/showtime <url>` captures a live website or a local dev server (`http://localhost:3000`) with Hyperframes and builds the video from its screenshots, colors, fonts, and text.
- `/showtime <github-url>` reads a repo from a read-only shallow clone, and can also capture the demo site it links.
- `/showtime --idea "..."` turns an idea with no code yet into a concept video with designed UI mockups.
- `/showtime <image paths>` uses your screenshots or mockups as the real UI.
- `--duration` now allows longer demos, up to 60 seconds; 15–25 seconds stays the default.
- README explains every source with examples.

## 1.0.0 — 2026-09-23

First release of Showtime by jimme.xyz.

- `/showtime` reads the current project and turns it into a short launch video with music, motion, and share copy, rendered with Hyperframes.
- Seven tone presets plus freeform direction, landscape, vertical, and square formats, and bundled music with beat cues.
- Launch site in `docs/` with the Showtime spotlight logo, neon magenta theme, and a gallery of apps by jimme.xyz: Bookmark Importer, Factuur Analyzer, and Perceel LeadGen.
- Installable as a Claude Code plugin via `/plugin marketplace add Swanta8/showtime`.
