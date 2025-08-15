# Shipping Reconciliation Automation

```mermaid
graph TB
    subgraph "📊 Data Sources"
        SHIP[Shipping Bills]
        INVOICE[Invoice Data]
        RATES[Rate Cards]
    end

    subgraph "☁️ AWS Infrastructure"
        S3[S3 Storage]
        LAMBDA[Lambda Functions]
        ECS[ECS Fargate]
        RDS[(RDS Database)]
        DYNAMO[(DynamoDB)]
    end

    subgraph "🔄 Processing Pipeline"
        INGEST[Data Ingestion]
        CLEAN[Data Cleaning]
        ENRICH[Data Enrichment]
    end

    subgraph "🧠 LLM Processing"
        PROMPT[Dynamic Prompt Engine]
        LLM[LLM Service - GPT-4/Claude]
        REASONING[Reasoning Engine]
        DECISION[Decision Engine]
    end

    subgraph "⚖️ Reconciliation"
        MATCHER[Smart Matcher]
        RULES[Business Rules]
        RESOLUTION[Auto-Resolution]
        AUDIT[Audit Trail]
    end

    subgraph "📈 Output"
        REPORTS[Reconciliation Reports]
        DASHBOARD[Management Dashboard]
        API[REST API]
    end

    SHIP --> S3
    INVOICE --> S3
    RATES --> S3

    S3 --> INGEST --> CLEAN --> ENRICH
    ENRICH --> PROMPT --> LLM --> REASONING --> DECISION
    DECISION --> MATCHER --> RULES --> RESOLUTION --> AUDIT
    AUDIT --> REPORTS --> DASHBOARD
    AUDIT --> API

    INGEST --> LAMBDA
    LAMBDA --> ECS
    ECS --> RDS
    ECS --> DYNAMO

    classDef dataClass fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    classDef awsClass fill:#ffebee,stroke:#c62828,stroke-width:2px
    classDef processClass fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
    classDef llmClass fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px
    classDef reconClass fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef outputClass fill:#e0f2f1,stroke:#00695c,stroke-width:2px

    class SHIP,INVOICE,RATES dataClass
    class S3,LAMBDA,ECS,RDS,DYNAMO awsClass
    class INGEST,CLEAN,ENRICH processClass
    class PROMPT,LLM,REASONING,DECISION llmClass
    class MATCHER,RULES,RESOLUTION,AUDIT reconClass
    class REPORTS,DASHBOARD,API outputClass
```

- **Goal**: Automate reconciliation of thousands of shipping bills using LLM-powered logic
- **Core**: Dynamic prompt layer + LLM reasoning + business rules + automated resolution
- **Stack**: AWS (Lambda, ECS, S3, RDS, DynamoDB), Python, GPT-4/Claude, LangChain
- **Benefits**: 10x faster processing, 95% automation, 70% cost reduction, real-time visibility 
