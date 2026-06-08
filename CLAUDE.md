# CLAUDE.md — Testat English Vocabulary

## Project Overview

A single-file SPA (`Testat-English Vocabulary.html`) built with Vanilla HTML/CSS/JS — no frameworks.
Final users are **non-technical English teachers** who need Plug-and-Play lesson management.
Students receive the file as an interactive vocabulary sheet at the end of each lesson.

---

## Collaboration Rules (Strictly Enforced)

1. **Discuss before coding** — explain the engineering plan and wait for "okay" before writing any code.
2. **Engineering partnership** — propose better approaches; the user makes final decisions.
3. **No empty praise** — only compliment genuinely good decisions; be direct about technical problems.
4. **Surgical edits only** — NEVER print the full HTML file. All changes = find/replace blocks.
5. **Comments in simple English** — all inline code comments must be in clear, easy English.
6. **Clean Code rules** — see section below.
7. **Egyptian Arabic dialect** — all conversation with the user.

---

## File Structure (Single File)

```
Testat-English Vocabulary.html
│
├── <head>                          [Lines ~1–13]
│   └── Google Fonts: Tajawal
│
├── <style>                         [Lines ~14–2257]
│   ├── CSS Dashboard (:root)       — all design tokens (colors, sizes, radii)
│   ├── Header                      — .main-header (sticky, glassmorphism)
│   ├── App Container               — .app-container (max-width: 1000px)
│   ├── Sticky Toolbar              — .sticky-toolbar-container (speed + stop + search)
│   ├── Section Cards               — .section-card, .section-header
│   ├── Vocabulary Grid             — .vocab-grid (2-col) + .vocab-row
│   ├── Definition Cards            — .vocab-grid.single-column + .def-row-wrapper
│   ├── Synonyms & Antonyms         — .syn-ant-container, .syn-box, .ant-box
│   ├── Verbs Section               — .verb-card, .verb-box (.v1/.v2/.v3)
│   ├── CTA Section                 — .cta-section (stars, orbit, phone mockup, store btns)
│   ├── Footer                      — .main-footer, .footer-inner
│   └── @media (max-width: 768px)   — full mobile overrides
│
├── <body>                          [Lines ~2260–2590]
│   ├── SVG Icon Library            — hidden <svg> with <symbol> elements (see below)
│   ├── <header class="main-header">
│   │   ├── Logo (Base64 PNG)       — cloned to footer via JS, never duplicated in HTML
│   │   └── Header marketing text
│   ├── <main class="app-container">
│   │   ├── .lesson-info-wrapper    — Unit card + Lesson card (edit data in JS)
│   │   ├── .sticky-toolbar-container — speed buttons + stop + search
│   │   └── #vocabulary-container  — EMPTY div, fully injected by renderLesson()
│   ├── <section class="cta-section">
│   │   ├── #cta-stars              — filled by initCTAStars()
│   │   ├── .cta-glow-right/left    — decorative radial glows
│   │   ├── .cta-text-col           — headline, sub, stats, store buttons, website CTA
│   │   └── .cta-phone-col          — animated phone mockup + social orbs
│   └── <footer class="main-footer">
│       ├── .footer-top             — brand + social links
│       └── .footer-bottom          — footer links + copyright
│
└── <script>                        [Lines ~2590–3358]
    ├── State: currentSpeechRate    — default 1.0
    ├── lessonData[]                — JSON array of sections (THE ONLY PLACE TO EDIT CONTENT)
    ├── Core functions
    └── DOMContentLoaded initializer
```

---

## SVG Icon Library (in `<body>`)

All icons live in the hidden `<svg>` block at the top of `<body>`.
**Never** inline raw SVG paths inside elements — always use `<use href="#icon-name">`.

| Symbol ID         | Usage                            |
|-------------------|----------------------------------|
| `#icon-speaker`   | Audio play buttons               |
| `#icon-search`    | Search box icon                  |
| `#icon-play`      | Play-all button                  |
| `#icon-stop`      | Stop button                      |
| `#icon-facebook`  | Social links (CTA + Footer)      |
| `#icon-youtube`   | Social links                     |
| `#icon-instagram` | Social links                     |
| `#icon-huawei`    | Huawei AppGallery store button   |
| `#icon-x-twitter` | Social links                     |
| `#icon-tiktok`    | Social links                     |
| `#icon-whatsapp`  | Social links                     |

---

## Data Structure — `lessonData[]`

This is the **only place** a teacher needs to edit to update lesson content.
Each object in the array = one section card, rendered by `renderLesson()`.

### Section Types (auto-detected by field presence)

```js
// Type 1: Standard Vocabulary (2-column grid)
{ en: "background", ar: "خلفية - أصل" }

// Type 2: Words & Definitions (single-column, def box below)
{ en: "background", ar: "خلفية", enDef: "where someone comes from", arDef: "الأصل الذي نشأ منه شخص ما" }

// Type 3: Synonyms & Antonyms (single-column, split green/red boxes)
{ en: "simple", ar: "بسيط", synEn: "easy", synAr: "سهل", antEn: "complex", antAr: "معقد" }

// Type 4: Verb Conjugation (single-column, 3-box flow: v1/v2/v3)
{ v1: "grow", v2: "grew", v3: "grown", ar: "ينمو" }
```

**Detection logic in `renderLesson()`:**
- Has `enDef` → Definition layout
- Has `synEn` → Synonyms & Antonyms layout
- Has `v1` → Verb layout
- Otherwise → Standard 2-column layout

---

## Key JavaScript Functions

| Function | Purpose |
|---|---|
| `renderLesson()` | Builds all section cards from `lessonData` and injects into `#vocabulary-container` |
| `filterWords()` | Live search — hides rows/cards that don't match, hides empty section cards |
| `speakWord(text, rowId)` | Plays a single word; highlights its row |
| `speakSubBox(text, boxId, class)` | Plays definition / synonym / antonym text; highlights the sub-box |
| `speakVerbSequence(v1, v2, v3, cardId)` | Plays all 3 verb forms in sequence |
| `playSection(sectionIndex, btnEl)` | Plays all visible words in a section sequentially |
| `globalStop()` | Cancels all speech, clears all highlights, resets play buttons |
| `clearAllHighlights()` | Removes all active highlight classes from every element |
| `setSpeed(rate)` | Updates `currentSpeechRate` and toggles active speed button |
| `initCTAStars()` | Generates random star particles for the CTA section background |
| `initWebsiteButtonEffects()` | Adds particle burst on website CTA button click |
| `cloneHeaderLogoToFooter()` | Clones the header `<img>` into `#footer-logo-link` — avoids Base64 duplication |

---

## CSS Design Tokens (`:root`)

All visual values are controlled from the CSS Dashboard at the top of `<style>`.
**Edit tokens here instead of hunting through the CSS.**

```css
/* Key tokens */
--brand-purple: #5c007a;
--brand-dark:   #38014e;
--brand-orange: #ff6600;
--bg-color:     #e5dfea;
--main-font:    'DIN Next LT Arabic', 'Tajawal', sans-serif;

/* Size tokens */
--word-en-size, --word-ar-size
--def-word-size, --def-trans-size, --def-en-size, --def-ar-size
--syn-ant-label-size, --syn-ant-en-size, --syn-ant-ar-size
--audio-btn-size, --audio-icon-size
--radius-small, --radius-medium, --radius-large
```

---

## Clean Code Rules

- **Logo image**: The Base64 PNG lives only in the `<header>` `<img>` tag. `cloneHeaderLogoToFooter()` copies it to the footer at runtime — never paste Base64 twice in HTML.
- **SVG icons**: Always add new icons as `<symbol>` in the library block and reference via `<use href="#icon-name">`.
- **Layout detection**: Section layout type is detected automatically by field presence — no `type` flag needed.
- **Highlight cleanup**: Always call `clearAllHighlights()` before starting any new audio.
- **LTR/RTL**: Arabic text always has `direction: rtl` and `text-align: right`. English is LTR default. Never mix them.
- **Responsive**: Mobile overrides live inside `@media (max-width: 768px)`. Extra breakpoints at 480px and 380px for footer link visibility.

---

## How to Add a New Lesson

Only two things change per lesson:

1. **Lesson info cards** (HTML, ~line 2324):
   ```html
   <span class="label">Unit 2:</span>
   <span class="title">New Unit Title</span>
   ```

2. **`lessonData[]`** (JS, ~line 2598): Replace/add sections with new vocabulary objects.

Everything else (rendering, audio, search, layout) works automatically.
