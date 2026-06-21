# Data Lake e Data Lakehouse

Aprender na prática a diferença entre um **Data Lake** (Parquet puro) e um **Data Lakehouse** (Delta Table), usando o mesmo dataset e as mesmas transformações, permitindo comparar as abordagens lado a lado.

# Dataset

**Online Retail Dataset** — Transações de uma loja de varejo online do Reino Unido (2010-2011).

- **Fonte**: [Kaggle](https://www.kaggle.com/datasets/vijayuv/onlineretail)
- **Registros**: ~541.000 transações
- **Formato original**: CSV (incluído em `data/raw/`)

# Arquitetura

```
data/
├── raw/                    ← CSV original
├── lake/                   ← Data Lake
│   ├── 01_bronze/          ← Dados brutos em Parquet
│   ├── 02_silver/          ← Dados limpos
│   └── 03_gold/            ← Agregações para BI
└── lakehouse/              ← Data Lakehouse
    ├── 01_bronze/          ← Dados brutos em Delta Table
    ├── 02_silver/          ← Dados limpos
    └── 03_gold/            ← Agregações para BI
```

# Como Rodar

### 1. Instalar dependências

```bash
uv sync
```

### 2. Executar os pipelines

```bash
# Pipeline Data Lake (Parquet)
uv run src/lake_pipeline.py

# Pipeline Data Lakehouse (Delta Table)
uv run src/lakehouse_pipeline.py
```

Ou execute cada camada individualmente:

```bash
# Data Lake
uv run src/lake_01_bronze.py
uv run src/lake_02_silver.py
uv run src/lake_03_gold.py

# Data Lakehouse
uv run src/lakehouse_01_bronze.py
uv run src/lakehouse_02_silver.py
uv run src/lakehouse_03_gold.py
```

### 3. Abrir o Dashboard

```bash
uv run streamlit run src/app.py
```
## 📁 Estrutura do Projeto

```
├── data/
│   └── raw/OnlineRetail.csv
├── notebooks/
│   ├── lake_explorations.ipynb
│   └── lakehouse_exploration.ipynb
├── src/
│   ├── lake_01_bronze.py         # CSV → Parquet
│   ├── lake_02_silver.py         # Limpeza + Star Schema (Parquet)
│   ├── lake_03_gold.py           # Agregações (Parquet)
│   ├── lake_pipeline.py          # Executa todo o pipeline Lake
│   ├── lakehouse_01_bronze.py    # CSV → Delta Table
│   ├── lakehouse_02_silver.py    # Limpeza + Star Schema (Delta)
│   ├── lakehouse_03_gold.py      # Agregações (Delta)
│   ├── lakehouse_pipeline.py     # Executa todo o pipeline Lakehouse
│   └── app.py                    # Dashboard Streamlit
├── pyproject.toml
└── README.md
```
