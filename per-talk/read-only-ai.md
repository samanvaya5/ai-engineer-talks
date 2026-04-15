# Deep Analysis: Cognitive Exhaust Fumes, or: Read-Only AI Is Underrated
**Speaker:** Šimon Podhajský | **Company:** Independent (personal project — "Fulan") | **Date:** 2026-04-08

## Core Beliefs

**Read-only AI is not a prototype for agents — it's a separate product category.** The speaker's central thesis is that the entire personal AI industry has made a category error by treating observation-only systems as a lesser form of agents. He believes observers and agents serve fundamentally different needs: agents optimize for speed (saving 30 seconds on a weather check), while observers optimize for insight (revealing you've been avoiding your most important project for two weeks). The industry framing of read-only as something you "graduate from" is, in his view, simply wrong.

**The cross-source signal is the actual product.** No single data source — email, browser history, task manager, journal, CRM — contains enough signal to be useful for self-reflection. The value emerges exclusively from correlation across sources. He uses the metaphor of "cognitive exhaust fumes": individual digital activities are waste, but analyzed in aggregate, they diagnose the "engine" (your cognition). This is a data-fusion thesis, not an AI-capability thesis.

**Agency should be reclaimed, not delegated.** He explicitly argues that the AI shouldn't write your drafts, schedule your meetings, or act on your behalf. The feedback loop between observation and action must be mediated by *you*. He acknowledges this is "a hard sell for this crowd" — a direct signal he knows the AI Engineer audience is ideologically committed to the agent paradigm.

**Risk asymmetry is the architectural principle.** A read-only error has zero downside (you ignore a bad suggestion). A write error is unbounded (a misfired email can nuke a relationship). This isn't just a safety argument — it's the foundational design constraint that shapes every technical decision in the system.

**He is skeptical of his own system's security.** Unusually for a conference talk, he devotes significant time to explaining why his architecture *doesn't* fully solve the security problem. He references Simon Willison's "lethal triquetra" and admits shell access still provides an exfiltration channel. His position is not "this is safe" but "I've examined where it isn't safe and made conscious choices about which risks to carry."

## Pain Points

**Token consumption is severe.** Cross-source queries against multiple databases eat enormous chunks of the context window. He explicitly warns against running these queries in non-fresh sessions and treats Claude's 1M token context window as a consumable resource that gets burned through quickly. The Clay MCP alone "takes forever to run." This is the primary operational bottleneck — the architecture works but is expensive and slow at inference time.

**Security remains fundamentally unsolved.** The mosaic effect means the very cross-referencing that makes the system valuable also makes it a devastating target. The lethal triquetra is only partially addressed — read-only removes natural exfiltration channels, but shell access remains an open vector. He's sending deeply personal data to Anthropic over a network that is "mostly open" with "a lot more information lying around than strictly required." He's honest that the system isn't fireproof.

**The CRM integration is the weakest link.** He describes the Clay MCP as the weakest part of the system — slow and presumably brittle. The cross-source magic depends on every source being queryable, and the relationship database is the shakiest pillar.

**Data minimization is an aspiration, not a reality.** He acknowledges that more data is being sent to the API than is strictly necessary for the query. This is a privacy concern that he's aware of but hasn't engineered around yet.

## Architecture & Technical Patterns

**Three-zone architecture: Sources → Workspace → Outputs.** This is the core design pattern. Sources are strictly read-only (six of them). The workspace is where Claude processes data via Python scripts. Outputs land in a separate Obsidian vault — physically and logically isolated from sources. The separation is intentional: outputs never flow back into sources, preventing contamination.

**Claude Skills/Slash Commands as orchestration layer.** The system runs inside Claude Desktop using custom slash commands that trigger Python scripts. These scripts pull data from the read-only sources, feed it to the Anthropic API, and produce structured Markdown outputs. He uses Cursor for reviewing the generated Markdown.

**Data sources identified:**
- Vivaldi browser history (SQLite database — direct filesystem access)
- Email (unspecified provider)
- Task manager (unspecified)
- Journal (unspecified)
- Clay CRM (via MCP — for relationship/contact data)
- One unspecified sixth source

**Cross-origin query pattern.** The most interesting technical pattern is the cross-source query: asking a natural language question that requires correlating data across multiple databases. The system activates a specific Claude skill that sequentially queries Vivaldi's SQLite, the Clay MCP, and other sources, then synthesizes. This is essentially a personal data lake with LLM-powered joins.

**Structured output via prompts.** He mentions "specified prompts" that produce structured outputs from the API. The weekly reflection follows a David Allen / GTD-style framework with sections for themes, tensions, commitments, relationships, notable moments, and reflection questions.

**Auto-accept mode for shell operations.** He mentions running Claude Code with "auto to auto mode" or "dangerous disk permissions" to avoid manual approval of bash operations. This is a practical convenience but also a security tradeoff he doesn't dwell on.

## Hidden Signals

**"Occasionally brutal reflection."** This phrase reveals the speaker values intellectual honesty over comfort. The system is designed to show you unpleasant truths — that your commitment tracking is "mostly notable by its omissions," that you're avoiding things. The product philosophy is closer to therapy than productivity.

**The system name is "Fulan."** Mentioned once in passing during the architecture walkthrough. The name choice (which sounds like it could derive from "fulano" — Portuguese/Spanish for "so-and-so" / an anonymous person) may reflect the system's role as an impersonal observer.

**He's not trying to productize this.** Despite presenting at AI Engineer, there's no pricing model, no onboarding flow, no discussion of scaling to other users. Every example is deeply personal. This is a personal tool that he's sharing as a provocation, not a product pitch. The fact that he presents it this way at a conference full of founders and product builders is itself a statement.

**The "hard sell for this crowd" aside.** He knows his audience is ideologically committed to agents and is deliberately pushing against that consensus. The talk is structured as a counter-argument to the room's default assumptions — a strategic choice that suggests he sees the agent-first orthodoxy as a genuine blind spot in the industry.

**The relationship decay use case.** Detecting that you haven't contacted people you care about is the most emotionally resonant application he describes. It's also the highest-stakes — these are the exact relationships a write-misfire would damage. The read-only constraint isn't just technical here; it's preserving the very thing the system monitors.

**"The observer changes your behavior, too."** He briefly acknowledges the Hawthorne effect — that being observed changes behavior — but argues it's acceptable because the feedback loop is "mediated by you, not automated." This is a nuanced philosophical position he doesn't fully explore but clearly has thought about.

## What's NOT Said

**No mention of local/on-device AI.** Everything goes to Anthropic's API. For a talk so concerned with data privacy and personal data exposure, the absence of any discussion about local models (Llama, Mistral, etc.) or on-device processing is conspicuous. He's accepted cloud dependency as a given.

**No cost analysis.** He doesn't mention how much this costs in API tokens per week. Given his explicit acknowledgment of heavy token usage, the economics are presumably non-trivial but deliberately not discussed.

**No multi-modal inputs.** No mention of voice, images, screenshots, or location data as sources. For a "cognitive exhaust" thesis, these are major omissions — much of cognition's digital footprint is visual and auditory.

**No real-time processing.** Everything appears to be batch (weekly reflections, on-demand queries). No discussion of streaming analysis, real-time alerts, or proactive nudges. This aligns with the read-only philosophy but also limits the system's responsiveness.

**No mention of how behavior actually changed.** He describes what the system reveals but never says whether acting on those revelations has measurably changed his life. The talk is about the tool's capabilities, not its outcomes.

**No data retention or deletion policy.** For someone paranoid about the mosaic effect and data exfiltration, there's no discussion of how long data is retained, whether it's rotated, or how it's deleted.

**No discussion of model switching or provider independence.** The system is tightly coupled to Claude/Anthropic. There's no mention of what happens if Anthropic changes pricing, terms of service, or model capabilities.

## Speaker's Worldview

Šimon Podhajský is a philosophical pragmatist who believes AI's highest-value application for individuals is not automation but *self-knowledge*. He sees the agent paradigm as a category error when applied to personal life — the risks of action are asymmetric, and delegation erodes the very agency that makes life meaningful. His engineering instincts are conservative in the best sense: he designs around what he can verify, acknowledges what he can't secure, and treats the absence of examination as the only unforgivable security posture. He is building for himself, not for a market, and presenting his work as an intellectual provocation to an industry he believes is collectively making a bet on the wrong abstraction.

## Key Quotes

1. **"The moment your AI writes to your data sources, the exhaust fumes are contaminated. You're no longer observing your cognition. You're observing a human-AI hybrid, and you can't tell which patterns are yours."** — The core philosophical argument for read-only, revealing his belief that data integrity is prerequisite to useful self-analysis.

2. **"The agent saves me 30 seconds on a weather check. The observer shows me I've been avoiding my most important project for 2 weeks."** — The sharpest framing of the value differential between agents and observers. Value-per-interaction isn't even close.

3. **"The industry frames read-only as a limitation you graduate from. I think that's wrong. Observers and agents are different tools, and a mirror isn't a broken butler."** — The talk's thesis in one sentence, including a memorable analogy that reframes the entire industry's assumption.

4. **"I'm not claiming the system is secure. I'm claiming that I've thought about where it isn't, and I've decided which risks I'm willing to carry. It's different from not knowing. The worst security posture is the one you haven't examined."** — Unusually honest for a conference demo. He's trading the rhetorical advantage of claiming security for intellectual credibility.

5. **"Read-only isn't just safer, it produces better analysis."** — The hidden twist in his argument. Read-only isn't a constraint he tolerates — it actively produces superior outcomes because it preserves the integrity of the signal he's analyzing.
