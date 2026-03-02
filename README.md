
```mermaid
graph LR
    subgraph "Legacy Enterprise Layer (DBA Roots)"
        DB[(Oracle / PeopleSoft)] -- "JDBC / Change Data Capture" --> Glue
    end

    subgraph "Feature Engineering & Transformation"
        Glue[AWS Glue / PySpark] -- "Clean & Aggregate" --> FS
    end

    subgraph "MLOps Feature Store"
        direction TB
        FS{Feature Store}
        Offline[(Offline Store: Amazon S3)]
        Online[(Online Store: Redis / DynamoDB)]
        DVC[[DVC: Data Versioning]]
        
        FS --> Offline
        FS --> Online
        Offline -.-> DVC
    end

    subgraph "Production Inference"
        Model[SageMaker Inference Endpoint] -- "Low-Latency Fetch" --> Online
        Offline -- "Batch Training" --> Training[Model Training Pipeline]
    end

    style DB fill:#f9f,stroke:#333,stroke-width:2px
    style FS fill:#00c2ff,stroke:#333,stroke-width:2px
    style Online fill:#ff4b4b,stroke:#333
    style DVC fill:#945dd6,stroke:#333
