# Deep Analysis: Using AI to Evaluate, Debug, and Manage Complex AI Systems
**Speaker:** Lawrence Jones | **Company:** incident.io | **Date:** 2026-04-26

## Core Beliefs

**Complex AI systems have crossed the threshold of human tractability.** Jones’ most sobering belief is that we are building systems (graphs of 50+ agents and thousands of prompts) that no human can debug or even fully understand in real-time. He argues that we have moved past the era of "looking at logs" and into an era where "you need AI assistance just to tell you if your AI is working."

**The File System is the ultimate "Agent UI."** While the industry is obsessed with building sophisticated dashboards and MCP servers, Jones believes the most effective interface for an agent is a structured, self-documenting file system. He argues that because tools like Claude Code and Cursor are optimized for `ls`, `grep`, and `cat`, we should "project" our internal system states into folders and files. This is a "back to basics" approach that leverages the agent's core training.

**Evals are "AI Unit Tests" and must be integrated into the dev loop.** Jones treats prompts as production code that requires the same rigor as Go or Rust. He believes every prompt change must be preceded by a failing eval and followed by a regression check. His workflow—failing eval -> prompt tweak -> pass -> simplify—is essentially TDD (Test-Driven Development) for LLMs.

**Meta-Analysis is the only way to scale quality.** You cannot improve a system by looking at individual failures once you hit a certain scale. You must use "Analysis Pipelines" (like their `scrapbook` tool) to cluster failures into cohorts, identify meta-patterns, and generate structural fixes. The human's role is to review the "Meta-Report," not the individual traces.

## Pain Points

**The "Context Limit" of Evals.** A major practical pain point: as eval suites grow to represent complex production incidents (2MB+ YAML files), they become unreadable for the very agents meant to fix them. The tools "choke" on their own tests.

**UI Blindness for Agents.** We build beautiful traces and graphs for humans (like LangSmith or internal UIs), but these are "invisible" to coding agents. This creates a friction point where a human sees a bug in a UI but can't easily "give" that bug to an agent to fix.

**Prompt Bloat.** The phenomenon where repeatedly "fixing" a prompt with new instructions makes it massive, contradictory, and expensive. Jones identifies the need for a "consolidation/simplification" pass at the end of every fix cycle.

**The "Needle in a Haystack" of Agent Graphs.** In a system with 50 agents, identifying which specific prompt in which sub-agent caused a subtle logic error is nearly impossible for a human. The "Trace" becomes too noisy to be useful.

## Architecture & Technical Patterns

**UI-to-FS Translation (Downloadable Traces).** Incident.io builds "Download as File System" buttons into their debugging UIs. This converts a complex trace into a folder structure that a coding agent can "explore." The hierarchy of the folders mirrors the hierarchy of the agent calls.

**The `eval tool` CLI.** Instead of agents reading entire YAML files, they use a specialized CLI to `query`, `replace`, or `add` specific test cases. This keeps the agent's context window focused on the *diff* and the *logic*, not the boilerplate.

**Red-Green Prompt Workflow.** An automated runbook where:
1. Agent creates a failing test case in the `eval tool`.
2. Agent modifies the prompt in the codebase.
3. Agent runs the test suite (using the CLI).
4. Agent performs a "Consolidation Pass" to keep the prompt clean.

**Scrapbook (Parallel Analysis Pipelines).** A repository of markdown "playbooks" that guide agents through batch analysis.
- **Stage 1:** 25 sub-agents analyze 25 failure cases in parallel.
- **Stage 2:** A "Summarizer Agent" clusters these into failure cohorts.
- **Stage 3:** The agent cross-references failures with the current Go codebase to propose architectural changes.

**Decision Logs in Code.** Treating the "thought process" of an agent as a first-class data citizen that is committed alongside the code or trace, allowing for "Incremental Analysis" that can be resumed.

## Hidden Signals

**"We pay you when things go wrong."** This is a high-stakes environment. Jones is signaling that "good enough" AI isn't acceptable in incident response. The rigor of their internal tooling is a direct result of the "Criticality" of their domain.

**"Self-documenting" file systems.** Jones is essentially saying that the "Folder Name" is a prompt. By naming a folder `step_04_log_query_generation`, he is providing zero-shot context to the agent. This is a subtle form of "Context Engineering" through infrastructure.

**"I would hope no one has this in production."** (Regarding the Pirate Prompt). A moment of levity that signals a "No-Bullshit" engineering culture. He’s acknowledging that early AI engineering is full of "cringe" hacks, but the goal is to move toward the "Professional AI SRE" standard.

**"Claw code / Codecs" mentioned by name.** Jones is using the latest, unreleased, or frontier tools. He’s signaling that Incident.io isn't just a "wrapper" company; they are a "power user" company that influences the tool-builders themselves.

## What's NOT Said

**Token Costs of Parallel Analysis.** Running 25 agents in parallel to analyze batch failures is likely extremely expensive. There is no mention of the ROI or the "cost-per-fix" of this automated pipeline.

**The "Human Oversight" frequency.** He talks about "Reviewing the PR," but how much time does a human spend checking the "Meta-Analysis" for hallucinations? He presents a very "clean" loop that might be messier in practice.

**Vendor Lock-in.** The heavy reliance on "Claude Code" and specific file-system navigation patterns suggests a workflow that might not translate to other agentic frameworks that don't prioritize terminal/FS access.

## Speaker's Worldview

Lawrence Jones is a **Systems Disciplinarian**. He believes that AI is a "wild" technology that can only be tamed through the most rigorous application of traditional software engineering principles (TDD, Unit Testing, CLI tooling). His worldview is that we should stop trying to "talk" to our systems and start "instrumenting" them so that other AIs can manage them. He sees the future not as "AI replacing engineers," but as "Engineers building AI systems that are managed by AI."

## Key Quotes

1.  **"Systems are now complicated enough that you can't as a human really tractably dig into how these things are performing. You need assistance to help you."** — The "Complexity Singularity" for software debugging.

2.  **"Download all of the UI that we have as a file system... these agents are fantastic at using file systems and just going through this data using standard tools."** — The most practical technical insight: don't build a better UI, build a better Folder.

3.  **"Prompt engineering is just software engineering... you want every time you make an adjustment to try and simplify as well."** — Treating the prompt as a refactorable asset.

4.  **"File systems are exceptionally good agent context. [...] It wouldn't have been half as effective as this ability to just download in bulk."** — Rejecting the "Conversational" interface in favor of the "Batch/Navigational" interface.
