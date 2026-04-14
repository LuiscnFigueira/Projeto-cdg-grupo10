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

 
Data de última atualização: 15/04/2026
