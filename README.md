# 🎛️ NIN MIDI Generator

**Generate MIDI patterns inspired by the Nine Inch Nails discography.**

A self-contained, single-file browser application (~5,700 lines) that combines a seeded pattern generation engine, a fully interactive piano roll, Web Audio API playback, real-time effects processing, and standard MIDI file export. No installation, no build step, no server required.

*Vibecoded by Robert James Cross.*

---

## How to Run

1. Download `NINMIDIFinal.html`.
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari).
3. Done. All logic runs client-side. No CDN dependencies; no internet connection required after download.

---

## Features at a Glance

- 14 NIN album presets, each with its own tempo range, scale palette, drum density, dissonance level, and mutation character
- 7 track types: Drums, Bass, Melody, Arpeggio, Pad/Chords, or Full Pattern (all tracks at once)
- Interactive piano roll with color-coded note blocks, zoom, drag-to-draw, click-to-toggle, and right-click delete
- Web Audio API playback with master distortion, delay, and reverb knobs
- Real-time waveform oscilloscope with a smooth orange playhead
- Export to standard `.mid` file or full JSON session snapshot
- Import JSON and restore entire sessions
- Shareable URL — patterns encode into the URL hash for one-click sharing
- Per-track locks, undo/redo history (20 states), and up to 32 named snapshots
- Keyboard shortcuts for every major action
- Auto-save to `localStorage` — your session persists on refresh

---

## Album Presets

Each album card configures the generator's tempo range, preferred scales, dissonance ceiling, mutation rate, and rhythmic character. Selecting an album also sets the midpoint tempo automatically.

| Album | Year | Character |
|---|---|---|
| Pretty Hate Machine | 1989 | Industrial pop, drum machines, gated snares |
| Broken | 1992 | Grinding industrial, distorted drums |
| The Downward Spiral | 1994 | Dark industrial / electronic hybrid |
| The Fragile | 1999 | Ambient complexity, analog warmth |
| With Teeth | 2005 | Live drums, rock energy |
| Year Zero | 2007 | Digital glitch, broken rhythms |
| Ghosts I–IV | 2008 | Instrumental ambient, cinematic |
| The Slip | 2008 | Raw garage energy |
| Hesitation Marks | 2013 | Minimal electronic, pulsing |
| Not the Actual Events | 2016 | Dark analog, jazz-industrial |
| Add Violence | 2017 | Sci-fi vintage textures |
| Bad Witch | 2018 | Jazz-industrial, high dissonance, chromatic |
| Ghosts V: Together | 2020 | Cavernous pads, ethereal drones |
| Ghosts VI: Locusts | 2020 | Granular horror ambient, subterranean bass |

---

## Controls Reference

### Core Parameters

| Control | Range | Notes |
|---|---|---|
| Tempo | 60–300 BPM | Slider + tap-tempo button |
| Key | C through B (all 12) | Root note for scale quantization |
| Scale | 16 options | See scale list below |
| Time Signature | 4/4, 3/4, 5/4, 7/8, Polyrhythmic | Polyrhythmic sets drums to 5/4 against 4/4 synths |
| Pattern Length | 1, 2, 4, 8, 16 bars + custom (1–256) | |
| Complexity | 1–10 | Controls note density and rhythmic variation |
| Swing | 0–100% | Delays off-beat hits for groove feel |
| Humanize Velocity | 0–100% | Adds random velocity and micro-timing variance |

### Advanced Parameters

| Control | Effect |
|---|---|
| User Motif Seed | Comma-separated notes (e.g. `C4, D#4, F4`) that melody/arp generators mutate from |
| Motif Intensity | 0% = strictly follow motif; 100% = treat as loose inspiration |
| Note Probability | Probability (10–100%) that any generated note actually fires; lower = sparser |
| Velocity Floor | Filters out notes below a minimum velocity threshold |
| Step Divisor | Gate/length multiplier for generative variations |
| Strict Scale Quantize | Checkbox — forces all notes to the selected scale |
| Euclidean Drum Algorithms | Checkbox — uses Euclidean rhythm distribution for drum patterns |

### Available Scales

Natural Minor, Harmonic Minor, Phrygian, Dorian, Mixolydian, Lydian, Locrian, Aeolian, Ionian, Chromatic, Pentatonic Minor, Blues, Whole Tone, Diminished, Half Diminished, Ambient (0,2,5,7,9)

---

## Curated Presets & Clip Seeds

Four named presets configure all parameters to match a signature NIN sound:

- **TDS-style Dense Percussion** — dense industrial drums in the *The Downward Spiral* style
- **Fragile-style Ambient Piano** — sparse, melodic, low tempo in the *The Fragile* style
- **With Teeth-style Tight Groove** — live drum feel with high humanize
- **Year Zero-style Digital Glitch** — high swing, broken rhythms, high mutation

Three MIDI clip seeds impose a starter drum grid before generation mutates it:

- **Basic Rock Beat** — kick on 1 and 3, snare on 2 and 4
- **Four on the Floor** — kick every quarter note
- **Broken Breakbeat** — irregular kick/snare pattern with displaced hats

---

## Piano Roll

The piano roll displays all generated tracks in a single scrollable view. Drum lanes appear at the top (color-coded by type), followed by a full two-octave pitched grid (C2–B5).

**Color coding:**

| Color | Track |
|---|---|
| Red | Kick |
| Orange | Snare |
| Yellow | Hi-hat |
| Green | Perc |
| Blue | Melody |
| Purple | Bass |
| Violet | Pad |

**Mouse controls:**

| Action | Result |
|---|---|
| Click + drag (piano key label) | Draw notes horizontally |
| Click (grid cell) | Toggle note on/off |
| Right-click (note) | Delete note |
| Scroll wheel | Zoom timeline in/out |
| Shift + click | Multi-select notes |
| Click + drag (knob) | Rotate effect knob |
| Double-click (knob) | Reset knob to default |
| Click (drum pad) | Audition that drum sound |

---

## Track Locks

Each track can be individually locked before re-generating. Locked tracks are preserved while unlocked tracks regenerate, enabling a layered composition workflow — build a bassline you like, lock it, then mutate the drums around it.

Lockable tracks: Drums (global), Kick, Snare, Hi-hat, Perc, Bass, Melody, Arpeggio, Pad.

---

## Generation Modes

| Mode | Description |
|---|---|
| Generate Pattern | Generates all unlocked tracks using the current album/settings |
| Generate Full Song | Creates a multi-section arrangement with verse/chorus/bridge variation |
| Controlled Chaos | Applies random mutations to unlocked tracks while respecting locks |
| Random Album | Randomly selects an album and regenerates |

---

## Effects

Three effects are applied to the Web Audio output in real time. All three have rotary knobs in the UI.

| Effect | Implementation |
|---|---|
| Distortion | WaveShaper node with a cubic curve |
| Delay | DelayNode with configurable feedback |
| Reverb | ConvolverNode with exponential-decay white-noise impulse response |

---

## History, Snapshots & Sessions

**History** (undo/redo) holds up to 20 states per session. Use Z to go back and X to go forward.

**Snapshots** are named saves stored in `localStorage` (up to 32). History items can be marked as Section A or Section B for MIDI arrangement export.

**Auto-save** — the active session (album, settings, pattern, history) is persisted to `localStorage` under the key `nin-midi-generator.session.v2` and restored automatically on next open.

---

## Export & Import

| Action | Output |
|---|---|
| Export MIDI | Downloads a `.mid` file — drums on MIDI Ch.10 (GM standard), pitched tracks on separate channels |
| Export JSON | Downloads a full session snapshot including history |
| Import JSON | Restores a previously exported session snapshot |
| Copy to Clipboard | Puts the current snapshot as JSON text onto the clipboard |
| Share Pattern | Encodes the pattern into the URL hash (Base64) and copies the URL |

### DAW Import Notes

The built-in help panel includes specific import instructions for: Ableton Live, Logic Pro, FL Studio, Bitwig Studio, Reason, Cubase, Pro Tools, and REAPER.

All exported MIDI contains raw note data only. Effects (reverb, delay, distortion) must be added in your DAW as post-processing — they are not embedded in the file.

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| Space | Play / Pause |
| Escape | Stop playback |
| G | Generate pattern |
| Shift+G | Generate full song |
| R | Random album |
| C | Controlled chaos |
| S | Save snapshot |
| Z | Undo |
| X | Redo |
| D | Toggle drum lock |
| B | Toggle bass lock |
| M | Toggle melody lock |
| P | Toggle pad lock |
| A | Toggle arpeggio lock |
| 1–5 | Select drum component (kick/snare/hihat/perc/tom) |
| Arrow keys | Navigate grid |
| Delete | Clear selected notes |
| Ctrl+S | Quick save to bank |
| Ctrl+E | Export MIDI |

---

## Generation Engine

The core generator is a seeded LCG (Linear Congruential Generator) pseudo-random number function, making patterns fully reproducible from a stored seed value. The seed is embedded in exported snapshots and shared URLs.

Pattern generation per track reads the selected album's characteristics object (containing `kickDensity`, `snarePattern`, `hihatStyle`, `intervalPref`, `rhythm`, `range`, `dissonance`, and `mutation`) and applies them against the active scale intervals, tempo, complexity, and swing settings to produce a `pattern` array of note objects. Each note carries: `step`, `note` (MIDI number), `type` (track), `velocity`, `duration`, and timing offset for swing/humanize.

NIN era groove templates (PHM era through Trilogy era) provide swing and humanize range presets that auto-apply when an album is selected.

A procedural **sound palette** is generated alongside each pattern using the album's `aggressive` and `industrial` flags to select from pools of descriptors (drum machines, synth hardware names, bass gear) and combine them into flavor text displayed in the UI.

---

## Tech Stack

All code is vanilla HTML, CSS, and ES2020+ JavaScript in a single file. No frameworks, no build tools, no external runtime dependencies.

| API / Feature | Use |
|---|---|
| Web Audio API | Synthesis, effects chain, oscilloscope analyser |
| Canvas 2D | Waveform visualizer and playhead |
| localStorage | Session persistence and snapshot storage |
| Blob / URL.createObjectURL | MIDI and JSON file downloads |
| FileReader API | JSON import |
| btoa / atob | URL hash pattern encoding |
| requestAnimationFrame | Smooth playhead and waveform animation |
