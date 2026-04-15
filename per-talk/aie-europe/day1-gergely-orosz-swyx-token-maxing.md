# Deep Analysis: Token Maxing & Software Engineering + AI
**Speaker:** Gergely Oroz (The Pragmatic Engineer) in conversation with swyx | **Date:** 2026-04-26

## Core Beliefs

**Token counts are the new "Lines of Code"—and just as dangerous.** Orosz identifies a pervasive new metric in big tech (Meta, Microsoft, Salesforce): the token leaderboard. He argues that leadership is using token spend as a proxy for "innovation" and "effort." This belief—that high usage equals high value—is driving a behavioral shift where engineers "token max" (generating unnecessary AI activity) to avoid being in the bottom performance quartile. It’s a classic Goodhart's Law failure: once a measure becomes a target, it ceases to be a good measure.

**The "Lead Code" filter still applies to AI.** Orosz draws a cynical but precise parallel between algorithmic interviewing and token maxing. Both are "hoops" that have little to do with daily productivity but serve to filter for "smart people willing to put up with absolute BS to keep a high-paying job." He believes Big Tech uses these arbitrary metrics not because they are accurate, but because they test compliance and the ability to navigate corporate friction.

**The feedback loop of agents is faster than the feedback loop of management.** He refutes the idea that being an "AI Engineer" is just being an Engineering Manager. EM work is about "people drama" and "six-month results." Agent orchestration is about "tech lead" style mentorship with a "mech suit" speed of execution. The core belief here is that agents provide the *leverage* of a team without the *latency* of human coordination.

**Theory of AI doesn't translate to skill in AI.** This is a profound epistemological claim: "understanding the theory will not make you better at using the tools." He contrasts this with traditional engineering, where knowing the compiler makes you better at low-level code. In the agentic era, "sense-making" and "vibe-checking" the tool's behavior is a distinct, non-theoretical skill that can only be acquired through hundreds of hours of usage.

**"One-pizza teams" are the new structural reality.** He notes that the traditional two-pizza team (8-10 people) is collapsing into one-pizza teams (3-4 people) even at 200-year-old companies like John Deere. This isn't just a cost-cutting measure; it's a realization that AI collapses roles (QA, DevOps, Product) into a single, high-bandwidth "Product Engineer."

## Pain Points

**The "Bottom 25%" anxiety.** Engineers at Meta and Salesforce are genuinely afraid that low token usage will be "weaponized" during layoffs. The pain point isn't that the AI is bad, but that the *surveillance* of AI usage is creating a culture of performative prompting. This leads to "strategic waste"—engineers running autonomous agents to build "junk" just to inflate their metrics.

**The Productivity Paradox (20% Up / 20% Down).** He cites a study where engineers *felt* 20% more productive but were *demonstrably* 20% less productive. This "illusion of competence" is a massive hidden risk. The AI makes the work *feel* easier (less friction), which the human brain interprets as speed, even if the resulting code is lower quality or requires more debugging later.

**The "Manual-less" Era.** He quotes Simon Willison: "there's no manual." The pain point for senior engineers is the loss of predictability. You can't just "read the docs" to understand how an agent will react to a specific codebase. This uncertainty is causing "experienced engineers to hold off," as the tools are often "mildly useful" or actively harmful on legacy systems.

**Retrofitting AI into legacy workflows.** While startups "just build stuff," big tech is struggling to "retrofit" AI into existing processes. The friction between "move fast" AI and "high-reliability" enterprise infra is creating a massive demand for internal platform work that off-the-shelf tools can't solve.

## Architecture & Technical Patterns

**Internal MCP Gateways and Service Discovery.** Orosz reveals that companies like Uber and Meta are building their own "MCP Gateways" integrated into their service discovery layers. This is the enterprise answer to tool-calling: a secure, governed way for agents to access internal APIs without leaking data or bypassing auth.

**Risk-Based Automated Code Review.** He notes that internal systems are being retooled to "categorize risk" of PRs using AI. This suggests a pattern where AI isn't just *writing* code, but acting as a "gatekeeper" that determines which human reviews are needed.

**Strategic Churn & Early Access (The Shopify Model).** Shopify's strategy—trading off high churn and high cost for a six-month head start on competition—is a specific "Innovation Architecture." They give GitHub "3,000 people of honest feedback" in exchange for being first. This is "innovation as a procurement strategy."

**Custom Background Coding Agents for Monorepos.** Large companies are realizing that off-the-shelf coding agents can't "fit the context" of a massive monorepo. They are building "custom background agents" that live inside the repo and have indexed knowledge that surpasses what a generic model can see via RAG.

## Hidden Signals

**"If you're not building an MCP gateway, what are you even doing?"** This is the strongest technical signal in the talk. It suggests that the "MCP standard" is already the winner for enterprise AI connectivity, even if it hasn't fully permeated the "public" discourse yet.

**"Anything with AI in it gets funded."** This is a meta-commentary on corporate resource allocation. "Platform Engineering" is dead; "Agent Experience" is the new way to get headcount. This signals a re-branding of the entire internal developer platform (IDP) stack as an "AI stack" to secure budget.

**The Brian Armstrong / Coinbase "Week of AI" Ultimatum.** This is a signal of the "Hard Pivot" management style. Firing an engineer for not using AI within a week is a radical cultural enforcement. It signals that at the highest levels of tech, "AI Skepticism" is now seen as a fireable offense, regardless of individual coding skill.

**"Leave your priors behind."** This phrase is Orosz's way of signaling a generational shift. He’s telling senior engineers that their hard-won intuition about "how computers work" might actually be a liability when working with non-deterministic agents. The new intuition is "probabilistic," not "logical."

## What's NOT Said

**The Quality of the "Maxed" Tokens.** He talks about the *quantity* of tokens being measured, but he doesn't discuss how (or if) anyone is measuring the *quality* of those tokens. If an engineer generates 1M tokens of garbage, does the leaderboard reward them? The silence on "Quality Evaluation" (Eval) suggests that big tech is currently in a "spray and pray" phase of adoption.

**The Long-Term Cost of "One-Pizza Teams."** If teams shrink from 10 to 3, what happens to mentorship? How do junior engineers learn if there are no "mid-level" tasks left because the AI did them? Orosz focuses on the productivity gain but ignores the "human capital" depletion.

**The failure rate of internal AI infra.** He mentions everyone is building MCP gateways and custom agents. He *doesn't* say how many of these projects are failing or how much technical debt is being created by "agentic" retooling.

**The "Pragmatic" ROI.** For someone called "The Pragmatic Engineer," there is surprisingly little math on the *actual* ROI of these tools beyond "it feels faster" or "it selects for smart people."

## Speaker's Worldview

Orosz is a corporate realist. He views the tech industry through the lens of power dynamics, incentives, and organizational behavior. He doesn't see AI as a "magic" technology, but as a "new metric" that will be gamed, weaponized, and eventually standardized. His worldview is one where the "Product Engineer" is the ultimate survivor—someone who can code, manage agents, understand the product, and navigate the "BS" of big tech lead-maxing leaderboards.

## Key Quotes

1. **"Token maxing happens at large companies... it's the person who's smart and willing to put up with absolute BS to get the job."** — The cynical reality of big tech AI adoption.

2. **"Understanding the theory will not make you better at using the tools... which is an absolute mind-f*ck honestly."** — The fundamental shift in engineering knowledge.

3. **"If you're in a large company and you're not already building an MCP gateway, what are you even doing?"** — The "infra" imperative for 2026.

4. **"We're in this stage where like just just leave your priors behind. Just have an open mind."** — The psychological requirement for the agentic era.

5. **"Two pizza teams are now just one pizza teams... partially because of these tools."** — The structural impact on engineering orgs.
