# Milestone 3: Modelação e Avaliação 
 
**Divisão do dataset:** Para selecionar a divisão de dados mais adequada, foram testados seis rácios distintos (65/35, 70/30, 75/25, 80/20, 85/15 e 90/10), com e sem estratificação pela variável alvo `Attrition_bin`. Os resultados comparativos de todos os *splits* encontram-se disponíveis no repositório do projeto (pasta `/resultados/splits/Objetivo2`). A divisão que produziu os melhores resultados globais foi a de 65% para treino e 35% para teste, com estratificação (random_state=42), garantindo a mesma proporção de colaboradores com atrito (~16%) em ambos os conjuntos. Esta abordagem é recomendada na literatura como boa prática em problemas de classificação com desequilíbrio de classes, assegurando que ambos os conjuntos representam fielmente a distribuição real dos dados (Géron, 2022; James et al., 2021). As variáveis categóricas originais foram removidas do conjunto de *features*, utilizando as respetivas versões já codificadas no *dataset* processado. Apenas variáveis numéricas foram consideradas, com normalização via `StandardScaler` aplicada ao modelo *baseline* (Regressão Logística), KNN e SVM, em linha com o CRISP-DM (Chapman et al., 2000).

**Métrica de Sucesso:** A métrica principal escolhida foi o F1-Score, por ser a mais adequada para *datasets* desequilibrados como o nosso (~84% Não Saiu vs ~16% Saiu). O F1-Score combina Precision e Recall numa única métrica, penalizando tanto os falsos positivos como os falsos negativos - característica particularmente relevante no contexto de atrito organizacional, onde a falha em identificar colaboradores em risco tem custos práticos para as organizações (Hom et al., 2017; James et al., 2021). Como métrica complementar foi utilizado o AUC-ROC, que mede a capacidade discriminativa do modelo independentemente do *threshold* de decisão (Géron, 2022; James et al., 2021).

Para além das métricas quantitativas, a interpretabilidade e a qualidade das probabilidades previstas foram definidas desde o início como critérios formais de seleção do modelo final. No contexto deste objetivo, as probabilidades geradas pelo modelo são a base do índice de risco - pelo que um modelo com probabilidades bem calibradas é preferível a um modelo de caixa-negra, independentemente do seu F1-Score isolado (Hom et al., 2017). Em caso de desempenho equivalente entre finalistas, seria preferido o modelo com maior capacidade de explicação direta dos coeficientes e com probabilidades interpretáveis.
 
## 2. Experiências Realizadas 
### 2.1. Modelo Baseline 
*O ponto de partida simples.* 
* **Algoritmo:** (p/ex.: Regressão Logística) 
* **Resultado:** (p/ex.: Accuracy: 0.72) 
 
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

 
Data de última atualização: 15/04/2026
