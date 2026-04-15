# Deep Analysis: The Future of MCP and Programmatic Tool Calling
**Speaker:** David Soria Parra | **Company:** Anthropic | **Date:** 2026-04-26

## Core Beliefs

**Connectivity is a stack, not a single solution.** Parra explicitly rejects the idea that any one method (Computer Use, MCP, or CLIs) is the "winner." He frames them as complementary tools for different environments: CLIs for local sandboxes, Computer Use for legacy/GUI interaction, and MCP for "rich semantics" and enterprise-grade governance. This is a strategic move to position MCP not as a competitor to existing methods, but as the sophisticated "connective tissue" that handles the "boring but important" logic like authorization and state management.

**Agents need their own interfaces (MCP Applications).** The opening demo of an agent shipping its own UI via MCP is a radical departure from the "chat bubble" paradigm. Parra believes the model shouldn't just generate text or code; it should serve functional, interactive mini-apps. This shifts the agent from being a "worker in a box" to a "service provider" that can project its own control plane into the host application.

**Orchestration should be programmatic, not conversational.** He is critical of the "call tool -> get result -> think -> call next tool" loop. He argues that letting the model orchestrate via inference is slow, expensive, and fragile. The "Future" is the model writing a script (JavaScript/Lua) that executes locally or in a V8 isolate to compose multiple tools in a single turn. This is "Code Mode" generalized beyond just building software—it's using code as a high-density coordination language for business logic.

**Standardization is the only way to avoid the "SDK Tax."** With 110 million downloads, Parra claims MCP has reached a "React-like" milestone of adoption. His belief is that 2026 will be the year of the "General Agent" (knowledge workers), and these agents cannot afford to integrate with 50 different SAS SDKs. They need a single protocol that provides semantics, resources, and tools in a uniform way.

## Pain Points

**REST-to-MCP "Cringe."** Parra is openly dismissive of tools that automatically wrap REST APIs into MCP servers. The pain point here is abstraction mismatch: REST APIs are designed for humans/browsers; agents need higher-level "intent-based" tools. A 1:1 mapping results in "horrible" agent performance because it forces the agent to handle low-level HTTP-style orchestration that it isn't optimized for.

**Context Window Bloat.** He acknowledges that the "experimentation phase" of loading every tool into the context window is failing. This creates "context poisoning" where the model gets confused by irrelevant tools. This is a direct echo of the O'Leary talk, but Parra’s solution is technical (Progressive Discovery) rather than procedural (new sessions).

**Scalability of Streamable HTTP.** A rare "under the hood" admission: the current MCP transport (SSE/Streaming HTTP) is "very hard to scale" for hyperscalers like Anthropic or Google. This reveals a bottleneck in the current protocol that prevents it from being a true "cloud-native" standard without the new "Stateless Transport Protocol" proposal.

**SDK Quality vs. Community Innovation.** He admits the official Anthropic Python SDK is inferior to the community-built `FastMCP`. This signals a humility in the development of the protocol—Anthropic is providing the spec, but the community is currently leading on the ergonomics.

## Architecture & Technical Patterns

**Progressive Discovery (On-Demand Tool Loading).** Instead of flooding the context, the client provides a "Tool Search" tool. The agent uses this to find what it needs, and the host loads those specific tool definitions on the fly. This moves the context usage from $O(N)$ (all tools) to $O(K)$ (only used tools).

**Programmatic Tool Calling (REPL Tool).** The agent is given access to a runtime (V8, Lua, etc.) and uses MCP’s `structured output` to understand return types. It then writes a script to filter, map, and reduce data across multiple MCP tools. This is "Tool Composition at the Edge."

**Stateless Transport Protocol.** A shift away from long-lived streaming connections to a request-response model that can be deployed on Cloud Run or Kubernetes. This is designed to make MCP servers behave like standard REST microservices, simplifying deployment and load balancing.

**Skills over MCP.** A new extension to the protocol that allows server authors to ship "Domain Knowledge" (best practices, playbooks, prompts) along with the tools. This effectively bundles the `agents.md` or `skills` context directly into the server, ensuring the agent knows *how* to use the tools it was just given.

**Server Discovery via well-known URLs.** Borrowing from `robots.txt` or `.well-known/security.txt`, this allows agents to "craw-discover" MCP capabilities on any website. It’s an attempt to turn the entire web into a machine-readable MCP graph.

## Hidden Signals

**"Boring but important enterprise stuff."** Parra uses this phrase to describe authorization and governance. This is a signal that Anthropic’s primary target for MCP is the Fortune 500. They are positioning MCP as the "Secure Gateway" for agents to touch sensitive data, distinguishing it from the less-governed "plugin" ecosystems of competitors.

**The Google/Anthropic Partnership on the Spec.** Mentioning that the new transport protocol comes from "our friends at Google" signals that MCP is effectively becoming a multi-polar industry standard, not just an Anthropic project. This cross-vendor alignment is a defensive move against OpenAI’s proprietary tool-calling ecosystems.

**"Coding agents are the ideal scenario... user is quite happy."** He’s setting a baseline. If you’re only building coding agents, you’ve "solved" the easy problem. The real challenge (and the 2026 goal) is "Knowledge Worker" agents. This implies that coding agents have reached a level of maturity where the "spectacle" of code flying by is no longer the cutting edge.

**"Skills over MCP" as a Registry Replacement.** By shipping skills with the server, they bypass the need for a centralized "Prompt Registry." The server becomes the authority on both its capabilities and its usage patterns.

## What's NOT Said

**Security of the REPL Tool.** While he advocates for the model to write and execute scripts to orchestrate tools, he doesn't detail the sandbox requirements or the risks of "Prompt Injection to RCE" in these V8 isolates.

**The Economic Split.** There is no discussion of how server authors might monetize these MCP apps or skills. It’s presented as a purely technical enablement layer, ignoring the "App Store" economics that usually follow such platforms.

**Compatibility with Non-LLM systems.** The protocol is heavily optimized for LLM semantics. There's no mention of how "classical" software might interact with these servers without a model in the loop.

## Speaker's Worldview

David Soria Parra sees the world through the lens of **Protocol Dominance**. He believes that the "Connectivity Wars" will be won not by the best model, but by the most ubiquitous standard. He views 2024-2025 as the "Wild West" of hacking and 2026 as the era of "Professionalized Connectivity." His focus is on removing the friction between "Intent" (the model) and "Action" ( the system), treating code as the ultimate glue.

## Key Quotes

1. **"Every time I see someone building another REST to MCP server conversion tool, it's a bit cringe... you should design for an agent."** — A mandate for intent-based design over legacy wrapping.

2. **"Connectivity is not one thing... it always means 'it depends' and there's a real connectivity stack."** — Rejecting the "one tool to rule them all" narrative.

3. **"2024 we built a bunch of demos... 2025 was all about coding agents... 2026 is all about putting these agents into production."** — The roadmap for the industry's transition from toy to utility.

4. **"MCP is this additional connective tissue that is just yet another tool in the toolbox for you to build an amazing agent."** — Positioning MCP as the mature choice for complex environments.
