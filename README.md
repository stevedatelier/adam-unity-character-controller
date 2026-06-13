# Adam Unity Character Controller

> An early Unity prototype exploring articulated character control through touch and language — one of the earliest foundations behind [Adam](https://adam10.com), our ongoing exploration into interactive characters and physical intelligence.

Read the full project write-up: [adam10.com/introducing-adam-10](https://adam10.com/introducing-adam-10)

---

<!-- BANNER IMAGE -->
<!-- Replace with project banner: ![Banner](docs/images/banner.png) -->

---

## Overview

Adam Unity Character Controller is a research prototype built to explore how an articulated 3D character can be controlled through direct touch and natural language input. It reflects an early stage of Adam's development and contains prototype systems, workflows, and design decisions that helped shape later versions.

This release is shared for **educational purposes**, experimentation, and community exploration.

---

<!-- DEMO GIF OR VIDEO PREVIEW -->
<!-- Replace with demo: ![Demo](docs/images/demo.gif) -->

---

## This GitHub Release

This repository contains the **touch-only prototype** — the foundation of Adam's control system as it existed in early development.

**What is not included:**
- AI reasoning or LLM-based command routing
- Voice input or speech-to-text
- Text / chat command interface

**Only touch interaction is supported in this release.** The full Adam 1.0 system — including voice commands, AI-powered intent routing, and the complete language interface described in the [case study](https://adam10.com/introducing-adam-10) — is not part of this open release.

> **Want to build with us?** Reach out at [steve@adam10.com](mailto:steve@adam10.com)
>
> **See the final product:** [adam10.com](https://adam10.com)

---

## What's Included

| Component | Description |
|---|---|
| Unity Project Files | Complete project, ready to open |
| Character Controller | Touch-driven articulated character system |
| Touch Interaction Prototype | Early finger-tracking and body-contact input |
| Animation & Articulation Setup | Rigged character with layered animation |
| Sample Scene | Fully lit and rendered reference scene |
| Project Structure & Reference Files | Documented layout for exploration |
| Video Tutorials | Coming soon |

---

## Getting Started

### Requirements

- Unity **2021.3 LTS** or later (URP)
- The **Assets** folder (distributed separately — see below)

### Installation

**1. Clone this repository**

```bash
git clone https://github.com/stevedatelier/adam-unity-character-controller.git
```

**2. Download the Assets folder**

The character models, textures, and scene assets are distributed separately due to file size. Download and place the `Assets/` folder in the root of the cloned repo:

> **[Download Assets from Google Drive](https://drive.google.com/drive/folders/1tja8iXRrmTxw8pPwiovO4kLRCUTuHR6M?usp=sharing)**

Your folder structure should look like this:

```
adam-unity-character-controller/
├── Assets/          ← place downloaded folder here
├── Packages/
├── ProjectSettings/
└── README.md
```

**3. Open in Unity**

Open the project folder in Unity Hub. Allow Unity to import and compile on first launch.

---

<!-- SCENE SCREENSHOT -->
<!-- Replace with Unity scene screenshot: ![Scene](docs/images/scene.png) -->

---

<!-- INSPECTOR / CONTROLLER SCREENSHOT -->
<!-- Replace with inspector screenshot: ![Inspector](docs/images/inspector.png) -->

---

## Project Structure

```
Assets/
├── Characters/          # Character mesh, FBX, and animations
├── ExampleAssets/       # Materials, props, and environment pieces
├── Editor/              # Custom editor tooling
└── ...

ProjectSettings/         # Unity project configuration (URP, physics, input)
Packages/                # Package manifest and lock file
```

---

## Credits

- Character model by **3DZipGuy**
- Additional assets belong to their respective owners and are used solely for demonstration purposes

---

## About Adam

The current release, **Adam 1.0**, is an AI-powered interactive character that can be controlled through touch, voice commands, and chat in real time.

To experience the latest version: [adam10.com](https://adam10.com)

---

## Adam 1.0: A Case Study

*Mar 30 '26 — Adam 1.0, Physical Intelligence Architecture*

<img src="docs/images/hero-architecture.svg" alt="ADAM Architecture Overview" width="100%">

ADAM's architecture is organized around a single division of responsibility. The system's motion vocabulary is authored, finite, and calibrated by the engine. The language interface is fully open-ended, accepting any phrasing a speaker might use to describe movement — including figurative, qualified, and contextual language.

How ADAM routes arbitrary natural language through a tiered decision pipeline to produce authored, calibrated motion, and the engineering mechanisms behind each stage.

---

### Response Time

Every prompt entering the routing pipeline has a measurable cost. Stage 1 heuristic intercepts carry zero inference overhead — the engine returns a fully authored beat sequence with no language model call. Stage 2 language model gate calls add latency proportional to the model's time-to-first-token. The post-processing pipeline is deterministic and adds negligible time.

When a voice command triggers the routing pipeline, the end-to-end path from speech capture to engine-executable output is broken into overlapping stages. Speech-to-text conversion, heuristic evaluation, and — when required — language model inference run with the following measured contributions:

| Stage | Latency |
|---|---|
| STT | ~80ms |
| LLM Gate | ~180ms (when fired) |
| Post-processing | <5ms |
| **Total** | **<270ms w/ LLM** |

<img src="docs/images/chart-latency-breakdown.svg" alt="Latency Breakdown" width="560">

Stage 1 heuristic: zero inference cost.

**Routing path comparison:**

| Path | Latency |
|---|---|
| Full LLM path | ~270ms |
| Generic operator | ~85ms |
| Heuristic (Stage 1) | <5ms |

<img src="docs/images/chart-vs-baseline.svg" alt="vs. Baseline" width="560">

<img src="docs/images/chart-streaming-mode.svg" alt="Streaming Mode" width="560">

Stage 1 resolves most prompts with zero inference cost.

---

### Governing Principle

ADAM's architecture is organized around a single division of responsibility. The system's motion vocabulary is authored, finite, and calibrated by the engine. The language interface is fully open-ended, accepting any phrasing a speaker might use to describe movement, including figurative, qualified, and contextual language.

These two properties are made compatible by keeping the domains structurally separate. The language model reasons about what the speaker intends. The engine determines what that intention physically means, how it is executed, and at what calibration. The language model is a semantic selector. The engine is a physical authority. Neither domain bleeds into the other's.

The practical consequence is that the system handles "walk like you're carrying something heavy" or "move like you're trying not to wake anyone up" without a new motion primitive for either case. The language model identifies the closest existing motion in the authored vocabulary, and the engine executes it exactly as designed. This is a different architecture from systems that ask a language model to generate motion geometry directly, which couples language understanding and physical design into a single inference step.

---

### Articulation Model

The authored vocabulary is constrained by the physical structure of the rig. Each joint has a defined axis and a calibrated range. The engine resolves every motion command within those constraints — there is no freeform deformation, only authored part-and-joint relationships.

Motion is resolved through discrete articulated segments. Every motion in the system — authored or LLM-selected — reduces to one of two primitives:

- **Hold** — reaches a target position and remains there
- **Oscillate** — moves out and returns on every cycle

The language model's most consequential decision in Format B output reduces to a single binary semantic question: is this prompt describing a state or an action? The semantic answer maps directly to a mechanical choice, and the mechanical choice produces the correct visual communication to a viewer.

<!-- FIGURE 5: Articulation — Assembled & Exploded Arm -->
<!-- Replace with: ![Figure 5 — Assembled Arm](docs/images/figure-5-assembled-arm.png) and ![Figure 5 — Exploded Arm](docs/images/figure-5-exploded-arm.png) -->
<!-- Two side-by-side video stills (3:4 ratio): left = assembled arm looping animation, right = exploded arm looping animation -->
<!-- Caption: Figure 5. Discrete articulated segments. Motion is resolved through authored parts and joints, not freeform deformation. -->

<!-- FIGURE 6: Full Body Assembled -->
<!-- Replace with: ![Figure 6 — Full Body Assembled](docs/images/figure-6-full-body.png) -->
<!-- Single video still (3:4 ratio): full body assembled looping animation -->
<!-- Caption: Figure 6. The same articulation logic extends across the full figure, allowing motion to be routed through a constrained physical vocabulary. -->

---

### Routing Pipeline

Every prompt passes through a four-stage deterministic cascade. Each stage has defined authority and defined exit conditions. A decision made at an earlier stage cannot be overridden by a later one.

| Stage | Name | Description |
|---|---|---|
| Input | Natural language | Any spoken, typed, or text command |
| Stage 1 | Heuristic match | Pattern-matches against the authored motion vocabulary. A match exits immediately with zero inference cost. Outputs: `walk`, `run`, `brace`, etc. |
| Stage 2 | Language model | Fires only when Stage 1 returns null. Returns Format A or Format B JSON. Outputs: `Format B — direct` |
| Stage 3 | Synthesis | Merges overlays onto locomotion bases and applies fallback logic |
| Stage 4 | Calibration | Deterministic calibration: scope filtering, alternation sequencing, duration expansion, and normalization |
| Output | Action plan | Engine-executable plan |

<img src="docs/images/figure-1-routing-pipeline.svg" alt="Figure 1 — Routing Pipeline" width="480">

*Figure 1. Four-stage routing cascade. Stage 1 pattern-matches against the authored motion vocabulary — a match exits immediately with zero inference cost. Stage 2 fires only when Stage 1 returns null. Stage 3 merges overlays onto locomotion bases and applies fallback logic. Stage 4 applies deterministic calibration.*

---

### Motion Operator Primitives

All motion output reduces to two mechanical operators. Research on action perception identifies this division directly: the brain categorizes physical motion as either a state (a configuration held) or an event (a transition in progress). The two operators instantiate this perceptual distinction mechanically in the engine.

| Operator | Behavior |
|---|---|
| **Hold** | Reaches target. Stays there. |
| **Oscillate** | Moves out. Returns. Repeats. |

Every motion in the system — authored or LLM-selected — reduces to one of these two primitives.

<img src="docs/images/figure-2-motion-primitives.svg" alt="Figure 2 — Motion Operator Primitives" width="560">

*Figure 2. Every motion in the system — authored or LLM-selected — reduces to one of two primitives. Hold commits to a position and remains. Oscillate moves out and returns, structurally, on every cycle.*

---

### Authored Vocabulary

The motion vocabulary is organized into four tiers. The tier structure encodes composition rules that the synthesis layer enforces at the beat level. A Tier 1.5 character overlay carries suppression and injection rules that activate specifically when merging onto a locomotion base — rules the language model never needs to reason about. The model selects from the vocabulary freely. The engine applies physical grammar downstream.

| Tier | Name | Type | Engine Behavior | Examples |
|---|---|---|---|---|
| T1 | Locomotion | Full gait — cyclic | Authored beat sequences, bilateral coupling, bypasses normalizer | `walk` `jog` `run` `sprint` `dance` `play` `fight` |
| T1.5 | Character Overlays | On T1 or standalone | Suppresses base arms, injects pose at beat 0, holds across cycle | `zombie` `injured` `proud` `scared` `drunk` |
| T2 | Postural-Reactive | One-shot or short-cycle | Cross-body state changes, authored timing preserved, normalizer bypassed | `alert` `cautious` `aim` `triumphant` `collapse` `recoil` `sneak` `brace` `search` `hesitate` `approach` `retreat` |
| T3 | Single-Region | Body-part targets | Multi-pass rig resolution, synonym index, side awareness enforced | `move/rotate arms` `move/rotate legs` `move/rotate forearms` `rotate head` `move body` `left/right variants` |

*T1.5 overlays branch from T1 — they layer onto locomotion or stand alone as stationary poses. T2 preserves authored timing and bypasses normalization. No combination of tier selections produces physically incoherent output.*

<img src="docs/images/figure-3-vocabulary-tiers.svg" alt="Figure 3 — Authored Vocabulary Tiers" width="100%">

*Figure 3. The four-tier vocabulary. T1.5 overlays branch from T1 — they layer onto locomotion or stand alone as stationary poses. T2 preserves authored timing and bypasses normalization. No combination of tier selections produces physically incoherent output.*

---

### LLM Output Schema

The language model outputs one of two JSON structures. Format A handles expressive, figurative, and qualified language. Format B handles prompts that name a single universally recognizable mechanical pattern — swim, march, fly, wave one arm — where the action structure is fully determined by the verb and a body group.

Format B routes through the engine's native operator paths directly, bypassing the shaper, so the output carries calibrated operator values without modification. Format A feeds the full overlay and locomotion synthesis paths.

**Format A** — Expressive, figurative, qualified, overlay prompts

| Field | Required | Values |
|---|---|---|
| `phrases` | yes | 2–6 strings from authored vocabulary. At most one T1.5 overlay entry. |
| `mode` | | `"steps"` / `"sequential"` / `"parallel"` |
| `energy` | | `"low"` / `"med"` / `"high"` — scales amplitude |
| `tempo` | | `"fast"` / `"med"` / `"slow"` — scales timing |
| `pose` | | boolean, suppresses duration expansion |

```json
{"phrases":["walk","zombie"],"energy":"low","tempo":"slow","pose":false}
```

**Format B** — Single operator, single group, simple bilateral

| Field | Required | Values |
|---|---|---|
| `selection` | yes | `"hold/pose"` or `"oscillate"` — the primary semantic decision |
| `group` | yes | `"arms"` / `"forearms"` / `"legs"` / `"knees"` / `"head"` / `"hips"` |
| `laterality` | yes | `"together"` / `"alternate"` / `"left only"` / `"right only"` |
| `lead_side` | | Initiating side when laterality is `"alternate"` |
| `cycles` | | Integer 1–6, complete pattern repetitions |

```json
{"selection":"oscillate","group":"arms","laterality":"alternate","lead_side":"left","cycles":3}
```

*Format B routes through the engine's native operator paths with no shaper inflation. Format A is the correct default when the model is uncertain.*

<img src="docs/images/figure-4-llm-schema.svg" alt="Figure 4 — LLM Output Schema" width="100%">

*Figure 4. The dual-format LLM output schema. Format B routes through the engine's native operator paths with no shaper inflation. Format A is the correct default when the model is uncertain.*

---

### Routing Guards

English surface structure produces specific ambiguities at routing boundaries. Four guards address the consequential ones, each introduced in response to an observable routing failure, each independently localized.

| Guard | Trigger | Effect |
|---|---|---|
| `Body-Noun Guard` | Operator verb, no body noun | Fails selectively — no body noun means no operator dispatch. Prompt reaches LLM and is routed as locomotion with qualifier. |
| `Walk-Overlay Guard` | Locomotion verb + overlay keyword | Bypasses plain-gait heuristic. LLM returns locomotion + overlay composition. Enables "walk like a zombie." |
| `Stationary-Simile Rule` | "Stand like" / "stand as if" | Verb determines route, not overlay term. "Stand like you're proud" results in a held pose. "Walk like you're proud" results in gait + overlay. |
| `Zombie Arm Suppression` | Zombie overlay on any locomotion base | Strips arm joints from the locomotion base before merge. Overlay arm pose injected at beat 0 only — holds across full cycle. |

*The body-noun guard's non-greedy design is the core mechanism for contextual locomotion prompts.*

---

### Synthesis Paths

Synthesis translates validated LLM intent into an engine-executable action plan by routing through existing authored paths. It generates no new motion.

**Locomotion + Overlay**

The locomotion plan is assembled from the Tier 1 authored family first. Zombie arm suppression strips arm joint entries if applicable. The Tier 1.5 overlay's body and head directives are then merged into the base beat structure. The overlay arm pose is injected at beat 0 only, which maintains continuous arm posture across all subsequent cycles without per-cycle re-injection logic.

**Stationary Overlay Fallback**

When the phrase list contains only a Tier 1.5 overlay with no locomotion base, the primary composition pass produces an empty action plan. Synthesis detects the empty plan and applies the fallback: the overlay becomes a standalone postural expression, `isPose=true` is set, and duration expansion is bypassed. The empty plan is the trigger — no special pre-classification needed.

**Targeted Operator Routing**

Format B group and laterality fields are translated into a target phrase string and routed through the existing engine phrase handler. Alternate laterality places lead-side joint actions in even steps and follow-side actions in odd steps, repeating across the declared cycle count. This path bypasses the shaper entirely, carrying native calibrated operator values from the outset.

---

### Post-processing Pipeline

| Stage | Function |
|---|---|
| `Scope Filter` | Builds allow/deny sets from body-region language. Runs before duration expansion — cycle count computed on retained actions only. |
| `Alternation Processor` | Splits into left, right, and neutral groups. Mirrors absent side by joint-ID substitution. Re-sequences into interleaved step indices. |
| `Duration Expander` | Computes cycle count as `ceil(targetSeconds / stepDuration)`. Mode-aware: parallel uses max duration; steps sums per-step maxima; sequential uses flat sum. |
| `Runtime Floor` | Floors each action timing to 0.05s on sequential non-pose plans. Aligns cycle-count computation with actual playback speed. |
| `Axis Normalization` | Validates axis values, fills missing degree and timing defaults. Every action fully specified before dispatch. |
| `Timing Normalizer` | Smooths variance across the action list. Applies 9-second global cap. Authored families bypass — their timing is preserved. |

*Duration expander is mode-aware: parallel, steps, and sequential plans require different cycle-count formulas. Runtime floor resolves the interaction between dense authored timing and duration expansion.*

---

### Regression Matrix

The routing pipeline's correctness is defined by a 22-case regression matrix. Every routing change is verified against it before deployment. The matrix targets the boundaries where incorrect routing produces the most visible communicative failure.

| Prompt | Route | Loco | Boundary |
|---|---|---|---|
| walk | `authored-family` | yes | Heuristic intercept, no LLM |
| jog | `authored-family` | yes | Sprint / jog / run family |
| move cautiously | `llm-format-a` | yes | Body-noun guard fails — LLM routes as loco + qualifier |
| move like you're trying not to be seen | `llm-format-a` | yes | Simile + travel — walk + sneak |
| walk like a zombie | `loco + overlay` | yes | Walk-overlay guard, arm suppression, beat-0 pose |
| run scared | `loco + overlay` | yes | Walk-overlay guard fires on run |
| stand proud | `stationary-overlay` | no | Overlay alone — stationary fallback, isPose=true |
| stand like you're scared | `stationary-overlay` | no | Stationary-simile rule fires, not loco+overlay |
| brace yourself | `authored-family` | no | Direct heuristic |
| move right arm | `generic-operator` | no | Body noun — operator dispatch, no LLM |
| rotate head | `generic-operator` | no | Rotate + body noun — 180° one-shot |
| move like something is wrong | `llm-format-a` | yes | No body noun — walk + cautious |
| stand like you're proud | `stationary-overlay` | no | Stationary-simile: proud alone, not walk + proud |
| move like a zombie | `loco + overlay` | yes | "move like" — guard fails — walk + zombie |

*The tricky-phrasing cases are the most diagnostic — they sit at the exact points where two routing paths appear equally valid.*

---

### System Invariants

1. **The engine owns all mechanical semantics.** Degree values, axis selection, timing, and joint resolution are determined exclusively by the engine. The language model selects from the authored vocabulary; the engine defines what every selection physically means.

2. **Authored families are fixed.** The heuristic layer, multi-body beat structures, and authored phrase definitions cannot be modified by the LLM path, synthesis layer, or post-processing pipeline.

3. **The shaper does not fire on authored plans.** Amplitude and timing inflation is bypassed for engine-authored families and targeted-operator plans. Authored motion executes with its designed calibration.

4. **At most one character overlay per composition.** Enforced by schema and synthesis logic. Two overlays in the same phrase list is undefined in the motion design space, excluded structurally.

5. **The post-processing pipeline is deterministic.** Identical inputs produce identical outputs. No stochastic element exists below the language model gate.

---

### Simplifying Multi-Touch Gestures with Unity's Input System

This section highlights the streamlined implementation of multi-touch gestures for rotating a figure in Unity, utilizing the Input System. Within the `Update()` method, the script monitors the boolean variable `isActive` to determine readiness for interaction. Upon activation, the figure changes color to signal readiness. It detects a single touch on the screen for rotation, controlled by `rotateSpeedModifier`. When the touch ends, the figure reverts to its default state, indicating inactivity. This approach efficiently enhances user interaction in 3D environments.

---

### Implications for Language-Controlled Physical Agents

The dominant paradigm in AI-controlled robotics frames the control problem as one of imitation. A human body is motion-captured at scale, a model learns to reproduce it, and when new behavior is needed, a human operator demonstrates or commands it directly via teleoperation. The robot's motion is, in this framing, always a derivative of human motion. The operator's hand is always, at some level, on the controls.

This framing transfers poorly to articulated systems that are designed objects rather than biomechanical replicas. A rig is not a human body. It has authored axes, calibrated ranges, and deliberate constraints. Those constraints are not deficiencies to engineer around. They are what gives the motion meaning. A 3D animator or technical director understands this from practice: a character moves within its vocabulary, and the vocabulary is what makes the movement readable. Remove the vocabulary and you remove the grammar. The output becomes physically possible but communicatively empty.

The engineering community building language-controlled physical agents has largely not absorbed this distinction. The result is systems that treat articulated motion as an underconstrained space to be filled by model output or direct operator input. Motion that is generated or commanded frame by frame does not read as autonomous. It reads as controlled, because it is. The operator's intention is legible in every frame.

ADAM proposes an inversion of this relationship. The physical vocabulary is authored, finite, and the system's own. The language model navigates it. The human describes an intention once, in natural language, and steps back. What the system does next comes from its own expressive library, not from an operator's hand or a model's approximation of captured human movement. The autonomy is architectural: when ADAM walks like a zombie or braces for impact, it is selecting from its own vocabulary. The motion belongs to the system. That distinction — between a system that is directed and a system that expresses — is the gap this architecture is designed to close.

---

### References

1. Jiang, B., et al. (2023). *MotionGPT: Human Motion as a Foreign Language.* NeurIPS 2023.
2. Tevet, G., et al. (2022). *Human Motion Diffusion Model.* arXiv:2212.04048.
3. Zhang, M., et al. (2023). *MotionDiffuse: Text-Driven Human Motion Generation with Diffusion Models.* IEEE TPAMI.
4. Ahn, M., et al. (2022). *Do As I Can, Not As I Say: Grounding Language in Robotic Affordances.* arXiv:2204.01691.
5. Zacks, J.M. and Tversky, B. (2001). *Event structure in perception and conception.* Psychological Bulletin, 127(1).
6. Lasseter, J. (1987). *Principles of traditional animation applied to 3D computer animation.* ACM SIGGRAPH, 21(4).
7. Schick, T., et al. (2023). *Toolformer: Language Models Can Teach Themselves to Use Tools.* NeurIPS 2023.
8. Beck, K. (2002). *Test-Driven Development: By Example.* Addison-Wesley.

---

## License

See [LICENSE](LICENSE) for details. Character and third-party assets remain the property of their respective owners.
