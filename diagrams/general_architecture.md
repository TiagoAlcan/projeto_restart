## Arquitetura Geral

Essa arquitetura é uma solução de software que visa resolver o problema de animais perdidos e encontrados.

O foco do projeto é o suporte a busca de animais perdidos e encontrados, com o foco em IA, análise de dados, web scraping e visualização de dados.

```mermaid
graph LR
    subgraph "Camada de Ingestão"
        API[APIs Externas<br/>- PetFinder<br/>- Redes Sociais]
        WS[Web Scrapers<br/>- Sites de Pets<br/>- Fóruns<br/>- Grupos Facebook]
    end
    
    subgraph "Camada de Mensageria"
        MQ[Sistema de Filas<br/>RabbitMQ/Kafka]
    end
    
    subgraph "Camada de Processamento - Data Lake"
        BRONZE[(Bronze Layer<br/>Dados Brutos)]
        SILVER[(Silver Layer<br/>Dados Limpos)]
        GOLD[(Gold Layer<br/>Dados Agregados)]
    end
    
    subgraph "Camada de Inteligência"
        AI[IA - LLM<br/>Análise e Relatórios]
        ML[ML Engine<br/>Matching de Pets]
    end
    
    subgraph "Camada de Apresentação"
        DASH[Dashboard Interativo<br/>Visualizações]
        REPORT[Sistema de Relatórios<br/>Gerados por IA]
    end
    
    subgraph "Usuários"
        OWNER[Donos de Pets]
        FINDER[Pessoas que<br/>encontraram pets]
    end
    
    API --> MQ
    WS --> MQ
    MQ --> BRONZE
    BRONZE --> SILVER
    SILVER --> GOLD
    GOLD --> AI
    GOLD --> ML
    AI --> DASH
    ML --> DASH
    AI --> REPORT
    DASH --> OWNER
    DASH --> FINDER
    REPORT --> OWNER
```

