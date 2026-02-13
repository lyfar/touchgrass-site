# Rain Stick — Comprehensive Research for Sound Healing iOS App

## 1. Physical Acoustics

### How the Rain Stick Produces Sound

The rain stick is an **idiophone/rattle** — a percussion instrument classified under Hornbostel-Sachs 112.13+133.1 (vessel rattle with friction). It produces sound through cascading granular collisions.

**Sound production mechanism:**
1. A long hollow tube (30-150 cm) is filled with small granular material (pebbles, seeds, beans, beads)
2. Internal obstructions (pins, thorns, or wooden pegs) are arranged **helically** along the inside wall
3. When tilted, gravity pulls the filling material downward
4. Pellets cascade through the helical obstacle course, bouncing off pins and tube walls
5. Each individual collision produces a tiny broadband impulse
6. The aggregate of thousands of simultaneous micro-collisions creates continuous **broadband noise** resembling rain

### Physics of Cascading Pellets

**Individual pellet collision:**
- Each pellet-to-pin collision produces a short impulse (~0.5-5 ms duration)
- The spectrum of a single collision is broadband with emphasis determined by:
  - Pellet mass and material (heavier = lower frequency emphasis)
  - Pin material and stiffness
  - Collision velocity (determined by tilt angle)
  - Tube wall resonance

**Statistical sound model:**
- The rain stick sound is analogous to a **Poisson process** of impulse events
- At high tilt angles: many simultaneous collisions → dense noise (approaching pink/white noise)
- At low tilt angles: sparse, individual "drop" sounds → clearly distinguishable impacts
- The transition between sparse and dense is key to the rain stick's character

**Frequency spectrum:**
| Component | Frequency Range | Source |
|-----------|----------------|--------|
| Sub-bass resonance | 60-200 Hz | Tube cavity resonance (Helmholtz-like) |
| Body | 200-2000 Hz | Pellet-pin collisions, tube wall vibration |
| Presence | 2000-6000 Hz | High-frequency collision transients |
| Air/shimmer | 6000-12000 Hz | Friction and small particle rattling |

### Acoustic Characteristics

- **Spectrum**: Broadband, approximately **pink noise** (1/f falloff — more energy in low frequencies)
- **Temporal envelope**: Gradual onset, sustained phase of variable density, gradual fadeout
- **Duration**: Depends on tube length — typically 3-30 seconds for a full cascade
- **Dynamics**: Controlled entirely by tilt angle and rotation speed
- **Stereo character**: The sound naturally "moves" as pellets travel through the tube
- **No definite pitch**: The rain stick produces noise, not tones — making it unique among sound healing instruments

### Material Influence on Timbre

| Material | Tube | Filling | Sound Character |
|----------|------|---------|-----------------|
| Cactus husk | Traditional | Pebbles | Warm, organic, mellow |
| Bamboo | Common modern | Seeds/beads | Bright, resonant, projecting |
| Birch wood | Premium | Small stones | Warm, earthy, deep |
| Plastic/PVC | Craft | Rice/lentils | Thin, high-frequency emphasis |

### Key Acoustic Parameters for Synthesis

| Parameter | Physical Correlate | Range |
|-----------|-------------------|-------|
| Tilt angle | Gravity component along tube axis | 0° (horizontal) to 90° (vertical) |
| Pellet density | Cascade rate / collisions per second | 0-1000+ events/sec |
| Pellet size | Individual impact spectrum shape | Small = bright, Large = dark |
| Pin spacing | Time between successive bounces | Tighter = faster cascade |
| Tube length | Total cascade duration | 30-150 cm |
| Tube diameter | Resonance frequency, spatial spread | 3-8 cm |
| Fill amount | Event density, weight | 10-30% of tube volume |

---

## 2. Digital Synthesis Approaches

### Granular Synthesis (Primary Approach)

Granular synthesis is the ideal technique for rain stick synthesis because:
- Rain is literally a granular phenomenon — millions of tiny sound events
- Individual "grains" map directly to individual pellet collisions
- Grain density can be smoothly controlled by tilt angle
- Natural micro-variation between grains creates organic texture

**Core algorithm:**
1. Define a library of micro-grains (tiny audio samples of individual impacts, 1-50ms)
2. Trigger grains at a rate controlled by tilt angle
3. Each grain has randomized parameters: pitch, amplitude, pan, duration
4. Overlap and sum grains to create continuous texture
5. Apply envelope shaping and tube resonance filtering

**Grain parameters:**

| Parameter | Range | Control Source |
|-----------|-------|---------------|
| Grain rate | 0-500 grains/sec | Tilt angle (accelerometer Y) |
| Grain duration | 2-50 ms | Random distribution |
| Grain pitch | 0.8×-1.5× base | Random (Gaussian) |
| Grain amplitude | 0.1-1.0 | Random + tilt-dependent |
| Grain pan | -1.0 to +1.0 | Position along tube + random |
| Grain shape | Hann/Gaussian window | Fixed |

### Layered Noise Approach

Complement granular synthesis with filtered noise layers:

**Layer 1 — Continuous Rain Base:**
- Pink noise generator
- Amplitude modulated by tilt angle
- Bandpass filtered (200-4000 Hz)
- Random amplitude modulation for "patter" effect

**Layer 2 — Individual Drops:**
- Short noise bursts (5-20ms) triggered stochastically
- Higher frequency content (2000-8000 Hz)
- Sparse at low tilt, dense at high tilt

**Layer 3 — Tube Resonance:**
- Low-frequency resonant filter modeling tube cavity
- Driven by the combined output of Layer 1 + 2
- Adds warmth and body

**Layer 4 — Stereo Movement:**
- Slowly panning the sound source along tilt direction
- Simulates pellets physically moving through the tube

### Synthesis Reference: Generating Rain with Pure Synthesis (Audiokinetic)

Key findings from professional rain sound design:
1. Rain sound = millions of **micro-bubbles** merging per second
2. Each raindrop/bubble is a **sine wave with modulated pitch**, duration ~10-20 ms
3. The aggregate approaches **pink noise**
4. Split into layers: high-frequency "boiling" + mid-frequency body + stereo noise + individual drops
5. Use **random modulators on filter frequency** for bubble-like quality
6. **Parametric EQ with LFO-modulated band** creates natural movement

### Alternative Approaches

1. **Filtered noise with modulation** — simplest, lowest CPU, less organic
2. **Particle system simulation** — model actual pellets bouncing (highest fidelity, highest CPU)
3. **Sample-based with crossfading** — record real rain stick at different angles, crossfade
4. **Stochastic synthesis** — Markov chain-based event triggering

---

## 3. Real-World Frequency/Acoustic Data

### Rain Stick Sound Spectrum

The rain stick does not produce pitched tones, but its spectrum has characteristic shapes:

**Typical spectral energy distribution:**
```
Energy
  │
  │████
  │██████
  │████████
  │██████████
  │████████████
  │██████████████
  │████████████████
  │██████████████████
  │████████████████████
  └──────────────────────── Frequency
   100   500  1k  2k  5k  10k Hz
   
   Approximately pink noise (1/f) with
   resonant peak around tube fundamental
```

**Tube resonance frequencies** (for common sizes):
| Tube Length | Fundamental Resonance | Character |
|------------|----------------------|-----------|
| 40 cm (16") | ~430 Hz | Short, bright |
| 60 cm (24") | ~285 Hz | Medium, balanced |
| 80 cm (32") | ~215 Hz | Long, warm |
| 100 cm (40") | ~170 Hz | Very long, deep |
| 120 cm (48") | ~140 Hz | Extra long, bass-heavy |

*Approximate — open tube resonance: f = c/(2L), c ≈ 343 m/s*

### Cascade Timing Data

| Tilt Angle | Cascade Duration | Density | Perceptual Quality |
|-----------|-----------------|---------|-------------------|
| 5-10° | 20-30 sec | Very sparse | Individual drops, contemplative |
| 15-25° | 10-15 sec | Sparse-medium | Light drizzle |
| 30-45° | 5-10 sec | Medium | Steady rain |
| 50-70° | 3-5 sec | Dense | Heavy rain |
| 75-90° | 1-3 sec | Very dense | Downpour, approaching noise |

### Sound Healing Frequency Bands

| Band | Frequency | Rain Stick Contribution |
|------|-----------|----------------------|
| Delta waves | 0.5-4 Hz | Amplitude modulation from slow tilt |
| Theta waves | 4-8 Hz | Grain density fluctuations at slow rates |
| Alpha waves | 8-13 Hz | Natural flutter of cascading particles |
| White/pink noise spectrum | 20-20,000 Hz | Full broadband coverage for masking |

---

## 4. Sound Healing Applications

### Therapeutic Uses

1. **Stress Reduction & Relaxation**
   - Broadband noise mimics natural rain — deeply calming, universally recognized
   - Activates the **parasympathetic nervous system** (relaxation response)
   - Continuous sound provides a "sonic blanket" that reduces anxiety
   - Can lower cortisol levels with regular use

2. **Sleep Aid**
   - Pink noise spectrum is proven to improve sleep quality
   - Continuous, non-threatening sound masks disruptive environmental noise
   - Slow tilting creates gradual onset perfect for sleep induction
   - Can be used as part of bedtime ritual

3. **Meditation Enhancement**
   - Provides consistent background sound that masks distracting noises
   - Sound doesn't demand attention — supports mindful awareness
   - Variable density gives meditator a focal point without requiring concentration
   - Natural sound helps connect to nature even indoors

4. **Brainwave Entrainment**
   - The gentle, continuous sound promotes alpha brainwave states (8-13 Hz)
   - Slow, rhythmic tilting can entrain brainwaves to theta states (4-8 Hz)
   - Transition from dense to sparse cascade mirrors transition from beta to alpha states

5. **Emotional Release**
   - Rain associations with cleansing and renewal
   - The "washing" quality of the sound supports emotional processing
   - Sound baths featuring rain sticks often used for grief work and letting go

6. **Physical Benefits**
   - Can contribute to lowering blood pressure and heart rate
   - Alleviates tension headaches through relaxation response
   - Reduces muscle tightness associated with chronic stress

### Practitioner Techniques

| Technique | Description | Application |
|-----------|-------------|-------------|
| **Slow continuous tilt** | Gentle 180° rotation over 20-30 seconds | Deep relaxation, sleep induction |
| **Quick flip** | Rapid rotation to produce brief cascade | Attention reset, transition between therapy phases |
| **Circular motion** | Rotate in slow circles, varying tilt | Continuous ambient texture for sound baths |
| **Pendulum swing** | Tilt back and forth rhythmically | Rhythmic entrainment, grounding |
| **Progressive density** | Start sparse, gradually increase tilt angle | Building energy, crescendo for emotional release |
| **Layering** | Combine with singing bowls or drone instruments | Multi-textured sound bath |
| **Spatial movement** | Move rain stick around client's body | Energy clearing, aura cleansing |

### Sound Bath Integration

The rain stick is typically used as:
- **Transition element** — between singing bowl segments
- **Background texture** — continuous layer under melodic instruments
- **Opening/closing ritual** — symbolizing cleansing with rain
- **Spatial element** — moved around the space for immersive experience

---

## 5. Interaction Design for iOS

### Tilt/Accelerometer Control (Primary Interface)

The rain stick is the most intuitive instrument for accelerometer-based control because its real-world playing technique IS tilting.

```
┌─────────────────────────────────────┐
│          RAIN STICK VIEW            │
│                                     │
│        ┌──────────────────┐         │
│        │  ╔══════════════╗ │        │
│        │  ║ ● ○  ●  ○  ●║ │        │
│        │  ║  ○  ●  ○ ●  ║ │        │
│        │  ║●   ○  ●  ○  ║ │←tilt   │
│        │  ║ ○ ●   ○  ● ║ │  angle  │
│        │  ║  ●  ○ ●  ○  ║ │        │
│        │  ║ ○  ●  ○  ●  ║ │        │
│        │  ╚══════════════╝ │        │
│        └──────────────────┘         │
│                                     │
│    Tilt angle: 34°  Density: ███░░  │
│                                     │
│    [Settings]  [Timer]  [Record]    │
└─────────────────────────────────────┘
```

**Accelerometer mapping:**

```swift
// Core motion data → synthesis parameters
let tiltAngle = atan2(acceleration.y, acceleration.z)  // 0 to π
let normalizedTilt = abs(sin(tiltAngle))  // 0.0 to 1.0

// Map to synthesis
grainDensity = normalizedTilt * maxGrainRate      // 0-500 grains/sec
noiseAmplitude = normalizedTilt * 0.8              // volume follows tilt
cascadeSpeed = normalizedTilt * 2.0                // rate of cascade
```

**Control parameters:**

| Gesture | Parameter | Effect |
|---------|-----------|--------|
| **Tilt forward/back** | Cascade density | More tilt = more "rain" |
| **Tilt speed** | Attack/release | Faster tilt = quicker onset |
| **Rotation** | Stereo pan | Pellets "move" across stereo field |
| **Shake** | Extra density burst | Brief downpour effect |
| **Hold level** | Sustained texture | Steady rain at chosen intensity |
| **Slow return to level** | Gradual fadeout | Natural cascade end |

**Visual feedback:**
- Animated particles cascading inside the tube, matching tilt direction
- Particle density and speed match audio output
- Tube orientation follows device orientation
- Ambient glow/water ripple effects around the instrument
- Background color shifts (darker = more rain)

### Touch Controls (Secondary)

For accessibility or when motion control isn't desired:
- **Vertical slider** → Controls tilt angle equivalent
- **Tap on tube** → Triggers individual drop sounds
- **Two-finger rotation** → Simulates physical rotation
- **Pinch** → Controls tube length (pitch/duration)

### Timer/Loop Mode

For meditation and sleep:
- Set duration (5, 10, 15, 30, 60 minutes)
- Auto-loop: continuous cascade with gentle variation
- Gradual fade-out at end of timer
- Can run in background with screen off

---

## 6. Synthesis Pseudocode (Swift-like)

```swift
class RainStickSynthesizer {
    let sampleRate: Float = 44100.0
    
    // === Grain Engine ===
    struct Grain {
        var buffer: [Float]       // pre-computed grain waveform
        var position: Int = 0     // playback position
        var isActive: Bool = false
        var amplitude: Float = 1.0
        var pan: Float = 0.0      // -1 left, +1 right
        var playbackRate: Float = 1.0
    }
    
    // Grain pool (pre-allocate for real-time safety)
    var grains: [Grain] = []
    let maxActiveGrains: Int = 64
    
    // Grain templates (pre-computed impact sounds)
    var grainTemplates: [[Float]] = []
    
    // Parameters driven by accelerometer
    var tiltAngle: Float = 0.0          // 0.0 (level) to 1.0 (vertical)
    var rotationRate: Float = 0.0       // rotation speed
    var cascadePosition: Float = 0.5    // 0=top, 1=bottom (stereo movement)
    
    // Noise generator for background
    var pinkNoiseFilter = PinkNoiseFilter()
    
    // Tube resonance filter
    var tubeFilter = BiquadFilter()  // bandpass at tube resonance
    
    // Internal state
    var grainAccumulator: Float = 0.0
    var timeSinceLastGrain: Float = 0.0
    
    init() {
        // Pre-generate grain templates
        generateGrainTemplates()
        
        // Initialize grain pool
        grains = (0..<maxActiveGrains).map { _ in
            Grain(buffer: [], isActive: false)
        }
        
        // Configure tube resonance (~250 Hz for medium tube)
        tubeFilter.configure(
            type: .bandpass,
            frequency: 250.0,
            q: 2.0,
            sampleRate: sampleRate
        )
    }
    
    // === Pre-generate grain templates ===
    func generateGrainTemplates() {
        // Create several variations of impact sounds
        for _ in 0..<8 {
            let grainLength = Int.random(in: 88...882)  // 2-20ms at 44.1kHz
            var grain = [Float](repeating: 0, count: grainLength)
            
            for i in 0..<grainLength {
                let t = Float(i) / Float(grainLength)
                
                // Noise burst with exponential decay
                let noise = Float.random(in: -1...1)
                let envelope = exp(-t * 8.0) * sin(Float.pi * t)  // attack + decay
                grain[i] = noise * envelope
            }
            
            // Apply slight bandpass to vary tonal character
            let centerFreq = Float.random(in: 1000...5000)
            grain = applyBandpass(grain, centerFreq: centerFreq, q: 1.5)
            
            grainTemplates.append(grain)
        }
    }
    
    // === Main process function (called per sample) ===
    func generateStereoSample() -> (Float, Float) {
        // Calculate grain trigger rate based on tilt
        let grainRate = calculateGrainRate(tilt: tiltAngle)
        
        // Accumulate time and trigger grains stochastically
        grainAccumulator += grainRate / sampleRate
        
        while grainAccumulator >= 1.0 {
            grainAccumulator -= 1.0
            triggerGrain()
        }
        
        // Sum all active grains
        var monoSum: Float = 0.0
        var leftSum: Float = 0.0
        var rightSum: Float = 0.0
        
        for i in 0..<grains.count {
            guard grains[i].isActive else { continue }
            
            // Read grain sample with playback rate
            let sampleIndex = Int(Float(grains[i].position) * grains[i].playbackRate)
            if sampleIndex >= grains[i].buffer.count {
                grains[i].isActive = false
                continue
            }
            
            let sample = grains[i].buffer[sampleIndex] * grains[i].amplitude
            grains[i].position += 1
            
            // Apply pan
            let pan = grains[i].pan
            leftSum += sample * (1.0 - pan) * 0.5
            rightSum += sample * (1.0 + pan) * 0.5
            monoSum += sample
        }
        
        // === Background noise layer ===
        let bgNoise = pinkNoiseFilter.generate() * tiltAngle * 0.2
        leftSum += bgNoise * Float.random(in: 0.8...1.0)
        rightSum += bgNoise * Float.random(in: 0.8...1.0)
        
        // === Apply tube resonance ===
        let resonance = tubeFilter.process(monoSum)
        leftSum += resonance * 0.3
        rightSum += resonance * 0.3
        
        return (leftSum, rightSum)
    }
    
    // === Calculate grain rate from tilt ===
    func calculateGrainRate(tilt: Float) -> Float {
        // Non-linear mapping: gentle at low tilt, aggressive at high tilt
        // Simulates physics: more gravity component = faster cascading
        let mapped = pow(tilt, 1.5)  // slight exponential curve
        
        // Sparse individual drops at low tilt → dense rain at high tilt
        let minRate: Float = 2.0     // occasional drops even at slight tilt
        let maxRate: Float = 400.0   // dense rain at full tilt
        
        return tilt < 0.05 ? 0 : minRate + mapped * (maxRate - minRate)
    }
    
    // === Trigger a single grain ===
    func triggerGrain() {
        // Find an inactive grain slot
        guard let slotIndex = grains.firstIndex(where: { !$0.isActive }) else {
            return  // all slots full, skip
        }
        
        // Select random template
        let template = grainTemplates.randomElement()!
        
        // Randomize parameters for organic sound
        grains[slotIndex].buffer = template
        grains[slotIndex].position = 0
        grains[slotIndex].isActive = true
        grains[slotIndex].amplitude = Float.random(in: 0.3...1.0) * tiltAngle
        grains[slotIndex].playbackRate = Float.random(in: 0.7...1.4)  // pitch variation
        
        // Pan based on cascade position + randomization
        grains[slotIndex].pan = (cascadePosition - 0.5) * 1.5
                               + Float.random(in: -0.3...0.3)
    }
    
    // === Accelerometer Update (called from CMMotionManager) ===
    func updateFromMotion(gravity: (x: Double, y: Double, z: Double)) {
        // Calculate tilt from gravity vector
        let rawTilt = Float(sqrt(gravity.x * gravity.x + gravity.y * gravity.y))
        
        // Smooth the tilt (low-pass filter to avoid jitter)
        let smoothing: Float = 0.1
        tiltAngle = tiltAngle + smoothing * (rawTilt - tiltAngle)
        tiltAngle = max(0, min(1, tiltAngle))
        
        // Calculate cascade direction for stereo
        let angle = Float(atan2(gravity.x, gravity.y))
        cascadePosition = (angle / Float.pi + 1.0) * 0.5  // normalize to 0-1
    }
    
    // === Pink noise generator ===
    struct PinkNoiseFilter {
        // Voss-McCartney algorithm (simplified)
        var b0: Float = 0, b1: Float = 0, b2: Float = 0
        var b3: Float = 0, b4: Float = 0, b5: Float = 0
        
        mutating func generate() -> Float {
            let white = Float.random(in: -1...1)
            b0 = 0.99886 * b0 + white * 0.0555179
            b1 = 0.99332 * b1 + white * 0.0750759
            b2 = 0.96900 * b2 + white * 0.1538520
            b3 = 0.86650 * b3 + white * 0.3104856
            b4 = 0.55000 * b4 + white * 0.5329522
            b5 = -0.7616 * b5 - white * 0.0168980
            let pink = b0 + b1 + b2 + b3 + b4 + b5 + white * 0.5362
            return pink * 0.11  // normalize
        }
    }
}

// === CMMotionManager Integration ===
import CoreMotion

class RainStickViewController {
    let motionManager = CMMotionManager()
    let synth = RainStickSynthesizer()
    
    func startMotionUpdates() {
        motionManager.deviceMotionUpdateInterval = 1.0 / 60.0  // 60 Hz
        motionManager.startDeviceMotionUpdates(to: .main) { [weak self] motion, error in
            guard let motion = motion else { return }
            self?.synth.updateFromMotion(gravity: (
                x: motion.gravity.x,
                y: motion.gravity.y,
                z: motion.gravity.z
            ))
        }
    }
}

// === Timer/Loop Mode for Meditation ===
class RainStickMeditationMode {
    var duration: TimeInterval = 600  // 10 minutes
    var autoVariation: Bool = true
    
    func generateAutoTilt(time: TimeInterval) -> Float {
        // Gentle sine-wave variation for auto mode
        let base: Float = 0.4  // medium rain
        let variation: Float = 0.2
        let slow = sin(Float(time) * 0.1) * variation  // ~60 sec cycle
        let medium = sin(Float(time) * 0.3) * variation * 0.5  // ~20 sec cycle
        
        var tilt = base + slow + medium
        
        // Fade out in last 30 seconds
        let fadeStart = Float(duration - 30)
        if Float(time) > fadeStart {
            let fadeProgress = (Float(time) - fadeStart) / 30.0
            tilt *= (1.0 - fadeProgress)
        }
        
        return max(0, min(1, tilt))
    }
}
```

### Key Implementation Notes for iOS

1. **Core Motion**: Use `CMMotionManager.deviceMotion` (not raw accelerometer) for gravity-compensated tilt
2. **Background audio**: Must configure `AVAudioSession` for background playback in timer/meditation mode
3. **Battery**: Accelerometer updates at 60 Hz is sufficient; reduce to 30 Hz to save battery
4. **Haptics**: Consider subtle haptic feedback (Core Haptics) on individual grain triggers at low density
5. **Screen off**: Support background audio playback for sleep mode
6. **Pre-compute grains**: Generate grain templates at app startup, not in real-time audio thread

---

## References

1. Wikipedia — "Rainstick" article
2. Healing Sounds — "Healing Benefits of Rainsticks for Stress Relief & Meditation"
3. Audiokinetic Blog — "Generating Rain With Pure Synthesis" (Wwise implementation)
4. Wikipedia — "Granular Synthesis"
5. Cornell Research — "Toward Animating Water with Complex Acoustic Bubbles"
6. Roads, Curtis (2001) — "Microsound" (MIT Press) — granular synthesis theory
7. Sound Healers Association — rainstick therapeutic applications
8. Holland, James (2003) — "Practical Percussion" (Scarecrow Press)
