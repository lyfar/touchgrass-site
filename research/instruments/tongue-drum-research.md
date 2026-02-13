# Steel Tongue Drum — Comprehensive Research for Sound Healing iOS App

## 1. Physical Acoustics

### How the Steel Tongue Drum Produces Sound

The steel tongue drum is an **idiophone** — a percussion instrument where the body itself vibrates to produce sound. It belongs to the slit drum family, evolved from propane tank instruments ("Hank Drums") and inspired by the handpan/Hang.

**Sound production mechanism:**
1. A steel resonance body (typically two hemispheres or a bowl shape) contains **laser-cut or hand-cut tongues** on the top surface
2. Tongues are tuned to specific pitches by adjusting their length, width, and curvature
3. When struck (by hand, fingertips, or rubber mallets), the tongue vibrates at its resonant frequency
4. The tongue vibration couples with the **air cavity inside the drum** (acoustic coupling)
5. The cavity acts as a **Helmholtz resonator**, amplifying and enriching the sound
6. Sound radiates from the sound hole (port) at the bottom and through the tongues themselves

### Coupled Tongue-Cavity Resonance

This is the key acoustic phenomenon that gives the tongue drum its characteristic warm, sustained, ethereal sound:

**Tongue vibration:**
- Each tongue acts as a **clamped-free plate** (clamped at the base, free at the tip)
- The fundamental vibration mode is a "flappy" mode — the tongue tip deflects up and down
- The second mode is a "twisty" mode — the tongue rotates about its long axis
- Higher modes involve progressively more complex bending patterns

**Cavity resonance:**
- The air volume inside the drum has its own resonant frequencies
- The lowest is the **Helmholtz resonance** — the cavity acts like a bottle when you blow across it
- Typical Helmholtz resonance: 50-150 Hz (depends on cavity volume and port size)

**Coupling mechanism:**
- When a tongue vibrates, it compresses/rarefies the air inside the cavity
- If a tongue frequency aligns with (or is near) a cavity resonance, **the whole system vibrates in unison**
- This coupling:
  - **Amplifies** the sound significantly
  - **Extends sustain** (energy transfers back and forth between tongue and air)
  - **Enriches overtones** (the cavity reinforces certain harmonics)
  - Creates the characteristic **warm, singing quality**

**Research findings (from experimental characterization studies):**
- The first two tongue modes and the Helmholtz resonance create a coupled three-mode system
- Proper tuning aligns tongue harmonics 1:2:3 with cavity resonances
- Striking one note can cause other notes to vibrate sympathetically (cross-coupling through the shared cavity)

### Vibration Mode Shapes (from FEA Modal Analysis)

From finite element analysis of tongue drum geometry (Jonathan South, 2020):

| Mode Type | Description | Frequency Range | Character |
|-----------|-------------|-----------------|-----------|
| **Bell modes** | Standing waves around the rim of the bowl | Low-mid | Bowl-like ring, no tongue involvement |
| **Tongue fundamental** | "Flappy" — tongue tip deflects vertically | Tuned pitch | Primary tone heard |
| **Tongue 2nd mode** | "Twisty" — tongue rotates about long axis | ~2× fundamental | First overtone |
| **Tongue 3rd mode** | Complex bending | ~3× fundamental | Second overtone |
| **Coupled modes** | Tongue + top plate interaction | Higher frequencies | Complex, rich overtones |
| **Helmholtz** | Air cavity "breathing" | ~80-150 Hz | Sub-bass warmth |

### Harmonic Structure

The harmonic ratios of a well-tuned tongue drum note:

| Partial | Ratio to Fundamental | Source | Relative Level |
|---------|---------------------|--------|----------------|
| f1 | 1.000 | Tongue fundamental | 0 dB (reference) |
| f2 | 2.000 | Tongue 2nd mode (tuned) | -6 to -12 dB |
| f3 | 3.000 | Tongue 3rd mode (tuned) | -12 to -20 dB |
| f_H | Variable (~0.3-0.5×) | Helmholtz resonance | -15 to -25 dB |
| f_sym | Various | Sympathetic from other tongues | -20 to -30 dB |

**Key difference from kalimba**: The tongue drum's overtones are approximately **harmonic** (1:2:3) because craftsmen explicitly tune the tongue shape to achieve this. The kalimba's overtones are anharmonic.

### Frequency Spectrum Characteristics

- **Attack**: Soft, rounded transient (especially with fingers/mallets) — NOT a sharp click
- **Sustain**: Long (3-10 seconds), slowly decaying
- **Decay shape**: Approximately exponential, with higher overtones decaying faster
- **Spectral envelope**: Concentrated energy at fundamental; progressive rolloff at higher partials
- **Bandwidth**: Each partial has narrow bandwidth (high Q factor) → very clean, pure tones
- **Sub-harmonics**: Helmholtz resonance adds low-frequency warmth below the fundamental

---

## 2. Digital Synthesis Approaches

### Modal Synthesis (Primary Approach)

Modal synthesis is the ideal technique for tongue drum synthesis because:
- The tongue drum naturally produces sound through discrete vibration modes
- Each mode has well-defined frequency, amplitude, and decay time
- A bank of resonant filters perfectly models the coupled tongue-cavity system
- It's computationally efficient for the rich, sustained tones needed

**Core algorithm:**
1. Analyze (or define) the modal frequencies, amplitudes, and decay rates for each note
2. Create a bank of **second-order IIR resonant filters**, each tuned to one mode
3. Excite all filters simultaneously with a shaped impulse (the "strike")
4. Sum all filter outputs for the final sound
5. Each filter naturally decays at its own rate

**Modal synthesis equation:**
For each mode i:
```
y[n] = b0 * x[n] - a1 * y[n-1] - a2 * y[n-2]

where:
  b0 = peak amplitude for mode i
  a1 = -2 * r * cos(2π * fi / fs)
  a2 = r²
  r  = e^(-π * Bi / fs)    (pole radius, determines decay)
  fi = frequency of mode i
  Bi = bandwidth of mode i (narrower = longer sustain)
  fs = sample rate
```

**Parameters per mode:**

| Parameter | Meaning | Typical Range |
|-----------|---------|---------------|
| fi | Mode frequency | 80 Hz - 2000 Hz |
| Bi | Bandwidth (3dB) | 0.5 - 10 Hz (very narrow!) |
| Ai | Amplitude | 0.0 - 1.0 |
| Q = fi/Bi | Quality factor | 50 - 500+ (very resonant) |

### Excitation Signal Design

The "strike" excitation is critical for realism:

**Finger strike:**
- Short raised cosine pulse, 3-8 ms duration
- Soft spectrum (limited high-frequency content)
- Lower velocity = shorter duration, less HF
- Produces warm, mellow tone

**Mallet strike:**
- Slightly longer pulse, 5-15 ms
- More mid-frequency energy
- Rubber mallet = softer; wood = brighter

**Excitation generation:**
```swift
func generateExcitation(velocity: Float, type: StrikeType) -> [Float] {
    let duration: Int  // samples
    let brightness: Float
    
    switch type {
    case .finger:
        duration = Int(sampleRate * 0.005 * (1.0 + velocity * 0.5))
        brightness = 0.3 + velocity * 0.3
    case .mallet:
        duration = Int(sampleRate * 0.010 * (1.0 + velocity * 0.3))
        brightness = 0.5 + velocity * 0.4
    }
    
    var excitation = [Float](repeating: 0, count: duration)
    for i in 0..<duration {
        let t = Float(i) / Float(duration)
        // Raised cosine with noise
        let envelope = 0.5 * (1.0 - cos(2.0 * .pi * t))
        let noise = Float.random(in: -1...1) * brightness
        let sine = sin(2.0 * .pi * t * 3.0)  // slight tonal quality
        excitation[i] = envelope * (sine * (1.0 - brightness) + noise * brightness) * velocity
    }
    return excitation
}
```

### Modal Data for Common Tongue Drum Notes

**Example: D3 note (146.83 Hz) on a D Kurd drum:**

| Mode | Frequency (Hz) | Amplitude | Bandwidth (Hz) | Decay Time (s) |
|------|----------------|-----------|-----------------|-----------------|
| Helmholtz | 82.0 | 0.08 | 5.0 | 2.0 |
| Fundamental | 146.83 | 1.00 | 1.0 | 6.0 |
| 2nd harmonic | 293.66 | 0.35 | 1.5 | 4.0 |
| 3rd harmonic | 440.49 | 0.12 | 2.0 | 3.0 |
| 4th harmonic | 587.33 | 0.04 | 3.0 | 2.0 |
| Cavity mode 1 | 420.0 | 0.06 | 8.0 | 1.5 |
| Shell mode | 850.0 | 0.02 | 15.0 | 0.5 |

### Sympathetic Resonance Modeling

When one note is struck, neighboring notes vibrate slightly:

```swift
// When note N is struck, also excite nearby notes at reduced amplitude
for otherNote in allNotes where otherNote != struckNote {
    let frequencyRatio = otherNote.frequency / struckNote.frequency
    // Notes with harmonic relationships resonate more
    let harmonicAffinity = isNearHarmonic(frequencyRatio) ? 0.05 : 0.01
    otherNote.excite(amplitude: velocity * harmonicAffinity)
}
```

### Alternative/Complementary Approaches

1. **Additive synthesis** — sum of decaying sinusoids (simpler than modal, less realistic coupling)
2. **Physical modeling** — finite-difference time-domain simulation (highest fidelity, very CPU-intensive)
3. **Karplus-Strong variant** — can approximate metallic percussion with modified feedback
4. **Sample-based with round-robin** — recorded samples with variation (highest realism, highest memory)
5. **Convolution with impulse responses** — record IR of each note, convolve with excitation

### Recommended Hybrid Approach for iOS

- **Modal synthesis** for the core tone (efficient, parametrically controllable)
- **Convolution reverb** (short IR) for the body/cavity character
- **Sympathetic resonance** layer for inter-note coupling
- **Sample-based attack** transient blended with modal body (optional, for highest quality)

---

## 3. Real-World Frequency Data

### Meinl Sonic Energy Steel Tongue Drum Scales

#### D Kurd (D Minor / Natural Minor) — Most Popular for Sound Healing

**Octave Steel Tongue Drum, 16" (OSTD1BK)**
9 notes, 440 Hz reference:

| Note | Frequency (Hz) | Position | Interval |
|------|----------------|----------|----------|
| D3 | 146.83 | Center (Ding) | Root |
| A3 | 220.00 | Surrounding | P5 |
| Bb3 | 233.08 | Surrounding | b6 |
| C4 | 261.63 | Surrounding | b7 |
| D4 | 293.66 | Surrounding | Octave |
| E4 | 329.63 | Surrounding | 9th (2) |
| F4 | 349.23 | Surrounding | b10 (b3) |
| G4 | 392.00 | Surrounding | 11th (4) |
| A4 | 440.00 | Surrounding | 12th (P5) |

Scale intervals: W-H-W-W-H-W-W (natural minor / Aeolian mode)

#### A Akebono (Japanese Scale)

**12" Steel Tongue Drum (STD2BK)**
8 notes, 440 Hz reference:

| Note | Frequency (Hz) | Position | Interval |
|------|----------------|----------|----------|
| A3 | 220.00 | Center | Root |
| B3 | 246.94 | Surrounding | M2 |
| C4 | 261.63 | Surrounding | b3 |
| E4 | 329.63 | Surrounding | P5 relative |
| F4 | 349.23 | Surrounding | b6 |
| A4 | 440.00 | Surrounding | Octave |
| B4 | 493.88 | Surrounding | M9 |
| C5 | 523.25 | Surrounding | b10 |

Scale formula: W-H-W+H-H-W+H-H-W (hemitonic pentatonic)

#### A Minor (Natural Minor / Aeolian)

| Note | Frequency (Hz) |
|------|----------------|
| A3 | 220.00 |
| B3 | 246.94 |
| C4 | 261.63 |
| D4 | 293.66 |
| E4 | 329.63 |
| F4 | 349.23 |
| G4 | 392.00 |
| A4 | 440.00 |

#### G Major (Ionian)

| Note | Frequency (Hz) |
|------|----------------|
| G3 | 196.00 |
| A3 | 220.00 |
| B3 | 246.94 |
| C4 | 261.63 |
| D4 | 293.66 |
| E4 | 329.63 |
| F#4 | 369.99 |
| G4 | 392.00 |

### Meinl Sonic Energy Complete Scale Catalog

| Scale Name | Root | Notes | Character |
|-----------|------|-------|-----------|
| D Kurd | D | D E F G A Bb C D | Meditative, passionate (minor) |
| A Akebono | A | A B C E F A B C | Japanese, contemplative |
| A Minor | A | A B C D E F G A | Melancholic, emotional |
| G Major | G | G A B C D E F# G | Bright, uplifting |
| C Major | C | C D E F G A B C | Pure, balanced |
| D Celtic Minor | D | D E F G A C D | Celtic, mystical |
| B Equinox | B | B C# D# E F# G# A# | Balanced, ethereal |

### Meinl Size/Category Guide

| Category | Size | Notes | Hz Standard | Best For |
|----------|------|-------|-------------|----------|
| Pocket | 3" | 6 | 440 Hz | Travel, gift |
| Mini | 6" | 8 | 440 Hz | Portable, bright |
| Small | 7" | 8 | 440 Hz | Thoughtful minor keys |
| Compact | 10" | 8-11 | **432 Hz** | Meditation, minor keys |
| Regular | 12" | 8-11 | 440 Hz | All-purpose, full sound |
| Octave | 10"/16" | 9-18 | 440 Hz | Each note in two octaves |

### 432 Hz Tuning Variants (Sound Healing)

For 432 Hz reference (multiply standard by 432/440 ≈ 0.9818):

| Note (440 Hz) | Standard (Hz) | 432 Hz Variant |
|---------------|---------------|----------------|
| D3 | 146.83 | 144.16 |
| A3 | 220.00 | 216.00 |
| C4 | 261.63 | 256.87 |
| D4 | 293.66 | 288.33 |
| E4 | 329.63 | 323.63 |
| A4 | 440.00 | 432.00 |

### Other Popular Tongue Drum Brands & Scales

| Brand | Notable Scales | Size Range |
|-------|---------------|------------|
| RAV Vast | D Celtic Minor, B Celtic, G Pygmy | 12"-20" |
| Beat Root | 6 switchable scales per drum | 14" |
| Idiopan | Dominant Pentatonic, Pygmy | 6"-14" |
| Hapi Drum | D Minor, D Major, C# Akebono | 8"-12" |
| UFO Drum | 432 Hz editions, custom | 10"-14" |

---

## 4. Sound Healing Applications

### Therapeutic Uses

1. **Meditation & Mindfulness**
   - Pentatonic tuning = impossible to play "wrong" notes
   - Long sustain creates continuous sonic environment
   - Intuitive playing encourages flow state
   - Perfect for guided meditation background

2. **Stress & Anxiety Relief**
   - Warm, low-frequency tones activate parasympathetic nervous system
   - Regular playing can lower cortisol levels
   - The act of striking the drum provides tactile grounding
   - Meinl markets specifically for "insomnia, anxiety relief, stress management"

3. **Sound Bath / Sound Therapy**
   - Clean, harmonically rich tones complement singing bowls
   - Multiple notes allow for melodic sound baths (unlike single-pitch bowls)
   - Sustained tones fill the room, creating immersive sonic environment
   - 432 Hz tuning variants specifically designed for therapeutic contexts

4. **Chakra Balancing**
   - Different scales correspond to different chakra systems
   - D Kurd (D minor) → associated with sacral and solar plexus chakras
   - Pentatonic scales avoid dissonance → smooth energy flow

5. **Neurological Benefits**
   - Rhythmic playing stimulates bilateral brain coordination
   - Repetitive melodic patterns can reduce seizure frequency in some epilepsy patients
   - Useful in occupational therapy for fine motor skill development

6. **Yoga & Movement Practice**
   - Sustaining tones provide sonic backdrop for flow sequences
   - Students can play between poses for mindful transition
   - The instrument's simplicity allows non-musicians to participate

### Practitioner Techniques

| Technique | Description | Therapeutic Goal |
|-----------|-------------|-----------------|
| **Single note meditation** | Strike one note, listen until silence, repeat | Deep focus, presence |
| **Slow scale walk** | Play each note in sequence, 5-10 sec between | Progressive relaxation |
| **Drone + melody** | Sustain bass note while playing melody on top | Grounding with movement |
| **Random intuitive play** | Play without pattern, trusting pentatonic safety | Creative expression, flow |
| **Rhythmic patterns** | Simple 4/4 or 3/4 time ostinatos | Entrainment, grounding |
| **Octave pairing** | Play same note in two octaves simultaneously | Fullness, harmonic richness |
| **Crescendo/decrescendo** | Gradually increase/decrease striking force | Emotional arc, release |
| **Silence integration** | Long pauses between strikes | Space for integration |

### Therapeutic Frequency Associations

| Frequency | Healing Association |
|-----------|-------------------|
| 128-146 Hz (D3) | Root/sacral grounding |
| 174 Hz | Pain relief, safety |
| 220 Hz (A3) | Heart opening |
| 256 Hz (C4 @ 432) | Balancing, centering |
| 285 Hz | Tissue healing |
| 396 Hz | Liberating guilt/fear |
| 417 Hz | Facilitating change |
| 432 Hz | "Frequency of the universe" — natural harmony |
| 528 Hz | "Love frequency" — DNA repair (claimed) |
| 639 Hz | Relationships, connection |

---

## 5. Interaction Design for iOS

### Zone Tapping Interface

The tongue drum's circular layout with central "ding" and surrounding "tone fields" maps naturally to a touch interface:

```
┌─────────────────────────────────────┐
│         TONGUE DRUM VIEW            │
│                                     │
│         ┌───┐   ┌───┐              │
│        │ G4 │ │ A4 │             │
│       ┌───┐ └─┬─┘ └─┬─┘ ┌───┐     │
│       │ F4 │   │     │   │ Bb3│    │
│       └─┬─┘ ┌─┴─────┴─┐ └─┬─┘     │
│         │   │          │   │       │
│    ┌───┐│   │   D3     │   │┌───┐  │
│    │E4 ││   │  (Ding)  │   ││C4 │  │
│    └───┘│   │          │   │└───┘  │
│         │   └─┬─────┬─┘   │       │
│       ┌─┴─┐   │     │   ┌─┴─┐     │
│       │ D4 │   │     │   │ A3│    │
│       └───┘ └─┴─┘ └─┴─┘ └───┘     │
│                                     │
│    Scale: D Kurd    Ref: 440 Hz    │
│    [Mallets] [Fingers] [Settings]  │
└─────────────────────────────────────┘
```

### Touch Interactions

| Gesture | Action | Sound Effect |
|---------|--------|-------------|
| **Single tap** | Strike note | Clean tone at touch velocity |
| **Tap force** (3D Touch) | Velocity control | Softer = warmer, harder = brighter |
| **Tap position on tongue** | Timbre variation | Center = fundamental; edge = more overtones |
| **Simultaneous multi-tap** | Chord | Multiple notes ringing together |
| **Long press** | Damped strike | Shorter, muted tone |
| **Tap then hold** | Strike + damp | Natural hand damping technique |
| **Swipe across zones** | Rolling pattern | Quick successive strikes |
| **Two-finger tap** | Octave pair | Same note in two octaves |

### Strike Position Modeling

Real tongue drums sound different depending on where you strike:

```swift
// Map touch position within a tongue zone to timbre
func calculateStrikeTimbre(touchPosition: CGPoint, tongueCenter: CGPoint, tongueSize: CGSize) -> StrikeTimbre {
    // Distance from center of tongue
    let dx = (touchPosition.x - tongueCenter.x) / tongueSize.width
    let dy = (touchPosition.y - tongueCenter.y) / tongueSize.height
    let distFromCenter = sqrt(dx * dx + dy * dy)
    
    // Center strike: more fundamental, less overtones
    // Edge strike: less fundamental, more overtones
    let fundamentalGain = 1.0 - distFromCenter * 0.4
    let overtoneGain = 0.3 + distFromCenter * 0.5
    let brightness = 0.3 + distFromCenter * 0.4
    
    return StrikeTimbre(
        fundamentalGain: fundamentalGain,
        overtoneGain: overtoneGain,
        brightness: brightness
    )
}
```

### Visual Feedback

- **Ripple animation**: Concentric circles emanating from struck tongue
- **Tongue glow**: Warm glow on active tongue, fading with decay
- **Sympathetic glow**: Subtle, dimmer glow on sympathetically vibrating tongues
- **Waveform visualization**: Optional circular waveform around the drum perimeter
- **Particle effects**: Soft particles floating upward from struck note

### Scale Selector

- Swipe or wheel selector for different scales (D Kurd, A Akebono, A Minor, G Major, etc.)
- Real-time re-tuning with smooth transition animation
- Custom scale builder for advanced users
- 440 Hz / 432 Hz toggle

---

## 6. Synthesis Pseudocode (Swift-like)

```swift
class TongueDrumSynthesizer {
    let sampleRate: Float = 44100.0
    
    // === Modal data per note ===
    struct ModalData {
        let frequency: Float        // Hz
        let amplitude: Float        // relative amplitude
        let bandwidth: Float        // Hz (determines decay time)
    }
    
    struct NoteVoice {
        var modes: [ResonantMode]
        var isActive: Bool = false
        var noteFrequency: Float
        
        struct ResonantMode {
            var frequency: Float
            var amplitude: Float
            var bandwidth: Float
            var y1: Float = 0
            var y2: Float = 0
            var a1: Float = 0
            var a2: Float = 0
            var b0: Float = 0
            
            mutating func configure(sampleRate: Float) {
                let r = exp(-Float.pi * bandwidth / sampleRate)
                a1 = -2.0 * r * cos(2.0 * Float.pi * frequency / sampleRate)
                a2 = r * r
                b0 = amplitude
            }
            
            mutating func process(_ input: Float) -> Float {
                let output = b0 * input - a1 * y1 - a2 * y2
                y2 = y1
                y1 = output
                return output
            }
            
            mutating func reset() {
                y1 = 0; y2 = 0
            }
        }
    }
    
    // Voice pool
    var voices: [NoteVoice] = []
    let maxPolyphony: Int = 9  // typical tongue drum note count
    
    // Scale definition
    var currentScale: TongueDrumScale = .dKurd
    
    // Sympathetic resonance
    var sympatheticEnabled: Bool = true
    var sympatheticAmount: Float = 0.03
    
    // Global effects
    var reverbMix: Float = 0.3
    
    enum StrikeType {
        case finger
        case mallet
    }
    
    // === Initialize with scale ===
    init(scale: TongueDrumScale) {
        self.currentScale = scale
        configureVoices(scale: scale)
    }
    
    func configureVoices(scale: TongueDrumScale) {
        voices = scale.notes.map { noteFreq in
            let modes = createModalData(fundamental: noteFreq)
            var voice = NoteVoice(
                modes: modes.map { modal in
                    var mode = NoteVoice.ResonantMode(
                        frequency: modal.frequency,
                        amplitude: modal.amplitude,
                        bandwidth: modal.bandwidth
                    )
                    mode.configure(sampleRate: sampleRate)
                    return mode
                },
                noteFrequency: noteFreq
            )
            return voice
        }
    }
    
    // === Create modal data for a given fundamental ===
    func createModalData(fundamental: Float) -> [ModalData] {
        return [
            // Helmholtz resonance (sub-harmonic warmth)
            ModalData(frequency: 82.0,            amplitude: 0.06, bandwidth: 5.0),
            // Fundamental (strongest)
            ModalData(frequency: fundamental,      amplitude: 1.00, bandwidth: 0.8),
            // 2nd harmonic (tuned octave)
            ModalData(frequency: fundamental * 2.0, amplitude: 0.30, bandwidth: 1.2),
            // 3rd harmonic (tuned octave + fifth)
            ModalData(frequency: fundamental * 3.0, amplitude: 0.10, bandwidth: 2.0),
            // 4th harmonic (subtle)
            ModalData(frequency: fundamental * 4.0, amplitude: 0.03, bandwidth: 3.0),
            // Shell resonance (inharmonic)
            ModalData(frequency: fundamental * 5.8, amplitude: 0.02, bandwidth: 12.0),
        ]
    }
    
    // === Strike a note ===
    func noteOn(
        noteIndex: Int,
        velocity: Float,
        strikeType: StrikeType = .finger,
        strikePosition: Float = 0.5  // 0=edge, 1=center
    ) {
        guard noteIndex < voices.count else { return }
        
        // Generate excitation signal
        let excitation = generateExcitation(
            velocity: velocity,
            type: strikeType,
            position: strikePosition
        )
        
        // Reset and excite the voice
        for i in 0..<voices[noteIndex].modes.count {
            voices[noteIndex].modes[i].reset()
        }
        voices[noteIndex].isActive = true
        
        // Feed excitation into the resonant filters
        exciteVoice(voiceIndex: noteIndex, excitation: excitation)
        
        // Sympathetic excitation of other notes
        if sympatheticEnabled {
            triggerSympathetic(struckIndex: noteIndex, velocity: velocity)
        }
    }
    
    // === Generate excitation impulse ===
    func generateExcitation(
        velocity: Float,
        type: StrikeType,
        position: Float
    ) -> [Float] {
        let durationMs: Float
        let brightness: Float
        
        switch type {
        case .finger:
            durationMs = 4.0 + velocity * 3.0    // 4-7 ms
            brightness = 0.2 + velocity * 0.3 + (1.0 - position) * 0.2
        case .mallet:
            durationMs = 8.0 + velocity * 5.0    // 8-13 ms
            brightness = 0.4 + velocity * 0.3
        }
        
        let samples = Int(sampleRate * durationMs / 1000.0)
        var excitation = [Float](repeating: 0, count: samples)
        
        for i in 0..<samples {
            let t = Float(i) / Float(samples)
            // Raised cosine envelope
            let envelope = 0.5 * (1.0 - cos(2.0 * Float.pi * t))
            // Mix of sine component and noise
            let tonal = sin(Float.pi * t * 2.0)
            let noise = Float.random(in: -1...1)
            let mix = tonal * (1.0 - brightness) + noise * brightness
            excitation[i] = envelope * mix * velocity
        }
        
        return excitation
    }
    
    // === Feed excitation into voice's modal filters ===
    var excitationBuffers: [[Float]] = []
    var excitationPositions: [Int] = []
    
    func exciteVoice(voiceIndex: Int, excitation: [Float]) {
        // Store excitation for processing in audio callback
        if voiceIndex >= excitationBuffers.count {
            excitationBuffers.append(excitation)
            excitationPositions.append(0)
        } else {
            excitationBuffers[voiceIndex] = excitation
            excitationPositions[voiceIndex] = 0
        }
    }
    
    // === Main audio render function ===
    func generateStereoSample() -> (Float, Float) {
        var leftSum: Float = 0.0
        var rightSum: Float = 0.0
        
        for v in 0..<voices.count {
            guard voices[v].isActive else { continue }
            
            // Get excitation sample (if still in excitation phase)
            var excSample: Float = 0.0
            if v < excitationBuffers.count && excitationPositions[v] < excitationBuffers[v].count {
                excSample = excitationBuffers[v][excitationPositions[v]]
                excitationPositions[v] += 1
            }
            
            // Process through all modal filters
            var voiceOutput: Float = 0.0
            for m in 0..<voices[v].modes.count {
                voiceOutput += voices[v].modes[m].process(excSample)
            }
            
            // Stereo placement based on note position
            let pan = calculateNotePan(noteIndex: v, totalNotes: voices.count)
            leftSum += voiceOutput * (1.0 - pan) * 0.707
            rightSum += voiceOutput * (1.0 + pan) * 0.707
            
            // Check if voice has decayed below threshold
            if abs(voiceOutput) < 0.0001 && excSample == 0 {
                voices[v].isActive = false
            }
        }
        
        // Apply gentle reverb/room simulation
        let (revL, revR) = applyReverb(left: leftSum, right: rightSum)
        leftSum = leftSum * (1.0 - reverbMix) + revL * reverbMix
        rightSum = rightSum * (1.0 - reverbMix) + revR * reverbMix
        
        return (leftSum, rightSum)
    }
    
    // === Sympathetic resonance ===
    func triggerSympathetic(struckIndex: Int, velocity: Float) {
        let struckFreq = voices[struckIndex].noteFrequency
        
        for i in 0..<voices.count where i != struckIndex {
            let otherFreq = voices[i].noteFrequency
            let ratio = otherFreq / struckFreq
            
            // Check for harmonic relationships (octave, fifth, fourth, third)
            let harmonicRelations: [(Float, Float)] = [
                (2.0, 0.05),   // octave
                (1.5, 0.03),   // fifth
                (3.0, 0.02),   // octave + fifth
                (4.0, 0.01),   // two octaves
            ]
            
            for (targetRatio, strength) in harmonicRelations {
                if abs(ratio - targetRatio) < 0.05 || abs(1.0/ratio - targetRatio) < 0.05 {
                    let sympVelocity = velocity * strength * sympatheticAmount
                    let gentleExcitation = generateExcitation(
                        velocity: sympVelocity,
                        type: .finger,
                        position: 0.5
                    )
                    exciteVoice(voiceIndex: i, excitation: gentleExcitation)
                    voices[i].isActive = true
                }
            }
        }
    }
    
    // === Pan note in stereo field ===
    func calculateNotePan(noteIndex: Int, totalNotes: Int) -> Float {
        // Circular layout: center note at 0, surrounding notes spread
        if noteIndex == 0 { return 0.0 }  // Ding = center
        let angle = Float(noteIndex - 1) / Float(totalNotes - 1) * 2.0 * Float.pi
        return sin(angle) * 0.6  // max 60% panning
    }
    
    // Simple reverb placeholder
    func applyReverb(left: Float, right: Float) -> (Float, Float) {
        // Use AVAudioUnitReverb in real implementation
        return (left * 0.3, right * 0.3)
    }
}

// === Scale Definitions ===
enum TongueDrumScale {
    case dKurd
    case aAkebono
    case aMinor
    case gMajor
    case dCelticMinor
    
    var notes: [Float] {
        switch self {
        case .dKurd:
            return [146.83, 220.00, 233.08, 261.63, 293.66, 329.63, 349.23, 392.00, 440.00]
        case .aAkebono:
            return [220.00, 246.94, 261.63, 329.63, 349.23, 440.00, 493.88, 523.25]
        case .aMinor:
            return [220.00, 246.94, 261.63, 293.66, 329.63, 349.23, 392.00, 440.00]
        case .gMajor:
            return [196.00, 220.00, 246.94, 261.63, 293.66, 329.63, 369.99, 392.00]
        case .dCelticMinor:
            return [146.83, 164.81, 174.61, 196.00, 220.00, 261.63, 293.66]
        }
    }
    
    var name: String {
        switch self {
        case .dKurd: return "D Kurd (D Minor)"
        case .aAkebono: return "A Akebono (Japanese)"
        case .aMinor: return "A Minor"
        case .gMajor: return "G Major"
        case .dCelticMinor: return "D Celtic Minor"
        }
    }
}

// === 432 Hz Variant ===
extension TongueDrumScale {
    var notes432: [Float] {
        return notes.map { $0 * (432.0 / 440.0) }
    }
}

// === Usage ===
let drum = TongueDrumSynthesizer(scale: .dKurd)

// Strike the center note (D3) with finger at medium velocity
drum.noteOn(noteIndex: 0, velocity: 0.7, strikeType: .finger, strikePosition: 0.8)

// Strike A3 with mallet at high velocity
drum.noteOn(noteIndex: 1, velocity: 0.9, strikeType: .mallet, strikePosition: 0.5)
```

### Key Implementation Notes for iOS (AVAudioEngine)

1. **AVAudioSourceNode** for real-time modal synthesis rendering
2. **Polyphony**: All 8-9 notes can ring simultaneously (plus sympathetic), so plan for 9+ active voices
3. **CPU optimization**: Pre-compute filter coefficients (`a1`, `a2`, `b0`) at note configuration, not per sample
4. **Latency**: Target < 10ms — use 128-frame buffer for responsive tapping
5. **Built-in reverb**: Use `AVAudioUnitReverb` with small room preset for natural body resonance
6. **Haptic feedback**: Use Core Haptics for subtle "thud" on strike (UIImpactFeedbackGenerator)
7. **Scale switching**: Can be done by reconfiguring filter coefficients — no need to recreate voices
8. **432 Hz mode**: Simply multiply all frequencies by 432/440 and reconfigure

---

## References

1. "Experimental characterization of the steel tongue drum" — ResearchGate, 2021
2. "The Science Behind the Sound: Understanding Acoustics of the Tongue Drum" — madeintheshademusic.com
3. "Modal Synthesis of the Handpan Sound" — Carrie Xu, MUMT307 project (McGill)
4. "Analysis and Synthesis of the Handpan Sound" — Eyal Alon, University of York, 2015
5. "Modal Analysis of a Tongue Drum" — Jonathan South, jsouthaudio.com, 2020
6. Meinl Sonic Energy — Steel Tongue Drum product catalog & specifications
7. Sweetwater — Meinl Octave Steel Tongue Drum D Kurd specifications
8. Sound Healing LAB — therapeutic applications documentation
9. Rossing & Isma — "Tuning of Steelpan/Handpan instruments" research
