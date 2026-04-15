# Deep Analysis: Coding Agents for AI Systems Engineering and CUDA Kernels
**Speaker:** Ben Burtenshaw | **Company:** Hugging Face | **Date:** 2026-04-26

## Core Beliefs

**"Go closer to the silicon" to remain relevant.** Burtenshaw's central thesis is a career survival strategy for engineers: as coding agents commoditize high-level application logic, the "contemporary" engineer must retreat into harder, lower-level problems like CUDA kernels and systems engineering. He views the "acceptance gradient" of AI tools as a signal that the frontier has moved from "can it code?" to "can it optimize hardware?"

**Memory, not compute, is the true bottleneck.** He challenges the layman's focus on FLOPs (compute) by highlighting that H100s often sit idle because of memory bandwidth limits (3TB/s vs. Petaflops). The goal of "Agentic Systems Engineering" isn't just to write code that runs, but to increase "arithmetic intensity" — keeping the GPUs "warm" by maximizing math per memory read. This moves the agent's objective from *functionality* to *efficiency*.

**Verifiability is the permission slip for autonomy.** The reason he can run an "AutoLab" for hours without supervision is that the output (training loss, benchmark speedup, bits-per-byte) is mathematically verifiable. He implicitly argues that the more "objective" the field (like CUDA or ML training), the more autonomy we can safely grant to agents.

**Standardized repos are the "Hard Drive" for agents.** Burtenshaw posits that the Hugging Face Hub isn't just for humans; it's the persistent storage layer for agents. Agents need a place to version, share, and "publish" kernels and models. This turns the Hub into a coordination fabric where one agent's output is another agent's "Skill."

## Pain Points

**The "Installation Hell" of specialized hardware.** The matrix of hardware generations, CUDA versions, and software dependencies is too complex for humans to manage manually. He sees this as a primary reason why kernel writing was seen as "unattainable" for agents. His solution is standardized project metadata (TOML) that makes the environment legible to the agent.

**The "Zeroshot" Quality Ceiling.** He describes "zeroshot" as the enemy of complex engineering. The "boring" but necessary part of his talk is the emphasis on "Skills" — file-based, version-controlled context that takes an agent from guessing to "few-shot" execution.

**Abstracted APIs as "Ceilings."** Burtenshaw makes a subtle but critical point: high-level abstractions often prevent agents from solving hard problems. Sometimes you need to "expose" the raw primitives (the "Silicon") so the agent can optimize beneath the abstraction layer.

**The "Overnight Agent" Anxiety.** Mentioned in the transition to the next speaker: the new psychological stress of "did my agent fail while I was sleeping?" This signals a shift from "work-life balance" to "agent-monitoring balance."

## Architecture & Technical Patterns

**The Game Boss Progression of Autonomy:**
1. **Interactive Hybrid:** Human + Agent writing CUDA kernels (High friction, high control).
2. **Zeroshot/Few-shot Tasks:** Agent fine-tuning a model (Medium autonomy).
3. **Multi-Agent Auto-Research (AutoLab):** A decentralized team (Researcher, Planner, Worker, Reporter) running for hours (High autonomy).

**The AutoLab Multi-Agent Loop:**
- **Researcher:** Literature scout (uses HF Papers CLI to search Archive).
- **Planner:** Maintains the hypothesis queue and experiment state.
- **Worker:** Implements the training script/architecture changes.
- **Reporter:** Monitors runs using Trachio and maintains a "Data Lake" of results.

**Trachio & Parquet as Agent-Native Observability.** Using open data formats (Parquet) instead of proprietary dashboards. This allows the agent to "read its own results" and generate its own visualizations (Gantt charts) to decide the next move.

**Skills as Versioned Context.** Projects at Hugging Face now include "managed skills" — context files maintained by the repo owners. This is an architectural decision to move context *into* the repo rather than keeping it in the prompt.

## Hidden Signals

**"Preaching to the choir."** This confirms the AI Engineer conference is no longer about convincing people AI is useful, but about defining the *standards* of its use.

**"Easy Speedups."** He suggests that because so much code isn't tuned for specific hardware, there is "low-hanging fruit" where agents can provide 94%+ speedups simply by re-writing kernels for a specific GPU generation. This implies a future where "Generic" software is replaced by "Agent-Optimized" hardware-specific variants.

**"Gas Town" as a metaphor for the Wild West.** Referencing his experimental projects as "Gas Town" signals a subculture of high-risk, high-autonomy experimentation that happens outside the "well-maintained" corporate repos.

**The shift from "Coding Agent" to "Research Agent."** By splitting the agent into Researcher/Planner/Worker, he is moving away from the "Chat with Code" paradigm toward a "Scientific Method" paradigm.

## What's NOT Said

**Energy and Token Costs.** Running a multi-agent "AutoLab" for hours on H100s is incredibly expensive. Burtenshaw omits the "cloud bill" aspect, focusing purely on the engineering triumph.

**The "Hallucination" of CUDA.** Writing CUDA is notoriously difficult to debug. He doesn't explain how he prevents an agent from writing code that compiles but subtly corrupts memory or produces incorrect math (beyond basic unit tests).

**Safety in Autonomous Research.** While his example is CUDA, the "Researcher" agent pulling from Archive and "Worker" agent implementing it is a pattern that could be applied to more sensitive domains. He avoids the "dual-use" or "biosafety" conversation entirely.

## Speaker's Worldview

Burtenshaw is a **Hardware Realist**. He believes the "AI layer" of the stack is becoming too crowded and that the real value (and career safety) lies in the "Silicon layer." He views agents not as assistants, but as the primary "residents" of the Hugging Face Hub, with humans acting as the architects of their environment and the verifiers of their results. He values "Open Primitives" over "Closed Abstractions."

## Key Quotes

1. **"The GPU is often waiting idle for this tensors to come back... we keep the GPUs warm."** — The core goal of kernel optimization.
2. **"Standard repos are the hard drive for agents."** — Redefining the Hugging Face Hub as an agentic operating system.
3. **"It takes a task from being zeroot to being fshot."** — On the necessity of file-based "Skills."
4. **"We don't always need to extract [abstract]; it's more about exposing well."** — A critique of the "black box" API trend.
5. **"Go closer to the silicon... that's where AI systems engineering comes in."** — His call to action for the "contemporary" engineer.
