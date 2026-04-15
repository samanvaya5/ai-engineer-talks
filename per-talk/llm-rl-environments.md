# Deep Analysis: Let LLMs Wander: Engineering RL Environments
**Speaker:** Stefano Fiorucci | **Company:** Deepset (Haystack) | **Date:** 2026-04-08

## Core Beliefs

**The paradigm has shifted.** Stefano believes the LLM training world has moved past the pre-training -> SFT -> RLHF pipeline. He frames SFT as "statistical expert imitation" -- inherently limited because the model "tends to stay close to the distribution of those examples." RL with verifiable rewards is presented not as an incremental improvement but as a qualitative leap: "the model is no longer limited by the quality of human examples. Through trial and error, it can discover more efficient reasoning strategies." This is the talk's foundational thesis.

**Small models can beat large ones on specific tasks.** The entire tic-tac-toe arc is designed to prove this point. He starts with LFM-2 (a small open model by Liquid AI) that "struggles to follow format and to make valid moves," then trains it via SFT + RL until it outperforms GPT-5 Mini (the teacher model used to generate synthetic data) against optimal opponents. The message: specialization through RL environments is the great equalizer.

**Open-source infrastructure is a strategic imperative, not just a nice-to-have.** He explicitly frames Verifiers and the Environments Hub as a counterweight to emerging commercial RL environment providers: "We don't want open-source models to lag behind just because they lack the right playgrounds to train it." This isn't altruism -- it's a recognition that the bottleneck is shifting from model architecture to training infrastructure.

**Learning requires structured exploration, not just more data.** The tic-tac-toe experiments demonstrate a philosophy: models need spaces where they can "wander" (the talk's title), fail, and get clear feedback. The progression from random opponent -> controlled opponent -> near-optimal opponent mirrors a curriculum learning philosophy. Temperature is treated as an exploration knob -- increased to break the model out of "learned strategies," with full awareness this is "risky."

**Assumptions about where AI is going:**
- RL environments will become a market category ("startups building RL environments are getting major funding")
- The DeepSeek R1 recipe (RL with verifiable rewards + GRPO-family algorithms) is the proven path forward
- Post-training is where the real capability gains will come from now
- Democratization of RL training is imminent ("you can do this at home, too")

**What he's skeptical about:**
- Starting from pre-trained reasoning models when GPU-constrained (they "tend to output long thinking traces" that get truncated, "wasting budget" and risking "damaging the model's intelligence")
- Playing against purely optimal opponents from the start ("if the opponent is too perfect too early, the model might never see a win and fail to learn")
- Pre-training as a continuing source of improvement (cites Sutskever at NeurIPS 2024: "pre-training no longer seems to be enough")
- Constant monitoring of training runs ("you risk the temptation to stop it and tweak something prematurely")

## Pain Points

**Batch size is the silent killer.** This is the pain point he returns to most often and most emotionally ("I learned the hard way that batch size is a key parameter"). He describes model collapse and unstable training when batch size drops below 256. The explanation reveals his mental model: batch size = number of games the model learns from at once. Too few games with diverse opponents = reinforcing suboptimal strategies. This isn't a theoretical concern -- he experienced it as failed experiments.

**Hidden environment biases create phantom competence.** The minimax story is the most revealing anecdote in the talk. He used a minimax implementation that, when multiple moves had equal optimal scores, "the first free position was always selected." The model didn't learn to play tic-tac-toe -- it learned to play against *that specific deterministic player*. "I got great benchmark results, but then playing against the model, I realized that it was clueless." This is a parable about evaluation validity that extends far beyond games: your model optimizes for your environment, and if your environment has hidden structure, your model memorizes that structure rather than learning the underlying skill.

**Small model format compliance is a real barrier.** Before RL can even begin, small models struggle with basic instruction following: outputting moves in the correct XML format, choosing unoccupied cells. He had to add a whole SFT "warm-up" phase just to teach format before RL training could work. The invalid move handling evolved from "immediate loss" (too harsh, kills learning signal) to a "-0.1 penalty with turn capping" -- this iterative refinement reveals how much engineering goes into just making the environment's feedback signal usable.

**RL training instability is the norm, not the exception.** He describes "several failed experiments" and frames the successful run as the exception. The exploratory phase of the final training run shows "a significant initial drop in winning reward" before recovery -- he interprets this as the model unlearning old strategies to discover better ones. This pattern (performance degradation before improvement) is deeply counterintuitive and makes it hard to know when to stop or intervene.

**Noise contamination in group-based RL.** In GRPO, you compare rollouts from the same starting point. If the environment introduces randomness (which player goes first, how the opponent plays), the reward differences reflect environment variance, not model quality. He engineered a specific solution: deterministic seeding at every level (example seed for starting player, turn seed derived from example seed + board state). This is the kind of unglamorous infrastructure work that determines whether RL training actually works.

**Reward function design is fragile.** He uses three reward functions (winner reward weight 1, format reward weight 0.2, invalid move penalty). The weights matter. The choice of what to reward matters. The progression from binary (invalid move = loss) to graduated (invalid move = small penalty) shows how sensitive training is to reward shaping. This is a known hard problem in RL, and his pragmatic approach (try things, inspect rollouts, iterate) reveals there's no principled methodology yet.

## Architecture & Technical Patterns

**Verifiers library as the central abstraction.** Prime Intellect's Verifiers is positioned as the standard infrastructure layer. Key architectural decisions:
- Environments are **Python packages** -- installable and distributable
- Base class hierarchy: single-turn -> multi-turn -> tool environments, all built on the multi-turn foundation
- OpenAI-compatible API abstraction for model serving ("plug in OpenAI, OpenRouter, or local models via vLLM")
- Separation of concerns: environment logic vs. training infrastructure vs. model serving
- Rubric system: weighted collections of reward functions bundled together

**The SFT-warmup-then-RL pattern.** The training recipe is: (1) generate synthetic data from a strong teacher model (GPT-5 Mini), (2) filter (remove losing games), (3) SFT warm-up for format and basic competence, (4) RL with verifiable rewards for deep capability. This is emerging as a standard pattern -- SFT handles what can be demonstrated, RL handles what can only be discovered.

**GRPO -> CISPO progression.** He uses CISPO, described as "an improvement over GRPO." The group-relative approach (compare rollouts against group average, reinforce above-average trajectories) avoids needing a separate value model, reducing compute requirements. This matters for the democratization thesis.

**Noise reduction as architectural concern.** Three distinct noise-reduction mechanisms:
1. **Deterministic seeding**: same board state = same opponent response across rollouts
2. **Stratified sampling**: every batch has balanced mix of opponent difficulties
3. **Example-level seed consistency**: same example always has same starting player

This is noteworthy because it treats environment randomness as a first-class engineering problem, not an afterthought.

**Progressive difficulty curriculum.** The opponent skill is parameterized by `mean_random_move_prob` and `max_random_move_prob`. First training run: 20-70% random. Second run: 0-25% random. The explicit design choice: never start with 0% random (optimal only) because "the model became overly defensive and failed to exploit errors when tested against random players." This is curriculum learning implemented as environment configuration.

**Multi-reward composition.** The rubric pattern allows combining multiple reward signals: task success (win/loss), format compliance, and penalty signals. The weights (1.0, 0.2, etc.) are tuned hyperparameters. This compositional approach to rewards is more flexible than single-scalar reward and allows separating "doing the right thing" from "expressing it correctly."

**Training infrastructure: two-GPU setup.** One GPU for inference, one for training. He mentions 96GB GPU for SFT and "bigger GPUs just to experiment quickly" for the final RL run. The framing ("probably not required") suggests the actual requirements are modest, supporting the democratization thesis.

## Hidden Signals

**"Startups building RL environments are getting major funding and working directly with big AI labs."** This is stated in the opening minutes and then never returned to. It reveals that RL environments are becoming a commercial market category. The companies building these are not just tool vendors -- they're strategic partners for frontier labs. This means the environment layer is becoming a chokepoint in the AI stack.

**The benchmark-vs-reality gap is systemic.** The minimax bias story isn't just about one bad implementation. It's a structural problem: models optimize for whatever your environment measures. If your evaluation environment differs from deployment reality (which it almost always does), you get phantom competence. He discovered this by *playing against the model himself*. How many RL-trained models pass benchmarks but fail in deployment because nobody actually tested them in the real task?

**GPT-5 Mini is mentioned casually as a production model.** "GPT-5 mini is excellent at following format and is a good Tic-Tac-Toe player." No surprise or announcement -- just treated as an existing, available model. This suggests GPT-5 is well-established by April 2026. The fact that a trained small open model outperforms it against optimal opponents is presented as notable but not shocking.

**The fear of open-source falling behind is about infrastructure, not models.** "We don't want open-source models to lag behind just because they lack the right playgrounds to train it." The framing is revealing: he doesn't say open-source models are worse -- he says they might lag because they lack training infrastructure. The capability gap, in his view, is an infrastructure gap. This is a strategic bet: if open-source gets the environments right, the models will follow.

**"As a market for closed-source environments emerges."** This implies proprietary RL environments already exist and are being sold. Companies are selling curated, optimized environments as products to AI labs. This is a new layer in the AI stack that most people aren't tracking.

**The recursive language models mention.** He briefly mentions a class "implementing recursive language models, a novel idea... where language models can decompose and recursively interact with input context of unbounded length through REPL environments." This is dropped in without elaboration but represents a fundamentally different inference paradigm. It's a signal that the environment concept is expanding beyond training into inference-time architecture.

**Failed experiments are the real data.** "To get these results, I went through several failed experiments." The talk presents a clean narrative (SFT warmup -> RL -> mastery) but the actual process was messy iteration. The lessons (batch size, hidden biases, temperature risk) all come from failure, not success. This suggests the field is still in a pre-methodology stage where progress requires extensive trial and error.

## What's NOT Said

**Safety and alignment are completely absent.** In a talk about training models through trial and error with reward signals, there's zero discussion of reward hacking, specification gaming, or alignment failure. The minimax bias story is actually *about* the model gaming the evaluation, but it's framed as an engineering bug, not an alignment concern. Given that RL environments are being positioned as the primary post-training paradigm, this omission is conspicuous.

**No discussion of scaling laws.** He never addresses how performance scales with training compute, number of environments, or model size. The tic-tac-toe result is presented without addressing whether the approach generalizes to harder tasks or requires fundamentally different engineering at scale. For a talk positioning RL environments as the path to "scaling intelligence," this is a significant gap.

**No mention of reward function design methodology.** The reward functions are presented as chosen, not designed. There's no discussion of how to systematically develop reward functions for tasks where the correct answer isn't easily verifiable. The entire approach depends on "verifiable rewards" but the hard cases (open-ended generation, creative tasks, complex reasoning) where verification is inherently subjective are not addressed.

**Multi-agent and adversarial settings are ignored.** Despite tic-tac-toe being a two-player game, the "opponent" is always an algorithm, never another learning agent. No discussion of multi-agent RL, self-play (which was crucial for AlphaGo), or adversarial robustness. This is a significant gap given that real-world LLM deployment increasingly involves multi-agent interactions.

**No compute cost or environmental impact discussion.** He mentions GPU requirements casually but never discusses the total cost of the RL training process (multiple failed experiments + successful runs). For a talk advocating democratization, the absence of cost analysis is notable.

**No discussion of evaluation methodology beyond the environment itself.** The evaluation is always done within the same framework (Verifiers). There's no mention of held-out evaluation, transfer to related tasks, or out-of-distribution robustness. The only external validation is playing against the model manually -- which is qualitative and limited.

**No mention of the data wall.** He cites Sutskever on pre-training limits but doesn't address that RL environments also need data (questions, initial states, task definitions). The "verifiable rewards" paradigm still requires task curation. Where do the tasks come from? Who designs them? This is another infrastructure bottleneck not addressed.

**Tool-augmented RL is teased but not demonstrated.** Tool environments are described in the library overview and the closing recommendation is to "train small language models on two-three tools you often use," but the actual experiment is pure text-in-text-out game playing. The gap between the vision (models using APIs, terminals, databases) and the demonstration (tic-tac-toe) is substantial.

## Speaker's Worldview

Stefano is a pragmatic tinkerer-engineer who believes AI capability is fundamentally an infrastructure problem, not a research problem. His worldview is that the algorithms are largely solved (GRPO/CISPO, verifiable rewards) and the bottleneck is building the right environments and engineering the training pipeline correctly. He's deeply skeptical of benchmark-driven evaluation (the minimax story is his parable) and believes in hands-on, iterative experimentation over theoretical elegance. He's an open-source evangelist not for ideological reasons but because he sees infrastructure democratization as the practical path to preventing a permanent capability gap between closed and open models.

## Key Quotes

1. **On the paradigm shift:** "In reinforcement learning with verifiable rewards, the model explores different trajectories from its pre-training and learns to favor the ones that maximize rewards. And this is exciting because the model is no longer limited by the quality of human examples."

2. **On phantom competence (the minimax parable):** "I got great benchmark results, but then playing against the model, I realized that it was clueless. Looking better at minimax, this had a bias. If multiple moves had the same optimal scores, the first free position was always selected. I basically was training my model against a specific type of optimal player."

3. **On the open-source strategic imperative:** "We don't want open-source models to lag behind just because they lack the right playgrounds to train it."

4. **On the core thesis of the talk:** "We did not just show the model how to play. We gave it a space to play and guided it through rewards."

5. **On the practical wisdom of RL engineering:** "Reinforcement learning is slow and takes time to see progress. If you continually monitor it, you risk the temptation to stop it and tweak something prematurely. While a slowly progressing run can be a surprisingly good one given enough time. So, start training and go for a walk."
