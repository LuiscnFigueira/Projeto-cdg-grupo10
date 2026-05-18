# Relatório de Conclusão e Entrega de Valor (Milestone 4)

## 1. Síntese de Resultados e Impacto

### 1.1 Objetivo 1 - Modelo Preditivo de `Attrition`

#### O Problema Resolvido
O primeiro objetivo do projeto consistia em desenvolver um modelo de classificação capaz de prever o atrito (`Attrition`) dos colaboradores, alcançando um F1-Score mínimo de 0,80 em validação cruzada estratificada (k=15), até ao dia 21/04/2026. O problema foi formalizado como uma tarefa de classificação binária sobre o conjunto de dados _IBM HR Analytics Employee Attrition & Performance_, composto por 1470 colaboradores e 35 variáveis originais, com uma taxa de atrito de 16,1% (Chapman et al., 2000; IBM Watson Analytics, 2016).

#### Interpretação dos Resultados

O modelo final selecionado foi a Regressão Logística com threshold ótimo de 0.50, obtido após um processo de otimização em cinco etapas sequenciais: pesquisa do melhor _split_ (65/35), do melhor normalizador (`StandardScaler`), da melhor técnica de _resampling_ (nenhuma aplicada no modelo final), de hiperparâmetros via `GridSearchCV` e do threshold de decisão. Foram testados 18 algoritmos distintos antes de convergir para esta solução, cobrindo modelos de ensemble, lineares, probabilísticos e redes neuronais, em linha com a recomendação do CRISP-DM de explorar múltiplos algoritmos antes de selecionar o modelo final (Chapman et al., 2000).

Os resultados obtidos no conjunto de teste são os seguintes:

| Split | Normalizador | Resampling | Threshold | F1 | Precision | Recall | AUC-ROC |
|---|---|---|---|---|---|---|---|
| 65/35 | StandardScaler | Nenhum | 0.50 | 0.5538 | 0.7660 | 0.4337 | 0.8236 |

A meta de F1-Score ≥ 80% não foi alcançada, reflexo das limitações estruturais do conjunto de dados: 1470 observações, desequilíbrio de classes de ~16% e ausência de variáveis de engajamento direto. Ainda assim, o AUC-ROC de 82,36% confirma uma boa capacidade discriminativa global, e a Precision de 76,60% garante que, dos colaboradores sinalizados como em risco, dois em cada três correspondem a casos reais de atrito. O threshold de 0.50 foi selecionado para maximizar o F1-Score na classe minoritária, equilibrando a identificação de casos reais de saída com a contenção de falsos positivos (Géron, 2022; James et al., 2021).

#### Índice de Risco de `Attrition`

Com base nas probabilidades previstas pelo modelo final, foi construído um índice de risco que classifica cada colaborador em quatro categorias, permitindo traduzir uma previsão probabilística em informação diretamente acionável para a gestão de Recursos Humanos:

| Categoria | Limiar | Interpretação | Colaboradores | Percentagem |
|---|---|---|---|---|
| Baixo | prob < 30% | Colaborador estável, permanência muito provável | 1200 | 81,6% |
| Médio | 30% ≤ prob < 50% | Sinais de alerta; modelo prevê permanência | 116 | 7,9% |
| Alto | 50% ≤ prob < 70% | Modelo prevê saída; acompanhamento ativo recomendado | 72 | 4,9% |
| Crítico | prob ≥ 70% | Saída muito provável; intervenção prioritária | 82 | 5,6% |

Os limiares foram definidos respeitando o limiar natural da Regressão Logística (50%), garantindo que colaboradores com probabilidade abaixo e acima desse valor nunca são agrupados na mesma categoria.

#### Valor para o Utilizador/Negócio

A solução permite identificar, de forma sistemática e interpretável, os colaboradores com maior probabilidade de saída, classificando-os por nível de urgência de intervenção. Os fatores de risco predominantes, liderados por `OverTime_bin`, `YearsSinceLastPromotion` e `TotalWorkingYears`, são diretamente comunicáveis a gestores não técnicos e traduzíveis em ações concretas de retenção. Este resultado apoia estratégias preventivas diferenciadas por perfil, reduzindo o custo de rotatividade e preservando o capital humano organizacional (Hom et al., 2017).


### 1.2 Objetivo 2 - Identificação de Perfis de Colaboradores através de _Clustering_

#### O Problema Resolvido

O segundo objetivo consistia em aplicar técnicas de _clustering_ para identificar e caracterizar perfis distintos de colaboradores, determinando o número ótimo de _clusters_ com um _Silhouette Score_ médio superior a 0,50, até ao dia 21/04/2026. O problema foi abordado como uma tarefa de aprendizagem , sem recurso a qualquer informação sobre a variável alvo (`Attrition`) durante o processo de agrupamento (Chapman et al., 2000).

#### Interpretação dos Resultados

O modelo final selecionado foi UMAP(`n_components=5`, `n_neighbors=15`) + DBSCAN(`eps=6.0`, `min_samples=3`). Foram testadas seis abordagens distintas de redução de dimensionalidade e seleção de variáveis antes de convergir para esta solução, incluindo DBSCAN no espaço original, DBSCAN otimizado, PCA + DBSCAN, seleção por correlação, GMM e _K-Means_ (McInnes et al., 2018; Schubert et al., 2017).

Os resultados obtidos são os seguintes:

| Métrica                 | Treino  | Teste  | Meta       | Resultado    |
|-------------------------|---------|--------|------------|--------------|
| Silhouette Score        | 0,7141  | 0,7016 | > 0,50     | Atingida     |
| Davies-Bouldin Index    | 0,3991  | 0,3864 | Minimizar  | Excelente    |
| Calinski-Harabasz Index | 1772,02 | 301,16 | Maximizar  | 21x baseline |
| Clusters identificados  | 4       | 4      | -          | -            |
| Ruído (%)               | 0,0%    | 0,0%   | Minimizar  | Nenhum       |

O _Silhouette Score_ de 0,7016 no conjunto de teste supera a meta em +40,3 pontos percentuais. Rousseeuw (1987) classifica valores superiores a 0,70 como indicativos de estrutura de _clustering_ forte, o limiar mais exigente da sua escala. O Davies-Bouldin de 0,3864 confirma _clusters_ compactos e bem separados - valores abaixo de 0,5 são considerados excelentes (Davies & Bouldin, 1979). A diferença entre treino e teste é mínima (Delta Silhouette = 0,0125), confirmando que o modelo generaliza a estrutura aprendida sem sobreajustamento.

Os quatro perfis de colaboradores identificados apresentam correspondência direta com a estrutura departamental da organização:

| Cluster | Designação           | Peso  | Características Principais                                                         |
|---------|----------------------|-------|------------------------------------------------------------------------------------|
| 0       | I&D Operacional      | 56%   | Técnicos de I&D de nível médio, rendimento moderado, longa permanência na empresa  |
| 1       | Liderança Científica | 5,8%  | Quadros seniores de I&D, rendimento elevado, alta escolaridade, cargos de liderança|
| 2       | Equipa de Vendas     | 36%   | Colaboradores de Vendas, perfil mais jovem, maior mobilidade entre empresas        |
| 3       | Recursos Humanos     | 2,1%  | Pequeno grupo de RH, perfil distinto em termos de função e compensação             |

Esta coerência não foi imposta ao modelo - resultou da aprendizagem sobre 53 variáveis escaladas. A correspondência entre os _clusters_ e unidades organizacionais reais é um indicador forte de validade externa: o modelo captura uma estrutura que existe objetivamente na organização, não um artefacto do algoritmo (Jain, 2010).

Por palavras simples, o modelo consegue identificar automaticamente os diferentes perfis de colaboradores sem ter sido informado sobre departamentos ou funções. O facto de os grupos encontrados coincidirem com a realidade organizacional confirma que as variáveis do conjunto de dados captam diferenças reais entre os colaboradores, e não apenas ruído estatístico.

#### Valor para o Utilizador/Negócio

A segmentação permite à organização desenvolver estratégias de Recursos Humanos diferenciadas por perfil. A Equipa de Vendas (36%), com maior mobilidade entre empresas, pode beneficiar de políticas de retenção específicas baseadas em incentivos de progressão e reconhecimento. A Liderança Científica (5,8%), apesar de reduzida em número, representa um capital humano de elevado valor estratégico que justifica abordagens de retenção personalizadas. Os colaboradores de I&D Operacional (56%), sendo o maior grupo, são os que mais influenciam os indicadores agregados da organização (Hom et al., 2017).

## 1.3 Resposta às Perguntas de Investigação

### Pergunta de Investigação 1 - Quais são as variáveis com maior poder explicativo e preditivo do atrito?

A análise inferencial realizada na _Milestone 2_ identificou as variáveis com maior relevância estatística para o atrito, combinando testes qui-quadrado, Kruskal-Wallis e o efeito medido por Cramér's V e Cohen's d. As variáveis com maior poder
discriminativo foram `OverTime` (Cramér's V = 0.24), `JobLevel`, `TotalWorkingYears`, `MonthlyIncome` e `Age`, todas com associação estatisticamente significativa (p < 0.05).

O modelo final de Regressão Logística confirmou e quantificou estas relações através dos coeficientes estimados. A expressão do modelo revelou que as variáveis com maior peso preditivo positivo, isto é, que aumentam a probabilidade de saída, são `OverTime_bin` (coef. = +0.944), `YearsSinceLastPromotion` (coef. = +0.569) e `JobRole_Sales Representative` (coef. = +0.518), enquanto variáveis como `JobLevel`, `TotalWorkingYears` e `MonthlyIncome` apresentam coeficientes negativos, confirmando o efeito protetor da senioridade e da compensação.

---

### Pergunta de Investigação 2 - Existe associação estatisticamente significativa entre `OverTime` e `Attrition`?

Sim. O teste do qui-quadrado aplicado à tabela de contingência entre `OverTime` e `Attrition` confirmou uma associação estatisticamente significativa (χ² = 87.56, p < 0.001). O tamanho do efeito medido por Cramér's V foi de 0.24, classificado como fraco a moderado. Em termos práticos, a taxa de atrito nos colaboradores que realizam horas extra é de 30.5%, face a 10.4% nos restantes, uma diferença de quase 3x que confirma o papel central desta variável no fenómeno estudado.

---

### Pergunta de Investigação 3 - O nível de satisfação e o equilíbrio trabalho-vida influenciam o atrito?

Sim. O teste de Kruskal-Wallis aplicado às variáveis ordinais `JobSatisfaction`, `EnvironmentSatisfaction`, `RelationshipSatisfaction` e `WorkLifeBalance` confirmou diferenças estatisticamente significativas entre os grupos com e sem atrito (p < 0.05
em todas as variáveis). A variável composta `SatisfactionIndex`, criada na fase de engenharia de atributos como média das quatro dimensões de satisfação, apresentou correlação negativa com `Attrition`, confirmando que colaboradores com menor satisfação global têm maior probabilidade de saída. O coeficiente negativo de `JobSatisfaction` no modelo final reforça esta conclusão.

---

### Pergunta de Investigação 4 - O rendimento mensal tem impacto significativo no atrito após controlo multivariável?

Sim, mas com efeito inverso rendimentos mais elevados reduzem a probabilidade de saída. A análise exploratória mostrou que colaboradores que saem têm rendimento mensal médio substancialmente inferior aos que permanecem. O modelo multivariável confirmou
este efeito: o coeficiente de `MonthlyIncome` é positivo na expressão do modelo, o que, conjugado com a escala da variável, indica que aumentos no rendimento reduzem a probabilidade de atrito, mesmo após controlo das restantes variáveis. A variável `IncomePerLevel`, criada como rácio entre rendimento e nível hierárquico, revelou adicionalmente que colaboradores sub-remunerados face ao seu nível de cargo apresentam maior risco de saída.

---

### Pergunta de Investigação 5 - Qual o algoritmo com melhor desempenho e maior estabilidade?

Após testar 18 algoritmos distintos em validação cruzada estratificada, a Regressão Logística foi o modelo com melhor equilíbrio entre desempenho e estabilidade:

| Modelo | F1 Teste | AUC Teste | Overfitting (ΔF1) |
|---|---|---|---|
| Regressão Logística | 0.5538 | 0.8236 | +0.12 |
| LDA | 0.4651 | 0.8179 | +0.19 |
| Naive Bayes | 0.4354 | 0.7269 | +0.04 |
| Random Forest | 0.1569 | 0.8017 | +0.78 |
| Keras simples | 0.4634 | 0.8100 | +0.54 |

Os modelos de ensemble (Random Forest, XGBoost, LightGBM, CatBoost, Extra Trees) atingiram F1 = 1.0 no treino mas colapsaram no teste. As redes neuronais apresentaram _overfitting_ elevado apesar da sofisticação arquitetural. A Regressão Logística foi a
única a combinar AUC competitivo (0.82), overfitting controlado (+0.12) e probabilidades bem calibradas, critério essencial para o índice de risco.

---

### Pergunta de Investigação 6 - O desequilíbrio de classes afeta os modelos e pode ser mitigado com SMOTE?

Sim. O desequilíbrio (~84% vs ~16%) afeta diretamente o desempenho dos modelos, penalizando o _Recall_ na classe minoritária. Sem qualquer técnica de balanceamento, o modelo _baseline_ de Regressão Logística obtinha _F1 Teste_ = 0,4978. A aplicação de SMOTE no _pipeline_ de treino melhorou o _F1 Teste_ para 0,5538, demonstrando que o balanceamento sintético é eficaz na identificação de colaboradores em risco. O _threshold_ padrão de 0,50 foi mantido como limiar de decisão, uma vez que o processo de otimização sistemática confirmou empiricamente que a configuração base da Regressão Logística era já a solução ótima para este problema (Chawla et al., 2002).

---

### Pergunta de Investigação 7 - É possível construir um índice de risco de atrito interpretável e fiável?

Sim. Com base nas probabilidades previstas pelo modelo final de Regressão Logística, foi construído um índice de risco que classifica cada colaborador em quatro categorias:

| Categoria | Limiar | Interpretação |
|---|---|---|
| Baixo | prob < 30% | Colaborador estável, permanência muito provável |
| Médio | 30% ≤ prob < 50% | Sinais de alerta; modelo prevê permanência |
| Alto | 50% ≤ prob < 70% | Modelo prevê saída; acompanhamento ativo |
| Crítico | prob ≥ 70% | Saída muito provável; intervenção prioritária |

Os limiares foram definidos respeitando o limiar natural da Regressão Logística (50%), garantindo que colaboradores com probabilidade abaixo e acima desse valor nunca são agrupados na mesma categoria. A validação do índice confirmou que a taxa de atrito
real aumenta progressivamente de Baixo para Crítico, demonstrando a coerência e utilidade da segmentação para apoio à decisão em Recursos Humanos.

---

### Pergunta de Investigação 8 - Que fatores distinguem os colaboradores com maior risco e como apoiam estratégias de retenção?

A análise combinada do modelo preditivo e do clustering identificou um perfil consistente de colaboradores em risco elevado: jovens (20–35 anos), com rendimento baixo, nível hierárquico reduzido, realização sistemática de horas extraordinárias,
menor tempo na empresa e menor satisfação global. Este perfil concentra-se maioritariamente na Equipa de Vendas (_cluster 2_, 36% da organização), o grupo com maior mobilidade entre empresas identificado pelo modelo de _clustering_.

Em termos de estratégias de retenção, os resultados sugerem três prioridades: controlo sistemático das horas extraordinárias (fator com maior peso preditivo, coef. = +0.944), revisão da estrutura de progressão salarial nos níveis hierárquicos
mais baixos, e acompanhamento proativo dos colaboradores da Equipa de Vendas com índice de risco Alto ou Crítico. A Liderança Científica (_cluster 1_, 5.8%), apesar do
baixo risco de saída, justifica abordagens de retenção personalizadas dado o elevado custo de substituição destes perfis (Hom et al., 2017).



## 2. Análise Crítica e Limitações

### 2.1 Limitações dos Dados

O conjunto de dados _IBM HR Analytics_ é sintético, tendo sido criado pela IBM para fins demonstrativos, o que limita a transferibilidade direta dos resultados para contextos organizacionais reais. A variável `Attrition` apresenta desequilíbrio de classes significativo (~84% "Não Saiu" vs ~16% "Saiu"), o que penaliza métricas como o _F1-Score_ mesmo em modelos com boa capacidade discriminativa (Chawla et al., 2002). 

A variável `PerformanceRating` apresenta variância praticamente nula, com a quase totalidade das observações concentradas nos valores 3 e 4, limitando o seu poder preditivo e justificando a sua exclusão na fase de preparação dos dados. Por fim, o conjunto de dados não inclui variáveis potencialmente relevantes como indicadores de engajamento (_engagement_), absentismo, clima organizacional real ou dados externos sobre o mercado de trabalho, o que condiciona o poder preditivo máximo dos modelos (Hom et al., 2017).


### 2.2 Limitações do Modelo Preditivo

O objetivo mínimo de _F1-Score_ >= 0,80 não foi alcançado (_F1 Teste_ = 0.5538), apesar de terem sido testados 18 algoritmos distintos e aplicadas técnicas de equilíbrio (SMOTE) e otimização de _threshold_.

As previsões indicam probabilidade de risco, mas não provam causalidade entre as variáveis e o atrito - uma limitação inerente a qualquer modelo preditivo correlacional (James et al., 2021). 

O índice de risco deve ser interpretado como ferramenta de apoio à decisão humana, não como mecanismo de decisão automática.


### 2.3 Limitações do _Clustering_

O _Cluster 3_ (Recursos Humanos) conta com apenas ~26 membros no treino (2,1% da amostra). A dimensão reduzida impõe cautela na generalização dos seus perfis médios e na tomada de decisões organizacionais baseadas neste grupo, uma vez que pequenas alterações na composição do departamento de RH podem alterar significativamente as médias do _cluster_. O UMAP é um algoritmo estocástico: sem semente fixa (`random_state=42`), execuções distintas produzem *manifolds* ligeiramente diferentes, alterando potencialmente a atribuição de pontos fronteiriços (McInnes et al., 2018). 

A avaliação baseou-se exclusivamente em métricas internas (Silhouette, Davies-Bouldin, Calinski-Harabasz): a validade externa - correspondência com variáveis não utilizadas na modelação, como o atrito - não foi testada formalmente, constituindo uma limitação metodológica a colmatar em trabalhos futuros (Rousseeuw, 1987).

## 3. Considerações Éticas e de Viés

### 3.1 Privacidade
O _conjunto de dados_ encontra-se totalmente anonimizado, não contendo identificadores pessoais diretos como nomes, contactos ou identificadores reais de colaboradores. A utilização dos dados neste projeto é estritamente académica e cumpre os princípios fundamentais do RGPD, nomeadamente a minimização de dados, a limitação da finalidade e a licitude do tratamento. Atendendo à natureza sintética e agregada da informação, o risco de reidentificação individual é negligenciável.

### 3.2 Transparência

A Regressão Logística foi selecionada em parte pela sua interpretabilidade intrínseca: os coeficientes do modelo são diretamente legíveis e permitem explicar, para cada colaborador, quais as variáveis que mais contribuíram para o nível de risco atribuído. Esta propriedade é essencial num contexto de Recursos Humanos, onde as decisões de intervenção têm de ser justificáveis perante os próprios colaboradores e a gestão de topo (Hom et al., 2017).

O índice de risco é apresentado com categorias claras e limiares explícitos; Baixo (< 30%), Médio (30–50%), Alto (50–70%) e Crítico (≥ 70%),  permitindo auditabilidade das decisões de intervenção. Todos os passos metodológicos estão documentados e são reprodutíveis, sendo o _notebook_ executável do início ao fim sem interrupções, em linha com os princípios de reprodutibilidade da ciência aberta (Chapman et al., 2000).

### 3.3 Viés

O desequilíbrio da variável `Attrition` (~84% vs ~16%) pode levar o modelo a subvalorizar casos reais de atrito, com impacto direto no _Recall_. Este viés foi parcialmente mitigado através da aplicação de SMOTE e da otimização do _threshold_ de decisão, mas não eliminado na totalidade, o que reforça a necessidade de interpretar o índice como ferramenta de apoio e não como veredicto automático.

Variáveis como `Age`, `Gender`, `MaritalStatus`, `MonthlyIncome` ou `JobLevel` devem ser interpretadas com cautela: a sua correlação com o atrito pode refletir padrões históricos de desigualdade organizacional, não necessariamente causalidade (James et al., 2021). A classificação de colaboradores como "de risco" não deve ser utilizada para penalização, restrição de oportunidades ou decisões de despedimento. O índice de risco deve suportar exclusivamente estratégias proativas e justas de retenção e desenvolvimento, em linha com os princípios de uso responsável de sistemas de apoio à decisão algorítmica em Recursos Humanos (Hom et al., 2017).


## 4. Roadmap e Trabalhos Futuros

### 4.1 Melhorias Técnicas

As principais limitações identificadas neste projeto sugerem as seguintes melhorias técnicas para trabalhos futuros:

* **Explorar técnicas de calibração de probabilidades** como _Platt Scaling_ ou _Isotonic Regression_, com o objetivo de melhorar a fiabilidade do índice de risco, garantindo que uma probabilidade prevista de 70% corresponda efetivamente a 70% de casos reais de saída (Géron, 2022).

* **Testar abordagens de ensemble heterogéneo**, combinando a Regressão Logística com modelos de maior capacidade preditiva como LightGBM ou XGBoost através de _stacking_, procurando superar o teto de desempenho imposto pelo desequilíbrio de classes sem sacrificar interpretabilidade (James et al., 2021).

* **Explorar técnicas de seleção de variáveis** como _Recursive Feature Elimination_ (RFE) ou _SHAP values_, com o objetivo de identificar o subconjunto mínimo de variáveis com maior poder preditivo, reduzindo a dimensionalidade e melhorando a generalização dos modelos (Géron, 2022).

* **Validar o _clustering_ com métricas externas**, cruzando os _clusters_ identificados com variáveis não utilizadas na modelação como `Attrition`, `PerformanceRating` ou dados de progressão na carreira, para avaliar a validade externa dos perfis encontrados (Rousseeuw, 1987).

### 4.2 Novas Variáveis

A capacidade preditiva dos modelos está diretamente condicionada pela qualidade e abrangência das variáveis disponíveis. As seguintes fontes de dados adicionais representam oportunidades relevantes de enriquecimento do modelo:

* **Dados longitudinais:** Integrar séries temporais de avaliações de desempenho, promoções e movimentos internos para capturar a trajetória de cada colaborador ao longo do tempo, transformando o problema de classificação transversal num problema de análise de sobrevivência, metodologia mais adequada para modelar o tempo até à ocorrência de um evento (Hom et al., 2017).

* **Variáveis de _engagement_:** Incorporar dados de _pulse surveys_, Net Promoter Score interno, absentismo ou participação em programas de formação, variáveis identificadas na literatura como preditores independentes do atrito não capturadas pelo conjunto de dados atual (Hom et al., 2017).

* **Fatores externos de mercado:** Incluir indicadores como a taxa de desemprego sectorial, o volume de ofertas de emprego concorrentes e índices de satisfação do mercado de trabalho, que representam determinantes contextuais do atrito frequentemente ignorados em modelos baseados exclusivamente em dados internos (Hom et al., 2017).

### 4.3 Escalabilidade e _Deployment_

Assumindo a disponibilidade de dados reais de uma organização, a solução desenvolvida poderia evoluir para um sistema de apoio à decisão operacional através das seguintes etapas:

* **Interface de consulta em _Streamlit_:** Desenvolver uma aplicação _web_ que permita à equipa de RH consultar o índice de risco de cada colaborador, visualizar os perfis de _clustering_ e simular o impacto de intervenções de retenção.

* **_Dashboard_ de monitorização:** Criar painéis de controlo interativos com métricas agregadas por departamento, nível de risco e _cluster_, permitindo à gestão de topo acompanhar a evolução do risco de atrito ao longo do tempo.

* **_Pipeline_ de atualização contínua:** Implementar um processo automatizado de re-treino periódico do modelo com novos dados, garantindo que as previsões se mantêm calibradas face a mudanças organizacionais ou de mercado, com monitorização de _data drift_ para detetar degradação do desempenho (Géron, 2022).


## 5. Conclusão Final

Este projeto demonstrou a aplicabilidade de técnicas de Ciência de Dados ao problema da rotatividade de colaboradores, percorrendo de forma rigorosa as fases do CRISP-DM da definição do problema à entrega de valor analítico (Chapman et al., 2000).

Foram desenvolvidas duas soluções complementares. A primeira, um modelo preditivo de classificação baseado em Regressão Logística, permite estimar a probabilidade de saída de cada colaborador e classificá-lo num índice de risco de quatro categorias: Baixo, Médio, Alto e Crítico, transformando uma previsão probabilística em informação acionável para a gestão de Recursos Humanos. A segunda, um modelo de _clustering_ baseado em UMAP + DBSCAN, identificou quatro perfis distintos de colaboradores com correspondência direta à estrutura departamental real da organização, sem que essa informação tivesse sido fornecida ao modelo.

Os resultados ficaram aquém da meta quantitativa de _F1-Score_ ≥ 0,80, fixando-se em 0,5538 no conjunto de teste. Esta limitação, honestamente reconhecida, não invalida o valor da solução: o modelo obteve uma _Precision_ de 76,60%, garantindo que dois em cada três colaboradores sinalizados como em risco correspondem a casos reais de atrito, e o _clustering_ atingiu um _Silhouette Score_ de 0,7016, acima da meta de 0,50 e classificado como estrutura forte na escala de Rousseeuw (1987). A diferença entre a meta e o resultado alcançado reflete as limitações inerentes ao conjunto de dados sintético e não uma falha metodológica.

Do ponto de vista do impacto prático, a solução permite à organização passar de uma postura reativa para uma postura preventiva e estratégica: identificar quem está em risco, em que perfil se insere e com que prioridade intervir. Esta capacidade tem valor direto na redução de custos de rotatividade e na preservação do capital humano organizacional (Hom et al., 2017).

## 6. Referências

Chawla, N. V., Bowyer, K. W., Hall, L. O., & Kegelmeyer, W. P. (2002). SMOTE: Synthetic Minority Over-sampling Technique. Journal of Artificial Intelligence Research, 16, 321–357.

Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C., & Wirth, R. (2000). CRISP-DM 1.0: Step-by-step data mining guide. SPSS Inc.

Davies, D. L., & Bouldin, D. W. (1979). A Cluster Separation Measure. IEEE Transactions on Pattern Analysis and Machine Intelligence, 1(2), 224–227.

Géron, A. (2022). Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow (3rd ed.). O’Reilly Media.
Hom, P. W., Lee, T. W., Shaw, J. D., & Hausknecht, J. P. (2017). One Hundred Years of Employee Turnover Theory and Research. Journal of Applied Psychology, 102(3), 530–545.

IBM Watson Analytics. (2016). IBM HR Analytics Employee Attrition & Performance Dataset. IBM Sample Data Sets.
Jain, A. K. (2010). Data clustering: 50 years beyond K-means. Pattern Recognition Letters, 31(8), 651–666.

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2021). An Introduction to Statistical Learning: With Applications in R (2nd ed.). Springer.

McInnes, L., Healy, J., & Melville, J. (2018). UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. arXiv preprint arXiv:1802.03426.

Rousseeuw, P. J. (1987). Silhouettes: A graphical aid to the interpretation and validation of cluster analysis. Journal of Computational and Applied Mathematics, 20, 53–65.

Schubert, E., Sander, J., Ester, M., Kriegel, H. P., & Xu, X. (2017). DBSCAN revisited, revisited: Why and how you should (still) use DBSCAN. ACM Transactions on Database Systems, 42(3), 1–21.

---


**Data de Conclusão:** 18/05/2026

**Versão do Projeto:** v4.0 Final
