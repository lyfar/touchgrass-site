---
topic: Physarum Simulation Parameter Space & Visual Preset Generation
date: 2026-02-12
source: Manual research (Sage Jenson 36 Points, Bleuje physarum-36p, Jeff Jones 2010 paper)
---

# Physarum Parameter Space Research

## Origin: 36 Points by Sage Jenson

Our simulation is based on Sage Jenson's "36 Points" system, which extends Jeff Jones's 2010 Physarum model.

### The Core Formula

For each of the 4 classic parameters (SD, SA, RA, MD), the 36 Points system uses:
```
parameter = base + amplitude × pow(sensedValue, exponent)
```

Where `sensedValue` (x) is the trail map intensity at the particle's position (clamped to 0-1).

This gives **12 parameters** (4 × 3) + 2 sensor bias offsets + 1 sensing scaling factor = **15 parameters**.

### Parameter Layout (our code matches Bleuje's)
```
Index  Name           Role
0      SD0            Sensor Distance base
1      SDE            Sensor Distance exponent
2      SDA            Sensor Distance amplitude
3      SA0            Sensor Angle base
4      SAE            Sensor Angle exponent
5      SAA            Sensor Angle amplitude
6      RA0            Rotation Angle base
7      RAE            Rotation Angle exponent
8      RAA            Rotation Angle amplitude
9      MD0            Move Distance base
10     MDE            Move Distance exponent
11     MDA            Move Distance amplitude
12     SB1            Sensor Bias 1 (vertical offset, absolute)
13     SB2            Sensor Bias 2 (forward offset, relative to heading)
14     SF             Sensing Factor (global scaling of sensed value)
```

## Key Insight: The Exponent Controls Everything

The exponent (E) in `base + amplitude × pow(x, E)` is the CRITICAL parameter:

- **E < 2**: Low selectivity. Even faint trails trigger response → **noise, diffuse patterns**
- **E = 2-8**: Moderate selectivity → **networks, organic forms**
- **E = 8-20**: High selectivity. Only strong trails matter → **highways, channels, distinct structures**
- **E > 20**: Extreme selectivity → **multiscale patterns, crystalline, sharp threshold behaviors**

### Phase Transitions by Exponent Regime

| SD Exponent | SA Exponent | RA Exponent | MD Exponent | Visual Result |
|-------------|-------------|-------------|-------------|---------------|
| High (20+)  | Low (<2)    | Low (<2)    | Low (<2)    | **Networks/highways** (particles only approach strong trails) |
| Low (<2)    | Low (<2)    | High (20+)  | Low (<2)    | **Coral/branching** (rotation only at antinodes) |
| Low (<2)    | Low (<2)    | Low (<2)    | High (12+)  | **Spots/clusters** (speed depends on trail) |
| High (15+)  | Low (<2)    | Low (<2)    | High (15+)  | **Waves** (sensing + speed coupled to trail) |
| Low (<2)    | High (50+)  | Low (<2)    | Low (<2)    | **Maze/channels** (angle gates on trail strength) |
| Med (3-10)  | Med (1-3)   | Med (1-3)   | Med (2-5)   | **Organic** (balanced response) |
| High (30+)  | Low (<3)    | Low (<1)    | Low (<6)    | **Crystalline** (extreme SD threshold) |
| Low (<5)    | Low (<3)    | Low (<1)    | Low (<1.5)  | **Nebula/soft** (minimal reactivity) |

## Bleuje's 24 Reference Points (Verified Working)

From `points_basematrix.h` — these are PROVEN parameter sets:

### Named Points with Visual Character
```
0  "pure_multiscale"          — SA=51.32(exp), complex maze patterns
1  "hex_hole_open"            — SD=28(exp), hexagonal network
2  "vertebrata"               — Static SD=17.92, vertebra-like
3  "star_network"             — RA=20(exp), star branching
4  "enmeshed_singularities"   — RA=3.35 fixed, entangled spots
5  "waves_upturn"             — MD=20(exp), wave-like motion
6  "more_individuals"         — Balanced, individual organisms
7  "sloppy_bucky"             — Loose buckminsterfullerene
8  "massive_structure"        — SB2=7.14, giant structures
9  "speed_modulation"         — MD=12.62(exp), speed-dependent clusters
10 "transmission_tower"       — SD+SA extreme (28+26 exp), tower patterns
11 "ink_on_white"             — RA=12.62(exp), ink-like spreading
12 "vanishing_points"         — Complex multi-param, perspective
13 "scaling_nodule_emergence" — SDA=100, MD=20 amp, nodular
14 "hyp_offset"               — SB1=2.3+SB2=0.82, hyperbolic
15 "strike"                   — SDA=402(!), extreme threshold
16 "clear_spaghetti"          — SA=5.2 fixed, MD=37.74 amp
17-23 "from bleuje"           — Additional tested points
```

## Critical Difference: pixelScaleFactor

**In Bleuje's code**: `sensorDistance` and `moveDistance` are multiplied by `250.0` (hardcoded for ~1280px canvas).

**In our code**: We use `sim.pixelScaleFactor` — but this ONLY scales SD and MD distances. It does NOT scale:
- Deposit amount
- Decay rate
- Dot size
- Blur kernel size
- Sensor bias offsets

### Resolution Independence Fix
For true resolution independence, ALL spatial parameters need scaling:
```
pixelScale = canvasWidth / referenceWidth  (reference = 1280)

Scale by pixelScale:
- sensorDistance (already done)
- moveDistance (already done)
- SB1 (sensor bias vertical)
- SB2 (sensor bias forward)
- dotSize
- Deposit? (needs density normalization: deposit / pixelScale²)
```

Non-spatial parameters stay constant:
- All exponents
- All angles (SA, RA)
- Decay factor
- Sensing factor

## Deposit & Decay: The Rendering Pipeline

### Bleuje's approach (simple, elegant):
```
deposit = sqrt(min(count, 100)) × depositFactor
decay = 0.75 (fixed)
display = tanh(pow(count/10, 1.7))
```

### Our approach (more complex):
```
deposit = sqrt(min(count, 100×dotScale)) × depositFactor × dotScale
decay = per-preset (0.06 - 0.30)
display = pow(tanh(7.5 × pow((count-1)/1000, 0.3)), 8.5) × drawOpacity × dotScale
```

**Key difference**: Bleuje uses FIXED deposit/decay and varies only the 15 particle parameters. We vary EVERYTHING, which creates a combinatorial explosion of tuning.

### Recommendation
Follow Bleuje's approach:
1. Fix deposit factor to ~0.4
2. Fix decay to ~0.75 (Bleuje) or narrow range 0.7-0.85
3. Fix blur to 1 pass of 3×3 kernel
4. Vary ONLY the 15 particle parameters + color mode
5. Dot size and draw opacity can be 2 additional "style" params

This reduces the preset space from ~25 dimensions to ~17, making it tractable.

## Preset Generation Strategy

### Approach 1: Reference Point Interpolation
Pick 2 proven reference points, interpolate between them:
```
preset[i] = refA[i] × (1-t) + refB[i] × t
```
Works because the parameter space is relatively smooth between known-good points.

### Approach 2: Regime-Based Generation (what PhysarumPresetGenerator does)
1. Define visual regimes (network, coral, spots, etc.)
2. Each regime has 2 reference points (from Bleuje's matrix)
3. Seed-based variation (±15%) around references
4. Fixed render settings per regime

### Approach 3: Evolutionary / Genetic Algorithm
1. Start with Bleuje's 24 reference points as population
2. Fitness function: pattern diversity score (e.g., fractal dimension, entropy)
3. Crossover: interpolate parameters between good individuals
4. Mutation: small random perturbation (±10-20%)
5. Selection: user picks favorites

### Approach 4: Dimensionality Reduction (most sophisticated)
1. Collect many working parameter sets
2. Run PCA or t-SNE on the 15D space
3. Find the 3-5 principal axes that explain most variance
4. Map a 2D or 3D "style space" to the full 15D space
5. Users navigate a smooth 2D plane of visual styles

## Interaction Model (Pen/Bg blending)

From Bleuje's interactive-physarum:
```
t = exp(-||P - C||² / σ²)
param = (1-t) × bgParam + t × penParam
```

This is EXACTLY what our code does. The proven approach is:
- bg = "idle" visual state (what you see without touching)
- pen = "active" state (what appears near touch)
- Both should be from the SAME regime but different character

## Sources
- Jeff Jones (2010): "Characteristics of pattern formation and evolution in approximations of Physarum transport networks"
- Sage Jenson: https://sagejenson.com/physarum, https://sagejenson.com/36points/
- Bleuje: https://github.com/Bleuje/physarum-36p, https://bleuje.com/physarum-explanation/
- Bleuje interactive: https://github.com/Bleuje/interactive-physarum
