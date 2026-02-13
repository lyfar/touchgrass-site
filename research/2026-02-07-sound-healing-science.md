# Sound healing science: what holds up and what doesn't

**The evidence for sound-based wellness interventions is real but far more modest than the market suggests.** Binaural beats show moderate effects on procedural anxiety (Hedges' g = 0.45 across 22 RCTs), and singing bowl sessions reliably produce short-term relaxation — but the core mechanism claimed by most products, brainwave entrainment, fails to replicate in the majority of EEG studies. A credible sound healing app can be built on genuine science, but it requires surgical precision in distinguishing evidence-based features from pseudoscientific marketing. This report covers all eight requested research domains, rates the evidence for each, and provides concrete recommendations for building an app that earns trust rather than borrowing it.

---

## 1. Binaural beats have moderate evidence for anxiety, weak evidence for everything else

The strongest evidence tier comes from three meta-analyses. Garcia-Argibay, Santed & Reales (2019, *Psychological Research*) pooled 22 RCTs and found an **overall effect size of Hedges' g = 0.45** — a medium effect across cognition, anxiety, and pain outcomes. Attention tasks showed the strongest signal (g Å 0.58), while theta-frequency beats actually *impaired* memory performance. A 2025 perioperative meta-analysis of 14 trials (n = 1,047) found large reductions in surgical anxiety (SMD = ?1.38), though extremely high heterogeneity (I? = 91.6%) and small-study bias likely inflated these estimates.

The frequency-specific findings break down as follows. **Delta/theta (1–8 Hz)** beats have the most consistent anxiety-reduction evidence across pre-surgical and dental settings. **Alpha (8–13 Hz)** shows moderate promise for stress biomarker reduction. **Beta (13–30 Hz)** results are contradictory — a well-powered 2022 study by Robison et al. provided Bayesian evidence *against* beta beats improving sustained attention. **Gamma (40 Hz)** has the strongest EEG entrainment evidence (confirmed in Fosco et al., 2025, *Nature*) but only modest behavioral effects.

The most critical finding for app developers concerns the *mechanism*. Ingendoh, Posny & Heine's 2023 systematic review in *PLOS ONE* examined 14 rigorous EEG studies: **only 5 (36%) supported the brainwave entrainment hypothesis, while 8 (57%) found contradictory results.** This means the foundational claim of most binaural beat products — that they "entrain" brainwaves to specific states — is not reliably demonstrated. Benefits may stem from relaxation, expectation effects, or the Ganzfeld effect (monotonous sensory input quieting the mind) rather than frequency-specific neural synchronization.

**Evidence ratings:**
- Procedural anxiety reduction: **Moderate**
- Acute pain (perioperative): **Weak-to-moderate**
- Attention/focus: **Weak** (inconsistent across well-designed studies)
- Sleep quality: **Weak** (best double-blind RCT found no significant benefit over music alone)
- Stress/relaxation: **Weak-to-moderate**
- Long-term benefits: **No evidence** (virtually unstudied)

**App recommendations:** Lead with delta/theta beats for relaxation and sleep contexts. Use **carrier frequencies of 200–500 Hz** (optimal for beat perception). Sessions should be at least 9 minutes. Do not claim "proven brainwave entrainment." Frame as "designed to promote relaxation" rather than "scientifically proven to change your brainwaves."

---

## 2. Solfeggio frequencies are numerological constructs, not ancient science

The specific Solfeggio frequencies (396, 417, 528, 639, 741, 852 Hz) were not used by Gregorian monks or derived from any ancient tradition. They were constructed in the 1990s by Dr. Joseph Puleo using **Pythagorean digit reduction applied to verse numbers in the Book of Numbers**, then popularized by Leonard Horowitz in *Healing Codes for the Biological Apocalypse* (1999). All digits sum to 3, 6, or 9 via repeated reduction — a numerological pattern, not a scientific one. Gregorian monks had no concept of Hz measurements; the unit wasn't defined until Heinrich Hertz in 1830.

The widely-cited "528 Hz repairs DNA" claim traces to Horowitz's numerological interpretation, not laboratory science. The most commonly referenced experiment (Glen Rein, ~1998) used complex chanting music, not isolated frequencies, and employed non-standard "scalar audio wave" methodology. In subsequent experiments with sound healer Jonathan Goldman, **a single 528 Hz tuning fork had no effect on DNA** — only the ascending sequence of all nine Solfeggio frequencies showed any effect. The astrocyte cell study (Matuszczak et al., 2017) showed protective effects against ethanol toxicity, not DNA repair. Peer-reviewed research on 528 Hz totals perhaps three small studies in lower-tier journals, all with significant limitations (n = 9 in the most cited human study).

**432 Hz vs. 440 Hz** has slightly better — though still preliminary — evidence. Calamassi & Pomponi's 2019 double-blind crossover pilot (n = 33) found **432 Hz music associated with a 4.79 bpm heart rate decrease** (p = 0.05) versus 440 Hz. A small spinal injury sleep study showed improvement. But effect sizes are tiny, studies are pilot-scale, and no plausible biological mechanism exists for why 8 Hz of tuning difference would produce differential physiological effects.

**How major apps handle Solfeggio content** reveals a clear credibility spectrum. Brain.fm explicitly states Solfeggio frequencies "lack substantial scientific evidence" and their effects "remain unproven." Headspace does not reference them at all. Calm takes a measured middle ground, noting "the science is still far from settled" and framing 528 Hz as a relaxation tool. Insight Timer is flooded with unvetted Solfeggio content including overt "DNA repair" claims. BetterSleep presents the Puleo/Horowitz narrative as established fact.

**Evidence rating:** No credible evidence for specific therapeutic properties of individual Solfeggio frequencies. The 432 Hz tuning question has **very weak** preliminary evidence.

**App recommendation:** Include Solfeggio content — it's enormously popular (millions of searches) — but frame it honestly. Model the Calm approach: "These tones are widely used in meditation and relaxation practices. While research on their specific effects is still developing, many people find them helpful." Place Solfeggio in a "Sound Healing Traditions" section separate from evidence-based content. Never claim DNA repair, cell regeneration, or organ-specific healing.

---

## 3. Singing bowl research shows relaxation effects but fails the placebo test

The most-cited study in the field, Goldsby et al. (2017, n = 62), found large effect sizes for tension reduction (? > 0.14, p < .001) after a 60-minute sound bath. However, **this landmark study had no control group**, used a convenience sample from the Chopra Center, and relied entirely on self-report. Two stronger RCTs exist: Cotoia et al. (2018, n = 60, double-blind) showed significant pre-operative anxiety reduction with objective biomarker confirmation (salivary ?-amylase, HRV), and Rio-Alamos et al. (2023, n = 50) found singing bowls produced a "more evident" relaxation response than progressive muscle relaxation, confirmed by EEG and HRV.

The most revealing study is the **only placebo-controlled trial.** Wepner et al. (2008, n = 54) compared crystal singing bowls to "silent bowls" (placebo) for chronic spinal pain. Both active and placebo groups improved versus no treatment, but **pain relief was not significantly different between real and sham singing bowls.** This suggests the benefit may be non-specific — the relaxation context matters more than the specific frequencies.

The most comprehensive systematic review (Cai et al., 2025, 19 clinical studies) rated **all 9 included RCTs as HIGH risk of bias** using the Cochrane tool, primarily because blinding is essentially impossible with this intervention. Stanhope & Weinstein (2020) found only 4 studies meeting inclusion criteria and concluded: "We cannot recommend singing bowl therapies at this stage." **No controlled clinical studies exist for PTSD, clinical anxiety disorders, or major depression.** No long-term follow-up data exists for any condition.

Proposed mechanisms include parasympathetic activation (supported by HRV data), brainwave entrainment toward alpha/theta states (some EEG support), vagal nerve stimulation (plausible but undemonstrated), and cortisol modulation (limited evidence). The critical unanswered question, as Harvard psychiatrist David Silbersweig noted: whether singing bowls provide benefits beyond any comparably relaxing sensory experience.

**Evidence ratings by condition:**

- Acute stress/tension reduction: **Moderate** (most consistent finding, some physiological confirmation)
- Situational anxiety: **Moderate** (two RCTs with physiological measures)
- Pain management: **Weak** (the only placebo-controlled trial was negative)
- PTSD: **No evidence**
- Sleep: **Weak**
- Clinical depression/anxiety disorders: **No evidence**

**App recommendation:** Sound bath content is safe, subjectively pleasant, and commercially popular. Frame it as a relaxation practice, not a treatment. High-quality recordings of singing bowls with spatial audio rendering could be a strong differentiator. Do not claim efficacy for clinical conditions.

---

## 4. Cymatics is legitimate physics with no therapeutic evidence

Cymatics — visualizing sound patterns via vibrating plates, membranes, or liquid surfaces — is well-established classical acoustics governed by Kirchhoff's biharmonic equation and Chladni's Law. The patterns are beautiful and fully explainable through conventional physics. Ernst Chladni documented the technique in 1787; Hans Jenny coined the term "cymatics" in 1967.

**There is no peer-reviewed evidence supporting therapeutic applications of cymatics.** The "cymatic therapy" method (Peter Guy Manners) claims specific frequencies can cure diseases by "restoring cellular resonance," but no clinical trials support this, and the approach is unregulated. Claims connecting cymatics to "sacred geometry healing" or Masaru Emoto's discredited water memory experiments are pseudoscience. The CymaScope instrument, while visually striking, has been criticized by acoustics experts at the University of Pennsylvania as lacking proper mathematical analysis.

Where cymatics genuinely shines is as an **educational and engagement tool.** Harvard and numerous universities use Chladni plate demonstrations for teaching acoustics. Edinburgh Napier University's CymaSense — a real-time 3D cymatics visualization — showed promise in autism spectrum music therapy, increasing communicative behaviors for both verbal and non-verbal participants (ACM DIS 2017). The mathematical models for generating cymatic patterns are computationally lightweight and feasible on modern phones using GPU shaders.

**App recommendation:** Use cymatics as a visual engagement layer, not a health intervention. Real-time visualization of the user's voice, breath, or session audio as cymatic patterns is scientifically accurate, visually captivating, and technically feasible at 60fps. Frame it as "see the science behind sound" — educational and aesthetically stunning. A breathing exercise where breath sounds create evolving cymatics patterns leverages legitimate focused-attention meditation principles. Never claim the patterns themselves have healing properties.

---

## 5. Spatial audio shows early promise through an immersion pathway

Direct evidence comparing spatial/3D audio to stereo for wellness outcomes is thin but emerging. The strongest physiological study, Yilmaz et al. (2025, *Brain and Behavior*, n = 46), found 3D sound significantly increased HRV parameters (SDNN, total power; p < 0.05) compared to control and mono conditions, suggesting **greater parasympathetic engagement without HPA axis activation.** A 2025 Korean EEG study found spatial audio (5.1 channel) produced more theta and low-alpha wave activity — states associated with meditation — while stereo produced high-beta and low-gamma indicative of external alertness.

The theoretical pathway is well-supported: spatial audio reliably increases sense of presence and immersion (strong evidence from VR research), and presence correlates with therapeutic effectiveness (Ling et al., 2014, meta-analysis). In audio-only contexts, the spatial advantage appears more pronounced; it may diminish when combined with strong visual VR stimuli due to visual dominance. Nature soundscapes in any format have robust stress-reduction evidence, and spatially accurate renderings should amplify this through ecological validity.

The commercial landscape is developing rapidly. Synctuition (MindSpa) pioneered fully spatial meditation audio with **100M+ minutes streamed**. Headspace XR launched on Meta Quest in 2024 with an RCT planned at Virginia Tech. Spatial Inc. deploys generative 3D soundscapes responsive to biosensors in Wellstar Health System clinical settings. Mindvalley launched on Apple Vision Pro in 2024.

**Evidence rating:** Spatial audio specifically enhancing relaxation vs. stereo: **Weak-to-moderate** (2–3 direct comparative studies). Immersive environments reducing stress: **Moderate**. Spatial audio increasing presence: **Strong** (well-established).

**App recommendation:** Implement spatial audio as a differentiator — the evidence trajectory is positive even if the current base is thin. Prioritize binaural 3D nature soundscapes (strongest evidence for nature sounds in stress reduction). Use object-based audio to position elements dynamically. Support Apple Spatial Audio via AirPods (largest installed base for head-tracked spatial audio). Provide Dolby Atmos output for distribution. Static binaural rendering is sufficient for meditation since users are typically still. Always offer high-quality stereo fallback.

---

## 6. Monaural beats outperform binaural beats for entrainment, but binaural beats lead in research volume

This finding may surprise many: **monaural beats produce significantly stronger cortical responses than binaural beats.** Schwarz & Taylor (2005, *Clinical Neurophysiology*) demonstrated that monaural beat auditory steady-state response (ASSR) amplitudes were significantly greater at nearly all electrode sites (p < 0.001). Becher & Chaieb et al. (2015, *European Journal of Neuroscience*) confirmed this with intracranial EEG in epilepsy patients, finding the most prominent power increases from monaural 40 Hz beats. Critically, binaural beat ASSRs become undetectable above 3 kHz carrier frequency, while monaural responses remain strong.

**Isochronic tones show the strongest EEG power modulation.** Dos Anjos et al. (2024, *Neuroscience*) directly compared isochronic tones vs. gamma binaural beats vs. white noise in 28 participants and found isochronic tones yielded **significantly greater EEG power changes (p < 0.001)**. Their modulation depth is ~50 dB versus binaural beats' ~3 dB — a 100,000:1 versus 2:1 ratio.

The key practical differences for app implementation are:

- **Binaural beats** require headphones (mandatory for dichotic presentation), work best at low frequencies (delta through alpha), and sound smooth and unobtrusive. They are sensitive to Bluetooth latency, which can desynchronize left/right channels.
- **Monaural beats** work with speakers or headphones, have no upper frequency limit, integrate naturally into music as a subtle vibrato, and have the strongest ASSR evidence.
- **Isochronic tones** work with speakers or headphones, show the strongest EEG modulation, but their pulsating character can be annoying and difficult to embed in music.

An important caveat on all three: ASSR demonstrates the brain *responds* to the stimulus, not that endogenous oscillations are entrained. This conflation is a major source of confusion in the field.

**App recommendation:** Implement all three methods weighted strategically. **Lead with monaural beats** for most use cases — best evidence-to-practicality ratio, no headphone requirement, embeds naturally into soundscapes. Use **isochronic tones** for focus/gamma applications as an optional "intense mode." Include **binaural beats** because they're commercially expected and best suited for sleep contexts. Use real-time audio synthesis (not pre-rendered files) for parametric control. Warn users about Bluetooth latency issues with binaural beats.

---

## 7. Regulatory guardrails permit wellness framing but prohibit disease claims

The FDA's updated General Wellness guidance (January 6, 2026) provides a clear two-factor test. A product qualifies for the general wellness exemption if it is (1) intended only for general wellness use and (2) presents low risk to safety. The FDA **explicitly lists sleep management, stress management, relaxation, and mindfulness** as permissible wellness categories — directly favorable for sound healing apps.

Disease-referenced claims are permitted only with specific phrasing: "may help to reduce the risk of" or "may help living well with" a chronic condition. Claims to diagnose, treat, cure, or prevent any disease push an app into regulated Software as a Medical Device (SaMD) territory.

The FTC's substantiation standard for health claims, "competent and reliable scientific evidence" (CARSE), generally requires **RCTs for health-related claims** (December 2022 guidance). Critically, the FTC warns that vague qualifiers like "may," "helps," and "preliminary" are generally inadequate as disclaimers — consumers interpret these as positive product attributes. A disclaimer cannot contradict an express claim.

**Recent enforcement provides clear precedent.** Lumosity paid $2M (2016) for claiming brain training improved cognition and delayed dementia without adequate evidence. The FTC explicitly targeted sound therapy during COVID-19, sending warning letters to Bioenergy Wellness Miami for claiming sound frequencies could "target Coronavirus." The FDA issued a warning letter to Whoop (July 2025) for blood pressure features deemed inherently diagnostic. SeniorLife Technologies was warned (August 2025) for AI health assessment without premarket authorization.

The critical language distinctions for a sound healing app:

- **Safe:** "Designed to help you relax," "supports healthy sleep habits," "tools for your stress management routine," "promotes a sense of calm"
- **Risky (requires substantiation):** "Reduces stress levels," "improves sleep quality," "science-backed frequencies"
- **Dangerous (disease claims):** "Treats anxiety/insomnia/depression," "heals trauma," "lowers blood pressure," "clinically proven to treat [condition]"

**Use "stress" rather than "anxiety" in marketing copy.** "Stress management" is explicitly listed as wellness by the FDA; "anxiety" can imply a clinical condition. Avoid the word "therapy" (use "experience" or "practice"). Never use "treatment" or "patients."

**Required disclaimers:** "This app is designed for general wellness, relaxation, and personal well-being purposes only. It is not intended to diagnose, treat, cure, or prevent any disease or medical condition. It is not a substitute for professional medical advice, diagnosis, or treatment." Place this prominently in the App Store listing, website, and in-app — not buried in fine print.

---

## 8. Five features that build genuine clinical credibility

The difference between real credibility and credibility theater is whether features generate actionable data or merely signal sophistication.

**Validated outcome measures are the single highest-impact, lowest-cost credibility feature.** The PSQI (sleep), GAD-7 (anxiety), and PSS-10 (stress) are all free, validated, available in 80+ languages, and take 2–5 minutes to complete. Embedding these in the app immediately positions it within the same research framework used by Headspace (14 published RCTs) and Calm (multiple university RCTs). Use PHQ-2 and GAD-2 (4 items total) for daily check-ins, with full scales monthly. Auto-score and visualize trends. This costs almost nothing to implement and generates the data foundation for every subsequent credibility step.

**Apple Health/Google Health Connect integration** enables passive collection of sleep stages, HRV, and heart rate data that can be correlated with sound therapy usage. Pre/post session HRV readings from an Apple Watch can demonstrate acute physiological response — this is genuine biofeedback, not theater. A 2024 Mount Sinai feasibility study confirmed that even 5-minute HRV biofeedback sessions produced measurable physiological changes in compliant participants. Camera-based PPG (smartphone camera + flashlight on fingertip) is validated for resting HRV measurement (HRV4Training showed 2.34% error versus clinical ECG) but less accurate during paced breathing — use it as a spot check, not real-time biofeedback.

**A single published pilot RCT fundamentally changes credibility positioning.** Headspace's strategy of providing free premium access to academic researchers generated 14 RCTs — most without bearing full research costs. Brain.fm secured an NSF grant and published in *Communications Biology* (Nature portfolio). A sound healing app should partner with one university for an n = 50–100, 8-week RCT, registered on ClinicalTrials.gov, using PSQI and GAD-7 as primary outcomes with waitlist control. Even one published study moves the app from "wellness product making claims" to "evidence-informed tool inviting scrutiny."

**Sound content should be designed around the strongest evidence, not the most marketable claims.** Pink noise was effective in **81.9% of studies** examining auditory sleep interventions in a *JCSM* systematic review — far outperforming white noise (33%). Delta-range binaural beats and monaural beats for sleep have moderate support. Brain.fm's amplitude modulation approach for focus has fMRI validation. Build protocols around these findings rather than Solfeggio frequencies or unsubstantiated "healing frequencies."

**A Scientific Advisory Board of 3–5 credentialed academics** in music neuroscience, sleep medicine, and digital health provides both guidance and credibility — if they have genuine advisory roles. Brain.fm employs an in-house Director of Science. Woebot was founded by a Stanford psychologist. The key is authentic involvement, not name-lending.

Consumer EEG integration (via Muse's Biofeedback+ feature) represents a **premium-tier opportunity** without hardware development costs. Users wear Muse headbands while listening to the app's content and receive post-session brain activity reports. This serves the power-user segment and generates rich data. However, consumer EEG has significant limitations — facial muscle artifacts, limited sensor count, and unclear evidence that measured signals cause target mental states — so position it as exploratory, not diagnostic.

---

## Conclusion: building on bedrock, not sand

The sound healing market is a landscape of moderate science buried under extraordinary claims. The honest picture: binaural beats produce real effects on procedural anxiety at medium effect sizes, monaural beats generate stronger cortical responses than most apps acknowledge, singing bowls reliably induce short-term relaxation through non-specific mechanisms, and spatial audio appears to enhance immersion through well-understood perceptual pathways. Solfeggio frequencies, "DNA repair" tones, and cymatic healing are marketing constructs without scientific foundations.

A credible app should do three things that most competitors do not. First, **separate evidence-based content from traditional/spiritual content** with honest framing for each. Second, **embed validated outcome measures and wearable integration** to generate real data rather than making unsupported claims. Third, **invest in at least one published clinical trial** — the single step that most dramatically distinguishes a serious product from the crowded wellness marketplace. The regulatory environment under the FDA's 2026 General Wellness guidance is favorable for apps positioned as relaxation, sleep management, and stress management tools, but the FTC's substantiation requirements mean every objective claim must be defensible with human clinical evidence.

The strongest technical differentiators grounded in real science are: monaural beats as the primary entrainment method (stronger evidence than binaural beats, no headphone requirement), spatial audio nature soundscapes (emerging evidence, high user engagement, growing platform support), pink noise for sleep (81.9% positive finding rate), HRV-responsive sound adaptation, and cymatics visualization as educational engagement. These features give users genuine value while building a defensible scientific foundation — the opposite of the industry's current approach of borrowing ancient-sounding language to dress up unsubstantiated claims.