## Diagrama de componentes
Esse diagrama apresenta os componentes do projeto e como eles irão interagir entre si.

Com a possiblidade de escalar o projeto, podemos adicionar mais componentes em cada camada.

```mermaid
graph LR
    subgraph "Data Ingestion Layer"
        A1[API Collector Service]
        A2[Web Scraper Service]
        A3[Scheduler Cron Jobs]
    end
    
    subgraph "Message Queue Layer"
        B1[RabbitMQ/Kafka Broker]
        B2[Dead Letter Queue]
        B3[Retry Mechanism]
    end
    
    subgraph "Processing Layer"
        C1[Consumer Workers]
        C2[Data Validator]
        C3[Data Transformer]
        C4[Data Enricher]
    end
    
    subgraph "Storage Layer - Medallion Architecture"
        D1[Bronze - Raw Data<br/>Formato Original]
        D2[Silver - Cleaned Data<br/>Schema Validado]
        D3[Gold - Business Data<br/>Agregado & Otimizado]
    end
    
    subgraph "AI/ML Layer"
        E1[Image Recognition<br/>Reconhecer Raças]
        E2[Text Analysis<br/>NLP Descrições]
        E3[Matching Algorithm<br/>Pets Perdidos x Encontrados]
        E4[Report Generator<br/>LLM para Relatórios]
    end
    
    subgraph "Application Layer"
        F1[REST API]
        F2[WebSocket Server]
        F3[Background Jobs]
    end
    
    subgraph "Presentation Layer"
        G1[Dashboard Web]
        G2[Mobile App]
        G3[Admin Panel]
    end
    
    subgraph "Monitoring & Observability"
        H1[Prometheus/Grafana]
        H2[ELK Stack Logs]
        H3[Alert Manager]
    end
    
    A1 --> B1
    A2 --> B1
    A3 --> A1
    A3 --> A2
    B1 --> C1
    B1 --> B2
    B2 --> B3
    B3 --> B1
    C1 --> C2
    C2 --> C3
    C3 --> C4
    C4 --> D1
    D1 --> D2
    D2 --> D3
    D3 --> E1
    D3 --> E2
    D3 --> E3
    D3 --> E4
    E1 --> F1
    E2 --> F1
    E3 --> F1
    E4 --> F1
    F1 --> G1
    F1 --> G2
    F1 --> G3
    F2 --> G1
    F3 --> E4
    
    C1 --> H2
    F1 --> H1
    H1 --> H3
```