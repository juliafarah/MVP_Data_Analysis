
*Repository dedicated to MVP Project for Data Analysis and Best Practices sprint for PUC-Rio Data Science and Analytics Post Graduate course.*

# MVP Data Analysis - Identificação de Gargalos e Gestão de Riscos

Este projeto é focado na extração, tratamento e limpeza (ETL) para realização da análise exploratória dos dados (EDA) a fim de obter insights acionáveis que suportam a tomada de decisão de negócios e o desenvolvimento de estratégias utilizando a plataforma Google Colab. 

O notebook com todo o passo a passo do MVP está disponível neste repositório e pode ser acessado através deste [link](https://github.com/juliafarah/MVP_Data_Analysis/blob/main/MVP_Data_Analysis.ipynb).

Ferramentas utilizadas:

* **Google Colab** (Plataforma de Dados)
* **Python** (Linguagem de Programação)
* **Matplotlib/Seaborn** (Visualização Gráfica)


## **1. Descrição do Problema**

Em uma economia globalizada, a eficiência da cadeia de suprimentos é um fator crítico para a competitividade e sobrevivência das multinacionais. O gerenciamento de uma malha logística em escala global, que opera simultaneamente com quatro modais de transporte (Marítimo, Aéreo, Rodoviário e Ferroviário), apresenta uma altíssima complexidade. Atrasos ou interrupções nesse fluxo, frequentemente causados por fatores externos incontroláveis, podem comprometer a continuidade das operações de ponta a ponta, gerando gargalos produtivos, quebra de nível de serviço (SLA) e perdas financeiras significativas.

## **2. Objetivo e Definição do Problema**

Este projeto utiliza um dataset de movimentações de carga globais para identificar os principais fatores de risco e interrupção na cadeia de suprimentos de 2024 a 2026 (YTD). 

O objetivo é conduzir uma **Análise Exploratória de Dados (EDA)** que avalie a vulnerabilidade e o tempo de entrega (Lead Time) de cada modal frente a variáveis críticas, como instabilidade geopolítica e severidade climática. O foco é extrair insights claros para suportar a tomada de decisão estratégica da gerência, e porpor planos de ação para mitigar de atrasos e a construir uma operação logística mais resiliente.

* Tipo de Aprendizado: **Supervisionado (Classificação)**


## 3. Coleta dos dados

* **Dataset:**  `global_supply_chain_risk_2026.csv`
* **Fonte:** [Kaggle](https://www.kaggle.com/datasets/nudratabbas/global-supply-chain-risk-and-logistics-2024-2026)
* **Definição dos Atributos originais do dataset:**
  | Coluna | Descrição |
  | :--- | :--- |
  | Shipment_ID | Identificador único da carga.
  | Date | Data de despacho da carga (2024 a 2026).
  | Origin_Port | Portos de origem (Hubs globais)
  | Destination_Port | Portos de destino (Hubs globais).
  | Transport_Mode | Modal (Sea, Air, Rail, Road).
  | Distance_km | Distância total percorrida.
  | Weight_MT | Peso da carga em Toneladas Métricas.
  | Fuel_Price_Index | Índice de custo de combustível no despacho.
  | Geopolitical_Risk_Score | Índice de risco regional (0 a 10).
  | Weather_Condition | Condição climática (Clear, Storm, Hurricane, Fog).
  | Carrier_Reliability_Score | Score de confiabilidade do transportador (0.5 a 1.0).
  | Lead_Time_Days | Tempo real de entrega (Variável Alvo para Regressão).
  | Disruption_Occurred | Flag de interrupção ou atraso (0: Não / 1: Sim).

* **Atributos criados posteriormente (Discretização)**
  
  | Coluna | Descrição |
  | :--- | :--- |
  | Lead_Time_Category | Categoria de LT (On-time, Delayed, Critical).
  | Weather_Severity_Rank | Ordenação númerica dada a severidade da condição climática (1 a 4).
  | Geopolitical_Risk_Level | Categorias para o Risco Geopolítico (Low Risk, Medium Risk, High Risk).


## 4. Pré-Processamento dos dados

O notebook com todo este passo a passo e análise do MVP está disponível neste repositório e pode ser acessado através deste [link](https://github.com/juliafarah/MVP_Data_Analysis/blob/main/MVP_Data_Analysis.ipynb).

1. Importando as bibliotecas e carregando o dataset.
2. Detalhes dos atributos.
3. Verificação de valores nulos.
4. Checar duplicidade.
5. Resumo estatístico.
6. Converter o tipo da coluna Date de *object* para *datetime*.
7. Definindo colunas por tipo (numéricas e categóricas).
8. Verificação manual de inconsistências comuns em Supply Chain.
9. Matriz de Correlação.
10. Verificando outliers (*anomalias*).
11. Tipo de distribuição de cada atributo numéricos.
12. Discretização (`Lead_Time_Category`).
13. Criação da coluna `Weather_Severity_Rank` para ordernar cada condição climática por grau de severidade.  


## 5. Análise Exploratória dos Dados

Com base nos dados tratados, as perguntas feitas a seguir foram respondidas e análise completa está disponível no [notebook](https://github.com/juliafarah/MVP_Data_Analysis/blob/main/MVP_Data_Analysis.ipynb).

**Perguntas a serem respondidas:**

  1. Qual a taxa de atraso/interrupção geral?
  2. Qual modal impacta mais a média do tempo de entrega?
  3. Qual o atraso real em dias para cada modal dada a condição climática?
  4. Quantas entregas de cada modal foram On-time/Delayed/Critical? (*Análise de Nível de Serviço (SLA)*)
  5. Como se distribui a confiabilidade operacional e como o clima altera a performance de entrega (SLA)?
  6. Qual é o impacto das interrupções (`Disruption_Occurred`) no tempo médio de entrega de cada modal?
  7. Qual é a probabilidade da entrega ultrapassar o Lead Time médio do modal dado o Risco Geopolítico?
  8. Quão exposto o Lead Time médio de cada modal fica com o impacto do risco geopolítico? (*Índice de Exposição & Impacto no SLA*)


## 6. Insights Obtidos (Conclusão)

A partir da taxa de interrupção de **61%** e da análise do comportamento das variáveis climáticas, geopolíticas e operacionais no tempo de entrega dos suprimentos, as principais conclusões foram:

**O modal maritimo (Sea)**: *Embora inevitável para o transporte de grandes volumes, é o **ponto de maior gargalo da cadeia**.*

* Possui o maior tempo médio (**LT = 40 dias**) e um Índice de Exposição **26 vezes** maior que o aéreo.

* Em cenários extremos (furacões), o tempo de entrega sofre picos de até **760%**.

* Interrupções reais triplicam sua latência, adicionando **36 dias extras** ao cronograma, um atraso residual suficiente para paralisar operações e gerar multas severas.


**Os modais terrestres (Road & Rail)**: *Na comparação terrestre, o caminhão (Road) consolida-se como a principal válvula de escape frente ao trem (Rail).*

* A rigidez estrutural do modal ferroviário (Rail) impede desvios em crises, fazendo seu Lead Time também **triplicar** sob interrupção.

* O modal rodoviário (Road) permite redirecionamento dinâmico. Isso faz com que absorva melhor o impacto, garantindo entregas até **18% mais ágeis** (uma vantagem de **8 dias**) em cenários de intercorrência climática.


**O modal Aéreo (Air)**: *Provou ser o único amortecedor confiável de todo o sistema logístico.*

* É incrivelmente resiliente: além de ser o mais rápido (**2 dias**), a pior interrupção possível **adiciona apenas 1 dia** ao seu prazo final.

* Justifica seu custo elevado como via exclusiva para peças críticas, pois raramente é paralisado por bloqueios climáticos ou geográficos. 

* Apesar de sua resiliência, não atua como substituto em massa para os demais modais devido a severas restrições de peso, volume e custo.

* Assim como os caminhões, usufrui da mudança de rota sob bloqueios. Um exemplo atual é a adaptação de rotas no Oriente Médio frente ao **conflito entre Irã (Pérsia) e Israel/EUA**, comprovando sua capacidade de contornar crises geopolíticas de forma rápida.


## 7. Possíveis Planos de Ação

**1. Implementar Safety Stock Dinâmico**
* Abandonar o uso exclusivo de médias históricas de entrega. O estoque de segurança deve ser dimensionado pelo Índice de Exposição (Lead Time x Risco Geopolítico).

* Como as cargas do modal **Marítimo (Sea)** possuem volumes massivos e não podem ser migradas para o modal aéreo em caso de crise, a aplicação de margens de estoque agressivas torna-se a única defesa viável para evitar o desabastecimento.

**2. Estabelecer o modal rodoviário como a rota de escape oficial e imediata em cenários de crise aguda no continente.** 

* Por suportar perfis de carga similares aos trens, sua flexibilidade de redirecionamento dinâmico deve ser ativada antes que a carga fique retida em modais rígidos como o Ferroviário (Rail).

**3. Usufruir do Modal Aéreo (Air) para "Micro-Contingências"**. 

* Dado o seu **baixo Índice de Exposição (8)** e as severas restrições de capacidade volumétrica e custo, *o frete aéreo jamais deve ser acionado na tentativa de absorver gargalos da operação principal (marítima).* Seu uso deve ser estritamente protocolado para "salvar" a linha de produção, transportando exclusivamente peças de reposição urgentes, críticas e de alto valor agregado.

**4. Desenvolver um alerta preventivo frente a previsões de eventos climáticos extremos (furacões)**. 

* Como é fisicamente inviável transferir os grandes volumes do mar para o ar de última hora, a estratégia deve focar em *antecipar os prazos de despacho* ou *redirecionar preventivamente as cargas para portos fora da rota da tempestade*, mitigando a exposição à ruptura de quase **70% do SLA**.




