---
topic: Tuning Fork — Sound Healing iOS App Synthesis Guide
date: 2026-02-08
source: Deep research (web sources + acoustic literature)
instruments: tuning-fork
---

# Tuning Fork: Comprehensive Synthesis Guide for iOS Sound Healing App

## 1. Physical Acoustics

### How a Tuning Fork Produces Sound

A tuning fork is a U-shaped metal bar that vibrates when struck. The two prongs (tines) act as coupled cantilever beams, fixed at the stem and free at the tips. When struck, the tines vibrate in opposition — moving toward and away from each other symmetrically.

**Key physics:**
- Each tine behaves as a free-clamped bar (Euler-Bernoulli beam theory)
- The fork's frequency depends on tine length, thickness, and material density
- Formula: **f = (1.875²/2π) × (t/L²) × √(E/12ρ)** where t = thickness, L = length, E = Young's modulus, ρ = density
- The symmetric vibration mode is the fundamental; asymmetric modes die quickly

### Material Properties

| Material | Young's Modulus (GPa) | Density (kg/m³) | Use Case |
|---|---|---|---|
| Steel (tool steel) | 200-210 | 7800-7850 | Standard medical/music forks |
| Aluminum alloy | 69-73 | 2700 | Sound healing (lighter, longer sustain) |
| Beryllium copper | 131 | 8250 | Precision laboratory forks |
| Crystal quartz | 72 | 2650 | "Crystal tuning forks" (marketing term, actually aluminum) |

**For synthesis:** Steel forks have slightly shorter decay (more damping); aluminum forks ring longer with a purer tone.

### Vibration Modes (from Dan Russell, Penn State Acoustics)

A tuning fork has several distinct vibration modes:

1. **Fundamental (symmetric flexural)** — The primary mode. Tines move in/out symmetrically. This is the frequency printed on the fork. Very pure near-sine output. Example: 426 Hz fork.

2. **Clang tone (antisymmetric flexural)** — The first overtone. Frequency is approximately **6.27× the fundamental** (NOT a harmonic integer). For a 426 Hz fork, the clang tone ≈ 2671 Hz. Dies out rapidly (within 0.1–0.5 seconds).

3. **Second symmetric mode** — Approximately **17.55× the fundamental**. Very weak, dies almost instantly.

4. **Stem vibration at 2× fundamental** — The stem vibrates up/down at exactly 2× the fundamental frequency (852 Hz for a 426 Hz fork). This is NOT a fork vibration mode — it's a nonlinear coupling effect. This is what you hear when you touch the stem to a table.

### Frequency Spectrum Analysis

**Typical spectrum of a struck tuning fork (first 0.5 seconds):**

| Component | Frequency Ratio | Relative Amplitude (dB) | Decay Time |
|---|---|---|---|
| Fundamental (f₀) | 1.000 | 0 dB (reference) | 10-30 seconds |
| 2nd harmonic (stem) | 2.000 | -20 to -30 dB | 2-5 seconds |
| Clang tone | ~6.27 | -10 to -15 dB initially | 0.1-0.5 seconds |
| 2nd overtone | ~17.55 | -30 to -40 dB | < 0.1 seconds |

**After ~1 second, the spectrum is essentially a pure sine wave at the fundamental.**

### Sound Radiation Pattern

- The fundamental mode radiates as a **longitudinal (linear) quadrupole** source
- In the near field: distinct directivity lobes (4 lobes at 0°, 90°, 180°, 270°)
- In the far field: transitions to a more uniform pattern
- The stem, when touched to a surface, radiates efficiently as a monopole (this is how practitioners use weighted forks on the body)

### Key Insight for Synthesis
A tuning fork is one of the simplest instruments to synthesize because after the initial transient (~0.5s), it's nearly a pure sine wave. The challenge is accurately modeling the first 500ms attack transient with the clang tone.

---

## 2. Digital Synthesis Approaches

### Recommended Algorithm: Additive Synthesis (Modal)

For tuning forks, **additive synthesis with 3-4 exponentially decaying sinusoids** is the optimal approach. It's computationally cheap, accurate, and perfect for real-time iOS.

### ADSR Envelope Characteristics

**Tuning fork has NO traditional ADSR — it's an impulsive excitation with exponential decay:**

| Parameter | Value | Notes |
|---|---|---|
| Attack time | 1-5 ms | Near-instantaneous (strike transient) |
| Decay time | N/A (no sustain transition) | — |
| Sustain level | 0 | Fork cannot sustain; always decaying |
| Release time | = remaining decay | Exponential, τ = 5-15 seconds |

**The envelope is best modeled as a sum of exponential decays**, one per partial:

```
envelope(t) = A_fundamental × e^(-t/τ_fundamental) + A_clang × e^(-t/τ_clang) + A_2nd_harmonic × e^(-t/τ_2nd)
```

### Decay Time Constants

| Component | Time Constant (τ) | T60 (time to -60dB) |
|---|---|---|
| Fundamental | 5-15 seconds (quality-dependent) | 12-35 seconds |
| 2nd harmonic (stem) | 1-3 seconds | 3-7 seconds |
| Clang tone | 0.03-0.15 seconds | 0.07-0.35 seconds |
| Higher overtones | < 0.05 seconds | < 0.12 seconds |

**Premium fork:** τ_fundamental ≈ 12-15s (long ring)
**Cheap fork:** τ_fundamental ≈ 3-5s (shorter, more overtone content)

### Synthesis Pseudocode (Additive)

```swift
// TuningForkSynthesizer — Additive synthesis for iOS
// Optimal for AudioKit / AVAudioEngine

struct TuningForkVoice {
    let sampleRate: Float = 44100.0
    var fundamentalFreq: Float = 528.0  // Hz
    var velocity: Float = 0.8           // 0-1, strike intensity
    
    // Partial parameters [freq_ratio, amplitude, decay_tau]
    let partials: [(ratio: Float, amp: Float, tau: Float)] = [
        (1.000,  1.00,  12.0),   // Fundamental — long pure ring
        (2.000,  0.05,   2.5),   // 2nd harmonic (stem vibration)
        (6.270,  0.15,   0.08),  // Clang tone — loud but brief
        (17.55,  0.02,   0.03),  // 2nd overtone — barely audible
    ]
    
    var phase: [Float] = [0, 0, 0, 0]
    var time: Float = 0.0
    var isActive: Bool = false
    
    mutating func strike(frequency: Float, velocity: Float) {
        self.fundamentalFreq = frequency
        self.velocity = velocity
        self.time = 0.0
        self.phase = [0, 0, 0, 0]
        self.isActive = true
    }
    
    mutating func nextSample() -> Float {
        guard isActive else { return 0.0 }
        
        var output: Float = 0.0
        let dt = 1.0 / sampleRate
        
        for i in 0..<partials.count {
            let freq = fundamentalFreq * partials[i].ratio
            let amp = partials[i].amp * velocity
            let tau = partials[i].tau
            
            // Exponential decay envelope
            let envelope = amp * exp(-time / tau)
            
            // Accumulate sine
            phase[i] += 2.0 * .pi * freq * dt
            if phase[i] > 2.0 * .pi { phase[i] -= 2.0 * .pi }
            
            output += envelope * sin(phase[i])
        }
        
        time += dt
        
        // Deactivate when inaudible (< -60dB from peak)
        if time > partials[0].tau * 4.6 { // 4.6τ ≈ -40dB
            isActive = false
        }
        
        return output
    }
}
```

### Weighted vs Unweighted Fork Synthesis Differences

| Parameter | Unweighted | Weighted |
|---|---|---|
| Fundamental amplitude | Reference (0 dB) | Reference (0 dB) |
| Clang tone amplitude | -10 to -15 dB | -25 to -35 dB (weights suppress) |
| Decay time (fundamental) | 8-15 seconds | 15-30 seconds (heavier = longer) |
| Perceived brightness | Brighter (more transient) | Warmer/deeper |
| Frequency range | 128-4096 Hz typical | 32-128 Hz typical (Otto tuners) |
| Vibration feel (haptic) | Minimal | Strong physical vibration |

**For weighted fork synthesis:** Reduce clang tone amplitude by 15-20 dB, increase fundamental decay time by 1.5-2×, and add a slight low-frequency modulation (0.5-2 Hz) to simulate the physical "buzz" feeling.

### Alternative Synthesis Approaches

| Method | Pros | Cons | Recommendation |
|---|---|---|---|
| **Additive (modal)** | Perfect for tuning forks; lightweight CPU; precise control | Limited complexity (not needed here) | **✅ PRIMARY** |
| **FM synthesis** | Can approximate clang tone quickly | Harder to control precisely; overkill | ⚠️ Not recommended |
| **Physical modeling** | Most physically accurate | Heavy CPU; unnecessary complexity for a simple fork | ❌ Overkill |
| **Sample-based** | Most realistic | Large memory; limited parameter control | ⚠️ Backup option |
| **Wavetable** | Fast; low CPU | Can't model time-varying spectrum well | ❌ Poor fit |

---

## 3. Real-World Frequency Data

### Complete Tuning Fork Frequency Reference for Sound Healing

#### Medical/Scientific Tuning Forks

| Frequency (Hz) | Note | Use | Notes |
|---|---|---|---|
| 128 | C3 | Neurological testing (Rinne/Weber) | Standard medical fork |
| 256 | C4 (Middle C) | Hearing tests, medical standard | Most common medical fork |
| 512 | C5 | Hearing tests (higher sensitivity) | Often paired with 256 Hz |
| 1024 | C6 | High-frequency hearing test | Less common |
| 2048 | C7 | High-frequency hearing test | Rare |
| 4096 | C8 | Crystal tuner / "angel" frequency | Very high, ethereal |

#### Solfeggio Frequencies

| Frequency (Hz) | Solfeggio Name | Therapeutic Claim | Note (approx) |
|---|---|---|---|
| 174 | — | Pain relief, anesthetic effect | ~F3 |
| 285 | — | Tissue healing, cellular repair | ~C#4/D4 |
| 396 | UT | Liberating guilt and fear | ~G4 |
| 417 | RE | Undoing situations, facilitating change | ~G#4/Ab4 |
| 528 | MI | "Love frequency," DNA repair | ~C5 |
| 639 | FA | Connecting relationships | ~D#5/Eb5 |
| 741 | SOL | Awakening intuition, expression | ~F#5 |
| 852 | LA | Returning to spiritual order | ~G#5/Ab5 |
| 963 | — | Divine consciousness, pineal activation | ~B5 |

#### Otto Tuners (Schumann Resonance Octaves)

| Frequency (Hz) | Octave | Type | Use |
|---|---|---|---|
| 7.83 | Base (Schumann) | N/A (too low for fork) | Reference only |
| 32 | 1st octave | Weighted, very heavy | Deep grounding, bone conduction |
| 64 | 2nd octave | Weighted | Relaxation, lower body work |
| 128 | 3rd octave | Weighted | Most popular Otto tuner; nervous system |
| 256 | 4th octave | Unweighted | Mental clarity |
| 512 | 5th octave | Unweighted | Higher awareness |

#### Planetary / Cosmic Octave Frequencies (Hans Cousto)

| Frequency (Hz) | Planet/Body | Therapeutic Claim |
|---|---|---|
| 126.22 | Sun | Vitality, confidence, leadership |
| 136.10 | Earth Year ("Om") | **Most popular.** Meditation, centering, heart chakra |
| 140.64 | Pluto | Transformation, letting go |
| 141.27 | Mercury | Communication, mental agility |
| 144.72 | Mars | Strength, will, action |
| 147.85 | Saturn | Discipline, structure, boundaries |
| 172.06 | Earth Platonic Year | Spiritual awareness |
| 183.58 | Jupiter | Expansion, abundance, wisdom |
| 194.18 | Earth Day | Grounding, physical energy |
| 207.36 | Uranus | Innovation, change |
| 210.42 | Moon (synodic) | Emotions, intuition, feminine energy |
| 211.44 | Neptune | Spirituality, dreams, imagination |
| 221.23 | Venus | Love, beauty, harmony |

#### Brain Tuner Set (binaural beat pairs)

Used in pairs to create specific brain wave frequencies:
- **Delta (0.5-4 Hz):** Deep sleep, healing
- **Theta (4-8 Hz):** Meditation, creativity
- **Alpha (8-13 Hz):** Relaxation, calm focus
- **Beta (13-30 Hz):** Active thinking, alertness

Typical pair: Fork A = 256 Hz, Fork B = 260 Hz → 4 Hz theta beat

#### Angel Tuners (Crystal Tuner Set)

| Frequency (Hz) | Name | Use |
|---|---|---|
| 4096 | Crystal Activator | Clearing crystals, opening crown chakra |
| 4160 | Angel Tuner 1 | Angelic communication (claimed) |
| 4225 | Angel Tuner 2 | Higher dimensional connection (claimed) |

#### Fibonacci Tuning Fork Set

| Ratio | Frequency (Hz) | Interval |
|---|---|---|
| 1 | ~256 (C4) | Unison |
| 1 | ~256 | Unison |
| 2 | ~288 | Major 2nd (9:8) |
| 3 | ~320 | Minor 3rd (6:5) |
| 5 | ~341.3 | Major 3rd (5:4) |
| 8 | ~384 | Perfect 4th (4:3) |
| 13 | ~426.7 | Augmented 4th |

#### Chakra Tuning Fork Frequencies

| Chakra | Frequency (Hz) | Note | Color |
|---|---|---|---|
| Root (Muladhara) | 256 | C4 | Red |
| Sacral (Svadhisthana) | 288 | D4 | Orange |
| Solar Plexus (Manipura) | 320 | E4 | Yellow |
| Heart (Anahata) | 341.3 | F4 | Green |
| Throat (Vishuddha) | 384 | G4 | Blue |
| Third Eye (Ajna) | 426.7 | A4 | Indigo |
| Crown (Sahasrara) | 480 | B4 | Violet |

---

## 4. Sound Healing Applications

### Practitioner Techniques

#### Weighted Forks (On-Body)
- **Placement:** Directly on the body at acupuncture points, joints, bones
- **Technique:** Strike fork on activator pad, immediately press stem to body point
- **Duration:** Hold for 10-30 seconds per point; repeat 2-3 times
- **Effect:** Vibration travels through bone and tissue; deeply relaxing
- **Popular forks:** Otto 128 Hz, Om 136.1 Hz
- **Key points:** C7 vertebra, sternum, sacrum, knee, shoulder, feet

#### Unweighted Forks (Off-Body / Biofield)
- **Placement:** 3-6 inches from the body, moved through the biofield
- **Technique:** Strike fork, hold near ears or sweep through energy field
- **"Combing" technique:** Slow sweeping motion from periphery toward body
- **Duration:** 30-60 seconds per area; full session 30-60 minutes
- **Popular forks:** Solfeggio set, Chakra set, 174 Hz

#### Biofield Tuning (Eileen McKusick method)
1. Activate 174 Hz unweighted fork
2. Start at the edge of the biofield (~5-6 feet from body)
3. Slowly move fork toward the body, listening for changes in tone
4. "Resistance" (tone changes) indicates stored energy/emotion
5. Hold fork at resistance points until tone clears
6. Guide energy back to the body's midline (chakra column)
7. Use weighted fork on the body to "ground" the energy

#### Session Structure (typical 60-minute session)
1. **Opening (5 min):** Om 136.1 Hz weighted on sternum
2. **Grounding (5 min):** Otto 128 Hz on feet/sacrum
3. **Biofield work (30 min):** Unweighted forks, combing technique
4. **Chakra balancing (15 min):** Chakra set, one fork per chakra
5. **Integration (5 min):** Om 136.1 Hz, deep breathing

### App Simulation Strategies

| Technique | App Simulation |
|---|---|
| Weighted on body | Play low-frequency + strong haptic feedback (Taptic Engine) |
| Off-body biofield | Play clearer tone + spatial audio (proximity based) |
| Bone conduction | Haptic emphasis, reduce speaker volume |
| Pair (binaural) | Pan Left/Right with slight frequency offset |

---

## 5. Interaction Design

### Touch Gestures

| Gesture | Action | Fork Response |
|---|---|---|
| **Tap** | Strike fork | Instant attack → long decay |
| **Long press** | Strike + hold to body | Strong haptic + muted audio (stem on surface) |
| **Double tap** | Quick re-strike | Reset envelope from current amplitude |
| **Swipe** | Mute/dampen | Rapid fadeout (0.2-0.5s) |
| **Two-finger pinch** | Adjust frequency | Bend pitch up/down |
| **Drag across screen** | Move fork through biofield | Spatial audio panning |

### Visual Feedback

- **Fork vibration animation:** Tines oscillate at visible frequency (use 2-5 Hz visual, not actual kHz)
- **Waveform visualization:** Real-time sine wave display showing the pure tone
- **Frequency label:** Large Hz display with note name
- **Decay indicator:** Circular progress ring showing remaining sustain
- **Color coding:** Match chakra colors to frequency
- **Glow effect:** Intensity mapped to amplitude

### Haptic Feedback (Core Haptics)

```swift
// Strike haptic pattern
let sharpImpact = CHHapticEvent(
    eventType: .hapticTransient,
    parameters: [
        CHHapticEventParameter(parameterID: .hapticIntensity, value: 0.9),
        CHHapticEventParameter(parameterID: .hapticSharpness, value: 0.8)
    ],
    relativeTime: 0
)

// Sustained vibration for weighted fork
let continuousVibration = CHHapticEvent(
    eventType: .hapticContinuous,
    parameters: [
        CHHapticEventParameter(parameterID: .hapticIntensity, value: 0.6),
        CHHapticEventParameter(parameterID: .hapticSharpness, value: 0.2)  // Warm, not buzzy
    ],
    relativeTime: 0.05,
    duration: 3.0
)
```

### Weighted vs Unweighted Mode Toggle
- **Weighted mode:** Deeper haptic, visual weight indicators on fork tines, body placement guide overlay
- **Unweighted mode:** Clearer/brighter sound, spatial audio emphasis, biofield visualization (aura-like rings)

---

## 6. Spectral Characteristics

### What Makes a Tuning Fork Sound Recognizable

1. **Extreme spectral purity** — After 0.5s, >99% of energy at fundamental
2. **The brief "clang" transient** — ~6.27× fundamental, creates the metallic "ting" on strike
3. **Very long decay** — 10-30 seconds, much longer than most instruments
4. **No vibrato or modulation** — Perfectly steady pitch (unlike voice or strings)
5. **Quadrupole radiation** — Characteristic directional quality when moved near ear

### Spectral Fingerprint Summary

```
Time 0.00-0.01s:  Broadband impulse noise (strike transient)
Time 0.01-0.10s:  Fundamental + clang tone (6.27×) + higher modes
Time 0.10-0.50s:  Fundamental + fading clang tone
Time 0.50-1.00s:  Fundamental + trace 2nd harmonic from stem
Time 1.00+:       Pure fundamental only (>-40dB purity)
```

### Quality Indicators (Premium vs Cheap)

| Characteristic | Premium Fork | Cheap Fork |
|---|---|---|
| Fundamental purity | >99% after 1s | 95-98% after 1s |
| Clang tone level | -15 dB relative | -8 to -10 dB relative |
| Decay time (T60) | 25-40 seconds | 5-15 seconds |
| Frequency accuracy | ±0.1 Hz | ±1-5 Hz |
| Inharmonic content | Very low | Noticeable wobble |
| Material | Precision machined steel/aluminum | Cast aluminum, rough edges |

### Weighted vs Unweighted Spectral Differences

**Weighted forks:**
- Fundamental shifted lower (32-128 Hz typically)
- Clang tone further suppressed (weights add mass at anti-nodes)
- Longer sustain (more mass = more energy storage)
- More energy transmitted through stem to body
- Less airborne sound projection

**Unweighted forks:**
- Higher frequencies (128-4096+ Hz)
- More prominent clang tone on strike
- Cleaner airborne projection
- Better for binaural beat creation
- More "bell-like" initial transient

### Formant Regions (What to Emphasize in Synthesis)

- **Body resonance coupling (weighted):** Broad peak 80-200 Hz when pressed to body
- **Metallic character:** The 6.27× clang tone gives the "tuning fork" identity
- **Air resonance:** No significant formants (unlike singing bowls or bells)
- **Beating effect (fork pairs):** When two forks are slightly detuned, audible beating at Δf Hz

---

## 7. Implementation Notes for iOS

### CPU Budget
- Additive synthesis with 4 partials = **negligible CPU** (< 1% on any modern iPhone)
- Can easily run 8+ simultaneous forks for pair/set playing
- No need for GPU/Metal — standard AudioUnit is sufficient

### Audio Engine Setup
```swift
// Recommended: AVAudioEngine with custom AudioUnit
let engine = AVAudioEngine()
let tuningForkNode = AVAudioSourceNode { _, _, frameCount, audioBufferList -> OSStatus in
    // Fill buffer with additive synthesis output
    for frame in 0..<Int(frameCount) {
        let sample = voice.nextSample()
        // Write to buffer...
    }
    return noErr
}
engine.attach(tuningForkNode)
engine.connect(tuningForkNode, to: engine.mainMixerNode, format: format)
```

### Recommended Preset Library Structure
```
TuningForkPresets/
├── solfeggio/      (174, 285, 396, 417, 528, 639, 741, 852, 963 Hz)
├── otto/           (32, 64, 128 Hz — weighted)
├── chakra/         (256, 288, 320, 341.3, 384, 426.7, 480 Hz)
├── planetary/      (126.22, 136.1, 141.27, ... Hz)
├── medical/        (128, 256, 512 Hz)
├── angel/          (4096, 4160, 4225 Hz)
└── custom/         (user-defined frequency)
```

---

## Sources

1. Russell, D.A. "Vibrational Modes of a Tuning Fork" — Penn State Acoustics. https://www.acs.psu.edu/drussell/Demos/TuningFork/fork-modes.html
2. Russell, D.A. "The Tuning Fork: An Amazing Acoustics Apparatus" — Acoustics Today, 2020.
3. HyperPhysics: Tuning Fork. http://hyperphysics.phy-astr.gsu.edu/hbase/Music/tunfor.html
4. Rossing, T.D. et al. "On the acoustics of tuning forks" — American Journal of Physics, 1992.
5. Wind Chimes Australia: Tuning Fork Frequencies Guide. https://windchimesaustralia.com.au/
6. Healing Sounds: Tuning Fork Frequencies Guide. https://healing-sounds.com/
7. Biofield Tuning: The Tuning Forks Used in Biofield Tuning. https://www.biofieldtuning.com/
8. Sound Healing Research Foundation: Biofield Tuning Protocol. https://soundhealingresearchfoundation.org/
9. McKusick, E. "Tuning the Human Biofield" — Healing Arts Press.
10. High Vibration Station: Tuning Fork Healing Sets. https://highvibrationstation.com/
