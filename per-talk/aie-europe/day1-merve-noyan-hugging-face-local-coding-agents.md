# Deep Analysis: Local Coding Agents & Hub Skills
**Speaker:** Merve Noyan | **Company:** Hugging Face (Open Source Team) | **Date:** 2026-04-26

## Core Beliefs

**Open source is the ultimate "differential" for security and reliability.** Noyan argues that open weights and open licenses (MIT/Apache 2.0) are not just philosophical choices but strategic engineering ones. Her key claim: if your model is open, its performance cannot be degraded by a provider without your knowledge (a direct swipe at reports of Claude’s performance dropping). Everything is "in the open," which guarantees stability for the engineer.

**Privacy is guaranteed by the Edge.** She believes the only true way to handle sensitive data in the AI era is to deploy to edge devices, browsers, and local servers. This "local-first" approach is her answer to the "security nightmare" mentioned by Malte Ubl.

**"Agentic Day Zero" is the new standard.** She observes a shift where every new model release (Gemma 4, Qwen 3.5, etc.) includes vision capabilities (VLMs) from the start. This implies that "pure" text models are becoming legacy; the future of agents is multimodal by default, allowing them to "see" screenshots and navigate UIs like humans.

**Complexity should be automated away by the agents themselves.** Whether it's "napkin math" for VRAM requirements or picking the best OCR model, she believes the agent should handle the ML engineering overhead. The human should just issue high-level directives like "train this model on this data."

## Pain Points

**Model Selection Fatigue.** With nearly 3 million models on the Hugging Face Hub, "picking the right one" has become a major bottleneck. She introduces benchmark buttons and inference provider routing as the technical fix for this cognitive overload.

**Hardware and VRAM "Napkin Math."** Fine-tuning and deploying models locally remains too "frictiony" for the average engineer. The effort required to calculate if a quantized model fits in 24GB of VRAM is a barrier she wants to eliminate via automation.

**Unstructured Data and OCR Scaling.** She highlights the pain of moving from 30,000 PDF papers to a machine-legible format. The manual effort of writing scripts and setting up infrastructure for massive OCR jobs is a primary target for agentic automation.

## Architecture & Technical Patterns

**Hugging Face Skills as Modular Agent Capabilities.** She introduces "skills" (LLM Trainer, Gradio, HF Dataset) as parameterized workflows that can be "plugged into" any agent (Claude, Gemini, or local). This is essentially a "plugin store" for agentic machine learning tasks.

**Agent Traces as a New Data Primitive.** Hugging Face has launched a "traces" repository type. The pattern here is: save your agent's execution steps (traces), visualize them, and then *fine-tune* a future agent on those traces. This creates a recursive loop where agents learn from their own (and others') successful executions.

**The "Hermes Agent" Self-Healing Pattern.** She highlights the Hermes agent for its superior memory management and "self-healing" capabilities—specifically, an agent fixing its own broken Slack integration. This signals a move toward agents that maintain their own environment.

**Buckets over S3.** She mentions "Buckets," a new infra product designed to be cheaper and faster than AWS S3, specifically optimized for the mounting and high-speed data needs of AI training jobs.

## Hidden Signals

**"I will just die on this hill" (re: Hermes Agent).** This phrasing signals a deep technical conviction that memory management and state persistence are the most important frontiers for agents, surpassing raw model intelligence.

**"Sci-fi at this point."** By describing "natural language to model training" as sci-fi, she is signaling that the threshold for what constitutes "difficult engineering" has shifted. What used to take a team of ML engineers now takes a single prompt.

**"Europe is taking actually a leadership role" (Echoing Malte Ubl).** This is the second time in the morning session that a major speaker has explicitly made this claim. It signals a coordinated effort to frame the "Agentic Era" as a European opportunity, contrasting it with the "Model Era" dominated by the US.

**"Napkin math" as a derogatory term.** She uses this to describe the manual calculations engineers currently have to do. This signals that Hugging Face wants to become the "Automatic Engine" that makes these calculations invisible.

## What's NOT Said

**The "Open Weights" vs. "Open Source" distinction.** While she mentions "open source," many of the models on the Hub are technically "open weights" with proprietary training data. She doesn't address the risk of "black box" open models.

**Energy Consumption.** Like Malte, she omits the massive environmental cost of the 24/7 agentic workflows and training jobs she is promoting.

**Skill Portability.** She mentions skills for Claude and Gemini, but doesn't discuss the difficulty of maintaining consistent agent behavior across different proprietary model backends.

## Speaker's Worldview

Merve Noyan is an open-source maximalist who believes in the democratization of AI through local execution and edge deployment. She sees a future where every engineer has a "mini-ML team" at their fingertips via agentic skills. Her worldview is one of "radical accessibility"—making the most complex ML tasks (training, quantizing, infra-management) accessible through simple natural language.

## Key Quotes

1. **"If you have everything in the open, nothing changes without you knowing. No performance degradation without you knowing."**
2. **"Having an AI engineer at your fingertips."**
3. **"I foresee that all of these models will be over time release day zero with vision capabilities."**
4. **"Training a model... is like sci-fi at this point."**
5. **"Europe against all odds is taking actually a leadership role in AI engineering."**
