# Product Requirements Document
## Resume Editor — Browser-Native Rich Text Editor
**Codename:** Antigravity | **Version:** 1.0.0 | **Status:** Draft | **Date:** March 2026

---

## Table of Contents

1. [Product Overview](#1-product-overview)
2. [Goals & Success Metrics](#2-goals--success-metrics)
3. [User Personas & Use Cases](#3-user-personas--use-cases)
4. [Feature Specifications](#4-feature-specifications)
5. [Technical Architecture](#5-technical-architecture)
6. [UX & Design Specifications](#6-ux--design-specifications)
7. [Constraints & Risks](#7-constraints--risks)
8. [Roadmap & Milestones](#8-roadmap--milestones)
9. [Open Questions](#9-open-questions)
10. [Approval & Sign-Off](#10-approval--sign-off)

---

## 1. Product Overview

### 1.1 Executive Summary

The Resume Editor is a **zero-dependency, browser-native rich text editor** purpose-built for resume and CV creation. It delivers a Microsoft Word-like authoring experience — bold, italic, underline, font selection, bullet lists, spacing, text sizing — entirely within a single HTML file.

No server. No backend. No database. No installation. Open the file, start writing.

### 1.2 Problem Statement

Job seekers face a fragmented resume creation experience:

- Heavy tools like Microsoft Word or Google Docs require accounts, internet, or paid subscriptions
- Online resume builders lock users into proprietary formats and charge for PDF export
- Plain-text editors lack formatting capability
- Template-based tools enforce rigid structure with no freeform editing

> **There is no lightweight, open, browser-native tool that gives users full formatting freedom, works offline, and exports clean PDFs — without any infrastructure cost on either side.**

### 1.3 Proposed Solution

A single-file HTML application (~600–1000 lines) that:

- Runs entirely in the browser — open the `.html` file, start typing
- Provides a sticky toolbar with all essential text formatting controls
- Auto-saves content to `localStorage` on every keystroke
- Exports a printer-ready PDF via `html2pdf.js` (CDN-loaded, no server)
- Can be hosted free on GitHub Pages, Netlify, or Vercel — or run completely offline

### 1.4 Document Metadata

| Field | Value |
|---|---|
| Product Name | Resume Editor |
| Codename | Antigravity |
| Version | 1.0.0 |
| Date | March 2026 |
| Author | Product Team |
| Stakeholders | Engineering, Design, QA |
| Status | Draft — For Review |
| Next Review | Q2 2026 |

---

## 2. Goals & Success Metrics

### 2.1 Product Goals

| ID | Goal | Description |
|---|---|---|
| G1 | Zero infrastructure | No server, no database. All state managed client-side via localStorage. |
| G2 | Word-like editing | Bold, italic, underline, strikethrough, font family, font size, text colour, alignment, bullet/numbered lists, line spacing. |
| G3 | Instant persistence | Auto-save to localStorage on every input event. User never loses work on accidental tab close. |
| G4 | Clean PDF export | One-click export to a print-ready PDF. No UI chrome, no toolbar artefacts. |
| G5 | Zero install, zero login | Open `.html` file in any browser. No account. No email. No subscription. |
| G6 | Accessible & responsive | Usable on desktop and tablet. Keyboard-accessible toolbar. WCAG 2.1 AA colour contrast. |

### 2.2 Success Metrics (KPIs)

- **Load time:** Time-to-first-character < 2 seconds on cold page load
- **PDF fidelity:** Exported PDF visually matches editor canvas (screenshot diff < 5% delta)
- **Data integrity:** 100% of content recoverable from localStorage after browser crash in manual QA
- **Feature coverage:** All 15 core formatting operations executable via toolbar (mouse-only path)
- **Cross-browser:** Identical rendering in Chrome 120+, Firefox 122+, Safari 17+, Edge 120+

### 2.3 Non-Goals (Out of Scope for v1.0)

- Real-time collaboration (no WebSockets, no shared state)
- Cloud save / sync (no user accounts, no backend API)
- Template library or resume wizards
- AI-assisted content suggestions *(planned v2.0)*
- Native mobile app (iOS/Android)
- DOCX or RTF import/export
- Spell-check / grammar engine (deferred to browser native)

---

## 3. User Personas & Use Cases

### 3.1 Personas

#### Persona A — The Active Job Seeker
| | |
|---|---|
| **Name** | Riya, 26 |
| **Context** | Software engineer actively interviewing, updates resume weekly |
| **Pain Point** | Tired of Google Docs requiring internet; wants a fast local tool |
| **Needs** | Quick formatting, PDF export, auto-save |
| **Success** | Produces a formatted, exported resume in under 10 minutes |

#### Persona B — The Career Changer
| | |
|---|---|
| **Name** | Marcus, 38 |
| **Context** | Transitioning from finance to product management, limited tech skills |
| **Pain Point** | MS Word is complex; online builders are rigid |
| **Needs** | Simple toolbar, obvious controls, no learning curve |
| **Success** | Formats resume confidently without reading documentation |

#### Persona C — The Developer / Power User
| | |
|---|---|
| **Name** | Karan, 31 |
| **Context** | Developer who wants to self-host or customise the editor |
| **Pain Point** | All resume tools are black boxes; can't modify layout or style |
| **Needs** | Single HTML file, clean code, easy to fork and extend |
| **Success** | Deploys a custom-branded version in < 30 minutes from source |

### 3.2 User Stories

| ID | As a... | I want to... | So that... |
|---|---|---|---|
| US-01 | Job seeker | Bold and italicise text with one click | I can emphasise key achievements quickly |
| US-02 | User | Have my draft auto-saved every few seconds | I never lose work if I accidentally close the tab |
| US-03 | User | Change the font family and size | I can match the typographic style expected in my industry |
| US-04 | User | Add bullet and numbered lists | I can structure experience and skills sections clearly |
| US-05 | User | Export my resume to PDF with one click | I can attach it to job applications immediately |
| US-06 | Power user | Clear all formatting with one button | I can start fresh without side effects |
| US-07 | Developer | Have all functionality in one `.html` file with commented code | I can fork and customise easily |

---

## 4. Feature Specifications

### 4.1 Feature Priority Matrix

> **Priority levels:** `P0` = must-have (launch blocker) · `P1` = should-have (launch target) · `P2` = nice-to-have (post-launch)

| Feature | Description | Priority |
|---|---|:---:|
| `contenteditable` Canvas | Full-page freeform editing area. Accepts keyboard input, copy-paste, drag-and-drop. Simulates a Word document canvas. | P0 |
| Bold / Italic / Underline | One-click toolbar buttons. Keyboard shortcuts `Ctrl+B`, `Ctrl+I`, `Ctrl+U`. Visual active-state indicator. | P0 |
| Strikethrough | Apply/remove strikethrough on selected text. | P0 |
| Font Family Selector | Dropdown with 8–12 web-safe + Google Fonts options (Serif, Sans-Serif, Mono, professional resume fonts). | P0 |
| Font Size Selector | Numeric input (8–72pt) or dropdown with common sizes. Up/down arrows to increment. | P0 |
| Text Colour Picker | Native `<input type=color>` in toolbar. Applies colour to selected text. | P0 |
| Text Alignment | Left, Centre, Right, Justify. Radio-group behaviour — only one active at a time. | P0 |
| Bullet List | Toggle unordered list. Supports nesting via `Tab` / `Shift+Tab`. | P0 |
| Numbered List | Toggle ordered list. Continues numbering correctly after list breaks. | P0 |
| Line Spacing | Dropdown: 1.0, 1.15, 1.5, 2.0. Applied to selected paragraphs. | P0 |
| Undo / Redo | `Ctrl+Z` / `Ctrl+Y` native browser undo stack. Toolbar buttons as visual affordance. | P0 |
| Auto-Save (localStorage) | Debounced save on every input event (300ms delay). Saves full HTML content of canvas. | P0 |
| PDF Export | `html2pdf.js` integration. Exports canvas to A4/Letter PDF. Strips toolbar from output. | P0 |
| Clear Formatting | Remove all inline formatting from selected text (font, colour, bold, italic, etc.). | P1 |
| Word & Character Count | Live count in status bar. Updates on every input event. | P1 |
| Section Divider / HR | Insert a horizontal rule to visually separate resume sections. | P1 |
| Indentation Controls | Increase / decrease paragraph indent. Works inside and outside lists. | P1 |
| Page Ruler / Margins | Visual ruler showing approximate page width at standard margin stops. | P1 |
| Dark Mode Toggle | Switch editor chrome between light (paper) and dark (night) themes. Canvas stays light for print accuracy. | P2 |
| Template Presets | 2–3 built-in layout templates (Minimal, Classic, Modern) pre-populated with dummy content. | P2 |
| Highlight Colour | Apply background highlight to selected text (yellow, green, pink, none). | P2 |

### 4.2 Toolbar Layout Specification

The toolbar is a **single sticky bar fixed to the top of the viewport**, structured in logical groups separated by visual dividers (`|`):

```
[ Logo ] | [ Font Family ▾ ] [ Size ⬆⬇ ] | [ B ] [ I ] [ U ] [ S ] | [ 🎨 ] | [ ≡◀ ≡ ≡▶ ≡≡ ] | [ ≔ 🔢 ⇥ ⇤ ] | [ Spacing ▾ ] | [ ✕ fmt ] [ ↩ ] [ ↪ ] | word count |                        [ Export PDF ]
```

| Group | Controls |
|---|---|
| Identity | Product logo / wordmark (non-interactive) |
| Font | Family dropdown · Size input · Size ↑↓ |
| Style | Bold · Italic · Underline · Strikethrough |
| Colour | Text colour picker |
| Alignment | Left · Centre · Right · Justify |
| Lists | Bullet · Numbered · Indent+ · Indent− |
| Spacing | Line spacing dropdown |
| Edit | Clear formatting · Undo · Redo |
| Actions | Word count (status) · **Export PDF** (right-aligned, accent colour) |

---

## 5. Technical Architecture

### 5.1 Technology Stack

| Layer | Choice | Rationale |
|---|---|---|
| Runtime | Browser (no Node.js) | Zero server cost; works offline |
| Language | Vanilla JS (ES2020+) | No framework dependency; single file |
| Markup | Single HTML5 file | All CSS in `<style>`, all JS in `<script>` |
| Editing Core | `contenteditable` + `document.execCommand()` | Native browser editing; no library needed |
| PDF Export | `html2pdf.js` v0.10+ via CDN | Zero npm at runtime; browser-side only |
| Persistence | `window.localStorage` | ~5MB limit; sufficient for any resume |
| Fonts | Google Fonts via CDN | Falls back to system fonts offline |
| Hosting | GitHub Pages / Netlify / Vercel / `file://` | All free; no backend needed |
| Build Pipeline | None required | Users open `.html` directly |

### 5.2 Editing Engine

#### 5.2.1 `contenteditable` Canvas

```html
<div id="canvas" contenteditable="true" spellcheck="true">
  Start writing your resume here...
</div>
```

Styled as an A4 page:
```css
#canvas {
  max-width: 820px;
  min-height: 100vh;
  padding: 60px;
  background: #f9f7f2;
  margin: 0 auto;
  box-shadow: 0 8px 40px rgba(0,0,0,0.18);
}
```

This gives us **for free from the browser:**
- Native cursor, selection, caret positioning
- Copy-paste with formatting preserved
- Drag-and-drop text
- Touch support on tablet
- Full `Selection` API access for pre-formatting context detection

#### 5.2.2 `document.execCommand()` Formatting Layer

| Command | Triggered By |
|---|---|
| `bold` | Bold button / `Ctrl+B` |
| `italic` | Italic button / `Ctrl+I` |
| `underline` | Underline button / `Ctrl+U` |
| `strikeThrough` | Strikethrough button |
| `fontName` | Font family dropdown |
| `fontSize` | Size input *(see workaround below)* |
| `foreColor` | Colour picker |
| `justifyLeft/Center/Right/Full` | Alignment buttons |
| `insertUnorderedList` | Bullet list button |
| `insertOrderedList` | Numbered list button |
| `indent` / `outdent` | Indent +/− buttons |
| `removeFormat` | Clear formatting button |

> **Note on deprecation:** `execCommand` is formally deprecated in the WHATWG spec but remains universally supported across Chrome, Firefox, Safari, and Edge with no announced removal timeline. An abstraction layer is prepared for future migration to the Input Events Level 2 API.

#### 5.2.3 Font Size Workaround

`execCommand('fontSize')` only accepts values 1–7 (mapped to fixed px sizes). To support arbitrary pt values:

```javascript
function setFontSize(ptValue) {
  // Step 1: Wrap selection in <font size="7"> via execCommand
  document.execCommand('fontSize', false, '7');
  // Step 2: Query all font[size="7"] elements in the canvas
  const fontEls = canvas.querySelectorAll('font[size="7"]');
  // Step 3: Replace size attribute with inline CSS font-size
  fontEls.forEach(el => {
    el.removeAttribute('size');
    el.style.fontSize = `${ptValue}pt`;
  });
}
```

### 5.3 Persistence Architecture

```javascript
// Save — debounced 300ms after last input
const save = debounce(() => {
  localStorage.setItem('resumeEditor_content', canvas.innerHTML);
  showStatus('Saved ✓');
}, 300);

canvas.addEventListener('input', save);

// Load — on DOMContentLoaded
window.addEventListener('DOMContentLoaded', () => {
  const saved = localStorage.getItem('resumeEditor_content');
  if (saved) canvas.innerHTML = saved;
});
```

- **Key:** `resumeEditor_content`
- **Value:** `innerHTML` of canvas div
- **Storage budget:** ~5MB per origin; a rich resume rarely exceeds 50KB
- **Conflict handling:** Last-write-wins (single-tab, single-user context)
- **beforeunload guard:** Warn user if unsaved changes exist at tab close

### 5.4 PDF Export Architecture

```javascript
function exportPDF() {
  const clone = canvas.cloneNode(true);
  // Strip any toolbar artefacts from clone
  clone.querySelectorAll('.no-print').forEach(el => el.remove());

  const options = {
    margin: [15, 15, 15, 15],       // mm
    filename: 'my-resume.pdf',
    image: { type: 'jpeg', quality: 0.98 },
    html2canvas: { scale: 2, useCORS: true },
    jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
  };

  html2pdf().set(options).from(clone).save();
}
```

**Export pipeline:**
1. Clone editor canvas DOM node
2. Strip all `.no-print` elements (toolbar, status bar)
3. Apply print-specific CSS overrides (white background, standard margins)
4. `html2canvas` rasterises DOM → canvas element
5. `jsPDF` converts canvas → PDF blob
6. Browser triggers file download

### 5.5 Hosting Options

| Platform | Cost | Deployment | Notes |
|---|---|---|---|
| GitHub Pages | Free | `git push` to `gh-pages` branch | Custom domain supported |
| Netlify | Free tier | Drag & drop `.html` file | Instant HTTPS, edge CDN |
| Vercel | Free tier | CLI or Git integration | Analytics available |
| Local `file://` | Free / offline | No deployment needed | Open `.html` directly in browser |

---

## 6. UX & Design Specifications

### 6.1 Design Tokens

| Token | Value | Usage |
|---|---|---|
| `--dark` | `#1A1A2E` | Toolbar background |
| `--accent` | `#C0392B` | CTA buttons, active states, borders |
| `--paper` | `#F9F7F2` | Editor canvas background |
| `--ink` | `#333333` | Body text on canvas |
| `--muted` | `#CCCCCC` | Toolbar icon default colour |
| `--font-ui` | `DM Sans` | All toolbar controls |
| `--font-canvas` | `Georgia` | Default canvas body text |
| `--radius` | `5px` | Toolbar buttons and dropdowns |
| `--shadow` | `0 8px 40px rgba(0,0,0,0.18)` | Canvas page shadow |

### 6.2 Toolbar UX Rules

- Buttons show **active state** (accent background) when formatting is active at cursor position
- Toolbar reflows to two rows on viewports < 900px. No horizontal scroll.
- All buttons have `title` attribute for native tooltip (e.g., `title="Bold (Ctrl+B)"`)
- Dropdowns reflect current formatting context when cursor moves through text
- Export PDF button is visually distinct: wider, accent-coloured, right-aligned
- Dividers between groups use a 1px vertical rule at 40% opacity

### 6.3 Canvas UX Rules

- **Max width:** 820px, centred — resembles an A4 sheet floating on a dark desktop
- **Min height:** 100vh — canvas grows vertically with content, never clips
- **Placeholder:** `"Start writing your resume here..."` in `#999`, disappears on first keystroke
- **Focus ring:** 2px accent-colour outline on container focus
- **Paste behaviour:** Preserves rich formatting (native). Word/Google Docs paste supported.
- **Print styles:** `@media print` hides toolbar, renders canvas full-width on paper

### 6.4 Status Bar

A thin bar below the canvas showing:

```
Words: 342   Characters: 1,847   |   Saved ✓   March 5, 2026 14:32
```

Updates in real time on every input event.

### 6.5 Accessibility Requirements

- All toolbar buttons keyboard-navigable via `Tab` / `Enter`
- WCAG 2.1 AA contrast ratio on all toolbar text and icons (4.5:1 minimum)
- `aria-label` on all icon-only buttons
- `aria-pressed` state on toggle buttons (bold, italic, etc.)
- Focus management returns to canvas after toolbar interaction

---

## 7. Constraints & Risks

### 7.1 Technical Constraints

| Constraint | Detail |
|---|---|
| `execCommand` deprecation | Formally deprecated in WHATWG spec; no browser removal announced. Abstraction layer prepared for Input Events L2 migration. |
| `localStorage` sync writes | Blocks main thread on writes. Debounce mitigates perceived lag for documents < 500KB HTML. |
| `html2pdf.js` rasterisation | Complex CSS (gradients, blend modes) may not render accurately. Canvas styles kept print-friendly by default. |
| Google Fonts CDN | Requires internet on first load. Falls back to system serif/sans-serif. Critical subset can be inlined for offline builds. |
| `file://` protocol | Some browser APIs (Clipboard API) require `https://` origin. All core features work on `file://`. |

### 7.2 Risk Register

| Risk | Likelihood | Impact | Mitigation |
|---|:---:|:---:|---|
| User loses work (accidental tab close) | Medium | High | Auto-save debounced on input + `beforeunload` warning if unsaved |
| PDF export visual mismatch | Medium | High | Cross-browser QA matrix + `@media print` CSS fallback |
| `execCommand` removed by browser | Low | Critical | Abstraction layer; monitor browser release notes; Input Events L2 ready |
| `localStorage` quota exceeded | Low | Medium | Monitor usage; alert user at > 4MB; offer "Download backup as .html" |
| Poor mobile usability | High | Medium | v1 targets desktop only; mobile viewport < 600px shows warning banner |
| CDN unavailability (html2pdf.js) | Low | Medium | Document offline fallback; consider bundling in future build |

---

## 8. Roadmap & Milestones

### 8.1 Release Plan

| Phase | Timeline | Deliverables |
|---|---|---|
| **v0.1** | Week 1–2 | HTML scaffold, canvas, basic bold/italic/underline, `localStorage` auto-save |
| **v0.5** | Week 3–4 | Full toolbar (font, size, colour, alignment, lists), PDF export, word count |
| **v1.0** | Week 5–6 | Polish: keyboard shortcuts, active states, status bar, cross-browser QA, README |
| **v1.1** | Month 2 | Dark mode, section dividers, template presets, performance optimisation |
| **v2.0** | Q3 2026 | AI-assisted content (Claude API), DOCX export, optional cloud save |

### 8.2 v2.0 Vision — Antigravity (AI-Powered Layer)

In v2.0, the editor integrates **Claude claude-opus-4-6** via the Anthropic API to provide intelligent resume co-piloting. The AI layer lives in a **toggleable side panel** — non-intrusive, secondary to the user's own writing.

| AI Feature | Description |
|---|---|
| **Smart Rewrite** | Select any bullet point → Claude rewrites with stronger action verbs and quantified impact |
| **ATS Optimisation** | Paste a job description → Claude highlights keyword gaps and suggests additions |
| **Tone Calibrator** | Switch between formal, conversational, and executive tones across the document |
| **Section Generator** | Type a job title + company → Claude drafts a first-pass experience section |
| **Achievements Extractor** | Paste a performance review → Claude converts it into resume bullet points |
| **Length Optimizer** | Condense or expand content to fit exactly one or two pages |

> The editor canvas remains the primary surface. Claude is the co-pilot, not the pilot.

---

## 9. Open Questions

| # | Question | Options | Owner | Due |
|---|---|---|---|---|
| Q1 | Font size: `execCommand` or direct CSS injection? | execCommand (simpler) vs. Selection API + CSS injection (more control) | Engineering | v0.1 |
| Q2 | Support paste-from-Word HTML cleanup? | Raw paste (default) vs. sanitise via DOMParser on paste | Product | v0.5 |
| Q3 | Minimum browser version target? | Chrome 100+, Safari 15+ (narrow) vs. broader legacy support | Engineering | v0.1 |
| Q4 | Default export format: A4 or Letter? | A4 (global default) vs. Letter (US) vs. user preference toggle | Design | v0.5 |
| Q5 | Include a starter template in v1.0? | No template vs. 1 minimal pre-filled starter | Product | v1.0 |

---

## 10. Approval & Sign-Off

| Role | Name | Signature | Date |
|---|---|---|---|
| Product Owner | | | |
| Engineering Lead | | | |
| Design Lead | | | |
| QA Lead | | | |

---

*Resume Editor — PRD v1.0.0 — Confidential*
*Last updated: March 2026*
