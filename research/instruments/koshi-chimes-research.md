# Koshi & Zaphir Chimes — Comprehensive Research for Sound Healing iOS App

> **Last Updated:** 2026-02-08
> **Purpose:** Digital synthesis reference for iOS app implementation

---

## 1. Physical Acoustics

### How Koshi Chimes Produce Sound

Koshi chimes are cylindrical resonant instruments handcrafted in France (at the foot of the Pyrenees) by artisan "Kabir" since ~2010. The creator previously worked at Shanti/Zaphir for several years.

**Construction:**
- **Resonance tube:** Thin layers of bamboo veneer treated with natural oil, forming a high acoustic quality cylinder
- **Dimensions:** 2.5" (64 mm) diameter × 6.5" (165 mm) length
- **Tines/rods:** 8 metal bars (rods/chords) of different lengths, welded with silver to a metal plate (soundboard) at the base
- **Striker:** A cord with a small striker hangs from the top; movement causes the striker to hit the rods

**Sound Production Mechanism:**
1. The striker impacts one or more metal rods
2. Each rod vibrates at its tuned fundamental frequency and overtones
3. Vibration energy transfers through the silver weld joints to the metal base plate
4. The base plate acts as a soundboard, coupling energy into the bamboo resonance tube
5. The bamboo cylinder acts as a Helmholtz-like resonator, amplifying and shaping the sound
6. The tube shapes the frequency response, adding warmth and body to the metallic partials

**Resonance Modes:**
- **Rod vibration (primary):** Each metal rod behaves as a clamped-free bar (cantilever). For a uniform bar clamped at one end, vibrational modes follow the relationship:
  - f₁ = fundamental
  - f₂ ≈ 6.27 × f₁ (2nd mode)
  - f₃ ≈ 17.55 × f₁ (3rd mode)
  - These are **inharmonic** — unlike strings, bars produce non-integer partial ratios
- **Tube resonance (secondary):** The bamboo cylinder has its own resonant modes that selectively amplify certain frequencies
- **Plate coupling:** The metal base plate has its own vibration modes that further color the sound

**Frequency Spectrum Characteristics:**
- Rich in overtones, but overtones are **inharmonic** (characteristic of struck bars/rods)
- The bamboo tube adds a warm, filtered quality
- Clear attack transient followed by long, shimmering decay
- "Circular tone range" — shorter rods' overtones overlap with longer rods' fundamentals, creating continuous cascading sound
- High Q-factor gives sustain of 3–8 seconds per strike
- Frequency content typically spans ~200 Hz to ~8000 Hz with strongest energy in the 400–2000 Hz range

### Koshi vs Zaphir: Physical Differences

| Property | Koshi | Zaphir |
|----------|-------|--------|
| Body material | Bamboo veneer (natural acoustic response) | Recycled cardboard/fibreboard |
| Body dimensions | 16.5cm × 6.5cm (taller, narrower) | 12.5cm × 6.5cm (shorter, Golden Ratio proportions) |
| Tines | 8 metal rods, silver-welded | 8 metal rods |
| Tunings available | 4 (elements) | 5 (Feng Shui seasons) |
| Sound character | More bell-like, mellow, earthy | Crystal-clear, brighter, longer decay |
| Volume | Slightly louder (bamboo resonance) | Quieter (cardboard dampens less selectively) |
| Wind sensitivity | Needs more wind (heavier striker plate) | Responds to lighter wind (larger strike surface) |
| Note range | All natural notes | Includes sharps (G#, C#, A#) |
| Blending | All 4 blend harmoniously | Some combinations can create dissonance |
| Origin | France, since 2010 | France (as Shanti), 30+ years |
| Maintenance | Needs periodic oiling (teak/tung oil) | No oiling needed |

---

## 2. Real-World Frequency Data

### Koshi Tunings (Official, from koshi.fr)

All notes listed in order of the 8 rods. Frequencies calculated at A4 = 440 Hz, assuming octave 4–5 range typical for these small chimes:

#### Terra (Earth) — C Major tonality
**Notes:** G, C, E, F, G, C, E, G
**Color code:** Green

| Rod | Note | Octave Est. | Frequency (Hz) |
|-----|------|-------------|-----------------|
| 1 | G | 4 | 392.00 |
| 2 | C | 5 | 523.25 |
| 3 | E | 5 | 659.26 |
| 4 | F | 5 | 698.46 |
| 5 | G | 5 | 783.99 |
| 6 | C | 6 | 1046.50 |
| 7 | E | 6 | 1318.51 |
| 8 | G | 6 | 1567.98 |

*Character: Grounding, warm, deep. Strong root/fifth relationship (C-G). Aligns with root chakra energy. Most "mellow" of the four.*

#### Aqua (Water) — D minor / Pentatonic
**Notes:** A, D, F, G, A, D, F, A
**Color code:** Blue

| Rod | Note | Octave Est. | Frequency (Hz) |
|-----|------|-------------|-----------------|
| 1 | A | 4 | 440.00 |
| 2 | D | 5 | 587.33 |
| 3 | F | 5 | 698.46 |
| 4 | G | 5 | 783.99 |
| 5 | A | 5 | 880.00 |
| 6 | D | 6 | 1174.66 |
| 7 | F | 6 | 1396.91 |
| 8 | A | 6 | 1760.00 |

*Character: Flowing, fluid, soothing. Pentatonic scale — no dissonant intervals. D minor without the third (ambiguous mode). Aligns with sacral chakra.*

#### Aria (Air) — A minor tonality
**Notes:** A, C, E, A, B, C, E, B
**Color code:** Yellow

| Rod | Note | Octave Est. | Frequency (Hz) |
|-----|------|-------------|-----------------|
| 1 | A | 4 | 440.00 |
| 2 | C | 5 | 523.25 |
| 3 | E | 5 | 659.26 |
| 4 | A | 5 | 880.00 |
| 5 | B | 5 | 987.77 |
| 6 | C | 6 | 1046.50 |
| 7 | E | 6 | 1318.51 |
| 8 | B | 6 | 1975.53 |

*Character: Light, airy, ethereal. A minor with added B (natural 9th). Most "open" and spacious sounding. Aligns with throat/third eye chakra.*

#### Ignis (Fire) — G Major / Pentatonic
**Notes:** G, B, D, G, B, D, G, A
**Color code:** Red

| Rod | Note | Octave Est. | Frequency (Hz) |
|-----|------|-------------|-----------------|
| 1 | G | 4 | 392.00 |
| 2 | B | 4 | 493.88 |
| 3 | D | 5 | 587.33 |
| 4 | G | 5 | 783.99 |
| 5 | B | 5 | 987.77 |
| 6 | D | 6 | 1174.66 |
| 7 | G | 6 | 1567.98 |
| 8 | A | 6 | 1760.00 |

*Character: Bright, energetic, warm. Strong G major triad with added A (sus2). Most "uplifting" of the four. Aligns with solar plexus/heart chakra.*

### Zaphir Tunings (5 Seasons / Feng Shui Elements)

#### Crystalide (Spring / Wood)
**Notes:** G, A, B, D, A, G, B, D

| Rod | Note | Est. Freq (Hz) |
|-----|------|-----------------|
| 1 | G | 392.00 |
| 2 | A | 440.00 |
| 3 | B | 493.88 |
| 4 | D | 587.33 |
| 5 | A | 880.00 |
| 6 | G | 783.99 |
| 7 | B | 987.77 |
| 8 | D | 1174.66 |

*Character: G major pentatonic feel (G-A-B-D). Fresh, open, joyful.*

#### Sunray (Summer / Fire)
**Notes:** G#, B, C#, E, G#, E, A, C#

| Rod | Note | Est. Freq (Hz) |
|-----|------|-----------------|
| 1 | G# | 415.30 |
| 2 | B | 493.88 |
| 3 | C# | 554.37 |
| 4 | E | 659.26 |
| 5 | G# | 830.61 |
| 6 | E | 659.26 |
| 7 | A | 880.00 |
| 8 | C# | 1108.73 |

*Character: E major / C# minor feel. Warm, radiant. Contains sharps — unique sparkle.*

#### Twilight (Autumn / Metal)
**Notes:** E, G, B, C, E, G, B, C

| Rod | Note | Est. Freq (Hz) |
|-----|------|-----------------|
| 1 | E | 329.63 |
| 2 | G | 392.00 |
| 3 | B | 493.88 |
| 4 | C | 523.25 |
| 5 | E | 659.26 |
| 6 | G | 783.99 |
| 7 | B | 987.77 |
| 8 | C | 1046.50 |

*Character: E minor / C major feel. Contemplative, introspective, cool.*

#### Blue Moon (Winter / Water)
**Notes:** D, F, A, B, C, E, A#, C

| Rod | Note | Est. Freq (Hz) |
|-----|------|-----------------|
| 1 | D | 293.66 |
| 2 | F | 349.23 |
| 3 | A | 440.00 |
| 4 | B | 493.88 |
| 5 | C | 523.25 |
| 6 | E | 659.26 |
| 7 | A#/Bb | 932.33 |
| 8 | C | 1046.50 |

*Character: Complex harmony (D minor + Bb). Mysterious, deep, haunting.*

#### Sufi (Intermediary Season / Earth)
**Notes:** F, A, D, F, A, G, A, D

| Rod | Note | Est. Freq (Hz) |
|-----|------|-----------------|
| 1 | F | 349.23 |
| 2 | A | 440.00 |
| 3 | D | 587.33 |
| 4 | F | 698.46 |
| 5 | A | 880.00 |
| 6 | G | 783.99 |
| 7 | A | 880.00 |
| 8 | D | 1174.66 |

*Character: D minor / F major feel. Meditative, spiritual, Middle Eastern undertones.*

---

## 3. Digital Synthesis Approaches

### Primary Method: Additive Synthesis with Inharmonic Partials

Koshi/Zaphir chimes are best synthesized using **additive synthesis** because:
- Each rod produces a distinct fundamental + inharmonic overtones
- Multiple rods sound simultaneously in random patterns
- The bamboo resonator applies a spectral filter
- Long, shimmering decays are the defining characteristic

### Synthesis Architecture

```
[Rod Excitation] → [Inharmonic Partial Generator] → [Resonator Filter] → [Reverb/Space]
       ↑                                                    ↑
  [Touch/Wind Input]                              [Bamboo Tube Model]
```

### Key Parameters

**Per-Rod Parameters:**
- `fundamentalFreq`: Hz (from tuning tables above)
- `amplitude`: 0.0–1.0 (initial strike energy)
- `inharmonicityFactor`: ~6.27 for 2nd partial, ~17.55 for 3rd (bar vibration ratios)
- `numPartials`: 4–8 per rod (higher = brighter, more CPU)
- `partialAmplitudes`: Decreasing array [1.0, 0.4, 0.15, 0.06, ...]

**Envelope (ADSR per partial):**
- **Attack:** 1–5 ms (metallic click of strike)
- **Decay:** 0 ms (immediate transition to sustain-decay)
- **Sustain:** N/A (no sustain phase — it's a struck instrument)
- **Release/Decay Time:** 2000–8000 ms (exponential decay, differs per partial)
  - Fundamental: longest decay (~5–8 sec)
  - Higher partials: shorter decay (~1–3 sec) — critical for realism
  - This creates the characteristic "shimmer" as high partials die first

**Resonator Parameters:**
- `tubeResonanceFreq`: ~800–1200 Hz center (bamboo tube resonance)
- `tubeQ`: 3–8 (moderate Q — not too narrow)
- `tubeGain`: 0.3–0.6 (how much the tube amplifies resonant frequencies)

### Partial Frequency Ratios for Metal Bars/Rods

For a clamped-free bar (cantilever), the vibration mode frequencies relate to the fundamental as:
```
Mode 1 (fundamental): f₁ = 1.000
Mode 2:               f₂ = 6.267 × f₁
Mode 3:               f₃ = 17.548 × f₁
Mode 4:               f₄ = 34.386 × f₁
```

However, Koshi rods are short, thin, and precisely tuned — the inharmonicity is less extreme than thick bars. For synthesis, use slightly adjusted ratios:
```
Practical synthesis ratios (approximated for thin rods):
Partial 1: 1.0    (fundamental)
Partial 2: 3.2–5.0  (adjusted — less inharmonic than theory)
Partial 3: 8.0–12.0
Partial 4: 15.0–20.0
```

**Important:** The exact inharmonicity should be a tunable parameter in the app. Let users adjust "brightness" which maps to inharmonicity factor.

---

## 4. Sound Healing Applications

### Therapeutic Uses
- **Stress reduction:** Alpha/theta brainwave entrainment through repetitive, gentle tones
- **Session transitions:** Mark beginning/end of meditation, yoga, or healing sessions
- **Grounding:** Terra tuning connects to root chakra and physical body
- **Emotional release:** Aqua tuning promotes emotional flow and cleansing
- **Mental clarity:** Aria tuning clears mental fog, enhances communication
- **Energy activation:** Ignis tuning sparks creativity and motivation

### Practitioner Techniques
1. **Opening bell:** Single gentle ring at session start — signals transition from "doing" to "being"
2. **Closing bell:** Final chime at session end — signals gentle return to awareness
3. **Sound washing:** Slow, continuous movement creating cascading tones over a client's body
4. **Element pairing:** Using specific tunings for specific therapeutic goals
5. **Attention anchor:** "Follow the sound from beginning to end" — meditation technique
6. **Transition marker:** Between different phases of a sound bath
7. **Space clearing:** Ringing chimes to "clear" a room's energy before sessions

### Session Placement
- **Beginning (0–5 min):** Gentle single rings to settle participants
- **During transitions:** Between singing bowls, gongs, or other instruments
- **Climax (optional):** Multiple chimes played simultaneously for immersive effect
- **Ending (last 5 min):** Gradually decreasing rings as session closes
- **Integration:** Final single ring followed by silence

### Elemental Healing Associations
| Element | Tuning | Chakra | Intention | Best For |
|---------|--------|--------|-----------|----------|
| Terra | C major | Root | Grounding, stability | Anxiety, disconnection |
| Aqua | D minor | Sacral | Flow, emotion | Emotional blocks, rigidity |
| Aria | A minor | Throat/3rd Eye | Clarity, breath | Mental fog, communication |
| Ignis | G major | Solar Plexus | Energy, transformation | Low motivation, stagnation |

---

## 5. Interaction Design for iOS

### Touch Gestures

#### Primary Interaction: Tilt/Gyroscope (Wind Simulation)
- **Physical device tilt** maps to wind hitting the chime
- Tilt angle → wind force → probability and intensity of rod strikes
- Small tilt = gentle, occasional notes
- Large tilt = multiple notes cascade rapidly
- **Rotation (yaw)** = swirling wind effect, triggers circular patterns

#### Touch-Based Interaction
- **Tap on chime:** Single rod strike (user selects which rod)
- **Swipe across chime:** Multiple rods struck in sequence (speed = tempo)
- **Long press + drag:** Continuous gentle ringing (like holding in wind)
- **Two-finger tap:** Strike two rods simultaneously (harmony)
- **Shake gesture:** Vigorous wind — all rods activated

#### Wind Simulation Algorithm
```
windForce = deviceTiltMagnitude * sensitivity
for each rod in chime:
    strikeProbability = windForce * rod.windExposure * random(0.5, 1.5)
    if random() < strikeProbability:
        strikeEnergy = windForce * random(0.3, 1.0)
        triggerRod(rod, energy: strikeEnergy)
        rod.cooldown = random(200, 800) ms  // prevent machine-gun effect
```

#### Visual Feedback
- Bamboo tube rendered in 3D with subtle sway matching device tilt
- Visible rods inside (optional transparent view)
- Particle effects: small light motes float upward on each note
- Color glow matching element (green/blue/yellow/red)

### UI Layout Suggestions
- **Main screen:** Full-screen chime visual, tilt to play
- **Element selector:** Swipe left/right to change tuning (Terra/Aqua/Aria/Ignis)
- **Settings gear:** Sensitivity, reverb, volume per rod
- **Session mode:** Timer + automatic wind patterns
- **Record button:** Capture and export audio

---

## 6. Synthesis Pseudocode (Swift-like)

```swift
// MARK: - Koshi Chime Synthesizer

struct ChimeRod {
    let fundamentalFreq: Float     // Hz
    let partialRatios: [Float]     // [1.0, 3.5, 9.2, 18.0]
    let partialAmplitudes: [Float] // [1.0, 0.35, 0.12, 0.04]
    let decayTimes: [Float]        // seconds per partial [6.0, 3.0, 1.5, 0.8]
    var phase: [Float]             // current phase per partial
    var amplitude: [Float]         // current amplitude per partial
    var isActive: Bool
    var timeSinceStrike: Float
}

struct KoshiTuning {
    let name: String               // "Terra", "Aqua", etc.
    let element: Element
    let rodFrequencies: [Float]    // 8 frequencies
}

// Tuning definitions
let terra = KoshiTuning(
    name: "Terra",
    element: .earth,
    rodFrequencies: [392.00, 523.25, 659.26, 698.46, 
                     783.99, 1046.50, 1318.51, 1567.98]
)

let aqua = KoshiTuning(
    name: "Aqua",
    element: .water,
    rodFrequencies: [440.00, 587.33, 698.46, 783.99,
                     880.00, 1174.66, 1396.91, 1760.00]
)

let aria = KoshiTuning(
    name: "Aria",
    element: .air,
    rodFrequencies: [440.00, 523.25, 659.26, 880.00,
                     987.77, 1046.50, 1318.51, 1975.53]
)

let ignis = KoshiTuning(
    name: "Ignis",
    element: .fire,
    rodFrequencies: [392.00, 493.88, 587.33, 783.99,
                     987.77, 1174.66, 1567.98, 1760.00]
)

class KoshiChimeSynth {
    let sampleRate: Float = 44100.0
    var rods: [ChimeRod] = []
    var tuning: KoshiTuning
    
    // Resonator (bamboo tube simulation)
    let resonatorCenterFreq: Float = 950.0  // Hz
    let resonatorQ: Float = 5.0
    let resonatorGain: Float = 0.4
    var resonatorFilter: BiquadFilter  // bandpass
    
    // Reverb for spaciousness
    var reverb: ConvolutionReverb
    
    init(tuning: KoshiTuning) {
        self.tuning = tuning
        self.resonatorFilter = BiquadFilter(
            type: .bandpass,
            frequency: resonatorCenterFreq,
            q: resonatorQ
        )
        self.reverb = ConvolutionReverb(preset: .smallRoom)
        
        // Initialize 8 rods
        for freq in tuning.rodFrequencies {
            let rod = ChimeRod(
                fundamentalFreq: freq,
                partialRatios: [1.0, 3.8, 10.5, 19.2],
                partialAmplitudes: [1.0, 0.35, 0.12, 0.04],
                decayTimes: [6.0, 3.0, 1.5, 0.7],
                phase: [0, 0, 0, 0],
                amplitude: [0, 0, 0, 0],
                isActive: false,
                timeSinceStrike: 0
            )
            rods.append(rod)
        }
    }
    
    // Strike a rod with given energy (0.0–1.0)
    func strikeRod(_ index: Int, energy: Float) {
        guard index < rods.count else { return }
        rods[index].isActive = true
        rods[index].timeSinceStrike = 0
        
        // Set initial amplitudes based on strike energy
        for i in 0..<rods[index].partialAmplitudes.count {
            rods[index].amplitude[i] = rods[index].partialAmplitudes[i] * energy
        }
        
        // Slight random phase for natural variation
        for i in 0..<rods[index].phase.count {
            rods[index].phase[i] = Float.random(in: 0..<(2 * .pi))
        }
    }
    
    // Wind simulation — call from motion/gyroscope handler
    func processWind(force: Float, dt: Float) {
        for i in 0..<rods.count {
            let strikeProbability = force * 0.3 * dt  // probability per frame
            let randomValue = Float.random(in: 0...1)
            
            if randomValue < strikeProbability && !rods[i].isActive {
                let energy = force * Float.random(in: 0.3...1.0)
                strikeRod(i, energy: min(energy, 1.0))
            }
        }
    }
    
    // Generate audio samples (called from audio render callback)
    func renderSamples(buffer: inout [Float], frameCount: Int) {
        let dt = 1.0 / sampleRate
        
        for frame in 0..<frameCount {
            var sample: Float = 0.0
            
            for rodIndex in 0..<rods.count {
                guard rods[rodIndex].isActive else { continue }
                
                rods[rodIndex].timeSinceStrike += dt
                var rodSample: Float = 0.0
                var anyPartialActive = false
                
                // Additive synthesis: sum all partials
                for p in 0..<rods[rodIndex].partialRatios.count {
                    let freq = rods[rodIndex].fundamentalFreq * rods[rodIndex].partialRatios[p]
                    
                    // Exponential decay
                    let decayRate = -1.0 / rods[rodIndex].decayTimes[p]
                    let env = rods[rodIndex].amplitude[p] * 
                              exp(decayRate * rods[rodIndex].timeSinceStrike)
                    
                    if env > 0.001 {
                        anyPartialActive = true
                        
                        // Phase accumulation
                        rods[rodIndex].phase[p] += 2 * .pi * freq * dt
                        if rods[rodIndex].phase[p] > 2 * .pi {
                            rods[rodIndex].phase[p] -= 2 * .pi
                        }
                        
                        rodSample += env * sin(rods[rodIndex].phase[p])
                    }
                }
                
                if !anyPartialActive {
                    rods[rodIndex].isActive = false
                }
                
                sample += rodSample
            }
            
            // Apply resonator filter (bamboo tube)
            sample = resonatorFilter.process(sample) * resonatorGain + sample * (1.0 - resonatorGain)
            
            // Soft clip to prevent distortion
            sample = tanh(sample * 0.5)
            
            buffer[frame] = sample
        }
        
        // Apply reverb (post-processing)
        reverb.process(&buffer, frameCount: frameCount)
    }
}

// MARK: - Wind Simulation from Device Motion

class ChimeMotionController {
    let motionManager = CMMotionManager()
    let chimeSynth: KoshiChimeSynth
    var sensitivity: Float = 1.0
    
    func startMotionUpdates() {
        motionManager.deviceMotionUpdateInterval = 1.0 / 60.0
        motionManager.startDeviceMotionUpdates(to: .main) { motion, error in
            guard let motion = motion else { return }
            
            // Calculate tilt magnitude
            let pitch = Float(motion.attitude.pitch)
            let roll = Float(motion.attitude.roll)
            let tiltMagnitude = sqrt(pitch * pitch + roll * roll)
            
            // Map tilt to wind force (0 = still, 1 = strong wind)
            let windForce = min(tiltMagnitude * self.sensitivity * 0.5, 1.0)
            
            // Process wind with ~16ms dt
            self.chimeSynth.processWind(force: windForce, dt: 1.0 / 60.0)
        }
    }
}

// MARK: - Session/Timer Mode

class ChimeSessionController {
    enum WindPattern {
        case gentle      // Slow, rare strikes — 1-2 notes every 3-5 sec
        case flowing     // Moderate — 2-4 notes every 1-3 sec
        case storm       // Active — rapid cascading notes
        case breathing   // Synced to a breath cycle (4-7-8 pattern)
    }
    
    func generateAutoWind(pattern: WindPattern, time: Float) -> Float {
        switch pattern {
        case .gentle:
            // Slow sine wave modulation
            return max(0, sin(time * 0.3) * 0.2 + Float.random(in: -0.05...0.05))
            
        case .flowing:
            // Moderate with random gusts
            return sin(time * 0.8) * 0.4 + Float.random(in: 0...0.15)
            
        case .storm:
            // Active, layered sine waves
            let base = sin(time * 1.5) * 0.3
            let gust = sin(time * 4.2) * 0.2
            return max(0, base + gust + 0.3)
            
        case .breathing:
            // 4-7-8 breathing: inhale(4), hold(7), exhale(8)
            let cycleLength: Float = 19.0  // seconds
            let phase = fmod(time, cycleLength)
            if phase < 4.0 {
                return 0.0  // Inhale — silence
            } else if phase < 11.0 {
                return 0.05  // Hold — very gentle
            } else {
                // Exhale — increasing wind
                let exhaleProgress = (phase - 11.0) / 8.0
                return exhaleProgress * 0.6
            }
        }
    }
}
```

---

## 7. Implementation Notes

### Performance Optimization for iOS
- Use `AVAudioEngine` with custom `AVAudioSourceNode` for real-time synthesis
- 4 partials × 8 rods = 32 oscillators max — very manageable on modern iOS
- Use SIMD/Accelerate framework for batch sin() calculations
- Pre-compute phase increments per frequency
- Consider using `vDSP` for buffer operations

### Audio Quality Considerations
- Sample rate: 44100 Hz (standard)
- Buffer size: 256–512 samples for low latency tilt-to-sound
- Apply dithering before output
- Use smooth parameter interpolation to avoid clicks

### Realism Enhancements
- Add slight random detuning (±2–5 cents) per strike for natural variation
- Vary attack time slightly (1–5 ms) per strike
- Add subtle noise burst at attack onset (metallic "click")
- Implement sympathetic resonance: when one rod rings, nearby-frequency rods vibrate slightly
- Consider convolution reverb with bamboo/wooden room IR

### Multiple Chime Support
- Allow playing 2–4 chimes simultaneously (all Koshi tunings blend)
- Implement spatial panning — each chime in a different stereo position
- Consider binaural panning for headphone mode

---

## References & Sources

1. **Koshi Official:** https://koshi.fr/en/shop/20-koshi-chime.html
2. **Sound Healing Lab:** https://soundhealinglab.com/products/koshi-chimes
3. **Didge Project:** https://www.didgeproject.com/product/koshi-chimes-4-elements-all-scales/
4. **Gaia Chimes (comparison):** https://gaiachimes.com/blogs/zaphir/koshi-chimes-compared-to-zaphir-chimes
5. **Vocal.media (Koshi vs Zaphir):** https://vocal.media/fyi/the-difference-between-koshi-and-zaphir-chimes
6. **Immersive Sound Exp:** https://immersivesoundexp.com/koshi-chimes/
7. **Zaphir tunings (Amazon/Gongs Unlimited):** Multiple sources confirmed
8. **Bar vibration physics:** Hyperphysics, HandWiki (strike tone ratios)
9. **Healing-sounds.com:** https://healing-sounds.com/blogs/koshi-chimes/
