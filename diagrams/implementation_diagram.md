## Diagrama de implementação

Esse diagrama apresenta a implementação da arquitetura em um ambiente de nuvem.

Com a implementação em nuvem, podemos ter uma arquitetura escalável e resiliente.

LEMBRANDO QUE ESSA ARQUITETURA É APENAS UM EXEMPLO, PODENDO SER ADAPTADA PARA OUTRAS NUVENS E ESTÁ SUJEITA A MUDANÇAS.

```mermaid
graph TB
    subgraph "Cloud Provider - AWS/Azure/GCP"
        subgraph "Container Orchestration - Kubernetes"
            subgraph "Namespace: Ingestion"
                POD1[API Scraper Pods<br/>Replicas: 3]
                POD2[Web Scraper Pods<br/>Replicas: 2]
            end
            
            subgraph "Namespace: Processing"
                POD3[Consumer Pods<br/>Replicas: 5]
                POD4[ETL Worker Pods<br/>Replicas: 3]
            end
            
            subgraph "Namespace: AI"
                POD5[ML Service Pods<br/>GPU Enabled]
                POD6[LLM Service Pods<br/>High Memory]
            end
            
            subgraph "Namespace: Application"
                POD7[API Backend Pods<br/>Replicas: 4]
                POD8[Dashboard Pods<br/>Replicas: 3]
            end
        end
        
        subgraph "Managed Services"
            MQ[RabbitMQ/Kafka<br/>Managed Service]
            S3[Object Storage<br/>S3/Blob Storage]
            DB[(PostgreSQL<br/>Managed DB)]
            REDIS[(Redis Cache)]
        end
        
        subgraph "Monitoring Stack"
            PROM[Prometheus]
            GRAF[Grafana]
            ELK[ELK Stack]
        end
    end
    
    subgraph "CDN & Load Balancer"
        LB[Load Balancer]
        CDN[CloudFront/CDN]
    end
    
    Internet([Internet]) --> LB
    LB --> POD7
    LB --> POD8
    CDN --> POD8
    
    POD1 --> MQ
    POD2 --> MQ
    MQ --> POD3
    POD3 --> S3
    POD3 --> DB
    POD4 --> S3
    S3 --> POD5
    S3 --> POD6
    POD5 --> DB
    POD6 --> DB
    POD7 --> DB
    POD7 --> REDIS
    POD7 --> S3
    
    POD1 -.-> PROM
    POD3 -.-> PROM
    POD7 -.-> PROM
    PROM --> GRAF
    POD1 -.-> ELK
    POD3 -.-> ELK
    POD7 -.-> ELK
```

