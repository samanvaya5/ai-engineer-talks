# Deep Analysis: Bending a Public MCP Server Without Breaking It
**Speaker:** Nimrod Hauser | **Company:** Buzz | **Date:** 2026-04-08

## Core Beliefs

- **Third-party agentic tools will never work well out of the box.** Hauser's entire thesis is that raw MCP server tools are "glorified integration code" written by teams who don't know your use case. The default state is failure — his first demo literally shows a hallucinated URL and a 404 screenshot. He believes you *must* actively reshape them.

- **Agents are inherently unreliable and must be constrained.** He repeatedly calls agents "non-deterministic, unpredictable things" and references known failure modes (needle-in-a-haystack, lost-in-the-middle). His philosophy is to treat agentic autonomy as a danger to be mitigated, not a feature to be celebrated.

- **Context engineering is the dominant lever for agent quality.** The talk's framework is essentially a taxonomy of context manipulation: curate tools to shrink context, rewrite descriptions to steer behavior, inject deterministic guardrails to override the agent, compose new tools to create differentiated context, and remove tasks from the agentic flow entirely. He treats context as the engineering surface.

- **Deterministic code beats agentic decision-making for anything that matters.** His guardrails pattern — intercepting tool invocations, validating paths, returning agent-friendly error messages instead of exceptions — reveals a deep skepticism that prompts alone can enforce behavior. The login flow being pulled entirely out of the agentic loop is the clearest expression of this.

- **Skepticism about MCP as plug-and-play.** Despite the talk's title, Hauser is not an MCP evangelist. He never once suggests that MCP's tool discovery or protocol features solve anything meaningful. MCP is just a delivery mechanism for functions. The value is entirely in how you reshape what arrives.

## Pain Points

- **Agent hallucination during navigation.** The baseline demo showed the agent inventing a URL (`buzz.co/spec-reviewer`) that doesn't exist, then screenshotting its own 404 error as "evidence." This is the canonical failure mode he's optimizing against — agents confidently doing the wrong thing with tools they don't understand.

- **Generic tool descriptions leading to poor tool selection.** He spends significant time on Playwright's shallow descriptions ("Press a key on the keyboard," "Close the page"). These descriptions, designed for maximum generality, give agents almost no guidance about *when* or *how* to use each tool in a specific workflow. This is presented as the root cause of most failures.

- **Agents ignoring instructions buried in long contexts.** He explicitly names "needle in a haystack" and "lost in the middle" as phenomena that make prompt-only approaches insufficient. His guardrails pattern exists because he's experienced agents simply ignoring rules stated in descriptions and prompts.

- **Security risks from agents unaware of multi-tenant architecture.** He identifies data leakage between tenants as a real production concern — agents that don't understand your database schema, folder structure, or tenant boundaries can inadvertently expose client data. This is presented as a *certainty*, not a hypothetical.

- **Login/authentication complexity.** He reveals that login is one of the hardest problems in practice — different clients have different mechanisms, secrets management is involved, and agents perform unreliably at it. His solution (pulling it out of the agentic loop entirely) tells you how painful it was to leave it in.

- **The context window trade-off as a constant tension.** He acknowledges that some of his techniques *add* to context (longer descriptions) while others *subtract* (curating tools), and there's no formula — you have to tinker. This is presented as an ongoing engineering challenge, not a solved problem.

## Architecture & Technical Patterns

### The Five-Stage Framework (V0 → V4)

The talk's core artifact is an incremental refactoring pattern, each version inheriting from a base class and overriding `get_tools()`:

1. **V0 — Baseline (vanilla `load_mcp_tools`):** 21 tools from Playwright, zero customization. Result: hallucinated URL, 404 screenshot, complete failure.

2. **V1 — Curation (tool filtering):** Identify tools irrelevant to the use case (drag, code execution, resize) and exclude them via list comprehension. Drops from 21 to 16 tools. Reduces context window load and narrows the agent's decision space.

3. **V2 — Wrapping (description replacement):** Create `ToolWrapper` class that generates new tool objects with custom descriptions while preserving original functionality. Key pattern: a dictionary mapping tool names to enhanced descriptions, with domain-specific guidance (e.g., "always prefer accessibility snapshot over visual snapshot," "call snapshot before click"). The new tool simply invokes the original tool — same function, different description.

4. **V3 — Deterministic Guardrails (path validation):** Intercept tool invocations for sensitive operations (screenshot saving). Extract the `path`/`filename` parameters, validate against an allowed root directory, and block invalid invocations. Critical design choice: return an agent-friendly error message rather than raising an exception, so the agent self-corrects rather than crashing the entire flow.

5. **V4 — Tool Composition (new tools from existing tools):** Inject an entirely new tool ("evidence screenshot") that wraps the existing screenshot tool with differentiated behavior and description. The agent now has two screenshot tools — one general, one for evidence — and the descriptions help it choose appropriately. The evidence tool enforces naming conventions (include ticket number in filename).

### Bonus Pattern: Tools-as-Functions

Login is handled deterministically by calling MCP tools directly (injecting JWT tokens into browser localStorage) *before* the agent starts. This removes an entire category of failure from the agentic loop.

### Emerging Best Practices

- **Inherit-and-override pattern for tool customization:** A base class with `get_tools()` that each version overrides. Clean separation of concerns, easy to A/B test.
- **Agent-facing error messages as alignment mechanism:** Instead of failing the flow, return descriptive errors that nudge the agent back on track. Treats the agent like a developer reading error messages.
- **Accessibility snapshot > visual snapshot for navigation:** A domain-specific insight — Playwright's accessibility snapshot (text-based DOM representation) gives agents better understanding of page structure than visual screenshots. This is a practical finding from production use.
- **Dual-tool pattern for differentiated behavior:** Rather than adding conditional logic to one tool, create two tools with distinct descriptions. Let the LLM's tool-selection mechanism handle the branching.

## Hidden Signals

- **"We've been building AI-powered code reviewers for the past few years now."** Buzz has been at this since 2023. This isn't a company experimenting with AI — they've been shipping agentic products for ~3 years. The framework comes from production scars.

- **"In our real product, pixel-perfect verification is something we we worked very hard on."** Dropped casually at the end. This suggests Buzz has solved visual regression testing to a pixel level using multimodal AI — a significantly harder problem than what the demo shows. The toy example hides how sophisticated the real system is.

- **"Each client might have a different logging mechanism and they can be tricky and they can have secrets."** This reveals that Buzz operates in a multi-tenant, client-integrated environment where they're logging into customer systems. The security concerns aren't theoretical — they're dealing with real client data access.

- **The "lost in the middle" and "needle in a haystack" references.** He names specific LLM failure modes, signaling deep familiarity with the research literature and firsthand experience with these phenomena in production. This is not a surface-level practitioner.

- **"Sometimes they'll be deterministic. Sometimes they'll be flexible. It depends."** This casual line reveals that Buzz likely runs different configurations for different clients/use cases, suggesting a maturing product with configurable agent behavior rather than a one-size-fits-all tool.

- **The entire framing of the talk as debugging a "broken MCP server."** He opens with a visual of a server on fire and closes with "green lights blinking, engines roaring, GPUs churning, millions of spec reviewers being executed." This is a narrative about taming chaos — the implicit message is that production agentic systems *start* broken and require systematic intervention to become reliable.

- **"An agent that gets this message is most likely to just try again, but aligned."** The word "aligned" is doing heavy lifting. He's describing a feedback loop where the agent's own error-correction mechanism is hijacked to enforce your constraints. This is a subtle but powerful pattern — using the agent's propensity to retry as a feature, not a bug.

## What's NOT Said

- **No mention of MCP's protocol-level features.** MCP supports tool discovery, capability negotiation, and server composition. Hauser treats MCP as a function delivery mechanism and never discusses any of these. Either they're not useful in practice, or he's deliberately simplifying. Either way, the omission speaks volumes about where MCP's actual value lies (or doesn't).

- **No discussion of cost or latency optimization.** Every tool in context, every retry from a guardrail rejection, every wrapped tool with longer descriptions — all of these have latency and token cost implications. In a production system processing "millions of spec reviewers," this would be a primary concern. Its absence suggests either (a) Buzz has solved it and it's not interesting to talk about, or (b) the cost picture is uglier than anyone wants to admit.

- **No mention of evaluation or measurement.** He shows a single pass/fail demo run. There's no discussion of how to measure whether V2 is actually better than V1 at scale, no metrics, no A/B testing framework, no regression testing. For a company shipping production agentic systems, this is a conspicuous gap.

- **No discussion of model choice or model switching.** The talk is model-agnostic to a fault. There's no mention of which LLM powers the agent, how model choice affects tool-selection behavior, or whether different models require different tool descriptions. Given how much of the talk is about context engineering for the LLM, ignoring the model itself is odd.

- **No mention of failure modes in the guardrails themselves.** What happens when the agent finds a creative workaround? What if it base64-encodes the path? What if it uses a different tool to write files? The deterministic guardrails are presented as airtight, but anyone with security experience knows they're probably not.

- **No discussion of versioning or maintenance.** When Playwright updates their MCP server and changes tool signatures, descriptions, or adds new tools — what happens to all these customizations? The framework assumes a stable upstream, which is a dangerous assumption in the current ecosystem.

- **No mention of multi-agent architectures.** The talk is entirely about a single agent with tools. There's no discussion of agent-to-agent communication, tool sharing across agents, or how these patterns scale to multi-agent systems — despite this being one of the hottest topics in agentic engineering.

## Speaker's Worldview

Hauser views AI agents as powerful but fundamentally unreliable components that require the same disciplined engineering applied to any integration: validation, wrapping, guardrails, and selective removal of autonomy. His instinct is always to *reduce* the agent's degrees of freedom rather than expand them. He comes from a backend/data engineering tradition (20 years, Salesforce, cyber, crypto) and approaches agents not as a paradigm shift but as a new interface to the same old problem of making third-party code work in your system. The agent is not a colleague — it's a dependency to be managed.

## Key Quotes

1. **"Agents are already non-deterministic, unpredictable things. You give them tools, and you get unpredictability at scale."** — The thesis statement. Tools multiply unreliability rather than reduce it.

2. **"The people at Playwright don't know what our specific use case is. This MCP server will need to cater to I don't know how many different use cases. It has to be generic. But, for us, using this... we might want to put in our own descriptions."** — The core insight: MCP's generality is its weakness in production, not its strength.

3. **"We don't return an exception. We don't want the whole agentic process to fail. Instead, we handle this nicely by creating a very nice agent-facing explanation... An agent that gets this message is most likely to just try again, but aligned."** — A sophisticated pattern for using agent error-correction as an alignment mechanism rather than treating retries as waste.

4. **"Sometimes you might want to take matters into your own hand... We just unburden the agent from this somewhat clunky action it needs to take, which we can just take off its hands, and not bother it and its context with it."** — The philosophical endpoint: the best agentic tool is sometimes no agentic tool at all. Deterministic code wins when the task is certain.

5. **"There's no one-size-fits-all. It's mainly a question of how do I kind of mold the tools to best fit my use case. Sometimes they'll be deterministic. Sometimes they'll be flexible. It depends."** — The honest conclusion: this is still a craft, not a science. No framework replaces domain-specific tinkering.
