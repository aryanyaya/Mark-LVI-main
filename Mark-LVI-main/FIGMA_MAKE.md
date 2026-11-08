# MARK LVI — Figma Make design spec

Paste this whole document into **Figma Make** (or feed it section by section) to
generate a faithful replica of the JARVIS / MARK LVI interface. Every colour,
size and label below is taken directly from the shipping app (`ui.py`,
`dashboard/static/*.html`), so what Figma Make builds will match the real thing.

**Product:** MARK LVI — a desktop voice-assistant HUD (dark, monospaced,
sci-fi instrument panel) plus a phone dashboard web app. Two visual families:

* **Desktop HUD** — near-black teal, `Courier New` everywhere, 1px borders,
  glowing cyan accents.
* **Phone dashboard** — softer dark navy, system sans-serif, indigo accent,
  rounded cards.

---

## 1. Design tokens — desktop HUD

### 1.1 Colour palette (default theme)

| Token | Hex | Used for |
|---|---|---|
| `BG` | `#00060a` | window background, HUD canvas |
| `DARK` | `#000d14` | header, side panels, footer, drawer, text displays |
| `PANEL` | `#010d14` | content/quiz panel, log background, drop zone |
| `PANEL2` | `#010f18` | metric-bar cards, info card, badges, quiz buttons |
| `BAR_BG` | `#011520` | progress-bar troughs |
| `BORDER` | `#0d3347` | default 1px borders, dividers |
| `BORDER_A` | `#0f4060` | metric-bar outlines, badge borders |
| `BORDER_B` | `#1a5c7a` | strong borders, hover borders, panel top edges |
| `PRI` | `#00d4ff` | primary cyan — titles, active borders, CPU bar |
| `PRI_DIM` | `#007a99` | dim cyan — subtitles, secondary borders |
| `PRI_GHO` | `#001f2e` | ghost-fill hover, grid dots, selected card fill |
| `ACC` | `#ff6b00` | orange — SPEAKING state, GPU bar, confirm/danger accent |
| `ACC2` | `#ffcc00` | yellow — THINKING state, MEM bar, OS label |
| `GREEN` | `#00ff88` | LISTENING, NET bar, success, ON toggles |
| `GREEN_D` | `#00aa55` | ON-toggle border |
| `RED` | `#ff3355` | errors, >85% bar fill, delete hover |
| `MUTED_C` | `#ff3366` | MUTED state, interrupt button |
| `TEXT` | `#8ffcff` | primary body text |
| `TEXT_MED` | `#5ab8cc` | secondary text, section headers |
| `TEXT_DIM` | `#3a8a9a` | captions, hints, disabled |
| `WHITE` | `#d8f8ff` | strong text, user log lines, filenames |
| `TMP bar` | `#ff6688` | temperature bar only |

**Accent hue-shift:** every token except `ACC`, `ACC2`, `GREEN`, `GREEN_D`,
`RED`, `MUTED_C` and `TMP` is derived from one accent hue — the whole teal
family hue-shifts together when the user picks a UI colour. Build all of them
from a single `--accent` variable so one change re-themes the design.

### 1.2 Typography

* **Font:** `Courier New` for **everything** in the desktop app. Weights: Regular
  and Bold. Sizes are in pt; at 1×≈ px.
* Phone dashboard: `-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`,
  with `Courier New` only for the pairing-key input.

| Role | Size | Weight |
|---|---|---|
| Window/app title (header centre) | 17 | Bold |
| Overlay panel titles (`⚙  CUSTOMISE ASSISTANT`) | 12–13 | Bold |
| Clock | 14 | Bold |
| Section titles in panels (`◈ SYS MONITOR`, `▸ ACTIVITY LOG`) | 7 | Bold |
| Overlay body/labels | 8–10 | Regular |
| Captions, hints, footnotes | 7 | Regular |
| Buttons | 7–9 | Bold |
| HUD status line (`◈  THINKING`) | 11 | Bold |
| Metric label / value | 7 Bold / 9 Bold | Bold |

**Label convention:** every section title is `SYMBOL + two spaces + UPPERCASE`,
e.g. `◈ SYS MONITOR`, `▸ ACTIVITY LOG`, `🔌  ENGINE & PROVIDERS`. Common
symbols: `◈ ▸ ● ○ ◆ ✕ ⚙ 🎛 🎧 🧠 🧩 🔌 ⚠ ♠`.

### 1.3 Shape, spacing, elevation

* Overlay cards: radius **6px** (Providers **8px**, Remote Access **14px**).
* Inner controls (buttons, inputs, combos): radius **3px**; section boxes and
  mode cards **5px**; clipboard action buttons **2px**.
* Overlay inner padding: **20/16** (sides/vertical), Setup **30/22**,
  Providers & Plugin Settings **22/18**, Remote **24/16**.
* Dividers: 1px `BORDER`, margin `2px 0`.
* No shadows anywhere — separation comes from 1px borders and background steps.
* Overlays float **centred** over the HUD with background
  `rgba(0,6,10,0.96)` + 1px `BORDER_B` border + 6px radius (dim the whole app
  behind them only slightly; the HUD keeps animating).

### 1.4 Component recipes

**Primary ghost button** — `▸  INITIALISE SYSTEMS`
`background: transparent` (hover `PRI_GHO`) · `color: PRI` ·
`border: 1px solid PRI_DIM` · radius 3 · hover border `PRI`.

**Secondary button** — `CLOSE`, `CANCEL`
`background: transparent` · `color: TEXT_MED` · `border: 1px solid BORDER` ·
radius 3 · hover `color: TEXT`, border `BORDER_B`.

**Drawer menu button** (left-aligned text, height 26–30)
`background: #00091a` (dim variant: transparent) · `color: PRI` (dim variant:
`TEXT_MED`) · `border: 1px solid PRI_DIM` (dim: `BORDER`) · radius 3 ·
hover fill `PRI_GHO`.

**ON / OFF toggle** (72×24 or full width, text `ON`/`OFF`)
* ON — `bg #001a08 · color #00ff88 · border 1px #00aa55`
* OFF — `bg transparent · color #3a8a9a · border 1px #0d3347`
* BROKEN (disabled) — `color #3a8a9a · border #0d3347`

**Text input** — `background #000d12 · color TEXT · border 1px BORDER ·
radius 3 · padding 4px 8px` · focus `border 1px PRI` · height 27–32.

**Combo box** — same as text input, height 29–30, font 9; popup selection row
fill `PRI_GHO`.

**Danger button** (interrupt) — `background #140008 · color MUTED_C ·
border 1px MUTED_C · radius 3`, hover `#200010`.

**Scrollbar** — track `BG/BG`, width 6–9px, no border; handle `BORDER_B`,
radius 3–4, min height 16–30.

---

## 2. Screen: Main HUD (desktop, default 980×700, min 820×580)

Window title: `JARVIS — MARK LVI`. Layout top→bottom:

```
┌─────────────────────────────────────────────────────────────┐
│ HEADER (54px)                                               │
├────────┬───────────────────────────────────┬────────────────┤
│ LEFT   │  CENTER (QSplitter, stretch 5)    │ RIGHT (340px)  │
│ 148px  │  ┌─ HUD / camera / video ──────┐  │ ACTIVITY LOG   │
│        │  │        (stretch 3)          │  │ (flex)         │
│        │  ├─ content panel (hidden) ────┤  │ ─────────────  │
│        │  │      ~220px when open       │  │ FILE UPLOAD    │
│        │  ├─ quiz panel (hidden) ───────┤  │ ─────────────  │
│        │  └─ 250px when open ──────────┘  │ COMMAND INPUT  │
│        │                                  │ INTERRUPT      │
│        │                                  │ MIC button     │
├────────┴──────────────────────────────────┴────────────────┤
│ FOOTER (22px)                                               │
└─────────────────────────────────────────────────────────────┘
```

### 2.1 Header (54px, `DARK`, bottom border 1px `BORDER_B`, padding 0 16)

Left → right:

1. Version badge `MARK LVI` — Courier 8, `PRI_DIM`.
2. **⚙** button 26×26, Courier 11 — Settings drawer trigger. Icon `TEXT_DIM`,
   border 1px `BORDER` radius 4; hover `PRI`; checked (open) `PRI` icon +
   `PRI_GHO` fill.
3. **🎛** button 26×26 — Controls drawer trigger, same style.
4. *(stretch)*
5. Centre block (vertically stacked, centred):
   * `JARVIS` — Courier **17 Bold**, `PRI` (upper-cased assistant name).
   * `A Friendly Assistant` — Courier 7, `PRI_DIM` (custom names get
     `Personal AI Assistant`; a custom long name gets
     `Just A Rather Very Intelligent System`).
6. *(stretch)*
7. Right block, right-aligned:
   * `00:00:00` clock — Courier **14 Bold**, `PRI`, updates every second.
   * `Mon 08 Nov 2026` date — Courier 7, `TEXT_DIM`.

### 2.2 Left panel (148px, `DARK`, right border 1px `BORDER`, padding 8/10)

1. Header `◈ SYS MONITOR` — Courier 7 Bold, `PRI`, bottom border 1px `BORDER`,
   padding-bottom 4.
2. **Five metric bars** (each 38px tall, min-width 80, card radius 4,
   fill `PANEL2`, outline 1px `BORDER_A`, padding 6):
   * Label top-left, Courier 7 Bold, `TEXT_DIM`.
   * Value top-right, Courier 9 Bold, in the bar's colour (`--` while warming).
   * 4px track along the bottom (y = H−9), fill `BAR_BG`, radius 2, inner fill
     colour: `>65%` → `ACC`, `>85%` → `RED`, else the bar's own colour.

   | Bar | Colour |
   |---|---|
   | `CPU` | `PRI` |
   | `MEM` | `ACC2` |
   | `NET` | `GREEN` |
   | `GPU` | `ACC` |
   | `TMP` | `#ff6688` |

3. **Info card** (`PANEL2`, 1px `BORDER`, radius 4, padding 6/5), 3 lines:
   * `UP  3:42` — Courier 8 Bold, `GREEN`
   * `PROC  214` — Courier 8, `TEXT_MED`
   * `OS  WIN` (or `macOS` / `LINUX`) — Courier 8, `ACC2`
4. *(stretch)*
5. **Three status badges** stacked at the bottom, centred, `PANEL2` fill,
   1px `BORDER_A`, radius 3, padding 4, Courier 7 Bold:
   * `AI CORE` / `ACTIVE` — `GREEN`
   * `SEC` / `CLEARED` — `PRI`
   * `PROTOCOL` / `MARK LVI` — `TEXT_DIM`

### 2.3 Centre — the HUD

Background `BG` with a **dot grid**: 1px dots every 48px in `PRI_GHO`.

Two modes (switchable from the Controls drawer):

**A. Animated face mode** — a wireframe/lit human head (dark shaded triangles,
cyan wireframe + scanline, accent-coloured iris/pupil/brows, glowing aura),
centred in the upper ⅔, with blinking and gaze saccades.

**B. Reactor-core mode** — concentric instrument graphic, centre `(W/2, H/2)`:

1. Radial atmosphere glow (brighter with voice level).
2. Four L-shaped corner frame marks, alpha 0.45.
3. Crosshair lines broken by a gap around the core, alpha 0.16.
4. Two outer circles (r and 0.93r), alpha 0.34 / 0.16.
5. **72 graduation ticks** every 5° (24 majors alpha 0.42, 48 minors 0.18).
6. **Five sweeping arc rings** at different radii/spans, rotating (faster while
   THINKING or SPEAKING).
7. **Voice ring:** 60 radial spikes that grow with audio level.
8. Inner ring + the assistant name centred inside it — Courier Bold, white,
   alpha 0.6–1.0 with voice.

**Status line** (below the core, horizontally centred, Courier **11 Bold**,
blink symbol swaps every ~0.63s):

| State | Text | Colour |
|---|---|---|
| muted | `⊘  MUTED` | `MUTED_C` |
| speaking | `●  SPEAKING` | `ACC` |
| thinking | `◈  THINKING` / `◇` | `ACC2` |
| processing | `▷  PROCESSING` / `▶` | `ACC2` |
| listening | `●  LISTENING` / `○` | `GREEN` |
| idle/sleep | `●  SLEEPING` / `○` | `PRI` |
| boot | `●  INITIALISING` / `○` | `PRI` |

**Waveform:** 36 bars, 7px wide + 1px gap (288px total, centred), 20px strip
below the status line, max height 24px. Idle: gentle ripple in `BORDER_B`.
Active: centre-weighted envelope, `PRI` when tall / `PRI_DIM` when short.
Muted: flat 2px bars in `MUTED_C`.

### 2.4 Right panel (340px, `DARK`, left border 1px `BORDER`, padding 8)

1. `▸ ACTIVITY LOG` — Courier 7 Bold, `TEXT_MED`.
2. **Log widget** (flex 1): `PANEL` fill, 1px `BORDER`, radius 4, padding 6,
   Courier 9. Lines appear with a **typewriter effect (6ms/char)** and are
   colour-tagged by prefix: `you:` → `WHITE`, `JARVIS:` → `PRI`,
   `file:` → `GREEN`, contains `err` → `RED`, else `TEXT_MED`. Selection fill
   `PRI_GHO`; scrollbar handle `BORDER_B`. Scrollback 600 lines.
3. Divider (1px `BORDER`).
4. `▸ FILE UPLOAD` header.
5. **Drop zone** (100px tall, radius 6, **dashed** 1.5px border, animated dash
   offset):
   * Idle: `PANEL` fill, dashed `BORDER` (alpha 0.6); upload-arrow glyph +
     `Drop a file or folder  or  Click to Browse` (Courier 8, `PRI_DIM`) +
     `Images · Video · Audio · PDF · Docs · Code · Project folders`
     (Courier 7, `#1a4a5a`). Hover: `#001218` fill, `BORDER_B` border, text
     `TEXT`.
   * Drag-over: `#001a24` fill, `PRI` dashed border, big `⬇` (Courier 20) +
     `Release to load` (Courier 8 Bold, `PRI`).
   * Loaded: `GREEN` dashed border, 22px category emoji left, filename
     (Courier 8 Bold `WHITE`, truncated at 34 chars), `PDF · 1.2 MB`
     (Courier 7 `TEXT_DIM`), parent path (Courier 6 `#1e5c6a`), red `✕`
     clear button at the right edge.
6. Hint line, Courier 7 `TEXT_MED`, wraps:
   `No file loaded — drop or click above to upload`
   (after load: `📄  report.pdf  ·  1.2 MB  ·  Tell JARVIS what to do with it`).
7. Divider.
8. `▸ COMMAND INPUT` header.
9. **Input row:** text field (flex, 30px, `background #000d14`, `WHITE` text,
   border `BORDER`, radius 3, placeholder `Type a command or question…`,
   focus border `PRI`) + `▸` send button 30×30 (`PANEL` fill, `PRI` glyph,
   border `PRI_DIM`, hover `PRI_GHO` fill + `PRI` border).
10. **`✋  INTERRUPT  [ESC]`** — full width, 34px, Courier 8 Bold, danger style
    (`#140008` / `MUTED_C`).
11. **Mic button** — full width, 30px, Courier 8 Bold, two states:
    * `🎙  MICROPHONE ACTIVE` — `bg #00140a · GREEN text · GREEN border`
    * `🔇  MICROPHONE MUTED` — `bg #140006 · MUTED_C text · MUTED_C border`

### 2.5 Footer (22px, `DARK`, top border 1px `BORDER`, padding 0 14)

Courier 7: left `[F4] Mute  ·  [F11] Fullscreen` (`TEXT_MED`), stretch, centre
`By FatihMakes` (`PRI_DIM`).

### 2.6 Content panel (below the HUD, hidden by default, opens ~220px tall)

`PANEL` fill, **top border 1px `BORDER_B`**, padding 12/7:

* Header row: `◈` (Courier 9 Bold `PRI`) + title (Courier 8 Bold `PRI`,
  letter-spacing 1, UPPERCASE, max 48 chars) + stretch + timestamp
  (Courier 7 `TEXT_DIM`) + `DISMISS  ✕` button (18px tall, Courier 7,
  transparent, 1px `BORDER` radius 2, hover `TEXT`/`BORDER_B`).
* 1px divider.
* Read-only text area: `DARK` fill, `TEXT` text, 1px `BORDER`, radius 3,
  padding 6/8, Courier 8.

Variants rendered into the same panel:

* **Code-agent job** — phase strip `● READING ● PLANNING ○ WRITING ○ CHECKING`
  (done `GREEN`, current `PRI`, pending `TEXT_DIM`) + elapsed `· 42s`, then
  `REQUEST …`, a summary quote (2px `PRI` left border), `FILES` list with
  state marks (`○ writing ✎ done ✓ fixing ↻ failed ✕`), and
  `STEPS — newest first` timed log lines.
* **Document review** — summary block (2px `PRI` left border), then findings:
  severity mark (`▲` serious/`RED`, `●` caution/`ACC2`, `·` note/`PRI_DIM`) +
  bold white heading, indented detail, quoted line (1px `BORDER` left border,
  `TEXT_DIM`), `→ suggestion` in `PRI`; footer `The document does not settle:`
  list.
* **Quiz panel** (sibling, ~250px): same chrome but `QUIZ` title, counter
  `3 / 10` right-aligned, question (Courier 9 `WHITE`), stacked answer buttons
  (`PANEL2` fill, 1px `BORDER` radius 2, Courier 8, left-aligned, min-height
  24; primary variant `PRI` text + `BORDER_B` border), or a typed-answer row
  (field + `ANSWER`), feedback note (`✓  correct` GREEN / `✕  <answer>` RED /
  `…  noted — I'll go over this one with you` ACC2), and `NEXT  →` /
  `FINISH  →` button (110px, right-aligned).

### 2.7 Camera & video pages (replace the HUD in the same frame)

* **Camera:** bg `#000308`, header `◈  CAMERA FEED` (Courier 8 Bold `PRI`) +
  stretch + `✕  CLOSE` (Courier 8 Bold `TEXT_DIM`, no border, hover `PRI`),
  centred image below. A floating **camera preview** (244×188) sits bottom-right
  with its own tiny `CAMERA` title bar and 16×16 `✕`.
* **Video:** bg `BG`, header `▶  VIDEO` (Courier 9 Bold `PRI`) +
  `🔇  SOUND OFF` / `🔊  SOUND ON` + `✕  CLOSE` (borderless Courier 8 Bold
  `TEXT_DIM`, hover `PRI`), black video area below. Sound is always off first.

### 2.8 Drawers (floating, 220px wide, drop from under the header at x=12,
y=54; `DARK` fill, 1px `BORDER_B` border, radius `0 0 6 6`, padding 10/8;
only one open at a time)

**⚙ SETUP drawer** — header `◈ SETUP` (Courier 7 Bold `PRI_DIM`, bottom border):

| Button (h) | Label |
|---|---|
| 30 | `◉  REMOTE CONTROL` (primary style) |
| 26 | `⊞  CREATE DESKTOP SHORTCUT` |
| 26 | `◉  AUTO-START: OFF` / `◉  AUTO-START: ON` |
| 26 | `⚙  CUSTOMISE ASSISTANT` |
| 26 | `🎧  AUDIO DEVICES` |
| 26 | `🧠  MEMORY` |
| 26 | `🧩  PLUGINS` |
| 26 | `⚙  PLUGIN SETTINGS` |
| 26 | `🔌  ENGINE & PROVIDERS` |

**🎛 CONTROLS drawer** — header `◈ CONTROLS`, buttons Courier 7, left-aligned:

| Label | States |
|---|---|
| `⛶  FULLSCREEN  [F11]` | dim style |
| `☀  MORNING BRIEF: ON` / `: OFF` | ON = green fill style |
| `🎙  WAKE WORD: ON` / `: OFF` / `⬇  WAKE WORD: DOWNLOAD` / `⬇  DOWNLOADING… (one-time)` | ON green, OFF dim |
| `😴  SLEEP NOW` / `👂  WAKE NOW` | appears only when wake word on |
| `🎚  PUSH-TO-TALK: Ctrl+Space` / `: OFF` | ON `#001a08`/`GREEN`, OFF dim |
| `🧑  HUD: ANIMATED FACE` / `◉  HUD: REACTOR CORE` | choice, not switch: `PANEL2` fill, `PRI` text, `BORDER_A` border |

### 2.9 Keyboard shortcuts (show as hints in the UI)

`F4` mute · `F11` fullscreen · `Esc` interrupt · `Ctrl+Space` push-to-talk.

---

## 3. Overlay dialogs (desktop)

All centred over the HUD, Courier New, `rgba(0,6,10,0.96)` background,
1px `BORDER_B`, radius 6 unless noted. Title style: glyph + spaces + UPPERCASE,
Courier 12 Bold `PRI`, then a 1px `BORDER` divider.

### 3.1 Initialisation (first boot) — 460×390, radius 6

1. `◈  INITIALISATION REQUIRED` — Courier 13 Bold `PRI`, centred.
2. `Configure J.A.R.V.I.S. before first boot.` — Courier 9 `PRI_DIM`, centred.
3. Divider.
4. `GEMINI API KEY` caption (Courier 8 `TEXT_DIM`, left) → password field,
   32px, placeholder `AIza…` (empty submit turns its border `RED`).
5. Divider.
6. `OPERATING SYSTEM` caption → `Auto-detected: Windows` (Courier 8 `ACC2`) →
   three 32px buttons, Courier 9 Bold: `⊞  Windows` · `  macOS` · `🐧  Linux`.
   *Selected:* solid fill + dark text (Windows `#00d4ff` on `#001a22`,
   macOS `#ffcc00` on `#1a1400`, Linux `#00ff88` on `#001a0d`).
   *Unselected:* `#000d12` / `TEXT_DIM` / 1px `BORDER`, hover `TEXT`/`BORDER_B`.
7. Full-width 36px primary button `▸  INITIALISE SYSTEMS`.

### 3.2 Customise assistant — 400×~588

`⚙  CUSTOMISE ASSISTANT` (12 Bold) →

* `ASSISTANT NAME` → input (32px, value `JARVIS`)
* `YOUR NAME  (leave blank for default sir / efendim)` → input
  (placeholder `e.g.  Tony   (leave blank for auto)`)
* `ASSISTANT VOICE` → pill buttons 28px, Courier 8 Bold, radius 3:
  `Charon` `Puck` `Kore` `Fenrir` `Aoede` — selected `PRI_GHO`/`PRI`/`PRI`
  border, else transparent/`TEXT_MED`/`BORDER`.
* `UI COLOUR  —  drag the handle` + `DEFAULT` button (64×20, Courier 7 Bold)
* **Hue wheel** 148×148: 16px rainbow ring, white 7.5px handle with dark
  outline, centre preview circle filled with the picked hue, 1px `BORDER_B`
  outline.
* hex input (28px, placeholder `#00d4ff   (custom hex colour)`)
* Row: `▸  APPLY CHANGES` (34px, primary) + `CANCEL` (34px, secondary).

### 3.3 Plugin manager — width 420

`🧩  PLUGIN MANAGER` (12 Bold) → divider → rows: plugin name (Courier 8,
`TEXT`; broken ones `TEXT_DIM` + `  (⚠ file)`) + right-aligned toggle 72×24
(`ON`/`OFF`/`BROKEN`) → full-width 30px `CLOSE`. Empty state:
`No plugins found in /plugins.`

### 3.4 Confirm banner — width 430, **orange danger theme**

Background `rgba(14,3,0,0.98)`, border 1px `ACC`, radius 6:

* `⚠  CONFIRM` — Courier 11 Bold `ACC`
* Title (runtime, Courier 10 Bold `TEXT`) + optional detail (Courier 8
  `TEXT_MED`), wrapped.
* `▸  CONFIRM` (32px, orange outline, hover translucent orange fill) +
  `CANCEL` (32px, secondary — **default/focused button**).

### 3.5 Audio devices — width 460

`🎧  AUDIO DEVICES` (12 Bold) → two identical blocks:

* Caption `MICROPHONE — what JARVIS hears you with` / `SPEAKERS — what JARVIS
  talks through` (Courier 8 `TEXT_DIM`) → row: combo (flex, 30px, first item
  `System default`, plus `Device Name  (not connected)` entries) + `TEST`
  button 64×30 (dim outline, hover `PRI`).
* Under TEST: 6px progress meter (track `#000d12`/`BORDER`, fill `PRI`) + status
  line Courier 7: `Speak now…` / `Playing a tone…` / `peak 82  —  Good` (good →
  `PRI`).
* Then `OUTPUT VOLUME` → slider (4px groove `#000d12`, filled `PRI_DIM`, 10px
  round `PRI` handle) + `78%` value (Courier 8 `TEXT`).
* Note `Applying reconnects the session. Your conversation is kept.`
  (Courier 7 `TEXT_DIM`).
* `▸  APPLY` + `CLOSE` (32px each).

### 3.6 Memory — width 520

`🧠  WHAT JARVIS REMEMBERS` (12 Bold) → caption:
`{n} stored facts — newest first. Nothing here is sent anywhere; it lives in
memory/long_term.json on this machine.` (Courier 7) → scroll box (max 420px,
1px `BORDER`, radius 3) of rows: **bold key** `— value` (Courier 8 `TEXT`),
right meta `pref · 2026-11-02` (Courier 7 `TEXT_DIM`), 20×20 `✕` delete
(hover red). Empty: `Nothing stored yet.` → full-width 30px `CLOSE`.

### 3.7 Engine & Providers — width 760, radius 8

`🔌  ENGINE & PROVIDERS` (13 Bold) → sub-line:
`Choose what powers the conversation. Gemini is the default and needs nothing
set up here.` (Courier 8 `TEXT_DIM`)

**Two mode cards** side by side (min-height 84, radius 5, left-aligned,
Courier 7, padding 10/12):
* `☁  GEMINI LIVE` + body `One connection carries your voice, its voice, the
  transcript and the tools together. The fastest option — about half a second
  to the first word.`
* `🖥  MY OWN SETUP` + body `Your choice of hearing, thinking and speaking. Can
  run entirely on this computer, with nothing leaving it. Slower: 1.5 to 4
  seconds.`
* Selected: `PRI_GHO` fill, `TEXT` text, 1px `PRI_DIM`. Unselected: transparent,
  `TEXT_DIM`, 1px `BORDER`, hover border `BORDER_B`.

**Scroll body** (230–360px tall):

* *Gemini mode:* one box (`PANEL`, 1px `BORDER`, radius 5) with two lines:
  `Nothing to set up. Your voice goes to Gemini and comes back as speech over
  one connection…` and `Pick MY OWN SETUP above to run it on your own machine…`
* *Pipeline mode:* three slot boxes, each `①  HEARING / turns what you say into
  words`, `②  THINKING / understands, decides, uses your tools`,
  `③  SPEAKING / reads the answer out loud` (head Courier 9 Bold `PRI`, sub 7
  `TEXT_DIM`), then: service combo (29px, first item `— not set —`, items like
  `🖥  Whisper (local)   —  on this machine` / `☁  OpenAI   —  needs install`),
  field rows (caption Courier 7 `TEXT_DIM`, input 27px), button row
  (25px outline buttons Courier 7 Bold): `TEST` (`PRI`), `↻ REFRESH`
  (`TEXT_MED`), `⌃ LESS` / `⌄ MORE`, `INSTALL …` (`ACC2`), then a status line
  (Courier 7: checking `TEXT_DIM`, `✓` `GREEN`, `✕` `RED`).
* `⚡  BACKGROUND REQUESTS` box — explanatory paragraph (Courier 7 `TEXT_DIM`),
  service combo (`Gemini only  —  the way it has always been` first), order
  combo (`Try Gemini first  —  faster` / `Try the other one first  —  keeps
  more on your machine`).
* `🛠  CODING` box — paragraph about the project model (saved to
  Desktop/JarvisProjects), combo (`Same as background requests` first,
  `…  ★ recommended` on one entry).

**Footer:** status line (Courier 7 `TEXT_DIM`, e.g. `All three set. SAVE &
APPLY reconnects; the conversation is kept.`) + row: `▸  SAVE  &  APPLY`
(stretch 2, 34px, `PRI_GHO` fill, `PRI` text, 1px `PRI_DIM`, radius 4) +
`CLOSE` (stretch 1, 34px, secondary).

### 3.8 Clipboard toast — 326×112, docked **bottom-centre** (6px from bottom),
auto-hides after 8s

`rgba(0,8,14,0.97)`, 1px `BORDER_B`, radius 6, padding 8/6:

* Header: `◈  CLIPBOARD DETECTED` (Courier 7 Bold `ACC2`) + stretch + 16×16 `✕`.
* Preview strip (28px): `"copied text, first 58 chars…"` — Courier 8 `TEXT` on
  `PANEL2`, 1px `BORDER`, radius 3.
* Four 22px buttons, Courier 7 Bold, `PANEL2` fill, 1px `BORDER`, radius 2,
  `TEXT_MED`, hover `PRI`/`BORDER_B`: `TRANSLATE` `SUMMARISE` `EXPLAIN` `FIX`.

### 3.9 Plugin settings — width 460, max height 560

`⚙  PLUGIN SETTINGS` (12 Bold) → divider → scrollable form, per section:
title (Courier 10 Bold `PRI`) → field captions (Courier 8 `TEXT_DIM`,
UPPERCASE) → controls (combo 30px / `ON`–`OFF` toggle 28px / text input 30px /
action button 30px `bg #00091a` `PRI` outline, label `TEST`) → status line
(`Testing…` / `Saved ✓`) → divider. Footer: `▸  SAVE` + `CLOSE` (34px).
Empty state: `No configurable plugins are installed.`

### 3.10 Remote access — 400×465, **radius 14**, `rgba(0,4,12,0.95)`

`◈  REMOTE ACCESS` (12 Bold, centred) → divider → **white QR tile** 176×176
(QR image 170×170, radius 10, white bg) → `Scan with phone camera to connect
instantly` (Courier 8 `TEXT_DIM`) → divider → `Or enter manually:` (7) →
URL (Courier 8 `PRI_DIM`) → **pairing key tile**: 4-character key, Courier
**28 Bold**, `ACC` on `PANEL2`, 1px `BORDER_B`, radius 8, wide letter-spacing
→ `Key expires in  04:59` (Courier 8 `TEXT_MED`) → row: `NEW KEY` +
`DISMISS` (32px, radius 5).
Connected state: key tile becomes `CONNECTED` (green), QR tile becomes a big
`✓` on `#001a0d`, timer line becomes `Phone connected — JARVIS ready` (green).
Warning variants: `⚠  {bind error}` and a Windows firewall message in
`#f4b942`.

---

## 4. Phone dashboard (web, mobile-first, single column)

Two screens. Shared tokens:
`--bg #07090f` · `--surface rgba(255,255,255,0.04)` ·
`--border rgba(255,255,255,0.08)` · `--accent #6366f1` (indigo) ·
`--adim rgba(99,102,241,0.15)` · `--text #dde3ed` · `--muted #5e6a7e` ·
`--green #22c55e` · error `#f87171`. Font: system sans.

### 4.1 Login screen (`login.html`)

One centred card: **340px wide**, padding 44/36, `--surface` fill,
1px `--border`, radius **22**, text centred:

1. `JARVIS` — 28px/700, letter-spacing 10px; the **J** in `--accent`, rest
   `--text`.
2. `Remote Access` — 11px, `--muted`, uppercase, letter-spacing 3px.
3. Hint: `Press Remote Control in the JARVIS desktop app to get a QR code or
   session key.` (12px `--muted`, "Remote Control" bold `--text`).
4. **Key input:** full width, padding 18/14, `rgba(255,255,255,0.05)` fill,
   1.5px `--border`, radius 14, **Courier New 30px bold uppercase**,
   letter-spacing 10px, centred; placeholder `· · · · · ·`; focus → accent
   border + `--adim` fill + 3px glow ring. Auto-submits at 6 characters; wrong
   key **shakes** the card.
5. Error line 12px `#f87171`: `Invalid or expired key` / `Connection error`.
6. `CONNECT` button — full width, `--accent` fill, white, 12px/700,
   letter-spacing 3px, radius 12; hover dim, press scale .98.
7. Footnote: `🎤 Voice commands:` + 3 steps with inline `code` chips
   (Courier 9 on `rgba(255,255,255,0.07)`, radius 3).

### 4.2 Dashboard screen (`app.html`)

1. **Toast** (fixed, top 66, centred, pill radius 99): green-tinted
   (`rgba(34,197,94,0.16)` / border 0.4 / `--green` text, 12px bold,
   letter-spacing 1) — `Connected to JARVIS`, `File sent!`, `URL copied!`,
   `🎤 Live`. Auto-hide 2.8s.
2. **Header** (padding 12/16, bottom border, `rgba(7,9,15,0.95)`):
   * `JARVIS` logo 15px/700, letter-spacing 6, J in accent.
   * **Status pill** (radius 99, 10px/700 uppercase): `Connecting` (neutral) →
     `Active` (green dot + green tint) or `Sleeping` (grey).
   * **Encryption badge** (radius 4, 9px/700, padding 3/8): `AES-GCM`
     (green tint) or `NO ENC` (red tint `#f87171`) — always honest.
   * spacer, then address chip `192.168.1.108:8765` (10px monospace,
     `--surface`, radius 6).
3. **Message feed** (scroll, padding 14/16, gap 10):
   * JARVIS bubble — left, max-width 82%, padding 10/14, radius 14
     (bottom-left 4), `--adim` fill, 1px indigo-tint border, 14px/1.55, label
     `JARVIS` (9px/700 uppercase, accent, opacity .45).
   * User bubble — right, `--surface` fill, `--border`, label `YOU` (`--muted`).
   * System line — centred, 11px `--muted`: `Remote session active.`
     / `Voice live — speak now` / `Connection lost — refresh to reconnect.`
   * **File card** (right): surface card radius 14, emoji icon 26px, label
     `SENDING → SENT` / `RECEIVED` (9px bold uppercase), filename (13px/600),
     size (11px `--muted`), 3px progress bar (accent → green at 100%), status
     `42%` → `✓` (green), plus `⬇ Download` link (11px accent/600).
4. **Footer composer** (padding 10/14, top border): input (flex, padding
   11/14, 1.5px `--border`, radius 12, placeholder
   `Send a command to JARVIS…`, focus accent) + `📎` attach button + `🎤` mic
   button (recording state: red tint, glyph `⏹`, 1s pulse) + `WAKE` button
   (only while sleeping; green outline) + `SEND` (accent fill, white, 11px/700,
   radius 12).

---

## 5. States & motion to reproduce

* HUD repaints ~30fps; status symbol blinks every ~0.63s; waveform and core
  arcs react to a simulated audio level; face mode blinks and glances down at
  the content panel when it opens.
* Log lines **type in** at 6ms/character, 20ms pause between lines.
* Drop zone dashed border animates (dash offset marches).
* Metrics update every 2s; clock every 1s.
* Drawers slide/drop under the header; only one open at a time.
* Overlays centre themselves and re-centre on window resize.
* Buttons: instant hover, no easing longer than 120ms. No drop shadows, no
  glass blur — this is an instrument panel, not a material design app.

## 6. What NOT to do

* No rounded-full pills except the phone dashboard's status/toast chips.
* No non-monospace fonts on the desktop HUD.
* No white/light theme — the app is dark by design (the accent hue may shift,
  the darkness never does).
* Status colours (`#ff6b00 #ffcc00 #00ff88 #ff3355 #ff3366`) never hue-shift.
