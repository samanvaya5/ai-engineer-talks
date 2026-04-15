# Deep Analysis: OpenRAG: An open-source stack for RAG
**Speaker:** Phil Nash (introduces himself as "Ash" / "Tom Nash" — likely Phil Nash) | **Company:** IBM (Developer Relations Engineer) | **Date:** 2026-04-08

## Core Beliefs

- **RAG is not dead, and it's not solved either.** The speaker explicitly dismisses the "RAG is dead" narrative as unserious — noting that if a business has less than a million tokens of data, "rag is dead and probably so are those businesses." This is a pointed jab at the trivialization of enterprise data complexity. He reinterprets the claim as "RAG is solved," which he then dismantles by enumerating the real-world friction points that make every RAG deployment unique.

- **Every RAG system will be fundamentally different.** The speaker believes that because "everyone's documents are different, every system will have different users, different questions, different interaction patterns and different expectations," there can be no one-size-fits-all solution. What's needed is a *high-quality baseline to build from*, not a finished product. This is the philosophical core of OpenRAG — opinionated defaults, but extensible.

- **Open source is the right governance model for infrastructural AI tooling.** The closing argument isn't just "we open sourced it" — it's a worldview statement: "We can do it with open source components out in the open. That's what I'd like to see." This positions IBM as betting on ecosystem and community rather than proprietary lock-in for the RAG layer.

- **Agents should do retrieval, not static pipelines.** The speaker is a clear believer in agentic retrieval over traditional top-K vector search. Rather than embedding a query and hoping, the agent decides "what searches to perform and what to do with the results." This is a strategic bet: the intelligence should live in the retrieval loop, not just the generation step.

- **Models shouldn't do arithmetic.** A small but telling belief: "agents and models shouldn't be doing arithmetic. They're language models, not math models." This reveals a pragmatic engineering philosophy — use the right tool for the right job, don't force capabilities where they don't belong.

## Pain Points

- **PDFs remain the nemesis.** The speaker calls PDFs "that enemy of all rag systems" and dedicates significant time to Docling's multiple PDF pipelines (standard with small focused models, OCR backends, and a VLM pipeline). The fact that PDF processing alone required two full pipelines and an OCR option tells you everything about how unsolved document ingestion is.

- **Chunking strategies are a hassle — and changing/testing them is painful.** This isn't just acknowledged, it's highlighted as a first-class problem. The speaker frames chunking not as a one-time decision but as an ongoing tuning problem that most teams underestimate.

- **Embedding model migration is a silent killer.** "Embeddings keep improving which is great for the industry but not very great when you've used something from a 6-month or a year ago." This is a real production pain point — your entire index becomes stale and migrating embedding models is expensive and operationally complex. OpenRAG's support for multi-model vector search is explicitly framed as a migration pathway, which reveals this is a problem IBM has hit internally.

- **Cost of large context windows.** "Not everyone is happy paying for a million input tokens every time you want to ask a question." The speaker is signaling that token economics are still a real constraint for production systems, even if the technology can theoretically handle it.

- **The "just works" gap.** The speaker spends considerable time on the UX of OpenRAG — cloud connectors to Google Drive, SharePoint, OneDrive for document sync. This reveals the real pain isn't just in the ML pipeline; it's in getting documents into the system in the first place. The sync button and cloud connectors are solving the "data gravity" problem that kills many RAG pilots.

## Architecture & Technical Patterns

- **Three-pillar stack: Docling → OpenSearch → LangFlow.** Docling handles document processing and chunking. OpenSearch handles hybrid vector + keyword search with filtering. LangFlow provides visual orchestration for both ingestion and generation flows. A fourth component, JVector (a disk-ANN KNN plugin), handles vector indexing with a disk-based architecture so the entire index doesn't need to fit in memory.

- **Hierarchical chunking via DocTags.** Docling doesn't just extract text — it produces an intermediate representation (a "Docling Document") modeled in an XML-ish format called DocTags. The chunker then uses the *hierarchy* encoded in DocTags rather than naive token-based splitting. This is a meaningful architectural choice: structure-aware chunking preserves semantic boundaries that generic chunkers destroy.

- **Dual PDF pipelines (specialized models vs. single VLM).** The standard pipeline uses "a number of small focused models" for layout analysis, table extraction, image extraction. The VLM pipeline uses a single 258M parameter Granite vision model to do it all at once. This tradeoff — specialized pipeline vs. all-in-one model — is the classic accuracy vs. simplicity tension, and offering both is a deliberate hedge.

- **Agentic retrieval over top-K search.** The generation side doesn't embed the query and do nearest-neighbor search. Instead, the query goes to an agent with tools, and the agent decides how many searches to perform and how to use the results. This is a significant architectural upgrade over naive RAG and aligns with the industry trend toward agentic patterns.

- **Air-gap capability.** "Docling itself can be run offline so it can run in air gap situations." The entire stack can be run with local models via Ollama. This is architecturally notable — it means the system is designed for enterprise and government deployments where data cannot leave the premises.

- **LangFlow as the extensibility layer.** The live demo of adding guardrails to the agent by dragging components in LangFlow is the key extensibility play. The speaker frames this as "as extensible as LangFlow can be," which is both a strength and a ceiling — you're limited by what LangFlow supports.

- **MCP server for composability.** OpenRAG exposes an MCP server, meaning it can be a tool for *other* agents. This is a forward-looking pattern — OpenRAG isn't just a standalone app, it's designed to be a node in a larger agentic ecosystem.

## Hidden Signals

- **IBM is positioning itself as the open-source AI infrastructure player.** This isn't just a developer tool demo — it's IBM staking territory. Docling came from IBM Research in Zurich. The Granite models are IBM's. LangFlow integration ties into IBM's broader AI platform play. The framing of "we can do it with open source components out in the open" is a direct competitive positioning against AWS/Azure/GCP proprietary RAG services.

- **Version 0.4.0 — this is early.** The speaker casually mentions the current version is 0.4.0. For a demo at AI Engineer, this is a signal that IBM is in market-testing mode, not production-deployment mode. The ask is explicitly for feedback, stars, and contributions — they're building community before claiming maturity.

- **The "secret fourth project" (JVector).** The speaker calls JVector "a secret fourth open-source project" with a knowing tone. Disk-based ANN indexing (so the whole index doesn't need to fit in memory) is a pragmatic choice that signals they're thinking about production scale — teams that have hit memory walls with HNSW in Elasticsearch/OpenSearch will recognize this immediately.

- **Multi-model vector search as a migration tool.** The speaker notes this "will slow down your vector search in practice but it is useful if you decide you need to migrate your embedding models." This is a feature built from operational pain — IBM has clearly dealt with the embedding migration problem in production and built the escape hatch they wished they'd had.

- **Nudges/suggestions as a design pattern.** The chat UI produces "little nudges" — suggested follow-up questions. This is a LangFlow-powered feature that the speaker highlights briefly. It's a UX pattern borrowed from search engines and chatbots that signals they're thinking about the end-user experience, not just the developer experience.

- **Google OAuth as the auth mechanism.** The cloud connector authentication is "set up with Google OAuth" and requires "an OAuth client and a secret." This is a pragmatic choice but also reveals the target audience: enterprise teams that already have Google Workspace, SharePoint, or OneDrive as their document stores. This is an enterprise product, not a consumer tool.

## What's NOT Said

- **No discussion of evaluation or metrics.** The speaker never mentions how to measure RAG quality — no RAGAS, no faithfulness scores, no relevance metrics, no A/B testing frameworks. For a talk that claims RAG is "hard," the complete absence of evaluation tooling is conspicuous. This suggests either (a) evaluation is still immature in OpenRAG, or (b) the speaker is deliberately avoiding the topic because the answer isn't good yet.

- **No mention of pricing, licensing details, or enterprise support.** It's "open source" but no mention of the specific license (Apache? MIT? Something more restrictive?). No mention of managed hosting or enterprise support tiers. For an IBM product, this is either an oversight or a sign that the business model hasn't been finalized.

- **No discussion of retrieval failure modes.** The demo shows successful retrieval, but there's no discussion of what happens when retrieval fails — hallucination handling, graceful degradation, confidence scoring, or user feedback loops. A mature RAG system needs to handle the "I don't know" case well.

- **No competitive positioning against other RAG frameworks.** No mention of LlamaIndex, Haystack, or other established RAG frameworks. No mention of managed RAG services from cloud providers. The speaker positions against the "RAG is dead/solved" narrative but not against actual competitors. This is either confidence or avoidance.

- **No mention of fine-tuning or domain adaptation.** The talk is entirely about the retrieval and orchestration layer. There's no discussion of fine-tuning embedding models, fine-tuning the LLM on domain data, or RLHF for the retrieval agent. The assumption is that base models + good retrieval is sufficient, which is a notable architectural bet.

- **No discussion of latency or performance benchmarks.** The speaker mentions that adding more models (OCR, image descriptions) "makes things a little slower" but provides no numbers. For a production RAG system, latency is a first-class concern that's completely absent.

- **No mention of security, access control, or document-level permissions.** OpenSearch supports filtering, but there's no discussion of row-level security, document access permissions, or multi-tenancy. For enterprise RAG, this is a critical gap.

## Speaker's Worldview

Phil Nash is a pragmatist who believes the AI industry underestimates the engineering complexity of production RAG systems. He sees the "RAG is solved" narrative as a dangerous oversimplification driven by people who haven't shipped real systems with real documents. His bet is that the winning play is an open-source, extensible baseline that teams customize to their domain — not a proprietary black box. He trusts agents to make smarter retrieval decisions than static pipelines, and he trusts the open-source community to outcompete closed platforms on infrastructural AI tooling.

## Key Quotes

1. *"If every business has less than a million tokens worth of data then sure, rag is dead and probably so are those businesses."* — A sharp dismissal of the context-window-will-solve-everything argument, revealing the speaker's enterprise-first perspective.

2. *"It turns out that rag is actually hard and it's hard for different reasons for different projects."* — The thesis statement of the entire talk. RAG's difficulty isn't one hard problem; it's a combinatorial explosion of domain-specific hard problems.

3. *"Embeddings keep improving which is great for the industry but not very great when you've used something from a 6-month or a year ago."* — An honest articulation of a production pain point that most RAG talks gloss over. Your embedding model choice becomes technical debt.

4. *"The model is actually responsible for deciding what searches to perform and what to do with the results."* — The architectural conviction underneath agentic retrieval. The speaker doesn't trust static top-K to be sufficient — the intelligence must be in the retrieval loop.

5. *"We can do it with open source components out in the open. That's what I'd like to see."* — The closing ideological statement. This isn't just a product pitch; it's a statement about how AI infrastructure should be built and governed.
