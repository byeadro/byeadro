# Setup — final README

Everything lives in your `byeadro/byeadro` profile repo. Paths below are exact.

## Files and paths

```
byeadro/
├── README.md                                    ← paste README.md here
├── assets/
│   └── banner.svg                               ← paste banner.svg here
├── .github/
│   └── workflows/
│       ├── summary-cards.yml                    ← image-2 style dashboard cards
│       ├── snake.yml                            ← contribution snake
│       ├── 3d-contrib.yml                       ← 3D isometric graph
│       ├── metrics.yml                          ← deep stats
│       ├── blog-posts.yml                       ← Substack feed
│       └── profile-3d-contrib/
│           └── settings.json                    ← orange theme for 3D graph
```

## Steps

### 1. Repo name check

The repo must be named `byeadro/byeadro`. Same as your username, exactly. Otherwise GitHub does not render it as a profile README.

### 2. Paste files

Drop everything in the folder structure above. Commit and push.

### 3. Repo permissions

Repo Settings → Actions → General → Workflow permissions → **Read and write permissions**. Save. Without this the workflows can't push generated SVGs back to the repo.

### 4. One secret to create

The metrics workflow needs a personal access token:

1. Go to https://github.com/settings/tokens?type=beta
2. Generate a fine-grained token, all repositories, read-only on Contents, Metadata, Issues, Pull requests
3. In the repo, Settings → Secrets and variables → Actions → New repository secret
4. Name: `METRICS_TOKEN`, value: paste token

The other four workflows use the built-in `GITHUB_TOKEN`. No setup.

### 5. First run

Actions tab, run each manually:

1. **Generate profile summary cards** — donut charts and dashboard cards
2. **Generate contribution snake** — orange snake to the `output` branch
3. **Generate 3D contribution graph** — writes 3D SVG to main
4. **Generate metrics** — writes metrics SVG to main
5. **Pull latest MWI posts** — populates the blog section

After the first successful run, the README fills in completely. Everything runs on schedule from then on.

## The banner scene

`assets/banner.svg` is a hand-written animated SVG. Here's what plays on a 15-second loop:

1. **0-4.5s** — The avatar walks in from the left. A speech bubble appears above his head typing "what's up" with a blinking cursor.
2. **4.5-5s** — He arrives at the light. Speech bubble fades.
3. **5-5.5s** — Front arm swings up, grabs the pull chain, one clean tug, releases.
4. **The moment of release** — Light bulb snaps on. Rays fire out. Filament glows. The big faded "AB" behind him illuminates to full orange.
5. **5.5-9.75s** — He stands in the light. Chain still swaying slightly from the tug.
6. **9.75-14.25s** — He walks off to the right.
7. **14.25-15s** — Light fades before the loop restarts off-screen.

The entire scene happens in your repo. No external service. If you edit `banner.svg`, the change shows up as soon as it commits.

Timings you might want to tweak (all in the `<style>` block near the top of the SVG):

- `walkloop 15s` — total loop length. Longer = slower everything.
- `chainpull` and `armfpos` — the pull moment. Currently synced at 35-37% of the loop.
- `raysglow`, `bulbcolor`, `abillumine` — all fire together at 37% (moment of release).
- `bubbleop` and the `<animate>` values in the bubble clipPath — bubble timing.

## What each piece does

| Piece | Where the SVG comes from | Cadence |
|---|---|---|
| Banner scene | `assets/banner.svg` in your repo | Animates in browser |
| Summary cards (donuts, stats, productive time) | Summary cards workflow | Daily |
| GitHub stats, streak, activity graph | External services | Real-time |
| Orange snake | Snake workflow → `output` branch | Every 12 hours |
| 3D contribution graph | 3d-contrib workflow → main | Daily |
| Deep metrics | Metrics workflow → main | Daily |
| Trophies | External service | Real-time |
| Tech stack icons | skillicons.dev | Static |
| Featured repos | External service | Real-time |
| Latest MWI posts | Blog workflow, Substack RSS | Daily |

## Still-a-placeholder list

- X handle in social row (link goes to x.com generic)
- Instagram handle in social row (same)
- Four pinned repos — currently `utility-bill-intelligence-platform`, `cee`, `embra-marketplace`, `shipcheck`. Swap freely in `README.md`.

## Troubleshooting

**Banner shows broken image.** Path in the README is `assets/banner.svg`. Confirm the file is at that path in your repo and pushed to `main`. If you rename the file, update the reference in `README.md`.

**Banner shows but doesn't animate.** Some browsers with reduced-motion accessibility settings turn off SVG animation. That's the user's setting, not something to fix in the SVG.

**Summary cards, 3D graph, metrics show broken images on first commit.** Normal. Those SVGs don't exist yet — they get generated when the workflows run. Trigger each workflow manually in the Actions tab.

**Snake shows broken image.** Snake needs the `output` branch created by the workflow. Check the workflow log for permission errors. If there is one, revisit Step 3.

**Substack section is empty.** Feed URL is hardcoded to `https://iambond.substack.com/feed`. If your Substack is at a different subdomain, edit `blog-posts.yml`.
