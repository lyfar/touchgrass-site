# Tingsha Cymbals — Comprehensive Research for Sound Healing iOS App

> **Compiled**: 2026-02-08
> **Purpose**: Digital synthesis reference, interaction design, and sound healing implementation
> **Instrument**: Tingsha (ting-sha, ཏིང་ཤགས་) — paired Tibetan hand cymbals

---

## Table of Contents
1. [Physical Acoustics](#1-physical-acoustics)
2. [Digital Synthesis](#2-digital-synthesis)
3. [Real-World Frequency Data](#3-real-world-frequency-data)
4. [Sound Healing Applications](#4-sound-healing-applications)
5. [Interaction Design for iOS](#5-interaction-design-for-ios)
6. [Spectral Analysis](#6-spectral-analysis)
7. [Synthesis Pseudocode (Swift)](#7-synthesis-pseudocode-swift)

---

## 1. Physical Acoustics

### 1.1 Instrument Construction

Tingsha are a pair of small, thick cymbals (2.5–4 inches / 6–10 cm diameter) connected by a leather strap or chain. Key physical characteristics:

- **Diameter**: 6–10 cm (most common: 6.5 cm and 7.5 cm)
- **Thickness**: 0.5–1 inch (significantly thicker than orchestral cymbals)
- **Weight**: Typically under 200g per pair
- **Shape**: Slightly concave, disc-shaped with central mounting hole
- **Edge profile**: Rounded rim that serves as the primary striking surface

### 1.2 Bronze Alloy Composition

Tingsha are traditionally cast from **bell bronze (B20)** — an alloy also used for church bells and professional cymbals:

| Metal     | Percentage | Acoustic Role |
|-----------|-----------|---------------|
| Copper    | ~78–80%   | Warmth, sustain, ductility |
| Tin       | ~20–22%   | Brightness, rigidity, higher partials |
| Trace metals | <2%    | Iron, silver, nickel — subtle tonal coloring |

**The "Seven Metal" tradition**: Some sacred tingsha incorporate gold, silver, copper, tin, iron, mercury, and lead — each associated with a celestial body (Sun, Moon, Venus, Jupiter, Mars, Mercury, Saturn). The acoustic effect is marginal since copper-tin dominates, but the tradition adds cultural significance.

**Key acoustic property of bell bronze**: Higher tin content (20% vs typical 8%) increases the alloy's rigidity and reduces internal damping, yielding:
- Brighter, higher-pitched overtones
- Longer sustain (reduced material energy absorption)
- Greater harmonic richness
- More prominent inharmonic partials (the "metallic shimmer")

### 1.3 Vibration Modes of a Circular Plate

Tingsha behave as **thick circular plates with free edges**. Their vibration modes are described by two numbers:

- **m** = number of diametric (radial) nodal lines
- **n** = number of circular nodal rings

Frequencies follow **Chladni's Law**:

```
f(m,n) = C × (m + 2n)^p
```

Where:
- C = constant depending on material properties, thickness, diameter
- p ≈ 2 for flat plates, but ranges **1.4–2.4** for curved bells/cymbals
- For tingsha (thick, slightly curved): p ≈ 1.6–2.0

**Key modes for tingsha-like thick cymbals:**

| Mode (m,n) | Relative Frequency | Character |
|------------|-------------------|-----------|
| (2,0)      | 1.00 (fundamental) | Primary tone — the "ring" |
| (3,0)      | ~1.65             | First overtone |
| (0,1)      | ~2.09             | First radial mode |
| (4,0)      | ~2.56             | Second diametric |
| (1,1)      | ~2.92             | Mixed mode |
| (5,0)      | ~3.65             | Higher diametric |
| (2,1)      | ~4.10             | Complex mixed mode |
| (6,0)      | ~4.95             | High diametric |

**Critical difference from orchestral cymbals**: Tingsha's thickness-to-diameter ratio is much higher, which:
- Raises the fundamental frequency significantly (2000+ Hz vs 200–400 Hz)
- Reduces the number of audible modes (cleaner, more "pitched" sound)
- Creates longer sustain (less energy transferred to chaotic modes)
- Produces a more bell-like, less "washy" timbre

### 1.4 The Beating Phenomenon

The defining acoustic signature of tingsha is the **beating pattern** created when two slightly detuned cymbals ring simultaneously.

**Physics of beating:**
```
f_beat = |f₁ - f₂|
```

When two cymbals with fundamentals of, say, 2500 Hz and 2503 Hz are struck together:
- Perceived pitch: ~2501.5 Hz (average)
- Beat frequency: 3 Hz (amplitude modulation at 3 cycles/second)
- Result: A characteristic "wah-wah-wah" shimmer

**Why tingsha pairs are deliberately detuned:**
- Beat frequencies of 1–5 Hz create a meditative, pulsating quality
- The beating occurs at every partial independently, creating complex shimmer
- Traditional makers match pairs by ear for aesthetically pleasing beat rates
- The interval between cymbals is described as "one octave higher than our brain's alpha state" (8–12 Hz alpha waves → beating creates binaural-like effects)

**Multi-partial beating**: Each pair of corresponding partials beats at its own rate:
- Fundamental beats at Δf₁
- 2nd partial beats at ~1.65 × Δf₁ (scaled by mode frequency ratio)
- 3rd partial beats at ~2.09 × Δf₁
- This creates a complex, evolving texture — NOT a simple tremolo

### 1.5 Frequency Spectrum Overview

The tingsha spectrum is characterized by:
- **Fundamental range**: 2000–4000 Hz (depending on size)
- **Dominant energy**: 2000–6000 Hz
- **Upper harmonics**: Extending to 10–15 kHz with decreasing energy
- **Spectral tilt**: Approximately −6 dB/octave above the fundamental
- **No significant low-frequency content** (unlike singing bowls)

### 1.6 The Three Phases of the Sound

Like bells, tingsha sound has distinct temporal phases:

1. **Strike transient** (0–5 ms): Brief burst of broadband inharmonic energy from the initial metal-on-metal collision. Rise time ~2–5 ms (small thick cymbals have fast transients).

2. **Ring phase** (5 ms–5 s): The core sustaining tone. Dominated by the lower modes (2,0), (3,0), (0,1). Complex beating pattern most prominent here. Relatively slow decay.

3. **Tail / Hum** (5–20 s): The fundamental (2,0) mode and possibly (0,1) dominate as higher modes decay faster. Beating continues but simplifies. Very pure, bell-like quality. Grokipedia estimates resonance of "approximately 10–20 seconds."

---

## 2. Digital Synthesis

### 2.1 Modal Synthesis Architecture

Modal synthesis is the ideal approach for tingsha. The architecture:

```
Excitation Signal → [Bank of Parallel Resonators] → Output
                     Mode 1: (f₁, A₁, τ₁)
                     Mode 2: (f₂, A₂, τ₂)
                     ...
                     Mode N: (fₙ, Aₙ, τₙ)
```

Each mode is a **decaying sinusoid** (exponentially damped oscillator):
```
mode_k(t) = A_k × exp(-t / τ_k) × sin(2π × f_k × t + φ_k)
```

Where:
- `f_k` = frequency of mode k
- `A_k` = initial amplitude
- `τ_k` = decay time constant (time to decay to 1/e ≈ 36.8%)
- `φ_k` = initial phase (randomized for natural sound)

**Number of modes needed**: 12–20 for convincing tingsha (fewer than orchestral cymbals due to thickness filtering out upper modes).

### 2.2 Implementing the Beating Effect

For a tingsha pair, synthesize **two parallel modal banks** with slightly offset frequencies:

```
Cymbal A: modes at f₁, f₂, f₃, ... (base frequencies)
Cymbal B: modes at f₁×(1+δ₁), f₂×(1+δ₂), f₃×(1+δ₃), ...
```

Where δ represents the detuning ratio for each mode:
- δ_fundamental ≈ 0.001–0.003 (1–8 Hz beat at 2500 Hz)
- Higher modes may have slightly different δ values
- Randomize δ slightly per strike for organic variation

**Beat rate control parameter**: Map a single user-facing "shimmer" knob to the detuning spread:
- Shimmer = 0: Both cymbals identical (no beating)
- Shimmer = 0.5: Traditional tingsha beating (~2–4 Hz)
- Shimmer = 1.0: Wide detuning (~6–10 Hz, more active shimmer)

### 2.3 Inharmonic Partial Ratios

Unlike strings (integer harmonics), tingsha partials follow **inharmonic ratios** characteristic of thick circular plates. Suggested ratios for synthesis:

```
Partial  Ratio    Amplitude   Decay(s)   Character
─────────────────────────────────────────────────────
1        1.000    1.00        12.0       Fundamental "ring"
2        1.647    0.72        9.5        First overtone
3        2.093    0.55        7.8        First radial
4        2.563    0.38        6.2        "Shimmer"
5        2.924    0.28        5.5        Mixed mode
6        3.651    0.18        4.0        Metallic color
7        4.097    0.12        3.2        High shimmer
8        4.952    0.08        2.5        Brilliance
9        5.438    0.05        2.0        Air
10       6.218    0.03        1.5        Ultra-high sparkle
11       7.105    0.02        1.2        Presence
12       8.326    0.01        0.8        Initial transient color
```

**These ratios are NOT integer multiples** — this is the source of metallic, bell-like quality. The ratios approximate Chladni's law with p ≈ 1.8 for a thick curved plate.

### 2.4 Frequency-Dependent Decay Modeling

Higher modes decay faster than lower modes. Model using:

```
R_k = b₁ + b₃ × (f_k)²
```

Where:
- `R_k = 1/τ_k` (loss factor, inverse of decay time)
- `b₁ ≈ 0.08` (global decay rate — controls overall sustain)
- `b₃ ≈ 3.0 × 10⁻⁹` (frequency-dependent damping)

For a fundamental at 2500 Hz:
- τ₁ ≈ 12 s (fundamental)
- τ₅ ≈ 5.5 s (at ~7300 Hz)
- τ₁₀ ≈ 1.5 s (at ~15500 Hz)

This creates the natural evolution: bright metallic attack → warm bell-like sustain → pure fundamental tail.

### 2.5 Excitation Signal Design

The strike transient shapes the initial "click" character:

```
excitation(t) = {
    A_strike × (t/t_rise) × exp(-t/t_decay)    for t > 0
    0                                             for t ≤ 0
}
```

Parameters:
- `t_rise ≈ 0.5–1.0 ms` (extremely fast — metal on metal)
- `t_decay ≈ 2–5 ms` (brief impulse)
- Add slight noise burst (0–3 ms) for realistic "clack" texture

**Velocity sensitivity**: Higher velocity should:
- Increase overall amplitude
- Disproportionately boost higher partials (brighter attack)
- Slightly shorten the excitation pulse
- Add more noise to the transient

### 2.6 Alternative: FM Synthesis Approach

For a lighter-weight implementation, FM synthesis can approximate the tingsha spectrum:

```
Carrier:Modulator ratio = 1 : 1.414 (√2, a classic bell ratio)
```

Properties:
- Non-integer c:m ratio produces inharmonic sidebands automatically
- Modulation index controls brightness (higher = more partials)
- Decreasing mod index over time simulates frequency-dependent decay
- Use 2–3 carrier/modulator pairs for richer spectrum

FM is more CPU-efficient but less controllable than modal synthesis. **Recommendation: Use modal synthesis for the main instrument; FM for a "lite" version or real-time preview.**

### 2.7 Spatial and Stereo Considerations

Tingsha are struck together, producing sound from a single point that then radiates:

- **Slight stereo spread**: The two cymbals radiate from slightly different positions (~5–10 cm apart)
- **Pan the two cymbal modal banks** slightly left and right (±5–15%)
- **Movement**: Sound practitioners often circle tingsha around the body — model with slow autopan or user-controlled pan
- **Room simulation**: Light reverb (hall or plate) enhances the spiritual atmosphere. Long pre-delay (30–50 ms) separates the dry strike from the room.

---

## 3. Real-World Frequency Data

### 3.1 Common Tingsha Frequencies by Size

| Diameter (cm) | Diameter (in) | Approx. Fundamental | Character |
|---------------|---------------|-------------------- |-----------|
| 4.5–5.0       | ~1.8–2.0      | 3500–4000 Hz        | Very high, bright, piercing |
| 5.5–6.5       | ~2.2–2.5      | 2800–3200 Hz        | High, clear, traditional |
| 7.0–7.5       | ~2.8–3.0      | 2200–2600 Hz        | Medium, warm, full |
| 8.0–9.0       | ~3.2–3.5      | 1800–2200 Hz        | Lower, deep, grounding |
| 9.5–10.0      | ~3.8–4.0      | 1500–1800 Hz        | Deepest, longest sustain |

**Key relationship**: Frequency is roughly inversely proportional to diameter squared (as predicted by plate theory):
```
f ∝ h / d²
```
Where h = thickness, d = diameter. Doubling diameter drops pitch by ~2 octaves.

### 3.2 Size vs. Acoustic Properties

| Property | Small (4.5 cm) | Medium (6.5 cm) | Large (9 cm) |
|----------|---------------|-----------------|--------------|
| Fundamental | ~3500 Hz | ~2500 Hz | ~1900 Hz |
| Sustain | 8–12 s | 12–18 s | 15–20+ s |
| Beating clarity | Very fast, subtle | Moderate, balanced | Slow, prominent |
| Overtone count | 8–12 | 12–16 | 15–20 |
| Attack character | Sharp, bright | Clear, pure | Warm, deep |
| Volume | Softer | Moderate | Louder |

### 3.3 Traditional and Modern Tunings

**Traditional Tibetan**: No standardized tuning system. Each pair is crafted and matched by ear. Emphasis on:
- Pleasing beat rate between the pair
- Clear, sustained ring
- Symbolic significance over musical pitch

**Modern adaptations**: Some manufacturers now offer:
- **432 Hz tuned**: Laser-etched for precise resonance at 432 Hz fundamental (popular in "healing frequency" community). Note: 432 Hz is far below typical tingsha range — this likely refers to octave-related tuning (432 × 4 = 1728 Hz, or 432 × 6 = 2592 Hz).
- **2500 Hz**: Marketed as a standard healing frequency for tingsha
- **Matched pairs**: Factory-tuned to within ±1–3 Hz for consistent beating

### 3.4 Specific Product Frequency Data

- **Himalayas Shop Plain Tingsha**: Marketed as "2500 Hertz frequency"
- **Meinl Sonic Energy Tingsha**: "One octave higher than brain's alpha state" (~8–12 Hz alpha × 2^n)
- **Therapy-grade tingsha**: Selected for consistent tonal quality and specific beat rates

---

## 4. Sound Healing Applications

### 4.1 Session Structure

#### Opening a Session
1. Three deliberate strikes, each allowed to decay fully
2. First strike: Announces the transition from mundane to sacred space
3. Second strike: Settles the energy
4. Third strike: Begins the silence / meditation

#### During a Session
- **Mindfulness anchor**: Single strike when guiding attention back to present
- **Transition marker**: Between meditation phases (breath → body scan)
- **Chakra work**: Circle resonating tingsha around body or chakra points
- **Sound bath accent**: Layer high-frequency tingsha over deeper bowls/gongs

#### Closing a Session
1. Three final strikes
2. Last strike allowed to fade completely into silence
3. Pause in silence for integration
4. Verbal guidance resumes

### 4.2 Striking Techniques

| Technique | Method | Sound | Use |
|-----------|--------|-------|-----|
| **Edge strike** | Horizontal, edges meet at ~45° angle | Clear, pure ring | Standard meditation use |
| **Flat strike** | Cymbals brought face-to-face | Duller, louder | Rarely used — too aggressive |
| **Glancing strike** | Edges brush past each other | Lighter, softer tone | Gentle mindfulness cues |
| **Spin strike** | Strike and rotate around body | Evolving spatial sound | Space clearing |
| **Held strike** | One held flat, other strikes edge | Different mode excitation | Tonal variation |

**Holding techniques:**
- **By the cord**: Standard. Allows free vibration, maximum sustain
- **Above the disc**: More control, slightly dampened sound
- **Damped hold**: Touch finger to cymbal after strike to stop ringing (important for timing)

### 4.3 Space Clearing Protocol

1. Set intention for clearing
2. Begin at room entrance
3. Walk clockwise around the perimeter
4. Strike tingsha every 3–5 steps
5. Pay extra attention to corners (energy collects there)
6. Move toward center
7. Final strike at center of room
8. Allow last ring to completely fade

### 4.4 Therapeutic Applications

| Application | Technique | Frequency/Duration |
|-------------|-----------|-------------------|
| **Stress reduction** | Slow, regular strikes | Every 10–15 seconds |
| **Aura cleansing** | Circle around body | Head to toe, 2–3 passes |
| **Chakra balancing** | Strike near each point | 7 points, hold until fade |
| **Sleep preparation** | Diminishing strikes | Getting softer, slower |
| **Focus/alertness** | Sharp, decisive strikes | 1–3 strikes only |
| **Energy field clearing** | Move through space | Combine with visualization |

### 4.5 Combination with Other Instruments

Tingsha occupy a unique **high-frequency niche** in the sound healing palette:

```
Frequency Spectrum of Sound Healing Instruments:
────────────────────────────────────────────────
Gong:          50–1000 Hz    (deep, enveloping)
Singing Bowl:  200–900 Hz    (warm, resonant)
Crystal Bowl:  300–1200 Hz   (clear, penetrating)
Chimes:        1000–3000 Hz  (airy, bright)
Tingsha:       2000–6000 Hz  (cutting, clarifying)  ← HERE
────────────────────────────────────────────────
```

Tingsha serve as **the highest-pitched primary instrument** in most sound healing setups, providing contrast and "awakening" energy against deeper instruments.

---

## 5. Interaction Design for iOS

### 5.1 Core Gesture Mapping

| Gesture | iOS Input | Sound Result | Haptic |
|---------|-----------|-------------|--------|
| **Strike** | Tap (center of screen) | Full strike, natural velocity | Medium impact |
| **Gentle strike** | Light tap / low force | Softer strike, fewer upper partials | Light impact |
| **Strong strike** | Hard tap / 3D Touch force | Loud strike, brighter, more transient | Heavy impact |
| **Let ring** | Default after any strike | Sound decays naturally (10–20s) | None |
| **Mute/Dampen** | Touch and hold (palm mute) | Rapid exponential cutoff (~200ms) | Soft thud |
| **Swirl** | Circular drag gesture | Slow pan modulation (space clearing) | Continuous light |
| **Repeat strike** | Double/triple tap | Quick successive strikes | Multiple impacts |

### 5.2 Detailed Interaction Flows

#### Primary: Single Strike
```
User taps screen
  → Haptic feedback (impact, medium)
  → Excitation signal triggers
  → Both cymbal modal banks activate
  → Sound rings naturally for 10-20 seconds
  → Visual: Cymbal animation, concentric ripples
```

#### Mute Gesture
```
User touches and holds during ring
  → Haptic: soft press
  → Rapid decay applied (τ → 0.1s for all modes)
  → Sound stops within 200-500ms
  → Visual: Ripples contract, cymbal stills
  → Release: Ready for next strike
```

#### Space Clearing Mode
```
User enters "clearing" mode
  → Circular drag gesture detected
  → Auto-strike every N degrees of rotation
  → Pan follows gesture position
  → Stronger strikes at cardinal points (corners)
  → Visual: Energy clearing animation
```

### 5.3 Visual Design Suggestions

- **Idle**: Pair of cymbals hanging, gentle ambient glow
- **Strike**: Flash of golden light at contact point, expanding rings
- **Ringing**: Concentric vibration rings pulse at the beat frequency
- **Muted**: Rings compress and fade quickly
- **Beat visualization**: Subtle pulsing glow synced to the beating frequency (1–5 Hz)

### 5.4 Parameters Exposed to User

| Parameter | Range | Default | Effect |
|-----------|-------|---------|--------|
| **Pitch** | 1500–4000 Hz | 2500 Hz | Fundamental frequency (maps to "size") |
| **Shimmer** | 0–100% | 40% | Detuning between pair (beat rate) |
| **Sustain** | 3–25 s | 12 s | Overall decay length |
| **Brightness** | 0–100% | 60% | High partial amplitude |
| **Reverb** | 0–100% | 30% | Room simulation |

### 5.5 Timer/Session Integration

- **Meditation timer**: Configure N strikes at start, M at end, interval strikes during
- **Mindfulness bell**: Random or interval-based strikes throughout the day (background)
- **Breathing pacer**: Strike on inhale/exhale transitions
- **Session recording**: Log strike times for session playback

---

## 6. Spectral Analysis

### 6.1 Measured Partial Frequencies (Estimated from Plate Theory)

For a typical **6.5 cm diameter, B20 bronze tingsha** with fundamental at ~2500 Hz:

| Mode | Freq (Hz) | Ratio | Amplitude (dB) | Decay T60 (s) |
|------|-----------|-------|-----------------|----------------|
| (2,0) | 2500 | 1.000 | 0 (ref) | 15–20 |
| (3,0) | 4118 | 1.647 | −3 | 12–15 |
| (0,1) | 5233 | 2.093 | −5 | 10–12 |
| (4,0) | 6408 | 2.563 | −8 | 8–10 |
| (1,1) | 7310 | 2.924 | −11 | 6–8 |
| (5,0) | 9128 | 3.651 | −15 | 4–6 |
| (2,1) | 10243 | 4.097 | −18 | 3–5 |
| (6,0) | 12380 | 4.952 | −22 | 2–3 |
| (3,1) | 13595 | 5.438 | −25 | 1.5–2.5 |
| (0,2) | 15545 | 6.218 | −30 | 1–2 |

### 6.2 Decay Characteristics

**Temporal decay envelope** (overall amplitude vs time):

```
Phase 1 — Strike transient:     0–5 ms      | −20 to 0 dB (rise) then −10 dB (first 50ms)
Phase 2 — Initial ring:         5 ms–2 s     | −10 to −20 dB (bright, complex)
Phase 3 — Sustained ring:       2–8 s        | −20 to −35 dB (warm, beating prominent)
Phase 4 — Tail:                 8–20 s       | −35 to −60 dB (fundamental only, pure)
```

**Frequency-dependent decay rates:**
```
Frequency Band    | T60 (time to −60 dB) | Character
──────────────────|─────────────────────|──────────────────
2–3 kHz           | 15–20 s             | Very long — the "ring"
3–5 kHz           | 10–15 s             | Long — warm overtones
5–8 kHz           | 5–10 s              | Medium — shimmer
8–12 kHz          | 2–5 s               | Short — initial brightness
12+ kHz           | 1–3 s               | Very short — transient sparkle
```

### 6.3 Beating Patterns

For a pair detuned by 3 Hz at the fundamental:

| Partial | Cymbal A (Hz) | Cymbal B (Hz) | Beat Rate (Hz) | Perceptual Effect |
|---------|---------------|---------------|-----------------|-------------------|
| 1 | 2500 | 2503 | 3.0 | Slow pulse, prominent |
| 2 | 4118 | 4123 | 4.9 | Moderate flutter |
| 3 | 5233 | 5239 | 6.3 | Fast shimmer |
| 4 | 6408 | 6415 | 7.7 | Rapid tremolo |
| 5 | 7310 | 7319 | 8.8 | Very fast, blends |

**Note**: The beat rate scales approximately with the mode frequency ratio, since each mode's detuning is proportional. This creates a **layered, evolving shimmer** — slow pulsing in the fundamental with increasingly fast modulation in the overtones.

### 6.4 Spectral Comparison

```
Tingsha vs Related Instruments:
─────────────────────────────────
                  Fundamental    Inharmonicity    Sustain    Beating
Tingsha           2000–4000 Hz   High             Very long  Prominent (paired)
Singing Bowl      200–900 Hz     Low-moderate     Very long  Sometimes (wobble)
Finger Cymbal     4000–8000 Hz   Very high        Short      Sometimes
Church Bell       100–1000 Hz    Moderate         Very long  Inherent (near-degenerate modes)
Orchestral Cymbal 200–600 Hz     Very high        Long       Not prominent
```

---

## 7. Synthesis Pseudocode (Swift)

### 7.1 Core Modal Resonator

```swift
/// A single exponentially-decaying sinusoidal mode
struct TingshaMode {
    let frequencyRatio: Float   // Ratio to fundamental
    let amplitude: Float        // Initial amplitude (linear, 0–1)
    let decayTime: Float        // Seconds to decay to 1/e
    
    var phase: Float = 0.0
    var currentAmplitude: Float = 0.0
    var frequency: Float = 0.0  // Computed from fundamental × ratio
    var decayRate: Float = 0.0  // 1/decayTime, precomputed
    
    mutating func prepare(fundamental: Float, sampleRate: Float) {
        frequency = fundamental * frequencyRatio
        decayRate = 1.0 / decayTime
        phase = Float.random(in: 0 ..< 2 * .pi) // Random initial phase
        currentAmplitude = amplitude
    }
    
    mutating func nextSample(sampleRate: Float) -> Float {
        let output = currentAmplitude * sin(phase)
        
        // Advance phase
        phase += 2 * .pi * frequency / sampleRate
        if phase > 2 * .pi { phase -= 2 * .pi }
        
        // Apply exponential decay
        currentAmplitude *= (1.0 - decayRate / sampleRate)
        
        return output
    }
}
```

### 7.2 Tingsha Cymbal Model (Single Cymbal)

```swift
/// Partial frequency ratios derived from thick circular plate theory
/// Based on Chladni's law with p ≈ 1.8 for bell-bronze tingsha
struct TingshaCymbalModel {
    
    static let modeDefinitions: [(ratio: Float, amp: Float, decay: Float)] = [
        // (frequencyRatio, amplitude, decayTime)
        (1.000, 1.00, 14.0),   // (2,0) — fundamental "ring"
        (1.647, 0.70, 11.0),   // (3,0) — first overtone
        (2.093, 0.50, 8.5),    // (0,1) — first radial mode
        (2.563, 0.35, 6.8),    // (4,0) — metallic color
        (2.924, 0.25, 5.8),    // (1,1) — mixed mode
        (3.651, 0.16, 4.2),    // (5,0) — shimmer
        (4.097, 0.10, 3.4),    // (2,1) — high shimmer
        (4.952, 0.06, 2.6),    // (6,0) — brilliance
        (5.438, 0.04, 2.0),    // (3,1) — air
        (6.218, 0.02, 1.5),    // (0,2) — sparkle
        (7.105, 0.01, 1.0),    // (4,1) — presence
        (8.326, 0.005, 0.7),   // (7,0) — transient color
    ]
    
    var modes: [TingshaMode]
    let fundamental: Float       // Hz (e.g., 2500)
    let sampleRate: Float
    
    init(fundamental: Float, sampleRate: Float = 44100) {
        self.fundamental = fundamental
        self.sampleRate = sampleRate
        self.modes = Self.modeDefinitions.map { def in
            TingshaMode(
                frequencyRatio: def.ratio,
                amplitude: def.amp,
                decayTime: def.decay
            )
        }
    }
    
    mutating func trigger(velocity: Float = 0.7) {
        for i in modes.indices {
            modes[i].prepare(fundamental: fundamental, sampleRate: sampleRate)
            // Velocity affects amplitude, biased toward higher partials
            let velocityScale = velocity * (1.0 + Float(i) * 0.1 * velocity)
            modes[i].currentAmplitude *= min(velocityScale, 1.5)
        }
    }
    
    mutating func nextSample() -> Float {
        var output: Float = 0.0
        for i in modes.indices {
            output += modes[i].nextSample(sampleRate: sampleRate)
        }
        return output * 0.15 // Master gain normalization
    }
    
    /// Rapid mute (finger damping)
    mutating func mute(dampingRate: Float = 50.0) {
        for i in modes.indices {
            modes[i].decayRate = dampingRate // Fast decay
        }
    }
}
```

### 7.3 Tingsha Pair with Beating

```swift
/// Full tingsha pair with controllable beating/shimmer
class TingshaPair {
    var cymbalA: TingshaCymbalModel
    var cymbalB: TingshaCymbalModel
    
    let shimmer: Float    // 0.0 = unison, 1.0 = wide detuning
    let panSpread: Float  // Stereo spread of the pair
    
    /// - Parameters:
    ///   - fundamental: Base frequency in Hz (2500 typical)
    ///   - shimmer: Detuning amount 0–1 (0.4 = ~3 Hz beat at fundamental)
    ///   - sampleRate: Audio sample rate
    init(fundamental: Float = 2500, shimmer: Float = 0.4, sampleRate: Float = 44100) {
        self.shimmer = shimmer
        self.panSpread = 0.1 // Slight stereo spread
        
        // Cymbal B is detuned based on shimmer parameter
        // shimmer 0.4 → ~0.12% detuning → ~3 Hz beat at 2500 Hz
        let detuneRatio: Float = 1.0 + shimmer * 0.003
        
        self.cymbalA = TingshaCymbalModel(
            fundamental: fundamental,
            sampleRate: sampleRate
        )
        self.cymbalB = TingshaCymbalModel(
            fundamental: fundamental * detuneRatio,
            sampleRate: sampleRate
        )
    }
    
    func strike(velocity: Float = 0.7) {
        cymbalA.trigger(velocity: velocity)
        cymbalB.trigger(velocity: velocity)
    }
    
    /// Returns stereo pair (left, right)
    func nextStereoSample() -> (Float, Float) {
        let sampleA = cymbalA.nextSample()
        let sampleB = cymbalB.nextSample()
        
        // Slight stereo panning of each cymbal
        let left  = sampleA * (0.5 + panSpread) + sampleB * (0.5 - panSpread)
        let right = sampleA * (0.5 - panSpread) + sampleB * (0.5 + panSpread)
        
        return (left, right)
    }
    
    func mute() {
        cymbalA.mute()
        cymbalB.mute()
    }
}
```

### 7.4 Strike Excitation with Transient Noise

```swift
/// Models the metal-on-metal strike impact
struct StrikeExcitation {
    let riseSamples: Int      // ~0.5 ms → 22 samples at 44.1 kHz
    let decaySamples: Int     // ~3 ms → 132 samples at 44.1 kHz
    let noiseAmount: Float    // 0–1, how much broadband noise in the strike
    
    var sampleIndex: Int = 0
    var isActive: Bool = false
    
    init(sampleRate: Float = 44100) {
        riseSamples = Int(0.0005 * sampleRate)    // 0.5 ms
        decaySamples = Int(0.003 * sampleRate)    // 3 ms
        noiseAmount = 0.3
    }
    
    mutating func trigger() {
        sampleIndex = 0
        isActive = true
    }
    
    mutating func nextSample() -> Float {
        guard isActive else { return 0 }
        
        let totalSamples = riseSamples + decaySamples
        guard sampleIndex < totalSamples else {
            isActive = false
            return 0
        }
        
        // Envelope: linear rise, exponential decay
        let envelope: Float
        if sampleIndex < riseSamples {
            envelope = Float(sampleIndex) / Float(riseSamples)
        } else {
            let decayPos = Float(sampleIndex - riseSamples) / Float(decaySamples)
            envelope = exp(-5.0 * decayPos)
        }
        
        // Mix impulse with filtered noise for metallic click
        let noise = Float.random(in: -1...1) * noiseAmount
        let impulse = (1.0 - noiseAmount)
        let output = envelope * (impulse + noise)
        
        sampleIndex += 1
        return output
    }
}
```

### 7.5 Complete Render Loop

```swift
/// Complete tingsha audio render for an audio buffer
func renderTingshaBuffer(
    pair: inout TingshaPair,
    excitation: inout StrikeExcitation,
    buffer: inout AudioBuffer,  // Stereo interleaved
    frameCount: Int
) {
    for frame in 0..<frameCount {
        // Get excitation sample (only active during first few ms after strike)
        let exc = excitation.nextSample()
        
        // Feed excitation into both cymbal models if active
        // (In a full implementation, excitation drives the resonator bank
        //  via convolution. Simplified here as additive.)
        
        // Get stereo output from the pair
        let (left, right) = pair.nextStereoSample()
        
        // Mix excitation transient with sustained resonance
        let transientGain: Float = 0.5
        buffer[frame * 2]     = left  + exc * transientGain
        buffer[frame * 2 + 1] = right + exc * transientGain
    }
}
```

### 7.6 Frequency-Dependent Decay Helper

```swift
/// Calculate decay time for a given mode frequency
/// Based on the loss model: R_k = b1 + b3 * f_k²
func calculateDecayTime(
    frequency: Float,
    globalDecay b1: Float = 0.08,
    frequencyDamping b3: Float = 3.0e-9
) -> Float {
    let lossRate = b1 + b3 * frequency * frequency
    return 1.0 / lossRate  // τ = 1/R (seconds)
}

// Examples:
// 2500 Hz → τ = 1/(0.08 + 3e-9 * 2500²) = 1/0.0800 ≈ 12.5 s
// 5000 Hz → τ = 1/(0.08 + 3e-9 * 5000²) = 1/0.155  ≈ 6.5 s
// 10000 Hz → τ = 1/(0.08 + 3e-9 * 10000²) = 1/0.38  ≈ 2.6 s
// 15000 Hz → τ = 1/(0.08 + 3e-9 * 15000²) = 1/0.755 ≈ 1.3 s
```

### 7.7 Natural Variation / Humanization

```swift
/// Add per-strike variation for organic feel
func humanize(mode: inout TingshaMode, amount: Float = 0.02) {
    // Slight frequency wobble (±2%)
    let freqVariation = 1.0 + Float.random(in: -amount...amount)
    mode.frequency *= freqVariation
    
    // Slight amplitude variation (±10%)
    let ampVariation = 1.0 + Float.random(in: -0.1...0.1)
    mode.currentAmplitude *= ampVariation
    
    // Slight decay variation (±5%)
    let decayVariation = 1.0 + Float.random(in: -0.05...0.05)
    mode.decayRate *= decayVariation
}
```

---

## Sources & References

### Primary Sources
- Wikipedia: [Tingsha](https://en.wikipedia.org/wiki/Tingsha)
- Grokipedia: [Tingsha](https://grokipedia.com/page/Tingsha) — detailed construction, history, acoustics
- Beer, Robert (2004). *Tibetan Ting-Sha: Sacred Sound for Spiritual Growth*. ISBN 1-85906-153-2

### Acoustics & Physics
- Chladni's Law: f(m,n) = C(m + 2n)^p, p varies 1.4–2.4 for bells/cymbals
- Rossing & Fletcher (2004). *Principles of Vibration and Sound*. Springer.
- Fletcher & Rossing (1998). *The Physics of Musical Instruments*. Springer.
- Perrin, Charnley & DePont (1983). "Normal modes of the modern English church bell." *J. Sound and Vibration*.

### Synthesis References
- Nathan Ho: [Exploring Modal Synthesis](https://nathan.ho.name/posts/exploring-modal-synthesis/) — comprehensive modal synthesis guide
- Sound On Sound: [Synthesizing Bells](https://www.soundonsound.com/techniques/synthesizing-bells) — practical bell synthesis
- McGill: [Percussion Synthesis](https://cim.mcgill.ca/~clark/nordmodularbook/nm_percussion.html) — additive, ring mod, FM approaches
- CCRMA Stanford: [FM Synthesis Part 2](https://ccrma.stanford.edu/software/clm/compmus/clm-tutorials/fm2.html) — bell FM ratios

### Sound Healing
- [Healing Sounds: Tingsha Meditation & Energy Clearing](https://healing-sounds.com/blogs/hand-cymbals/tingsha-cymbals-meditation-energy-clearing)
- [The Mindful Store: How to Use Tingsha Bells](https://mindfulstore.com.au/blogs/learn/how-to-use-tingsha-bells-and-their-powerful-benefits)
- [Himalayas Shop: Tingsha Bell Guide](https://www.himalayasshop.com/blogs/guides/tingsha-bell-everything-you-need-to-know)

### Cymbal Alloys
- [Modern Drummer: Cymbal Alloys](https://www.moderndrummer.com/2011/10/what-you-need-to-know-about-cymbal-alloys/)
- B20 Bell Bronze: 80% Cu, 20% Sn — standard for quality tingsha and cymbals
- Bell metal: [Wikipedia](https://en.wikipedia.org/wiki/Bell_metal) — 78% Cu, 22% Sn

### Bell Tuning
- Keltek Trust: True Harmonic ratios — Hum:0.25, Prime:0.50, Tierce:0.60, Quint:0.75, Nominal:1.0
- Hibberts: [The musical pitch of bells](https://www.hibberts.co.uk/the-musical-pitch-of-bells/)
