#  QA Buddy - LLM-Powered Test Automation (Profile Version)

```mermaid
graph TB
    subgraph "📋 Input Sources"
        REQ[Requirements]
        UI_SPEC[UI Specifications]
        API_SPEC[API Specs]
    end

    subgraph "📄 Processing"
        PARSER[Document Parser]
        OCR[OCR Engine]
        EXTRACTOR[Content Extractor]
    end

    subgraph "🖥️ UI Analysis"
        SCREENSHOT[UI Capture]
        ELEMENT_DETECTOR[Element Detection]
        FIELD_EXTRACTOR[Field Extraction]
    end

    subgraph "🧠 LLM Processing"
        CONTEXT_BUILDER[Context Builder]
        PROMPT_ENGINE[Dynamic Prompts]
        LLM_SERVICE[LLM Service - GPT-4/Claude]
        TEST_GENERATOR[Test Generator]
    end

    subgraph "⚙️ Test Generation"
        FRAMEWORK_ADAPTER[Framework Adapter]
        TEST_TEMPLATE[Test Templates]
        ASSERTION_BUILDER[Assertions]
    end

    subgraph "🚀 Execution"
        BROWSER_DRIVER[Browser Testing]
        MOBILE_EMULATOR[Mobile Testing]
        API_CLIENT[API Testing]
    end

    subgraph "📊 Reporting"
        TEST_REPORTS[Test Reports]
        COVERAGE_DASHBOARD[Coverage Dashboard]
        EXPORT_ENGINE[Export Engine]
    end

    REQ --> PARSER
    UI_SPEC --> PARSER
    API_SPEC --> PARSER

    PARSER --> OCR --> EXTRACTOR
    EXTRACTOR --> SCREENSHOT --> ELEMENT_DETECTOR --> FIELD_EXTRACTOR

    EXTRACTOR --> CONTEXT_BUILDER
    FIELD_EXTRACTOR --> CONTEXT_BUILDER

    CONTEXT_BUILDER --> PROMPT_ENGINE --> LLM_SERVICE --> TEST_GENERATOR
    TEST_GENERATOR --> FRAMEWORK_ADAPTER --> TEST_TEMPLATE --> ASSERTION_BUILDER

    ASSERTION_BUILDER --> BROWSER_DRIVER
    ASSERTION_BUILDER --> MOBILE_EMULATOR
    ASSERTION_BUILDER --> API_CLIENT

    BROWSER_DRIVER --> TEST_REPORTS
    MOBILE_EMULATOR --> TEST_REPORTS
    API_CLIENT --> TEST_REPORTS

    TEST_REPORTS --> COVERAGE_DASHBOARD --> EXPORT_ENGINE

    classDef inputClass fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    classDef processClass fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
    classDef uiClass fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef llmClass fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px
    classDef testClass fill:#fce4ec,stroke:#ad1457,stroke-width:2px
    classDef execClass fill:#e0f2f1,stroke:#00695c,stroke-width:2px
    classDef reportClass fill:#f1f8e9,stroke:#558b2f,stroke-width:2px

    class REQ,UI_SPEC,API_SPEC inputClass
    class PARSER,OCR,EXTRACTOR processClass
    class SCREENSHOT,ELEMENT_DETECTOR,FIELD_EXTRACTOR uiClass
    class CONTEXT_BUILDER,PROMPT_ENGINE,LLM_SERVICE,TEST_GENERATOR llmClass
    class FRAMEWORK_ADAPTER,TEST_TEMPLATE,ASSERTION_BUILDER testClass
    class BROWSER_DRIVER,MOBILE_EMULATOR,API_CLIENT execClass
    class TEST_REPORTS,COVERAGE_DASHBOARD,EXPORT_ENGINE reportClass
```

- **Goal**: Generate comprehensive test scripts from requirements and UI analysis using LLM
- **Core**: Document parsing + UI analysis + LLM processing + test generation + execution
- **Stack**: Python, FastAPI, GPT-4/Claude, Selenium/Playwright, Docker, PostgreSQL
- **Benefits**: 10x faster test creation, 95% automation, 80% maintenance reduction, 60% faster cycles 
