## Diagrama do fluxo de dados
Esse diagrama representa o fluxo de dados do projeto.

Com essa arquitetura, podemos ter uma visão macro do projeto e entender como os dados fluem entre os componentes e como podemos implementar e, no futuro, escalar o projeto.

```mermaid
flowchart TD
    Start([Início do Pipeline]) --> Collect{Tipo de Fonte}
    
    Collect -->|API| API1[Chamada API<br/>PetFinder, etc]
    Collect -->|Scraping| WS1[Web Scraper<br/>BeautifulSoup/Selenium]
    
    API1 --> Validate1[Validação Inicial<br/>Schema/Format]
    WS1 --> Validate1
    
    Validate1 --> Queue[Publicar na Fila<br/>RabbitMQ/Kafka Topic]
    
    Queue --> Consumer[Consumer Service<br/>Processa Mensagens]
    
    Consumer --> Bronze{Armazenar<br/>Bronze Layer}
    Bronze -->|Parquet/JSON| S3Bronze[(S3/MinIO<br/>Dados Brutos)]
    
    S3Bronze --> ETL1[ETL Pipeline 1<br/>Limpeza de Dados]
    
    ETL1 --> Silver{Armazenar<br/>Silver Layer}
    Silver -->|Parquet| S3Silver[(S3/MinIO<br/>Dados Limpos)]
    
    S3Silver --> ETL2[ETL Pipeline 2<br/>Agregação & Enriquecimento]
    
    ETL2 --> Gold{Armazenar<br/>Gold Layer}
    Gold -->|Parquet/Delta| S3Gold[(S3/MinIO<br/>Dados Prontos)]
    
    S3Gold --> Analytics[Camada Analítica]
    
    Analytics --> ML[ML Processing<br/>- Matching Pets<br/>- Detecção Padrões]
    Analytics --> AIProc[IA Processing<br/>- Geração Relatórios<br/>- Resumos]
    
    ML --> DB[(Database<br/>PostgreSQL/MongoDB)]
    AIProc --> DB
    
    DB --> API2[API Backend<br/>FastAPI/Node.js]
    
    API2 --> Dashboard[Dashboard<br/>React/Vue/Streamlit]
    
    Dashboard --> User([Usuário Final])
    
    style Bronze fill:#cd7f32
    style Silver fill:#c0c0c0
    style Gold fill:#ffd700
```

