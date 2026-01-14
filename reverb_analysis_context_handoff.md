# Reverb Analysis Toolkit – Project Context Handoff

This document captures the full context, goals, structure, and current implementation state of the **gen / analyse** offline reverb analysis toolkit, so work can continue in a fresh chat without loss of continuity.

Author: Kian  
Tone preference: neutral, factual, no fluff  
Environment: Linux, Python, CLI-only  
Audio format: WAV, 48 kHz, mono or stereo  

---

## 1. High-level goal

Build a **non–real-time, offline reverb analysis suite** in Python to support:

1. **Listening tests**
2. **Signal analysis & visualisation**

The toolkit is split into two CLI-driven subsystems:

- `gen`     → deterministic test signal generator (writes WAV files)
- `analyse` → analysis + plotting (reads WAV files)

Readability and inspectability of code are higher priority than performance.

---

## 2. Design principles

- Explicit naming (no cryptic variables)
- One analysis → one plot → one CLI command
- No global matplotlib state
- Deterministic signals (seeded where randomness exists)
- Mono is a first-class citizen, not an afterthought
- Stereo supported where meaningful
- Internal representation: float32, range [-1, 1]

---

## 3. Folder structure (current)

reverb_analysis/
├── gen/
│   ├── __init__.py
│   ├── signals.py
│   └── cli.py
│
├── analyse/
│   ├── __init__.py
│   ├── io.py
│   ├── plotting.py
│   ├── ir.py
│   └── cli.py
│
└── test_tones/        # default output directory for gen

---

## 4. gen/ – Test signal generator

### 4.1 gen/signals.py

All generators return **mono** `GeneratedSignal(samples, sample_rate_hz)`.

Implemented generators:

- generate_impulse
- generate_click
- generate_impulse_train
- generate_noise (white / pink)
- generate_noise_burst
- generate_sine
- generate_sine_burst
- generate_log_sine_sweep
- generate_pluck_like (band-limited noise + decay)
- generate_karplus_strong_pluck (Karplus–Strong string model)

Karplus–Strong parameters:
- fundamental_frequency_hz
- feedback_decay_factor
- lowpass_blend
- excitation_noise_bandlimit_hz

### 4.2 gen/cli.py

CLI entrypoint: `python -m gen.cli`

Global options:
- --sample_rate_hz (default 48000)
- --channel_mode {mono,stereo}
- --output_directory (default: ./test_tones)

Each generator is a subcommand with full `--help`.

Output:
- WAV PCM16
- mono or stereo (L=R for stereo)
- written to output_directory

---

## 5. analyse/ – Analysis & plotting

### 5.1 analyse/io.py

Responsibilities:
- Load WAV (mono or stereo)
- Convert int16/int32/float WAV → float32 [-1, 1]
- Validate sample rate (48 kHz)
- Channel handling helpers

Key helpers:
- load_wav_file (supports mono_or_stereo)
- get_analysis_channels (mono-aware, stereo-aware)
- downmix_to_mono
- duplicate_mono_to_stereo

Mono handling philosophy:
- Mono input → analyse mono
- Stereo input → analyse L/R unless explicitly downmixed

---

### 5.2 analyse/plotting.py

Centralised matplotlib helpers:
- create_figure_and_axis
- finalize_and_show_or_save
- plot_time_series (supports alpha for channel opacity)
- plot_log_magnitude_over_time
- plot_spectrogram
- plot_waterfall_lines
- plot_scatter

No global rcParams changes.

---

### 5.3 analyse/ir.py

Impulse-response style visualisation.

Plots:
1. Full waveform
2. Early waveform zoom (default 0–80 ms)
3. Log-magnitude tail view (dB vs time)

Features:
- Mono or stereo input
- Optional stereo downmix
- Channel overlay with adjustable opacity (for correlation inspection)

Settings container:
ImpulseResponseViewSettings:
- early_window_seconds
- log_magnitude_floor_db
- use_mono_downmix
- secondary_channel_alpha (if added)

Convenience wrapper:
- plot_ir_from_wav_file()

---

### 5.4 analyse/cli.py

CLI entrypoint: `python -m analyse.cli`

Currently implemented command:
- analyse ir

Options:
- --input_wav_file_path
- --early_window_seconds
- --log_magnitude_floor_db
- --use_mono_downmix
- --output_basename
- --no_show

CLI is designed to grow with additional subcommands:
- decay
- rt60bands
- spectrogram
- waterfall
- modecloud
- diffusion
- modulation

---

## 6. Planned next analysis modules

In priority order:

1. analyse/decay.py
   - Schroeder EDC
   - T20 / T30 / RT60
   - Multi-slope decay detection

2. analyse/rt60bands.py
   - Octave / 1⁄3 octave band decay vs frequency

3. analyse/spectrogram.py
   - Log-frequency spectrogram
   - Modulation visibility

4. analyse/waterfall.py
   - Cumulative spectral decay (CSD)

5. analyse/modes.py
   - Mode cloud (frequency vs decay persistence)

6. analyse/diffusion.py
   - Autocorrelation vs time
   - Echo density vs time
   - Decorrelaton metrics

---

## 7. Intended workflow

1. Generate test tones:
   gen noise_burst
   gen sweep
   gen karplus_pluck

2. Process through reverb (DAW / plugin / hardware)

3. Analyse outputs:
   analyse ir
   analyse decay
   analyse waterfall

4. Cross-reference with printed listening-test questionnaires

---

## 8. Contextual motivation (important)

This toolkit supports **algorithmic reverb research**, including:

- FDN reverbs (16×16)
- Parametric butterfly / Hadamard / Householder matrices
- Early reflection networks
- Scattering vs diffusion vs density
- Physical modelling behaviours (strings, plates, bodies)
- Mode clouds, eigenvalue intuition, decay shaping

The analysis tools are intended to *inform tuning*, not replace listening.

---

## 9. How to resume in a new chat

In a new conversation, say something like:

“I’m continuing a Python project called reverb_analysis with gen/ and analyse/ folders.
I have a context handoff document describing the architecture and current code state.
I want to continue implementing analyse/decay.py next, following the same style.”

This document should provide sufficient context to proceed immediately.
