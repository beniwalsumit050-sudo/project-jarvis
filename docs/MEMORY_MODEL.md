# Project JARVIS — Memory Architecture Model

This document outlines the multi-tiered memory architecture for Project JARVIS, detailing how information is ingested, structured, indexed, retrieved, and consolidated across sessions.

---

## 1. Multi-Tier Memory Hierarchy

JARVIS uses a four-tier memory model inspired by human cognitive systems and modern AI retrieval architectures:

```
+-------------------------------------------------------------------------+
|                        TIER 1: WORKING MEMORY                           |
|  - Active prompt context window                                         |
|  - Current turn scratchpad & intermediate tool traces                   |
|  - Ephemeral task plan & step status                                    |
+-------------------------------------------------------------------------+
                                    |
                                    v
+-------------------------------------------------------------------------+
|                        TIER 2: EPISODIC MEMORY                          |
|  - Full chronological session transcripts                               |
|  - Command execution logs and tool call outcomes                        |
|  - Interaction timestamps and session markers                           |
+-------------------------------------------------------------------------+
                                    |
                                    v
+------------------------------------+------------------------------------+
|                                    |                                    |
|   TIER 3: SEMANTIC MEMORY          |    TIER 4: ENTITY & FACT GRAPH     |
|   - Vector embeddings (Chunks)     |    - Key-value user preferences    |
|   - Indexed local documentation    |    - Structured entities & links   |
|   - Codebase & note snippets       |    - People, projects, credentials |
+------------------------------------+------------------------------------+
```

---

## 2. Tier Details & Specifications

### 2.1 Tier 1: Working Memory (In-Memory / Context Window)
- **Scope**: Current session turn and active multi-step reasoning loop.
- **Components**:
  - System instruction & persona definition.
  - Active conversation history (sliding window).
  - Dynamic tool definitions (filtered to relevant tools).
  - Scratchpad for multi-step reasoning (Plan-Act-Observe trace).
- **Lifecycle**: Purged or summarized upon task completion.

### 2.2 Tier 2: Episodic Memory (Session Records)
- **Scope**: Historical conversation sessions and chronological events.
- **Storage**: Append-only SQLite or structured JSONL files.
- **Fields**:
  - `session_id`, `message_id`, `timestamp`, `role`, `content`, `tool_calls`, `tool_results`.
- **Purpose**: Enables session replay, debugging, audit trails, and offline memory distillation.

### 2.3 Tier 3: Semantic Memory (Vector Store & RAG)
- **Scope**: Unstructured knowledge, past Q&A pairs, reference files, indexed user notes.
- **Storage**: Local embedded vector database (e.g., ChromaDB / LanceDB / Qdrant).
- **Indexing Strategy**:
  - Chunk size: ~500–1000 tokens with 10% overlap.
  - Metadata: `source_uri`, `timestamp`, `category`, `importance_score`.
  - Embeddings: High-efficiency local or API-based embedding models.
- **Search Method**: Hybrid search (Cosine similarity + BM25 keyword matching) with Reciprocal Rank Fusion (RRF).

### 2.4 Tier 4: Entity & Fact Graph (Structured Knowledge)
- **Scope**: Stable user facts, preferences, project metadata, and entity relationships.
- **Storage**: SQLite relational schema or key-value document store.
- **Examples**:
  - `user.timezone = "Asia/Kolkata"`
  - `user.preferred_editor = "VS Code"`
  - `entity(Project JARVIS).path = "c:\Users\ravim\project-jarvis"`
  - `relationship(User -> works_on -> Project JARVIS)`

---

## 3. Memory Pipeline Lifecycle

```
[User Input / Tool Result]
            │
            ▼
┌───────────────────────┐
│ 1. Working Memory     │ (Added to active context)
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│ 2. Episodic Store     │ (Persisted to session database)
└───────────┬───────────┘
            │
      (Background / Async)
            │
      ┌─────┴─────────────────────────────────┐
      ▼                                       ▼
┌───────────────────────┐           ┌───────────────────────┐
│ 3. Semantic Ingestion │           │ 4. Fact Extraction    │
│ - Chunking            │           │ - LLM fact extractor  │
│ - Embedding           │           │ - Entity relationship │
│ - Vector Indexing     │           │ - Upsert to Graph/DB  │
└───────────────────────┘           └───────────────────────┘
```

---

## 4. Context Assembly & Budget Management

When formulating an LLM request, the **Context Assembler** enforces a strict token budget:

1. **Static System Base** (~10% budget): System prompt, tool schemas, safety constraints.
2. **Entity & Preference Facts** (~10% budget): Retrieved relevant facts and active user profile.
3. **Retrieved Semantic Chunks** (~30% budget): Top-k relevant memories and search excerpts.
4. **Recent Conversation Turns** (~30% budget): Recent N turns from working memory.
5. **Generation Headroom** (~20% budget): Reserved for model response and tool arguments.

---

## 5. Memory Privacy & Governance

- **Local Storage**: All memory indices and databases reside in the local application data directory.
- **Explicit Erasure**: Commands such as `/forget <topic>` or `/clear-history` remove associated vector records and entity nodes.
- **Exclusion Rules**: Configurable file patterns (e.g., `.env`, `*.pem`, `credentials.json`) are strictly ignored by semantic indexers.
