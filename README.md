<img src="docs/assets/logo.svg" alt="Showtime by jimme.xyz logo" width="72">

# Showtime by jimme.xyz

**You built it. Now it is showtime.**

![the /showtime launch site — you built it, now it is showtime](docs/assets/hero.png)

Showtime is a Claude Code skill, run with `/showtime`, that turns the project you created into a short, shareable launch video — music, motion, and share copy included. One command, powered by [Hyperframes](https://hyperframes.heygen.com/).

Made by [jimme.xyz](https://jimme.xyz).

## Install

```bash
/plugin marketplace add Swanta8/showtime
/plugin install showtime@showtime
```

Then run `/showtime` inside any project.

**Any other agent** — one command via the [`skills`](https://github.com/vercel-labs/skills) CLI (Cursor, Codex, Copilot, Gemini CLI, opencode, and more):

```bash
npx skills add https://github.com/Swanta8/showtime --skill showtime
```

Add `-g` to install globally (available in every project); drop it to scope to the current one.

<details>
<summary>No installer? Copy the skill directly.</summary>

```bash
rsync -a --exclude '.DS_Store' skills/showtime/ ~/.claude/skills/showtime/
```

Restart Claude Code after copying.
</details>

### Also works with

This repo exposes the skill at every agent's standard discovery path via symlinks. No extra config needed.

| Agent | How it discovers |
|---|---|
| **Google Antigravity** | Auto-detects from `.agents/skills/showtime/` at project root or `~/.gemini/config/skills/showtime/` globally |
| **opencode** | Auto-detects from `.opencode/skills/showtime/` at project root |
| **Codex CLI** | Reads `.agents/skills/showtime/`, walking up to repo root |
| **Claude Code** | Also reads `.claude/skills/showtime/` (in addition to the `.claude-plugin/` marketplace install above) |
| **Other agents** | Point custom instructions at `skills/showtime/SKILL.md` — see [`docs/other-agents.md`](docs/other-agents.md) |

> **Windows users:** Git requires `git config core.symlinks true` (or `git clone -c core.symlinks=true`) and Windows Developer Mode or Administrator privileges to create symlinks. If symlinks don't work on your system, copy `skills/showtime/` to the agent's skill directory manually instead.

## Use it

From any project directory, ask your agent:

```text
let's /showtime
```

Or steer the tone:

```text
/showtime --tone "fake Series A launch from 2016"
```

Voiceover is off by default. Enable it explicitly with:

```text
/showtime --voice
```

Narration uses Kokoro through Hyperframes when enabled.

You get a `showtime-output/` folder with the plan, a composition brief, share copy, and the rendered `showtime.mp4`.

## How it works

`/showtime` owns the story — the product angle, tone, and which moments to show. It hands a focused brief to [Hyperframes](https://hyperframes.heygen.com/), which builds, times, and renders the video.

## Requirements

- An agent that supports Agent Skills — Claude Code, opencode, Codex CLI, or any agent with custom instructions (see "Also works with" above)
- Node.js 22+
- FFmpeg on `PATH`
- Hyperframes CLI — `npx hyperframes` (check it with `npx hyperframes doctor`)

## What's in this repo

- `skills/showtime/` — the skill, references, and bundled music + SFX
- `docs/` — the launch site (GitHub Pages)
- `.claude-plugin/` — plugin manifest + marketplace catalog
- `.claude/skills/showtime/` — symlink → `skills/showtime/` (Claude Code discovery)
- `.agents/skills/showtime/` — symlink → `skills/showtime/` (Codex CLI + opencode discovery)
- `.opencode/skills/showtime/` — symlink → `skills/showtime/` (opencode discovery)

## Credits

- Music — [ende.app](https://ende.app/en) "Happy Beats / Business Moves"
- Sound effects — [Kenney](https://kenney.nl/)
- Video generation — [Hyperframes](https://hyperframes.heygen.com/)

## Contributing

Contributions, ideas, and new demo showtimes are welcome — open an issue or a PR.
