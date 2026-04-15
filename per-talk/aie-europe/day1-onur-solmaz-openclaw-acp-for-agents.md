# Deep Analysis: ACP for Standardized Agent Interactions
**Speaker:** Onur Solmaz | **Company:** TextCortex (Founding Engineer) / OpenClaw Maintainer | **Date:** 2026-04-26

## Core Beliefs

**Interoperability via ACP is the antidote to wasted engineering effort.** Solmaz argues that the current state of agentic development—where every editor (VS Code, Zed, etc.) builds its own proprietary plugins for every agent (Codex, Cloud Code)—is a massive waste of resources. His core belief is that the Agent Client Protocol (ACP) will do for agents what LSP did for language support: standardize the interface so you "build once, ship everywhere."

**Apply agents generously—like an "ointment."** This is a visceral metaphor. Solmaz believes AI shouldn't just be used for big tasks; it should be applied to *every* friction point in the development lifecycle. If a problem can be solved with an agent, it *must* be solved with an agent to take the human out of the low-leverage loop.

**We are moving from "Building Agents" to "Automating the Automator."** The focus is shifting toward higher-level orchestration. Solmaz believes the real value lies in creating Standard Operating Procedures (SOPs) for agents—programmatic workflows that guide agents through complex tasks (like PR reviews) without human intervention.

**The Enterprise agent will be defined by massive inference consumption.** He sees a divergence between personal and enterprise agents. While personal agents are "assistants," enterprise agents will be task-specific, on-demand, and consume vastly more compute. This necessitates a shift toward robust infrastructure (Kubernetes) for agent management.

## Pain Points

**The "Telephone Game" of Multi-Model Oracles.** He describes the frustration of using one model (like Opus) to direct another (like Codex). The loss of nuance in this "paraphrasing" loop leads to suboptimal results, identifying a need for direct, standardized protocols (ACP) that preserve intent across model boundaries.

**"AI Slop" in Pull Requests.** As a maintainer, he identifies a growing problem: high-volume, low-quality, AI-generated PRs that maintainers can't ignore (because they might contain valid fixes) but struggle to process manually. This "slop" is the primary driver for his work on automated PR triage.

**Lack of Multi-Agent Provisioning in Chat Apps.** Current platforms (Slack, Discord) make it difficult to talk to multiple task-specific agents simultaneously without creating dozens of separate "Apps." This cosmetic and technical limitation is a bottleneck for "one agent per task" workflows.

**Permission Bloat vs. Manual Toil.** Finding the balance between giving an agent enough access to be useful (SSH, GitHub CLI) and preventing it from "deleting all your emails" or creating premature PRs.

## Architecture & Technical Patterns

**Standard Operating Procedures (SOPs) for Agents.** Solmaz’s ACPX engine implements "RALF" (Review-Refactor Loops). These are not just prompts but structured programs (JSON output, ETL-like flows) that force agents through mechanical checks (linting, CI passing, intent judging) before a human ever sees the code.

**On-Demand Disposable Agents on Kubernetes.** Instead of a single persistent agent, his "Spritz" orchestrator spins up dedicated Kubernetes pods for specific tasks. This provides the "full computer" power of OpenClaw with the isolation and scalability of enterprise infra.

**State and Data Synchronization.** To solve the "mobile-first" development problem, he uses synchronization algorithms (inspired by Dropbox/Rsync) to keep workspace files consistent between the agent's remote pod and the user's local or mobile interface.

**Discord-Driven Development (DDD/TDD).** A workflow where 1-5 agents operate in parallel channels, coordinated by a human "commander." This is a radical departure from the single-threaded "IDE" model.

## Hidden Signals

**"OpenClaw is not secure... it's work in progress."** This is a rare, high-signal moment of candor. It signals that the current boom in agentic tools is currently prioritizing *capability* over *security*, and that the community is aware of the "root awakening" coming.

**"Apply agents generously... like an ointment."** This choice of words suggests that AI is seen as a healing or smoothing agent for the "pain" of traditional software development. It implies that "un-agentic" work is inherently painful or broken.

**"I chose ACP because... only Zed had built them."** This highlights the "Path of Least Resistance" in standard adoption. Standards don't win because they are better; they win because they are the first to be implemented in tools developers actually use.

**"Automating the automator."** This signals the emergence of a new "Meta-Engineer" role—someone who doesn't write code or even prompts, but rather designs the *systems* that govern agentic output.

## What's NOT Said

**The Economic Cost of "Generous Application."** Running 5 agents in parallel on Kubernetes pods for every side project is astronomically expensive in terms of both inference (tokens) and infrastructure (cloud spend). He ignores the ROI question entirely.

**Collision Resolution.** While he mentions state sync, he doesn't address how to handle situations where two parallel agents edit the same file in ways that conflict—the classic "merge conflict" problem, but at agent speed.

**The "Black Box" of SOPs.** If the SOP itself is flawed, the agentic loop could just become a faster way to generate "consistent garbage." He assumes the SOPs will be high-quality.

## Speaker's Worldview

Onur Solmaz is a "Systems Architect" of the agentic era. He views the current state of AI as a chaotic "Telephone Game" that needs to be disciplined through standardized protocols (ACP) and robust orchestration (Kubernetes). He is an "Automation Accelerationist" who believe that the solution to the problems created by AI (like PR slop) is *more* AI, applied through structured, programmatic workflows.

## Key Quotes

1. **"Apply agents generously on problems... like an ointment."**
2. **"Automating the automator."**
3. **"We are basically creating standard operating procedures for agents."**
4. **"I chose ACP because... only Zed had built them... if only you could just standardize them under one interface."**
5. **"One task per session... there will be a lot more money to be made in enterprise agents."**
