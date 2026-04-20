# Milestone 3: Modelação e Avaliação 
 
## 1. Estratégia de Modelação 

### 1.1 Divisão do Dataset (Treino/Teste)

Para selecionar a divisão de dados mais adequada, foram testados seis rácios distintos (65/35, 70/30, 75/25, 80/20, 85/15 e 90/10), com semente aleatória fixa (random_state=42) para garantir a reprodutibilidade dos resultados (Chapman et al., 2000). Os resultados comparativos de todas as divisões encontram-se disponíveis no repositório do projeto (pasta `/resultados/splits/Objetivo3`). A divisão que produziu os melhores resultados globais foi a de 80% para treino e 20% para teste, garantindo um conjunto de treino suficientemente representativo para estabilidade dos clusters e um conjunto de teste com dimensão adequada à avaliação da generalização dos modelos.

Esta proporção assegura que o conjunto de treino dispõe de informação suficiente para a identificação de padrões estáveis, enquanto o conjunto de teste permite avaliar se os agrupamentos identificados se mantêm quando aplicados a dados não observados durante o treino - critério especialmente relevante em aprendizagem não supervisionada, onde não existe uma métrica externa de referência (Jain, 2010; Géron, 2022).


### 1.2 Normalização dos Dados

Antes da aplicação dos algoritmos de clustering, foi realizada a normalização das variáveis numéricas utilizando o método `StandardScaler`, que transforma cada variável para média zero e desvio padrão unitário (z-score). Esta etapa é crítica porque algoritmos como K-Means, DBSCAN e GMM baseiam-se em distâncias euclidianas: sem normalização, variáveis com maior escala - como o salário mensal, cujos valores superam a ordem de grandeza de variáveis binárias (0/1) - dominariam o cálculo das distâncias, distorcendo os resultados e comprometendo a qualidade dos clusters (Géron, 2022; James et al., 2021).

Para garantir rigor metodológico e evitar data leakage, o processo foi conduzido em conformidade com as boas práticas estabelecidas na literatura (Chapman et al., 2000; Géron, 2022):

•	O `StandardScaler` foi ajustado exclusivamente no conjunto de treino (.fit_transform), aprendendo os parâmetros estatísticos (μ, σ) a partir apenas dessas observações

•	Os parâmetros aprendidos foram posteriormente aplicados ao conjunto de teste (.transform), garantindo que nenhuma informação do teste influenciou a normalização

Após a normalização, verificou-se que a média das variáveis ≈ 0 e o desvio padrão ≈ 1, confirmando que todas as variáveis contribuem de forma equilibrada para a formação dos clusters. Este procedimento é particularmente relevante neste dataset, dado que integra simultaneamente variáveis numéricas contínuas (como por exemplo: salário e anos de experiência) e variáveis binárias resultantes de codificação one-hot de variáveis categóricas, cujas escalas nativas seriam incompatíveis sem normalização.

### 1.3 Métricas de Sucesso

A avaliação dos modelos de clustering foi realizada com base em métricas internas, uma vez que não existem rótulos reais que permitam uma validação supervisionada. Foram utilizadas três métricas complementares, em conformidade com as boas práticas da literatura especializada (Jain, 2010).

A métrica principal definida foi o Silhouette Score, que mede simultaneamente a coesão interna dos clusters - quão próximas estão as observações dentro do mesmo grupo - e a separação entre clusters distintos. Os seus valores variam entre -1 e 1, sendo valores mais elevados indicativos de melhor qualidade de agrupamento. Foi definido como objetivo atingir um valor superior a 0,50, correspondente a clusters bem definidos e separados.

Como métricas complementares, foram consideradas:

•	Davies-Bouldin Index (Davies & Bouldin, 1979): avalia a sobreposição média entre clusters, computando o rácio entre a dispersão intra-cluster e a distância inter-cluster. Valores mais baixos indicam melhor separação

•	Calinski-Harabasz Index (Caliński & Harabasz, 1974): mede a razão entre a dispersão inter-cluster e a dispersão intra-cluster. Valores mais elevados indicam clusters mais compactos e bem separados

Para além das métricas quantitativas, foi analisada a estabilidade dos modelos através da consistência dos resultados entre treino e teste - um modelo robusto deverá apresentar métricas equiparáveis em ambos os conjuntos, indicando capacidade de generalização. Desta forma, a seleção do modelo final baseou-se numa abordagem multi-critério, combinando desempenho quantitativo, robustez estatística e interpretabilidade dos resultados no contexto do problema de negócio: a identificação de perfis de colaboradores associados ao fenómeno de `Attrition` (Jain, 2010; James et al., 2021).

 
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

Caliński, T., & Harabasz, J. (1974). A dendrite method for cluster analysis. Communications in Statistics, 3(1), 1–27.

Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C., & Wirth, R. (2000). CRISP-DM 1.0: Step-by-step data mining guide. SPSS Inc.

Davies, D. L., & Bouldin, D. W. (1979). A cluster separation measure. IEEE Transactions on Pattern Analysis and Machine Intelligence, 1(2), 224–227.

Géron, A. (2022). Hands-on machine learning with Scikit-Learn, Keras, and TensorFlow (3rd ed.). O’Reilly Media.

Jain, A. K. (2010). Data clustering: 50 years beyond K-means. Pattern Recognition Letters, 31(8), 651–666.

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2021). An introduction to statistical learning with applications in R (2nd ed.). Springer.
 
Data de última atualização: 21/04/2026
