
Repository dedicated to MVP Project for Data Analysis and Best Practices sprint for PUC-Rio Data Science and Analytics Post Graduate course.

# MVP Data Analysis - Logistica

## **1. Descrição do Problema**

Em uma economia globalizada, a eficiência da cadeia de suprimentos é um fator crítico para a competitividade e sobrevivência das multinacionais. O gerenciamento de uma malha logística em escala global, que opera simultaneamente com quatro modais de transporte (Marítimo, Aéreo, Rodoviário e Ferroviário), apresenta uma altíssima complexidade. Atrasos ou interrupções nesse fluxo, frequentemente causados por fatores externos incontroláveis, podem comprometer a continuidade das operações de ponta a ponta, gerando gargalos produtivos, quebra de nível de serviço (SLA) e perdas financeiras significativas.

## **2. Objetivo e Definição do Problema**

Este projeto utiliza um dataset de movimentações de carga globais para identificar os principais fatores de risco e interrupção na cadeia de suprimentos. 

O objetivo é conduzir uma Análise Exploratória de Dados (EDA) que avalie a vulnerabilidade e o tempo de entrega (Lead Time) de cada modal frente a variáveis críticas, como instabilidade geopolítica e severidade climática. O foco é extrair insights claros para suportar a tomada de decisão estratégica da gerência, visando a mitigação de atrasos e a construção de uma operação logística mais resiliente.

* Tipo de Aprendizado: **Supervisionado (Classificação)**


## 3. Coleta dos dados:

* **Dataset:** <.csv>
* **Fonte:** Kaggle
* **Definição dos Atributos:**
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


## 4. Pré-Processamento dos dados

1. Tratamento de valores faltantes ou nulo
2. Resumo estatístico
4. Verificação tipo de cada atributo
5. Verificação de valores inconsistentes em campos críticos como Lead_Time_Days e Distancia_Km  
6. Alteração do tipo do atributo Date de dtype object ➔ datetime
7. Verificação do tipo de distribuição dos atributo númerico
8. Matriz de Correlação
9. Verificação de outleirs (anomalias)
10. Aplicação do Ordinal Encoding para o atributo weather_condition (1 a 4) para representar a hierarquia de severidade climática, permitindo que o modelo matemático compreenda a escala de intensidade dos eventos.

O notebook com todo este passo a passo e análise do MVP está disponível neste repositório e pode ser acessado através deste [link](https://github.com/juliafarah/MVP_Data_Analysis/blob/main/MVP_Data_Analysis.ipynb).


## 5. Análise Exploratória dos Dados

Com base nos dados tratados, as perguntas feitas a seguir foram respondidas e análise completa está disponível no [notebook](https://github.com/juliafarah/MVP_Data_Analysis/blob/main/MVP_Data_Analysis.ipynb).

**Perguntas a serem respondidas:**

  1. Qual a taxa de interrupção geral?
<img width="375" height="350" alt="image" src="https://github.com/user-attachments/assets/8c5fa0ff-fb86-42d7-a228-24dad69fc5d5" />
  


  2. Qual modal impacta mais a media do tempo de entrega?

<img width="529" height="370" alt="image" src="https://github.com/user-attachments/assets/99316d29-2b45-4534-9137-698467876ee4" />


  3. Qual é a probabilidade da carga ultrapassar a média propria do modal de tempo de entrega, ou seja, atrasar de acordo com o risco geopolitico?

<img width="840" height="390" alt="image" src="https://github.com/user-attachments/assets/f5ffa752-96f3-40da-9879-7f5d4cac0b86" />



  4. Qual o impacto real das condições climáticas no atraso médio de cada modal?

<img width="645" height="395" alt="image" src="https://github.com/user-attachments/assets/31924f4a-8dad-44a8-8bbf-ae42c9275e59" />



  5. Quão exposto o Lead Time médio de cada modal fica com o impacto do risco geopolítico?

<img width="679" height="290" alt="image" src="https://github.com/user-attachments/assets/c8125a70-750d-4c39-9b7c-acd839bbf6e9" />


  6. Qual o impacto das interrupções no Lead Time médio por modal?

<img width="590" height="390" alt="image" src="https://github.com/user-attachments/assets/6d9ef96d-9ce4-4c88-8ba4-9792711daebe" />




## 6. Insights Obtidos

A partir da análise do comportamento das variáveis climáticas, geopolíticas e operacionais no tempo de entrega dos suprimentos, as principais conclusões foram:  

* **O modal maritimo (Sea):**

Embora inevitável para o transporte de grandes volumes, este é o gargalo central da cadeia. Com o maior tempo médio de entrega (LT = 40 dias), ele apresenta um Índice de Exposição **26 vezes** maior que o aéreo. Na prática, este modal é altamente vulnerável a crises como mostra o projeto que em cenários de eventos climáticos extremos (furacões) elevam o tempo médio de entrega do modal em até **760%**, e interrupções reais **triplicam** sua latência, *adicionando **36 dias** extras* ao cronograma. Este atraso residual é suficiente para paralisar operações e gerar multas severas.

* **Os modais terrestres (Road & Rail):**

Na comparação entre os modais terrestres, o caminhão (**Road**) consolida-se como a principal rota de escape frente ao trem (**Rail**). A rigidez estrutural da malha ferroviária impede desvios durante crises geopolíticas ou climáticas, fazendo com que o trem também **triplique** seu tempo médio de espera (Lead Time médio) sob interrupção. O modal **rodoviário (Road)**, por permitir redirecionamento dinâmico de rotas, absorve melhor o impacto e garante entregas até **18% mais ágeis** (*uma vantagem de **8 dias***) em cenários de intercorrência climática.

**O modal Aéreo (Air):**

O transporte **aéreo (Air)** provou ser o único amortecedor confiável do sistema. Além de ser o mais rápido (**2 dias**), sua resiliência é incomparável dado que a pior interrupção possível adiciona apenas **1 dia** ao seu prazo final. Ele raramente tem as operações paralisadas por bloqueios geográficos e/ou severidade climática, justificando seu custo elevado como a via exclusiva para peças de reposição crítica.

Porém, assim como ocorre com o caminhão no modal **rodoviário (Road)**, o transporte aéreo usufrui da possibilidade de mudança de rota em situações de bloqueios aéreos temporários como vem acontecendo atualmente no Oriente Médio, uma das rotas mais movimentadas do mundo, dado ao conflito entre Irã (Pérsia) e Israel/Estados Unidos.



## 7. Conclusão e Plano de Ação

A fim de reduzir a taxa de falhas, a estratégia de suprimentos é altamente aconselhável a adoção de políticas de **Safety Stock** (*estoque de segurança*) principalmente para os suprimentos transportados pelo modal **martimo (Sea)** dimensionadas pelo Índice de Exposição, e não apenas pela média histórica de entrega.

Cargas alocadas no modal **marítimo (Sea)** ou **ferroviário (Rail)** em rotas de alto risco geopolítico exigem margens de estoque mais agressivas, enquanto o modal **rodoviário (Road)** deve ser institucionalizado como a alternativa prioritária em casos extremos dado a sua flexibilidade de redirecionamento.





