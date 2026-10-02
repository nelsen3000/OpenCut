# OpenCut Brand Kit

> OpenCut — the open-source CapCut alternative. A simple but powerful video editor that runs in the browser. This kit documents the brand **as it exists in this repo** (dark-first editor UI, shadcn-style token system). This copy carries the upstream OpenCut brand; nothing here renames or re-marks the project.
>
> Design fix on this branch (2026-10-02, measured): light-theme `--muted-foreground` `#808080` → `#595959` (3.95:1 FAIL → 7.0:1 AA).

## 1. Colors

Design tokens from `apps/web/src/app/globals.css` (`:root` = light, `.dark` = dark; dark is the default theme). One primary accent (blue); neutrals carry everything else.

**Light theme**

| Token | Hex | Use |
|---|---|---|
| `--background` | `#ffffff` | Page |
| `--foreground` | `#1c1c1c` | Body text |
| `--primary` | `#1389dd` | Primary actions |
| `--primary-foreground` | `#e8e8e8` | Text on primary |
| `--secondary` / `--accent` | `#e8eaed` | Secondary surfaces |
| `--muted` | `#d9d9d9` | Muted surfaces |
| `--muted-foreground` | `#595959` | Secondary text (fixed 2026-10-02) |
| `--destructive` | `#e91616` | Destructive |
| `--border` | `#d4d4d4` | Borders |
| `--card` | `#d8dbde` | Cards |
| `--panel-background` / `--panel-accent` | `#e8eaed` / `#d8dbde` | Editor panels |

**Dark theme (default)**

| Token | Hex | Use |
|---|---|---|
| `--background` | `#0a0a0a` | Page |
| `--foreground` | `#e3e3e3` | Body text |
| `--primary` | `#2298ec` | Primary actions |
| `--primary-foreground` | `#171717` | Text on primary |
| `--card` / `--popover` | `#262626` | Cards, popovers |
| `--muted-foreground` | `#a3a3a3` | Secondary text |
| `--destructive` | `#ff3333` | Destructive |
| `--border` | `#2b2b2b` | Borders |
| `--panel-background` / `--panel-accent` | `#1c1c1c` / `#262626` | Editor panels |

Chart tokens: `#3b82f6`-family blue, `#22c55e`-family green, `#f59e0b`-family amber, `#a855f7`-family purple, `#ec4899`-family pink (both themes).

**Measured contrast pairs (computed 2026-10-02):**

| Pair | Ratio | Verdict |
|---|---|---|
| Dark: `--foreground` on `--background` | 15.43:1 | AAA |
| Dark: `--primary-foreground` on `--primary` | 5.78:1 | AA |
| Dark: `--muted-foreground` on `--background` | 7.85:1 | AAA |
| Light: `--foreground` on `--background` | 20.38:1 | AAA |
| Light: `--muted-foreground` `#595959` on white (fixed) | 7.0:1 | AA |
| Light: white on `--destructive` | 4.57:1 | AA |
| Light: `--primary-foreground` on `--primary` | 3.03:1 | **FAILS AA body text** — passes 3:1 large-text/UI only. Flagged for Jon (see §8): light-theme primary buttons should use darker blue or darker text; left unchanged because dark is the default theme and this shifts the upstream brand color. |

## 2. Typography

- **Default / body:** Inter (`next/font`, `defaultFont` in `apps/web/src/lib/font-config.ts`), `--font-sans`.
- **In-editor text tool:** Inter, Roboto, Open Sans, Playfair Display, Comic Neue (+ Arial, Helvetica, Times New Roman, Georgia) — user-chosen per text element.
- Base size 0.95rem/1.5 line-height; xs 0.8rem. Tight tracking on the landing hero (`tracking-tighter`, `text-4xl md:text-[4rem]`).

## 3. Logo

Files: `apps/web/public/logo.svg` (32×32), `apps/web/public/logo.png`, favicons 16/32/96 + Android icons, `manifest.json` name "OpenCut".

The mark: a white rounded-square frame with an inner square cutout — reads as a stylized "C" / film-frame. Shown on dark backgrounds (the SVG paths are white fill). Manifest description: "A simple but powerful video editor that gets the job done. In your browser."

Clearspace: the width of the frame stroke on all sides. Minimum: 16px favicon, 24px in-app. Don't recolor the mark, don't stretch it, don't add a gradient, don't set the white SVG on a light background without the dark tile.

## 4. Tone & Voice

Plain, confident, no hype. Short sentences. The product does the talking.

Examples (from the repo's own copy):
- Good: "The Open Source Video Editor."
- Good: "A simple but powerful video editor that gets the job done. Works on any platform."
- Good: "Free & open source."
- Bad: "Unleash your limitless creative potential with our revolutionary AI-powered editing ecosystem."

Docs and UI copy use standard action words (Export, Rename, Delete, Try early beta). Never invent user counts, stars, or testimonials.

## 5. Spacing & Layout

- Radius base `--radius: 1rem`; lg = radius, md = −2px, sm = −8px. One radius language across the app.
- Editor layout: media panel (left) / preview (center) / properties panel (right) / timeline (bottom) — panels use `--panel-background`/`--panel-accent`, never glass.
- Landing: centered hero, `max-w-3xl`, generous vertical rhythm.

## 6. Components

- **Buttons** (`components/ui/button.tsx`): `default` (bg-foreground/text-background), `primary` (bg-primary/text-primary-foreground), `primary-gradient` (cyan-400→blue-500, white text — marketing use only), `destructive`, `outline`, `secondary`, `text`, `link`. Focus rings via `--ring`.
- **Theme toggle** (`components/theme-toggle.tsx`): dark default, light available; swap at the semantic token level.
- **Editor primitives:** timeline (tracks, playhead, markers), media panel with tab bar (media/text/captions/sounds/stickers/settings), properties panel, export button, onboarding overlay, keyboard-shortcuts help.
- **Feedback:** `sonner` toaster; dialogs for rename/delete.
- Motion: accordion 200ms ease-out; landing fades 600–1000ms. Respect `prefers-reduced-motion` (add where missing — flagged).

## 7. Imagery

Product screenshots and the landing background (`landing-page-dark.png`, inverted in light mode). No AI-generated marketing imagery in the repo; no text baked into images. Export thumbnails are user content.

## 8. Usage Rules — Do / Don't

Do:
- Design dark-first; verify every surface in light mode too.
- Keep the one-blue accent; neutrals carry the rest.
- Use real lucide icons as inline SVG; statuses as plain words (no emoji anywhere).
- Keep secondary text at `--muted-foreground` or darker — never hand-tuned grays.

Don't:
- Don't put body text on `--primary` in light theme (3.03:1 — fails AA). Flagged for Jon: recommend darkening light `--primary` or its foreground; needs a brand call.
- Don't add glass/translucency on cards, lists, or timeline tracks.
- Don't invent users, stats, or reviews in landing copy.
- Don't re-mark or rename the project — this copy carries the upstream OpenCut brand.

**Flags for Jon:** (1) light-theme primary-button contrast (3.03:1) needs a brand-color decision; (2) `prefers-reduced-motion` coverage in the editor's motion components is unverified; (3) hero CTA is a `Link`-wrapped `Button type="submit"` with no form — should be `type="button"` (one-line fix, left for review).
