## Diagrama de sequência
Esse diagrama apresenta a sequência de eventos que ocorrem no projeto.

Com a sequencia lógica de eventos, podemos entender melhor o funcionamento do projeto e onde poderão haver quebras na execução.

```mermaid
sequenceDiagram
    participant Scheduler
    participant APIScraper
    participant Queue as RabbitMQ/Kafka
    participant Consumer
    participant Bronze
    participant Silver
    participant Gold
    participant AI
    participant Dashboard
    
    Scheduler->>APIScraper: Trigger coleta (a cada X minutos)
    APIScraper->>APIScraper: Buscar dados de APIs/Sites
    APIScraper->>Queue: Publicar mensagem com dados brutos
    
    Queue->>Consumer: Consumer recebe mensagem
    Consumer->>Consumer: Validar formato inicial
    Consumer->>Bronze: Salvar dados brutos (JSON/Parquet)
    
    Note over Bronze: Dados armazenados exatamente<br/>como recebidos da fonte
    
    Bronze->>Consumer: Trigger ETL Pipeline
    Consumer->>Consumer: Limpar dados (remover duplicatas,<br/>normalizar campos, validar schema)
    Consumer->>Silver: Salvar dados limpos (Parquet)
    
    Note over Silver: Dados validados e padronizados,<br/>prontos para uso
    
    Silver->>Consumer: Trigger Aggregation Pipeline
    Consumer->>Consumer: Agregar, enriquecer,<br/>criar métricas de negócio
    Consumer->>Gold: Salvar dados finais (Delta/Parquet)
    
    Note over Gold: Dados otimizados para queries<br/>e análises de negócio
    
    Gold->>AI: Enviar dados para processamento IA
    AI->>AI: Gerar insights, relatórios,<br/>matching de pets
    AI->>Dashboard: Atualizar dashboard com novos dados
    Dashboard->>Dashboard: Renderizar visualizações
```