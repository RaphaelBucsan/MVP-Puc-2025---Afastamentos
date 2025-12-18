# MVP-Puc-2025---Afastamentos
1) Visão Geral

Este projeto apresenta a construção de um pipeline de dados em nuvem, utilizando o Databricks Community Edition e Apache Spark, com o objetivo de analisar afastamentos de colaboradores a partir de dados fictícios.
A solução foi estruturada seguindo uma arquitetura em camadas (Bronze, Silver e Gold), culminando em um modelo estrela analítico, pronto para consultas SQL e integração com ferramentas de visualização.

2) Objetivo do MVP

Analisar padrões de afastamento de colaboradores, respondendo perguntas como:

- Quais localidades concentram mais afastamentos?
- Quais são os principais motivos de afastamento?
- Existe sazonalidade nos afastamentos ao longo do tempo?
- Qual o volume total de dias afastados por localidade?

3) Arquitetura Utilizada
Dados CSV
   ↓
Camada Bronze (dados brutos)
   ↓
Camada Silver (dados tratados)
   ↓
Camada Gold (modelo estrela)

4) Tecnologias Utilizadas
-> Databricks Community Edition
-> Apache Spark
-> Delta Lake
-> SQL (Databricks SQL)
-> Python (PySpark)
-> GitHub

5) Estrutura do Banco:
  5.1) Camada Bronze – Dados Brutos
    A camada Bronze armazena os dados conforme coletados, com mínima transformação.

      Tabelas:

       - bronze.dim_localidade;
       - bronze.dim_tempo;
       - bronze.dim_motivo_afastamento;
       - bronze.fato_afastamentos;
   
   5.2) Camada Silver – Dados Tratados
        Nesta camada são aplicadas regras de qualidade e padronização:

        # Normalização de textos;
        # Criação de chaves substitutas (SK);
        # Resolução de inconsistências;

      Tabelas:
       - silver.dim_localidade;
       - silver.dim_motivo_afastamento;
       - silver.dim_tempo;
       - silver.fato_afastamentos;
   
   5.3) Camada Gold – Modelo Estrela
        A camada Gold contém o modelo final para análise, estruturado em esquema estrela.
   
         -> Dimensões:
          - gold.dim_localidade
          - gold.dim_motivo_afastamento
          - gold.dim_tempo

         -> Fato:
          - gold.fato_afastamentos
   
           *A tabela fato concentra métricas como*:
             1) Quantidade de colaboradores afastados;
             2) Total de dias de afastamento;
   
7) Modelo Estrela
   
A solução final segue o padrão de Data Warehouse, com uma tabela fato central e múltiplas dimensões conectadas por chaves substitutas.
Esse modelo permite análises eficientes e consultas SQL performáticas.

9) Qualidade dos Dados
    
Foram realizadas análises de qualidade para identificar:

* Valores nulos;
* Duplicidades;
* Inconsistências textuais;
* Problemas de integridade referencial;
* Os principais problemas identificados foram tratados na camada Silver e documentados.

-> Exemplos SQL Utilizados: 
-- Afastamentos por localidade: 

/* SELECT
  l.localidade,
  SUM(f.qtd_colaboradores_afastados) AS total_afastamentos
FROM gold.fato_afastamentos f
JOIN gold.dim_localidade l
  ON f.sk_localidade = l.sk_localidade
GROUP BY l.localidade
ORDER BY total_afastamentos DESC */;

-- Principais Motivos de Afastamento: 
/* SELECT
  m.motivo_afastamento,
  SUM(f.total_dias_afastamento) AS dias_afastados
FROM gold.fato_afastamentos f
JOIN gold.dim_motivo_afastamento m
  ON f.sk_motivo_afastamento = m.sk_motivo_afastamento
GROUP BY m.motivo_afastamento
ORDER BY dias_afastados DESC */;

-> Estrutura Repositório: 

├── data/
│   └── csv/
├── notebooks/
│   ├── bronze/
│   ├── silver/
│   └── gold/
├── sql/
│   └── analytics.sql
└── README.md

-- AUTOAVALIAÇÃO: 
O MVP permitiu aplicar conceitos fundamentais de engenharia de dados, incluindo ingestão, modelagem, transformação e análise de dados em nuvem. As principais dificuldades estiveram relacionadas à configuração do ambiente Databricks e organização dos catálogos, superadas ao longo do desenvolvimento.

   
