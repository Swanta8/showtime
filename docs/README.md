# showtime — site

Static single-page launch site for `/showtime`. Plain HTML/CSS/JS, no framework, no build step. This folder is the GitHub Pages deploy root.

## Local preview

```bash
python3 -m http.server 8000
open http://localhost:8000
```

## Sanity check

```bash
node ../scripts/check-docs.mjs
```

This verifies local docs links and media paths, plus the gallery count copy.

## Deploy (GitHub Pages)

Fully self-contained — no build step. In the repo's **Settings → Pages**, set the source to **Deploy from a branch**, branch `main`, folder **`/docs`**.

The gallery ships committed: each app shown (`examples/bookmark-importer/`, `examples/factuur-analyzer/`, `examples/perceel-leadgen/`) has a `showtime.jpg` poster and a `site.jpg` thumbnail, plus a `showtime.mp4` once its video is rendered. Cards without a video show the poster as a still (`<img class="card-still">`). Heavy composition sources (`showtime-output-*/`) are git-ignored.

## Adding / updating a gallery example

1. Render the showtime video and place `showtime.mp4`, `showtime.jpg` (poster), and `site.jpg` (thumbnail) under `docs/examples/<slug>/`. Pick `showtime.jpg` as the **best** frame, not an arbitrary one — grab the video's strongest settled beat (the hook line, or the hero/logo reveal) full-res with ffmpeg, then bake it as the video's frame 0 so idle thumbnails everywhere show it:

   ```bash
   # extract the best settled beat as the poster
   ffmpeg -ss 3.2 -i docs/examples/<slug>/showtime.mp4 -frames:v 1 -q:v 2 docs/examples/<slug>/showtime.jpg

   # replace only frame 0 with the poster (same duration, frames, and audio)
   cd docs/examples/<slug>
   ffmpeg -y -i showtime.mp4 -i showtime.jpg \
     -filter_complex "[0:v][1:v]overlay=0:0:enable='eq(n,0)'[v]" \
     -map "[v]" -map 0:a? -c:v libx264 -crf 18 -preset slow -pix_fmt yuv420p \
     -c:a copy -movflags +faststart showtime.poster.mp4 && mv showtime.poster.mp4 showtime.mp4
   ```
2. Add a card in `index.html` pointing at those paths.
3. Un-ignore the slug in `.gitignore` and commit the assets.
