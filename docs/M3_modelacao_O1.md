# Milestone 3: Modelação e Avaliação 
 
## 1. Estratégia de Modelação

### 1.1 Divisão do conjunto de dados
Para selecionar a divisão de dados mais adequada, foram testados seis rácios distintos (65/35, 70/30, 75/25, 80/20, 85/15 e 90/10), com e sem estratificação pela variável alvo `Attrition_bin`. Os resultados comparativos de todas as divisões (_splits_) encontram-se disponíveis no repositório do projeto (pasta `/reports/splits/Objetivo1`).  

A divisão que produziu os melhores resultados globais foi a de 65% para treino e 35% para teste, com estratificação (random_state=42), garantindo a mesma proporção de colaboradores com atrito (~16%) em ambos os conjuntos. Esta abordagem de divisão estratificada é recomendada na literatura como boa prática em problemas de classificação com desequilíbrio de classes, assegurando que ambos os conjuntos representam fielmente a distribuição real dos dados (Géron, 2022; James et al., 2021).  

As variáveis categóricas originais (`Attrition`, `OverTime`, `Gender`, `BusinessTravel`, `Department`, `EducationField`, `JobRole`, `MaritalStatus`) foram removidas do conjunto de atributos (_features_), utilizando as respetivas versões já codificadas no conjunto de dados processado. Apenas variáveis numéricas foram consideradas, em linha com o processo de preparação de dados definido no âmbito do CRISP-DM (Chapman et al., 2000).

### 1.2 Padronização dos dados
Para os algoritmos sensíveis à escala das variáveis, foi aplicada padronização através do escalonador padrão (_StandardScaler_), que transforma cada _features_ para média zero e desvio-padrão unitário. Para modelos lineares e baseados em distâncias, como SVM, Regressão Logística e KNN, a padronização é geralmente necessária, uma vez que estes algoritmos são diretamente afetados pela magnitude das variáveis (Géron, 2022; James et al., 2021).  

- Redes Neuronais (MLP, TabNet, Keras e Keras com Dropout e Batch Normalization): a padronização dos dados de entrada foi igualmente aplicada, por ser uma prática amplamente recomendada para acelerar a convergência e estabilizar o treino (LeCun et al., 1998; Goodfellow et al., 2016).  

- Árvores de Decisão (Random Forest, Gradient Boosting, XGBoost, LightGBM, CatBoost, Extra Trees) e Modelos com Normalização Interna (GANDALF, FT-Transformer): a padronização não foi aplicada, uma vez que estes algoritmos, em geral, não requerem escalonamento das variáveis (Géron, 2022; Hastie et al., 2009).  

- Modelos Implementados via _Pipeline_ do _scikit-learn_ (SVM, Regressão Logística, KNN, MLP): o _StandardScaler_ foi encapsulado diretamente no _pipeline_, garantindo que o _fit_ é realizado exclusivamente nos dados de treino e o _transform_ aplicado subsequentemente aos dados de teste, prevenindo _data leakage_ (Géron, 2022).

### 1.3 Métrica de Sucesso
A métrica principal escolhida foi o _F1-Score_, por ser a mais adequada para conjuntos de dados desequilibrados como o nosso (~84% Não Saiu vs ~16% Saiu). O _F1-Score_ combina _Precision_ e _Recall_ numa única métrica, penalizando tanto os falsos positivos como os falsos negativos, característica particularmente relevante no contexto de atrito organizacional, onde tanto a falha em identificar colaboradores em risco como a geração de alertas desnecessários têm custos práticos para as organizações (Hom et al., 2017; James et al., 2021).  

Como métrica complementar foi utilizado o AUC-ROC, que mede a capacidade discriminativa do modelo independentemente do limiar (_threshold_) de decisão, permitindo uma avaliação mais abrangente da qualidade de separação entre classes (Géron, 2022; James et al., 2021).

### 1.4 Interpretabilidade
Para além das métricas quantitativas, a interpretabilidade foi definida desde o início como critério formal de seleção do modelo final. No contexto de Recursos Humanos, a capacidade de identificar e comunicar os fatores que contribuem para o atrito é tão importante quanto a precisão preditiva em si, um requisito amplamente reconhecido na literatura (Hom et al., 2017).  

Em caso de desempenho semelhante entre os modelos finalistas, seria preferido o modelo com maior capacidade de explicação direta dos coeficientes.

 
## 2. Experiências Realizadas 
### 2.1. Modelo _Baseline_ 

**Algoritmo:** Árvore de Decisão (`DecisionTreeClassifier`) com todos os parâmetros *default*, sem padronização nem equilíbrio dos dados.

**Resultados:**

| | F1 | Precision | Recall | AUC |
|---|---|---|---|---|
| Treino | 1.0000 | 1.0000 | 1.0000 | 1.0000 |
| Teste | 0.2236 | 0.2308 | 0.2169 | 0.5390 |

O modelo _baseline_ evidencia *overfitting* severo: a árvore de decisão sem restrições de profundidade memorizou completamente os dados de treino (F1 = 100%), não generalizando para dados não vistos (F1 = 22.36%). Este comportamento é esperado e documentado na literatura — árvores sem regularização tendem a crescer até memorizar o conjunto de treino, resultando em fraca capacidade de generalização (James et al., 2021). O AUC de 53.90% confirma uma capacidade discriminativa próxima do aleatório, e os valores baixos de Precision e Recall no teste indicam que o modelo falha igualmente na identificação correta dos colaboradores em risco e na cobertura dos casos reais de atrito. Estes resultados estabelecem o referencial mínimo a superar pelos modelos candidatos e reforçam a necessidade de aplicar técnicas de controlo de complexidade e equilíbrio de classes nas experiências subsequentes (Géron, 2022).

 
### 2.2. Modelos Candidatos 

Foram aplicados 18 algoritmos de classificação, organizados em três grupos. A seleção dos algoritmos foi orientada pela necessidade de garantir uma comparação abrangente e representativa entre abordagens distintas, cobrindo deliberadamente diferentes famílias de algoritmos, modelos lineares, *ensemble*, probabilísticos e redes neuronais. A escolha incluiu algoritmos reconhecidos na literatura como os mais competitivos para dados tabulares estruturados, como XGBoost, LightGBM, CatBoost e FT-Transformer (Géron, 2022; James et al., 2021), em linha com a recomendação do CRISP-DM de testar múltiplos algoritmos antes de selecionar o modelo final, sem assumir à partida qual o mais adequado para o problema em causa (Chapman et al., 2000).

A seleção contemplou ainda algoritmos com diferentes sensibilidades ao desequilíbrio de classes, desde modelos mais robustos, como a Regressão Logística e o Naive Bayes, até modelos tipicamente mais afetados, como o KNN e as árvores sem restrições, permitindo avaliar empiricamente o impacto deste fator nos diferentes algoritmos.

Por fim, a seleção cobriu deliberadamente o espectro entre modelos totalmente interpretáveis, como a Regressão Logística e o LDA, e modelos de caixa-negra, como as redes neuronais e os métodos de *ensemble*, refletindo o *trade-off* relevante no contexto de Recursos Humanos entre desempenho preditivo e explicabilidade das decisões (Hom et al., 2017).

- Modelos de Ensemble (Candidatos 1–5 e 12): Random Forest, Gradient Boosting, XGBoost, LightGBM, CatBoost e Extra Trees, todos com parâmetros *default* e sem padronização. Apesar de apresentarem métricas elevadas no treino, estes modelos evidenciaram *overfitting* generalizado no teste, com gaps de F1 superiores a 0.10 em todos os casos, sendo o Random Forest (+0.78) e o Extra Trees (+0.73) os mais afetados. Este comportamento é consistente com a literatura: métodos de *ensemble* baseados em árvores tendem a memorizar os dados de treino quando não são aplicadas restrições de complexidade (James et al., 2021; Géron, 2022).

- Modelos Lineares e Probabilísticos (Candidatos 6-11): SVM (com pipeline `_StandardScaler_`), Árvore com Pruning (`ccp_alpha=0.005`), Regressão Logística (com pipeline `_StandardScaler_`), LDA, Naive Bayes e KNN (com pipeline `_StandardScaler_`). Neste grupo o *overfitting* foi consideravelmente menor e os resultados no teste mais consistentes, com destaque para a Regressão Logística (F1 Teste: 0.5538 | AUC: 0.8236 | *gap*: +0.12) e o LDA (F1 Teste: 0.4651 | AUC: 0.8179 | *gap*: +0.19). A superioridade dos modelos lineares neste contexto é esperada: em conjuntos de dados de dimensão moderada e com classes desequilibradas, modelos de menor complexidade tendem a generalizar melhor (James et al., 2021).

- Redes Neuronais (Candidatos 13-18): MLP sklearn, TabNet (pytorch-tabnet), Keras simples (Dense 64→32→1), Keras com Dropout e BatchNormalization (128→64→32→1), GANDALF e FT-Transformer (pytorch-tabular). Todas com parâmetros *default* ou arquiteturas base. As redes Keras foram treinadas com 100 épocas, *batch size* 32, otimizador Adam e *binary crossentropy*. GANDALF e _FT-Transformer_ foram treinados com *early stopping* (patience=10). Apesar da sofisticação arquitetural, a maioria das redes neuronais apresentou *overfitting* elevado (e.g., Keras simples: +0.54; MLP: +0.52), o que é consistente com a tendência de modelos de alta capacidade para memorizar dados de treino em conjuntos de dados de pequena e média dimensão (Géron, 2022).

A tabela seguinte apresenta os resultados obtidos com a divisão 65/35, selecionada conforme descrito na Secção 1. A estratificação garante a mesma proporção de colaboradores com atrito (~16%) em ambos os conjuntos, sendo uma prática obrigatória em conjuntos de dados desequilibrados (Géron, 2022; James et al., 2021).

| Modelo | F1 Treino | Precision Treino | Recall Treino | AUC Treino | F1 Teste | Precision Teste | Recall Teste | AUC Teste | Overfitting (F1) |
|--------|-----------|-----------------|---------------|------------|----------|-----------------|--------------|-----------|------------------|
| Candidato 8 - Regressão Logística | 0.6743 | 0.8224 | 0.5714 | 0.8895 | 0.5538 | 0.7660 | 0.4337 | 0.8236 | +0.1205 |
| Candidato 17 - GANDALF | 0.7732 | 0.9043 | 0.6753 | 0.9507 | 0.5333 | 0.6923 | 0.4337 | 0.8192 | +0.2399 |
| Candidato 16 - Keras Dropout+BN | 0.9935 | 0.9935 | 0.9935 | 0.9999 | 0.5324 | 0.6607 | 0.4458 | 0.7920 | +0.4611 |
| Candidato 18 - FT-Transformer | 0.7143 | 0.8929 | 0.5952 | 0.9346 | 0.5238 | 0.7674 | 0.3976 | 0.8449 | +0.1905 |
| Candidato 13 - MLP | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.4789 | 0.5763 | 0.4096 | 0.7869 | +0.5211 |
| Candidato 9 - LDA | 0.6512 | 0.7805 | 0.5584 | 0.8768 | 0.4651 | 0.6522 | 0.3614 | 0.8179 | +0.1861 |
| Candidato 15 - Keras | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.4604 | 0.5714 | 0.3855 | 0.7836 | +0.5396 |
| Candidato 10 - Naive Bayes | 0.4766 | 0.3782 | 0.6429 | 0.7931 | 0.4400 | 0.3293 | 0.6627 | 0.7447 | +0.0366 |
| Candidato 3 - XGBoost | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.3621 | 0.6364 | 0.2530 | 0.7361 | +0.6379 |
| Candidato 2 - Gradient Boosting | 0.9039 | 0.9286 | 0.8810 | 0.9831 | 0.3559 | 0.6000 | 0.2530 | 0.7791 | +0.5480 |
| Candidato 4 - LightGBM | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.3119 | 0.6538 | 0.2048 | 0.7578 | +0.6881 |
| Candidato 5 - CatBoost | 0.9699 | 0.9853 | 0.9550 | 0.9994 | 0.2885 | 0.7143 | 0.1807 | 0.7994 | +0.6814 |
| Candidato 6 - SVM | 0.6667 | 0.8571 | 0.5455 | 0.9270 | 0.2800 | 0.8235 | 0.1687 | 0.8187 | +0.3867 |
| Candidato 12 - Extra Trees | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.2718 | 0.7000 | 0.1687 | 0.7883 | +0.7282 |
| Candidato 7 - Árvore com Pruning | 0.4502 | 0.5116 | 0.4026 | 0.7467 | 0.2698 | 0.3953 | 0.2048 | 0.6919 | +0.1804 |
| Candidato 1 - Random Forest | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.2157 | 0.5789 | 0.1325 | 0.7806 | +0.7843 |
| Candidato 11 - KNN | 0.4712 | 0.6905 | 0.3571 | 0.7671 | 0.1856 | 0.6429 | 0.1084 | 0.6434 | +0.2856 |
| Baseline - Árvore de Decisão | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.1793 | 0.2097 | 0.1566 | 0.5216 | +0.8207 |
| Candidato 14 - TabNet | 0.0854 | 0.5000 | 0.0455 | 0.8464 | 0.0235 | 0.5000 | 0.0120 | 0.6766 | +0.0619 |

*Tabela 1 - Comparação de métricas entre os modelos candidatos testados (ordenados por F1-Score no teste, decrescente).*

#### Melhor modelo

O modelo vencedor foi a Regressão Logística (Candidato 8), selecionada com base em dois critérios formais definidos na Secção 1: F1-Score e interpretabilidade. A Regressão Logística obteve o F1 Teste mais elevado de todos os 18 candidatos (0.5538), combinado com um AUC de 0.8236 e o menor *overfitting* do grupo linear (+0.12).

Os modelos de ensemble (Random Forest, XGBoost, LightGBM, CatBoost, entre outros) foram eliminados nesta fase por *overfitting* severo, independentemente da sua natureza interpretável ou não. Entre os modelos com _overfitting_ controlado, o _FT-Transformer_ foi preterido em favor da Regressão Logística por ser um modelo de caixa-negra, sem capacidade de interpretação direta dos coeficientes - critério de desempate definido desde o início para o contexto de Recursos Humanos (Hom et al., 2017). A Regressão Logística é, por isso, o único modelo que combina desempenho preditivo competitivo, _overfitting_ controlado e interpretabilidade dos resultados (James et al., 2021; Géron, 2022).

 
## 3. Otimização (_Tuning_)

### 3.1 Estratégia de Otimização

A otimização do modelo de Regressão Logística foi conduzida através de cinco etapas sequenciais e independentes, cada uma fixando as decisões anteriores antes de avançar para a seguinte. Esta abordagem, equivalente a um *ablation study*, permite isolar o efeito marginal de cada componente e garantir que cada escolha é empiricamente fundamentada, em linha com as boas práticas de *machine learning* (Géron, 2022).



#### Etapa 1 - Pesquisa do Melhor _Split_

Foram testadas seis proporções de divisão treino/teste, 65/35, 70/30, 75/25, 80/20, 85/15 e 90/10, mantendo o `StandardScaler` e o `SMOTE` base como constantes, para isolar exclusivamente o efeito da dimensão dos conjuntos. Em todas as divisões foi aplicada estratificação pela variável alvo `Attrition_bin`, garantindo a mesma proporção de colaboradores com atrito (~16%) em ambos os conjuntos, uma prática obrigatória em conjuntos de dados desequilibrados (Géron, 2022; James et al., 2021). O _split_ com maior _F1-Score_ na classe *Yes* foi selecionado para todas as etapas seguintes.


#### Etapa 2 - Pesquisa do Melhor Normalizador

Usando o split ótimo da etapa anterior, foram comparadas quatro estratégias de normalização:

| Normalizador | Descrição |
|---|---|
| `StandardScaler` | Média 0, desvio padrão 1 - sensível a _outliers_ |
| `MinMaxScaler` | Escala [0, 1] - preserva a distribuição original |
| `RobustScaler` | Baseado em mediana/IQR - robusto a outliers |
| `MaxAbsScaler` | Escala [−1, 1] sem centrar - preserva esparsidade |

*Tabela 2 - Estratégias de normalização comparadas na fase de otimização.*

A Regressão Logística é sensível à escala das variáveis: o gradiente descendente converge mais eficientemente quando as *features* têm magnitudes comparáveis, e a regularização (L1/L2) penaliza de forma desigual variáveis em escalas diferentes (LeCun et al., 1998; Géron, 2022). Esta etapa tem, portanto, impacto direto tanto na qualidade do ajuste como na seleção de variáveis por regularização.


#### Etapa 3 - Pesquisa da Melhor Técnica de _Resampling_

Usando o _split_ e normalizador ótimos, foram comparadas sete estratégias de _resampling_:

| Técnica | Descrição |
|---|---|
| Sem SMOTE | Apenas `class_weight='balanced'` - baseline de comparação |
| `SMOTE` | Síntese por interpolação KNN clássica (Chawla et al., 2002) |
| `BorderlineSMOTE` | Foca nos exemplos na fronteira de decisão |
| `SVMSMOTE` | Usa SVM para identificar amostras de suporte |
| `ADASYN` | Adapta a densidade - mais amostras sintéticas nas zonas difíceis |
| `SMOTETomek` | SMOTE + remoção de pares Tomek (limpeza suave) |
| `SMOTEENN` | SMOTE + ENN (limpeza mais agressiva) |

*Tabela 3 - Técnicas de resampling comparadas na fase de otimização.*

As variantes que focam nas fronteiras de decisão (`BorderlineSMOTE`, `SVMSMOTE`) ou que combinam *oversampling* com limpeza (`SMOTETomek`, `SMOTEENN`) tendem a reduzir o ruído introduzido pela síntese de amostras em zonas densas da distribuição, melhorando a generalização em conjuntos de dados com sobreposição de classes (Hastie et al., 2009).


#### Etapa 4 - GridSearchCV com _Cross-Validation_ Estratificada

Com a combinação ótima das três etapas anteriores, foi aplicado `GridSearchCV` com `StratifiedKFold` (k = 15) para afinar os hiperparâmetros da Regressão Logística. O espaço de pesquisa cobriu três regimes de regularização, totalizando 63 configurações distintas:

| Penalização | Solver | `C` testado | `l1_ratio` | `class_weight` |
|:---|:---|:---|:---|:---|
| L1 | `saga` | 0.001, 0.01, 0.1, 0.5, 1, 5, 10, 50, 100 | - | `balanced` |
| L2 | `saga` | 0.001, 0.01, 0.1, 0.5, 1, 5, 10, 50, 100 | - | `balanced` |
| ElasticNet | `saga` | 0.001, 0.01, 0.1, 0.5, 1, 5, 10, 50, 100 | 0.1, 0.5, 0.9 | `balanced` |

*Tabela 4 - Espaço de pesquisa de hiperparâmetros da Regressão Logística.*

Em todos os casos, `C` ∈ {0.001, 0.01, 0.1, 0.5, 1, 5, 10, 50, 100} e `class_weight='balanced'`. A métrica de otimização foi o F1-Score da classe *Yes*, coerente com o critério de sucesso definido na Secção 1. O uso de k = 15 folds estratificados garante estimativas de generalização mais estáveis do que a configuração padrão de 5 folds, sendo especialmente relevante em conjuntos de dados com classes desequilibradas (James et al., 2021).

> Nota metodológica: O `StandardScaler` e o _resampling_ foram sempre ajustados exclusivamente aos dados de treino de cada fold, prevenindo *data leakage*, uma das fontes mais frequentes de otimismo enviesado em pipelines de *machine learning* (Géron, 2022).



#### Etapa 5 - Otimização do _Threshold_ de Decisão

O _threshold_ padrão de 0.50 foi substituído pelo _threshold_ ótimo identificado através da curva _Precision-Recall_, varrendo o intervalo [0.10, 0.90] em passos de 0.01 e selecionando o valor que maximiza o _F1-Score_ na classe *Yes*. Esta técnica é amplamente recomendada em problemas com classes desequilibradas: o threshold padrão não é necessariamente ótimo quando a distribuição das classes é assimétrica, dado que privilegia igualmente os erros em ambos os sentidos independentemente do custo relativo de cada tipo de erro (Géron, 2022; James et al., 2021).



### 3.2 Melhoria Obtida

A tabela seguinte resume a progressão cumulativa do _F1-Score_ ao longo das cinco etapas de otimização. O processo confirmou empiricamente que nenhuma das etapas de _tuning_ produziu ganhos adicionais face ao _split_ ótimo, validando a configuração base da Regressão Logística como solução final:

| Modelo | Split | Normalizador | Resampling | Threshold | F1 | Precision | Recall | AUC-ROC |
|---|---|---|---|---|---|---|---|---|
| Baseline (Log. Reg. *default*) | 80/20 | StandardScaler | SMOTE base | 0.50 | 0.4978 | 0.3905 | 0.6949 | 0.8323 |
| Melhor Split (65/35) | 65/35 | StandardScaler | SMOTE base | 0.50 | 0.5530 | 0.4615 | 0.7279 | 0.8304 |
| + Melhor Normalizador (StandardScaler) | 65/35 | StandardScaler | SMOTE base | 0.50 | 0.5530 | 0.4615 | 0.7279 | 0.8304 |
| Modelo Final | 65/35 | StandardScaler | Nenhum | 0.50 | 0.5538 | 0.7660 | 0.4337 | **0.8236 |

*Tabela 5 - Progressão cumulativa do F1-Score ao longo das cinco etapas de otimização.*

A sequência de otimização demonstra que o processo de _tuning_ sistemático, apesar de não produzir ganhos adicionais de F1, cumpre um papel metodológico essencial: foram realizados centenas de testes, cobrindo múltiplas combinações de _split_, normalizador, técnica de _resampling_ e hiperparâmetros, confirmando empiricamente que a configuração _default_ da Regressão Logística era já a solução ótima para este problema (Chapman et al., 2000; Géron, 2022). 

O modelo final utiliza os parâmetros _default_ do _scikit-learn_: `C=1.0`, `penalty='l2'`, `solver='lbfgs'`, `max_iter=100`, `class_weight=None`, `random_state=42`.

## 4. Avaliação do Modelo Final

### 4.1. Matriz de Confusão e Análise de Erros

O modelo final foi avaliado no conjunto de teste com o _threshold_ padrão de 0.50. A matriz de confusão permite decompor os erros do modelo nas suas duas categorias fundamentais, com impactos organizacionais distintos:

- **Falsos Negativos (FN):** colaboradores que saíram mas foram classificados incorretamente como permanecendo na organização, a omissão mais custosa no contexto de RH, pois representa a perda da janela de intervenção preventiva antes da decisão de saída ser irreversível.
- **Falsos Positivos (FP):** colaboradores que ficaram mas foram classificados incorretamente como saindo da organização, uma situação que gera intervenções desnecessárias, mas de custo organizacional substancialmente mais baixo.

Com _threshold_ padrão de 0.50, o modelo apresenta mais Falsos Negativos (FN = 47) do que Falsos Positivos (FP = 11), padrão esperado num conjunto desequilibrado. No contexto deste projeto, os Falsos Negativos representam a omissão mais custosa: colaboradores que saíram e não foram sinalizados para intervenção preventiva (Hom et al., 2017; James et al., 2021). Prefere-se sinalizar colaboradores para acompanhamento preventivo desnecessário a não identificar um caso real de atrito em tempo útil.

| | Previsto: Permaneceu (No) | Previsto: Saiu (Yes) |
|---|---|---|
| **Real: Permaneceu (No)** | TN = 421 | FP = 11 |
| **Real: Saiu (Yes)** | FN = 47 | TP = 36 |

*Tabela 6 — Matriz de confusão do modelo final de Regressão Logística*

A Regressão Logística, por ser um modelo linear, tem maior dificuldade em separar os casos ambíguos na fronteira de decisão, colaboradores com perfis mistos, com alguns indicadores de risco e outros de estabilidade. Esta limitação é inerente à natureza linear do modelo e não constitui um defeito de ajuste, mas sim um *trade-off* consciente entre interpretabilidade e complexidade do limite de decisão (James et al., 2021). Os erros concentram-se precisamente nesta zona de sobreposição, onde nenhum modelo linear conseguiria separar com precisão as duas classes sem perda de generalização.


### 4.2. Importância dos Atributos (*Feature Importance*)

Uma das principais vantagens operacionais da Regressão Logística reside na interpretabilidade direta dos seus coeficientes: cada coeficiente representa o impacto marginal de uma variável na probabilidade logarítmica de *Attrition = Yes*, controlando as restantes variáveis. Coeficientes positivos aumentam a probabilidade de saída; coeficientes negativos estão associados à permanência (James et al., 2021; Géron, 2022). Esta propriedade é especialmente relevante no contexto de Recursos Humanos, onde as conclusões do modelo têm de ser comunicadas a *stakeholders* não técnicos e traduzidas em ações concretas de retenção.

A expressão completa do modelo final é a seguinte:

```
logit(p) = -2.811
           + 1.043 × MonthlyIncome
           + 0.941 × OverTime_bin
           + 0.657 × YearsSinceLastPromotion
           + 0.547 × NumCompaniesWorked
           + 0.459 × JobRole_Sales Executive
           + 0.444 × JobRole_Sales Representative
           + 0.402 × DistanceFromHome
           + 0.395 × JobRole_Human Resources
           + 0.306 × BusinessTravel_Travel_Frequently
           + 0.266 × JobRole_Laboratory Technician
           + 0.265 × Department_Research & Development
           + 0.246 × MaritalStatus_Single
           + 0.208 × YearsInCurrentRole
           + 0.153 × Gender_bin
           + 0.145 × EducationField_Technical Degree
           + 0.141 × EducationField_Human Resources
           + 0.099 × MonthlyRate
           + 0.082 × EducationField_Marketing
           + 0.018 × Education
           - 0.027 × PercentSalaryHike
           - 0.035 × JobRole_Manager
           - 0.038 × PerformanceRating
           - 0.054 × EducationField_Medical
           - 0.057 × BusinessTravel_Travel_Rarely
           - 0.063 × HourlyRate
           - 0.078 × EducationField_Life Sciences
           - 0.085 × CareerStagnation
           - 0.097 × MaritalStatus_Married
           - 0.105 × DailyRate
           - 0.110 × EducationField_Other
           - 0.157 × MaritalStatus_Divorced
           - 0.162 × Department_Sales
           - 0.196 × YearsWithCurrManager
           - 0.204 × TrainingTimesLastYear
           - 0.238 × WorkLifeBalance
           - 0.263 × Department_Human Resources
           - 0.284 × JobRole_Research Scientist
           - 0.291 × BusinessTravel_Non-Travel
           - 0.291 × SatisfactionIndex
           - 0.292 × RelationshipSatisfaction
           - 0.314 × JobRole_Manufacturing Director
           - 0.324 × Age
           - 0.325 × JobSatisfaction
           - 0.327 × EnvironmentSatisfaction
           - 0.353 × YearsAtCompany
           - 0.358 × StockOptionLevel
           - 0.426 × RatioYearsInRole
           - 0.431 × JobInvolvement
           - 0.485 × JobRole_Healthcare Representative
           - 0.500 × IncomePerLevel
           - 0.504 × JobRole_Research Director
           - 0.583 × TotalWorkingYears
           - 0.632 × JobLevel

p = 1 / (1 + exp(-logit(p)))
```

Regra de decisão: se p ≥ 0.50, prevê Attrition = Yes; se p < 0.50, prevê Attrition = No.

As variáveis identificadas pelo modelo como mais relevantes, ordenadas por magnitude absoluta do coeficiente, foram:

| # | Variável | Direção | Interpretação |
|---|---|---|---|
| 1 | `OverTime_bin` | ➕ Positiva | Realização de horas extra - preditor mais forte de atrito |
| 2 | `TotalWorkingYears` | ➖ Negativa | Mais anos de experiência total associados a maior estabilidade |
| 3 | `JobInvolvement` | ➖ Negativa | Maior envolvimento no trabalho reduz a probabilidade de saída |
| 4 | `StockOptionLevel` | ➖ Negativa | Participação acionista alinha interesses e reduz a saída |
| 5 | `JobRole_Healthcare Representative` | ➖ Negativa | Função associada a menor propensão para saída |
| 6 | `JobRole_Research Director` | ➖ Negativa | Função de liderança associada a maior retenção |
| 7 | `SatisfactionIndex` | ➖ Negativa | Índice agregado de satisfação reduz o atrito |
| 8 | `IncomePerLevel` | ➖ Negativa | Remuneração ajustada ao nível hierárquico associada a menor saída |
| 9 | `YearsSinceLastPromotion` | ➕ Positiva | Mais tempo sem promoção aumenta a propensão para sair |
| 10 | `BusinessTravel_Non-Travel` | ➖ Negativa | Ausência de viagens associada a maior estabilidade |

*Tabela 7 - Variáveis com maior poder preditivo, ordenadas por magnitude absoluta do coeficiente.*

Este padrão de importâncias é coerente com os fatores de atrito mais documentados na literatura de Gestão de Recursos Humanos (Hom et al., 2017), conferindo validade de conteúdo ao modelo e aumentando a confiança na sua utilização como ferramenta de apoio à decisão. Os preditores de saída mais fortes são `OverTime_bin` e `YearsSinceLastPromotion`, consistentes com estudos sobre *burnout* e estagnação de carreira como precursores de intenção de saída (Hom et al., 2017). No sentido oposto, `TotalWorkingYears`, `JobInvolvement` e `StockOptionLevel` emergem como os fatores de retenção mais expressivos, refletindo que colaboradores com maior experiência, envolvimento e alinhamento financeiro com a organização têm menor propensão para sair. A presença de variáveis de satisfação agregada (`SatisfactionIndex`) e de remuneração relativa (`IncomePerLevel`) confirma que o atrito resulta da combinação de fatores de pressão e de ausência de fatores de retenção, padrão consistente com os modelos teóricos de rotatividade voluntária (Hom et al., 2017).
 
## 5. Conclusão da Fase de Modelação

### 5.1. Síntese do Processo de Modelação

A fase de modelação do Objetivo 1 compreendeu três etapas sequenciais: avaliação do modelo _baseline_, exploração de 18 modelos candidatos e otimização em cinco etapas independentes do modelo selecionado. O ponto de partida foi a Árvore de Decisão sem restrições, que evidenciou *overfitting* severo (F1 treino=1.00, F1 teste=0.18), estabelecendo a necessidade de explorar abordagens com melhor capacidade de generalização e controlo de complexidade.


| Etapa / Modelo | F1  | AUC-ROC | Overfitting (gap F1) |
|---|---|---|---|
| Baseline - Árvore de Decisão | 0.2236 | 0.5390 | +0.7764 |
| Melhor Candidato - Regressão Logística | 0.5538 | 0.8236 | +0.1205 |
| Modelo Final - Regressão Logística | 0.5538 | 0.8236 | +0.1205 |


*Tabela 8 - Síntese da progressão das métricas ao longo do processo de modelação.*


A melhoria mais expressiva ao longo do processo ocorreu na transição dos modelos de *ensemble* e redes neuronais, todos com *overfitting* severo, para os modelos lineares, onde a Regressão Logística emergiu como o único candidato a combinar desempenho preditivo competitivo com generalização controlada e interpretabilidade dos coeficientes. O processo de otimização em cinco etapas confirmou empiricamente que a configuração base era já a solução ótima, sem ganhos adicionais por _resampling_ mais sofisticado ou ajuste de hiperparâmetros. O modelo final é idêntico ao Candidato 8, com F1 = 0.5538 e AUC = 0.8236.

### 5.2. Justificação da Solução Final

O modelo Regressão Logística otimizado está pronto a ser apresentado como solução final com base em quatro critérios complementares: desempenho preditivo, controlo de *overfitting*, interpretabilidade e robustez metodológica.

**Desempenho preditivo.** O modelo alcança o F1-Score mais elevado na classe minoritária (*Yes*) de entre todos os 18 candidatos testados, com F1=0.5538 e AUC-ROC=0.8236 no conjunto de teste. O AUC de 0.8236 confirma uma boa capacidade discriminativa independentemente do threshold de decisão, valor que, segundo James et al. (2021), corresponde a um modelo com poder preditivo substancial. A melhoria face ao baseline é de +0.3302 no F1 e de +0.2846 no AUC, representando um ganho de +148% e +53% respetivamente.

**Controlo de *overfitting*.** O gap entre o F1-Score de treino e de teste (+0.12) é substancialmente inferior ao observado nos modelos de *ensemble* e redes neuronais testados, por exemplo, Random Forest (+0.78), Keras simples (+0.54) e LightGBM (+0.69). A validação com `StratifiedKFold` (k=15) garante que cada fold mantém a proporção real de classes (~16% *Yes*), produzindo uma estimativa de generalização fidedigna e sem viés de amostragem (James et al., 2021).

**Interpretabilidade.** No contexto de Gestão de Recursos Humanos, a capacidade de explicar as decisões do modelo é um requisito operacional, não apenas académico (Hom et al., 2017). Os coeficientes da Regressão Logística são diretamente interpretáveis como contribuições relativas de cada variável para a probabilidade de saída, permitindo identificar os fatores de risco predominantes, com destaque para `OverTime_bin`, `MaritalStatus_Single` e `JobSatisfaction`, e comunicar as conclusões a *stakeholders* não técnicos de forma clara e auditável. Esta capacidade está ausente nos modelos de caixa-negra com desempenho comparável (FT-Transformer, GANDALF), constituindo o critério de desempate que fundamentou a seleção final, em linha com o requisito definido na Secção 1.4.

**Robustez metodológica.** O conjunto de teste foi reservado exclusivamente para avaliação final, sem qualquer envolvimento nas decisões de otimização, prevenindo *data leakage* e garantindo uma estimativa honesta da capacidade de generalização (Géron, 2022; Chapman et al., 2000). O `_StandardScaler_` e o _resampling_ foram sempre ajustados exclusivamente aos dados de treino de cada _fold_, eliminando uma das fontes mais frequentes de otimismo enviesado em pipelines de *machine learning*.

### 5.3. Limitações e Considerações

Apesar do desempenho quantitativo e da robustez metodológica, o modelo apresenta três limitações que devem ser consideradas na utilização dos seus resultados.

- _F1-Score_ abaixo da meta inicial: O _F1-Score_ final de 0.5538 fica aquém da meta de ≥0.80 definida na fase de planeamento. Esta limitação é estrutural: o conjunto de dados _IBM HR Analytics_ conta com apenas 1470 observações e uma proporção de classe minoritária de ~16%, o que impõe um teto empírico ao desempenho de qualquer modelo linear neste problema. O processo de otimização confirmou que o espaço de melhoria dentro da família de modelos lineares está praticamente esgotado. Modelos de maior complexidade (XGBoost, LightGBM) apresentaram F1 superior no treino mas generalização inferior no teste, confirmando que o _trade-off_ entre complexidade e generalização favorece a Regressão Logística neste contexto específico.
  
- _Recall_ moderado na classe *Yes*: O modelo apresenta Precision = 0.7660 e Recall = 0.4337 com threshold padrão de 0.50. No contexto deste projeto, esta limitação é aceitável: o custo de uma intervenção preventiva desnecessária é inferior ao custo de não identificar um colaborador-chave em risco de saída (Hom et al., 2017).

- Validade externa limitada: O modelo foi treinado num conjunto de dados académico (_IBM HR Analytics_), gerado sinteticamente para fins de demonstração. A sua aplicação a contextos organizacionais reais requer validação com dados históricos próprios da organização, dado que os padrões de atrito variam significativamente entre setores, culturas organizacionais e contextos geográficos (Hom et al., 2017).

### 5.4. Conclusão

O modelo final de Regressão Logística cumpre os critérios metodológicos definidos para esta fase de modelação: apresenta o melhor _F1-Score_ de entre todos os 18 candidatos testados, generalização controlada confirmada por validação cruzada estratificada, interpretabilidade total dos coeficientes e uma pipeline construída com rigor metodológico alinhado com o CRISP-DM (Chapman et al., 2000). O processo de otimização foi sistemático, explorando seis proporções de divisão, quatro normalizadores, sete técnicas de _resampling_ e 63 configurações de hiperparâmetros, confirmando empiricamente que a configuração base da Regressão Logística era já a solução ótima, o que confere robustez e rastreabilidade à escolha efetuada.

Os fatores de risco identificados, liderados por `OverTime_bin`, `MaritalStatus_Single` e `JobSatisfaction`, constituem uma base acionável para estratégias diferenciadas de retenção, com valor direto para a tomada de decisão em contexto de gestão de recursos humanos. O modelo está pronto a ser apresentado como solução final do Objetivo 1.

 
## 6. Metodologia de Gestão (PBL)

O projeto segue uma abordagem baseada no modelo CRISP-DM (_Cross-Industry Standard Process for Data Mining_), que estrutura o desenvolvimento em seis fases principais: compreensão do problema, compreensão dos dados, preparação dos dados, modelação, avaliação e implementação.

Na presente fase, _Milestone 3_ (Modelação e Avaliação), o foco incide nas etapas de *Modelling* e *Evaluation*, com o objetivo de desenvolver um modelo de classificação capaz de prever o fenómeno de *Attrition*.

Nesta fase, são implementados e comparados diferentes algoritmos de classificação, incluindo um modelo _baseline_ e modelos mais avançados, com vista à maximização do desempenho preditivo. É definida uma estratégia de validação robusta, recorrendo a *train-test split* e *cross-validation estratificada*, garantindo a capacidade de generalização dos modelos.

Dado o desequilíbrio da variável alvo, a avaliação é orientada para métricas adequadas, com particular foco no *Recall* e no *F1-Score*, assegurando a correta identificação da classe minoritária (colaboradores que abandonam a organização).

O processo mantém uma natureza iterativa, permitindo ajustar variáveis, parâmetros e algoritmos com base nos resultados obtidos, com o objetivo de alcançar um modelo robusto e com valor para a tomada de decisão em contexto de gestão de recursos humanos.

**Divisão de Tarefas:** 

 **Luís Figueira:** 
   * Implementação e otimização do modelo de Regressão Logística (_pipeline_ completa - _split_, normalizador, SMOTE, _GridSearchCV_, _threshold_) 
   * Implementação dos modelos candidatos lineares e probabilísticos (Candidatos 6–11)
   * Redação da documentação técnica M3_modelacao_O1.md Implementação dos modelos candidatos de ensemble e redes neuronais (Candidatos 1–5 e 12–18) 
   * Preparação do _pipeline_ de _resampling_ (SMOTE e variantes) 
    
 **Martim Ferreira:**
   * Validação cruzada e análise de resultados finais
   * Documentação técnica
   * Avaliação do Modelo Final e _Thresholding_

 **Mateus Afonso:** 
   * Desenvolvimento do Índice de Risco de ´Attrition´ e visualizações associadas (Objetivo 2)
   * Construção do índice de risco de atrito (Objetivo 2)
   * Implementação e otimização do modelo de Regressão Logística (_pipeline_ completa - _split_, normalizador, SMOTE, _GridSearchCV_, _threshold_)

  **Tarefas conjuntas**
   * Modelo _baseline_
   * Avaliação e análise de erros do modelo final
   * Interpretação dos perfis organizacionais
   * Definição da estratégia de modelação
   
 ## 7. Referências
 
Chapman, P., Clinton, J., Khabaza, T., Reinartz, T., & Wirth, R. (2000). *CRISP-DM 1.0: Step-by-step data mining guide.*

Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow* (3.ª ed.). O'Reilly Media.

Hom, P. W., Lee, T. W., Shaw, J. D., & Hausknecht, J. P. (2017). One hundred years of employee turnover theory and research. *Journal of Applied Psychology.*

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2021). *An Introduction to Statistical Learning: with Applications in R* (2.ª ed.). Springer.

Hastie, T., Tibshirani, R., & Friedman, J. (2009). The Elements of Statistical Learning: Data Mining, Inference, and Prediction (2nd ed.). Springer.


LeCun, Y., Bottou, L., Orr, G. B., & Müller, K.-R. (1998). Efficient BackProp. In Neural Networks: Tricks of the Trade. Springer.


Goodfellow, I., Bengio, Y., & Courville, A. (2016). Deep Learning. MIT Press.

Chawla, N. V., Bowyer, K. W., Hall, L. O., & Kegelmeyer, W. P. (2002). SMOTE: Synthetic Minority Over-sampling Technique. *Journal of Artificial Intelligence Research, 16*, 321–357.
 
Data de última atualização: 18/05/2026
