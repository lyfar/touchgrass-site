---
topic: Handpan (Hang Drum) — Sound Healing iOS App Synthesis Guide
date: 2026-02-08
source: Deep research (web sources + acoustic literature + Eyal Alon MSc thesis)
instruments: handpan
---

# Handpan: Comprehensive Synthesis Guide for iOS Sound Healing App

## 1. Physical Acoustics

### How a Handpan Produces Sound

The handpan (also called hang drum, after the original Hang® by PANArt) is a hand-played steel percussion instrument consisting of two hemispherical shells joined at the rim. Sound production involves multiple coupled vibration systems:

**Three-component sound system:**
1. **Tone field vibration** — Each hammered dimple (note area) vibrates as a tuned membrane
2. **Shell resonance** — The entire shell vibrates sympathetically
3. **Helmholtz resonance** — The enclosed air cavity acts as a Helmholtz resonator through the bottom port (Gu)

### Material Properties

| Property | Value | Notes |
|---|---|---|
| Material | Nitrided steel (DC04) or stainless steel (AISI 430/441) | Nitrided is traditional; stainless gaining popularity |
| Thickness | 0.8-1.2 mm | Thinner = more responsive, less sustain |
| Diameter | 50-60 cm (typically 53-57 cm) | Larger = lower Helmholtz resonance |
| Shell depth | ~10-12 cm per half | Affects internal air volume |
| Weight | 3-6 kg | Stainless tends heavier |

**Nitrided steel (gas-nitrided DC04):**
- Young's modulus: ~200 GPa (surface-hardened layer ~500 HV)
- Surface becomes extremely hard, enabling precise tuning
- Internal steel remains ductile for vibration

**Stainless steel (AISI 430):**
- Young's modulus: ~200 GPa
- More consistent material properties
- Brighter, longer sustain
- Corrosion resistant

### Vibration Modes of a Handpan Note

Each tone field is carefully hammered to produce **three tuned harmonics** in a 1:2:3 ratio:

| Mode | Name | Frequency Ratio | Description |
|---|---|---|---|
| Mode 1 | **Fundamental** | 1 | Primary pitch of the note |
| Mode 2 | **Octave** | 2 (exactly) | Tuned to 1 octave above fundamental |
| Mode 3 | **Fifth + Octave** (compound fifth) | 3 (exactly) | Tuned to an octave + fifth above fundamental |
| (Mode 4) | Fourth overtone | ~4 (some makers) | Only in lower notes; sometimes tuned to 2 octaves |

**This 1:2:3 harmonic ratio is what makes the handpan extraordinary** — unlike bells or cymbals (which have inharmonic partials), the handpan note fields are carefully tuned so the overtones form a perfect harmonic series. This creates a clear, musical, bell-like tone.

### Helmholtz Resonance

The enclosed cavity of the handpan acts as a Helmholtz resonator:

- **Resonance frequency typically: C2–G2 (65–98 Hz)**
- Saraz handpans: typically Eb2–F2 range (78–87 Hz)
- This is usually **lower than any played note** on the instrument
- The Helmholtz tone is excited by every note strike — it adds a subtle low-frequency "bloom"
- Visible in spectrograms as a constant low-frequency peak (~82 Hz in typical D Kurd)

**Formula:** f_H = (c/2π) × √(A/(V×L_eff))
where c = speed of sound, A = port area, V = cavity volume, L_eff = effective port length

### Sympathetic Resonance

A critical feature of the handpan: **striking one note causes other notes to vibrate sympathetically**, especially:
- Notes sharing harmonics (e.g., D3 and D4)
- Adjacent notes on the shell
- Notes whose overtones match the Helmholtz frequency

This creates the handpan's characteristic "cathedral" or "shimmering" quality.

### Frequency Spectrum Analysis (from Eyal Alon, University of York)

**Typical spectrum of a single handpan note strike:**

| Component | Frequency | Relative Amplitude | Decay Time |
|---|---|---|---|
| Helmholtz resonance | 65-98 Hz (varies) | -15 to -25 dB | 1-3 seconds |
| Fundamental (Mode 1) | Note frequency | 0 dB (reference) | 3-8 seconds |
| Octave (Mode 2) | 2× fundamental | -3 to -10 dB | 2-5 seconds |
| Compound Fifth (Mode 3) | 3× fundamental | -6 to -15 dB | 1.5-4 seconds |
| Shell modes | Various | -20 to -30 dB | 0.5-2 seconds |
| Attack transient | Broadband | -10 dB peak | < 10 ms |

**Key observation:** The handpan spectrum is remarkably harmonic (1:2:3), unlike most percussion. This is why it sounds so musical and "singing."

---

## 2. Digital Synthesis Approaches

### Recommended Algorithm: Modal Synthesis (Resonance Filter Bank)

Modal synthesis is the proven best approach for handpan (demonstrated in Eyal Alon's MSc research at University of York). It uses a **bank of second-order IIR resonance filters**, each representing one spectral peak.

### Why Modal Synthesis is Best

| Method | Suitability | Pros | Cons |
|---|---|---|---|
| **Modal synthesis** | **✅ BEST** | Proven for handpan; lightweight; accurate; parametric control | Need to extract mode data per note |
| Physical modeling (FEM) | ⚠️ Research-grade | Most physically accurate | Extremely CPU heavy; overkill for app |
| Additive synthesis | ✅ Good alternative | Simple; low CPU | Harder to capture attack transient |
| Sample-based (hybrid) | ✅ Good for quality | Most realistic | Large file sizes; limited parameter control |
| FM synthesis | ❌ Poor fit | Can approximate harmonic content | Hard to get the right timbre; metallic artifacts |
| Granular | ❌ Poor fit | — | Not suited for tonal percussion |

### ADSR Envelope Characteristics

The handpan has a distinctive **soft attack → medium decay → no sustain → natural release**:

| Parameter | Value | Notes |
|---|---|---|
| Attack time | **5-20 ms** | Soft fingertip = 15-20ms; sharp tap = 5ms |
| Decay 1 (initial) | **0.5-2 seconds** | Higher partials and transient noise |
| Decay 2 (main body) | **3-8 seconds** | Fundamental and harmonics |
| Sustain | **0** | No sustain — always decaying |
| Release | N/A | Natural exponential decay continues |

**The handpan envelope is best modeled as a sum of exponentially decaying modes:**

```
note(t) = Σ Aᵢ × e^(-t/τᵢ) × sin(2π × fᵢ × t + φᵢ)
```

### Decay Time Constants Per Mode

| Mode | Typical τ (seconds) | T60 (seconds) | Notes |
|---|---|---|---|
| Helmholtz | 0.5-1.5 | 1-3.5 | Dies moderately fast |
| Fundamental | 1.5-4.0 | 3.5-9.0 | Longest sustained mode |
| Octave | 1.0-2.5 | 2.3-5.8 | Shorter than fundamental |
| Compound Fifth | 0.5-2.0 | 1.2-4.6 | Shorter still |
| Shell modes | 0.2-0.8 | 0.5-1.8 | Brief |
| Attack noise | 0.002-0.01 | — | Broadband impulse |

### Modal Synthesis Pseudocode

```swift
// HandpanSynthesizer — Modal synthesis for iOS
// Based on Eyal Alon's approach using IIR resonance filters

struct HandpanNote {
    let sampleRate: Float = 44100.0
    
    // Each mode: (frequency Hz, amplitude, bandwidth Hz)
    // Bandwidth controls decay rate: r = e^(-π × B / fs)
    struct Mode {
        var frequency: Float    // Hz
        var amplitude: Float    // Linear (0-1)
        var bandwidth: Float    // Hz (controls decay; smaller = longer decay)
        var phase: Float = 0
        // IIR filter state
        var y1: Float = 0       // y[n-1]
        var y2: Float = 0       // y[n-2]
    }
    
    var modes: [Mode]
    var isActive: Bool = false
    
    // D3 note example: fundamental = 146.83 Hz
    static func dKurdD3() -> HandpanNote {
        return HandpanNote(modes: [
            // Helmholtz resonance
            Mode(frequency: 82.0,    amplitude: 0.08,  bandwidth: 1.5),
            // Fundamental
            Mode(frequency: 146.83,  amplitude: 1.0,   bandwidth: 0.5),
            // Octave (2×)
            Mode(frequency: 293.66,  amplitude: 0.6,   bandwidth: 0.8),
            // Compound fifth (3×)
            Mode(frequency: 440.0,   amplitude: 0.3,   bandwidth: 1.2),
            // Shell mode 1
            Mode(frequency: 520.0,   amplitude: 0.05,  bandwidth: 3.0),
            // Shell mode 2
            Mode(frequency: 680.0,   amplitude: 0.03,  bandwidth: 4.0),
            // Attack transient mode
            Mode(frequency: 1200.0,  amplitude: 0.15,  bandwidth: 50.0),
            Mode(frequency: 2400.0,  amplitude: 0.08,  bandwidth: 80.0),
        ])
    }
    
    mutating func strike(velocity: Float) {
        isActive = true
        // Reset filter states
        for i in 0..<modes.count {
            modes[i].y1 = 0
            modes[i].y2 = 0
        }
    }
    
    mutating func nextSample(excitation: Float) -> Float {
        guard isActive else { return 0.0 }
        
        var output: Float = 0.0
        var maxAmp: Float = 0.0
        
        for i in 0..<modes.count {
            let f = modes[i].frequency
            let B = modes[i].bandwidth
            let A = modes[i].amplitude
            
            // Calculate IIR coefficients
            let r = exp(-.pi * B / sampleRate)
            let a1 = -2.0 * r * cos(2.0 * .pi * f / sampleRate)
            let a2 = r * r
            
            // Second-order IIR resonance filter
            let y = A * excitation - a1 * modes[i].y1 - a2 * modes[i].y2
            
            output += y
            maxAmp = max(maxAmp, abs(y))
            
            // Update state
            modes[i].y2 = modes[i].y1
            modes[i].y1 = y
        }
        
        // Deactivate when silent
        if maxAmp < 1e-6 { isActive = false }
        
        return output
    }
}

// Excitation signal: short noise burst simulating finger strike
func generateExcitation(sampleRate: Float, duration: Float = 0.005) -> [Float] {
    let numSamples = Int(sampleRate * duration)
    var excitation = [Float](repeating: 0, count: numSamples)
    for i in 0..<numSamples {
        let t = Float(i) / Float(numSamples)
        // Exponentially decaying noise burst
        excitation[i] = Float.random(in: -1...1) * exp(-t * 10.0)
    }
    return excitation
}
```

### Velocity Sensitivity

The handpan responds dramatically to touch velocity:

| Velocity | Effect on Synthesis |
|---|---|
| **Soft (pp)** | Fundamental dominant; very little overtone; long decay; quiet attack |
| **Medium (mf)** | Balanced 1:2:3 harmonics; moderate attack transient |
| **Hard (ff)** | Strong overtones; prominent attack noise; shorter decay; possible pitch bend |
| **Slap** | Broadband noise burst; all modes excited; percussive |

**Implementation:** Scale Mode amplitudes and bandwidths with velocity:
```swift
// Velocity scaling
for i in 0..<modes.count {
    modes[i].amplitude *= pow(velocity, 0.5 + Float(i) * 0.3)
    // Higher modes scale more with velocity (they're only present on hard strikes)
}
```

### Sympathetic Resonance Implementation

```swift
// When note X is struck, lightly excite nearby notes
func applySympathicResonance(struckNote: Int, allNotes: inout [HandpanNote], amount: Float = 0.05) {
    let struckFreq = allNotes[struckNote].modes[0].frequency  // fundamental
    for i in 0..<allNotes.count where i != struckNote {
        let otherFreq = allNotes[i].modes[0].frequency
        // Check if any harmonics match
        for harmonic in 1...4 {
            let ratio = (struckFreq * Float(harmonic)) / otherFreq
            let nearestInt = round(ratio)
            if abs(ratio - nearestInt) < 0.02 {  // Within 2% = sympathetic excitation
                allNotes[i].strike(velocity: amount / Float(harmonic))
            }
        }
    }
}
```

---

## 3. Real-World Frequency Data

### Standard Handpan Tuning Reference (A=440 Hz)

#### D Kurd (Most Popular Scale)

The D Kurd is the "standard" handpan scale — ~60%+ of handpans sold are in D Kurd or D Celtic.

**D Kurd 9-note (standard):** D3 / A3, Bb3, C4, D4, E4, F4, G4, A4

| Position | Note | Frequency (Hz) | Interval from Ding |
|---|---|---|---|
| Ding (center) | D3 | 146.83 | Unison |
| 1 (bottom-left) | A3 | 220.00 | Perfect 5th |
| 2 (bottom-right) | Bb3 | 233.08 | Minor 6th |
| 3 (left) | C4 | 261.63 | Minor 7th |
| 4 (right) | D4 | 293.66 | Octave |
| 5 (upper-left) | E4 | 329.63 | Major 9th |
| 6 (upper-right) | F4 | 349.23 | Minor 10th |
| 7 (top-left) | G4 | 392.00 | Perfect 11th |
| 8 (top-right) | A4 | 440.00 | Perfect 12th |

**Overtones for each note (1:2:3 ratio):**

| Note | Fundamental | Octave (2×) | Compound 5th (3×) |
|---|---|---|---|
| D3 | 146.83 | 293.66 | 440.00 |
| A3 | 220.00 | 440.00 | 660.00 |
| Bb3 | 233.08 | 466.16 | 698.46 |
| C4 | 261.63 | 523.25 | 783.99 |
| D4 | 293.66 | 587.33 | 880.00 |
| E4 | 329.63 | 659.26 | 987.77 |
| F4 | 349.23 | 698.46 | 1046.50 |
| G4 | 392.00 | 783.99 | 1174.66 |
| A4 | 440.00 | 880.00 | 1318.51 |

**Helmholtz resonance: ~82 Hz (E2)**

#### D Celtic Minor / Amara (2nd Most Popular)

**D Celtic 9-note:** D3 / A3, C4, D4, E4, F4, G4, A4, C5

| Position | Note | Frequency (Hz) |
|---|---|---|
| Ding | D3 | 146.83 |
| 1 | A3 | 220.00 |
| 2 | C4 | 261.63 |
| 3 | D4 | 293.66 |
| 4 | E4 | 329.63 |
| 5 | F4 | 349.23 |
| 6 | G4 | 392.00 |
| 7 | A4 | 440.00 |
| 8 | C5 | 523.25 |

(Skips Bb3 compared to Kurd — makes it "cleaner" for improvisation)

#### Other Popular Scales

**E Pygmy:** E3 / B3, C4, E4, F#4, G4, B4, C5, E5

| Note | Hz |
|---|---|
| E3 | 164.81 |
| B3 | 246.94 |
| C4 | 261.63 |
| E4 | 329.63 |
| F#4 | 369.99 |
| G4 | 392.00 |
| B4 | 493.88 |
| C5 | 523.25 |
| E5 | 659.26 |

**F# Hijaz (Middle Eastern):** F#3 / C#4, D4, E#4, F#4, G#4, A4, C#5, D5

| Note | Hz |
|---|---|
| F#3 | 185.00 |
| C#4 | 277.18 |
| D4 | 293.66 |
| E#4 (F4) | 349.23 |
| F#4 | 369.99 |
| G#4 | 415.30 |
| A4 | 440.00 |
| C#5 | 554.37 |
| D5 | 587.33 |

**C# Annaziska (minor):** C#3 / G#3, A3, B3, C#4, D#4, E4, F#4, G#4

**D Integral:** D3 / A3, Bb3, C4, D4, E4, F4, A4

**Akebono (Japanese):** D3 / A3, Bb3, D4, E4, F4, A4, Bb4, D5

### 432 Hz Tuning Variant

For sound healing, many practitioners prefer **A4 = 432 Hz** instead of standard 440 Hz. All frequencies shift down proportionally:

| Note | 440 Hz tuning | 432 Hz tuning | Difference |
|---|---|---|---|
| D3 | 146.83 | 144.16 | -2.67 Hz |
| A3 | 220.00 | 216.00 | -4.00 Hz |
| D4 | 293.66 | 288.33 | -5.33 Hz |
| A4 | 440.00 | 432.00 | -8.00 Hz |

**Conversion formula:** f_432 = f_440 × (432/440) = f_440 × 0.98182

---

## 4. Sound Healing Applications

### Why Handpan for Sound Healing

The handpan is called the **"holy grail of sound therapy"** (Saraz Handpans) due to:
- **Pure harmonic overtone series (1:2:3)** — Perceived as calming and orderly
- **Pentatonic scales** — Almost impossible to play "wrong notes"
- **Full 360° sound radiation** — Envelops the listener
- **Tactile playing method** — Intimate, meditative act of playing
- **Long sustain** — Notes blend and overlap, creating natural ambiance
- **Sympathetic resonance** — Creates a "living" sound field

### Therapeutic Applications

| Application | Technique | Duration | Scale Preference |
|---|---|---|---|
| **Sound bath** | Slow, intentional playing; let notes ring | 30-60 min | D Kurd, D Celtic |
| **Meditation accompaniment** | Sparse, spacious playing | 15-30 min | Any minor scale |
| **Yoga class** | Background atmosphere during savasana | 5-15 min | E Pygmy, D Celtic |
| **One-on-one therapy** | Instrument placed near/on client's body | 20-40 min | D Kurd (432 Hz) |
| **Chakra alignment** | Play notes corresponding to chakra frequencies | 20-30 min | Custom tuning |
| **Stress relief** | Free improvisation | 10-20 min | D Celtic (easiest) |
| **Sleep/relaxation** | Very slow, quiet, single notes | 20-45 min | Any minor |

### Session Patterns

**Typical Sound Bath Structure:**

1. **Opening silence** (1-2 min) — Settle, breathe
2. **Single Ding strikes** (3-5 min) — Central note, spaced 10-15 seconds apart
3. **Slow melodic exploration** (10-15 min) — Adjacent notes, building pattern
4. **Full expression** (10-15 min) — All notes, rhythmic patterns, harmonics
5. **Gradual simplification** (5-10 min) — Return to simpler patterns
6. **Final Ding** (2-3 min) — Single note, let it ring to silence
7. **Integration silence** (2-3 min) — Complete quiet

### 432 Hz vs 440 Hz for Healing

| Aspect | 440 Hz | 432 Hz |
|---|---|---|
| Standard | International concert pitch | Alternative/"healing" pitch |
| Sound quality | Brighter, more energetic | Warmer, softer, more relaxed |
| Healing claim | No specific claim | "Aligned with nature," "heart-centered" |
| Evidence | Standard; no healing claim | Anecdotal; no peer-reviewed evidence |
| Compatibility | Works with all instruments | Only with other 432 Hz instruments |
| **Recommendation** | Include both tuning options in app | Default 440, user toggle to 432 |

### Handpan for Specific Conditions (Practitioner Reports)

| Condition | Approach | Notes |
|---|---|---|
| Anxiety | Slow D Celtic, 432 Hz, sparse playing | Focus on long sustain |
| Insomnia | Very quiet E Pygmy, single notes | Near-silence between notes |
| Pain management | Sound placed near affected area | Vibration therapy claim |
| PTSD | Careful, predictable patterns | Avoid sudden loud strikes |
| Depression | Gently building patterns, major scale | Uplifting progressions |

---

## 5. Interaction Design

### Touch Interface Layout

The handpan has a distinct circular layout with notes arranged around a central Ding:

```
        [7]   [8]
      /           \
    [5]    Ding    [6]
      \           /
    [3]           [4]
      \           /
        [1]   [2]
```

**Map this directly to the touch screen** with circular note zones.

### Touch Gestures

| Gesture | Action | Sound Response |
|---|---|---|
| **Tap (fingertip)** | Normal strike | Standard note with balanced harmonics |
| **Tap (thumb)** | Heavier strike | More fundamental, less overtone |
| **Quick flick** | Light grace note | Soft, mostly fundamental |
| **Press & hold** | Muted/dampened strike | Short, percussive, "tok" |
| **Two-finger tap** | Harmonic touch | Emphasize octave overtone |
| **Swipe across center** | Ding glissando | Pitch bend down across Ding |
| **Tap center** | Ding strike | Deep central note + Helmholtz bloom |
| **Palm press** | Mute all | Rapid decay of all ringing notes |
| **Shake device** | Brush/roll pattern | Multiple rapid light strikes |

### Velocity Sensitivity Zones

Each note zone should respond to **touch force (3D Touch/pressure)** or **touch size/speed:**

| Velocity Layer | Touch Type | Sound |
|---|---|---|
| pp (pianissimo) | Light tap, small contact | Fundamental only; soft; long decay |
| mp (mezzo-piano) | Normal tap | Balanced harmonics; medium attack |
| mf (mezzo-forte) | Firm tap | Clear overtones; moderate attack |
| ff (fortissimo) | Hard/fast tap | Full spectrum; bright attack; shorter decay |
| Slap | Very hard, large contact | Percussive; noise-heavy; brief |

### Visual Feedback

- **Note glow:** Each zone glows on strike, intensity = velocity, fades with decay
- **Ripple animation:** Concentric circles emanate from strike point
- **Sympathetic indicators:** Adjacent notes subtly glow when sympathetically excited
- **Waveform mode:** Optional spectral visualization showing the 1:2:3 harmonics
- **Sustain trails:** Fading color trails showing how long each note rings
- **Scale overlay:** Show note names / Hz values on each zone

### Haptic Feedback

```swift
// Handpan strike haptic — softer than tuning fork
let fingerStrike = CHHapticEvent(
    eventType: .hapticTransient,
    parameters: [
        CHHapticEventParameter(parameterID: .hapticIntensity, value: velocity * 0.7),
        CHHapticEventParameter(parameterID: .hapticSharpness, value: 0.3)  // Soft, rounded
    ],
    relativeTime: 0
)

// Ding strike — deeper, heavier haptic
let dingStrike = CHHapticEvent(
    eventType: .hapticTransient,
    parameters: [
        CHHapticEventParameter(parameterID: .hapticIntensity, value: 0.9),
        CHHapticEventParameter(parameterID: .hapticSharpness, value: 0.15)  // Very deep
    ],
    relativeTime: 0
)

// Mute gesture — quick dampen
let muteHaptic = CHHapticEvent(
    eventType: .hapticTransient,
    parameters: [
        CHHapticEventParameter(parameterID: .hapticIntensity, value: 0.4),
        CHHapticEventParameter(parameterID: .hapticSharpness, value: 0.6)
    ],
    relativeTime: 0
)
```

### Playing Modes for App

1. **Free Play** — All notes active, improvise freely
2. **Guided Meditation** — Notes light up in sequence, user follows along
3. **Sound Bath Generator** — Auto-play mode with customizable patterns and tempo
4. **Learning Mode** — Teaches basic patterns and techniques
5. **Drone Mode** — Continuous sustained tone from selected notes (looping with crossfade)

---

## 6. Spectral Characteristics & What Makes Handpan Recognizable

### Sonic Fingerprint

The handpan has an immediately recognizable sound defined by:

1. **Clean 1:2:3 harmonic series** — Unlike bells (inharmonic) or drums (noise). The handpan's tuned overtones create a "singing" quality that bridges percussion and melody.

2. **Soft attack transient** — Played with fingers, not sticks. The attack is warm and rounded (5-20ms rise), unlike the sharp transient of a mallet strike.

3. **Helmholtz "bloom"** — Every strike excites the low cavity resonance (~80 Hz), adding a subtle bass warmth that's felt as much as heard.

4. **Long, even decay** — 3-8 seconds of smooth exponential decay, with harmonics fading at slightly different rates (higher modes decay faster).

5. **Sympathetic resonance halo** — After each strike, other notes gently ring in sympathy, creating a sustained "cloud" of harmony. This is the "cathedral effect."

6. **Steel timbre** — A specific metallic brightness that's NOT cymbal-like (too harmonic) and NOT bell-like (too free). It sits in a unique timbral space.

### Spectral Envelope Shape

```
Frequency:   |    Fundamental    Octave    Comp.5th    Shell modes
             |         |           |          |           |
Amplitude:   |    ████████     ██████     ████       ██   ██
(dB)     0dB |    ████████     ██████     ████       ██   ██
        -10  |    ████████     ██████     ████       ██   ██
        -20  |    ████████     ██████     ████
        -30  |    ████████     ██████
        -40  |    ████████
             |_____|___________|__________|___________|________
                  f₀           2f₀        3f₀        4f₀+
```

### Attack Transient Analysis

The initial 10-20ms contains:
- Broadband noise from finger impact on steel
- All modes excited simultaneously
- Higher modes (>3f₀) decay within 50-200ms
- The noise content is what distinguishes fingertip vs thumb vs mallet strikes

**For synthesis:** Use a short (5ms) noise burst convolved with the resonance filter bank as the excitation signal. Vary noise spectrum to simulate different strike types.

### Decay Behavior

```
Time 0.000-0.005s:  Impact noise + all modes at peak
Time 0.005-0.050s:  High shell modes rapidly decay; attack transient fades
Time 0.050-0.500s:  Compound fifth begins to fade; fundamental + octave dominant
Time 0.500-3.000s:  Fundamental dominant; octave fading; Helmholtz fading
Time 3.000-8.000s:  Fundamental only, gradually approaching silence
Time 8.000+:        Silence (or very faint sympathetic ringing from other notes)
```

### Quality Indicators

| Characteristic | Premium Handpan | Budget/Low Quality |
|---|---|---|
| Harmonic ratios | Exactly 1:2:3 | Slightly off (±5-15 cents) |
| Crosstalk | Minimal unwanted coupling | Neighboring notes buzz/rattle |
| Sustain | 5-10 seconds | 2-4 seconds |
| Helmholtz integration | Smooth, supportive | Boomy or absent |
| Attack consistency | Even across all notes | Inconsistent brightness |
| Sympathetic resonance | Musical, pleasant | Chaotic, uncontrolled |
| Tuning stability | Stays in tune for years | Drifts with temperature |

---

## 7. Implementation Notes for iOS

### CPU Budget

Modal synthesis with 8 modes per note × 9 notes = **72 second-order IIR filters**
- Each filter: ~10 FLOPS per sample
- At 44.1 kHz: 72 × 10 × 44100 = **31.8 MFLOPS**
- Modern iPhone: capable of **billions of FLOPS**
- **Conclusion: absolutely negligible CPU usage.** No Metal/GPU needed.

### Polyphony

- Support **9 simultaneous voices** (one per note area)
- Plus sympathetic resonance: additional 8 voices at low amplitude
- Total: 17 voices maximum ≈ 136 IIR filters = still trivial

### Audio Engine Architecture

```swift
// AVAudioEngine setup for handpan
let engine = AVAudioEngine()
let format = AVAudioFormat(standardFormatWithSampleRate: 44100, channels: 2)!

// Spatial audio for natural panning
// Map note positions to stereo field:
// Left notes → pan left; Right notes → pan right; Center → center
let notePanning: [Float] = [
    0.0,    // Ding (center)
   -0.4,    // Note 1 (bottom-left)
    0.4,    // Note 2 (bottom-right)
   -0.6,    // Note 3 (left)
    0.6,    // Note 4 (right)
   -0.5,    // Note 5 (upper-left)
    0.5,    // Note 6 (upper-right)
   -0.3,    // Note 7 (top-left)
    0.3,    // Note 8 (top-right)
]
```

### Recommended Preset Library Structure

```
HandpanPresets/
├── scales/
│   ├── d-kurd-440/       (D3/A3,Bb3,C4,D4,E4,F4,G4,A4)
│   ├── d-kurd-432/       (same notes, 432 Hz base)
│   ├── d-celtic-440/     (D3/A3,C4,D4,E4,F4,G4,A4,C5)
│   ├── d-celtic-432/
│   ├── e-pygmy/          (E3/B3,C4,E4,F#4,G4,B4,C5,E5)
│   ├── f-hijaz/          (F#3/C#4,D4,F4,F#4,G#4,A4,C#5,D5)
│   ├── c-amara/
│   ├── d-akebono/
│   └── custom/           (user-defined scale builder)
├── playing-modes/
│   ├── free-play/
│   ├── sound-bath/       (auto-play patterns)
│   ├── guided-meditation/
│   └── learning/
└── settings/
    ├── tuning (440/432 Hz)
    ├── reverb-amount (0-100%)
    ├── sympathetic-resonance (on/off/amount)
    └── velocity-curve (linear/logarithmic/custom)
```

### Reverb Recommendation

Handpan almost always benefits from reverb in therapeutic settings:
- **Type:** Hall or Cathedral reverb
- **Pre-delay:** 20-40ms
- **Decay time:** 2-4 seconds
- **Wet/dry:** 30-50% for sound healing
- **High-frequency damping:** Moderate (tame brightness, keep warmth)

Use Apple's built-in `AVAudioUnitReverb` with `.cathedral` or `.largeHall2` preset as starting point.

---

## Sources

1. Alon, E. "Analysis and Synthesis of the Handpan Sound" — MSc Thesis, University of York, 2015. https://etheses.whiterose.ac.uk/12260/
2. Carrie Chen: "Modal Synthesis of the Handpan Sound" — MUMT307 Project. https://carrieeex.github.io/MUMT307-project/
3. PANArt: "The Extraordinary Sound of the Hang" — Research paper, 2009.
4. Rossing, T.D. & Morrison, A. "Acoustics of the Hang" — Proceedings of ISMA, 2007.
5. Saraz Handpans: Handpan Resonance and Wave Interference. https://www.sarazhandpans.com/
6. The Sound Artist: Acoustics of the Handpan. https://thesoundartist.com/
7. Cosmos Handpan: D Kurd Handpan Notes Chart. https://www.cosmoshandpan.com/
8. Isthmus Instruments: Handpan Scale Guide. https://www.isthmusinstruments.com/
9. Saraz Handpans: Sound Healing & Meditation. https://www.sarazhandpans.com/handpan-community/sound-healing-meditation/
10. Sound Sculpture: Handpan Scales. https://sound-sculpture.de/
