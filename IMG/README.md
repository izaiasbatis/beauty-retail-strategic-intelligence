Engenharia e Modelagem de Dados

A base da solução reside em uma arquitetura de dados robusta, desenhada para suportar análises de alta performance e escalabilidade. O modelo foi construído utilizando a metodologia **Star Schema (Esquema Estrela)**, garantindo integridade referencial e eficiência nos filtros cruzados.

### 1\. Arquitetura Star Schema

O modelo é composto por duas tabelas **Fato** centrais e quatro tabelas **Dimensão**, permitindo a análise de performance versus metas de forma granular.

  * **Fato Transação:** Contém o histórico detalhado de cada operação, incluindo métricas cruciais como `tempo_atendimento_min`, `receita_liquida` e status de produtividade.
  * **Fato Meta:** Armazena os objetivos estratégicos por unidade, permitindo o cálculo em tempo real do `% de Realizado`.
  * **Dimensões (d\_):** \* `DIM_UNIDADE`: Atributos geográficos e identificação das 35 franquias.
      * `DIM_ITEM`: Categorização de produtos e serviços para análise de mix e Pareto.
      * `DIM_CLIENTE`: Gestão de base ativa, inativa e classificação de fidelidade.
      * `DIM_CALENDARIO`: Inteligência de tempo para análises de sazonalidade e crescimento MoM (Month-over-Month).

### 2\. Dicionário de Dados e Relacionamentos

Os relacionamentos foram estabelecidos via chaves substitutas (Surrogate Keys) para otimizar o processamento:

  * **1:N (Um para Muitos):** Relacionamentos diretos das dimensões para as fatos, evitando a ambiguidade de dados e garantindo que as métricas de faturamento não sejam duplicadas.

### 3\. Visualização Técnica do Modelo

Abaixo, a representação visual da estrutura lógica e física implementada:

| Visão do Modelo (Star Schema) | Detalhe das Tabelas (Metadados) |

mpressiona qualquer recrutador ou líder técnico. Gostou da estrutura?
