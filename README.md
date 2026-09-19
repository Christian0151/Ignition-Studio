# Open Ignition Studio

A modern, browser-based firing-plan editor for the **RaspEasyFire** firing system – replacing outdated vendor software with a portable, offline-capable single-file web app.

![status](https://img.shields.io/badge/status-developer%20preview-orange)
![version](https://img.shields.io/badge/version-0.38-blue)
![license](https://img.shields.io/badge/license-Unlicense-green)

---

## Why?

The software shipped with common firing systems (especially RaspEasyFire) feels dated – both visually and in day-to-day handling. **Open Ignition Studio** was written to provide:

- a **modern, clean UI** (dark "studio" theme, Canvas-based timeline),
- **more intuitive workflows** (drag & drop, context menus, live feedback),
- a **portable setup** – one HTML file, runs offline, works on tablets too,
- **proven logic preserved 1:1** from the original hardware rules (validation, time formats, export formats) – nothing reinvented where it didn't need to be.

No build step, no server, no installation.

---

## Features

### Timeline & Cue Management
- Canvas-based timeline with **virtualized rendering** – smooth even for long tracks and heavy zoom (up to 2000 px/s)
- Waveform display for loaded audio (Web Audio API)
- **Drag & drop** cues in time and lane, marquee multi-select, pan by dragging the waveform
- Playhead auto-scroll during playback
- Live traffic-light status per cue (green → red → gray around ignition time)

### Editors & Dialogs
- **Cue editor** (slave, channel, times, position, angle, price, function tag)
- **Sequence generator** for consecutive channels (stepper)
- **Flamer dialog** with automatic trigger-sequence calculation (350 ms pulses, 100 ms minimum spacing)
- **Label format picker** (Avery 5160, Avery 3475, Dymo)
- **Module overview** with personal default template

### Auto-Cue (port of `auto_patch.py`)
- Position-based slave/channel assignment
- Subset detection for slave reuse
- Post-optimization pass, conflict flagging for cues without position or over hardware capacity

### Import & Export
- **ZPL** (real RaspEasyFire format, Windows-1252 encoded) – import and export, including automatic flamer expansion to multi-triggers
- **JSON** as native working format (with File System Access API in-place saving on Chrome/Edge)
- **PDF reports** via the browser print dialog (full script, position lists, module requirements)
- **Label printing** via the browser print dialog
- Music and map image files are exported as companion files alongside the show file

### Field Map
- Load your own aerial image, place positions as markers
- Marker names **are the same position tokens** as the `pos` field on cues – no second source of truth
- Click marker → filter table; double-click → rename (including update of all affected cues)

### Convenience
- DE/EN toggle
- Undo/Redo (hooked into `autosave()`, so every change site gets it for free)
- Browser autosave (localStorage) with restore after accidental tab close
- Full keyboard control (`Space` = play/pause, `C` = cue at playhead, `Ctrl+S/E/A/Z/Y`)

---

## Technical Notes

| Area | Implementation |
|---|---|
| Architecture | Single-file HTML, no build, no external libraries |
| Graphics | Pure Canvas 2D with virtualization (spacer div + docked canvas) |
| Audio | Web Audio API, custom waveform peak computation tied to track length (500 buckets/s) |
| Persistence | localStorage for autosave, File System Access API where available, otherwise classic download |
| i18n | Central `translations{de,en}` object with `tr()` helper and `[data-tr]` attributes |
| Ports | Numerous comments reference the original Python sources (`main_window.py`, `timeline_widget.py`, `dialogs.py`, `auto_patch.py`, `pdf_export.py`) |

**Intentional deviations from the original** are explicitly marked in the code – e.g. the added 100 ms interval lock in the stepper dialog (missing in the original), or the Windows-1252 encoding fix during ZPL import.

---

## Supported Firing System

### RaspEasyFire
- 16/32 channels per module
- 100 ms radio minimum spacing enforced
- Flamer objects expanded to multi-triggers on export

---

## Roadmap / Not Yet Implemented

- **Cobra** – Currently **not implemented**. Some groundwork exists in the code (CSV export/import parser, separate constraint checks, hardware model for 18-cue banks), but it is **unverified and untested against real hardware**. Treat all Cobra-related code paths as experimental scaffolding, not as a working feature. If you're using Cobra hardware and want to help bring this to a real, verified state, feedback and test reports are very welcome.

---

## Getting Started

1. Download `ignition_studio.html`
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari – desktop or tablet)
3. That's it. No install, no network, no dependencies.

Load the built-in sample via **Beispiel laden / Load sample** to explore the UI, or go straight to **⚙ Module** to set up your hardware.

---

## Status

**v0.38 – Developer Preview** by Christian Röhrle.

The RaspEasyFire feature set is already broad (full import/export, all editors, field map, undo/redo). As a developer preview it's not yet a finished release – real-hardware verification of some export details (e.g. flamer multi-trigger sequences) is still pending.

---

## Feedback & Contact

- **Developer:** Christian Röhrle
- **Email:** christian.roehrle@gmail.com

Bug reports, hardware verification results, and pull requests are welcome.

---

## License

This is free and unencumbered software released into the **public domain**.

Anyone is free to copy, modify, publish, use, compile, sell, or distribute this software, either in source code form or as a compiled binary, for any purpose, commercial or non-commercial, and by any means.

In jurisdictions that recognize copyright laws, the author has dedicated any and all copyright interest in the software to the public domain. We make this dedication for the benefit of the public at large and to the detriment of our heirs and successors. We intend this dedication to be an overt act of relinquishment in perpetuity of all present and future rights to this software under copyright law.

**THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.**

For more information, please refer to <https://unlicense.org>
