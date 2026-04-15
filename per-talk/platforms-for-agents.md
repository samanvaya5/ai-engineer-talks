# Deep Analysis: Platforms for Humans and Machines: Engineering for the Age of Agents

**Speaker:** Juan Herreros Elorza | **Company:** Banking Circle | **Date:** 2026-04-08

---

## Core Beliefs

1. **AI agents don't create new best practices — they make ignoring old ones catastrophic.** The central thesis is that everything he recommends (self-service, API-first, documentation, shift-left) was already correct before LLMs. Agents just make the cost of ignoring these practices immediately visible rather than slowly corrosive. This is a deliberately conservative framing — he's not selling a revolution, he's saying "you should have done this already, and now you have no excuse."

2. **Platform engineering is the foundation for AI productivity, not model quality.** He spends zero time discussing which models to use, prompt engineering, or RAG architectures. His implicit belief is that the bottleneck for AI-assisted development is not the intelligence of the model but the quality of the surrounding engineering infrastructure. The agent is only as good as the platform it operates on.

3. **Self-service is non-negotiable.** He distinguishes between "technically self-service" (the components exist) and actually self-service (a single entity can complete a flow end-to-end). If you need to "fetch building blocks from five different places," it fails the test — for humans and especially for agents.

4. **Documentation is infrastructure, not a nice-to-have.** He positions agent.md files, skills, centralized docs, and API-accessible documentation not as knowledge management but as a core part of the platform's architecture. Without structured documentation, the agent literally cannot function.

5. **He is skeptical of graphical observability.** He explicitly calls out that agents "are not going to be looking at those graphical interfaces" — dashboards, UI-based monitoring. His skepticism extends to any human-centric interface pattern that hasn't been re-thought for machine consumption.

## Pain Points

1. **The "deployment wall" — the gap between writing code and running it.** The extended narrative (easily 30%+ of the talk) is about a developer who writes code but cannot deploy it without tribal knowledge, copy-pasting pipelines, begging an infrastructure person, and waiting. This is clearly a lived wound. The specificity of the story ("maybe that deployment will succeed only to realize... you also needed a database") suggests this exact sequence happened repeatedly at Banking Circle.

2. **Tribal knowledge as the silent killer.** The story hinges on "ask someone on the team" → "copy from an existing pipeline" → "talk to the infrastructure person." Every step relies on human-to-human knowledge transfer. He frames this as the fundamental incompatibility between how platforms currently work and how agents need them to work.

3. **The verification gap.** He identifies a subtle but critical problem: even if an agent deploys successfully, how does it *know* it succeeded? Humans check dashboards. Agents can't. This is an underexplored pain point across the industry — the absence of machine-readable success signals.

4. **Documentation discoverability.** "Many of us have written documentation, put it somewhere, and then were unable to find it." This isn't just a complaint — it's the reason he advocates for API-accessible documentation and agent.md files. The documentation exists but is structurally inaccessible.

5. **The double-edged sword of contribution.** He's candid that encouraging contributions is both necessary and dangerous. More AI-assisted contributions means more code flowing into the platform, but the platform team still bears maintenance responsibility. The tension between openness and control is unresolved — he suggests "a combination" of policies and markdown guidance, which reads more as honest uncertainty than a solved problem.

## Architecture & Technical Patterns

1. **Platform "Atlas" — a layered internal developer platform.** Banking Circle's platform is structured as sub-platforms: compute (Kubernetes), infrastructure (blob storage, databases, secrets), messaging, and observability. This is a textbook platform engineering architecture following the Team Topologies model.

2. **API-first with multiple interface layers.** The recommended stack: well-defined API at the bottom, with CLI, MCP server, or other wrappers on top. The API provides schema validation, authentication, authorization, and discoverability. The agent interacts via the API (directly or through wrappers) in a feedback loop. This is a practical blueprint.

3. **agent.md / compiler_instructions.md as a configuration layer.** He treats these files as a genuine part of the platform's interface — not just helpful hints but structured instructions that define how agents should build, test, deploy, and verify within a repository. He envisions a hierarchy: a general agent.md for the organization, with repository-specific overrides.

4. **"Skills" as codified workflows.** He mentions skills (markdown documents that encode conventions and interaction patterns) as a way to tell agents "when you do this type of task, you should do it like that." This is essentially prompt engineering as infrastructure.

5. **Shift-left as an agent-enabler.** Validation should happen locally, before pushing to version control. The reasoning is agent-specific: if the agent can fail fast in its local loop, it can iterate rapidly without waiting for CI pipelines. This reframes shift-left from a quality practice to a velocity practice for agents.

6. **Metrics framework: DORA + SPACE + platform-specific.** He references DORA metrics (deployment frequency, lead time, MTTR), SPACE (developer satisfaction), and custom platform metrics (support request volume) as measurement tools. The notable pattern is measuring before and after platform changes to validate impact.

## Hidden Signals

1. **"Take advantage" of the AI hype.** The most revealing line in the entire talk is the closing advice: use AI as the "excuse to implement some best practices that again were always best practices if you didn't have the chance to do it until now." This is a shrewd political signal — he's telling the audience that AI is a lever for organizational change that platform engineers have been wanting for years. The subtext is that platform engineering teams have been losing internal battles for prioritization, and AI hype is their weapon now.

2. **The implicit admission of technical debt.** The story about the developer is so specific and so painful that it reads like a confession. Banking Circle clearly went through a period of ad-hoc infrastructure, tribal deployment processes, and documentation neglect. Atlas is the cleanup, and agents are the justification for doing it properly.

3. **No mention of agentic workflows or multi-agent systems.** He talks about "a coding agent" — singular, local, working on one developer's machine. He doesn't discuss autonomous agents, multi-agent orchestration, or agents communicating with each other. This reveals his mental model: the agent is an extension of the developer, not an independent actor.

4. **The word "trillion" in the first minute.** He leads with "we process over 1 trillion euro per year" and "over 700 regulated financial institutions." This is not just context — it's establishing credibility and signaling that his recommendations come from a high-stakes, regulated environment. He's implicitly saying: if this works in fintech with compliance overhead, it'll work anywhere.

5. **Uncertainty about the DORA metrics.** He literally forgets one of the four DORA metrics mid-sentence ("and another one that I can't really remember right now" — it's change failure rate). This small moment reveals he's a practitioner speaking from experience, not a framework evangelist. He cares about the practice, not the taxonomy.

## What's NOT Said

1. **No discussion of model selection, prompt engineering, or AI evaluation.** For a talk at an AI Engineer conference, the absence of any model-specific guidance is striking. He never mentions which agents, which models, or which coding tools Banking Circle uses. This is a deliberate choice — he's arguing that the infrastructure layer matters more than the model layer.

2. **No mention of security implications of agent access.** He briefly mentions authentication and authorization, but there's no discussion of the security risk of giving agents broad platform access, no mention of audit trails for agent actions, no discussion of agent-specific permissions vs. developer permissions. In fintech. This is a conspicuous gap.

3. **No failure stories.** The entire talk is prescriptive — here's what to do. He shares no stories of things that went wrong specifically with AI agents. Given that the talk is framed around real experience at Banking Circle, the absence of "we tried X with agents and it broke" stories suggests either that their agent usage is still early-stage or that he's curating a clean narrative.

4. **No mention of cost.** Agents calling APIs in loops, running shift-left validation locally, iterating repeatedly — this has cost implications (compute, API calls, token usage). In a fintech context, the absence of cost discussion is notable.

5. **No discussion of organizational change management.** He mentions executive attention on AI but doesn't address how to restructure teams, change incentives, or manage the cultural shift of treating platforms as agent-first. The assumption is that the technical changes will drive the organizational ones.

6. **No mention of MCP specifically as a strategy.** He references MCP servers in passing ("it could be something like an MCP server around the API") but doesn't treat it as a first-class architectural pattern. Given that MCP was becoming the standard for agent-tool interaction by early 2026, this is either an intentional avoidance of hype or a sign that Banking Circle hasn't adopted it deeply.

## Speaker's Worldview

Juan is a **platform engineering pragmatist** who sees AI agents not as a paradigm shift but as a forcing function for discipline that should have existed all along. His worldview is infrastructure-first: the model is a commodity, the platform is the differentiator. He believes engineering organizations are politically constrained more than technically constrained, and that AI hype is a rare window to push through long-overdue structural improvements. He is not an AI enthusiast — he is a platform engineer who has found a powerful new argument for doing his job properly.

## Key Quotes

1. **"These pain points that the developers were facing suddenly become much more obvious when an agent is facing them. And they are the limiting factor in how productive this coding agent can be."** — The core diagnostic: agents don't create problems, they reveal them.

2. **"If it is technically self-service but it requires fetching some building blocks from five different places and putting them together and then triggering a flow somewhere else then it's not really self-service."** — A sharp definition that most "self-service" platforms fail.

3. **"Agents are not going to be looking at those graphical interfaces. So you also need to think how does observability look like if the prime user is going to be an AI agent."** — A quiet paradigm shift: observability must be redesigned for machine consumers.

4. **"Take advantage. Everyone from the executive level to the individual contributors are looking at AI now. You can use AI as the excuse to implement some best practices that again were always best practices."** — The most strategically revealing line in the talk: AI as political leverage for platform engineering.

5. **"The machine is not going to go and try the pipeline and then go up to the second floor and talk to the person in that other team."** — Deceptively simple, but it captures the entire thesis: every human-dependent workflow is a dead end for agents.
