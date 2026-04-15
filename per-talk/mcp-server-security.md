# Deep Analysis: Your Insecure MCP Server Won't Survive Production
**Speaker:** Tun Shwe & Jeremy Frenay | **Company:** Lenses | **Date:** 2026-04-08

## Core Beliefs

**Design IS security.** The central thesis is not that OAuth will save you — it's that bad MCP design is inherently insecure design. Tun states explicitly: "A badly designed MCP server is also a badly secured one. Poor design and poor security compound each other." They believe the industry is approaching MCP backwards: bolting auth onto poorly designed servers rather than designing for security from the first tool definition.

**Agents are fundamentally different consumers than humans.** They adopt and extend Jeremy Lowin's framework of three dimensions of difference (discovery, iteration, context), but add a critical layer: each dimension "casts a security shadow." This is a worldview where every agent capability creates a corresponding vulnerability. They believe agents will use anything you give them "with confidence" — meaning agents don't have the skepticism or judgment humans apply when encountering unexpected behavior.

**Production MCP is inevitable and non-optional.** They treat the transition from local stdio to remote HTTP as a certainty, not a choice. Their framing of the "security cliff" implies they believe any serious MCP deployment will need to cross it, and the current local-only pattern is a temporary stage, not a stable equilibrium.

**The ecosystem is moving toward OAuth 2.1 with short-lived scoped tokens.** They explicitly dismiss API-key-based approaches as the present majority (>50% of MCP servers) but frame them as obsolete: "What we see the ecosystem moving towards is short-lived, scoped tokens via OAuth 2.1."

**Skepticism about current state:** They are deeply skeptical of the current MCP ecosystem's security posture. Standard IO fails under minimal load (20 of 22 requests failed at 20 concurrent connections). API keys are shared, long-lived, unscoped, and create confused deputy vulnerabilities. DCR (Dynamic Client Registration) is acknowledged as necessary but vulnerable to phishing. They trust the protocol specs but not the implementations.

## Pain Points

**The "Security Cliff" is their most vivid problem.** The transition from local to remote MCP isn't gradual — it's a cliff. You go from zero security concerns to needing "OAuth, token management, CORS configuration, TLS, rate limiting and more — all at once." There's no halfway house. This isn't just an engineering challenge; it's an organizational capability gap. Teams comfortable with local MCP suddenly need enterprise-grade identity infrastructure.

**Scale failure of stdio transport.** They cite Stacklok's benchmark: 20 of 22 requests failed with just 20 simultaneous connections. This is devastating and not widely discussed. It means local MCP literally cannot survive any meaningful multi-agent or multi-user scenario.

**Tool poisoning as an unsolved threat.** This comes up repeatedly — it's OWASP MCP #3. The core problem: tool descriptions are a writable surface that models execute without question. "Attackers can embed hidden instructions inside descriptions that are invisible in the UI, but the model will follow them without question." What makes it worse is that neighboring MCP servers can shadow your legitimate descriptions if your documentation isn't complete and unambiguous.

**Token cost of discovery.** Every agent connection enumerates every tool and reads every description — every single time. This isn't just a security problem; it's a cost and latency problem that compounds at scale.

**The OAuth implementation complexity for MCP is staggering.** Jeremy spends significant time on this. "More than 10 specifications to implement" just for the core flow, client discovery, metadata, and token lifecycle. The MCP spec mandates PKCE. You need DCR or CIMD. You need token exchange (RFC 8693). This is months of specialized work, not a weekend project.

**Data leakage through conversation history.** When agents retry, they send the full conversation history over the wire — including sensitive data from previous tool calls. "Each round trip is a chance for data leakage." This is a systemic problem baked into how agents work.

## Architecture & Technical Patterns

**Coarse-grained outcome-based tool design.** Instead of exposing fine-grained CRUD operations, they advocate squashing everything into single tool calls that produce a desired outcome. "One permission check, one audit log entry, one place to enforce authorization." This is the opposite of the REST-ification pattern many MCP servers follow.

**Schema-level input constraining with Pydantic.** Accept top-level primitives and enums. No free-form nested payloads. The root cause of injection flaws is "almost always unconstrained string arguments that get passed downstream to a shell, a query engine, or an API."

**Documentation as a defensive layer.** This is a novel pattern: complete, unambiguous tool descriptions as a security measure. The mechanism is that thorough documentation "crowds out the space that a poisoned neighboring server would try to fill." Your documentation quality directly determines your resistance to tool poisoning attacks.

**Token exchange pattern (RFC 8693).** The MCP server acts as an OAuth client itself, exchanging the delegation token from the user for a session token scoped to the upstream API. This is the "confused deputy" defense — the MCP server never passes through the user's raw credential.

**CIMD over DCR.** Client ID Metadata Document is the preferred approach since November 2025. Instead of a growing database of client registrations, the client owner exposes metadata at a public URL. The authorization server fetches it during authorization. This makes identity verification meaningful — "proving that you control https://cursor.ai is meaningful, unlike proving that you can post on the registration endpoint."

**MCP read-only annotations and resource conversion.** Non-destructive tools should use the MCP read-only annotation, or better yet, be converted to MCP resources entirely. This is a concrete pattern for minimizing blast radius.

**Data masking at the server level.** PII fields (email, phone, national insurance numbers) should be masked before the agent ever sees them. "Agents should never be exposed to data that they have no business handling."

## Hidden Signals

**Lenses is positioning MCP as the enterprise gateway for agentic AI.** The talk opens with Lenses as "the de facto streaming data layer for providing trusted real-time context to agentic AI" with "governance, security and large scale at the top of mind." The MCP server isn't a side project — it's the interface through which Lenses becomes indispensable to the agent ecosystem. They're betting that companies will need a governed, secure layer between their data and agents, and MCP is the protocol that makes that happen.

**OWASP MCP Top 10 is treated as established authority.** They reference it multiple times as though the audience should already know it. This signals that MCP security is already a formalized discipline with its own threat model, not something nascent.

**"More than 50% of MCP servers" use insecure API key patterns.** This is a staggering statistic dropped almost casually. It means the vast majority of the current MCP ecosystem is not production-ready by their standards, and the knowledge gap is enormous.

**The "compliance with regulations such as the EU AI Act" mention.** This is the one regulatory reference. It's strategic — they're linking MCP governance to emerging legal requirements. The implication: if you can't trace what an agent did end-to-end, you can't comply with regulation. This is a forcing function for enterprise adoption of secure MCP.

**"Tracing for agent AI follows the same principles as distributed system observability, but applied to autonomous decision-making."** This frames agent observability as an evolution of existing DevOps practice — making it feel tractable to enterprise buyers rather than a novel unsolved problem.

**CIMD preferred "since November 2025."** This is a very specific date, signaling that the MCP auth ecosystem is evolving rapidly and that what was best practice even a few months ago (DCR) is now considered insufficient.

## What's NOT Said

**No mention of prompt injection defense beyond tool poisoning.** They address tool poisoning via documentation quality and input constraining, but never discuss the broader prompt injection problem — how to defend against adversarial inputs in the data the agent processes, not just in tool descriptions. This is the harder, unsolved version of the problem.

**No discussion of multi-agent security.** The talk focuses on single-client-to-server interactions. What happens when agents talk to each other through MCP? When one agent's output becomes another's tool input? The attack surface multiplies.

**No cost analysis.** Token costs from repeated discovery are mentioned as expensive, but there's no framework for thinking about the economics of secure MCP. OAuth flows, token validation, CIMD fetching — all add latency and compute. What's the cost of security?

**No failure modes of the OAuth flow itself.** They describe the ideal OAuth path but never discuss what happens when the authorization server is down, when tokens expire mid-conversation, or when consent is denied mid-workflow. Production security includes graceful degradation.

**No mention of MCP server-to-server communication.** The architecture is always client-server. What about MCP servers that need to call other MCP servers? The security model for chained MCP calls is unaddressed.

**No discussion of testing security.** How do you test your MCP server for the vulnerabilities they describe? No mention of fuzzing, penetration testing, automated security scanning, or even manual testing strategies for tool poisoning.

**No mention of the human-in-the-loop question.** They never address when and whether a human should approve agent actions. The assumption is that the security model should be robust enough to operate autonomously, which is a strong philosophical position left unstated.

## Speaker's Worldview

Tun Shwe and Jeremy Frenay see AI agents as powerful but fundamentally naive consumers that will faithfully execute whatever they discover, making the design of their interfaces a security-critical discipline. They believe the industry is in a dangerous transitional phase where MCP has escaped the lab but hasn't yet acquired the enterprise-grade identity and governance infrastructure that production demands. Their philosophy is fundamentally defensive and architectural: the way you prevent agent security disasters is not by building better agents but by building narrower, more constrained interfaces that make exploitation structurally difficult rather than merely policy-difficult.

## Key Quotes

1. *"A badly designed MCP server is also a badly secured one. Poor design and poor security compound each other."* — The thesis statement. Security isn't a layer; it's a property of good design.

2. *"There's no halfway house because you can't do a little bit of production. You're either behind the wall or you're standing out in the open."* — Captures the brutal reality of the security cliff and why incremental approaches fail.

3. *"If you don't write clear, complete instructions, an attacker-controlled tool description in a neighboring MCP server can shadow yours."* — Reframes documentation quality as a security property, not a UX concern.

4. *"Standard IO falls over the moment you add concurrency. So, if you want to scale out, you have to cross the chasm."* — Destroys the assumption that local MCP is production-viable at any scale.

5. *"Proving that you control https://cursor.ai is meaningful, unlike proving that you can post on the registration endpoint."* — A sharp articulation of why CIMD represents genuine identity assurance while DCR is security theater.
