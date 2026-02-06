## Diagrama de estado
Esse diagrama apresenta os estados que os dados passam durante o processamento.

Com esse diagrama, podemos entender melhor o ciclo de vida dos dados e como eles são processados em cada etapa.

```mermaid
stateDiagram-v2
    [*] --> Coletado: Dados chegam
    
    Coletado --> Validando: Iniciar validação
    Validando --> Bronze: Validação OK
    Validando --> Erro: Validação falhou
    Erro --> DeadLetter: Mover para DLQ
    DeadLetter --> [*]
    
    Bronze --> Limpeza: ETL Pipeline 1
    Limpeza --> Silver: Dados limpos
    Limpeza --> Erro: Erro na limpeza
    
    Silver --> Agregacao: ETL Pipeline 2
    Agregacao --> Gold: Dados agregados
    Agregacao --> Erro: Erro na agregação
    
    Gold --> Processamento_IA: Enviar para IA
    Processamento_IA --> Pronto: Processado
    Processamento_IA --> Erro: Erro no processamento
    
    Pronto --> Dashboard: Disponível
    Dashboard --> [*]
    
    note right of Bronze
        Layer Bronze:
        - Dados brutos
        - Imutável
        - Formato original
    end note
    
    note right of Silver
        Layer Silver:
        - Dados limpos
        - Schema validado
        - Tipos corretos
    end note
    
    note right of Gold
        Layer Gold:
        - Dados agregados
        - Métricas de negócio
        - Otimizado para queries
    end note
```
