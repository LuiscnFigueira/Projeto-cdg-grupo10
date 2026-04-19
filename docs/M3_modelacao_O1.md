# Milestone 3: Modelação e Avaliação 
 
## 1. Estratégia de Modelação

**Divisão do dataset:** Utilizámos uma divisão de 80% para treino e 20% para teste, estratificada pela variável alvo `Attrition_bin` (random_state=42), garantindo a mesma proporção de colaboradores com atrito (~16%) em ambos os conjuntos. Esta abordagem de divisão estratificada é recomendada na literatura como boa prática em problemas de classificação com desequilíbrio de classes, assegurando que ambos os conjuntos representam fielmente a distribuição real dos dados (Géron, 2022; James et al., 2021). As variáveis categóricas originais (`Attrition`, `OverTime`, `Gender`, `BusinessTravel`, `Department`, `EducationField`, `JobRole`, `MaritalStatus`) foram removidas do conjunto de features, utilizando as respetivas versões já codificadas no dataset processado. Apenas variáveis numéricas foram consideradas, em linha com o processo de preparação de dados definido no âmbito do CRISP-DM (Chapman et al., 2000).

**Métrica de Sucesso:** A métrica principal escolhida foi o F1-Score, por ser a mais adequada para datasets desequilibrados como o nosso (~84% Não Saiu vs ~16% Saiu). O F1-Score combina *Precision* e *Recall* numa única métrica, penalizando tanto os falsos positivos como os falsos negativos — característica particularmente relevante no contexto de atrito organizacional, onde tanto a falha em identificar colaboradores em risco como a geração de alertas desnecessários têm custos práticos para as organizações (Hom et al., 2017; James et al., 2021). Como métrica complementar foi utilizado o AUC-ROC, que mede a capacidade discriminativa do modelo independentemente do *threshold* de decisão, permitindo uma avaliação mais abrangente da qualidade de separação entre classes (Géron, 2022; James et al., 2021).

 
## 2. Experiências Realizadas 
### 2.1. Modelo Baseline 

**Algoritmo:** Árvore de Decisão (`DecisionTreeClassifier`) com todos os parâmetros *default*, sem normalização nem balanceamento dos dados.

**Resultados:** F1 Treino: 1.0000 | F1 Teste: 0.1793 | Precision: 0.2097 | Recall: 0.1566 | AUC Teste: 0.5216

O modelo baseline evidencia *overfitting* severo: a árvore de decisão sem restrições de profundidade memorizou completamente os dados de treino (F1 = 1.0000), não generalizando para dados não vistos (F1 = 0.1793). O gap de 0.8207 entre o F1 de treino e de teste confirma este comportamento - esperado e documentado na literatura, dado que árvores de decisão sem regularização tendem a crescer até memorizar o conjunto de treino, resultando em fraca capacidade de generalização (James et al., 2021). A Precision (0.2097) e o Recall (0.1566) igualmente baixos indicam que o modelo falha tanto na identificação correta dos colaboradores em risco como na cobertura dos casos reais de atrito. O valor de AUC = 0.5216 confirma uma capacidade discriminativa próxima do aleatório (0.50), reforçando a necessidade de aplicar técnicas de controlo de complexidade e balanceamento de classes nas experiências subsequentes (Géron, 2022).

 
### 2.2. Modelos Candidatos 
*Listagem dos algoritmos testados e a justificação da escolha.* 
 
| Algoritmo | Parâmetros Base | Métrica (Treino) | Métrica (Teste) | Notas | 
| :--- | :--- | :--- | :--- | :--- | 
| Random Forest | n_estimators=100 | 0.95 | 0.82 | Sinais de overfitting | 
| XGBoost | default | 0.88 | 0.85 | Melhor generalização | 
| SVM | kernel='rbf' | 0.80 | 0.79 | Lento no treino | 
 
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
