# Hub de Inteligência Cambial: Plataforma USD/BRL 📈

Pipeline de dados ponta a ponta (End-to-End) focado na captura, processamento, resiliência e visualização em tempo real da cotação do Dólar Americano comercial (USD-BRL). 

O projeto simula um ambiente de produção real, integrando dados intradiários de alta frequência com dados oficiais consolidados de fechamento diário, focado em governança de dados e tolerância a falhas.

## 🛠️ Stack Tecnológica
* **Orquestração:** Apache Airflow
* **Linguagem Principal:** Python (Requests, Logging)
* **Banco de Dados:** PostgreSQL (Camadas relacionais estruturadas)
* **Visualização de Dados (BI):** Apache Superset
* **Fontes de Dados:** AwesomeAPI (Intraday) & API do Banco Central do Brasil (Bacen - Diário)

## 🏗️ Arquitetura e Engenharia de Dados

### 1. Ingestão de Dados e Orquestração (Airflow)
O pipeline é dividido em duas esteiras autônomas com regras de negócio e frequências distintas:
* **Esteira Intraday (AwesomeAPI):** Captura a variação do dólar a cada 15 minutos (`*/15 * * * *`). Configurada para respeitar as janelas de proteção contra sobrecarga de chamadas (*Rate Limiting*) da API pública.
* **Esteira de Fechamento Diário (Bacen):** Extrai o dado oficial da PTAX ao final do dia (`0 19 * * *`). 

### 2. Resiliência e Tolerância a Falhas (Tratamento de Erros)
Visando a estabilidade em produção contra instabilidades de APIs de terceiros (como erros `HTTP 500 Internal Server Error`), a DAG foi blindada utilizando a estratégia de **Exponential Backoff**:
```python
default_args = {
    'retries': 10,
    'retry_delay': timedelta(minutes=1),
    'retry_exponential_backoff': True,
    'max_retry_delay': timedelta(hours=1),
}
