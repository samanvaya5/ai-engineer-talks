# Deep Analysis: AI Agents as the New Application Layer
**Speaker:** Malte Ubl | **Company:** Vercel (CTO) | **Date:** 2026-04-26

## Core Beliefs

**AI engineering is the legitimate successor to web development.** Malte frames the current moment not as a sub-discipline, but as a total evolutionary leap. He explicitly compares the rise of AI engineering to the early days of web development (referencing his JSConf EU days), suggesting that AI is the new "mainstream discipline" that will define the next decade. This isn't just about adding features; it's about a fundamental shift in how software is conceived and delivered.

**The software market is highly elastic: cheaper production equals massive volume expansion.** He proposes an economic thesis that we are "speedrunning" an experiment in market elasticity. By lowering the cost of software creation, we aren't just doing the same work faster; we are enabling a vast "circle of software that should exist" but wasn't economically viable to build with traditional methods. This is a direct rebuttal to "AI replaces engineers" anxiety—he argues demand will scale to fill the newfound capacity.

**The "SAS Apocalypse" is a pivot, not an end.** He acknowledges the Silicon Valley narrative that companies will "make" rather than "buy" software, but predicts SAS survival through adaptation. The underlying belief is that the "make vs. buy" decision is shifting because agents make custom internal tools drastically cheaper to build and maintain.

**Infrastructure must adapt to agent-written code.** He believes that if humans didn't write the code, they won't have the same "strong feelings" about its runtime details. This implies a future where infrastructure is a "just works" black box for agent-generated workloads, prioritizing deployment speed and reliability over manual tuning.

## Pain Points

**The Security "Nightmare" of 1999 Redux.** Malte describes the current state of AI security as reminiscent of the early internet—everything is potentially hackable, and we don't yet know how to build secure foundations. He predicts a "rude awakening" for the industry, suggesting that our current "agent harnesses" are fundamentally insecure.

**Wrong Architecture in Agent Harnesses.** He identifies a specific architectural flaw in most current tools: combining the harness (the logic directing the agent) with the execution environment (where the generated code runs). He advocates for strict isolation, praising Anthropic for adopting this separation.

**The "Boring Work" Morale Tax.** He uses Vercel's support agent as a case study for "deflection." The pain point isn't just cost; it's the "toil" that destroys job satisfaction. By automating the 90% "boring" cases, he argues agents actually *save* the human profession by leaving only the "actually interesting" problems.

## Architecture & Technical Patterns

**CLI-First/API-First over UI-First.** A startling statistic: 60% of Vercel's own page views are now AI agents. This drives an architectural requirement: "What's the CLI? How does an agent use this?" Ubl is signaling that the era of human-centric GUI as the primary interface is ending. UI is becoming "something that's so cheap" and secondary to machine-legible interfaces.

**The "Compress the Research" Pattern.** He identifies a high-leverage agent archetype: Business Event → Automated Research Agent → Human Decision. This pattern maintains a constant risk profile (human stays in the loop) while providing 10x-100x speedups in business processes. It's a "low-hanging fruit" strategy for enterprise AI.

**Sandbox Innovation (Just Bash).** He's betting on lightweight, high-performance sandboxes (zip nanosecond startup time) like his `Just Bash` project. The pattern is: agents love bash, but they need a safe, fast environment to use it.

**Model Commoditization vs. Application Layer Value.** He sees the "Big Model Labs" (OpenAI, Anthropic, Google) as becoming commoditized. His architecture focuses on building a "stable layer on top" that can thrive independent of which model is currently the leader. He specifically praises Google for driving the "price floor" down via infrastructure efficiency.

## Hidden Signals

**"Vibe Coding" as a serious technical term.** By using "vibe coding" in a keynote, the CTO of Vercel is legitimizing a shift away from rigorous syntax toward high-level intent and orchestration. It signals that "correctness" is being redefined around outcomes rather than code structure.

**Europe as the Leader in Engineering (not Models).** This is a strategic narrative violation. He's telling the European audience: "You lost the model race, but you're winning the engineering race." By citing AI SDK, Pi, and OpenClaw, he's attempting to ground the AI boom in European engineering culture (Berlin, Austria).

**"60% of page views on versell.com were AI agents."** This is perhaps the most significant data point in the talk. It suggests the "Dead Internet Theory" is becoming a functional reality for infrastructure providers, where the majority of traffic is non-human.

**"Europe isn't going to play a major role on the model side... I don't think it needs to."** This signals a belief that the real power and margin will eventually migrate away from the LLM providers and toward the companies that control the "agentic infrastructure" and "application layer."

## What's NOT Said

**The reliability gap.** While he mentions 90% support deflection, he doesn't talk about the failure rate of more complex agents or the cost of human supervision for the research-compression pattern.

**Vercel's proprietary lock-in.** He talks about a "stable layer on top" of models, but that layer is increasingly Vercel's own AI SDK and infrastructure. The "independence" he preaches is model-independence, not platform-independence.

**Energy and Environmental costs.** In a talk about massive software volume expansion ("the circle gets filled out"), the environmental impact of running millions of agents 24/7 is conspicuously absent.

## Speaker's Worldview

Malte is an economic pragmatist who sees AI as a massive expansion of the software industry's footprint. He views the disruption not as a threat to engineers, but as a liberation from "toil" and an opportunity to build the software that was previously too expensive to exist. He is deeply focused on the "how we build" (agents) and the "what we build" (agent-ready infrastructure).

## Key Quotes

1. **"AI engineering is the legit legitimate successor to web development as a really mainstream discipline."**
2. **"We are speedrunning what's really an experiment in economics of how elastic the software market is."**
3. **"UI is now something that's so cheap. The era of the human-centric dashboard is shifting to API and CLI."**
4. **"Europe against all odds is taking actually a leadership role in AI engineering."**
5. **"In a commoditized world... we the engineers are the powerful ones. It's the application layer where the real innovation happens."**
