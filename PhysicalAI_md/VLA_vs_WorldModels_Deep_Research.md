# VLA vs World Models in Physical AI/Robotics: Deep Research Report

**Research Date:** January 2026
**Coverage Period:** 2024-2025 Research and Implementations

---

## Executive Summary

This report provides a comprehensive analysis of the relationship between Vision-Language-Action (VLA) models and World Models in Physical AI/Robotics. Key findings include:

- **VLA models** provide end-to-end perception-to-action pipelines with strong semantic understanding but limited planning capabilities
- **World Models** excel at prediction and planning but traditionally lack semantic grounding
- **Convergence is underway**: Major players (NVIDIA, Figure AI, Google, Physical Intelligence) are adopting hybrid dual-system architectures
- **The debate continues**: Yann LeCun champions pure world models, while most production systems use VLA-centric approaches
- **Timeline**: Commercial deployment 2025-2026, mass market adoption 2027-2030, potential AGI integration early 2030s

---

## 1. Architectural Comparison

### 1.1 How VLA Models Work (End-to-End Architecture)

Vision-Language-Action (VLA) models integrate computer vision, natural language understanding, and robotic control within unified computational frameworks. Unlike traditional robots programmed for specific tasks, VLAs can see environments, understand natural language instructions, and generate appropriate physical actions.

#### Core Architecture Components

```
┌─────────────────────────────────────────────────────────────────┐
│                    VLA MODEL ARCHITECTURE                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────┐   ┌──────────┐   ┌──────────────┐                │
│  │  Visual   │   │ Language │   │Proprioceptive│                │
│  │ Encoder   │   │ Encoder  │   │   Encoder    │                │
│  │(ViT/DINO) │   │(LLaMA)   │   │  (MLP/Trans) │                │
│  └─────┬─────┘   └────┬─────┘   └──────┬───────┘                │
│        │              │                │                         │
│        └──────────────┼────────────────┘                         │
│                       │                                          │
│                       ▼                                          │
│              ┌────────────────┐                                  │
│              │ Fusion Backbone│                                  │
│              │  (Transformer) │                                  │
│              └───────┬────────┘                                  │
│                      │                                           │
│                      ▼                                           │
│              ┌────────────────┐                                  │
│              │ Action Decoder │                                  │
│              │(Diffusion/AR)  │                                  │
│              └───────┬────────┘                                  │
│                      │                                           │
│                      ▼                                           │
│              ┌────────────────┐                                  │
│              │  Robot Actions │                                  │
│              │  (50-200 Hz)   │                                  │
│              └────────────────┘                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Key Characteristics:**
- **Single forward pass**: Scene understanding + language instruction → robot actions
- **End-to-end learning**: No explicit intermediate representations
- **Pre-trained backbones**: Leverage internet-scale VLM knowledge (PaLI, Gemma, LLaMA)
- **Action output**: Discrete tokens (OpenVLA) or continuous flow matching (π0)

#### Action Decoding Approaches

| Approach | Examples | Pros | Cons |
|----------|----------|------|------|
| **Autoregressive Discrete** | OpenVLA, RT-2 | Simple, leverages LLM training | Quantization errors, slow inference |
| **Diffusion/Flow Matching** | π0, GR00T N1 | Smooth trajectories, precise control | Higher compute, training complexity |
| **Hybrid** | Helix, OFT | Best of both worlds | Architecture complexity |

### 1.2 How World Models Work (Prediction + Planning)

World models infer and predict real-world dynamics by modeling the external environment. They maintain an internal representation of the world and predict the consequences of actions so agents can "imagine, plan, and decide before they act."

#### Core Architecture Components

```
┌─────────────────────────────────────────────────────────────────┐
│                   WORLD MODEL ARCHITECTURE                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌────────────────┐                                             │
│  │ Current State  │ (Observation at time t)                     │
│  │    s_t         │                                             │
│  └───────┬────────┘                                             │
│          │                                                       │
│          ▼                                                       │
│  ┌────────────────────────────────────────┐                     │
│  │         WORLD MODEL CORE               │                     │
│  │  ┌─────────────────────────────────┐   │                     │
│  │  │     Dynamics Predictor          │   │                     │
│  │  │  s_{t+1} = f(s_t, a_t)         │   │                     │
│  │  └─────────────────────────────────┘   │                     │
│  │  ┌─────────────────────────────────┐   │                     │
│  │  │     Reward/Value Predictor      │   │                     │
│  │  │  r_t = g(s_t, a_t)             │   │                     │
│  │  └─────────────────────────────────┘   │                     │
│  └────────────────────────────────────────┘                     │
│          │                                                       │
│          ▼                                                       │
│  ┌────────────────────────────────────────┐                     │
│  │       IMAGINATION / PLANNING           │                     │
│  │  - Rollout multiple action sequences   │                     │
│  │  - Evaluate predicted outcomes         │                     │
│  │  - Select optimal action sequence      │                     │
│  └───────────────┬────────────────────────┘                     │
│                  │                                               │
│                  ▼                                               │
│          ┌────────────────┐                                     │
│          │ Optimal Action │                                     │
│          │      a*        │                                     │
│          └────────────────┘                                     │
└─────────────────────────────────────────────────────────────────┘
```

**Key Approaches:**

| Architecture Type | Examples | Mechanism |
|-------------------|----------|-----------|
| **RSSM-based** | Dreamer, DreamerV3 | Recurrent state space models for latent dynamics |
| **JEPA-based** | V-JEPA 2 (Meta) | Joint embedding prediction in abstract space |
| **Transformer-based** | Genie, Cosmos | Attention-based sequence modeling |
| **Video Generation** | 1X World Model, GR-2 | Generate future video frames, then decode actions |

### 1.3 Detailed Comparison: Data Flow and Decision Making

| Aspect | VLA Models | World Models |
|--------|------------|--------------|
| **Input** | Image + Language + Proprioception | State observation |
| **Output** | Direct motor actions | Predicted future states |
| **Planning** | Implicit in learned policy | Explicit through imagination |
| **Semantic Understanding** | Strong (pre-trained VLM) | Weak (physics-focused) |
| **Physical Reasoning** | Weak (action-focused) | Strong (dynamics modeling) |
| **Latency** | Low (single forward pass) | Higher (rollout required) |
| **Data Efficiency** | Lower (needs robot data) | Higher (can use video) |
| **Generalization** | To new instructions | To new dynamics |

**Critical Insight from Research:**
> "VLA models take a textual instruction and current observation as input, directly producing a predicted action for the robot to execute. In contrast, world models output a sequence of future observations, which can be either explicit visual frames or latent representations, requiring a subsequent step such as an action decoder or rejection sampling for optimal action planning." - [Large VLM-based VLA Survey](https://arxiv.org/html/2508.13073v1)

---

## 2. Integration Approaches (2024-2025)

### 2.1 NVIDIA GR00T N1: Dual-System Architecture

**Release:** March 2025
**Architecture:** System 1 + System 2 (inspired by Kahneman's "Thinking, Fast and Slow")

```
┌─────────────────────────────────────────────────────────────────┐
│                    NVIDIA GR00T N1                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  SYSTEM 2 (Slow Thinking) - Eagle-2 VLM                 │    │
│  │  - Scene understanding                                   │    │
│  │  - Language comprehension                                │    │
│  │  - High-level planning                                   │    │
│  │  - Operates at 10 Hz on L40 GPU                         │    │
│  │  - 1.34B parameters (VLM component)                     │    │
│  └──────────────────────┬──────────────────────────────────┘    │
│                         │ Latent representations                 │
│                         ▼                                        │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  SYSTEM 1 (Fast Thinking) - Diffusion Transformer       │    │
│  │  - Cross-attends to S2 output                           │    │
│  │  - Generates precise motor commands                     │    │
│  │  - Operates at 120 Hz                                   │    │
│  │  - Flow-matching action generation                      │    │
│  │  - 63.9ms inference for 16-action chunk                 │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Total Parameters: 2.2B                                          │
│  Training Data: Robot trajectories + Human videos + Synthetic    │
└─────────────────────────────────────────────────────────────────┘
```

**Key Innovation:** The two systems are "tightly coupled" - the Diffusion Transformer constantly cross-attends to the VLM's output, ensuring physical actions are aligned with task understanding.

**Quote from NVIDIA:**
> "System 1 is trained on human demonstration data and a massive amount of synthetic data generated by the NVIDIA Omniverse platform."

**Source:** [NVIDIA GR00T N1 Announcement](https://nvidianews.nvidia.com/news/nvidia-isaac-gr00t-n1-open-humanoid-robot-foundation-model-simulation-frameworks), [arXiv Paper](https://arxiv.org/abs/2503.14734)

---

### 2.2 Figure AI Helix: VLM + Action Architecture

**Release:** February 2025
**Distinction:** First VLA to control entire humanoid upper body at high frequency

```
┌─────────────────────────────────────────────────────────────────┐
│                       FIGURE AI HELIX                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  SYSTEM 2 - 7B Parameter VLM                            │    │
│  │  - Pretrained on internet-scale data                    │    │
│  │  - Processes monocular RGB + proprioception             │    │
│  │  - Operates at 7-9 Hz                                   │    │
│  │  - Outputs latent semantic vector                       │    │
│  └──────────────────────┬──────────────────────────────────┘    │
│                         │                                        │
│                         ▼                                        │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  SYSTEM 1 - 80M Parameter Transformer                   │    │
│  │  - Cross-attention encoder-decoder                      │    │
│  │  - Reactive control at 200 Hz                           │    │
│  │  - Multi-scale convolutional vision backbone            │    │
│  │  - Stereo vision (upgraded from monocular)              │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Training: ~500 hours teleoperation + auto-generated labels     │
│  Deployment: Runs entirely on embedded GPUs in Figure 02        │
└─────────────────────────────────────────────────────────────────┘
```

**Key Design Philosophy:**
> "Prior approaches faced a fundamental tradeoff: VLM backbones are general but not fast, and robot visuomotor policies are fast but not general. Helix resolves this tradeoff through two complementary systems trained end-to-end to communicate."

**Recent Upgrade (2025):** Stereo vision backbone with multiscale feature extraction network, merging features from both cameras before tokenization.

**Source:** [Figure AI Helix Announcement](https://www.figure.ai/news/helix)

---

### 2.3 1X World Model: "Imagine-then-Act" Approach

**Approach:** Fundamentally different from traditional VLA - generates video first, then extracts actions

```
┌─────────────────────────────────────────────────────────────────┐
│                    1X WORLD MODEL (1XWM)                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Step 1: VIDEO IMAGINATION                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  14B Generative Video Model                             │    │
│  │  - Input: Starting frame + text prompt                  │    │
│  │  - Output: Future video frames                          │    │
│  │  - Pre-training: 900 hours egocentric human video       │    │
│  │  - Fine-tuning: 70 hours robot data                     │    │
│  └──────────────────────┬──────────────────────────────────┘    │
│                         │ Generated video                        │
│                         ▼                                        │
│  Step 2: ACTION EXTRACTION                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Inverse Dynamics Model (IDM)                           │    │
│  │  - Translates imagined video frame-by-frame             │    │
│  │  - Outputs exact motor commands                         │    │
│  │  - Executes on NEO humanoid                             │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Key Capability: Zero-shot task execution                        │
│  Example: Opening air-fryer, brushing hair - never trained       │
└─────────────────────────────────────────────────────────────────┘
```

**Paradigm Difference:**
| Traditional VLA | 1X World Model |
|-----------------|----------------|
| Observation → Action | Observation → Video → Action |
| Reactive | Predictive |
| Single possible future | Multiple imagined futures |
| Limited planning | Rich planning through simulation |

**Quote from 1X:**
> "When given a prompt like 'pack the lunchbox,' NEO's AI doesn't just guess the next arm movement. Instead, it first imagines a short video of the future, visualizing itself completing the task."

**Unique Value:** World models enable evaluation at scale - if you have 1000 tasks, you can verify whether a new model improves all tasks by comparing imagined outcomes.

**Source:** [1X World Model](https://www.1x.tech/discover/1x-world-model)

---

### 2.4 Google Gemini Robotics: World Understanding Integration

**Release:** March 2025 (Gemini Robotics), September 2025 (Gemini Robotics 1.5)

```
┌─────────────────────────────────────────────────────────────────┐
│                   GOOGLE GEMINI ROBOTICS                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  GEMINI 2.0 FOUNDATION                                  │    │
│  │  - Multimodal: text, images, video, audio               │    │
│  │  - Native world understanding                           │    │
│  │  - Spatial reasoning capabilities                       │    │
│  └──────────────────────┬──────────────────────────────────┘    │
│                         │                                        │
│          ┌──────────────┴──────────────┐                        │
│          ▼                             ▼                        │
│  ┌────────────────────┐    ┌─────────────────────────┐          │
│  │ Gemini Robotics    │    │ Gemini Robotics-ER      │          │
│  │ (VLA Model)        │    │ (Embodied Reasoning)    │          │
│  │ - Direct action    │    │ - High-level planning   │          │
│  │ - Visual→Motor     │    │ - Tool calling          │          │
│  │ - Dexterous tasks  │    │ - Progress estimation   │          │
│  │   (origami, cards) │    │ - Google Search access  │          │
│  └────────────────────┘    └─────────────────────────┘          │
│                                                                  │
│  Gemini Robotics 1.5 (Sept 2025):                               │
│  - "Thinks before acting" - shows reasoning process             │
│  - Cross-embodiment learning                                    │
│  - State-of-the-art spatial understanding benchmarks            │
│                                                                  │
│  Partnership: Boston Dynamics (CES 2026) for humanoids          │
└─────────────────────────────────────────────────────────────────┘
```

**Key Differentiation:**
> "Unlike Pi0 and GR00T N1 which integrate reasoning and action within unified architectures, Gemini Robotics 1.5 takes a fundamentally different approach: separating high-level reasoning from low-level control through a dual-model agentic system."

**Source:** [Google DeepMind Gemini Robotics](https://deepmind.google/blog/gemini-robotics-brings-ai-into-the-physical-world/)

---

### 2.5 Physical Intelligence π0: Flow Matching Approach

**Release:** October 2024 (paper), 2025 (open-source)

```
┌─────────────────────────────────────────────────────────────────┐
│                    PHYSICAL INTELLIGENCE π0                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  PaliGemma VLM Backbone (3B parameters)                 │    │
│  │  - SigLIP visual encoder                                │    │
│  │  - Gemma language encoder                               │    │
│  │  - Internet-scale semantic knowledge                    │    │
│  └──────────────────────┬──────────────────────────────────┘    │
│                         │                                        │
│                         ▼                                        │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  FLOW MATCHING ACTION GENERATION                        │    │
│  │  - Novel architecture (not autoregressive)              │    │
│  │  - Progressive denoising: noise → motor actions         │    │
│  │  - Smooth, real-time trajectories at 50 Hz              │    │
│  │  - Action chunks of size 50 (1 second lookahead)        │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Training Data:                                                  │
│  - 7 robotic platforms                                          │
│  - 68 unique tasks                                              │
│  - 10k+ hours of robot data                                     │
│                                                                  │
│  Tasks Demonstrated:                                             │
│  - Laundry folding, table bussing, grocery bagging              │
│  - Box assembly, object retrieval                               │
└─────────────────────────────────────────────────────────────────┘
```

**Why Flow Matching?**
> "Unlike standard robotic policies, π0 employs flow matching to produce smooth, real-time action trajectories at 50Hz, making it highly efficient, precise, and adaptable for real-world deployment."

**Open Source:** Available via [openpi repository](https://github.com/Physical-Intelligence/openpi) with checkpoints for ALOHA and DROID platforms.

**Source:** [Physical Intelligence Blog](https://www.physicalintelligence.company/blog/pi0), [arXiv Paper](https://arxiv.org/abs/2410.24164)

---

### 2.6 OpenVLA vs World Model Approaches

**OpenVLA (Stanford, June 2024):** 7B parameter open-source VLA baseline

| Aspect | OpenVLA | World Model Approaches |
|--------|---------|------------------------|
| **Architecture** | Discrete token autoregressive | Video generation + IDM |
| **Action Space** | 256-bin discretization | Continuous |
| **Inference Speed** | Slow (sequential decoding) | Variable |
| **Precision** | Limited by quantization | Higher fidelity |
| **Planning** | Implicit | Explicit imagination |
| **Data Efficiency** | Lower | Higher (video pretraining) |

**OpenVLA Limitations:**
1. **Quantization errors**: Discretizing continuous actions loses information
2. **Inference bottleneck**: Autoregressive decoding is slow
3. **Visual forgetting**: Catastrophic forgetting during fine-tuning
4. **Limited resolution**: Fine-grained control requires thousands of bins

**Recent Improvements:**
- **FAST tokenizer** (2025): 15x faster inference via compression
- **OFT recipe** (March 2025): 25-50x faster, 20%+ success rate boost
- **Emergent world model**: Research shows OpenVLA encodes latent state transitions

**Source:** [OpenVLA GitHub](https://github.com/openvla/openvla), [arXiv Paper](https://arxiv.org/abs/2406.09246)

---

## 3. Technical Deep Dive

### 3.1 Why Pure VLA Has Limitations

#### Hallucination and Stability
> "Reasoning-driven approaches still face challenges of hallucination, stability, and interpretability, while efficiency mechanisms often trade off accuracy or generality."

**Key Issues:**
- **Shortsighted decisions**: Single forward pass doesn't consider long-term consequences
- **Semantic-physical mismatch**: VLM knowledge doesn't always translate to valid physics
- **Distribution shift**: Novel environments cause unpredictable behavior

#### Generalization Failures
> "VLA approaches demonstrably falter in unstructured, real-world settings—particularly when confronted with novel objects, ambiguous natural language instructions, or previously unseen environmental configurations."

**Specific Limitations:**
| Challenge | Impact |
|-----------|--------|
| Novel objects | Cannot reason about unfamiliar item properties |
| Ambiguous instructions | Misinterprets complex or unclear commands |
| New environments | Performance degrades outside training distribution |
| Lighting changes | Vision backbone sensitivity |
| Long-horizon tasks | No explicit planning mechanism |

#### Control Precision
> "These models demonstrate impressive generalization to unseen objects and tasks, yet their control precision is somewhat limited by the discretization of actions."

**Source:** [VLA Survey](https://arxiv.org/html/2509.19012v1)

---

### 3.2 Why Pure World Model Has Limitations

#### Semantic Understanding Gap
> "World Models (WMs) face limitations in abstract reasoning and generalization. They struggle with open-ended semantic tasks due to their focus on physical simulation rather than contextual understanding."

**Key Issues:**
- **No language grounding**: Cannot interpret natural language instructions natively
- **Domain specificity**: Models trained on rigid objects fail on deformable materials
- **Lack of common sense**: Physical simulation without semantic context

#### The Semantic-Physical Misalignment
> "MLLMs enable contextual task reasoning but overlook physical constraints, while WMs excel at physics-aware simulation but lack high-level semantics."

**Challenges in Joint Systems:**
1. **Real-time synchronization**: High-latency semantic processing vs. fast physics
2. **Plan-reality mismatch**: MLLM plans may violate unmodeled physical constraints
3. **Memory management**: Continuous WM updates can overwhelm semantic context

#### Empirical Evidence
> "Today's general-purpose vision-language AI models—which understand images and text but do not generate clearly defined world models—often make errors; a benchmark paper presented at a 2025 conference reports 'striking limitations' in their basic world-modeling abilities, including 'near-random accuracy when distinguishing motion trajectories.'"

**Source:** [Embodied AI Survey](https://arxiv.org/html/2509.20021v1), [Scientific American](https://www.scientificamerican.com/article/world-models-could-unlock-the-next-revolution-in-artificial-intelligence/)

---

### 3.3 How Joint Architectures Solve Both Problems

#### The Dual-System Solution

```
┌────────────────────────────────────────────────────────────────────┐
│              JOINT VLA + WORLD MODEL ARCHITECTURE                   │
├────────────────────────────────────────────────────────────────────┤
│                                                                     │
│    VLM PROVIDES:                    WORLD MODEL PROVIDES:          │
│    ├─ Semantic grounding            ├─ Physics prediction          │
│    ├─ Language understanding        ├─ Long-horizon planning       │
│    ├─ Common-sense reasoning        ├─ Consequence anticipation    │
│    └─ Generalization to new tasks   └─ Safety verification         │
│                                                                     │
│                          SYNERGY                                    │
│    ┌─────────────────────────────────────────────────────────┐     │
│    │  MLLMs enhance WMs: semantic knowledge for task         │     │
│    │  decomposition and long-horizon reasoning               │     │
│    │                                                         │     │
│    │  WMs assist MLLMs: physical world representations      │     │
│    │  and future predictions for grounded decision-making   │     │
│    └─────────────────────────────────────────────────────────┘     │
│                                                                     │
└────────────────────────────────────────────────────────────────────┘
```

**Research Insight:**
> "MLLMs can enhance WMs by injecting semantic knowledge for task decomposition and long-horizon reasoning, while WMs can assist MLLMs by building the physical world's internal representations and future predictions, making joint MLLM-WM a promising architecture for embodied systems."

---

### 3.4 "Imagine-and-Verify Loop" Concept

Several recent approaches implement verification mechanisms:

#### VLA-in-the-Loop Framework
> "The core innovation lies in the use of a lightweight, composite World Model, not for continuous state prediction, but as an on-demand, event-triggered 'corrector.'"

**Process:**
1. VLA proposes action
2. World model checks viability
3. If unviable: generate video of successful trajectory
4. IDM extracts corrected actions
5. Execute corrected, robust action

#### Chain-of-Thought Visual Planning (CoT-VLA)
> "The model interposes latent visual goals—predicting future images—before generating corresponding action sequences, effectively embedding a sub-goal generation mechanism."

#### WorldVLA: Joint Imagination and Action
> "WorldVLA introduces an autoregressive action world model that jointly learns to predict visual outcomes and generate actions within a unified token-based architecture. This mutual reinforcement between the world model and the action model improves both visual imagination and action generation."

---

### 3.5 Action Chunking vs Continuous Prediction

#### Action Chunking
**Definition:** Predicting multiple actions at once, executing them without intermediate replanning.

| Model | Chunk Size | Frequency | Approach |
|-------|------------|-----------|----------|
| π0 | 50 actions | 50 Hz | Flow matching |
| OpenVLA-OFT | 16 actions | Variable | Parallel decoding |
| GR00T N1 | 16 actions | 120 Hz | Diffusion |

**Benefits:**
- Temporal consistency across actions
- Reduced compounding error
- Lower latency (batch processing)

**Trade-off:**
> "Action chunking linearly scales up action dimensions in VLA models with increased chunking sizes. This reduces the inference efficiency."

#### Continuous Prediction (Flow/Diffusion)
**Mechanism:** Start with noise, progressively denoise to smooth action trajectory

**Advantages:**
- Higher precision than discrete tokens
- Natural temporal smoothness
- Better for dexterous manipulation

**Research Finding:**
> "Diffusion-based decoders demonstrate superior cross-domain transfer and robustness compared to autoregressive heads."

**Source:** [VLA Survey](https://vla-survey.github.io/)

---

## 4. Current Research Trends

### 4.1 Key Papers Comparing VLA vs World Model (2024-2025)

| Paper | Focus | Key Finding |
|-------|-------|-------------|
| **GR-2** (ByteDance, Oct 2024) | Video generation pretraining | 38M video pretraining enables world model behavior in VLA |
| **TriVLA** (2025) | Triple-system architecture | Adding episodic world model (S3) improves both planning and control |
| **MinD** (2025) | Dual-system world model | Diffusion-based video generator + risk analysis |
| **WorldVLA** (2025) | Unified autoregressive | Joint visual prediction and action generation |
| **UWM** (2025) | Unified World Models | Action + video diffusion in single transformer |
| **VLA-in-the-Loop** (2025) | Online correction | World model as event-triggered corrector |

### 4.2 Benchmark Results Summary

#### LIBERO Benchmark
| Model | Success Rate | Notes |
|-------|--------------|-------|
| Recent SOTA | >95-99% | Near-saturated, clean splits |
| With robustness analysis | ~60-70% | Reveals memorization tendency |

#### VLABench (2025)
| Model | Track 1 SR |
|-------|------------|
| Pi0-5-ft-primitive | 40.6% |
| Pi0-ft-primitive (10 tasks) | 47% |
| Pifast-ft-primitive | 51.2% |

#### OpenVLA Comparisons
| Comparison | Result |
|------------|--------|
| OpenVLA vs RT-2-X (55B) | +16.5% success rate |
| VLA² vs OpenVLA | +44.2% on hard benchmarks |
| OpenVLA-OFT vs OpenVLA | 26x faster, 3x lower latency |

#### Meta V-JEPA 2 (World Model)
- **Zero-shot robot planning**: 65-80% success rate
- **Environment**: New objects in unseen environments
- **No robot-specific training data required**

### 4.3 Company/Lab Approaches Summary

| Organization | Primary Approach | Key System | Philosophy |
|--------------|------------------|------------|------------|
| **Physical Intelligence** | VLA (Flow Matching) | π0, π0.5 | Single unified model |
| **NVIDIA** | Dual-System VLA | GR00T N1 | System 1/2 + World Models (Cosmos) |
| **Figure AI** | Dual-System VLA | Helix | Fast reactive + slow reasoning |
| **Google DeepMind** | VLA + Embodied Reasoning | Gemini Robotics | Agentic dual-model |
| **Meta** | World Model | V-JEPA 2 | Predict then act (LeCun vision) |
| **1X Technologies** | World Model + IDM | 1XWM | Imagine then act |
| **ByteDance** | Video-pretrained VLA | GR-2 | World model via video generation |
| **Stanford/Berkeley** | Open-source VLA | OpenVLA | Accessible baseline |

### 4.4 Research Community Debate

#### The LeCun Position (World Models)
> "The path to superintelligence - just train up the LLMs, train on more synthetic data, hire thousands of people to school your system in post-training, invent new tweaks on RL - I think is complete bullshit. It's just never going to work." - Yann LeCun

**Key Arguments:**
- LLMs are a "dead end" for physical intelligence
- Robots need to predict consequences before acting
- Dog-level intelligence is harder than human-level (Moravec's paradox)
- Simulation of physics (friction, manipulation) is fundamentally hard

#### The VLA Position (Direct Policy)
**Key Arguments:**
- End-to-end learning captures what matters
- VLMs provide crucial semantic grounding
- Simpler architecture, faster deployment
- Scaling laws suggest continued improvement

#### Emerging Consensus
> "The designs and functions of LLMs, VLMs, and VLAs align with the spirit of world models, as they aim to represent and reason about world dynamics. Therefore, these models should not be excluded from the broader conceptual scope of world modeling."

**Source:** [World Models Survey](https://arxiv.org/html/2511.02097v2), [Yann LeCun Interview](https://techcrunch.com/2025/12/19/yann-lecun-confirms-his-new-world-model-startup-reportedly-seeks-5b-valuation/)

---

## 5. Future Direction

### 5.1 Will VLA and World Model Converge?

**Evidence of Convergence:**

1. **Architectural Integration**
   - TriVLA: Triple-system with explicit world model (System 3)
   - GR-2: Video prediction as implicit world model
   - WorldVLA: Unified autoregressive action world model
   - NVIDIA Cosmos + GR00T: World model for data generation + VLA for control

2. **Shared Design Patterns**
   - Dual-system architectures becoming standard
   - Flow matching/diffusion replacing autoregressive
   - Video pretraining as common practice

3. **Research Trajectory**
   > "These efforts indicate a converging trajectory toward generalist embodied agents trained on massive, diverse datasets and equipped with modular reasoning capabilities."

### 5.2 Expert Opinions on the "Right" Architecture

| Expert/Organization | View | Predicted Architecture |
|---------------------|------|------------------------|
| **Yann LeCun (Meta)** | World models essential | JEPA-based prediction + planning |
| **Physical Intelligence** | VLA sufficient with scale | Unified flow-matching policy |
| **NVIDIA** | Hybrid necessary | Dual-system VLA + world model data |
| **Google DeepMind** | Agentic systems | Separate reasoning + action models |
| **Academic Consensus** | Convergence inevitable | Modular systems with both |

**Key Insight:**
> "Solely relying on LLMs, VLMs, or VLAs often constrains a system's capacity for long-horizon prediction, reasoning, and imagination, all of which are essential for modeling dynamic and interactive environments."

### 5.3 Timeline Predictions

```
┌──────────────────────────────────────────────────────────────────┐
│                    PHYSICAL AI TIMELINE                           │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  2024 │ Foundation laid                                          │
│       │ - π0, OpenVLA released                                   │
│       │ - RT-2 demonstrates VLA paradigm                         │
│       │ - Genie 1/2 show video world models                      │
│       │                                                          │
│  2025 │ Dual-system architectures mature                         │
│       │ - GR00T N1, Helix, Gemini Robotics released             │
│       │ - 1X World Model demonstrates imagine-then-act           │
│       │ - V-JEPA 2 achieves zero-shot robot control             │
│       │ - Commercial pilots begin                                │
│       │                                                          │
│  2026 │ Commercial deployment                                    │
│       │ - Boston Dynamics + DeepMind partnership                 │
│       │ - Warehouse/logistics scaling                            │
│       │ - Industry-specific solutions                            │
│       │                                                          │
│  2027-│ Mass market adoption                                     │
│  2030 │ - Collaborative robots in homes/factories                │
│       │ - Cross-embodiment generalization                        │
│       │ - Unified foundation models                              │
│       │                                                          │
│  2030+│ AGI integration possibility                              │
│       │ - Physical AI as AGI testing ground                      │
│       │ - Human-level manipulation dexterity                     │
│       │                                                          │
└──────────────────────────────────────────────────────────────────┘
```

**Market Projections:**
- AI robotics sector: $16.10B (2024) → $124.77B (2030)
- Physical Intelligence raised $400M at $2.4B valuation (2024)
- World Labs raised $230M at $1B valuation (2024)
- Yann LeCun's AMI seeking $5B+ valuation (2025)

### 5.4 Open Challenges

1. **Data Scarcity**
   > "The prevailing narrative is that data scarcity is the primary bottleneck holding robotics back from achieving the success seen in language applications."

2. **Cross-Robot Generalization**
   > "Cross-robot action generalization remains a central challenge in VLA research... it still falls short of achieving true zero-shot generalization."

3. **Real-Time Constraints**
   > "Unlike text or images, robotic actions must be physically grounded and executed in real-time, imposing constraints on model latency and reliability."

4. **Safety and Verification**
   > "The five biggest challenges in VLA models are: (1) Representation, (2) Execution, (3) Generalization, (4) Safety, and (5) Dataset and Evaluation."

---

## 6. Conclusion

### Key Takeaways

1. **VLA and World Models are converging**, not competing. The most successful systems (GR00T N1, Helix, TriVLA) combine semantic understanding from VLMs with predictive capabilities from world models.

2. **Dual-system architectures dominate** current production systems, inspired by Kahneman's "Thinking, Fast and Slow" cognitive framework. System 2 (slow reasoning) + System 1 (fast action) is becoming the standard pattern.

3. **The debate is philosophical, not practical**. While Yann LeCun argues world models are essential and VLAs are insufficient, most production systems use VLA-centric approaches enhanced with world model components.

4. **Action chunking + flow matching** has emerged as the preferred action generation approach, replacing autoregressive discrete tokens for better precision and speed.

5. **Video pretraining** (GR-1, GR-2, 1X World Model) represents a promising path to inject world knowledge into robotic policies without requiring massive robot-specific datasets.

6. **Timeline**: We are at the beginning of commercial deployment (2025-2026), with mass market adoption expected 2027-2030.

### The Path Forward

The field is moving toward **modular, hierarchical architectures** that combine:
- Pre-trained VLMs for semantic understanding
- World models for prediction and planning
- Fast reactive policies for real-time control
- Verification loops for safety and robustness

The ultimate goal is **unified foundation models** that can perceive, reason, predict, and act across diverse embodiments and environments - bringing us closer to general-purpose physical AI.

---

## Sources

### Primary Sources
- [NVIDIA GR00T N1 Paper](https://arxiv.org/abs/2503.14734)
- [Physical Intelligence π0 Paper](https://arxiv.org/abs/2410.24164)
- [Figure AI Helix](https://www.figure.ai/news/helix)
- [Google DeepMind Gemini Robotics](https://deepmind.google/blog/gemini-robotics-brings-ai-into-the-physical-world/)
- [1X World Model](https://www.1x.tech/discover/1x-world-model)
- [Meta V-JEPA 2](https://ai.meta.com/blog/v-jepa-2-world-model-benchmarks/)
- [OpenVLA](https://github.com/openvla/openvla)

### Survey Papers
- [VLA Survey](https://vla-survey.github.io/)
- [Large VLM-based VLA Survey](https://arxiv.org/html/2508.13073v1)
- [Pure VLA Survey](https://arxiv.org/html/2509.19012v1)
- [World Models for Embodied AI](https://arxiv.org/html/2510.16732v1)
- [Embodied AI: From LLMs to World Models](https://arxiv.org/html/2509.20021v1)

### News and Analysis
- [Scientific American: World Models](https://www.scientificamerican.com/article/world-models-could-unlock-the-next-revolution-in-artificial-intelligence/)
- [Built In: AI World Models](https://builtin.com/articles/ai-world-models-explained)
- [Yann LeCun's AMI Startup](https://techcrunch.com/2025/12/19/yann-lecun-confirms-his-new-world-model-startup-reportedly-seeks-5b-valuation/)
- [ICLR 2026 VLA Analysis](https://mbreuss.github.io/blog_post_iclr_26_vla.html)

### GitHub Repositories
- [OpenVLA](https://github.com/openvla/openvla)
- [Physical Intelligence openpi](https://github.com/Physical-Intelligence/openpi)
- [ByteDance GR-1](https://github.com/bytedance/GR-1)
- [Awesome VLA List](https://github.com/JiuTian-VL/Large-VLM-based-VLA-for-Robotic-Manipulation)
