# Deep Analysis: DDD and TDD against AI Slop
**Speaker:** Matt Pocock | **Company:** AI Hero | **Date:** 2026-04-26

## Core Beliefs

**Software fundamentals matter more now than they ever have.** Pocock’s central thesis is a direct challenge to the "AI will do it all" hype. He argues that rather than making old engineering principles obsolete, AI makes them a survival requirement. If you don't understand DDD, TDD, and clean architecture, you won't be able to steer the AI; you'll just be drowned in "AI Slop."

**The "Specs to Code" movement is a dangerous fallacy.** He is openly critical of the idea that we can just write a spec and ignore the code. He calls this "vibe coding by another name" and notes that ignoring the code leads to a rapid collapse into garbage. His experience is that repeated "compilation" of specs to code without human architectural oversight leads to "software entropy"—a state where the codebase becomes harder to change and more bug-prone with every iteration.

**Code is not cheap; bad code is the most expensive it's ever been.** This is his most provocative stance. The prevailing narrative is that AI makes code "free." Pocock counters that a codebase that is hard to change prevents you from utilizing AI's full potential. Therefore, the cost of bad architecture is magnified in the AI era because it creates a bottleneck for the very tool that is supposed to provide speed.

**The Human is the Strategic Officer; the AI is the Tactical Sergeant.** This metaphor complements O'Leary's "junior developer" model but adds a layer of command and control. The AI is great at the "on the ground" execution, but the human must maintain the "strategic level" of system design.

## Pain Points

**"Outrunning your headlights."** This is his term for AI tools that generate huge amounts of code without feedback loops. If the rate of feedback is the speed limit, most AI tools are currently speeding into a brick wall. The failure mode is the AI producing 500 lines of code that looks correct but fails silently or violates non-obvious constraints.

**The "Invisible Design Concept" Gap.** He references Frederick Brooks to explain why AI often fails: the human and the AI don't share a "design concept"—the ephemeral, unwritten theory of what is being built. Without this shared mental model, the AI makes "technically correct but contextually wrong" decisions.

**Cognitive Fatigue (The "Knackering" Effect).** Pocock identifies a real human cost: the exhaustion of trying to keep up with the volume of code an AI can produce. This fatigue is a sign that the architecture is too complex for the human to manage, necessitating a shift to "deep modules" that can be treated as gray boxes.

**AI Verbosity and Communication Barriers.** The AI often uses too many words or inconsistent terminology, leading to "talking at cross-purposes." This is a language gap that traditional "domain expert vs. developer" patterns (DDD) were designed to solve.

## Architecture & Technical Patterns

**Deep Modules (The Ousterhout Principle).** Pocock advocates for "deep modules"—large modules with simple, well-defined interfaces that hide complexity. This is an architectural guardrail: let the AI handle the complex implementation *inside* the module, while the human focuses on designing and testing the *interface*. This allows the human to treat parts of the system as "gray boxes," saving cognitive load.

**The "Grill Me" Skill.** This is a practical alignment pattern. Instead of letting the AI start coding immediately, the "Grill Me" skill forces the AI to interview the human (sometimes 100+ questions) until a shared design concept is reached. It turns the AI into an adversary to harden the plan before execution.

**Ubiquitous Language as a Machine-Human Sync.** Using DDD's ubiquitous language (formalized in a markdown file) ensures that the AI and human use the same terms in chat and code. This reduces verbosity and improves the alignment of the implementation with the plan.

**TDD as a Speed Limit.** He prescribes Test-Driven Development not just for quality, but as a mechanism to force the AI to take "small, deliberate steps." TDD acts as the governor on the AI's engine, preventing it from outrunning its headlights.

## Hidden Signals

**"Claude Code for real engineers."** By using this "provocative" course title, Pocock is carving out a niche for professional engineering standards in a market saturated with "build an app in 5 minutes" demos. He is selling *professionalism* as the differentiator.

**Direct Critique of "Plan Mode."** He explicitly states his "Grill Me" skill is better than the native "Plan Mode" in Claude Code. This signals a trend where power users are already finding vendor-default AI behaviors (eagerness to execute) to be suboptimal for serious engineering.

**"I personally believe this is better than the default plan mode... don't at me on this."** This phrase signals a burgeoning "expert vs. novice" split in AI tool usage. There is a "right" way to use these tools that involves more friction up front but better results at the end.

**The shift from "Code Review" to "Interface Design."** His advice to "design the interface, delegate the implementation" suggests that the senior engineer of the future spends zero time looking at individual lines of code and 100% of their time on API design and test suites.

## What's NOT Said

**Legacy Codebase Migration.** He focuses on building things "the right way," but doesn't address the nightmare of applying these principles to a 10-year-old "spaghetti" codebase where AI entropy has already set in.

**The Time Cost of "Grilling."** While "Grill Me" produces better code, the human time required to answer 100 questions is significant. There is no discussion of the ROI of this time investment versus traditional development.

**Can AI learn to be "Deep"?** He assumes humans must design the "deep modules." He doesn't explore if an agentic system could eventually learn to refactor its own shallow modules into deep ones based on these principles.

## Speaker's Worldview

Matt Pocock is a "Software Fundamentalist" who believes the path forward is through the past. He views the AI age not as a reason to abandon discipline, but as a reason to double down on it. His worldview is one of "Strategic Humanism"—where the human provides the wisdom and the architecture, and the machine provides the labor. He is deeply skeptical of "vibe coding" and believes that without rigorous structure, AI-assisted development is just a faster way to build a disaster.

## Key Quotes

1. **"Software fundamentals matter now more than they actually ever have."**
2. **"Bad code is the most expensive it's ever been."**
3. **"The rate of feedback is your speed limit."**
4. **"Design the interface, delegate the implementation."**
5. **"Specs to code is not investing in the design of the system. We are divesting from it."**
