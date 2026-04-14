# Milestone 3: Modelação e Avaliação 
 
## 1. Estratégia de Modelação 
*Descrevam como prepararam os dados para os algoritmos.* 
* **Divisão do dataset:** (p/ex.: "Utilizámos uma divisão de 70% para treino e 30% para teste 
com semente aleatória (random_state) fixa.") 
* **Métrica de Sucesso:** (p/ex.: "A métrica principal escolhida foi o F1-Score, pois o nosso 
dataset é desequilibrado e queremos evitar falsos negativos.") 
 
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

Na presente fase - Milestone 3 (Modelação e Avaliação) - o foco incide na aplicação de técnicas de aprendizagem não supervisionada, com o objetivo de identificar padrões e perfis distintos de colaboradores através de métodos de *clustering*.

São implementados algoritmos de agrupamento, nomeadamente o K-Means, sendo exploradas diferentes configurações para determinar o número ótimo de clusters. Para tal, são utilizadas métricas internas de avaliação, como o *Silhouette Score*, o método do cotovelo (*Elbow Method*) e indicadores adicionais como o índice de Davies-Bouldin e Calinski-Harabasz.

A análise inclui ainda a interpretação dos clusters obtidos, através da caracterização estatística dos grupos e da identificação de padrões relevantes no contexto organizacional.

À semelhança das restantes abordagens, o processo é iterativo, permitindo ajustar as variáveis utilizadas, aplicar técnicas de redução de dimensionalidade e testar diferentes algoritmos, com o objetivo de melhorar a qualidade dos agrupamentos e extrair conhecimento útil para a segmentação de colaboradores.

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
