# Projeto de IA para busca de animais perdidos e encontrados


## Estrutura de Pastas Sugerida (MVP)

Esta estrutura foi desenhada para ser **realista para um início de projeto**, mantendo a modularidade necessária para crescer.

### Por que essa separação?

1.  **Isolamento de Responsabilidades:**
    *   **`src/`**: Contém todo o código-fonte executável, separado por domínio (API, Scrapers, Dashboard, ML). Isso facilita que diferentes pessoas trabalhem em diferentes partes sem conflitos.
    *   **`notebooks/`**: Essencial para projetos de dados. É onde a exploração e os experimentos de IA acontecem antes de virarem código de produção.
    *   **`data/`**: Mantém os dados locais organizados, simulando as camadas do Data Lake (Bronze/Silver/Gold) sem precisar de infraestrutura complexa na nuvem logo de cara.

2.  **Simplicidade Operacional:**
    *   Em vez de múltiplos repositórios ou configurações complexas de Kubernetes, usamos um `docker-compose.yml` na raiz para subir todo o ambiente de desenvolvimento com um comando.

### Estrutura de Diretórios

```
project_restart/
│
├── src/                    # Código-fonte principal
│   ├── api/                # Backend (FastAPI/Express)
│   │   ├── routes/
│   │   └── main.py
│   │
│   ├── scrapers/           # Robôs de coleta de dados
│   │   ├── spiders/        # Lógica de extração
│   │   └── pipelines/      # Processamento inicial
│   │
│   ├── ml_engine/          # Núcleo de Inteligência Artificial
│   │   ├── models/         # Modelos treinados (.pkl, .h5)
│   │   └── processors/     # Lógica de inferência e matching
│   │
│   └── dashboard/          # Frontend (Streamlit/React)
│
├── notebooks/              # Área de experimentação (Jupyter)
│   ├── 01_exploration/     # Análise exploratória
│   └── 02_modeling/        # Treinamento de modelos
│
├── data/                   # Simulação de Data Lake local
│   ├── raw/                # Dados brutos (Bronze)
│   ├── processed/          # Dados limpos (Silver)
│   └── curated/            # Dados finais para consumo (Gold)
│
├── infra/                  # Configurações de infraestrutura
│   ├── docker/             # Dockerfiles específicos
│   └── seeds/              # Dados iniciais para banco de dados
│
├── tests/                  # Testes automatizados
│
├── .env.example            # Exemplo de variáveis de ambiente
├── docker-compose.yml      # Orquestração local dos serviços
└── README.md               # Documentação geral
```