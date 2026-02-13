# Shruti Box & Tanpura — Comprehensive Research for Sound Healing iOS App

> **Last Updated:** 2026-02-08
> **Purpose:** Digital synthesis reference for iOS app implementation

---

## 1. Physical Acoustics

### Shruti Box: How It Produces Sound

The shruti box is an Indian drone instrument derived from the harmonium, stripped of its keyboard to focus exclusively on continuous drone production. It's a **free-reed aerophone**.

**Construction:**
- **Body:** Rectangular wooden box (teak or pine), typically 12.5" × 16.25"
- **Bellows:** Hand-operated dual bellows system — one side inhales air, the other stores it under spring pressure
- **Reeds:** 12–13 brass or copper free reeds, tuned to a chromatic scale (typically C3–C4 or C2–C3)
- **Valves/Stops:** Small levers/switches on the front panel open/close individual reed chambers
- **Air chamber:** Internal wooden resonating cavity

**Sound Production Mechanism:**
1. Player pumps bellows by hand, compressing and expanding to maintain steady airflow
2. Air flows from bellows into the internal chamber at positive pressure
3. When a valve is opened, pressurized air passes over the corresponding free reed
4. The reed vibrates freely through its frame slot — never sealing the gap, but periodically modulating airflow
5. Periodic air pulses are emitted, creating the sound waveform
6. The wooden body acts as a resonating chamber, shaping the timbre

### Free Reed Physics

A free reed is fundamentally an **acoustic negative resistance oscillator**:

**Oscillation Mechanism:**
1. Wind pressure pushes the reed tongue toward the frame aperture
2. As the tongue approaches the frame, the Bernoulli effect accelerates air through the narrowing gap
3. This creates a pressure drop that further pulls the tongue through
4. An impulse occurs at mid-cycle as the tongue passes through the frame slot
5. The tongue's elasticity (spring force) and inertia carry it through to the other side
6. The tongue then returns freely, creating a nearly sinusoidal oscillation
7. The cycle repeats — each pass produces an air pulse

**Key Physics Properties:**
- **High Q-factor:** Free reeds oscillate almost isochronously (frequency nearly independent of amplitude)
- **Frequency stability:** Pitch barely changes with blowing pressure (unlike many wind instruments)
- **Self-sustaining:** As long as sufficient airflow exists, oscillation continues
- **Rich harmonics:** Both odd and even harmonics are strongly present
- **Three-region waveform:** Each cycle shows two air-pulse regions + a high-frequency burst (~12.5 kHz) from the impulse excitation of higher-order tongue modes

**Frequency Spectrum:**
- Strong fundamental frequency
- Rich harmonic series (both odd and even harmonics)
- Considerable acoustic energy extends well beyond human hearing
- Spectrum varies with blowing pressure:
  - Low pressure → fewer harmonics, softer, more sine-like
  - Higher pressure → more harmonics, brighter, buzzier
- The wooden enclosure acts as a resonant filter, emphasizing certain frequency bands

### Tanpura: The Original Drone Instrument

The tanpura is a 4-string (sometimes 5) plucked instrument that provides the harmonic drone foundation in Indian classical music. Understanding it is essential because shruti box synthesis should capture its qualities.

**Construction:**
- **Body (tumba):** Dried gourd (Miraj style) or solid wood (Tanjore style)
- **Neck (dand):** Long hollow wooden neck, ~1 meter for male instruments
- **Bridge:** Table-shaped, curved-top bridge — critical for the jivari effect
- **Strings:** 4 strings — typically steel wire (for octave strings) and brass/bronze (for tonic/fifth strings)
- **Cotton thread (jivari):** A thread placed between string and bridge surface

**Standard Tuning (4 strings):**
- Pa-Sa-Sa-SA (5-8-8-1): Fifth, middle tonic, middle tonic, low tonic
- Or Ma-sa-sa-Sa (4-8-8-1): Fourth (when Pa is omitted in raga)
- Five-string: Pa-Ni-sa-sa-SA (5-7-8-8-1)

### The Jivari Effect — Key to Tanpura Sound

The jivari (jawari) is what makes the tanpura unique and is the most important element to model for synthesis.

**Mechanism:**
1. When a string is plucked, it vibrates with large amplitude
2. The vibrating string makes intermittent grazing contact with the curved bridge surface
3. As amplitude decreases, the contact point gradually shifts ("creeps") up the bridge slope
4. This shifting contact creates a dynamic, time-varying filtering effect
5. The cotton thread shifts the contact sequence, changing harmonic content
6. Result: A sustained "buzzing" sound with overtones that appear, swell, and recede over 3–10 seconds

**Acoustic Result:**
- Rich, shimmering overtone texture
- Harmonics appear to "bloom" — rising and falling in prominence
- Creates perception of movement within a static drone
- Certain harmonics resonate with focused clarity at different moments
- The overall sound is described as "prismatic" — scattering a fundamental tone into its harmonic spectrum

---

## 2. Real-World Frequency Data

### Shruti Box Standard Range

Standard shruti box: 13 notes, one chromatic octave (C3–C4, 440 Hz reference)

| Note | Frequency (Hz) | Solfège (Indian) | Common Drone Use |
|------|----------------|-------------------|-------------------|
| C3 | 130.81 | Sa (low) | ✅ Primary tonic |
| C#3 | 138.59 | Re komal | |
| D3 | 146.83 | Re | |
| D#3 | 155.56 | Ga komal | |
| E3 | 164.81 | Ga | |
| F3 | 174.61 | Ma | ✅ Fourth drone |
| F#3 | 185.00 | Ma tivra | |
| G3 | 196.00 | Pa | ✅ Fifth drone |
| G#3 | 207.65 | Dha komal | |
| A3 | 220.00 | Dha | |
| A#3 | 233.08 | Ni komal | |
| B3 | 246.94 | Ni | |
| C4 | 261.63 | Sa (upper) | ✅ Upper tonic |

**Common Sa Positions by Voice Type:**
- Male vocalists: C3, C#3, D3 (lower fundamental)
- Female vocalists: F3, G3, A3 (a fifth higher typically)
- The tonic (Sa) can be set at any note — the app should allow this

### Tanpura Tuning Frequencies

From ragajunglism.org — standard Sa-Pa drone reference frequencies:

| Sa Note | Sa Frequency (Hz) | Pa Frequency (Hz) | Character |
|---------|--------------------|--------------------|-----------|
| F#2 | ~92.5 | ~138.6 | Very deep male |
| G2 | ~98.0 | ~146.8 | Deep male |
| G#2 | ~103.8 | ~155.6 | Male low |
| A2 | 110.0 | 164.8 | Standard male |
| A#2 | ~116.5 | ~174.6 | Male mid |
| B2 | ~123.5 | ~185.0 | Male upper |
| C3 | ~130.8 | ~196.0 | Common male |
| C#3 | ~138.6 | ~207.7 | Male high |
| D3 | ~146.8 | ~220.0 | Male high |
| D#3 | ~155.6 | ~233.1 | Female low |
| E3 | ~164.8 | ~246.9 | Female common |
| F3 | ~174.6 | ~261.6 | Female standard |

### 432 Hz vs 440 Hz Tuning

Many sound healing practitioners prefer 432 Hz tuning. The app should support both:
- **440 Hz:** Standard concert pitch
- **432 Hz:** "Healing frequency" — popular in sound therapy
- Some shruti boxes are specifically tuned to 432 Hz
- Some to 320 Hz (specialized low-frequency models)

---

## 3. Digital Synthesis Approaches

### Approach A: Physical Modeling (Recommended for Tanpura/Jivari)

#### Modified Karplus-Strong with Bridge Interaction

The tanpura's jivari effect requires modeling string-bridge collision. The research by Van Walstijn et al. (DAFx 2016, Queen's University Belfast) provides the gold standard approach:

**Architecture:**
```
[Pluck Excitation] → [Delay Line (string)] → [Loss Filter] → [Output]
       ↑                     ↓         ↑
       └──── [Feedback] ←────┘         │
                                        │
                              [Bridge Collision Model]
                                        │
                              [Cotton Thread Offset]
```

**Key Components:**
1. **Stiff string model:** Delay line with allpass dispersion filters for inharmonicity
2. **Frequency-dependent loss:** Low-pass filter in feedback loop (higher harmonics decay faster)
3. **Bridge collision:** Non-linear contact force when string position touches bridge curve
4. **Cotton thread:** Shifts the contact sequence, modifying harmonic bloom pattern
5. **Body resonance:** Modal filter simulating gourd/wood body

**Van Walstijn Model Summary:**
- Uses **modal expansion** of string dynamics for computational efficiency
- Incorporates distributed (not point) bridge contact
- Provably stable time-stepping scheme with exact modal parameters
- Key feature: reproduces the "precursor" — a band of overtones that decreases in frequency as vibrations decay
- Won 4th Prize in DAFx-16 Best Paper Awards

### Approach B: Subtractive/Additive Hybrid (Recommended for Shruti Box)

For the shruti box's free-reed drone, use a hybrid approach:

**Architecture:**
```
[Reed Oscillator] → [Waveshaper] → [Body Resonance Filter] → [Output]
       ↑                                      ↑
  [Bellows Pressure]                  [Wood Box IR/Filter]
```

**Reed Oscillator Model:**
1. Generate a near-sinusoidal waveform at the reed frequency
2. Apply waveshaping based on bellows pressure to add harmonics
3. At low pressure: nearly sinusoidal (few harmonics)
4. At high pressure: more square-wave-like (rich harmonics)
5. The waveform shape determines timbre

**Based on DAFx 2023 Harmonium Paper (Bhattacharjee et al.):**
- Source-filter structure
- Reed sound synthesized using a physical model with bellows pressure as control parameter
- Wooden enclosure modeled as a filter (can be estimated from recorded notes)
- Allows different timbres by changing the enclosure filter

### Approach C: Combined Shruti-Tanpura Hybrid

For the app, combine both instruments conceptually:
- **Shruti mode:** Continuous drone, bellows-pressure-controlled
- **Tanpura mode:** Plucked strings with jivari bloom
- **Hybrid:** Shruti drone as background + tanpura bloom overlay

### ADSR Envelopes

**Shruti Box (Free Reed):**
- **Attack:** 50–200 ms (reed needs time to start oscillating)
- **Decay:** N/A (continuous sound)
- **Sustain:** Indefinite (while bellows provide air)
- **Release:** 100–500 ms (reed oscillation dies as air stops)
- Bellows pumping creates natural volume swells — model as slow LFO on amplitude

**Tanpura (Plucked String):**
- **Attack:** 5–20 ms (string pluck)
- **Decay:** 3000–10000 ms (long, slow decay — varies by string)
- **Sustain:** N/A (decaying)
- **Release:** The string continues ringing until amplitude is negligible
- **Jivari envelope:** Harmonics bloom and recede over the decay period
  - Peak harmonic richness: ~200–500 ms after pluck
  - Gradual harmonic simplification as amplitude decreases
  - Last 1–2 seconds: primarily fundamental remaining

---

## 4. Sound Healing Applications

### Therapeutic Properties of Drone

**Brainwave Entrainment:**
- Continuous drone stimulates **alpha** (8–12 Hz) and **theta** (4–8 Hz) brainwave states
- The rich harmonic content provides multiple frequency anchors for the brain
- Beatless, stable pitch creates deep relaxation response
- Reduced auditory processing demands → meditative states

**Chakra Associations (Shruti Box Notes):**
| Note | Chakra | Location | Quality |
|------|--------|----------|---------|
| C | Root (Muladhara) | Base of spine | Grounding, safety |
| D | Sacral (Svadhisthana) | Below navel | Creativity, emotion |
| E | Solar Plexus (Manipura) | Stomach | Power, confidence |
| F | Heart (Anahata) | Chest | Love, compassion |
| G | Throat (Vishuddha) | Throat | Communication, truth |
| A | Third Eye (Ajna) | Forehead | Intuition, insight |
| B | Crown (Sahasrara) | Top of head | Connection, transcendence |

### Practitioner Techniques

**Shruti Box Techniques:**
1. **Single note drone:** Open one note (typically Sa) for foundational stability
2. **Sa-Pa drone:** Open tonic + fifth for rich, full drone (most common)
3. **Sa-Ma drone:** Open tonic + fourth for raga-specific work
4. **Chord drones:** Open 2–4 notes simultaneously for richer harmonic textures
5. **Bellows breathing:** Sync bellows pumping with breath for somatic entrainment
6. **Dynamic swells:** Vary bellows pressure for emotional waves
7. **Note transitions:** Slowly open/close notes during session for harmonic movement

**Tanpura Techniques:**
1. **Cyclic plucking:** Regular, slow plucking pattern (Pa-sa-sa-Sa repeat)
2. **Let harmonics bloom:** Allow full decay before replucking
3. **Jivari adjustment:** Subtle tuning creates different harmonic colors per raga
4. **Continuous drone carpet:** Overlapping plucks create seamless background

### Session Structure

**Opening (5–10 min):**
- Begin with Sa drone only — establishes tonal center
- Gradually increase bellows pressure / volume
- Let participants settle and sync breathing

**Development (15–30 min):**
- Add Pa (fifth) for richer harmonic field
- Introduce gentle bellows swells
- Optional: layer tanpura plucking over shruti drone
- Can combine with voice/singing bowls over drone

**Peak (5–10 min):**
- Full harmonic palette — multiple notes open
- Maximum bellows energy
- Tanpura plucking at peak intensity

**Resolution (10–15 min):**
- Gradually close notes back to Sa only
- Reduce bellows pressure
- Slower tanpura plucking with longer gaps
- Final Sa drone fades to silence

---

## 5. Interaction Design for iOS

### Primary Gesture: Bellows Simulation

**Two-thumb bellows pump:**
- Place thumbs at bottom of screen
- Push/pull motion (up/down swipe) simulates bellows compression
- Faster pumping = more air pressure = louder, brighter drone
- Smooth, slow pumping = gentle, warm drone
- Provides haptic feedback for organic feel

**Alternative: Auto-bellows mode**
- Toggle for continuous drone without manual pumping
- Slider controls steady-state pressure level
- Optional breathing LFO for natural volume swells

### Note Selection

**Reed valve toggles:**
- 13 toggle buttons (C to C) arranged like a shruti box front panel
- Tap to open/close individual notes
- Visual indicator: lit/glowing when open
- Drag across multiple notes to open a chord quickly

**Preset chords:**
- Sa only (tonic)
- Sa-Pa (tonic + fifth) — most common
- Sa-Ma (tonic + fourth)
- Full chord presets for specific ragas
- Custom chord memory slots

### Tanpura Mode Gestures

**String plucking:**
- 4 vertical string representations on screen
- Swipe down on a string to pluck
- Pluck velocity → initial amplitude
- Auto-pluck mode: cyclic plucking at adjustable tempo (40–80 BPM typical)

### Advanced Controls

**Jivari slider:**
- 0.0 = clean, no bridge buzz
- 0.5 = moderate buzz (default)
- 1.0 = maximum harmonic richness
- Maps to bridge curvature / contact parameters in synthesis model

**Sa pitch selector:**
- Wheel/picker to set tonic note (C through B)
- Fine-tune slider for ±50 cents
- 440 Hz / 432 Hz toggle

**Drone intensity:**
- Volume + harmonic richness combined control
- Gentle → Full
- Maps to both amplitude and spectral brightness

### Visual Design

**Shruti Box Mode:**
- Top-down view of wooden box with visible reed valves
- Bellows animation at bottom responding to pump gesture
- Reed valves glow warm amber/gold when open
- Subtle particle/smoke effects showing air flow
- Dark wood textures with brass accents

**Tanpura Mode:**
- Elegant string view with visible bridge
- Strings vibrate when plucked (animated sine displacement)
- Jivari glow/shimmer at bridge contact point
- Harmonic visualization: spectral waterfall or mandala showing blooming overtones

---

## 6. Synthesis Pseudocode (Swift-like)

```swift
// MARK: - Shruti Box Synthesizer

class ShrutiBoxSynth {
    let sampleRate: Float = 44100.0
    var reeds: [FreeReed] = []
    var bellowsPressure: Float = 0.0  // 0.0–1.0
    var bodyFilter: BiquadFilter      // models wooden enclosure
    
    struct FreeReed {
        let frequency: Float           // Hz
        var isOpen: Bool = false        // valve state
        var phase: Float = 0.0
        var amplitude: Float = 0.0
        var targetAmplitude: Float = 0.0
        let attackRate: Float = 0.005   // per sample
        let releaseRate: Float = 0.002  // per sample
    }
    
    // Standard C3–C4 chromatic tuning at 440 Hz
    static let standardFrequencies: [Float] = [
        130.81, 138.59, 146.83, 155.56, 164.81, 174.61, 185.00,
        196.00, 207.65, 220.00, 233.08, 246.94, 261.63
    ]
    
    // Note names for UI
    static let noteNames = ["C", "C#", "D", "D#", "E", "F", "F#",
                            "G", "G#", "A", "A#", "B", "C"]
    
    init(baseFrequency: Float = 130.81, tuningHz: Float = 440.0) {
        // Scale frequencies if using non-standard tuning
        let tuningRatio = tuningHz / 440.0
        
        for freq in Self.standardFrequencies {
            let reed = FreeReed(frequency: freq * tuningRatio)
            reeds.append(reed)
        }
        
        // Wooden body resonance filter
        bodyFilter = BiquadFilter(type: .peak, frequency: 400.0, q: 2.0, gain: 3.0)
    }
    
    func openReed(_ index: Int) {
        guard index < reeds.count else { return }
        reeds[index].isOpen = true
    }
    
    func closeReed(_ index: Int) {
        guard index < reeds.count else { return }
        reeds[index].isOpen = false
    }
    
    // Called ~60 Hz from bellows gesture handler
    func setBellowsPressure(_ pressure: Float) {
        bellowsPressure = max(0, min(1, pressure))
    }
    
    // Free reed waveform generator
    // At low pressure: near-sinusoidal
    // At high pressure: more harmonics (quasi-square wave)
    func reedWaveform(phase: Float, pressure: Float) -> Float {
        let sine = sin(phase)
        
        // Waveshaping: blend sine → clipped (adds harmonics with pressure)
        let clipAmount = pressure * 0.8
        let shaped = sine + clipAmount * (sine * sine * sine)
        
        // Normalize
        return shaped / (1.0 + clipAmount)
    }
    
    func renderSamples(buffer: inout [Float], frameCount: Int) {
        let dt = 1.0 / sampleRate
        
        for frame in 0..<frameCount {
            var sample: Float = 0.0
            
            for i in 0..<reeds.count {
                // Determine target amplitude
                if reeds[i].isOpen && bellowsPressure > 0.05 {
                    reeds[i].targetAmplitude = bellowsPressure
                } else {
                    reeds[i].targetAmplitude = 0.0
                }
                
                // Smooth amplitude envelope (attack/release)
                if reeds[i].amplitude < reeds[i].targetAmplitude {
                    reeds[i].amplitude += reeds[i].attackRate
                    reeds[i].amplitude = min(reeds[i].amplitude, reeds[i].targetAmplitude)
                } else if reeds[i].amplitude > reeds[i].targetAmplitude {
                    reeds[i].amplitude -= reeds[i].releaseRate
                    reeds[i].amplitude = max(reeds[i].amplitude, 0.0)
                }
                
                guard reeds[i].amplitude > 0.001 else { continue }
                
                // Phase accumulation
                reeds[i].phase += 2 * .pi * reeds[i].frequency * dt
                if reeds[i].phase > 2 * .pi {
                    reeds[i].phase -= 2 * .pi
                }
                
                // Generate reed waveform with pressure-dependent harmonics
                let reedSample = reedWaveform(
                    phase: reeds[i].phase,
                    pressure: bellowsPressure
                ) * reeds[i].amplitude
                
                sample += reedSample
            }
            
            // Apply body resonance filter
            sample = bodyFilter.process(sample)
            
            // Gentle saturation (warm analog feel)
            sample = tanh(sample * 0.4)
            
            buffer[frame] = sample
        }
    }
}

// MARK: - Tanpura Synthesizer (Karplus-Strong + Jivari)

class TanpuraSynth {
    let sampleRate: Float = 44100.0
    var strings: [TanpuraString] = []
    var bodyResonance: ModalResonator
    
    struct TanpuraString {
        let frequency: Float
        var delayLine: [Float]         // Circular buffer
        var delayIndex: Int = 0
        let delayLength: Int           // samples = sampleRate / frequency
        var amplitude: Float = 0.0
        var isPlucked: Bool = false
        
        // Jivari parameters
        var jivariAmount: Float = 0.5  // 0 = clean, 1 = max buzz
        var bridgeCurve: Float = 0.3   // Bridge curvature parameter
        var threadPosition: Float = 0.2 // Cotton thread offset
        
        // Loss filter state
        var prevSample: Float = 0.0
        let damping: Float = 0.998     // Decay rate (close to 1 = long sustain)
    }
    
    init(saFrequency: Float = 130.81) {
        // Standard Sa-Pa tuning: Pa, sa, sa, SA
        let pa = saFrequency * 1.5       // Perfect fifth
        let saMiddle = saFrequency * 2.0  // Middle octave
        let saLow = saFrequency           // Low octave
        
        let frequencies = [pa, saMiddle, saMiddle, saLow]
        
        for freq in frequencies {
            let delayLen = Int(sampleRate / freq)
            let string = TanpuraString(
                frequency: freq,
                delayLine: [Float](repeating: 0, count: delayLen),
                delayLength: delayLen
            )
            strings.append(string)
        }
        
        // Body resonance (gourd/wood modes)
        bodyResonance = ModalResonator(modes: [
            (freq: 180.0, q: 15.0, gain: 0.3),
            (freq: 350.0, q: 10.0, gain: 0.5),
            (freq: 520.0, q: 8.0, gain: 0.2),
            (freq: 750.0, q: 12.0, gain: 0.15)
        ])
    }
    
    func pluck(stringIndex: Int, velocity: Float = 0.7) {
        guard stringIndex < strings.count else { return }
        strings[stringIndex].isPlucked = true
        strings[stringIndex].amplitude = velocity
        
        // Initialize delay line with filtered noise (pluck excitation)
        for i in 0..<strings[stringIndex].delayLength {
            let noise = Float.random(in: -1...1) * velocity
            // Low-pass the noise for warmer pluck
            let filtered = noise * 0.7 + (i > 0 ? strings[stringIndex].delayLine[i-1] * 0.3 : 0)
            strings[stringIndex].delayLine[i] = filtered
        }
    }
    
    // Auto-pluck in traditional pattern: Pa-sa-sa-SA
    func autoPluckCycle(time: Float, bpm: Float = 50.0) {
        let beatInterval = 60.0 / bpm
        let cycleTime = fmod(time, beatInterval * 4)
        
        let beats = [0, 1, 2, 3]  // Pa, sa, sa, SA
        for (i, beat) in beats.enumerated() {
            let beatTime = Float(beat) * Float(beatInterval)
            if abs(cycleTime - beatTime) < 0.01 {
                pluck(stringIndex: i, velocity: i == 3 ? 0.8 : 0.6)
            }
        }
    }
    
    // Jivari bridge interaction model
    func applyJivari(sample: Float, string: inout TanpuraString) -> Float {
        guard string.jivariAmount > 0 else { return sample }
        
        // Bridge curve: string grazes the curved bridge surface
        let bridgeHeight = string.bridgeCurve * 0.01  // Normalized bridge height
        let threadOffset = string.threadPosition * bridgeHeight
        
        // When string position is near bridge surface, apply non-linear contact
        let contactThreshold = bridgeHeight + threadOffset
        
        if abs(sample) < contactThreshold * string.jivariAmount {
            // String is in contact with bridge — creates buzz
            // Apply subtle waveshaping to add harmonic content
            let contactDepth = 1.0 - (abs(sample) / contactThreshold)
            let buzzAmount = contactDepth * string.jivariAmount * 0.3
            
            // Asymmetric soft clipping (bridge contact is one-sided)
            let jivariSample = sample + buzzAmount * (sample * sample) * sign(sample)
            
            // Add subtle high-frequency content (bridge resonance)
            let highFreqNoise = Float.random(in: -0.001...0.001) * buzzAmount
            
            return jivariSample + highFreqNoise
        }
        
        return sample
    }
    
    func renderSamples(buffer: inout [Float], frameCount: Int) {
        for frame in 0..<frameCount {
            var sample: Float = 0.0
            
            for i in 0..<strings.count {
                guard strings[i].isPlucked else { continue }
                
                // Read from delay line
                let readIndex = strings[i].delayIndex
                let currentSample = strings[i].delayLine[readIndex]
                
                // Karplus-Strong averaging filter (loss filter)
                let nextIndex = (readIndex + 1) % strings[i].delayLength
                let nextSample = strings[i].delayLine[nextIndex]
                
                // Low-pass filter: average of adjacent samples, with damping
                var filtered = (currentSample + nextSample) * 0.5 * strings[i].damping
                
                // Apply stiffness (allpass filter for inharmonicity)
                let stiffness: Float = 0.002  // Small amount for tanpura
                filtered = filtered + stiffness * (filtered - strings[i].prevSample)
                strings[i].prevSample = filtered
                
                // Apply jivari bridge interaction
                let jivariOutput = applyJivari(sample: filtered, string: &strings[i])
                
                // Write back to delay line
                strings[i].delayLine[readIndex] = jivariOutput
                
                // Advance delay line pointer
                strings[i].delayIndex = nextIndex
                
                // Deactivate if energy is gone
                if abs(jivariOutput) < 0.0001 {
                    strings[i].isPlucked = false
                }
                
                sample += currentSample
            }
            
            // Apply body resonance
            sample = bodyResonance.process(sample)
            
            // Soft limiting
            sample = tanh(sample * 0.6)
            
            buffer[frame] = sample
        }
    }
}

// MARK: - Combined Shruti + Tanpura Controller

class DroneSynthController {
    let shrutiSynth: ShrutiBoxSynth
    let tanpuraSynth: TanpuraSynth
    
    var shrutiMix: Float = 0.7      // How much shruti box in mix
    var tanpuraMix: Float = 0.3     // How much tanpura in mix
    var masterVolume: Float = 0.8
    
    // Modes
    enum DroneMode {
        case shrutiOnly          // Pure shruti box drone
        case tanpuraOnly         // Pure tanpura plucked drone
        case hybrid              // Both layered
        case shrutiWithJivari    // Shruti drone with tanpura-like jivari buzz
    }
    
    var mode: DroneMode = .hybrid
    
    init(saNote: String = "C", octave: Int = 3, tuningHz: Float = 440.0) {
        let saFreq = Self.noteToFrequency(note: saNote, octave: octave, 
                                           tuningHz: tuningHz)
        shrutiSynth = ShrutiBoxSynth(baseFrequency: saFreq, tuningHz: tuningHz)
        tanpuraSynth = TanpuraSynth(saFrequency: saFreq)
    }
    
    static func noteToFrequency(note: String, octave: Int, 
                                 tuningHz: Float = 440.0) -> Float {
        let noteMap: [String: Int] = [
            "C": -9, "C#": -8, "D": -7, "D#": -6, "E": -5,
            "F": -4, "F#": -3, "G": -2, "G#": -1, "A": 0,
            "A#": 1, "B": 2
        ]
        guard let semitones = noteMap[note] else { return tuningHz }
        let midiNote = (octave + 1) * 12 + semitones + 9
        return tuningHz * pow(2.0, Float(midiNote - 69) / 12.0)
    }
    
    func renderMixedBuffer(buffer: inout [Float], frameCount: Int) {
        var shruti = [Float](repeating: 0, count: frameCount)
        var tanpura = [Float](repeating: 0, count: frameCount)
        
        switch mode {
        case .shrutiOnly:
            shrutiSynth.renderSamples(buffer: &shruti, frameCount: frameCount)
            buffer = shruti.map { $0 * masterVolume }
            
        case .tanpuraOnly:
            tanpuraSynth.renderSamples(buffer: &tanpura, frameCount: frameCount)
            buffer = tanpura.map { $0 * masterVolume }
            
        case .hybrid:
            shrutiSynth.renderSamples(buffer: &shruti, frameCount: frameCount)
            tanpuraSynth.renderSamples(buffer: &tanpura, frameCount: frameCount)
            for i in 0..<frameCount {
                buffer[i] = (shruti[i] * shrutiMix + tanpura[i] * tanpuraMix) * masterVolume
            }
            
        case .shrutiWithJivari:
            shrutiSynth.renderSamples(buffer: &shruti, frameCount: frameCount)
            // Apply jivari-like post-processing to shruti signal
            for i in 0..<frameCount {
                buffer[i] = applyJivariPostProcess(shruti[i]) * masterVolume
            }
        }
    }
    
    // Apply tanpura-like harmonic bloom to any signal
    func applyJivariPostProcess(_ sample: Float) -> Float {
        // Subtle asymmetric waveshaping for jivari character
        let shaped = sample + 0.15 * sample * sample * sign(sample)
        return shaped
    }
}

// MARK: - Bellows Gesture Recognizer

class BellowsGestureController {
    var currentPressure: Float = 0.0
    var targetPressure: Float = 0.0
    let smoothingRate: Float = 0.1
    
    // Called from touch handler
    func onBellowsGesture(panTranslation: Float, velocity: Float) {
        // Map vertical pan gesture to bellows pressure
        // Pushing down = compress bellows = more pressure
        let normalizedForce = abs(velocity) / 1000.0  // Normalize
        targetPressure = min(normalizedForce, 1.0)
    }
    
    // Auto-bellows with breathing LFO
    func autoBellows(time: Float, breathRate: Float = 0.15) -> Float {
        // Natural breathing pattern
        let breath = sin(time * breathRate * 2 * .pi)
        let pressure = 0.5 + breath * 0.2  // Oscillate around 0.5
        return max(0.1, pressure)  // Never fully stop
    }
    
    // Called per frame (~60 Hz)
    func update() -> Float {
        currentPressure += (targetPressure - currentPressure) * smoothingRate
        return currentPressure
    }
}
```

---

## 7. Implementation Notes

### Performance Considerations

**Karplus-Strong (Tanpura):**
- 4 strings × 1 delay line each = very CPU efficient
- Delay line sizes: sampleRate/freq = 44100/130 ≈ 339 samples (small!)
- Jivari processing adds minimal overhead
- Total: extremely lightweight — suitable for background processing

**Free Reed (Shruti):**
- 13 potential oscillators (but typically only 1–4 active)
- Waveshaping is cheap (polynomial operations)
- Body filter: single biquad = negligible cost
- Total: very lightweight

### Audio Pipeline
```
AVAudioEngine
  └── AVAudioSourceNode (custom render)
        ├── ShrutiBoxSynth.render()
        ├── TanpuraSynth.render()
        ├── Mix + Master
        └── Optional: Reverb (AVAudioUnitReverb)
```

### Realism Enhancements

**Shruti Box:**
- Add slight random pitch variation per reed (±3 cents) — reeds are never perfectly tuned
- Model bellows air pressure variation (slight flutter/wobble)
- Add subtle mechanical noise (valve clicks, bellows creak) as very quiet samples
- Double-reed option: two reeds per note, slightly detuned, for chorus/beating effect

**Tanpura:**
- Implement proper string stiffness (allpass dispersion) for realistic inharmonicity
- Model string coupling through the bridge (sympathetic resonance)
- Add subtle body resonance with 3–5 modal frequencies
- Jivari "bloom" should evolve over 3–10 seconds per pluck
- Consider pre-computing jivari patterns and using lookup tables for efficiency

### Key UX Decisions
1. **Auto-play vs manual:** Most sound healing practitioners want "set and forget" — auto-bellows and auto-pluck should be default
2. **Drone presets:** Sa, Sa-Pa, Sa-Ma, and custom chord presets
3. **Session timer:** Built-in timer with gradual volume fade
4. **Background audio:** Must continue playing when app is backgrounded
5. **Low-latency for manual mode:** 256 sample buffer when user is actively pumping bellows

---

## References & Sources

1. **Wikipedia — Tanpura:** https://en.wikipedia.org/wiki/Tanpura
2. **Wikipedia — Shruti Box:** https://en.wikipedia.org/wiki/Shruti_box
3. **Ragajunglism — Tanpura Samples & Frequencies:** https://ragajunglism.org/ragas/tanpuras/
4. **Van Walstijn et al. — "A Real-Time Synthesis Oriented Tanpura Model" (DAFx 2016):** https://pure.qub.ac.uk/en/publications/a-real-time-synthesis-oriented-tanpura-model
5. **Bhattacharjee et al. — "Physically inspired signal model for harmonium sound synthesis" (DAFx 2023):** https://www.dafx.de/paper-archive/2023/DAFx23_paper_47.pdf
6. **Colin Pykett — "The Physics of Free Reeds":** https://www.colinpykett.org.uk/physics-of-free-reed-wind-instruments.htm
7. **Organology — Shruti Box:** https://organology.net/instrument/shruti-box/
8. **Shruti Sounds:** https://www.shrutisounds.com/the-shruti-box
9. **Brooklyn Healing Arts — Shruti Boxes:** https://brooklynhealingarts.com
10. **Didge Project — Shruti Box:** https://www.didgeproject.com/product/shruti-box-monoj-kumar-sardar-bros-3-sizes-available/
11. **Pisharody & Gupta — "Experimental investigations of tanpura acoustics":** http://home.iitk.ac.in/~ag/papers/tanpura.pdf
12. **Datta et al. — "Acoustical Analysis of the Tanpura" (2019, Springer):** doi:10.1007/978-981-13-2610-3
13. **Karplus-Strong Algorithm — Stanford CCRMA:** https://ccrma.stanford.edu/~jos/pasp/Karplus_Strong_Algorithm.html
