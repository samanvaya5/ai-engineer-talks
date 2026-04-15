# Deep Analysis: Architecting Long-Running, Multi-Day Agent Missions
**Speaker:** Luke Alvoeiro | **Company:** Factory | **Date:** 2026-04-26

## Core Beliefs

**Human attention is the primary bottleneck of software engineering, not intelligence.** Alvoeiro's opening claim is a direct challenge to the "LLMs aren't smart enough" narrative. He argues that today's models can already solve 50 features at once, but they fail because humans can't supervise 50 simultaneous implementations. The "mission" architecture is designed to shift the human role from "active developer" to "asynchronous project manager," treating human attention as a scarce resource to be optimized rather than an input into the loop.

**Autonomy requires a shift from "sessions" to "missions."** A single agent session is fragile; a "mission" is an ecosystem. This distinction is foundational. Sessions are limited by context windows and accumulated baggage; missions are defined by structured handoffs between ephemeral workers with clean slates. He is arguing that longevity (running for days or weeks) is an architectural property, not a model capability.

**Validation must be decoupled from implementation and adversarial by design.** He is deeply critical of the standard "write code, then write tests" workflow for agents. His belief is that tests written *after* code only confirm decisions already made—they don't catch bugs. True validation requires a "validation contract" written *before* a single line of code exists, and verified by agents who have never seen the implementation.

**Serial execution is superior to parallelism for complex software tasks.** This is a contrarian take in the "agent swarm" era. He argues that parallel agents conflict, step on each other, and make inconsistent architectural decisions, leading to coordination overhead that cancels out speed gains. His prescription is serial execution with targeted parallelism for read-only operations (research, code review). Correctness compounds; speed doesn't matter if you're drifting off course.

## Pain Points

**Context window "baggage" is the silent killer of long-running tasks.** He identifies a core failure mode: as an agent works on a feature, it accumulates context that eventually degrades its attention and leads to "hallucinations of the past." His solution—killing the worker agent after every feature and starting a new one with a git-inherited clean slate—is an admission that LLM attention is a finite, depleting resource within a session.

**Coordination overhead eats the gains of parallel agent teams.** He describes the "agent swarm" failure mode where multiple agents working on the same codebase create a "mess" of inconsistent decisions. The token cost of coordinating 10 agents can exceed the cost of the work itself. This reflects a reality most "multi-agent" demos ignore: software development is a highly coupled domain where the "unit of work" is rarely independent.

**Tests that confirm decisions rather than catching bugs.** He highlights the "coverage trap" where an agent achieves 90% test coverage, but the tests are simply reflections of the code's existing logic. This is a specific failure mode of agentic TDD: if the same agent writes the test and the code, the test will inherit the agent's blind spots.

**The "Bitter Lesson" anxiety.** He acknowledges the fear that a new model release (e.g., GPT-5 or Claude 4) will make complex orchestration frameworks obsolete. His response is to push logic into 700 lines of prompt-based skills rather than hard-coded state machines, but the anxiety remains a visible driver of his architectural choices.

## Architecture & Technical Patterns

**The Three-Role Architecture: Orchestrator, Worker, Validator.**
*   **Orchestrator:** Acts as the strategic sounding board and planner. Produces the "validation contract."
*   **Worker:** Ephemeral agents that implement features one by one, inheriting the codebase via Git.
*   **Validator:** Adversarial agents that verify behavior against the contract.

**Validation Contracts as the Source of Truth.** Written during the planning phase, these consist of assertions (sometimes hundreds) that define "done" independently of the implementation. This is essentially "Behavior Driven Development" (BDD) repurposed as an agentic guardrail.

**"Droid Whispering" and Model Routing.** He introduces a tiered model strategy:
*   **Planning:** Slow, careful reasoning models (e.g., o1).
*   **Implementation:** Fast, code-fluent models.
*   **Validation:** Precise instruction-following models (often from a different provider to avoid training-data bias).
This "model seat" strategy suggests that the future of agentic engineering is not picking the "best" model, but managing an ensemble of specialized failures.

**User Testing via Computer Use.** A critical component of their validation loop is spawning the app and using "computer use" agents to interact with the UI. He notes that missions spend most of their time "waiting for the real world" (e.g., page loads, button clicks) rather than generating tokens. This is a shift from LLM-as-generator to LLM-as-QA-engineer.

**Structured Handoffs and Self-Healing.** When a worker finishes, it produces a structured summary (features completed, issues found, commands run). If a validator fails a milestone, the orchestrator uses this handoff to rescope the mission and assign "corrective work." This is a closed-loop control system for software development.

## Hidden Signals

**"Droid Whispering" as a professional identity.** By naming this skill, Alvoeiro is attempting to carve out a new category of engineering. It's not "prompt engineering"; it's the intuition of how models *compose under pressure*. This signals that the "hard part" of agentic engineering is becoming the management of LLM idiosyncrasies, not the code itself.

**The shift from "Code Generation" to "System Verification."** The fact that 60% of time/tokens are spent on implementation but validation *never succeeds on the first try* is the most important data point in the talk. It signals that we have reached a point where generating code is "easy," but ensuring that code does what we wanted is the new "hard problem."

**Model-agnosticism as a security/correctness feature.** Using a different model provider for validation to avoid "training data bias" is a sophisticated insight. It implies that if a model was trained on a specific bug, it will likely miss that bug during review. Cross-provider validation is the "double-blind" study of AI engineering.

**Mission Control as a Productization of Anxiety.** The "Mission Control" UI (budget tracking, worker status, handoff summaries) is a direct response to the "black box" problem of long-running agents. It signals that users don't actually want autonomy; they want *traceable, supervised progress*.

## What's NOT Said

**The actual cost of a 16-day mission.** He mentions heavy prompt caching, but never gives a dollar amount for a 16-day run. If an agent is constantly researching and validating over 16 days, the token bill could easily rival or exceed a junior engineer's salary for the same period.

**The "Mission Control" fatigue.** While he says you can "go hang out with your friends," the reality of managing a 16-day mission involves constant "arguing with the orchestrator" and reviewing validation failures. The "human attention" bottleneck might just be shifting from "writing code" to "triaging agent failures."

**Security of live app interaction.** Spawning an app and letting an agent click buttons ("computer use") in a mission that can run for 16 days implies a very robust (and expensive) sandboxing infrastructure that he glosses over.

**The "Bitter Lesson" reality.** Despite claiming prompt-driven logic avoids the bitter lesson, a model with a massive context window and "perfect" reasoning would render the "ephemeral worker" and "serial feature" architecture redundant. The architecture is a workaround for *current* model limitations.

## Speaker's Worldview

Alvoeiro views the current state of AI as a period of "intelligence surplus but attention deficit." He sees the path to AGI-level software engineering not through bigger models, but through more disciplined orchestration. His worldview is deeply rooted in dev-tools and git-culture; he treats agents as volatile, baggage-heavy entities that must be contained within strict, adversarial roles. He is a "systemist"—someone who believes the harness is more important than the horse.

## Key Quotes

1. **"The bottleneck in software engineering nowadays is not intelligence. It's now limited by human attention."** — The core thesis.

2. **"Tests written after implementation don't catch bugs. They confirm decisions."** — A devastating critique of current agentic workflows.

3. **"We run features serially. ... It seems slower on paper, but the error rate drops dramatically and when you have tasks that run for many days, the sort of correctness compounds."** — The argument for discipline over speed.

4. **"Droid whispering... is this idea that you need to be able to mentally model how different LLMs interact, where they fail, how those failures compound over a multi-day run."** — The birth of a new engineering discipline.

5. **"Validation never succeeds on the first go. That's in the mission."** — An admission that failure is a feature, not a bug, of long-running autonomy.
