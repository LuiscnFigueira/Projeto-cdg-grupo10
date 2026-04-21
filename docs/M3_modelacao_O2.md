# Milestone 3: Modelação e Avaliação 

## 1. Estratégia de Modelação

### 1.1 Divisão do Dataset
O dataset foi dividido em 80% para treino (1176 observações)
e 20% para teste (294 observações), com estratificação pela variável alvo
`Attrition_bin` (random_state=42), garantindo a mesma proporção de colaboradores
com atrito (~16%) em ambos os conjuntos e evitando fugas de informação
(*data leakage*). Esta abordagem é recomendada na literatura como boa prática em
problemas de classificação com desequilíbrio de classes, assegurando que ambos os
conjuntos representam fielmente a distribuição real dos dados (Géron, 2022;
James et al., 2021).

### 1.2 Normalização dos Dados
A normalização via `StandardScaler` foi aplicada seletivamente apenas aos modelos
sensíveis à escala das variáveis: Regressão Logística (Baseline), KNN e SVM. Os
restantes modelos candidatos (Naive Bayes, LDA, Extra Trees, AdaBoost, LightGBM,
XGBoost e Random Forest) não requerem normalização pela sua natureza algorítmica,
nomeadamente por serem baseados em árvores de decisão ou por tratarem as variáveis
de forma independente. As variáveis categóricas originais foram removidas do conjunto
de *features*, utilizando as respetivas versões já codificadas no *dataset*
processado. Apenas variáveis numéricas foram consideradas, em linha com o
CRISP-DM (Chapman et al., 2000).

### 1.3 Métrica de Sucesso
A métrica principal escolhida foi o **F1-Score**, por ser a mais adequada para
*datasets* desequilibrados como o nosso (~84% Não Saiu vs ~16% Saiu). O F1-Score
combina Precision e Recall numa única métrica, penalizando tanto os falsos positivos
como os falsos negativos — característica particularmente relevante no contexto de
atrito organizacional, onde a falha em identificar colaboradores em risco tem custos
práticos para as organizações (Hom et al., 2017; James et al., 2021). O **Recall**
assume especial relevância: em RH, o custo de não identificar um colaborador em risco
(Falso Negativo) é tipicamente superior ao custo de uma intervenção preventiva
desnecessária (Falso Positivo). Como métrica complementar foi utilizado o **AUC-ROC**,
que mede a capacidade discriminativa do modelo independentemente do *threshold* de
decisão (Géron, 2022; James et al., 2021).

Para além das métricas quantitativas, a **interpretabilidade** e a **qualidade das
probabilidades previstas** foram definidas desde o início como critérios formais de
seleção do modelo final. No contexto deste objetivo, as probabilidades geradas pelo
modelo são a base do índice de risco — pelo que um modelo com probabilidades bem
calibradas é preferível a um modelo de caixa-negra, independentemente do seu
F1-Score isolado (Hom et al., 2017). Em caso de desempenho equivalente entre
finalistas, seria preferido o modelo com maior capacidade de explicação direta dos
coeficientes e com probabilidades interpretáveis.
 
## 2. Experiências Realizadas

### 2.1. Modelo Baseline — Regressão Logística

O modelo de referência escolhido foi a **Regressão Logística** com parâmetros por
omissão (*default*), por ser um modelo de baixa complexidade, interpretável e
adequado a problemas de classificação binária (Géron, 2022). Este modelo serve como
patamar mínimo de desempenho para comparação com todos os modelos candidatos.

| Métrica | Treino | Teste |
|:---|---:|---:|
| Accuracy | 0.9065 | 0.8639 |
| Precision | 0.8175 | 0.6296 |
| Recall | 0.5421 | 0.3617 |
| F1-Score | 0.6519 | 0.4595 |
| AUC-ROC | 0.8819 | 0.8170 |

O modelo baseline apresentou uma diferença de F1 de +0.19 entre treino e teste,
sinalizando overfitting moderado. O Recall no teste (36.2%) revelou-se o principal
ponto fraco — mais de metade dos colaboradores que saíram não foram identificados
pelo modelo com o threshold padrão de 0.50.

### 2.2. Modelos Candidatos

Foram testados 9 algoritmos candidatos de maior complexidade, com o objetivo de
superar o desempenho do baseline. A tabela seguinte apresenta os resultados
comparativos de todos os modelos, ordenados por F1-Score no teste:

| Modelo | F1 Treino | F1 Teste | AUC Teste | Overfitting (ΔF1) |
|:---|---:|---:|---:|:---:|
| Baseline — Regressão Logística | 0.6519 | **0.4595** | **0.8170** | +0.19 |
| 1. Naive Bayes | 0.4752 | 0.4354 | 0.7269 | +0.04  |
| 2. LDA | 0.6454 | 0.4167 | 0.8099 | +0.23 |
| 6. AdaBoost | 0.6133 | 0.3889 | 0.8022 | +0.22 |
| 7. LightGBM | 1.0000 | 0.3548 | 0.7820 | +0.65 |
| 8. XGBoost | 1.0000 | 0.3333 | 0.7617 | +0.67 |
| 5. SVM | 0.6851 | 0.2712 | 0.8086 | +0.41 |
| 4. Extra Trees | 1.0000 | 0.2143 | 0.8099 | +0.79 |
| 3. KNN | 0.4223 | 0.1724 | 0.6464 | +0.25 |
| 9. Random Forest | 1.0000 | 0.1695 | 0.7956 | +0.83 |

**Análise crítica dos resultados:**

- **Modelos com overfitting severo (ΔF1 > 0.40):** LightGBM, XGBoost, Extra Trees e
Random Forest atingiram F1 = 1.0 no treino mas colapsaram no teste, evidenciando
memorização dos dados de treino sem capacidade de generalização. Estes modelos foram
descartados para efeitos de otimização.

- **Naive Bayes:** Foi o único modelo sem sinais relevantes de overfitting (ΔF1 =
+0.04), mas apresentou o AUC-ROC mais baixo (0.727) e probabilidades pouco calibradas
para o contexto de construção de um índice de risco, tornando-o menos adequado ao
objetivo do trabalho.

- **Modelo vencedor — Regressão Logística (Baseline):** Apesar de ser o modelo mais
simples, apresentou o **melhor F1-Score no teste** (0.4595) entre os modelos com
overfitting controlado, o **AUC-ROC mais elevado** (0.8170) e **probabilidades bem
calibradas** — critério essencial para a construção de um índice de risco fiável e
interpretável (Hom et al., 2017). A sua seleção para otimização foi, portanto,
sustentada tanto por critérios de desempenho como de adequação ao objetivo de negócio.
 
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

Na presente fase - Milestone 3 (Modelação e Avaliação) - o foco incide na utilização de modelos supervisionados para estimar a probabilidade de ocorrência do fenómeno de *Attrition*, permitindo a construção de um índice de risco de abandono.

A partir das probabilidades previstas pelos modelos de classificação desenvolvidos, é criado um sistema de segmentação de risco, categorizando os colaboradores em níveis de risco, com base em limiares definidos.

A validação do modelo baseia-se em métricas adequadas ao problema de classificação, assegurando que as probabilidades estimadas são fiáveis e discriminativas. Adicionalmente, é considerada a capacidade do modelo em suportar decisões de negócio, nomeadamente na identificação preventiva de colaboradores em risco de saída.

O processo mantém uma abordagem iterativa, permitindo ajustar os limiares de classificação e refinar o modelo com base nos resultados obtidos, garantindo que o índice de risco gerado é interpretável, consistente e útil para a gestão estratégica de recursos humanos.

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
 
Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C., & Wirth, R. (2000). *CRISP-DM 1.0: Step-by-step data mining guide*. SPSS Inc.

Géron, A. (2022). *Hands-on machine learning with Scikit-Learn, Keras, and TensorFlow* (3rd ed.). O'Reilly Media.

Hom, P. W., Lee, T. W., Shaw, J. D., & Hausknecht, J. P. (2017). One hundred years of employee turnover theory and research. *Journal of Applied Psychology, 102*(3), 530–545.

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2021). *An introduction to statistical learning with applications in R* (2nd ed.). Springer.

 
Data de última atualização: 20/04/2026
