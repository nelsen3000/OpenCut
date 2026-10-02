# OpenCut AI Export Differentiator — spec (Phase 0)

Status: SPEC — no code yet. Draft PR only; nothing merges without Jon's tap.

## The differentiator in one paragraph

OpenCut does not win by out-editing CapCut — the timeline editor is table stakes. It wins at the
last step: one tap turns a finished timeline into **per-platform masters** (right pixels, right
codec, captions in safe zones) with a **C2PA authenticity manifest baked in at export time**.
Creators post natively everywhere instead of uploading one wrong-sized file, and every export
carries machine-readable proof of where it came from. Generic export is the commodity; smart,
signed export is the moat.

## Where export stands today (verified 2026-10-02)

- Client-side render via `mediabunny` (`apps/web/src/lib/export.ts`): `CanvasSource` →
  MP4/WebM, quality low → very_high.
- One export dialog (`export-button.tsx`): format + quality + include-audio, then a local download.
- **No per-platform presets. No aspect-ratio reframe. No provenance/C2PA. No AI anywhere.**

## What "AI export" means (the moat)

### 1. Per-platform export presets
One source timeline → N platform masters. Presets are driven by the house pixel table
(social-distribution report §3.2 / `@bedrock/social-content` `ImageSizes`) — **imported, never
copied**, so OpenCut and the publishing packages can never drift apart:

| Platform | Preset | Size |
|---|---|---|
| Instagram feed | Portrait / square | 1080×1350 (4:5) / 1080×1080 |
| Instagram Stories / Reels, TikTok, YouTube Shorts | Vertical video | 1080×1920 (9:16) |
| X in-feed | Landscape | 1600×900 (16:9) |
| YouTube thumbnail | Still | 1280×720 (16:9) |
| Facebook shared | Landscape | 1200×630 |
| Pinterest standard pin | Still | 1000×1500 (2:3) |
| LinkedIn post | Square | 1200×1200 |

- Reframe per preset: center-crop with subject tracking (AI helper, Phase 1); Phase 0 ships safe
  center-crop with manual nudge.
- Captions burned in with text kept **out of platform UI zones** (§3.2 safe zones: keep clear of
  top 14–15% and bottom 20–35% on vertical surfaces).
- `validateAsset()` runs **before** the export completes — the same fail-fast philosophy as the
  house scheduler: a misconfigured preset fails at export time, never as a broken upload.

### 2. C2PA manifest embedded at export time
Every export is signed with a C2PA manifest recording: the tool (OpenCut), the actions applied
(trim, crop, reframe, captions), and AI-content flags. Why now:

- **EU AI Act Art. 50** is in force (since Aug 2, 2026) — AI-generated or AI-modified media needs
  machine-readable disclosure.
- **TikTok** requires `is_aigc=true` on API publish for AI content; **Meta** labels "Made with AI".
  An export that carries its own manifest makes those disclosures automatic instead of manual.
- It is the authenticity story no generic clone has: "exported and signed by OpenCut."

This follows the house AI picture pipeline philosophy (§4.2): C2PA embedded at generation —
OpenCut applies the same rule at export time.

### 3. Phase 0 vs Phase 1
- **Phase 0 (this spec):** presets + safe zones + validate-before-export + C2PA manifest.
- **Phase 1 (later):** AI helpers — auto-reframe subject tracking, auto-captions, silence trim,
  hook detection (first-3-seconds scoring per the house video rules §4.3).

## What stays generic (not the moat)

- The timeline editor itself, the `mediabunny` render core, MP4/WebM containers.
- The local-first privacy pitch ("your videos stay on your device") — export stays client-side in
  Phase 0.
- Publishing to platforms — that is the `@bedrock/social-publishers` job, not OpenCut's. OpenCut
  produces correctly-sized, signed files; the publishers post them.

## House reuse (not duplicated)

- Pixel table: import `@bedrock/social-content` `ImageSizes` (report §3.2).
- C2PA-at-generation philosophy: house AI picture pipeline §4.2.
- BistroLens AI picture pipeline (Grok speccing 2026-10-02): OpenCut export follows the same house
  conventions; converges when Grok's spec lands rather than inventing a second standard.

## Guardrails

- Phase 0 is client-side only — no server render farm (keeps the privacy pitch honest).
- No secrets in code; signer keys are names-only placeholders until Jon wires them.
- Draft PR only. No merge, no deploy.

## Open questions for Jon

1. **Platform priority** — all 8 platforms day one, or start with TikTok / Instagram / YouTube and
   expand?
2. **C2PA signing** — self-hosted signer (we hold the keys) or a third-party C2PA service?
3. **AI helpers scope** — Phase 0 = presets + C2PA only, or green-light auto-reframe +
   auto-captions for Phase 1 now?
4. **Client-side forever?** — export stays 100% on-device, or is a server render farm acceptable
   later for heavy jobs?
