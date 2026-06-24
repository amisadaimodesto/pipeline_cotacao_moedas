# Hub de Inteligência Cambial: Plataforma USD/BRL 📈

Pipeline de dados ponta a ponta (End-to-End) focado na captura, armazenamento, transformação e visualização em tempo real da cotação do Dólar Americano comercial (USD-BRL). 

O projeto adota a arquitetura modern data stack moderna sob o conceito de **ELT**, integrando dados intradiários de alta frequência com dados oficiais do Banco Central, garantindo resiliência na ingestão, governança nas transformações e alta performance na camada de BI.

## 🛠️ Stack Tecnológica
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
