# Hub de Inteligência Cambial: Plataforma USD/BRL 📈

Pipeline de dados ponta a ponta (End-to-End) focado na captura, armazenamento, transformação e visualização em tempo real da cotação do Dólar Americano comercial (USD-BRL). 

O projeto adota a arquitetura modern data stack moderna sob o conceito de **ELT**, integrando dados intradiários de alta frequência com dados oficiais do Banco Central, garantindo resiliência na ingestão, governança nas transformações e alta performance na camada de BI.

## 🛠️ Stack Tecnológica
* **Orquestração:** Apache Airflow
* **Ingestão (Extract & Load):** Python (Requests, Logging)
* **Banco de Dados (Data Warehouse):** PostgreSQL
* **Transformação e Modelagem (Transform):** dbt (Data Build Tool)
* **Visualização de Dados (BI):** Apache Superset (ECharts)
* **Fontes de Dados:** AwesomeAPI (Intraday) & API do Banco Central do Brasil (Bacen - Diário)

## 🏗️ Arquitetura e Fluxo de Dados (ELT)
