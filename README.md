# Staccatone SoulTaker P7 Poly

**A polyphonic, heavily crushed emulation of the 1923 Staccatone electronic instrument — reborn in 2026 with extreme DSP mangling.**

This is a **Cmajor** processor designed specifically for **Amorph Instrument** (by Artists in DSP). It starts with a pure Hartley-oscillator sine wave (flute-like timbre of the original vacuum-tube Staccatone), then obliterates it through:

- Extreme square-law mu compression ("Pressure 7 – Soul-Taker")
- Coarse bit reduction (default 64× / 6-bit crunch)
- Authentic tube-style tanh saturation glow
- Per-voice high-pass noise-shaped dither (NJAD/airwindows-inspired)
- 8-voice polyphony with last-note-priority voice stealing and per-voice staccato envelopes

Perfect for glitchy, lo-fi, retro-futuristic leads, pads, drones, or experimental sound design in your DAW.

![Staccatone SoulTaker Screenshot Placeholder](https://via.placeholder.com/800x400/111/eee?text=Amorph+Instrument+UI+with+SoulTaker+P7)  
*(Screenshot of Amorph UI with knobs – replace with your actual capture)*

## Features

- **Polyphony**: Fixed 8-voice pool (expandable; MIDI chord support)
- **Oscillator**: Manual phase-accum sine (faithful to 1923 Hartley LC tank purity)
- **Crushing Chain**:
  - Bit-shift down/up for coarse digital degradation
  - Square-law mu compressor with extreme intensity (6–12 range, default 8.0 → heavy pumping/gating)
  - tanh soft saturation (tube-like warmth and even harmonics)
  - Per-voice high-pass shaped quantization noise (NJAD-style: airy hiss, less low-end mud)
- **Envelope**: Instant attack + tunable exponential decay (staccato character preserved)
- **MIDI Integration**: Note-on/off, velocity → amplitude scaling
- **Exposed Parameters** (Amorph knobs):
  - Input Gain
  - Pressure 7 Intensity
  - Bit Crush Shift (log2)
  - Decay Multiplier
  - High-Pass NJAD Dither toggle

## Demo / Audio Examples

*(Add links to SoundCloud/YouTube/X demos here once uploaded)*

- Short stab chords → crunchy staccato bursts
- Sustained pads → breathing, pumping distortion walls
- Low bass notes → gritty sub-crunch with artifacts

## Requirements

- **Amorph Instrument** plugin (VST/AU/CLAP) from Artists in DSP
  - Download: [amorph.instrument](https://artistsindsp.com) (or wherever hosted)
- A DAW that supports MIDI input (Ableton, Logic, Reaper, FL Studio, etc.)
- MIDI keyboard/controller or sequencer for poly play

## Installation & Usage

1. **Clone or download** this repo (or copy the `.cmajor` file from the gist/repo).
2. Open **Amorph Instrument** in your DAW on a MIDI/instrument track.
3. In Amorph's editor panel (Lab/Code mode):
   - Paste the full code from [`Staccatone_SoulTaker_P7_Poly.cmajor`](./Staccatone_SoulTaker_P7_Poly.cmajor)
   - Hit **Compile** (or the build button)
4. Play MIDI notes/chords:
   - Low velocities → subtle drive
   - High velocities → full aggression
   - Hold notes → hear extreme mu pumping
   - Quick triggers → sharp staccato decay
5. Tweak knobs live:
   - Crank **Pressure 7 Intensity** to 10–12 for near-gating collapse
   - Lower **Bit Crush Shift** to 4–5 for extreme lo-fi noise
   - Toggle dither off for raw quantization crunch

## Code Structure Highlights

- `struct Voice { ... }` — Per-voice state (phase, envelope, ditherError, etc.)
- Fixed pool with oldest-note stealing
- Event handlers for MIDI noteOn/velocity and parameter changes
- Main loop mixes active voices with normalization

```cmajor
// Snippet: Voice stealing on noteOn
int best = -1;
int maxAge = -1;
for (int i = 0; i < numVoices; ++i) {
    if (!voices[i].active) { best = i; break; }
    if (voices[i].age > maxAge) { maxAge = voices[i].age; best = i; }
}
