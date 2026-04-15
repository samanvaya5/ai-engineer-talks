# Deep Analysis: Vertical AI and Durable UI Artifacts over Chat
**Speaker:** Jacob Lauritzen | **Company:** Legora | **Date:** 2026-04-26

## Core Beliefs

**Chat is a low-bandwidth, one-dimensional constraint on agent potential.** Lauritzen argues that forcing complex "work trees" (DAGs) into a linear chat interface is the root cause of "context rot" and agent failure. Chat collapses the multidimensional nature of professional work into a single timeline, leading to compaction and loss of critical detail. His core belief is that agents should not be constrained by the limitations of human language.

**The "Economics of Production" have inverted.** Doing the work is now the cheapest part of the process. Planning and reviewing are the new bottlenecks. Therefore, the value of an AI tool is no longer in its ability to generate content (which is commoditized) but in its ability to facilitate "high-control" planning and "high-trust" verification.

**The Verifier’s Rule defines the frontier of Vertical AI.** If a task is easy to verify, AI will solve it. The challenge for Vertical AI (like Legal or Finance) is that the "truth" is often subjective or only verifiable in a distant future (e.g., a court ruling). Lauritzen believes the goal of Vertical AI is to engineer "proxies for verification" (like "Golden Contracts") to bring subjective tasks into the realm of solvable AI problems.

**Planning is a temporary crutch that will disappear.** He makes a bold claim that "planning isn't going to stay around." He views the big upfront planning phase (common in tools like Claude Code or Kilo) as inefficient. Instead, he believes in a combination of "Skills" (encoded judgment) and "Elicitation" (on-demand questioning), where the agent moves forward autonomously and logs decisions for later review.

## Pain Points

**Context Rot and Compaction.** The specific failure mode where a long-running agent begins to "forget" earlier parts of a session or conflates instructions. This is the "flashbang" of white-screen complexity that makes users "give up" on complex agents.

**The Review Bottleneck.** Reviewing a massive output (like a 50-page contract or a 100-file PR) is "extremely painful." If the agent produces work faster than a human can verify it, the productivity gain is neutralized.

**Low-Bandwidth Collaboration.** Using a chat box to fix "Clause 3" of a contract is inefficient. You have to tell the agent what to do, it has to rewrite the whole thing, and you have to re-verify everything. This "linearizing" of a non-linear document is a major friction point.

**Planning Blindness.** The human cannot plan for what they haven't seen yet. If an agent is reviewing 100 contracts, it might encounter a "special clause" that wasn't in the initial plan. The planning-heavy approach fails here because it lacks "contingency awareness."

## Architecture & Technical Patterns

**Durable UI Artifacts.** Instead of a chat history, the interface is a persistent document or a tabular view. These are "high-bandwidth" primitives where humans and agents can "co-habit." A user can highlight a specific cell in a table or a clause in a document and "tag" an agent to it.

**Decision Logs & Unblocking.** To keep agents moving, they are instructed to: "If unsure, make a decision, unblock yourself, but write it to a decision log." The human then reviews the log asynchronously. This is an "optimistic execution" pattern for agents.

**Golden Source Proxies.** For tasks that are hard to verify (like legal strategy), the architecture uses a "Golden Source" (a set of historically successful documents) as a benchmark. The agent calculates a similarity score or "diff" against the golden set to verify its own work.

**Encoded Skills (Judgment Nodes).** Rather than a global prompt, judgment is encoded into specific nodes of the work tree. For example, a "Confidentiality Review Skill" is a modular piece of logic that handles that specific sub-task with specialized context, ensuring consistency across different documents.

**Tabular Review Primitives.** For batch processing (like reviewing 100 employment contracts), the agent spins up a spreadsheet-like view where it "flags" specific items for human take. This allows the human to process hundreds of agent decisions in minutes.

## Hidden Signals

**"Fastest in history" / Hiring in London.** This is a high-confidence signal. Legora is positioning itself as a leader in the Vertical AI space, using the talk to attract elite talent who are tired of building generic "chat wrappers."

**"Agents are not humans."** This is a critical philosophical signal. While many speakers (like O'Leary) use human metaphors (junior developers) to set expectations, Lauritzen is moving toward a post-human interface. If agents aren't human, why use human language as the API? He’s signaling a move toward "Machine-to-Human" UI protocols that look more like dashboards and less like conversations.

**"Planning vs. Skills."** By saying planning will disappear, he is implicitly critiquing the current trend of "Plan-Implement" loops. He’s betting on a more "streaming" and "reactive" model of intelligence where judgment is distributed throughout the execution, not front-loaded.

**The "Golden Contract" as a Linting tool.** He describes checking definitions in a contract as "essentially linting." This treats legal text as code, implying that the future of vertical professional work is the "Code-ification" of every industry.

## What's NOT Said

**The "Hallucination" Gap in Verification.** He talks about verification proxies, but doesn't address what happens if the *verification agent* itself hallucinates. Who verifies the verifier?

**Integration with Legacy Systems.** Law firms are notoriously slow to adopt new software. There's no mention of how these "Durable UI Artifacts" integrate with Word, Outlook, or existing document management systems.

**Data Privacy.** In the legal vertical, data sovereignty is paramount. He avoids the "boring" but critical discussion of how these swarms of agents handle PII and client confidentiality at the architectural level.

## Speaker's Worldview

Jacob Lauritzen is a **Productivity Structuralist**. He believes that the shape of our tools (Chat) is currently strangling the potential of our intelligence (Agents). His worldview is that "Vertical AI" is not just about domain-specific data, but about domain-specific **User Experience**. He sees a future where software is a shared workspace between humans and machines, where "Durable Artifacts" are the source of truth, and language is just one of many ways to interact with it.

## Key Quotes

1.  **"Doing the actual work is extremely cheap... but now you have to spend time planning and you have to spend a lot of time reviewing the work."** — The core economic shift of the agentic era.

2.  **"Chat is one-dimensional... It tries to collapse this work tree into a single sort of linear thing."** — The technical diagnosis of why current AI tools feel "messy."

3.  **"I don't think planning is going to stay around. [...] You basically have to do all the work to just know what to do."** — A contrarian take on the most popular agentic pattern of 2024.

4.  **"Agents aren't humans. [...] We should not constrain them to human language."** — The manifesto for the next generation of AI interfaces.
