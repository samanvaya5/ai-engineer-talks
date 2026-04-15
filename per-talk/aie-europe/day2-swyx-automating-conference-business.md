# Deep Analysis: Automating a $9M Conference Business Using AI Agents
**Speaker:** swyx (Shawn Wang) | **Company:** AI Engineer / Cognition | **Date:** 2026-04-26

## Core Beliefs

**The "Tiny Team" is the ultimate economic unit of the agentic era.** swyx defines this as teams with "more millions in revenue than number of employees." His core belief is that the "one-person billionaire" is a distraction; the real revolution is 9-person teams running $9M+ businesses by replacing headcount with agentic workflows. He views headcount not as a sign of growth, but as a sign of unoptimized "human yak shaving."

**Agents for "Everything Else" is the 2026 breakout trend.** He argues that we've over-indexed on agents for coding and under-indexed on agents for boring, routine knowledge work (ETL, buying, logistics). His "lobster story" isn't a joke; it's a demonstration that a coding agent (Devon) with web access is actually a serverless Executive Assistant. The intelligence is a commodity; the "unlock" is the agent's ability to navigate the messy real world (phone calls, emails, websites).

**Non-technical employees are the best "Agent Whisperers."** swyx observes that his designer, who lacks formal coding training, communicates with agents more effectively than engineers do. By treating the agent as a "human colleague" (using redlines, annotations, and natural language), non-technical staff bypass the "prompt engineering" trap and move straight to "collaboration." The agent doesn't need a manual; it needs a peer who knows what "good" looks like.

**"Agent Experience" (AX) is replacing User Experience (UX).** Citing Malte Ubl (Vercel), swyx claims 60% of Vercel's traffic is now bots. This leads to a radical conclusion: dashboards are dead. If your primary user is an agent, your UI should be an MCP, an API, or a CLI. He characterizes the "custom dashboard" era as a legacy constraint for humans that agents find annoying.

## Pain Points

**"Yak Shaving" is the silent productivity killer.** He identifies the dependency tree crawling (Python environments, dependency hell) as the primary source of developer burnout. Agents "saving the yak shaves" is his #1 value prop. The pain isn't the code; it's the 45 minutes of "pre-work" required to write one line of code.

**The "CMS Sanity" trap.** He admits to throwing away his CMS (Sanity) because the friction of a GUI was higher than the friction of asking an agent to "manage the code." The pain point here is the middle-man: the GUI that exists to help humans manage data but actually slows down agents.

**Management Psychosis vs. Employee Realism.** A rare, honest admission: "I clearly have the most psychosis." He identifies the tension between a visionary leader who wants to "AI-ify everything" and the employees who have to deal with the "sh*t when it goes wrong." This is the friction of the transition: leaders see the $9M potential; employees see the 2 AM debugging session when the agent deletes the production database.

**SAS Overload.** He views the modern SAS stack as a tax on "tiny teams." The pain is paying for a dozen tools that are 80% overlapping. His solution — "kicking out SAS and building it ourselves because we can" — is a declaration of war on the subscription economy.

## Architecture & Technical Patterns

**The "Agent-as-Source-of-Truth" CMS.** Instead of a database or a headless CMS, swyx uses code (managed by Devon) as the source of truth for the conference schedule. The "UI" for the data is a screenshot or a forwarded email to the agent. This is a "No-UI" architecture where the agent is the query engine and the data entry clerk.

**Parallelism > Autonomy.** He argues that we value "autonomy" (the agent does it all) too much and "parallelism" (10 agents doing 10 things) too little. His workflow isn't about one agent being a genius; it's about a "swarm" handling the volume of 130 speakers and 6,000 attendees simultaneously.

**Figma-to-Devon-to-Live.** Using "Co-work" and Devon to bypass the front-end developer entirely. The architectural pattern here is "Direct-to-Implementation" (DTI). The design *is* the spec, and the agent *is* the compiler.

**Serverless Knowledge Work.** Using coding agents for non-coding tasks like buying physical goods (the London lobster). This repurposes the agent's browser-automation and reasoning capabilities as a "serverless human" on demand.

**MCP-First Infrastructure.** Shifting internal tools from dashboards to Model Context Protocol (MCP) servers. This ensures that any agent he hires (Devon, OpenClaw, NanoClaw) can immediately "plug in" to the business's data without a custom integration.

## Hidden Signals

**"Nvidia bought Grok for $20 billion."** (Note: Cross-referencing Sarah Chieng's talk). This continues to be the massive industry signal of the 2026 conference. swyx's focus on "Agent Experience" validates why Nvidia would want the "inference stack" to be vertically integrated.

**"60% of Vercel traffic is agents."** This is the "Aha" metric of the talk. It signals that the web has already flipped. We are no longer building for humans who click; we are building for agents who crawl.

**"Psychosis" as a leadership trait.** By calling his drive "psychosis," swyx signals that the transition to an AI-native company is irrational and painful. It requires a level of "delusion" that isn't found in standard corporate management.

**"AGI Pills" as physical merch.** This isn't just a meme; it's a cult-building signal. It mimics the "orange pilling" of Bitcoin, suggesting that "Agentic Engineering" is becoming a belief system, not just a job.

## What's NOT Said

**Profit Margins.** He mentions $9M revenue for 9 people, but not the token costs or the "Devon bill." If each of those 207-reply threads costs $50-$100 in tokens, the "Tiny Team" economics might be tighter than they look.

**Data Privacy.** He's forwarding emails and pasting screenshots into third-party agents (Cognition/Devon) without mentioning privacy or security. This implies a "move fast and break things" attitude toward company data that might not fly in a regulated enterprise.

**The "Cognition Adviser" Conflict.** He mentions he "just advises for the company now," but uses Devon for every example. The talk is as much a product pitch as a case study, which masks potential weaknesses in the agentic workflow.

**Employee Churn/Retention.** He mentions his employees are "having more fun," but he also mentions arguing with them about replacing their tools. The long-term impact on "human" morale when the boss is constantly trying to "AI-ify" their jobs is left unaddressed.

## Speaker's Worldview

swyx is an "Accelerationist Realist." He believes the "Tiny Team" future is inevitable and that resistance is futile (and expensive). He views humans as "curators of fun" and "decision makers," while agents handle the "yak shaving" of life. His worldview is deeply pragmatic: if a tool works (Devon, Co-work), use it; if a tool is annoying (Sanity), kill it. He values "psychosis" (visionary drive) over "sanity" (status quo) and sees the primary role of a leader in 2026 as "prescribing the AGI pill" to a skeptical workforce.

## Key Quotes

1. **"I am getting more work out of my employees because they enjoy doing it... I'm no longer talking about lines of code. I'm getting more productivity out of my humans."** — The ultimate reframing of the "AI replaces humans" narrative.

2. **"Anytime there's random yak shaving... agents save you the yak shaves. A model of productivity that doesn't appreciate parallelism is not fully capturing the benefit."** — Identifies the "unseen" value of agents.

3. **"Your dashboards don't matter. Your APIs matter. Your CLIs matter. Your MCPs matter. Your primary user is shifting towards what people are calling Agent Experience."** — The death knell for traditional SaaS UI.

4. **"I clearly have the most psychosis... bring [employees] along the journey and don't talk down or ignore their concerns."** — A rare moment of managerial empathy in an accelerationist talk.

5. **"Agents for everything else are coming. Wake up, use it, bring it home to work."** — The call to action. Simple, urgent, and all-encompassing.
