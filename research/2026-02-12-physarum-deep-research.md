# Physarum Simulation: A Comprehensive Technical Reference
## Agent-Based Slime Mold Modeling and Generative Art
*Deep Research — 642 sources — Claude.ai Research Mode, 2026-02-12*

**The Physarum polycephalum agent-based model is a deceptively simple system—particles sense, turn, move, deposit, and diffuse—that produces an extraordinary range of emergent visual patterns spanning networks, spots, waves, mazes, coral, and crystalline structures.** The model's power stems from the coupling between discrete Lagrangian agents and a continuous Eulerian trail field, creating feedback loops that mirror reaction-diffusion dynamics without requiring differential equations. Jeff Jones' foundational 2010 paper established the core algorithm with just 6–7 parameters, while Sage Jenson's "36 Points" system transformed it into a generative art medium by making every parameter a nonlinear function of local trail concentration, expanding the design space to 20 dimensions per configuration. This report covers the mathematical foundations, parameter space structure, resolution scaling, automated discovery methods, model extensions, and GPU rendering pipelines for real-time artistic Physarum.

## The Jones Algorithm and Its Six-Step Core Loop

Jeff Jones' 2010 paper "Characteristics of Pattern Formation and Evolution in Approximations of Physarum Transport Networks" (*Artificial Life* 16(2):127–153) defines a dual-layer hybrid model: a population of discrete agents, each with position **(x, y)** and heading **θ**, coupled to a 2D scalar trail map **T[x,y]** representing chemoattractant concentration.

Each agent executes six sub-steps per timestep:

**Step 1 — Sensing.** Three sensors sample the trail map at front-left, front, and front-right positions, offset from the agent by sensor distance **SO** at angles **±SA** from heading:

```
FL = T[x + SO·cos(θ + SA), y + SO·sin(θ + SA)]
F  = T[x + SO·cos(θ),      y + SO·sin(θ)]
FR = T[x + SO·cos(θ - SA), y + SO·sin(θ - SA)]
```

**Step 2 — Rotation.** The heading updates based on a max-selection rule: if the front sensor reads highest, continue straight; if both lateral sensors exceed the front, randomly turn left or right by rotation angle **RA**; otherwise, turn toward whichever lateral sensor reads higher.

**Step 3 — Movement.** The agent advances: `(x, y) ← (x + SS·cos(θ), y + SS·sin(θ))` where **SS** is the step size. Jones' original model rejects moves into occupied cells (collision detection), though most artistic implementations omit this.

**Step 4 — Deposition.** Trail is deposited at the agent's current position: `T[x,y] += depositAmount`.

**Step 5 — Diffusion.** A **3×3 mean filter** (box blur) is applied to the entire trail map, approximating isotropic diffusion.

**Step 6 — Decay.** The trail map is multiplicatively attenuated: `T[x,y] *= decayFactor` where decayFactor ∈ (0, 1).

The seven core parameters are **SA** (sensor angle), **SO** (sensor offset/distance), **RA** (rotation angle), **SS** (step size), **depositAmount**, **decayFactor**, and the diffusion kernel size. Jones found that only **three parameters—SA, RA, and SO—significantly affect patterning**, with population density and decay rate as secondary influences. This makes the system's effective dimensionality remarkably low despite its rich output space.

### Jenson's Power-Law Modulation: The 36 Points Formula

Sage Jenson's critical innovation, implemented in the "36 Points" series (2019–2022, released on Feral File), makes each simulation parameter a **nonlinear function of the local trail concentration** at the particle's position:

```
parameter(x) = base + amplitude × pow(x, exponent)
```

Here **x = T[particle_position]** is the trail value directly beneath the particle (not at the sensor). The three coefficients—**base**, **amplitude**, and **exponent**—are specified for each of the core parameters (sensor distance, sensor angle, rotation angle, move distance), yielding **14 core parameters** in Bleuje's reimplementation. Bleuje added a **15th parameter**: a rescaling factor for the sensed value x. Jenson's original 36 Points uses **20 parameters per "Point"** (each keyboard key 0–9, A–Z maps to a point in ℝ²⁰), with additional parameters controlling deposit behavior, respawn timing, and decay.

The power-law exponent creates qualitatively different feedback regimes:
- When **exponent > 1**, parameters respond disproportionately to high trail concentrations, amplifying positive feedback and producing **dense clustering, thick networks, and crystalline collapse**.
- When **exponent < 1**, parameters respond more to low concentrations, promoting **uniform exploration and fine tendrils**.
- When **exponent = 1**, the relationship is linear.

Different exponents applied to different parameters simultaneously generate the complex, evolving dynamics visible in Jenson's work—particles in dense trail regions behave fundamentally differently from those in sparse regions, creating multi-scale structure within a single parameter configuration.

### Phase Transitions and Connections to Reaction-Diffusion Theory

Jones explicitly demonstrated that his single-level agent model reproduces **a wide range of Turing-type reaction-diffusion patterns** without differential equations. Under **chemoattraction** (the default), agents produce reticulated networks, labyrinthine patterns, irregular spots, and planar sheets. Under **chemorepulsion** (agents flee deposited trail), the system generates **regular periodic spot arrays and stripe patterns**—the hallmark Turing patterns arising from activator-inhibitor dynamics. The agents act simultaneously as activator (depositing trail) and inhibitor (competing for space and consuming resources through their movement).

**No analytical solutions exist** for the Jones/Physarum agent model. The system is inherently discrete, stochastic, and nonlinear. The closest analytical framework is the qualitative mapping to reaction-diffusion PDEs, but no exact correspondence has been established. The MCPM (Monte Carlo Physarum Machine) by Elek et al. (*Artificial Life* 28(1):22–57, 2022) provides a probabilistic generalization with a mutation probability `P_mut = d₁^SE / (d₀^SE + d₁^SE)` that smoothly interpolates between deterministic max-selection (SE → ∞) and random exploration (SE → 0), enabling more formal analysis of the transition between pattern regimes.

## How the SA/RA Ratio Governs Pattern Topology

The parameter space, while nominally 12–20 dimensional, has an intrinsic dimensionality of approximately **3–5**. The **SA × RA interaction** is the single most critical determinant of pattern type:

- When **RA > SA** (e.g., SA=22.5°, RA=45°), the system produces **dynamic branching networks** that never fully stabilize—networks continuously bifurcate and sprout new trails, exhibiting characteristics of deterministic chaos with sensitivity to initial conditions and complex aperiodic dynamics.
- When **RA = SA** (e.g., both 45°), the system converges to **stable hexagonal networks**—lacunae contract and the network minimizes toward a regular hexagonal tiling, representing a clear attractor in the dynamical system.
- When **RA < SA**, increased contraction drives the system toward **spot formation and isolated dense clusters**.

The **sensor offset SO** acts as a pure scaling parameter: small SO (~3 px) produces fine-grained structures with narrow network paths; large SO (~15–25 px) produces coarse, wide structures with "vacancy islands" within blobs. This parameter controls pattern wavelength without changing pattern type.

**Population density** determines connectivity: low density yields sparse, disconnected islands; medium density (3–15% of pixels) produces connected networks; high density creates dense, sheet-like masses. The **decay factor** controls trail persistence, effectively tuning the temporal coupling between agents.

The boundary between the dynamic (RA > SA) and stable (RA = SA) regimes is relatively **sharp**, suggesting a bifurcation-like transition. Other regime boundaries are more gradual—the continuum from reticulated networks through labyrinthine patterns to spots as coupling decreases is smooth.

### Pattern Type Reference Table

| Pattern type | SA | RA | SO | Density | Stability |
|---|---|---|---|---|---|
| Dynamic reticulated network | 22.5° | 45° | 9 | 15% | Unstable (chaotic) |
| Stable hexagonal network | 45° | 45° | 9 | 15% | Attractor |
| Labyrinthine | ~30–40° | ~45° | 9 | ~10% | Transitional |
| Irregular spots/islands | High | Low | 9 | Low | Metastable |
| Regular spot arrays | — | — | — | — | Chemorepulsion, stable |
| Stripe patterns | — | — | — | — | Chemorepulsion, stable |

The MCPM paper by Elek et al. documented additional morphological features in their continuous 3D model: **tubular filaments, loops, fuzzy membranes, thin fibers, branches, and undulating pulses**, with "unstable polyphorms" representing phase transitions between configurations. They found sensing angle and sensing distance are the most significant shape determinants, consistent with Jones' findings.

## Resolution-Invariant Scaling: A Complete Framework

Making Physarum simulations produce visually identical patterns across different resolutions requires careful parameter scaling. The fundamental principle: **spatial parameters scale linearly with resolution, angular and temporal parameters remain fixed, and particle count scales with area**.

For a scale factor **s = R_new / R_reference**:

| Parameter | Scaling rule | Rationale |
|---|---|---|
| Sensor distance (SO) | SO × s | Spatial measurement in pixels |
| Step size (SS) | SS × s | Spatial measurement in pixels |
| Sensor angle (SA) | Unchanged | Angular, dimensionless |
| Rotation angle (RA) | Unchanged | Angular, dimensionless |
| Particle count (N) | N × s² | Maintains particles-per-pixel density |
| Deposit amount | Unchanged | Per-particle, density compensates |
| Decay factor | Unchanged | Temporal rate per timestep |
| Blur kernel size | n × s (or use fixed 3×3 with s² passes) | Maintains physical diffusion rate |

The diffusion scaling deserves special attention. A 3×3 box blur has effective variance **σ² = (2/3)Δx²** per pass, where Δx is the grid spacing. When resolution doubles, Δx halves, so each pass diffuses 4× less physical distance. To maintain the same physical diffusion rate per timestep, either **scale the kernel size linearly** (3×3 → 5×5 or 7×7 at 2× resolution) or **increase pass count quadratically** (1 pass → 4 passes at 2× resolution). The quadratic pass scaling is GPU-friendlier since 3×3 kernels are highly optimized, but introduces a critical caveat: if decay is applied per blur pass rather than once per timestep, the effective decay becomes `decayFactor^(s²)`, which will drastically over-decay at higher resolutions. The solution is to **apply decay only once after all blur passes**, or adjust: `decayFactor_new = decayFactor_ref^(1/s²)`.

**Particle density thresholds** matter significantly for pattern formation:
- Below ~**0.1 particles/pixel**, trails are too sparse for coherent patterns.
- At **0.5–1.0 particles/pixel** (Jones' original specification), clean network/filamentary patterns form.
- At **1–5 particles/pixel**, rich textures emerge (labyrinthine, spotted, reticulated).
- At **5–10+ particles/pixel** (Bleuje uses ~6.3 at 1920×1088), very dense patterns form but risk trail saturation.
- Above **~20 particles/pixel**, diminishing returns as the trail map becomes uniformly bright.

No established standard framework for resolution-independent agent-based simulations exists in the literature—the scaling relationships above are derived from first principles of diffusion equation discretization, scale-space theory, and practical observations from implementers.

## Automated Discovery of Interesting Parameter Sets

The most promising approach for systematically exploring the Physarum parameter space is **MAP-Elites** (Mouret & Clune, 2015), a quality-diversity algorithm that maintains a grid archive indexed by behavioral descriptors, where each cell stores the highest-fitness solution found for that behavioral niche.

For Physarum, the genotype is the ~15–20 parameter vector. **Behavior dimensions** could include fractal dimension (1.0–2.0), network density (fraction of occupied trail cells), branching factor, spatial coverage, or cluster count. The **fitness function** combines automated aesthetic measures: fractal dimension near the human-preferred range of **D ≈ 1.3–1.5** (Taylor et al.), structural complexity, JPEG compression ratio as a complexity proxy, and a non-triviality filter. The algorithm randomly mutates occupied archive cells and places offspring in the appropriate behavior cell if they exceed the existing occupant's fitness, naturally producing a diverse collection of high-quality parameter sets.

**Pyribs** (github.com/icaros-usc/pyribs) is the best-maintained quality-diversity library, implementing CMA-ME, CMA-MAE, and other MAP-Elites variants with a modular ask-tell interface. For simpler baselines, **pymap_elites** (github.com/resibots/pymap_elites) provides a reference implementation in approximately one page of code. The JAX-based **QDax** library enables GPU-accelerated large-scale experiments.

**Novelty search** (Lehman & Stanley, 2011) offers an alternative: abandon fitness entirely and reward only behavioral novelty, computed as the average distance to the k-nearest neighbors in a behavior archive. For Physarum, behavior vectors could encode image statistics (mean, variance, fractal dimension, edge density, spatial frequency spectrum) or CNN feature vectors from a pretrained network. This approach naturally fills the archive with diverse patterns without requiring aesthetic fitness definitions. **Minimal Criteria Novelty Search** adds a quality threshold (e.g., non-trivial pattern formation) while optimizing for diversity.

**Interactive Evolutionary Computation** (Takagi, 2001) replaces automated fitness with human judgment: present populations of 15–40 Physarum configurations, let the user select favorites, breed the next generation. The practical limitation is **user fatigue**—humans tire after evaluating many generations. The optimal hybrid approach uses automated aesthetic filtering to pre-select candidates, then presents the top results for human selection.

A complete pipeline would: (1) sample the parameter space via MAP-Elites with automated fitness, (2) apply UMAP to the output image feature vectors to create a 2D navigation map, (3) present the archive to a human for interactive refinement, and (4) use the selected favorites as seeds for focused local search. The `pyribs` + `umap-learn` Python ecosystem provides all necessary components.

## Multi-Species Interactions and Other Extensions

### Multi-Species Simulations

Multiple particle types with independent parameters can share the simulation space through **cross-trail sensing**. Each species maintains a separate trail map (often mapped to R, G, B channels). For inter-species repulsion, a red particle computes its effective signal as `rtrail[x,y] - gtrail[x,y] - btrail[x,y]`, creating competitive exclusion that produces striking territorial patterns. A more general **interaction matrix** approach defines attraction/repulsion coefficients between every species pair, generating temporary per-species "view" maps by weighting all trail maps according to the matrix.

Notable multi-species implementations include fogleman/physarum (Go, multiple species), MNoichl/physarum (Python, interacting populations), and gubebra's WebGPU version with browser-playable presets.

### Reaction-Diffusion Coupling and Hybrid Models

Jones' model is itself a Lagrangian-Eulerian hybrid formally related to the **Material Point Method** (as Sage Jenson observed). The connection to reaction-diffusion is not just analogy—the agent population with deposit-diffuse-decay dynamics creates the same mathematical structure as activator-inhibitor systems. Andrew Adamatzky (2009) explicitly modeled Physarum as an encapsulated reaction-diffusion computer using the Oregonator equation. Coupling Physarum agents with explicit Gray-Scott or Fitzhugh-Nagumo dynamics on the trail map extends the pattern vocabulary further.

### Spectral Methods for Trail Processing

FFT-based convolution replaces pixel-based Gaussian blur with frequency-domain multiplication, achieving **O(N² log N) complexity regardless of kernel size** versus O(N² K²) for spatial convolution. This enables arbitrarily large diffusion radii, custom frequency-domain filters (band-pass filtering to promote specific pattern scales, anisotropic diffusion kernels), and efficient reaction-diffusion coupling where the diffusion operator becomes a simple eigenvalue multiplication. GPU FFT libraries (cuFFT, clFFT, Metal Performance Shaders) enable real-time spectral processing. FFT naturally handles periodic (wrapping) boundary conditions.

### 3D Physarum and Volumetric Visualization

The most significant 3D implementation is **Polyphorm** (github.com/CreativeCodingLab/Polyphorm) by Elek et al. at UC Santa Cruz, which extends Jones' model to the **Monte Carlo Physarum Machine** and applies it to reconstructing the cosmic web from galaxy survey data (37,600 galaxies from SDSS). The system operates on a **720³ voxel grid with 10M agents** and uses progressive volumetric Monte Carlo path tracing for visualization.

Sage Jenson created 3D Physarum forms during a Nervous System residency (2022), working with **1080³ volumes (over 1 billion voxels)** using custom C++/CUDA code and a GPU path tracer employing Beer's law transmittance (~19.5 trillion texture reads per 8K image). Physical outputs were produced through bronze casting of 3D-printed wax resin forms.

Other 3D implementations include Barbelot/Physarum3D (Unity, volume ray-casting) and Physarealm (Rhino/Grasshopper plugin for architectural design from Tsinghua University).

### Adaptive Parameters and Heterogeneous Environments

Jenson's power-law modulation is itself the most important adaptive parameter technique—particles in dense trail regions develop different behaviors than those in sparse regions, creating **self-organization at the parameter level**. Environmental heterogeneity can be introduced through attraction/repulsion maps projected into the trail field, dynamic time-varying attractors, and boundary conditions (periodic/wrapping, reflecting, absorbing, or Jenson's bounded respawn technique where particles far from center have higher respawn probability).

## Real-Time GPU Rendering Pipeline for Artistic Physarum

### Three-Shader Architecture

The standard GPU pipeline, exemplified by Bleuje's interactive-physarum implementation, uses three compute shader passes per frame:

1. **Move shader** (`computeshader_move.glsl`): Each thread handles one particle—sense trail at three sensor positions using bilinear sampling, apply rotation rule, advance position, increment an atomic counter at the particle's pixel. On a 1920×1088 map with **~13.1M particles, this runs at 60 FPS on an RTX 4060**.

2. **Deposit shader** (`computeshader_deposit.glsl`): Reads atomic counters, adds trail to the trail map, and simultaneously generates the display image by non-linearly mapping particle counts to brightness.

3. **Diffusion shader** (`computeshader_diffusion.glsl`): Applies 3×3 box blur (separable for efficiency) and multiplicative decay. Uses **ping-pong double-buffered textures**—swap read/write buffers each frame to avoid race conditions.

Agent data is stored in **SSBOs** (Shader Storage Buffer Objects) or compute buffers as vec2 pairs (position + direction). The trail map uses `uimage2D` with **imageAtomicAdd** for deposit to handle race conditions when multiple agents deposit to the same pixel. An alternative approach (Jon Baker, jbaker.graphics) uses vertex shaders to process agents and render them as GL_POINTS, with compute shaders handling trail diffusion.

### Trail Map Visualization and the Dynamic Range Problem

Trail values span an enormous dynamic range—areas with many overlapping agents accumulate very high values while sparse areas remain near zero. Direct linear mapping to brightness loses all detail. Three approaches handle this:

- **Logarithmic mapping**: `brightness = log(1 + trail) / log(1 + maxTrail)` — compresses high values while preserving low-end detail.
- **Gamma/power curve**: `brightness = pow(trail / maxTrail, γ)` with γ < 1 (e.g., 0.4) to brighten dark areas.
- **Reinhard tone mapping**: `brightness = trail / (trail + 1)` — simple sigmoid that naturally compresses HDR values into [0,1].
- For maximum quality, render to **floating-point textures** (R16F or R32F) and apply filmic tone mapping (ACES: `(x(2.51x + 0.03)) / (x(2.43x + 0.59) + 0.14)`).

Bleuje's key coloring technique maintains **two trail map channels**: the current trail and a time-delayed copy, creating temporal differentiation that maps to different color axes. Color LUTs (1D gradient textures sampled by trail intensity) enable instant style changes without affecting simulation dynamics. This cleanly **separates pattern topology from visual style**: the sensor/movement parameters determine *what* patterns emerge while deposit rate, decay, diffusion, color mapping, and post-processing determine *how* they look.

### Post-Processing for Artistic Output

**Bloom/glow** extracts bright pixels above a threshold, applies multi-pass separable Gaussian blur to them (or progressive mip-chain downsampling for physically-based bloom), then additively blends the result back before tone mapping. **Edge detection** (Sobel/Laplacian on the trail map) creates contour effects highlighting trail boundaries. **Background subtraction** reveals network topology. Multi-species simulations map each species' trail to a color channel (R, G, B), with species interaction creating natural color mixing.

### iOS Metal Optimization

For mobile GPU targets, several Metal-specific optimizations apply:
- **Threadgroup sizing** should be 16×16 or 32×32 for 2D image processing, with processing multiple work items per thread to amortize launch overhead.
- Use the `constant` address space for read-only simulation parameters and `device` for read-write buffers.
- Textures are preferred for trail maps because hardware-accelerated bilinear filtering is free (critical for sensor sampling), but **read-write textures require A11 GPUs or later**.
- Use **half-precision (float16)** where possible, and **R16F format** for single-channel trail maps instead of RGBA to reduce memory bandwidth by 4×.
- Function constants enable shader specialization at compile time, removing dead code paths.

The iOS Metal implementation by robertwaltham (github.com/robertwaltham/simulation-unlimited) demonstrates a working pipeline with 3 particle "flavours" using R/G/B channels, LFO modulation of parameters, and A11+ GPU requirement. Practical particle budgets for 60fps on A-series chips are approximately **500K–2M agents** depending on resolution; reducing trail map resolution (e.g., 512×512) and upscaling for display is an effective optimization. On M-series chips, millions of agents are feasible in real-time.

## Key Repositories and References

Essential implementations:
- **Bleuje/interactive-physarum** (C++/GLSL, openFrameworks) — Most detailed artistic implementation with 36 Points-inspired parameter interpolation — [GitHub](https://github.com/Bleuje/interactive-physarum)
- **SebLague/Slime-Simulation** (C#/HLSL, Unity) — 1.5K stars, the most popular learning reference — [GitHub](https://github.com/SebLague/Slime-Simulation)
- **fogleman/physarum** (Go) — Clean multi-species implementation — [GitHub](https://github.com/fogleman/physarum)
- **CreativeCodingLab/Polyphorm** (C++) — 3D MCPM for cosmic web reconstruction — [GitHub](https://github.com/CreativeCodingLab/Polyphorm)
- **robertwaltham/simulation-unlimited** (Swift/Metal) — iOS Metal implementation — [GitHub](https://github.com/robertwaltham/simulation-unlimited)
- **pyribs** (Python) — Best quality-diversity library for automated parameter discovery — [Docs](https://docs.pyribs.org/en/stable/)
- **AtwoodDeng/Physarum** (Unity) — LUT coloring and 3D volume raymarching — [GitHub](https://github.com/AtwoodDeng/Physarum)

Foundational papers:
- Jones (2010) "Characteristics of Pattern Formation..." — *Artificial Life* 16(2):127–153
- Elek et al. (2022) "Monte Carlo Physarum Machine..." — *Artificial Life* 28(1):22–57
- Sims (1991) "Artificial Evolution for Computer Graphics" — SIGGRAPH
- Bleuje's explanation: https://bleuje.com/physarum-explanation/
- Sage Jenson's writeup: https://cargocollective.com/sagejenson/physarum

## Conclusion

The Physarum agent model occupies a rare position in computational art: simple enough to implement in a weekend, deep enough to sustain years of exploration. The **SA/RA ratio** is the master control for pattern topology, the **power-law trail-dependent parameter modulation** is the key innovation that transforms it from simulation to art medium, and the **effective dimensionality of ~3–5** means the 15–20 dimensional parameter space is navigable despite its apparent complexity.

The most actionable next steps:
1. Applying MAP-Elites with automated aesthetic fitness to systematically chart the parameter space, producing curated archives far beyond Jenson's manually discovered 36 Points
2. Implementing proper resolution-invariant scaling using the s²-particle-count / s-spatial-parameter framework
3. Exploring spectral trail processing to move beyond the 3×3 box blur limitation, enabling multi-scale diffusion dynamics that could unlock entirely new pattern regimes
