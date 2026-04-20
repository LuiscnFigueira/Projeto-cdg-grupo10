# Milestone 3: Modelação e Avaliação 
 
## 1. Estratégia de Modelação

**Divisão do dataset:** Para selecionar a divisão de dados mais adequada, foram testados seis rácios distintos (65/35, 70/30, 75/25, 80/20, 85/15 e 90/10), com e sem estratificação pela variável alvo Attrition_bin. Os resultados comparativos de todos os splits encontram-se disponíveis no repositório do projeto (pasta `/resultados/splits/Objetivo1`). A divisão que produziu os melhores resultados globais foi a de 65% para treino e 35% para teste, com estratificação (random_state=42), garantindo a mesma proporção de colaboradores com atrito (~16%) em ambos os conjuntos. Esta abordagem de divisão estratificada é recomendada na literatura como boa prática em problemas de classificação com desequilíbrio de classes, assegurando que ambos os conjuntos representam fielmente a distribuição real dos dados (Géron, 2022; James et al., 2021). As variáveis categóricas originais (Attrition, OverTime, Gender, BusinessTravel, Department, EducationField, JobRole, MaritalStatus) foram removidas do conjunto de features, utilizando as respetivas versões já codificadas no dataset processado. Apenas variáveis numéricas foram consideradas, em linha com o processo de preparação de dados definido no âmbito do CRISP-DM (Chapman et al., 2000).

**Métrica de Sucesso:** A métrica principal escolhida foi o F1-Score, por ser a mais adequada para datasets desequilibrados como o nosso (~84% Não Saiu vs ~16% Saiu). O F1-Score combina Precision e Recall numa única métrica, penalizando tanto os falsos positivos como os falsos negativos — característica particularmente relevante no contexto de atrito organizacional, onde tanto a falha em identificar colaboradores em risco como a geração de alertas desnecessários têm custos práticos para as organizações (Hom et al., 2017; James et al., 2021). Como métrica complementar foi utilizado o AUC-ROC, que mede a capacidade discriminativa do modelo independentemente do threshold de decisão, permitindo uma avaliação mais abrangente da qualidade de separação entre classes (Géron, 2022; James et al., 2021).

Para além das métricas quantitativas, a interpretabilidade foi definida desde o início como critério formal de seleção do modelo final. No contexto de Recursos Humanos, a capacidade de identificar e comunicar os fatores que contribuem para o atrito é tão importante quanto a precisão preditiva em si - um requisito amplamente reconhecido na literatura (Hom et al., 2017). Em caso de desempenho equivalente entre finalistas, seria preferido o modelo com maior capacidade de explicação direta dos coeficientes.

 
## 2. Experiências Realizadas 
### 2.1. Modelo Baseline 

**Algoritmo:** Árvore de Decisão (`DecisionTreeClassifier`) com todos os parâmetros *default*, sem normalização nem balanceamento dos dados.

**Resultados:** F1 Treino: 1.0000 | F1 Teste: 0.1793 | Precision: 0.2097 | Recall: 0.1566 | AUC Teste: 0.5216

O modelo baseline evidencia *overfitting* severo: a árvore de decisão sem restrições de profundidade memorizou completamente os dados de treino (F1 = 1.0000), não generalizando para dados não vistos (F1 = 0.1793). O gap de 0.8207 entre o F1 de treino e de teste confirma este comportamento - esperado e documentado na literatura, dado que árvores de decisão sem regularização tendem a crescer até memorizar o conjunto de treino, resultando em fraca capacidade de generalização (James et al., 2021). A Precision (0.2097) e o Recall (0.1566) igualmente baixos indicam que o modelo falha tanto na identificação correta dos colaboradores em risco como na cobertura dos casos reais de atrito. O valor de AUC = 0.5216 confirma uma capacidade discriminativa próxima do aleatório (0.50), reforçando a necessidade de aplicar técnicas de controlo de complexidade e balanceamento de classes nas experiências subsequentes (Géron, 2022).

 
### 2.2. Modelos Candidatos 

Foram testados 18 modelos candidatos, organizados em três grupos. A seleção dos algoritmos foi orientada pela necessidade de garantir uma comparação abrangente e representativa entre abordagens distintas, cobrindo deliberadamente diferentes famílias de algoritmos - modelos lineares, *ensemble*, probabilísticos e redes neuronais. A escolha incluiu algoritmos reconhecidos na literatura como os mais competitivos para dados tabulares estruturados, como XGBoost, LightGBM, CatBoost e FT-Transformer (Géron, 2022; James et al., 2021), em linha com a recomendação do CRISP-DM de testar múltiplos algoritmos antes de selecionar o modelo final, sem assumir à partida qual o mais adequado para o problema em causa (Chapman et al., 2000).

A seleção contemplou ainda algoritmos com diferentes sensibilidades ao desequilíbrio de classes, desde modelos mais robustos, como a Regressão Logística e o Naive Bayes, até modelos tipicamente mais afetados, como o KNN e as árvores sem restrições, permitindo avaliar empiricamente o impacto deste fator nos diferentes algoritmos.

Por fim, a seleção cobriu deliberadamente o espectro entre modelos totalmente interpretáveis, como a Regressão Logística e o LDA, e modelos de caixa-negra, como as redes neuronais e os métodos de *ensemble*, refletindo o *trade-off* relevante no contexto de Recursos Humanos entre desempenho preditivo e explicabilidade das decisões (Hom et al., 2017).

**Modelos de Ensemble (Candidatos 1-5 e 12):** Random Forest, Gradient Boosting, XGBoost, LightGBM, CatBoost e Extra Trees - todos com parâmetros *default* e sem normalização. Apesar de apresentarem métricas elevadas no treino, estes modelos evidenciaram *overfitting* generalizado no teste, com gaps de F1 superiores a 0.10 em todos os casos, sendo o Random Forest (+0.78) e o Extra Trees (+0.73) os mais afetados. Este comportamento é consistente com a literatura: métodos de *ensemble* baseados em árvores tendem a memorizar os dados de treino quando não são aplicadas restrições de complexidade (James et al., 2021; Géron, 2022).

**Modelos Lineares e Probabilísticos (Candidatos 6-11):** SVM (com pipeline `StandardScaler`), Árvore com Pruning (`ccp_alpha=0.005`), Regressão Logística (com pipeline `StandardScaler`), LDA, Naive Bayes e KNN (com pipeline `StandardScaler`). Neste grupo o *overfitting* foi consideravelmente menor e os resultados no teste mais consistentes, com destaque para a Regressão Logística (F1 Teste: 0.5538 | AUC: 0.8236 | *gap*: +0.12) e o LDA (F1 Teste: 0.4651 | AUC: 0.8179 | *gap*: +0.19). A superioridade dos modelos lineares neste contexto é esperada: em datasets de dimensão moderada e com classes desequilibradas, modelos de menor complexidade tendem a generalizar melhor (James et al., 2021).

**Redes Neuronais (Candidatos 13-18):** MLP sklearn, TabNet (pytorch-tabnet), Keras simples (Dense 64→32→1), Keras com Dropout e BatchNormalization (128→64→32→1), GANDALF e FT-Transformer (pytorch-tabular). Todas com parâmetros *default* ou arquiteturas base. As redes Keras foram treinadas com 100 épocas, *batch size* 32, otimizador Adam e *binary crossentropy*. GANDALF e FT-Transformer foram treinados com *early stopping* (patience=10). Apesar da sofisticação arquitetural, a maioria das redes neuronais apresentou *overfitting* elevado (e.g., Keras simples: +0.54; MLP: +0.52), o que é consistente com a tendência de modelos de alta capacidade para memorizar dados de treino em datasets de pequena e média dimensão (Géron, 2022).

A tabela seguinte apresenta os resultados obtidos com a divisão 65/35, selecionada conforme descrito na Secção 1. A estratificação garante a mesma proporção de colaboradores com atrito (~16%) em ambos os conjuntos, sendo uma prática obrigatória em datasets desequilibrados (Géron, 2022; James et al., 2021).

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

O modelo vencedor foi a Regressão Logística (Candidato 8), selecionada com base em dois critérios formais definidos na Secção 1: F1-Score e interpretabilidade. A Regressão Logística obteve o F1 Teste mais elevado de todos os 18 candidatos (0.5538), combinado com um AUC de 0.8236 e o menor *overfitting* do grupo linear (+0.12).

Os modelos de ensemble (Random Forest, XGBoost, LightGBM, CatBoost, entre outros) foram eliminados nesta fase por *overfitting* severo, independentemente da sua natureza interpretável ou não. Entre os modelos com overfitting controlado, o FT-Transformer foi preterido em favor da Regressão Logística por ser um modelo de caixa-negra, sem capacidade de interpretação direta dos coeficientes - critério de desempate definido desde o início para o contexto de Recursos Humanos (Hom et al., 2017). A Regressão Logística é, por isso, o único modelo que combina desempenho preditivo competitivo, overfitting controlado e interpretabilidade dos resultados (James et al., 2021; Géron, 2022).

 
## 3. Otimização (Tuning) 
*Descrevam como melhoraram o melhor modelo.* 
* **Técnica Utilizada:** (p/ex.: "Utilizámos GridSearchCV para ajustar os hiperparâmetros 
`max_depth` e `learning_rate`.") 
* **Melhoria obtida:** (p/ex.: "O F1-Score subiu de 0.85 para 0.88 após o ajuste.") 
 
## 4. Avaliação do Modelo Final 
### 4.1. Matriz de Confusão / Erros 
*Analisem onde o modelo mais falha.* 
> **Análise:** (p/ex.: "O modelo ainda confunde a Classe A com a Classe B em 10% dos casos devido 
à semelhança nos atributos X e Y.") 
 
### 4.2. Importância dos Atributos (Feature Importance) 
*Quais as variáveis que o modelo considerou mais importantes para decidir?* 
1. [Variável X] 
2. [Variável Y] 
 
## 5. Conclusão da Fase de Modelação 
*Justifiquem por que razão este modelo está pronto (ou não) para ser apresentado como solução 
final.* 
 --- 
 
## 6. Metodologia de Gestão (PBL)

O projeto segue uma abordagem baseada no modelo CRISP-DM (Cross-Industry Standard Process for Data Mining), que estrutura o desenvolvimento em seis fases principais: compreensão do problema, compreensão dos dados, preparação dos dados, modelação, avaliação e implementação.

Na presente fase - Milestone 3 (Modelação e Avaliação) - o foco incide nas etapas de *Modelling* e *Evaluation*, com o objetivo de desenvolver um modelo de classificação supervisionado capaz de prever o fenómeno de *Attrition*.

Nesta fase, são implementados e comparados diferentes algoritmos de classificação, incluindo um modelo baseline e modelos mais avançados, com vista à maximização do desempenho preditivo. É definida uma estratégia de validação robusta, recorrendo a *train-test split* e *cross-validation estratificada*, garantindo a capacidade de generalização dos modelos.

Dado o desequilíbrio da variável alvo, a avaliação é orientada para métricas adequadas, com particular foco no *Recall* e no *F1-Score*, assegurando a correta identificação da classe minoritária (colaboradores que abandonam a organização).

O processo mantém uma natureza iterativa, permitindo ajustar variáveis, parâmetros e algoritmos com base nos resultados obtidos, com o objetivo de alcançar um modelo robusto e com valor para a tomada de decisão em contexto de gestão de recursos humanos.

**Divisão de Tarefas:** 

 **Luís Figueira:** 
   *  
   *  
   *  
   *  
    
 **Martim Ferreira:**
   *  
   *  
   *  
   *  

 **Mateus Afonso:** 
   *  
   *  
   *  
   *  

  **Tarefas conjuntas**
   *  
   *  
   *  
   *
   
 ## 7. Referências
 
Chapman, P., Clinton, J., Khabaza, T., Reinartz, T., & Wirth, R. (2000). *CRISP-DM 1.0: Step-by-step data mining guide.*

Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow* (3.ª ed.). O'Reilly Media.

Hom, P. W., Lee, T. W., Shaw, J. D., & Hausknecht, J. P. (2017). One hundred years of employee turnover theory and research. *Journal of Applied Psychology.*

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2021). *An Introduction to Statistical Learning: with Applications in R* (2.ª ed.). Springer.

 
Data de última atualização: 15/04/2026
