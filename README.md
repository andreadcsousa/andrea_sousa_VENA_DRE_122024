# 📉 Otimização de Pipeline de ETL e Análise Financeira Gerencial

Este repositório apresenta a resolução completa de um case técnico avançado de engenharia e análise de dados para o setor de consultoria de gestão. O projeto engloba desde a depuração de erros em fluxos de integração legados (*Pentaho Data Integration*), migração de arquitetura para código moderno em Python, até a entrega de uma estrutura analítica de Demonstração do Resultado do Exercício (DRE) no Power BI.

## 🎯 Cenário e Desafio de Negócio

O objetivo do projeto consistia em identificar e corrigir uma divergência crucial em um processo de fechamento financeiro automatizado: os números finais do **Resultado Gerencial do Período** gerados pelo pipeline de dados não batiam com os arquivos fontes originais fornecidos pelas unidades de negócio.

O desafio foi dividido em três grandes pilares técnicos:
1. **Auditoria de Dados e Infraestrutura:** Conexão, tratamento de formatos e ingestão via banco relacional local (SQLite).
2. **Engenharia Reversa e Correção de Bugs:** Analisar os arquivos de transformação (`.ktr`) e jobs (`.kjb`) no Pentaho (PDI) para encontrar onde a regra de negócio quebrava e corrigi-la.
3. **Modernização/Refatoração:** Replicar integralmente as etapas de transformação lógica do Pentaho para scripts otimizados em Python (Pandas).

## 🔍 Investigação e Solução do Erro de ETL (Root Cause Analysis)

Após um trabalho minucioso de auditoria cruzando as tabelas auxiliares, a tabela de *staging* (`staging_dre`) e a tabela dimensão de estrutura, a causa raiz do problema foi localizada dentro do fluxo de carga do PDI:
* **O Bug:** Havia uma incompatibilidade de tipos de dados (*data type mismatch*) no mapeamento dos campos e na tipagem da coluna `LinhaEstrutura` na tabela de *staging*, que passava os valores como inteiros, quebrando o relacionamento direto com a dimensão sintética e gerando omissão de linhas financeiras na fato.
* **A Resolução:** Ajuste no mapeamento do componente de transformação, padronização do fluxo na camada de *staging* para garantir a consistência com as regras sintéticas da DRE, tratamento de registros duplicados e correção do campo temporal `MesRef` através de conversões Unix Epoch via comandos SQL.

## 🛠️ Tecnologias e Arquitetura Utilizadas

* **Pentaho Data Integration (PDI):** Manutenção dos arquivos `.ktr` (Transformações) e `.kjb` (Jobs) para garantir o funcionamento correto do pipeline original.
* **Python & Pandas:** Utilizados no Google Colab para criar uma solução de ETL moderna, flexível e modular, substituindo totalmente a dependência do Pentaho para as etapas de transformação.
* **SQLite & DBeaver:** Camada de banco de dados local para persistência e validação das queries estruturais da DRE.
* **Power BI (ODBC Connection):** Ferramenta de consumo analítico para elaboração da DRE com suporte a análises verticais e horizontais complexas.

## 🐍 Pipeline em Python (Refatoração do ETL)

A lógica de ETL contida no Pentaho foi totalmente reescrita em Python de forma performática. O script desenvolvido realiza:
1. Conexão integrada ao banco SQLite local via biblioteca padrão do Python.
2. Ingestão simultânea dos arquivos fontes analíticos e de apoio.
3. Tratamento semântico completo dos dados, incluindo formatação de strings, conversão correta de datas e tratamento de nulos.
4. Exportação programática direta para o banco de dados via Pandas (`to_sql`), deixando as tabelas prontas para atualização automatizada.

## 📊 Entrega Analítica: Relatório DRE no Power BI

Com o banco de dados corrigido e saneado, o relatório gerencial foi estruturado contendo:
* **DRE Hierárquica:** Organizada respeitando a estrutura contábil corporativa.
* **Análise Vertical:** Medição do impacto percentual de cada linha de custo/despesa diretamente sobre a Receita Bruta.
* **Análise Horizontal:** Avaliação comparativa de evolução e desempenho financeiro entre os meses de referência.
* **Layout Corporativo:** Alinhamento visual utilizando boas práticas de UI/UX para facilitar o consumo por diretores e CFOs.

<img src="./dashboard_dre1.jpg" width="100%">  
<img src="./dashboard_dre2.jpg" width="100%">
