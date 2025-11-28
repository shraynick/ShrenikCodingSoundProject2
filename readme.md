# Project Two — Interfaces 02: Launch Pads (Pro+, Safe)

A browser-based musical **launch pad** built with **Tone.js**.

Play scales, improvise melodies, record audio, and build patterns with a step-sequencer — all in the browser with no installs.

> Designed as an interactive “Project Two” interface: neon, responsive, classroom-safe.

---

## Features

### 🎛 Launch Pad Grid

- **Configurable grid**: `3×3`, `4×4`, `5×5`, or `6×6`
- **Keys**: C → B (with enharmonics like C#, Db, etc.)
- **Scales**:
  - Major (Ionian)
  - Minor (Aeolian)
  - Dorian
  - Mixolydian
  - Pentatonic (Major / Minor)
  - Blues
  - Chromatic
- **Layouts**:
  - Left→Right (by scale)
  - Top→Bottom (by scale)
  - Snake (zig-zag rows)
  - Pitch Class (C→B, grouped by pitch class)
- **Octaves**:
  - Octave span: 1–4
  - Starting octave: 0–7

Pads are:

- Color-coded by pitch class
- Labeled (e.g., `C4`, `Eb5`)
- Animated with a ripple effect when hit

---

### 🎹 Performance Controls

- **Waveform**: `sine`, `triangle`, `square`, `sawtooth`
- **Effects**:
  - Reverb (Freeverb)
  - Ping-pong delay
- **Master volume**: -36 dB → 0 dB
- **Tempo**: 40–200 BPM
- **Swing**: 0–60% (8th-note swing)
- **Chord mode**:
  - One pad triggers a **triad** built from the scale span
- **Latch**:
  - When on, pads stay sounding after you press them until manually released or Panic is hit
- **Arpeggiator (Arp)**:
  - Cycles **held notes** in time with the tempo
  - Works with both single notes and chords
- **Metronome (Met)**:
  - Simple click using a membrane synth
  - Follows tempo & swing settings

---

### 🎛 Sequencer (16 × 8)

- **16 steps** (columns)
- **8 lanes** (rows)
- Each row is mapped to a note from the *current scale/grid*  
  (top rows are generally higher notes)
- Controls:
  - `Sequencer: On/Off` — start / stop playback
  - `Clear` — clear all steps
  - `Random` — fills steps with a moderate random density
- Visual playhead:
  - Current column is highlighted with a `play` outline
- Sequencer state is saved as part of a preset

---

### 🎧 Recording

- Uses **Tone.Recorder** if the browser provides it
- **Record** button:
  - First click: start recording
  - Second click: stop and render audio
- A **Download** button appears with a `.wav` file:
  - Default name: `project-two-launchpads.wav`
- If recording isn’t available, the button disables itself safely

---

### 🎨 Themes & UI

- Themes:
  - **Neon** (default)
  - **Light**
  - **Dark**
- Switch via:
  - Theme dropdown in the header
  - `.` (period key) to cycle themes
- **Error Console**:
  - Appears at top when something breaks
  - Buttons:
    - `Copy` — copy logs
    - `Clear` — clear logs and hide
    - `Hide` — hide without clearing
  - Uses `window.__LP_ERR()` internally to push app-level errors

---

### 🎹 Keyboard & MIDI

**Computer keyboard mapping:**

- Pads are mapped left→right, top→bottom across:
  - Number row: `1 2 3 4 5 6 7 8 9 0`
  - Top row: `q w e r t y u i o p`
  - Home row: `a s d f g h j k l ;`
  - Bottom row: `z x c v b n m , . /`

**Global keyboard shortcuts:**

- `Space` — Hold sustain (like a sustain pedal)
- `L` — Toggle **Latch** on/off
- `Esc` — Panic (stop all notes, clear active pads)
- `.` — Cycle theme

**MIDI input:**

- Uses `navigator.requestMIDIAccess`
- Select device from **MIDI Input** dropdown
- Behavior:
  - MIDI **Note On** → trigger closest pad by pitch
  - MIDI **Note Off** → release that pad
  - MIDI **CC64 (sustain pedal)**:
    - Values ≥ 64 → sustain on
    - Below 64 → sustain off & release non-latched notes

---

### 💾 Presets & Persistence

Stored in `localStorage`:

- Theme key: `lp_theme`
- Preset key: `lp_project_two_preset`

Saved preset includes:

- Key, scale, layout, grid size
- Octave count & start octave
- Waveform, reverb, delay, master volume
- Tempo & swing
- Chord mode on/off
- Latch on/off
- Sequencer on/off
- Full 2D sequencer step matrix

Controls:

- **Save Preset 💾** — captures current state
- **Load** — restores the last saved state

On page load:

1. Restores last theme (if any)
2. Tries to auto-load the last preset
3. Falls back to default config if needed

---

## How to Run

### Requirements

- A modern browser (Chrome, Edge, Firefox, or Safari)
- A simple static HTTP server (recommended)

### Quick Local Setup

1. Save the main file as `index.html`.
2. Serve it with one of:
   - **Node**:
     ```bash
     npx serve .
     ```
   - **Python**:
     ```bash
     python -m http.server 8000
     ```
   - **VS Code**:
     - Use the *Live Server* extension.
3. Open the URL, e.g. `http://localhost:8000`.

> You can also double-click the file to open it, but some features (audio/MIDI) may behave better when served via HTTP.

---

## How to Operate

### 1. Basic Sound Check

1. Open the page in your browser.
2. **Click anywhere** inside the window once to allow audio (browsers require a user action).
3. Click any pad or press a mapped key (e.g., `1`, `q`, `a`, `z`).
4. You should hear a tone and see:
   - Pad highlight
   - Ripple animation
   - Waveform moving in the **Waveform** scope panel

---

### 2. Configure the Scale & Grid

In the **Scale & Layout** panel (left):

1. **Key**:  
   Choose `C`, `D`, `E`, etc. (optionally `C#`, `Db`, etc.).
2. **Scale**:  
   Try `Major (Ionian)` or `Minor (Aeolian)` first.
3. **Grid Size (n×n)**:  
   Pick `4` for a classic 4×4 layout.
4. **Octaves**:
   - Set `Octaves` to `2` for a comfortable range.
   - `Start Octave` to `3` or `4` for mid-range notes.
5. **Layout**:
   - Start with `Left→Right (by scale)`.
   - Experiment with `Snake` or `Pitch Class (C→B)` later.

Click pads or use keyboard to hear how the layout changes.

---

### 3. Shape the Sound

In the **Synthesis** panel:

1. **Waveform**:
   - `sine` → smooth, mellow
   - `square` → chiptune/retro
   - `sawtooth` → bright/edgy
2. **Reverb** slider:
   - 0% = dry
   - 50–70% for ambient/spacey sounds
3. **Delay** slider:
   - Adds echo that bounces left/right
4. **Master volume**:
   - Use around `-12 dB` to keep things comfortable
   - Increase towards `0 dB` if it’s too quiet

The waveform scope will show the resulting signal shape in real time.

---

### 4. Perform with Latch, Chords, Arp & Metronome

In the **Performance** panel:

1. **Latch**:
   - Click `Latch: Off` to toggle:
     - On: `Latch: On` — pads stay active after tap; tap again or Panic to stop.
2. **Chord**:
   - Click `Chord: Off` → `Chord: On`:
     - Each pad plays a scale-based triad instead of a single note.
3. **Arp**:
   - Enable **Latch** or manually hold a few keys.
   - Turn on `Arp: Off` → `Arp: On`:
     - The app cycles through held notes in time with the tempo (8th notes).
4. **Metronome**:
   - `Met: Off` → `Met: On` to hear a click at the current tempo.
5. **Tempo** & **Swing**:
   - Adjust **Tempo** slider (e.g., 110 → 140 BPM).
   - Add **Swing** (e.g., 20–30%) for a more “groovy” feel.

Use `Esc` at any time to trigger **Panic** and stop everything quickly.

---

### 5. Use the Sequencer

In the **Sequencer** panel:

1. Click `Sequencer: Off` → `Sequencer: On`  
   This starts a looping 16-step pattern (tied to Tone.Transport).
2. Grid layout:
   - Left column: row labels (notes).
   - Next 16 columns: steps 1–16.
3. To create a pattern:
   - Click cells to toggle them **On/Off**.
   - On = highlighted cell; Off = default background.
4. `Random` button:
   - Fills the grid with a random pattern (good for inspiration).
5. `Clear` button:
   - Turns off all steps.
6. Watch the **playhead**:
   - The current column is outlined with a `play` highlight.
   - Notes in “On” cells for that column will play at each tick.

You can keep changing the scale, grid, and sound while the sequencer is running.

---

### 6. Save & Load Presets

1. Set up a configuration you like:
   - Key/scale, grid, sound settings, sequencer pattern, etc.
2. Click **Save Preset 💾**:
   - Shows `Saved ✔️` briefly, then returns to `Save Preset 💾`.
3. Later (or after reloading page), click **Load**:
   - Restores:
     - All controls
     - Sequencer steps
     - Mode buttons (Chord, Latch, Sequencer on/off)

**Autoload**:  
On page refresh, the app automatically tries to restore the last saved preset.

---

### 7. Record & Download Audio

1. Click **Record ⏺**:
   - Recording starts; text becomes `Stop ⏹`.
2. Improvise:
   - Play pads, use sequencer, change modes.
3. Click **Stop ⏹**:
   - Recording stops and renders a `.wav` file.
4. A **Download ⤓** button appears:
   - Click it to save `project-two-launchpads.wav` to your device.

If recording isn’t supported in your browser, the Record button will show as unavailable.

---

## File Structure

This project is a **single-file app**:

- `index.html`  
  Contains:
  - HTML for controls, grid, sequencer, scope, and error console
  - Embedded CSS for layout and theming
  - Embedded JS for:
    - Tone.js setup (synth, effects, recorder, transport)
    - Pad/grid logic & keyboard mapping
    - Sequencer and playhead
    - MIDI input
    - Presets & theme persistence
    - Error console & waveform scope

---

## Accessibility & Safety

- Pads use:
  - `<button>` elements
  - `role="gridcell"`
  - `aria-label` like “Pad 5 C#4”
- Error console:
  - `role="alert"`
  - `aria-live="assertive"` for screen readers
- Audio:
  - Respects browser autoplay policies
  - Audio context is started only after a **pointer** interaction

---

## Customization Ideas

- Add **variable sequencer length** (e.g., 8 / 16 / 32 steps).
- Add **per-step velocity** (click with modifier key to change intensity).
- Export/import presets as JSON files to share with others.
- Add **different chord types** (maj7, min7, sus2, sus4, etc.).
- Add a **Humanize** toggle to slightly randomize timing/velocity.


> “Project Two – Interfaces 02: Launch Pads”

