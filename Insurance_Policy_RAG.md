# Insurance Policy Knowledge Retrieval - RAG

## Project Summary
- Built a scalable Retrieval-Augmented Generation (RAG) pipeline to extract precise answers from 500+ page policy documents for customer service.
- Enables faster, consistent responses, reducing manual effort and improving customer satisfaction.

##  System Architecture

```mermaid
graph TB
    %% User Channels
    subgraph "🎯 User Channels"
        WEB[Web App / Chat Widget]
        AGENT[Agent Console]
        API[REST API]
    end

    %% API & Orchestrator
    subgraph "🧩 API & Orchestration"
        GW[API Gateway]
        ORCH[Query Orchestrator - LangChain/LangGraph]
        GUARD[Guardrails - PII/Policy Rules]
        CACHE[Response Cache - Redis]
        OBS[Observability - Prometheus + Logs]
    end

    %% Retrieval Core
    subgraph "🔎 Retrieval Core"
        REWRITE[Query Rewriter - Clarify/Expand]
        HYB[Hybrid Retriever - Vector + BM25]
        RERANK[Cross-Encoder Reranker - bge-reranker/Cohere]
        CTX[Context Builder - Chunk Merge + Windowing]
    end

    %% Knowledge Layer
    subgraph "📚 Knowledge Layer"
        VDB[(Vector DB - Pinecone/pgvector/FAISS)]
        IDX[Index Store - Metadata, Scores]
        STORE[(Object Storage - S3/Blob for PDFs)]
    end

    %% Ingestion Pipeline
    subgraph "📥 Ingestion & Indexing"
        LOAD[Document Loaders - PDF, HTML, DOCX]
        PARSE[Parsing & Cleaning - PyMuPDF, OCR, Tables]
        CHUNK[Semantic Chunking - 1-2k tokens, overlap 10-15%]
        META[Metadata Enrichment - policy_id, section, page, version]
        EMB[Embedding Service - bge-large/text-embedding-3-large]
        UPSERT[Upsert to Vector DB]
    end

    %% Generation Layer
    subgraph "🧠 Generation Layer"
        PROMPT[Prompt Composer - Templates + Instructions]
        LLM[LLM - GPT-4o/Gemini/Llama-3.1]
        CITE[Citation Builder - Section/Page/Score]
        SCORE[Confidence Scoring - Retrieval/Answer]
    end

    %% Feedback & Evaluation
    subgraph "🔁 Feedback, Eval & Governance"
        FT[User Feedback - 👍/👎, edits]
        EVL[Offline Eval - ASQA, Faithfulness, Coverage]
        DRIFT[Drift Monitor - Index freshness]
        SCHED[Scheduler - Re-index, Refresh]
    end

    %% Edges: User -> API
    WEB --> GW
    AGENT --> GW
    API --> GW
    GW --> ORCH
    ORCH --> GUARD
    ORCH --> CACHE
    ORCH --> REWRITE

    %% Retrieval flow
    REWRITE --> HYB
    HYB --> RERANK
    RERANK --> CTX
    CTX --> PROMPT

    %% Generation
    PROMPT --> LLM
    LLM --> CITE
    CITE --> SCORE
    SCORE --> ORCH
    ORCH --> CACHE

    %% Knowledge connections
    HYB --> VDB
    HYB --> IDX
    CTX --> VDB

    %% Ingestion connections
    LOAD --> PARSE --> CHUNK --> META --> EMB --> UPSERT --> VDB
    PARSE --> STORE

    %% Feedback loop
    ORCH --> FT
    FT --> EVL
    EVL --> SCHED
    SCHED --> LOAD
    DRIFT --> SCHED

    %% Observability
    ORCH --> OBS
    HYB --> OBS
    LLM --> OBS

    %% Styling
    classDef api fill:#eef7ff,stroke:#1e88e5,stroke-width:2px
    classDef ret fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px
    classDef kn fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef inj fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef gen fill:#e0f7fa,stroke:#006064,stroke-width:2px
    classDef fb fill:#fce4ec,stroke:#ad1457,stroke-width:2px

    class GW,ORCH,GUARD,CACHE,OBS api
    class REWRITE,HYB,RERANK,CTX ret
    class VDB,IDX,STORE kn
    class LOAD,PARSE,CHUNK,META,EMB,UPSERT inj
    class PROMPT,LLM,CITE,SCORE gen
    class FT,EVL,DRIFT,SCHED fb
```

## Query Flow (RAG)

```mermaid
sequenceDiagram
    participant U as User
    participant GW as API Gateway
    participant OR as Orchestrator
    participant RW as Query Rewriter
    participant HY as Hybrid Retriever
    participant RR as Reranker
    participant CT as Context Builder
    participant LM as LLM

    U->>GW: Ask question
    GW->>OR: Forward request
    OR->>RW: Rewrite/clarify
    RW-->>OR: Improved query
    OR->>HY: Retrieve (Vector + BM25)
    HY-->>OR: Top-k candidates
    OR->>RR: Cross-encode rerank
    RR-->>OR: Re-ranked passages
    OR->>CT: Build context (merge/window)
    CT-->>OR: Context + citations
    OR->>LM: Compose prompt + context
    LM-->>OR: Answer + citations
    OR-->>GW: Response (answer, sources, confidence)
    GW-->>U: Display with citations
```

## Implementation Notes
- Chunking: 1,000–1,500 tokens, 10–15% overlap; section-aware; table capture where possible.
- Hybrid retrieval: Reciprocal Rank Fusion (RRF) of vector and BM25 results; rerank top 50 to top 8.
- Metadata: `policy_id`, `section_id`, `page`, `version`, `effective_date`, `jurisdiction`.
- Guardrails: PII redaction, policy scope constraints, refusal for out-of-scope queries.
- Caching: cache key = normalized_query + policy_scope + version; TTL 12–24h.
- Citations: include section title + page range + similarity score; clickable deep links.
- Evaluation: faithfulness (LLM-as-judge + string entailment), coverage, latency budgets.
- Re-indexing: scheduled on new policy versions; maintain active+shadow indexes for zero-downtime swaps.

## Suggested Stack
- Loaders: PyMuPDF, Unstructured.io
- Embeddings: `bge-large-en`, `text-embedding-3-large`, or `E5-large` via TEI
- Vector DB: pgvector (Postgres), Pinecone, or FAISS (local)
- Reranker: `bge-reranker-large` / Cohere Rerank
- Orchestration: LangChain/LangGraph, FastAPI
- LLM: GPT-4o / Gemini / Llama 3.1 (via vLLM for cost control)
- Infra: Redis cache, S3/Blob storage, Prometheus/Grafana, Docker

## Outcomes
- Precise answers from 500+ page policies with citations
- Faster, consistent customer support, reduced manual workload
- Auditable responses with source grounding and confidence scores 
