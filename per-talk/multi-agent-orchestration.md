# Deep Analysis: From Chaos to Choreography: Multi-Agent Orchestration Patterns That Actually Work
**Speaker:** Sandipan Bhaumik | **Company:** Databricks (ex-AWS) | **Date:** 2026-04-08

## Core Beliefs

**Multi-agent AI is a distributed systems problem wearing an AI costume.** Bhaumik's central thesis is that the moment you go from one agent to many, you cease doing AI engineering and start doing distributed systems engineering. He believes this so deeply that the actual LLM — the model, the prompts, the retrieval — is barely discussed. The entire talk is about state, coordination, failure, and contracts. He's not wrong, but the framing reveals more about his background (18 years in data infrastructure, AWS, Databricks) than about the full picture of multi-agent challenges.

**Complexity grows exponentially, not linearly.** He explicitly states that going from one agent to five doesn't make things 5x harder — it makes them 25x harder. He uses the combinatorial argument (N agents = N(N-1)/2 connections), which frames multi-agent fundamentally as a coordination problem. This is a distributed systems orthodox view, borrowed directly from service-oriented architecture and microservices literature.

**Orchestration beats choreography for production.** Despite presenting both patterns neutrally, his weighting is clear: he gives orchestration the "hero" treatment with the full production architecture walkthrough, while choreography gets a cautionary "this will destroy you" framing. Financial services — his primary regulated-industry reference — uses orchestration "almost exclusively." The subtext: if you're building real systems, you should too.

**Immutable state is non-negotiable.** He is emphatic that shared mutable state is the root cause of multi-agent bugs. His prescription — append-only state versioning, frozen Python dataclasses, schema validation at every handoff — is essentially event sourcing applied to agent workflows. This is a strong architectural conviction, not a suggestion.

**Circuit breakers are "the single most important failure recovery pattern."** This is an unusually strong claim. He positions circuit breakers not as optional resilience but as mandatory infrastructure for every agent call. This signals that in his production experience, agent failures are frequent and catastrophic enough to warrant this level of defensive engineering.

## Pain Points

**Race conditions in shared state — the war story that anchors the entire talk.** The credit decisioning system is the emotional and technical centerpiece. A caching layer between agents and PostgreSQL returned stale credit scores to the risk assessment agent. 20% of decisions were wrong. It took 2 days to diagnose. Customers who should have been flagged were getting approved. He returns to this story repeatedly because it encapsulates his core argument: the model worked, the prompts worked, the architecture failed. The specific failure mode — cache invalidation, not database concurrency — is telling. It's an infrastructure problem, not an AI problem.

**The demo-to-production chasm.** He describes the pattern with precision: one agent demos beautifully, leadership loves it, then product asks for five more agents and everything collapses. This is clearly a pattern he's witnessed repeatedly. The phrase "brilliant engineers make the same mistakes over and over" suggests this isn't a junior engineer problem — it's a paradigm problem. People are approaching multi-agent with the wrong mental model.

**Debugging choreography is a nightmare.** He devotes significant energy to this: "When something fails, you're playing detective with no real clue." The specific failure modes — which agent failed to publish, whether events were consumed, whether they were consumed twice — are classic distributed systems debugging headaches. His warning that teams choose choreography "because it feels more agentic" and then "spend months firefighting" reveals this is a live, recurring pain point.

**Teams ship race conditions to production.** The shared mutable state problem is presented as something "most people do first, and it's wrong." He explicitly calls out that teams assume databases handle concurrency automatically but don't configure isolation levels, explicit locks, or serializable transactions. "We did it. We did that mistake." The personal admission makes it clear this isn't theoretical — it's a mistake he shipped.

**Lost updates through concurrent writes.** The specific pattern — two agents reading the same value (680), both writing different updates (750 and 720), last-write-wins — is presented as the canonical failure mode. This is a textbook concurrency bug, but the point is that in AI systems, the consequences are amplified because the outputs drive automated decisions.

## Architecture & Technical Patterns

**Choreography vs. Orchestration decision framework.** He presents a 2x2 matrix: workflow complexity (simple → complex) vs. autonomy requirements (low → high). Choreography for simple/high-autonomy. Orchestration for complex/low-autonomy. Hybrid (choreography + saga) for complex/high-autonomy. The framework is pragmatic but reveals his bias: the "interesting quadrant" (complex + autonomous) gets the least airtime, and the entire production architecture is orchestration-only.

**Immutable state snapshots with versioning.** The core state management pattern: agents produce sealed, immutable state versions stored in an append-only log (implemented as Delta Lake rows). Each handoff validates schema, increments version, creates a new frozen object. No agent ever modifies existing state — it only appends. This is event sourcing / CQRS thinking applied to agent orchestration.

**Data contracts between agents.** Every agent declares input/output schemas. Handoff boundaries include contract validation — for example, rejecting research output with confidence below 0.7. On Databricks, these schemas are registered in Unity Catalog for centralized governance and versioning. This is essentially type-safe agent communication, treating each agent as a microservice with a published API contract.

**Circuit breaker pattern.** Wraps every agent call: closed (normal) → open (fail fast after threshold failures) → half-open (test with one request after timeout) → closed/open. He positions this as mandatory, not optional. On Databricks, enforced at the Model Serving / AI Gateway layer.

**Saga/Compensation pattern.** Every agent implements `execute()` and `compensate()`. The orchestrator tracks successful executions and walks backward through the list on failure, calling compensate in reverse order. This is classic distributed transaction compensation from the saga pattern literature, directly ported to agent workflows.

**Production stack (Databricks-specific):**
- **Orchestrator:** LangGraph wired into Mosaic AI Agent Framework
- **Agent registration:** Unity Catalog functions (SQL/Python) or registered models
- **Serving:** Databricks Model Serving with AI Gateway (circuit breakers, retries, rate limits)
- **State storage:** Delta Lake (immutable, versioned, append-only)
- **Tracing:** MLflow (per-agent traces, latency, inputs/outputs, token usage)
- **Governance:** Unity Catalog (access control, lineage, audit trail)
- **Higher-level abstraction:** Agent Bricks (packaged orchestration patterns)

## Hidden Signals

**This is a Databricks product pitch architected as an engineering talk.** The talk structure follows a classic enterprise sales pattern: establish the problem with a visceral war story, introduce frameworks that make you feel the pain, then reveal the solution that maps precisely to the vendor's product capabilities. Every pattern — state management, circuit breakers, governance, tracing — has a direct Databricks product mapping. The slide showing the Databricks production architecture is the actual payload of the talk. This doesn't make the content wrong, but it shapes what's emphasized and what's omitted.

**The "Agent Bricks" signal.** Mentioned twice, almost in passing: "tools like Agent Bricks are starting to package these orchestration patterns for common multi-agent use cases, so you don't need to rebuild them every time." This is significant — it suggests Databricks is moving up the abstraction ladder from infrastructure to pre-built multi-agent workflow templates. The word "starting" implies early-stage, which means this is a strategic bet, not a mature product.

**Regulated industries are the real market.** Financial services and healthcare are mentioned as his primary domains. The emphasis on audit trails, rollback, compensation, and governance maps directly to compliance requirements. The multi-agent market that matters most to Databricks isn't startups building creative tools — it's enterprises in regulated industries building automated decisioning systems where wrong outputs have legal consequences.

**"Agents are dumb."** This is a loaded architectural choice. By positioning agents as stateless, input-output workers with no autonomy, he's making an explicit design decision that AGI-adjacent behaviors (self-correction, negotiation, emergent coordination) are not part of the production multi-agent story. The orchestrator is the brain; agents are compute nodes. This is the anti-agentic position within the agentic AI movement.

**The AWS-to-Databricks career arc.** His background is infrastructure, not AI. He spent years building distributed data systems at AWS and now applies that same thinking at Databricks. This explains why the talk contains zero discussion of model capabilities, prompt engineering, retrieval quality, or any AI-native concerns. The mental model is: "you've already solved the AI part — now solve the plumbing." Whether that assumption is valid is the talk's biggest unexamined question.

**The "billions of transactions" claim.** Near the end: "This runs 24/7 across billions of transactions." This is either hyperbole or a signal that these patterns are already deployed at massive scale within Databricks' customer base. If true, it means multi-agent orchestration is already a mature production pattern in enterprise, not the emerging frontier the talk's title suggests.

## What's NOT Said

**No discussion of when NOT to use multi-agent.** The entire talk assumes you're already committed to multi-agent. There's no mention of the common failure mode where a single well-prompted agent with good tools outperforms a multi-agent system with coordination overhead. This is a critical omission — many multi-agent systems are over-engineered solutions to problems that a single agent with function calling could handle.

**Zero mention of model quality or prompt engineering.** In Bhaumik's worldview, the LLM is a black box that either works or fails. There's no discussion of model selection, prompt optimization, fine-tuning, RAG quality, or hallucination management. These are treated as solved problems upstream. This is a massive assumption — in practice, many multi-agent failures are caused by poor model outputs, not poor orchestration.

**No cost or latency analysis.** Orchestrated multi-agent systems with circuit breakers, state versioning, schema validation, and compensation logic add significant latency and compute cost per transaction. No mention of token costs, latency budgets, or throughput tradeoffs. In production, these are often the constraints that kill architectures that are technically correct.

**No mention of agent communication protocols.** No discussion of MCP (Model Context Protocol), A2A (Agent-to-Agent), or any emerging standard for inter-agent communication. The architecture assumes all communication flows through the orchestrator via proprietary function calls. This is a vendor-aligned position — standardized protocols would reduce the need for Databricks-specific orchestration tooling.

**No human-in-the-loop patterns.** Despite focusing on financial services and healthcare — domains where human oversight is often legally required — there's no discussion of when to escalate to a human, how to handle human approval steps in agent workflows, or how to manage the human-agent handoff. This is a significant gap for regulated industries.

**No testing or evaluation strategy.** No mention of how to test multi-agent systems, how to evaluate agent outputs in production, or how to validate that orchestration patterns work correctly. MLflow tracing is mentioned for observability, but testing (unit, integration, end-to-end) is completely absent.

**No security discussion beyond governance.** Unity Catalog provides access control, but there's no discussion of prompt injection across agent boundaries, adversarial inputs propagating through the pipeline, or secure state management. In a system where agents pass data to each other through shared state, attack surface multiplies.

**No open-source or multi-cloud alternatives.** Every technical solution maps to a Databricks product. LangGraph is the only non-Databricks tool mentioned, and it's positioned as something "wired into" the Databricks framework. No mention of Temporal, Airflow, Kubernetes-based orchestration, or any alternative that might compete with the Databricks stack.

## Speaker's Worldview

Bhaumik views multi-agent AI through the lens of a career infrastructure engineer: the AI is the easy part, the plumbing is the hard part, and most teams fail because they don't understand distributed systems fundamentals. His philosophy is that agents should be dumb, stateless workers orchestrated by a smart central coordinator, with immutable state, strict contracts, and defensive failure handling at every boundary. He is building for a world where AI reliability comes not from better models but from better infrastructure — a worldview that maps precisely to Databricks' commercial interests.

## Key Quotes

1. **"They think adding more agents is just like adding more features. It's not. It's building a distributed system."** — The thesis in one sentence. Reveals that the fundamental mistake is cognitive, not technical.

2. **"The problem was not with the model. The problem wasn't with the prompts. The problem was we built a distributed system without distributed system thinking. And that's what kills multi-agent projects, not bad AI, but bad architecture."** — The most direct statement of his worldview. Note the word "kills" — this isn't about suboptimal performance, it's about project failure.

3. **"Agents are dumb. They just take the input, they do the work, they return the output. The orchestrator does all the smart coordination."** — A deliberately provocative architectural choice. In a field obsessed with agent autonomy, Bhaumik is arguing for the opposite: agents as stateless compute nodes.

4. **"I have seen teams choose choreography because it feels more agentic, more autonomous. Then they spend months firefighting because they can't debug distributed event flows. Don't make that mistake."** — The subtext: the AI community's aesthetic preference for autonomous agents is actively harmful in production. Pragmatism over ideology.

5. **"Demos are easy. You use an LLM to show something cool. Everyone can do it. These things don't work in production. In production, you have to build systems, and systems are hard."** — The closing shot. Positions the entire AI demo culture as unserious and separates "people who build demos" from "people who build systems."
