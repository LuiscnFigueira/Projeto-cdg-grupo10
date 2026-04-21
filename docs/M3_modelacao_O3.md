# Milestone 3: Modelação e Avaliação 
 
## 1. Estratégia de Modelação 

### 1.1 Divisão do Dataset (Treino/Teste)

Para selecionar a divisão de dados mais adequada, foram testados seis rácios distintos (65/35, 70/30, 75/25, 80/20, 85/15 e 90/10), com semente aleatória fixa (`random_state=42`) para garantir a reprodutibilidade dos resultados (Chapman et al., 2000). Os resultados comparativos de todas as divisões encontram-se disponíveis no repositório do projeto (pasta `/resultados/splits/Objetivo3`). A divisão que produziu os melhores resultados globais foi a de **80% para treino e 20% para teste???????**, garantindo um conjunto de treino suficientemente representativo para estabilidade dos *clusters* e um conjunto de teste com dimensão adequada à avaliação da generalização dos modelos.

Esta proporção assegura que o conjunto de treino dispõe de informação suficiente para a identificação de padrões estáveis, enquanto o conjunto de teste permite avaliar se os agrupamentos identificados se mantêm quando aplicados a dados não observados durante o treino - critério especialmente relevante em aprendizagem não supervisionada, onde não existe uma métrica externa de referência (Jain, 2010; Géron, 2022).


### 1.2 Normalização dos Dados

Antes da aplicação dos algoritmos de *clustering*, foi realizada a normalização das variáveis numéricas utilizando o método `StandardScaler`, que transforma cada variável para média zero e desvio padrão unitário (z-score). Esta etapa é crítica porque algoritmos como *K-Means*, DBSCAN e GMM baseiam-se em distâncias euclidianas: sem normalização, variáveis com maior escala - como o salário mensal, cujos valores superam a ordem de grandeza de variáveis binárias (0/1) - dominariam o cálculo das distâncias, distorcendo os resultados e comprometendo a qualidade dos *clusters* (Géron, 2022; James et al., 2021).

Para garantir rigor metodológico e evitar *data leakage*, o processo foi conduzido em conformidade com as boas práticas estabelecidas na literatura (Chapman et al., 2000; Géron, 2022):

•	O `StandardScaler` foi ajustado exclusivamente no conjunto de treino (.fit_transform), aprendendo os parâmetros estatísticos (μ, σ) a partir apenas dessas observações

•	Os parâmetros aprendidos foram posteriormente aplicados ao conjunto de teste (.transform), garantindo que nenhuma informação do teste influenciou a normalização

Após a normalização, verificou-se que a média das variáveis ≈ 0 e o desvio padrão ≈ 1, confirmando que todas as variáveis contribuem de forma equilibrada para a formação dos *clusters*. Este procedimento é particularmente relevante neste dataset, dado que integra simultaneamente variáveis numéricas contínuas (como por exemplo: salário e anos de experiência) e variáveis binárias resultantes de codificação *one-hot* de variáveis categóricas, cujas escalas nativas seriam incompatíveis sem normalização.

### 1.3 Métricas de Sucesso

A avaliação dos modelos de *clustering* foi realizada com base em métricas internas, uma vez que não existem rótulos reais que permitam uma validação supervisionada. Foram utilizadas três métricas complementares, em conformidade com as boas práticas da literatura especializada (Jain, 2010).

A métrica principal definida foi o *Silhouette Score*, que mede simultaneamente a coesão interna dos *clusters* - quão próximas estão as observações dentro do mesmo grupo - e a separação entre *clusters* distintos. Os seus valores variam entre -1 e 1, sendo valores mais elevados indicativos de melhor qualidade de agrupamento. Foi definido como objetivo atingir um valor superior a 0,50, correspondente a *clusters* bem definidos e separados.

Como métricas complementares, foram consideradas:

•	*Davies-Bouldin Index* (Davies & Bouldin, 1979): avalia a sobreposição média entre *clusters*, computando o rácio entre a dispersão intra-cluster e a distância inter-cluster. Valores mais baixos indicam melhor separação

•	*Calinski-Harabasz Index* (Caliński & Harabasz, 1974): mede a razão entre a dispersão inter-cluster e a dispersão intra-cluster. Valores mais elevados indicam clusters mais compactos e bem separados

Para além das métricas quantitativas, foi analisada a estabilidade dos modelos através da consistência dos resultados entre treino e teste - um modelo robusto deverá apresentar métricas equiparáveis em ambos os conjuntos, indicando capacidade de generalização. Desta forma, a seleção do modelo final baseou-se numa abordagem multi-critério, combinando desempenho quantitativo, robustez estatística e interpretabilidade dos resultados no contexto do problema de negócio: a identificação de perfis de colaboradores associados ao fenómeno de `Attrition` (Jain, 2010; James et al., 2021).

 
## 2. Experiências Realizadas 
### 2.1. Modelo Baseline 

**Algoritmo:** K-Means* com `n_clusters=4` e parâmetros totalmente *default* - sem `random_state` nem `n_init`.

**Resultados:** *Silhouette* Treino: 0.0745 | *Silhouette* Teste: 0.0696 | *Davies-Bouldin* Treino: 3.3150 | *Davies-Bouldin Teste*: 3.2991 | *Calinski-Harabasz* Treino: 73.49 | *Calinski-Harabasz* Teste: 18.76

O modelo *baseline* evidencia uma segmentação muito fraca. O *Silhouette Score* de 0.0696 no teste está muito próximo de zero, o que indica que as observações não estão bem separadas nos *clusters* atribuídos - valor que Rousseeuw (1987) associa a estrutura praticamente inexistente. O *Davies-Bouldin* de 3.32 é também elevado, sinalizando *clusters* sobrepostos e pouco compactos (Davies & Bouldin, 1979). A distribuição dos *clusters* no treino é assimétrica (Cluster 1 com 429 observações, Cluster 0 com apenas 161), o que revela que o *K-Means default* não consegue encontrar agrupamentos equilibrados nem coerentes. Este comportamento é consistente com as limitações conhecidas do algoritmo: o *K-Means* assume *clusters* esféricos e de dimensões semelhantes, e degrada-se em *datasets* de alta dimensionalidade com distribuições heterogéneas (Jäin, 2010; Géron, 2022). Os resultados treino e teste são consistentes (diferença de 0.0049), o que confirma ausência de *overfitting*, mas também ausência de estrutura real nos dados quando abordados com este método sem otimização.

 
### 2.2. Modelos Candidatos 

Foram testados seis modelos candidatos, cobrindo diferentes paradigmas de *clustering*: particionamento, densidade, modelo probabilístico, hierarquia e variante escalável. A seleção foi orientada pela recomendação CRISP-DM de testar múltiplos algoritmos antes de selecionar o modelo final, sem assumir à partida qual o mais adequado para o problema (Chapman et al., 2000). Em problemas de *clustering* sem supervisão, a diversidade de abordagens é particularmente importante dado que diferentes algoritmos capturam estruturas distintas nos dados (James et al., 2021; Géron, 2022).
 | Modelo                        | Parâmetros Base                         | Silhouette (Treino) | Silhouette (Teste) | Davies-Bouldin (Treino) | Davies-Bouldin (Teste) | Calinski-Harabasz (Treino) | Calinski-Harabasz (Teste) |
|-------------------------------|-----------------------------------------|---------------------|---------------------|--------------------------|-------------------------|-----------------------------|----------------------------|
| Candidato 2 - DBSCAN         | eps=8.0, min_samples=5                 | 0.1708              | 0.1709              | 1.5778                   | 1.4790                    | 34.39                       | 8.5054                       |
| Candidato 3 - GMM            | n=4, covariance_type='full'            | 0.0867              | 0.0874              | 2.6863                   | 3.3225                    | 74.08                       | 17.9619	                     |
| Candidato 4 - Agglomerative  | k=4, linkage='ward'                    | 0.0785              | 0.0809              | 2.7545                   | 2.6197	                    | 72.43                       | 18.6351                     |
| Candidato 1 — K-Means        | k=4 (Silhouette Search)                | 0.0818              | 0.0768              | 2.6212                   | 3.2991                    | 73.44                       | 18.7567	                       |
| Candidato 6 - MiniBatch K-Means | k=4, batch_size=default            | 0.0414              | 0.0362              | 3.6706                   | 3.6929                    | 66.46                       | 17.2167                       |
| Candidato 5 - OPTICS         | min_samples=3, xi=0.05                 | N/A                 | N/A                 | N/A                      | N/A                    | N/A                         | N/A                       |

**Particionamento (Candidatos 1 e 6):** O *K-Means* Otimizado e o *MiniBatch K-Means* foram testados como variantes do baseline. A pesquisa do k ótimo por *Silhouette* no conjunto de treino confirmou k=4 como o valor mais adequado - o mesmo do baseline - o que sugere que a limitação não está no núméro de *clusters* mas na capacidade do *K-Means* em capturar a estrutura real dos dados. Com k=4, o *K-Means* Otimizado apresentou *Silhouette* de 0.0818 (treino) e 0.0768 (teste), uma melhoria marginal face ao *baseline* (0.0745/0.0696). O *MiniBatch K-Means* foi o modelo com pior desempenho geral (*Silhouette* teste de 0.0362), resultante da estocasticidade dos *batches* que, sem `random_state`, introduz instabilidade nas atribuições. Este comportamento é consistente com a literatura: o *K-Means* degrada-se em *datasets* de alta dimensionalidade com distribuições não esféricas (Jäin, 2010; Géron, 2022).

**Modelo Probabilístico (Candidato 3 — GMM):** O *Gaussian Mixture Model* com quatro componentes e covariance_type='full' foi selecionado após pesquisa do núméro ótimo de componentes por *Silhouette* no treino. Atingiu *Silhouette* de 0.0867 (treino) e 0.0874 (teste), ligeiramente superior ao *K-Means*. A vantagem do *GMM* face ao *K-Means* reside na capacidade de modelar *clusters* de formas elípticas e de diferentes dimensões através da matriz de covariância completa; no entanto, o ganho foi inferior ao esperado, sugerindo que a distribuição dos dados não segue uma mistura de Gaussianas bem separada. O *Davies-Bouldin* de 2.69 evidencia ainda sobreposioção considerável entre grupos (James et al., 2021).

**Hierárquico (Candidato 4 — Agglomerative Clustering):** O *Agglomerative Clustering* com linkage='ward' e k=4 produziu resultados muito próximos do *K-Means* Otimizado: *Silhouette* 0.0785/0.0809 e DB de 2.75. O dendrograma confirmou que o corte a quatro *clusters* não corresponde a uma separação natural bem definida. A vantagem da abordagem hierárquica é não requerer k fixo à partida e permitir análise do dendrograma para validação, mas o desempenho métrico ficou aquem do DBSCAN (James et al., 2021; Géron, 2022).

**Baseado em Densidade (Candidatos 2 e 5 — DBSCAN e OPTICS):** Os algoritmos baseados em densidade apresentaram comportamentos opostos. O OPTICS com min_samples=3, xi=0.05 e min_cluster_size=0.05 identificou apenas um *cluster* com 1172 observações e 4 pontos de ruído (0.3%), tornando as métricas incalculáveis (núméro de clusters insuficiente). Este resultado sugere que os parâmetros não foram adequados para a densidade do dataset e que o OPTICS, por ser mais sensível ao limiar de *cluster* mínimo, requer afinação mais cuidadosa do que o DBSCAN (Ankerst et al., 1999).

O DBSCAN com eps=8.0 e min_samples=5 foi o modelo com melhor desempenho da fase de candidatos: *Silhouette* de 0.1708 (treino) e 0.1709 (teste), *Davies-Bouldin* de 1.5778 — substancialmente inferior a todos os restantes — e apenas 1.1% de ruído (13 pontos). O algoritmo identificou 3 *clusters* naturais a partir da densidade local dos dados, sem necessidade de definir k à partida. A consistência entre treino e teste (diferença de 0.0001) demonstra estabilidade da estrutura aprendida. A aplicação ao conjunto de teste foi realizada via KNN (k=5), *proxy* supervisionado necessário dado que o DBSCAN não possui método .predict() nativo (Géron, 2022; Schubert et al., 2017). O DBSCAN foi, por isso, selecionado como modelo a otimizar.

Em síntese, o intervalo de *Silhouette* nos candidatos válidos situa-se entre 0.0362 (MiniBatch K-Means) e 0.1709 (DBSCAN), todos abaixo do limiar de 0.50 definido como meta na Secção 1. Este resultado reflete a dificuldade inerente à segmentação de dados de Recursos Humanos com elevada dimensionalidade (53 variáveis), onde as distâncias euclidianas tendem a tornar-se uniformes - fenómeno conhecido como *curse of dimensionality* (Bellman, 1957; James et al., 2021). A fase de otimização endereça precisamente este problema através de redução de dimensionalidade por PCA, que permitirá avaliar se a qualidade de *clustering* melhora substancialmente (Jolliffe & Cadima, 2016).

 
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
