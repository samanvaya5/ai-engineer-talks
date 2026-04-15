# Deep Analysis: Replacing 15,000 Lines of Code in Cursor with Markdown Skills
**Speaker:** David Gomes | **Company:** Cursor (Anysphere) | **Date:** 2026-04-26

## Core Beliefs

**Markdown is the new code.** Gomes's most radical claim is that mature agentic features should be migrated *out* of native application code (TypeScript/Rust) and *into* prose-based "Skills." By replacing 15,000 lines of complex worktree management code with a 40-line Markdown file, Cursor is betting that LLM reasoning is now a reliable enough "runtime" to handle system-level logic. This isn't just a refactor; it's a paradigm shift in how features are architected.

**The "Best of N" Competition is the superior workflow.** Parallization is not just for speed, but for quality. Gomes views the IDE's role as a "Tournament Organizer" where multiple models (Opus, GPT-4, Grok, etc.) compete on the same prompt. The value isn't in the raw code they produce, but in the "Parent Agent's" ability to critique their differences and "stitch together" a hybrid solution that no single model could have achieved alone.

**Sub-agents are the primary scaling primitive.** The complexity of managing multiple repos and worktrees is delegated to specialized sub-agents. The "Parent" is an orchestrator/judge, not a doer. This hierarchy is what allows Cursor to support multi-repo setups and complex "judging" logic that was previously impossible in a monolithic agent.

**Maintainability is a first-class feature.** Gomes is refreshingly honest about "selfish" maintenance. For advanced features used by only 10% of users, the cost of "native" code (tests, dependencies, edge cases) is too high. "Agentic implementation" is the solution for the "Power User Long Tail."

## Pain Points

**The "Vibes-Based" Safety Gap.** Moving from native OS-level isolation (where the app physically blocks file access) to "prompt-based" isolation (where we ask the agent nicely to stay in the directory) is dangerous. Gomes admits it is "vibes-based" and "knock on wood." Agents, especially smaller ones like Haiku, "leak" out of their worktrees or "go haywire" in long sessions.

**The "Slower Feeling" of Transparency.** Because the agent now creates the worktree "in-chat," it feels slower to the user, even if the actual execution time is the same. The "magic" of the background process has been replaced by the "manual labor" of the agent, which can be a psychological regression in UX.

**Discoverability vs. Cleanliness.** By hiding advanced features behind slash commands (`/worktree`, `/best-of-n`), Cursor is sacrificing discoverability to keep the UI from becoming "polluted." This creates a "Power User Wall" where users have to already know the feature exists to find it.

**Disk Bloat and Git Rigidity.** Git worktrees are slow, disk-heavy, and only work in Git repos. This is a technical "ceiling" that prevents the feature from working in large monorepos or non-standard project structures.

## Architecture & Technical Patterns

**Server-Side Feature Injection.** Commands like `/worktree` are server-side prompts. This allows Cursor to iterate on the "logic" of a feature (improving the prompt) and deploy it to all users instantly without requiring an app update. The feature is "live-streamed" into the client.

**Orchestrator-Judge-Worker Pattern:**
- **User:** Issues the slash command.
- **Parent Agent:** Plans the worktrees, spins up sub-agents.
- **Sub-Agents:** Execute tasks in isolated environments.
- **Parent Agent (Judge):** Reviews outputs, grades them, and presents a table to the user.
- **User/Parent:** Merges the "best bits" into the primary checkout.

**Evals for "Constraint Leakage."** Cursor uses headless CLI evals (via Brain Trust) with two specific scorers:
1. **Positive Scorer:** Did the work happen in the worktree?
2. **Negative Scorer:** Did the agent touch *anything* in the primary checkout?

**RL-Driven Boundary Training.** Cursor is training its "Composer" models specifically on "Directory Boundary Tasks." They are baking "Stay in your worktree" into the model's weights, acknowledging that prompting alone is insufficient for system-level reliability.

## Hidden Signals

**The "Haiku" Litmus Test.** Gomes's observation that Haiku fails the isolation test while Grok/Composer pass signals that "Agentic Engineering" has a minimum reasoning floor. You cannot "compress" these complex workflows into cheap models yet.

**Cursor 3.0 "Agent Window" Inversion.** A subtle hint that Cursor 3.0 moves away from the "Editor with a Chatbox" to an "Agent UI where you happen to see code." This is a total inversion of the IDE hierarchy.

**"Selfish Maintenance."** This signal is loud: the Cursor team is small and overwhelmed. They are using AI to solve their own "engineering debt" problem, even at the cost of "native" reliability.

**Parallelization beyond Git.** Gomes's tease of a non-Git parallelization primitive suggests Cursor is looking at virtualized filesystems or container-based "sandboxes" as the next level of isolation.

## What's NOT Said

**Security of Sub-agents.** There is no discussion of how sub-agents are sandboxed against malicious code execution during "setup scripts."

**The "Mixed Feedback" Details.** He brushes over the user complaints on the forums. The subtext is that power users hate losing the "native" UI control and feel like the "agentic" version is less predictable.

**Competitive Benchmarking.** He compares models (Kimmy, Grok, GPT) but doesn't mention *how* he chooses which models are "worthy" of being sub-agents in the default best-of-n.

## Speaker's Worldview

Gomes is an **Agentic Pragmatist**. He believes that the complexity of modern software can only be managed by "deleting code" and replacing it with "reasoning." He values **Iterative Agility** over **Native Perfection**. He views the IDE not as a tool for the human to write code, but as an **Orchestration Layer** for a fleet of models. His worldview is one where the "Prose" of the prompt becomes the "Source of Truth" for the entire application.

## Key Quotes

1. **"Markdown is basically the new code."** — The core thesis of the talk.
2. **"I recently opened a PR... it was a massive deletion of code. Like 15,000 lines."** — The "Architecture by Deletion" mantra.
3. **"The agent will hallucinate or go a bit haywire and start doing things they shouldn't."** — Honest admission of the "vibes-based" safety risk.
4. **"The parent can stitch together pieces and bits from different implementations."** — The value proposition of Best-of-N.
5. **"If the agent can't see something, it can surely not respect it."** (Actually said by Ronacher, but echoed by Gomes's worktree logic).
