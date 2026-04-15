# Deep Analysis: Curing FOMAT (Fear Of Missing Agent Time) with Mobile Command
**Speaker:** Michael Richman | **Company:** Bitly | **Date:** 2026-04-26

## Core Beliefs

**FOMAT (Fear Of Missing Agent Time) is the new developer anxiety.** Richman identifies a shift in the psychological state of the modern engineer: we no longer fear missing a party (FOMO); we fear our agent is sitting idle waiting for a "yes/no" while we're away from our desks. This belief reframes the "productivity" gains of AI as a new type of logistical burden—the "babysitting" requirement.

**Agents are low-touch in theory, but high-maintenance in practice.** He rejects the "set it and forget it" narrative of autonomous agents. He argues that the current state of the art requires constant back-and-forth. The "truth" is that agents block frequently on small decisions, and every minute they wait is "missed agent time" that compounds over a project's lifecycle.

**The new "Flow" is Agent Choreography, not deep coding.** This is a profound shift in professional identity. He argues that the classic "zone" (hyper-focused on one problem) is being replaced by a multi-threaded "choreography" where the engineer moves between 4-5 agents, unblocking one and redirecting another. The "elegance" of the work is now in the coordination, not the implementation.

**Physical detachment from the dev machine is a productivity killer.** He believes that the "tether" to the terminal or IDE is the last remaining vestige of the pre-agentic era. If an agent can work for 5 hours, the human shouldn't have to be at their desk for 5 hours to supervise it. Productivity is now a function of availability, not presence.

## Pain Points

**Agent Idleness and "The 2-Minute Block."** He describes the frustration of returning to a desk to find an agent stopped 2 minutes after you left, having wasted 30 minutes of potential work. This is the specific "pain" that Command & Control (C2) aims to solve.

**Cognitive Load of Session Management.** "I cannot keep track of more than two or three sessions at a time." He identifies a human cognitive limit: once you have 5+ agents running across different tools (Claude Code, Cursor, Gemini), you lose track of which one is doing what. This "state fragmentation" is the primary barrier to scaling agentic output.

**Tool Fragmentation.** The need to use multiple platforms (Claude for reasoning, Cursor for IDE, custom CLIs for internal tasks) creates a "multi-window hell." There is no "single pane of glass" to see the status of the entire "agent army."

**The "In-Bed Prompting" Reality.** He admits to starting his day with prompts from bed. This highlights a dark side of agentic engineering: the work-life boundary is dissolving because the "cost" of starting a task is now low enough to do while half-asleep.

## Architecture & Technical Patterns

**Command & Control (C2) System Architecture:**
*   **Daemon Layer:** A lightweight, open-source process that runs alongside the agent platforms (Claude Code, Cursor, etc.) on the dev machine or cloud VM.
*   **Control Plane:** A cloud-based aggregator that monitors the lifecycle of all agents. It detects when an agent is "blocked" or "done."
*   **API/UI Layer:** A mobile (iOS/Android) and web interface that communicates with the control plane to provide push notifications and a "single pane of glass" view.

**Single Pane of Glass.** C2 is designed to be "coding tool agnostic." It aggregates sessions regardless of whether they are running on a local Mac or a cloud VM, providing a unified dashboard for all agent activity.

**Session Subscription for Notifications.** A key feature is the ability to "subscribe" to a specific high-stakes session for push notifications while letting background tasks run silently in a "recent" or "radar" section.

**Overview Dashboard (The "Standup Summary").** Using LLMs to summarize the state of multiple active sessions into a "standup" view. This is an example of using AI to manage AI—context compression for the human manager.

## Hidden Signals

**The "Agent Choreography" euphemism.** While he presents it as an "elegant new flow," it's actually a description of high-speed context switching. He is signaling that the era of "deep work" might be over for senior engineers, replaced by a "managerial overhead" role where they supervise a fleet of digital workers.

**"Droid Whispering" via Mobile.** By enabling agent management from a watch or phone, he is moving the developer role closer to "Operations" or "On-Call." The developer is never "off," because the agent is always "on."

**Productizing the "Always-On" Culture.** The existence of C2 signals that the industry is moving toward a 24/7 development cycle where the human is the "on-call supervisor" for the agent's "night shift."

**The Bitly Influence.** As a leader at Bitly, Richman is obsessed with "links" and "redirection." C2 is essentially a "URL shortener for agent state"—taking a complex, fragmented state and making it accessible via a simple, mobile-friendly interface.

## What's NOT Said

**Security and Remote Access.** He glosses over the massive security implications of a daemon that can interact with your terminal/IDE and expose that state to a mobile app via a cloud control plane. The "single pane of glass" is also a single point of failure for a developer's entire machine.

**The Irony of "Curing FOMO."** By making agents accessible from bed and on walks, C2 might actually *increase* anxiety and FOMAT. If you can check your agent from your watch, you'll never truly "get away" from it, despite his claims about "time away being important."

**Human Review Quality on Mobile.** Can you actually perform high-quality code review or unblock a complex architectural decision from a phone screen? The talk assumes the "decisions" are simple (yes/no), but most agent blocks are for complex ambiguities that require a full IDE context to resolve.

## Speaker's Worldview

Richman is a "Pragmatic Multi-Threader." He views the future of engineering as a management challenge. He accepts that agents are currently "dumb" and need "babysitting," so his solution is to build a better "baby monitor." He sees the human's value not in the writing of code, but in the "choreography" of intent across multiple specialized tools. His worldview is one of "Ubiquitous Agency": if the agents are working everywhere, the engineer must be able to lead them from anywhere.

## Key Quotes

1. **"What is fear of missing agent time? It's when you get back to your desk and realize that after two minutes it actually stopped to ask you a question and it's been waiting there blocked the entire time."** — The pain point that defines the talk.

2. **"The new type of flow in the agentic world is more about agent choreography. Multiple agents working in parallel with you moving between them, unblocking one, redirecting another."** — A redefinition of the "Zone" for the AI era.

3. **"I often start my day with prompts that I issue from bed, honestly."** — A revealing look at the dissolving work-life boundaries of the agentic engineer.

4. **"I needed a system that is a single pane of glass into all of my agent sessions, regardless of which platform they're running on."** — The core architectural requirement for the fragmented AI market.

5. **"Whatever your agent, you can reach it and it can reach you."** — The promise (and threat) of the always-available developer.
