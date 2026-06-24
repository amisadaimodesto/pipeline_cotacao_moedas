# Hub de Inteligência Cambial: Plataforma USD/BRL 📈

Pipeline de dados ponta a ponta (End-to-End) focado na captura, armazenamento, transformação e visualização em tempo real da cotação do Dólar Americano comercial (USD-BRL). 

O projeto adota a arquitetura modern data stack moderna sob o conceito de **ELT**, integrando dados intradiários de alta frequência com dados oficiais do Banco Central, garantindo resiliência na ingestão, governança nas transformações e alta performance na camada de BI.

## 🛠️ Stack Tecnológica
* ![WSL](https://img.shields.io/badge/WSL-0078D4?style=for-the-badge&logo=windows&logoColor=white) - **Ambiente de Desenvolvimento**
* ![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black) - **Sistema Operacional**
* ![API](https://img.shields.io/badge/API-REST-orange?style=for-the-badge) - **Fonte de Ingestão de Dados**
* ![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-017CE1?style=for-the-badge&logo=Apache%20Airflow&logoColor=white) - **Orquestração**
* ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) - **Linguagem Principal / Ingestão**
* ![PostgreSQL](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) - **Banco de Dados / Data Warehouse**
* ![dbt](https://img.shields.io/badge/dbt-FF694B?style=for-the-badge&logo=dbt&logoColor=white) - **Transformação e Modelagem (ELT)**
* ![Apache Superset](https://img.shields.io/badge/Apache%20Superset-007A87?style=for-the-badge&logo=apache-superset&logoColor=white) - **Visualização de Dados (BI)**

## 🏗️ Arquitetura e Fluxo de Dados (ELT)

```text
plataforma-usd-brl/
├── 🌐 Fontes (APIs)
│   ├── Banco Central (Bacen - Dados Diários)
│   └── AwesomeAPI (Intraday - Alta Frequência)
├── 🚀 Orquestração (Apache Airflow)
│   └── Processo: Extract & Load via Python Requests
├── 🗄️ Banco de Dados (PostgreSQL)
│   ├── 📥 Camada Raw / Staging (Dados Brutos)
│   └── 📤 Camada Analytics / Marts (Dados Modelados)
├── ⚙️ Transformação (dbt)
│   └── Componentes: Models, Truncate & Materializations
└── 📊 Consumo / BI (Apache Superset)
    └── Componente: Dashboards, KPI Cards & Sparklines

### 1. Ingestão e Orquestração (Apache Airflow)
O pipeline é gerenciado por esteiras autônomas com frequências distintas no Airflow, responsáveis por extrair os dados brutos e carregá-los direto no PostgreSQL:
* **Esteira Intraday (AwesomeAPI):** Captura a variação cambial a cada 15 minutos (`*/15 * * * *`) durante o pregão.
* **Esteira de Fechamento (Bacen):** Extrai o dado consolidado da PTAX ao final do dia (`0 19 * * *`).

### 2. Transformação e Modelagem de Dados (dbt)
Após a carga dos dados brutos (*raw data*), o **dbt** assume o papel de higienização, centralização das regras de negócio e modelagem analítica. 
* **Camada Staging:** Limpeza de tipos, tratamento de timestamps e renomeação de colunas.
* **Camada Analytics / Marts:** Criação de tabelas e views otimizadas para o BI. Modelos como `bacen_minimas_mensais` e `bacen_variacao_dolar` foram desenvolvidos no dbt para calcular janelas históricas e agregações pesadas direto no banco de dados, poupando poder de processamento da camada de visualização.

### 3. Resiliência e Tolerância a Falhas
Visando a estabilidade contra instabilidades de APIs de terceiros (como erros `HTTP 500 Internal Server Error` no Bacen), a esteira foi blindada utilizando a estratégia de **Exponential Backoff** no Airflow:
```python
default_args = {
    'retries': 10,
    'retry_delay': timedelta(minutes=1),
    'retry_exponential_backoff': True,
    'max_retry_delay': timedelta(hours=1),
}

###Camada de Consumo e Negócio (Apache Superset)
O Dashboard atua como um terminal financeiro executivo alimentado diretamente pelas tabelas modeladas pelo dbt:

KPI Cards (Big Numbers): Valor atual (Tempo Real), Máxima e Mínima do dia atualizados dinamicamente com filtros customizados em SQL para expurgar ruídos de madrugadas, focando estritamente no horário comercial (10:00:00 às 19:00:00).

Gráficos de Linha Dinâmicos: Análise detalhada da performance diária (%d/%m) ajustada para exibir a evolução exata do dólar dentro do mês corrente.
