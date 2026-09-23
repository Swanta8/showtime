---
name: showtime
description: Turn a project, website, app, GitHub repo, idea, or set of screenshots into a short, polished, shareable demo or launch video using Hyperframes. Use when someone says "/showtime", "it's showtime", "make a launch video", "make a demo video of this site", "turn this URL into a video", "turn this idea into a video", or wants to share what they built. Works from the current project code by default; a URL, repo link, idea, or image paths in the invocation switch the source.
---

# /showtime

You built it. Now it is showtime.

## Invocation dispatch (must happen first)

Before inspecting the project, parse the complete `/showtime` invocation. If the
invocation contains `--voice`, set `voice.enabled = true`. Enable narration
only for that run. Do not enable narration automatically and do not fall back
to the normal no-voice workflow.

`/showtime` turns a project, website, app, idea, or set of screenshots into a short, polished, shareable demo video using Hyperframes. It is opinionated and fun, and it always shows the real thing when there is one.

## What this skill does

1. Reads the source to understand the product: the current project code, a live website, a GitHub repo, an idea, or screenshots.
2. Plans a short showtime concept specific to this product.
3. Scripts and storyboards the video.
4. Hands a focused composition brief to Hyperframes.
5. Validates, renders, and writes share copy.

## Parsing the invocation

The user may invoke with natural language or flags:

```
/showtime
/showtime --tone chaotic
/showtime --tone polished --format vertical
/showtime this. Make it feel like a ridiculous startup launch.
/showtime https://example.com
/showtime http://localhost:3000 --tone app-store
/showtime https://github.com/owner/repo
/showtime --idea "a budgeting app that roasts your spending"
/showtime ./screens/home.png ./screens/result.png
```

Parse these options:

| Option | Values | Default |
|---|---|---|
| `--tone` | preset or freeform description | inferred |
| `--format` | `landscape`, `vertical`, `square` | `landscape` |
| `--duration` | seconds, up to 60 | auto (15-25s) |
| `--no-music` | flag | music on |
| `--no-sfx` | flag | sfx on |
| `--title` | string | inferred from project |
| `--voice` | flag | narration off |
| `--idea` | description of a product that does not exist yet | none |
| URL, repo link, or image paths | positional | current project |

## Choosing the source

Pick exactly one primary source from the invocation, in this order:

1. `--idea "<text>"`, or a request that describes a product with no code, URL, or images: **idea**.
2. A `https://github.com/<owner>/<repo>` (or other git host) link: **repo**.
3. Any other `http://` or `https://` URL, including `localhost`: **website**.
4. One or more image paths (`.png`, `.jpg`, `.webp`) or images pasted into the conversation: **images**.
5. Nothing of the above: **project**, the current working directory.

Images can also be added next to another source as extra UI material. Record the chosen source at the top of `showtime-plan.md`. Each source has its own inspection path in Step 1.

Voice is opt-in. If `--voice` is present, use Kokoro via Hyperframes and do
not add any provider-selection logic. The voice workflow is intentionally
single-provider.

Tone can be a preset (`default`, `polished`, `yc-parody`, `chaotic`, `deadpan`, `cinematic`, `app-store`) or a creative direction such as "fake Series A launch from 2016", "museum exhibit", or "overproduced mobile game ad".

When the user gives freeform tone direction, map it to the nearest preset for pacing and structure, but preserve the user's direction in the plan and composition brief.

## Narration guidance

When `--voice` is enabled, write narration that complements the visuals, does
not simply read visible text, matches scene pacing, sounds natural and
conversational, and moves smoothly between scenes. Keep the script concise and
specific to the product so the voice feels like part of the edit rather than a
separate narration track.

---

## Output directory

By default, output goes to `showtime-output/`. To avoid overwriting previous runs, use a timestamped directory:

```
showtime-output-2026-05-04-143022/
```

Use a timestamp when:
- The user explicitly asks for a new run without overriding previous results
- A `showtime-output/` directory already exists in the project

Generate the timestamp at the start of the run (`YYYY-MM-DD-HHmmss`) and use it consistently for all output paths in that run: plan, brief, composition, render, and share copy.

## Skill directory

`<skill-dir>` is the directory containing this `SKILL.md`. Claude Code prints it as "Base directory for this skill" when the skill loads; for other agents it's wherever the skill was installed. Bundled assets are under `<skill-dir>/assets/` and scripts under `<skill-dir>/scripts/`. Don't guess an install path: a plugin install, a `~/.claude/skills/` copy, and this repo all put it somewhere different.

---

## Step 1: Inspect the project

**Read:** [references/step-1-inspect.md](references/step-1-inspect.md)

Inspect the chosen source (project, website, repo, idea, or images) and extract the information needed to plan the showtime video.

**Gate:** You can answer all 9 questions in the showtime planning rubric.

---

## Step 2: Plan and storyboard

**Read:** [references/step-2-plan.md](references/step-2-plan.md)

Write `<output-dir>/showtime-plan.md` (where `<output-dir>` is `showtime-output/` or the timestamped variant chosen above). Answer the planning rubric. Commit to a creative angle. Write the beat-by-beat storyboard including scenes, text, timing, transitions, and SFX cues.

When music is selected, include a compact `Music cue guidance` section: read the bundled track's cue preset from `<skill-dir>/assets/music/cues/` if present, otherwise note cues will be detected at composition time (any track now supports beat sync — see `references/audio.md`). Cue metadata is optional timing guidance only: story, readability, pacing, and product clarity stay primary.

**Gate:** `<output-dir>/showtime-plan.md` exists with a full storyboard. Scene durations sum to 15–25 seconds, or to the requested `--duration` (at most 60).

---

## Step 3: Hand off to Hyperframes

**Read:** The Hyperframes domain skills — `hyperframes-core`, `hyperframes-animation`, `hyperframes-creative`, `hyperframes-keyframes`, `hyperframes-cli`. /showtime is its own workflow: do not enter the `hyperframes` entry-point intent interview or route into its generic promo / launch-video workflow.
**Read:** [references/step-3-compose.md](references/step-3-compose.md)
**Read:** [references/audio.md](references/audio.md)

Write the composition brief and use Hyperframes to create the video implementation in `<output-dir>/composition/`.

`/showtime` owns the product angle, source material, storyboard, tone, format, audio selection, music cue guidance, and delivery expectations. Hyperframes owns the concrete composition structure, exact animation timing, animation mechanics, runtime choices, linting rules, and render workflow.

**Gate:** `npx hyperframes check` passes with zero errors inside `<output-dir>/composition/` (the single browser gate before render — see hyperframes-cli for what it audits).

---

## Step 4: Validate, render, and deliver

**Read:** [references/step-4-deliver.md](references/step-4-deliver.md)

Validate, preview, render to `<output-dir>/showtime.mp4`, pick the best poster frame into `<output-dir>/showtime.jpg`, bake that poster as the video's frame 0 so it's the idle thumbnail everywhere, and write `<output-dir>/share-copy.txt`.

**Gate:** `<output-dir>/showtime.mp4` exists. A best-frame poster `<output-dir>/showtime.jpg` is picked (not an arbitrary frame) and baked as frame 0 of `showtime.mp4`. Share copy is written.

---

## Tone system

Seven tone presets ship with `/showtime`. Each changes scripting energy, pacing, typography personality, and transition style. Presets are defaults, not limits.

Full definitions: [references/tones.md](references/tones.md)

| Tone | Energy | One-liner |
|---|---|---|
| `default` | Playful, clean, postable | The good-vibes default |
| `polished` | Serious, elegant | For projects that are not jokes |
| `yc-parody` | Deadpan startup energy | Fake seriousness applied to absurd projects |
| `chaotic` | Fast, loud, aggressive | Over-the-top and unhinged |
| `deadpan` | Calm, dry, understated | The joke is that nothing is a joke |
| `cinematic` | Dramatic, trailer-scale | Big motion, bigger claims |
| `app-store` | Smooth, feature-card clean | Corporate but not boring |

Always allow a freeform creative direction to refine or override the preset.

---

## Creative laws

These apply to every showtime video regardless of tone.

**Short.** 15–25 seconds by default. A longer demo (up to 60 seconds) only when the user asks for it with `--duration`.

**Readable.** Keep the pace high through motion and cuts, never by flashing text. Every line a viewer must read holds long enough to read it (short label ~0.8s settled; a sentence ~0.3s per word). Fast-in, then hold — never fast-in, then gone.

**Specific.** The video must feel like it was made for this exact product, not any product.

**Show the thing.** At least one scene must display actual UI, copy, or a key visual from the product. For an idea, that is a designed mock of its UI, based on the idea itself. No abstract filler.

**No generic SaaS language.** "Streamline your workflow" is banned. Use the project's actual copy and claims.

**The hook is everything.** The first 2 seconds determine whether someone keeps watching. Plan the hook before anything else.

**Funny earns its place.** Humor should come from the project's absurdity, not from trying to be funny.

**Pattern:**
```
Hook (2-3s) → Reveal (2-4s) → 2-3 sharp highlights (5-12s) → Punchline/outro (2-4s)
```

Adapt this. Not every project needs exactly 3 highlights. The pattern is a starting shape, not a template.
