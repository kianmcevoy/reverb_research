Below is a **signal-analysis playbook** that mirrors what you did in listening, but turns it into **measurable evidence**. Think of this as _confirming hypotheses_, not discovering sound from scratch.

---

# How to approach signal analysis (mindset first)

**Order matters.** Always do this sequence:

1. **Hypothesis from listening**  
    (“This feels ER-driven with slow diffusion and late modulation.”)
    
2. **Targeted test**  
    (Pick the analysis that isolates _that_ claim.)
    
3. **Compare across presets / reverbs**  
    (Patterns matter more than absolute numbers.)
    

Never run “all tests” blindly. Run **the test that answers one question**.

---

# Core test signals (fixed set)

Use the _same_ signals every time:

- Dirac impulse
    
- Short noise burst (10–50 ms)
    
- Muted pluck
    
- Sustained sine
    
- Log sine sweep (20 Hz → 20 kHz, slow)
    

---

# The essential tests (what each reveals)

## 1) Impulse response (IR)

**This is the foundation.**

### What to do

- Capture the IR at high resolution (≥ 96 kHz if possible).
    
- Plot **linear time** and **log-time** views.
    

### What it reveals

- **ER timing & geometry** (first 0–80 ms)
    
- **Density ramp**
    
- **When diffusion “kicks in”**
    
- Predelay
    
- Discrete vs statistical behaviour
    

### What to look for

- Clear spikes → geometry-heavy
    
- Fast noise-like onset → high early diffusion
    
- Smooth ramp → Lexicon-style diffusion
    
- Lumps / periodicity → poor diffusion or modes
    

**Identifies:** ER model, diffusion speed, scattering placement

---

## 2) Energy decay curve (EDC / Schroeder)

Compute cumulative squared IR (backwards).

### What it reveals

- **Decay shape** (linear, bloom, multi-slope)
    
- Early vs late decay differences
    
- Where “Bloom” actually begins
    

### What to look for

- Single straight line → simple exponential decay
    
- Two slopes → frequency-dependent damping or structure
    
- Upward curvature → bloom / feedback coupling
    

**Identifies:** damping design, bloom mechanisms

---

## 3) Frequency-dependent RT60 (banded decay)

Split IR into octave or 1/3-octave bands → EDC per band.

### What it reveals

- **Damping strategy**
    
- LF vs HF decay ratios
    
- Crossover frequency between low/high RT60
    

### What to look for

- Smooth monotonic shortening toward HF → classic air absorption
    
- Midband bumps → plates / bodies
    
- Irregular LF → modes or poor LF control
    

**Identifies:** damping filters, shelf vs LP/HP, crossover placement

---

## 4) Spectrogram (time–frequency)

Use log-frequency spectrogram of IR or noise burst response.

### What it reveals

- **Dispersion** (tilted / curved energy traces)
    
- Mode persistence
    
- Late-tail modulation smear
    

### What to look for

- Vertical streaks → no dispersion
    
- Slanted / curved streaks → dispersion (allpasses)
    
- Wobbling lines → modulation
    

**Identifies:** dispersion allpasses, modulation placement

---

## 5) Modal analysis (pluck or sine burst)

Excite with short tonal signal.

### What it reveals

- **Modes**
    
- Resonant structure
    
- Coupling strength
    

### What to look for

- Long-lived narrow peaks → resonant structure
    
- Even decay across spectrum → statistical reverb
    
- Clusters → plate / body behaviour
    

**Identifies:** physicality, coupling topology

---

## 6) Correlation / echo density analysis

Compute autocorrelation or echo density vs time.

### What it reveals

- **Density vs diffusion distinction**
    
- Repetition / periodicity
    

### What to look for

- Rapid decorrelation → good diffusion
    
- Repeating correlation peaks → poor mixing
    
- Density rising but correlation staying high → dense but not diffused
    

**Identifies:** matrix quality, scattering effectiveness

---

## 7) Modulation detection

Feed a sustained sine or narrowband noise.

### What to do

- Look at instantaneous frequency or spectrogram.
    

### What it reveals

- **Modulation depth and rate**
    
- Whether modulation is delay-based or gain-based
    

### What to look for

- Sidebands → delay modulation
    
- Amplitude wobble only → gain modulation
    
- Slow wandering → random LFOs
    

**Identifies:** modulation type and placement

---

## 8) Time-segmented analysis (critical)

Repeat analyses on **time windows**:

- 0–20 ms
    
- 20–80 ms
    
- 80–300 ms
    
- 300 ms+
    

### Why

Most reverbs **change behaviour over time**.

**Identifies:** early/late handoff, time-varying diffusion, bloom ramps

---

# Test → element mapping (cheat sheet)

|Element|Best test|
|---|---|
|ER geometry|IR (linear time)|
|Density|IR + echo density|
|Diffusion|Autocorrelation|
|Dispersion|Spectrogram|
|Damping|Banded RT60|
|Bloom|EDC curvature|
|Modes|Pluck analysis|
|Modulation|Sustained sine|
|Scattering|Early IR + correlation|
|Physicality|Modes + late decay|

---

# How to compare reverbs properly

- **Normalize IR energy** before comparison
    
- Align IRs to first arrival
    
- Compare **shapes**, not loudness
    
- Overlay plots between presets
    

Ask:

> “What _changes shape_ when I turn this knob?”

That tells you what the control actually does.

---

# Common mistakes to avoid

- ❌ Analyzing only full IR, not time slices
    
- ❌ Ignoring correlation (density ≠ diffusion)
    
- ❌ Trusting spectrograms without listening context
    
- ❌ Over-interpreting small differences
    
- ❌ Forgetting modulation smears measurements (average over time)
    

---

# Minimal toolset (you don’t need fancy gear)

- Any DAW with:
    
    - IR capture
        
    - Spectrogram
        
    - EQ with band solo
        
- Optional scripting (Python / MATLAB) for:
    
    - EDC
        
    - correlation
        
    - batch comparisons
        

---

## One-sentence takeaway

> **Signal analysis doesn’t replace listening—it verifies which blocks must exist to produce what you already heard, by isolating time structure, frequency-dependent decay, correlation, and modulation separately.**

When you’re ready, I can:

- give you a **step-by-step analysis checklist per preset**,
    
- help you build a **small Python notebook** for batch reverb analysis,
    
- or show **what your own reverb’s plots _should_ look like** for each size/physicality target.