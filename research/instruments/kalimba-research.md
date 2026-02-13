# Kalimba — Comprehensive Research for Sound Healing iOS App

## 1. Physical Acoustics

### How the Kalimba Produces Sound

The kalimba (African thumb piano) is a **lamellophone** — an instrument that produces sound by plucking flexible metal tongues (tines/lamellae). It belongs to the idiophone family in the Hornbostel-Sachs classification system.

**Sound production mechanism:**
1. A metal tine is deflected by the player's thumb and released
2. The tine vibrates as a **clamped-free beam** (cantilever) with an intermediate support point (bridge)
3. Vibrations transfer through the bridge to the **resonator body** (wooden box or gourd)
4. The resonator body amplifies sound through both structural resonances and acoustic (Helmholtz) cavity resonance
5. Sound holes in the resonator box allow air-coupled sound to radiate

### Vibration Modes

The kalimba tine follows the **Euler-Bernoulli beam equation** with boundary conditions:
- One end **clamped** (held by the pressure bar)
- One intermediate point **simply supported** (the bridge/saddle)
- One end **free** (the playing end)

**Key findings from acoustic research (Benson & Dobson, 2012, JASA):**

The overtone sequence is **inharmonic** — the overtone ratios f(2)/f(1) and f(3)/f(1) are NOT integers. This is fundamentally different from strings:

| Mode | Ideal Cantilever Ratio | Kalimba Tine (measured) |
|------|----------------------|------------------------|
| 1st (fundamental) | 1.000 | 1.000 |
| 2nd overtone | ~5.4× | ~5.0× fundamental |
| 3rd overtone | ~13.3× | ~14.0× fundamental |

**Why anharmonic?**
- Unlike a string (which has integer harmonic ratios 1:2:3:4...), a vibrating beam's modal frequencies follow ratios proportional to the square of mode numbers
- The bridge point creates a node, and vibration modes on both sides of the bridge interact
- The clamped end prevents simple sinusoidal modes
- Coupling with the resonator body broadens resonant peaks and supports anharmonic modes

### Frequency Spectrum Characteristics

- **Fundamental**: strong, well-defined peak
- **2nd overtone**: approximately 5× fundamental — creates a bright, bell-like quality
- **3rd overtone**: approximately 14× fundamental — adds shimmer
- **Resonator modes**: the wooden box has both acoustic (air cavity) and structural resonances that color the sound
- **Attack transient**: brief burst containing broadband energy, quickly settling to modal frequencies
- **Decay**: relatively fast (1-3 seconds), exponential, with higher modes decaying faster
- **Buzz**: optional — some traditional mbira use bottle caps or shells on the resonator to add a characteristic buzzing timbre

### Resonator Box Modes

The resonator box contributes:
- **Helmholtz resonance** from the air cavity (low-frequency boost)
- **Structural resonances** from the wood panels (some alignment with tine overtones)
- **Sound hole radiation** — the rear sound holes allow direct acoustic radiation and can be manipulated by the player (finger vibrato)

---

## 2. Digital Synthesis Approaches

### Karplus-Strong Synthesis (Primary Approach)

The Karplus-Strong algorithm is ideal for kalimba synthesis because:
- It naturally models plucked/struck vibrating elements
- It produces the characteristic quick attack and exponential decay
- The feedback filter can be tuned to create the bell-like, slightly inharmonic timbre

**Algorithm overview:**
1. Fill a delay line with noise (or shaped excitation) of length L = sampleRate / frequency
2. Output samples from the delay line
3. Each sample leaving the delay line passes through a lowpass filter (averaging filter)
4. Filtered output is fed back into the delay line
5. The process naturally produces exponentially decaying oscillation

**Modifications for kalimba timbre:**

| Parameter | Kalimba Modification |
|-----------|---------------------|
| Excitation | Short burst of band-limited noise (5-15ms) with slight high-frequency emphasis |
| Loop filter | Asymmetric — preserve more high-frequency content than guitar to get bell-like quality |
| Delay line length | Primary: fs/f0; Secondary short delay for inharmonic overtone |
| Loop gain | 0.995-0.999 (shorter sustain than guitar) |
| Allpass filter | Add for fractional delay tuning and slight pitch drift |
| DC blocker | Essential — prevents low-frequency drift |

**Extended Karplus-Strong (EKS) for Kalimba:**
- Add a **second, shorter delay line** (at ~5× fundamental) to simulate the anharmonic 2nd overtone
- Use **pick position filtering** (comb filter) to simulate thumb position
- Add slight **detuning modulation** for the natural "wobble" of metal tines

### Alternative/Complementary Approaches

1. **Modal synthesis** — bank of resonant IIR filters at measured modal frequencies (good for accuracy)
2. **Additive synthesis** — sum of decaying sinusoids at f1, ~5f1, ~14f1 (simplest approach)
3. **Physical modeling via digital waveguide** — more complex but highest fidelity
4. **Sample-based with velocity layers** — highest realism, highest memory cost

### Recommended Hybrid Approach for iOS

Combine Karplus-Strong with modal synthesis:
- **Karplus-Strong** for the fundamental and natural-sounding decay
- **Additional resonant filters** at overtone frequencies (~5× and ~14× fundamental)
- **Resonator body simulation** via convolution with short impulse response or simple reverb

---

## 3. Real-World Frequency Data

### Standard 17-Key Kalimba in C Major

The standard layout alternates notes left-right from the center (longest tine = lowest note):

```
Physical layout (left to right, player's perspective):
D6  B5  G5  E5  C5  A4  F4  D4  C4  E4  G4  B4  D5  F5  A5  C6  E6

Position:  L8  L7  L6  L5  L4  L3  L2  L1  C   R1  R2  R3  R4  R5  R6  R7  R8
```

**Frequency table (A4 = 440 Hz, Equal Temperament):**

| Note | Octave | Frequency (Hz) | Position |
|------|--------|----------------|----------|
| C4   | 4      | 261.63         | Center   |
| D4   | 4      | 293.66         | L1       |
| E4   | 4      | 329.63         | R1       |
| F4   | 4      | 349.23         | L2       |
| G4   | 4      | 392.00         | R2       |
| A4   | 4      | 440.00         | L3       |
| B4   | 4      | 493.88         | R3       |
| C5   | 5      | 523.25         | L4       |
| D5   | 5      | 587.33         | R4       |
| E5   | 5      | 659.26         | L5       |
| F5   | 5      | 698.46         | R5       |
| G5   | 5      | 783.99         | L6       |
| A5   | 5      | 880.00         | R6       |
| B5   | 5      | 987.77         | L7       |
| C6   | 6      | 1046.50        | R7       |
| D6   | 6      | 1174.66        | L8       |
| E6   | 6      | 1318.51        | R8       |

### C Major Pentatonic Tuning (Alternative)

Remove F and B notes for a pure pentatonic (no "wrong" notes):

| Note | Frequency (Hz) |
|------|----------------|
| C4   | 261.63         |
| D4   | 293.66         |
| E4   | 329.63         |
| G4   | 392.00         |
| A4   | 440.00         |
| C5   | 523.25         |
| D5   | 587.33         |
| E5   | 659.26         |
| G5   | 783.99         |
| A5   | 880.00         |
| C6   | 1046.50        |

### Other Common Kalimba Tunings

| Tuning | Notes | Character |
|--------|-------|-----------|
| C Minor | C D Eb F G Ab Bb | Melancholic, emotional |
| A Minor Pentatonic | A C D E G | Dark, bluesy |
| G Major | G A B C D E F# | Bright, warm |
| D Minor (Kurd) | D E F G A Bb C | Middle Eastern feel |
| "Evil" Pentatonic (Maurice White) | Custom minor pentatonic | African, Earth Wind & Fire vibe |
| Middle Eastern (Hijaz) | A Bb C# D E F G# | Exotic, dramatic |

### 432 Hz Tuning Variant

For sound healing, some prefer A4 = 432 Hz. Multiply all frequencies by 432/440 ≈ 0.9818:
- C4 = 256.87 Hz, A4 = 432.00 Hz, etc.

---

## 4. Sound Healing Applications

### Therapeutic Uses

1. **Stress Reduction & Anxiety Relief**
   - The bright, bell-like tones promote alpha brainwave states
   - Repetitive melodic patterns induce relaxation response
   - The instrument's simplicity encourages meditative focus

2. **Music Therapy for Physical Disabilities**
   - Minimal physical effort required (thumb movement only)
   - Beneficial for patients with limited mobility, arthritis, or motor impairments
   - Vibrations can be felt through the hands, engaging proprioceptive feedback

3. **Prenatal and Infant Therapy**
   - Gentle frequencies suitable for calming infants
   - Can be played against the mother's abdomen for prenatal stimulation
   - Creates lasting bonding between parent and child through sound

4. **Hearing Impairment Support**
   - Vibrations through the body of the instrument allow experience of music through touch
   - The resonant body provides strong tactile feedback

5. **Cognitive and Neurological Benefits**
   - Improves concentration and mental clarity
   - Supports cognitive rehabilitation for brain injury patients
   - Helps with autism spectrum disorder — structured, predictable sound patterns

6. **Depression and Mood Enhancement**
   - Light, whimsical tones enable positive mood changes
   - The act of creating music provides sense of achievement and control
   - Pentatonic tuning makes it impossible to play "wrong" notes — reduces performance anxiety

### Practitioner Techniques

| Technique | Description | Healing Application |
|-----------|-------------|-------------------|
| **Slow arpeggios** | Gentle cascading up/down the scale | Deep relaxation, sleep induction |
| **Tremolo** | Rapid alternating between two adjacent tines | Energy activation, attention focusing |
| **Sound hole vibrato** | Cover/uncover rear sound holes with fingers | Wah-wah effect, emotional expression |
| **Glissando** | Sliding thumb across multiple tines | Energy sweeping, transition between states |
| **Drone** | Sustained repeat of a single note or interval | Grounding, centering |
| **Call-and-response** | Play-pause-play pattern | Interactive therapy, client engagement |
| **Binaural patterns** | Slightly detuned dual notes | Brainwave entrainment |

### Chakra Associations

| Chakra | Kalimba Note | Frequency Range |
|--------|-------------|-----------------|
| Root | C | 256-262 Hz |
| Sacral | D | 288-294 Hz |
| Solar Plexus | E | 320-330 Hz |
| Heart | F | 341-349 Hz |
| Throat | G | 384-392 Hz |
| Third Eye | A | 426-440 Hz |
| Crown | B | 480-494 Hz |

---

## 5. Mbira Relationship

### Historical Connection

- **Lamellophone family**: Both kalimba and mbira are lamellophones — instruments with plucked tines
- **Origin**: Wood/bamboo-tined lamellophones appeared in West Africa ~3,000 years ago. Metal-tined versions emerged in the Zambezi River valley ~1,300 years ago
- **Mbira dzaVadzimu**: The iconic Shona mbira of Zimbabwe, with 22-28 keys, used in spiritual ceremonies
- **Hugh Tracey**: In the 1950s, ethnomusicologist Hugh Tracey created a Westernized version inspired by mbira, naming it "kalimba" (possibly: ka = little, limba = sound/music)
- **Key difference**: Mbira is deeply tied to Shona ceremonial tradition; kalimba is the modernized, Western-tuned derivative
- **Sonic difference**: Traditional mbira often has bottle cap buzzers and uses non-Western tunings; kalimba uses Western equal temperament

### Design Implications for App

- Offer both "Kalimba" (clean, Western) and "Mbira" (buzzy, traditional) sound presets
- Include non-Western tuning options for authentic mbira experience
- Consider adding optional buzzer/rattle layer for mbira mode

---

## 6. Interaction Design for iOS

### Multi-Tine Touch Interface

```
┌─────────────────────────────────────┐
│          KALIMBA VIEW               │
│                                     │
│   ┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐    │
│   │D6││G5││C5││F4││C4││E4││B4│    │
│   │  ││  ││  ││  ││  ││  ││  │    │
│   │  ││  ││  ││  ││  ││  ││  │    │
│   └──┘│  ││  ││  ││  ││  ││  │    │
│       │  ││  ││  ││  ││  ││  │    │
│       └──┘│  ││  ││  ││  │└──┘    │
│           │  ││  ││  ││  │        │
│           └──┘│  ││  │└──┘        │
│               │  ││  │            │
│               └──┘└──┘            │
│     (Tines get shorter outward)    │
└─────────────────────────────────────┘
```

**Touch interactions:**
1. **Single tap** → Pluck single tine (velocity from tap force)
2. **Swipe across tines** → Glissando effect (trigger multiple tines with slight timing offset)
3. **Two-thumb simultaneous** → Chord (parallel touch on both sides)
4. **Long press** → Damped/muted pluck
5. **3D Touch / pressure** → Velocity control (harder press = louder/brighter)
6. **Thumb slide on single tine** → Pitch bend/vibrato

**Visual feedback:**
- Tine visually vibrates on pluck (subtle oscillation animation)
- Glow/particle effect on active tine
- Ripple emanating from struck tine through resonator body
- Color intensity maps to velocity

**Layout options:**
- Standard alternating (authentic layout)
- Linear ascending (beginner-friendly)
- Circular/radial layout

---

## 7. Synthesis Pseudocode (Swift-like)

```swift
class KalimbaSynthesizer {
    let sampleRate: Float = 44100.0
    
    // Karplus-Strong delay line
    var delayLine: [Float] = []
    var delayIndex: Int = 0
    var delayLength: Int = 0
    
    // Inharmonic overtone resonators
    var overtone2Filter: ResonantFilter  // ~5× fundamental
    var overtone3Filter: ResonantFilter  // ~14× fundamental
    
    // Parameters
    var loopGain: Float = 0.997
    var brightness: Float = 0.5  // controls lowpass cutoff
    var velocity: Float = 1.0
    
    struct ResonantFilter {
        var frequency: Float
        var bandwidth: Float
        var amplitude: Float
        var y1: Float = 0, y2: Float = 0
        
        mutating func process(_ input: Float, sampleRate: Float) -> Float {
            let r = exp(-Float.pi * bandwidth / sampleRate)
            let a1 = -2.0 * r * cos(2.0 * Float.pi * frequency / sampleRate)
            let a2 = r * r
            let output = amplitude * input - a1 * y1 - a2 * y2
            y2 = y1
            y1 = output
            return output
        }
    }
    
    func noteOn(frequency: Float, velocity: Float) {
        self.velocity = velocity
        
        // Calculate delay line length for fundamental
        delayLength = Int(sampleRate / frequency)
        delayLine = [Float](repeating: 0, count: delayLength)
        delayIndex = 0
        
        // Fill delay line with shaped noise burst (excitation)
        for i in 0..<delayLength {
            // Band-limited noise with slight high-freq emphasis
            let noise = Float.random(in: -1...1)
            let shaped = noise * velocity
            // Apply short fade-in to avoid click
            let envelope = min(Float(i) / 10.0, 1.0)
            delayLine[i] = shaped * envelope
        }
        
        // Configure overtone resonators (anharmonic!)
        let overtone2Freq = frequency * 5.07  // kalimba-specific ratio
        let overtone3Freq = frequency * 13.8  // kalimba-specific ratio
        
        overtone2Filter = ResonantFilter(
            frequency: overtone2Freq,
            bandwidth: 30.0,   // relatively narrow
            amplitude: 0.15 * velocity
        )
        overtone3Filter = ResonantFilter(
            frequency: overtone3Freq,
            bandwidth: 50.0,   // wider = faster decay
            amplitude: 0.05 * velocity
        )
    }
    
    func generateSample() -> Float {
        // Read from delay line
        let output = delayLine[delayIndex]
        
        // === Karplus-Strong loop filter ===
        // Two-point average (simple lowpass) with brightness control
        let nextIndex = (delayIndex + 1) % delayLength
        let filtered = brightness * delayLine[nextIndex] 
                     + (1.0 - brightness) * output
        
        // Apply loop gain (controls decay time)
        let fedBack = filtered * loopGain
        
        // Write back to delay line
        delayLine[delayIndex] = fedBack
        
        // Advance delay index
        delayIndex = (delayIndex + 1) % delayLength
        
        // === Add inharmonic overtones ===
        let overtone2 = overtone2Filter.process(output, sampleRate: sampleRate)
        let overtone3 = overtone3Filter.process(output, sampleRate: sampleRate)
        
        // Mix fundamental + overtones
        let mixed = output * 0.7 + overtone2 + overtone3
        
        // DC blocking filter (simple high-pass)
        return dcBlock(mixed)
    }
    
    // Simple DC blocker
    var dcPrev: Float = 0, dcOut: Float = 0
    mutating func dcBlock(_ input: Float) -> Float {
        dcOut = input - dcPrev + 0.995 * dcOut
        dcPrev = input
        return dcOut
    }
    
    // === Resonator body simulation ===
    func applyResonatorBody(_ signal: Float) -> Float {
        // Simple comb filter to simulate box resonance
        // Helmholtz resonance typically around 200-400 Hz
        let helmholtzDelay = Int(sampleRate / 300.0)  // ~300 Hz resonance
        // ... implement short comb filter here
        return signal  // placeholder
    }
    
    // === Glissando (swipe across tines) ===
    func triggerGlissando(
        fromNote: Int, 
        toNote: Int, 
        duration: Float,
        scale: [Float]  // frequencies in the scale
    ) {
        let noteCount = abs(toNote - fromNote) + 1
        let noteSpacing = duration / Float(noteCount)
        
        for i in 0..<noteCount {
            let noteIndex = fromNote + (toNote > fromNote ? i : -i)
            let delay = Float(i) * noteSpacing
            let vel = velocity * (0.8 + 0.2 * Float.random(in: 0...1))
            
            // Schedule note with delay
            scheduleNote(
                frequency: scale[noteIndex],
                velocity: vel,
                atTime: delay
            )
        }
    }
}

// === Usage Example ===
let kalimba = KalimbaSynthesizer()

// Play C4
kalimba.noteOn(frequency: 261.63, velocity: 0.8)

// Generate audio buffer
var buffer = [Float](repeating: 0, count: 44100 * 3)  // 3 seconds
for i in 0..<buffer.count {
    buffer[i] = kalimba.generateSample()
}
```

### Key Implementation Notes for iOS (AVAudioEngine)

1. **Use AVAudioSourceNode** for real-time synthesis callback
2. **Polyphony**: Maintain array of voice instances (8-17 voices for full chord)
3. **Latency**: Target < 10ms for responsive feel — use small buffer sizes (128-256 frames)
4. **Fractional delay**: Use allpass interpolation for accurate tuning between sample boundaries
5. **Velocity layers**: Map touch force to both amplitude AND brightness (harder = louder + brighter)
6. **Stereo**: Pan each tine slightly based on its physical position (left tines → left channel)

---

## References

1. Benson, D. & Dobson, S. "The tones of the kalimba (African thumb piano)" — JASA, Vol. 131, No. 1, January 2012
2. "Modes of the kalimba resonator box" — JASA, Vol. 124, No. 4, 2008
3. Karplus, K. & Strong, A. "Digital Synthesis of Plucked-String and Drum Timbres" — Computer Music Journal, 1983
4. Jaffe, D. & Smith, J.O. "Extensions of the Karplus-Strong Plucked String Algorithm" — Computer Music Journal, 1983
5. Kubik, G. "Kalimba, Nsansi, Mbira: Lamellophone in Africa" — 1998
6. Hugh Tracey / Kalimba Magic — kalimbamagic.com
7. Physics Stack Exchange — kalimba anharmonicity discussion
