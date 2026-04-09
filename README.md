# Strategic Business Intelligence Case - Setor de Beleza e Serviços

## 1\. Visão Geral do Projeto

Neste repositório, apresento a resolução de um case técnico focado em inteligência de negócios para uma rede de franquias. Meu objetivo foi diagnosticar oscilações de faturamento e identificar gargalos operacionais em 35 unidades. A análise foca no pilar central do modelo de negócio: a **eficiência e rapidez no atendimento**, utilizando dados reais para separar unidades de alta performance daquelas que necessitam de intervenção estratégica.

## 2\. Pilares da Solução

  * **Data Modeling (Star Schema):** Estruturei o modelo de dados relacionando tabelas fato de transações com dimensões de calendário, unidades e clientes para garantir performance nos filtros cruzados.
  * **Performance por Unidade:** Desenvolvi dashboards para identificar disparidades de faturamento e produtividade entre as franquias.
  * **Análise de Retenção e Recorrência:** Implementei lógicas para monitorar a saúde da base, separando o crescimento por aquisição da fidelização real.
  * **Eficiência Operacional:** Criei métricas de controle de tempo (SLAs) para monitorar o status de produtividade por serviço prestado.

## 3\. Tecnologias Utilizadas

  * **SQL (Google BigQuery):** Utilizei para todo o processo de ETL, limpeza e criação de variáveis complexas de negócio.
  * **Power BI:** Desenvolvi dashboards executivos com foco em UX para tomada de decisão rápida.
  * **Análise Estratégica:** Apliquei visão de RevOps para sugerir planos de ação baseados nos dados extraídos.

## 4\. Metodologia Técnica (Engenharia de Dados)

Para entregar valor, não utilizei apenas dados brutos. Apliquei lógicas de SQL para criar métricas que o sistema original não fornecia, garantindo a rastreabilidade da jornada do cliente:

```sql
-- Minha lógica para classificação de produtividade e fidelidade
SELECT 
    id_cliente,
    -- Identificação de gargalo operacional baseado no SLA da marca
    CASE 
        WHEN tipo_item = 'SERVIÇO' AND tempo_atendimento_min > tempo_padrao_min THEN 'LENTO'
        WHEN tipo_item = 'PRODUTO' AND tempo_atendimento_min > 0 THEN 'ALERTA: PRODUTO COM TEMPO'
        ELSE 'REGULAR' 
    END AS status_produtividade,
    -- Identificação de retenção real (First Time vs Loyal)
    CASE 
        WHEN COUNT(*) OVER(PARTITION BY id_cliente) = 1 THEN 'NOVO'
        WHEN ROW_NUMBER() OVER(PARTITION BY id_cliente ORDER BY data_transacao) = 1 THEN 'PRIMEIRA VEZ (POTENCIAL FIEL)'
        ELSE 'RECORRENTE' 
    END AS perfil_cliente
FROM base_transacional;
```

## 5\. Insights Estratégicos (Resumo Executivo)

  * **Performance Temporal e Concentração de Receita:** Identifiquei uma crescente em 2025 com pico em Novembro, mas sem sustentação em 2026. A análise revela que **apenas 5 itens representam 35,32% da receita líquida**, evidenciando uma dependência crítica de poucos serviços (Escova e Hidratação).
  * **Gargalo Operacional Crítico:** O pilar de "Rapidez" está em risco. Identifiquei que serviços de alto valor, como *Tratamento Repair*, chegam a registrar tempos de execução de **01:11:00**, impactando o giro de cadeiras. Na unidade **São Paulo (U001)**, mais de 58% dos atendimentos em determinados períodos foram classificados como **"LENTO"**.
  * **Disparidade de Faturamento:** Existe uma variação de quase 100% no faturamento entre o Top 5 e o Bottom 5. Enquanto unidades líderes performam acima de R$ 20k, unidades como **Belo Horizonte (U007)** e **Florianópolis (U012)** estagnaram na faixa de R$ 10k, sugerindo falhas na aplicação do playbook operacional regional.
  * **Diagnóstico de Retenção (Balde Furado):** Embora a base conte com mais de 1.000 clientes ativos, identifiquei apenas **4 clientes com perfil VIP**. O crescimento é impulsionado por novos clientes (aquisição), mas a taxa de conversão para "Recorrente" é insuficiente para amortizar o CAC (Custo de Aquisição de Cliente) a longo prazo.

## 6\. Recomendações Sugeridas

1.  **Otimização de Produtividade:** Implementar reciclagem operacional na unidade de São Paulo para reduzir o volume de atendimentos "Lentos" e recuperar o Ticket Médio por hora.
2.  **Régua de LTV:** Criar uma régua de comunicação automatizada para os clientes captados nos picos de novembro, visando convertê-los de "Novo" para "Recorrente".
3.  **Auditoria de Mix nas Bottom Units:** Investigar se o baixo desempenho em BH e Florianópolis é decorrente de falta de fluxo ou baixa penetração dos 5 itens que sustentam 35% da receita da rede.

-----

## 7\. Visualização dos Resultados

| Dashboard de Performance por Unidade | Análise de Perfil de Cliente |
| :--- | :--- |
|  |  |

> **Nota Técnica:** Todos os relacionamentos de tabelas foram validados (Star Schema) para evitar duplicidade de dados e garantir que as métricas de faturamento reflitam o valor líquido após deduções de cancelamentos e descontos.

-----

*Status: Concluído*
