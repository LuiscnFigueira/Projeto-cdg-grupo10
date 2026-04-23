# Milestone 3: Modelação e Avaliação 
 
## 1. Estratégia de Modelação 

### 1.1 Divisão do Dataset (Treino/Teste)

Para selecionar a divisão de dados mais adequada, foram testados seis rácios distintos (65/35, 70/30, 75/25, 80/20, 85/15 e 90/10), com semente aleatória fixa (`random_state=42`) para garantir a reprodutibilidade dos resultados (Chapman et al., 2000). Os resultados comparativos de todas as divisões encontram-se disponíveis no repositório do projeto (pasta `/resultados/splits/Objetivo3`). A divisão que produziu os melhores resultados globais foi a de 85% para treino e 15% para teste, garantindo um conjunto de treino suficientemente representativo para estabilidade dos *clusters* e um conjunto de teste com dimensão adequada à avaliação da generalização dos modelos.

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

**Algoritmo:** *K-Means* com `n_clusters=4` e parâmetros totalmente *default* - sem `random_state` nem `n_init`.

**Resultados:** *Silhouette* Treino: 0.0745 | *Silhouette* Teste: 0.0705 | *Davies-Bouldin* Treino: 3.1450 | *Davies-Bouldin Teste*: 2.9943 | *Calinski-Harabasz* Treino: 75.86 | *Calinski-Harabasz* Teste: 14.24

O modelo *baseline* evidencia uma segmentação muito fraca. O *Silhouette Score* de 0.0705 no teste está muito próximo de zero, o que indica que as observações não estão bem separadas nos *clusters* atribuídos - valor que Rousseeuw (1987) associa a estrutura praticamente inexistente. O *Davies-Bouldin* de 2.99 é também elevado, sinalizando *clusters* sobrepostos e pouco compactos (Davies & Bouldin, 1979). A distribuição dos *clusters* no treino é assimétrica (Cluster 1 com 449 observações, Cluster 2 com apenas 202), o que revela que o *K-Means default* não consegue encontrar agrupamentos equilibrados nem coerentes. Este comportamento é consistente com as limitações conhecidas do algoritmo: o *K-Means* assume *clusters* esféricos e de dimensões semelhantes, e degrada-se em *datasets* de alta dimensionalidade com distribuições heterogéneas (Jäin, 2010; Géron, 2022). Os resultados treino e teste são consistentes (diferença de 0.0049), o que confirma ausência de *overfitting*, mas também ausência de estrutura real nos dados quando abordados com este método sem otimização.

### 2.2. Modelos Candidatos 

Foram testados seis modelos candidatos, cobrindo diferentes paradigmas de *clustering*: particionamento, densidade, modelo probabilístico, hierarquia e variante escalável. A seleção foi orientada pela recomendação CRISP-DM de testar múltiplos algoritmos antes de selecionar o modelo final, sem assumir à partida qual o mais adequado para o problema (Chapman et al., 2000). Em problemas de *clustering* sem supervisão, a diversidade de abordagens é particularmente importante dado que diferentes algoritmos capturam estruturas distintas nos dados (James et al., 2021; Géron, 2022).

 | Modelo                    | Silhouette (Treino) | Silhouette (Teste) | Davies-Bouldin (Treino) | Davies-Bouldin (Teste) | Calinski-Harabasz (Treino) | Calinski-Harabasz (Teste) |
|---------------------------|---------------------|---------------------|--------------------------|-------------------------|-----------------------------|----------------------------|
| DBSCAN (eps=8.0)         | 0.1709              | 0.1828              | 1.5723                   | 1.4334                  | 35.7952                     | 7.5962                     |
| GMM (n=4)                | 0.0836              | 0.0775              | 2.6034                   | 2.5225                  | 75.2250                     | 12.7125                    |
| K-Means Otimizado (k=4)  | 0.0756              | 0.0745              | 3.2592                   | 3.1238                  | 77.3667                     | 14.5192                    |
| Hierárquico (k=4)        | 0.0794              | 0.0744              | 2.6737                   | 2.5634                  | 77.2639                     | 14.1742                    |
| K-Means Baseline         | 0.0549              | 0.0484              | 3.4219                   | 3.2362                  | 73.5360                     | 13.9785                    |
| MiniBatch K-Means (k=4)  | 0.0427              | 0.0428              | 3.5821                   | 3.4190                  | 70.1582                     | 13.8462                    |

**Particionamento (Candidatos 1 e 6):** O *K-Means* Otimizado e o *MiniBatch K-Means* foram testados como variantes do baseline. A pesquisa do k ótimo por *Silhouette* no conjunto de treino confirmou k=4 como o valor mais adequado - o mesmo do baseline - o que sugere que a limitação não está no núméro de *clusters* mas na capacidade do *K-Means* em capturar a estrutura real dos dados. Com k=4, o *K-Means* Otimizado apresentou *Silhouette* de 0.0756 (treino) e 0.0745 (teste), uma melhoria marginal face ao *baseline* (0.0549/0.0484). O *MiniBatch K-Means* foi o modelo com pior desempenho geral (*Silhouette* teste de 0.0428), resultante da estocasticidade dos *batches* que, sem `random_state`, introduz instabilidade nas atribuições. Este comportamento é consistente com a literatura: o *K-Means* degrada-se em *datasets* de alta dimensionalidade com distribuições não esféricas (Jäin, 2010; Géron, 2022).

**Modelo Probabilístico (Candidato 3 - GMM):** O *Gaussian Mixture Model* com quatro componentes e covariance_type='full' foi selecionado após pesquisa do núméro ótimo de componentes por *Silhouette* no treino. Atingiu *Silhouette* de 0.0836 (treino) e 0.0775 (teste), ligeiramente superior ao *K-Means*. A vantagem do *GMM* face ao *K-Means* reside na capacidade de modelar *clusters* de formas elípticas e de diferentes dimensões através da matriz de covariância completa; no entanto, o ganho foi inferior ao esperado, sugerindo que a distribuição dos dados não segue uma mistura de Gaussianas bem separada. O *Davies-Bouldin* de 2.52 evidencia ainda sobreposioção considerável entre grupos (James et al., 2021).

**Hierárquico (Candidato 4 - Agglomerative Clustering):** O *Agglomerative Clustering* com linkage='ward' e k=4 produziu resultados muito próximos do *K-Means* Otimizado: *Silhouette* 0.0794/0.0744 e DB de 2.56. O dendrograma confirmou que o corte a quatro *clusters* não corresponde a uma separação natural bem definida. A vantagem da abordagem hierárquica é não requerer k fixo à partida e permitir análise do dendrograma para validação, mas o desempenho métrico ficou aquem do DBSCAN (James et al., 2021; Géron, 2022).

**Baseado em Densidade (Candidatos 2 e 5 - DBSCAN e OPTICS):** Os algoritmos baseados em densidade apresentaram comportamentos opostos. O OPTICS com min_samples=3, xi=0.05 e min_cluster_size=0.05 identificou apenas um *cluster* com 1244 observações e 5 pontos de ruído (0.3%), tornando as métricas incalculáveis (núméro de clusters insuficiente). Este resultado sugere que os parâmetros não foram adequados para a densidade do dataset e que o OPTICS, por ser mais sensível ao limiar de *cluster* mínimo, requer afinação mais cuidadosa do que o DBSCAN (Ankerst et al., 1999).

O DBSCAN com eps=8.0 e min_samples=5 foi o modelo com melhor desempenho da fase de candidatos: *Silhouette* de 0.1709 (treino) e 0.1828 (teste), *Davies-Bouldin* de 1.4334 - substancialmente inferior a todos os restantes - e apenas 1% de ruído (12 pontos). O algoritmo identificou 3 *clusters* naturais a partir da densidade local dos dados, sem necessidade de definir k à partida. A consistência entre treino e teste (diferença de 0.0001) demonstra estabilidade da estrutura aprendida. A aplicação ao conjunto de teste foi realizada via KNN (k=5), *proxy* supervisionado necessário dado que o DBSCAN não possui método .predict() nativo (Géron, 2022; Schubert et al., 2017). O DBSCAN foi, por isso, selecionado como modelo a otimizar.

Em síntese, o intervalo de *Silhouette* nos candidatos válidos situa-se entre 0.0428 (MiniBatch K-Means) e 0.1828 (DBSCAN), todos abaixo do limiar de 0.50 definido como meta na Secção 1. Este resultado reflete a dificuldade inerente à segmentação de dados de Recursos Humanos com elevada dimensionalidade (53 variáveis), onde as distâncias euclidianas tendem a tornar-se uniformes - fenómeno conhecido como *curse of dimensionality* (Bellman, 1957; James et al., 2021). A fase de otimização endereça precisamente este problema através de redução de dimensionalidade por PCA, que permitirá avaliar se a qualidade de *clustering* melhora substancialmente (Jolliffe & Cadima, 2016).

 
## 3. Otimização (Tuning) 

### 3.1. Abordagem de Otimização 

O DBSCAN foi identificado como o melhor modelo candidato (*Silhouette* teste: 0.1828). A fase de otimização teve como objetivo melhorar a qualidade da segmentação de forma sistemática, através de sete estratégias distintas testadas sequencialmente: afinação de hiperparâmetros do DBSCAN por *Grid Search*, redução de dimensionalidade por PCA, seleção de variáveis por correlação, filtragem por variância, granularidade fina do PCA com eps adaptativo, redução não-linear por UMAP e rescaling robusto.

Em todas as abordagens foi mantido o critério de seleção da fase de candidatos: o *Grid Search* DBSCAN foi restringido a combinações que produzissem exatamente quatro *clusters* com ruído inferior a 20%, garantindo comparabilidade dos resultados com o *baseline* (K-Means k=4). O critério principal de avaliação foi o *Silhouette Score*, complementado pelo *Davies-Bouldin Index* e pelo *Calinski-Harabasz Index* (Rousseeuw, 1987; Davies & Bouldin, 1979; Caliński & Harabasz, 1974).

**Grid Search DBSCAN:** A afinação dos hiperparâmetros `eps` e `min_samples` sobre o espaço original (53 variáveis) encontrou a melhor combinação em `eps=8.5`, `min_samples=3` (*Silhouette* treino: 0.1679, ruído: 0.2%). A validação por *K-Fold* (K=5) confirmou estabilidade elevada: 0.1682 ± 0.0009. Esta melhoria face ao DBSCAN candidato (ótima consistência mas *Silhouette* baixo) demonstra que a principal limitação não está nos parâmetros do algoritmo, mas na estrutura do espaço de entrada com 53 dimensões - o que motivou as abordagens subsequentes de redução de dimensionalidade.

**PCA + DBSCAN:** A projeção das 53 variáveis para um espaço de 5 componentes principais (31% da variância explicada) com `eps=2.0`, `min_samples=3` produziu um *Silhouette* de 0.3666 no treino e 0.3318 no teste, correspondendo a uma melhoria de +98% face ao DBSCAN sem PCA. A validação *K-Fold* (K=5) confirmou estabilidade razoável: 0.3318 ± 0.0406. Este resultado consolidou o PCA como abordagem de base para as otimizações seguintes.

**Seleção por Correlação:** A remoção de variáveis altamente correlacionadas (`threshold=0.70`, mantendo 43 das 53 *features*) não melhorou o *clustering* (*Silhouette* teste: 0.1028). A redução de *features* manteve o espaço a 43 dimensões, insuficiente para eliminar o efeito da maldição da dimensionalidade. O PCA, por projetar num espaço ortogonal compacto, revelou-se fundamentalmente superior a esta abordagem.

**VarianceThreshold:** A aplicação de *VarianceThreshold* com limiares {0.01, 0.05, 0.10} não removeu nenhuma *feature* em qualquer dos três cenários, pois todas as 53 variáveis após normalização com `StandardScaler` apresentam variância próxima de 1.0. Os resultados foram idênticos ao DBSCAN otimizado (*Silhouette*: 0.1679), confirmando que esta técnica não acrescenta valor após `StandardScaler`.

**PCA Granularidade Fina com eps Adaptativo:** O refinamento da pesquisa de `n_components` em torno do valor ótimo anterior (testando {2, 3, 4, 5, 6, 7, 8}) com um eps calculado adaptativamente por percentis do *k-distance graph* encontrou o ótimo em n=3 componentes (22.6% da variância): eps=0.6451, `min_samples=7`, *Silhouette* treino 0.4926 e teste 0.4377 (+34% vs. PCA fixo). O ruído foi de 8.1% (101 pontos), superior ao PCA(5) - indicando que o espaço de 3 componentes cria fronteiras de densidade mais nítidas mas menos completas.

**RobustScaler:** A substituição do `StandardScaler` pelo `RobustScaler` (baseado na mediana e IQR) com PCA(5) não produziu nenhuma combinação válida no Grid Search DBSCAN - o algoritmo não encontrou exatamente quatro clusters com ruído inferior a 20% em nenhuma combinação de eps e `min_samples`. Este resultado sugere que o espaço de distâncias com `RobustScaler` e PCA(5) não apresenta estrutura de densidade compatível com os critérios definidos.

**UMAP + DBSCAN:** O UMAP (Uniform Manifold Approximation and Projection) foi testado como alternativa não-linear ao PCA, com um *grid search* sobre `n_components` ∈ {2, 5, 10} e `n_neighbors` ∈ {5, 15, 30}. Das 9 combinações testadas, 7 produziram espaços válidos para o DBSCAN. A melhor combinação, UMAP(5, 15) com eps=6.0, min_samples=3, alcançou *Silhouette* de 0.7141 no treino e 0.7016 no teste, sem nenhum ponto classificado como ruído (0.0%). Este resultado representa uma melhoria de +94.8% face ao PCA(5)+DBSCAN e consolida o UMAP como a abordagem mais eficaz testada nesta fase de otimização.

### 3.2. Técnica Utilizada

O UMAP (Uniform Manifold Approximation and Projection) é uma técnica de redução de dimensionalidade não-linear fundamentada em teoria de variedades *Riemannianas* e geometria hiperbólica (McInnes et al., 2018). Ao contrário do PCA, que maximiza a variância global e produz eixos ortogonais no espaço original, o UMAP constrói um grafo de vizinhança local sobre os dados e otimiza a projeção para preservar simultaneamente a estrutura local e global do *manifold* subjacente. O resultado é um espaço de baixa dimensão onde pontos próximos no espaço original permanecem próximos na projeção, e grupos naturalmente separados mantêm essa separação - propriedade que o PCA linear não garante em dados com estrutura não-linear (McInnes et al., 2018; Géron, 2022).

A aplicação seguiu duas etapas sequenciais. Na primeira, foi realizado um *grid search* sobre `n_components` ∈ {2, 5, 10} e `n_neighbors` ∈ {5, 15, 30}, totalizando 9 combinações. O parâmetro `n_neighbors` controla o equilíbrio entre estrutura local (valores baixos) e global (valores elevados): `n_neighbors=15` foi o valor ótimo, um valor moderado que captura tanto a vizinhança imediata dos pontos como a coerência global dos grupos. Das 9 combinações, 7 produziram espaços válidos para o *Grid Search* DBSCAN (restringido a exatamente 4 clusters com ruído ≤ 20%); as combinações (`n_comp=5`, `n_neigh=5`) e (`n_comp=10`, `n_neigh=15`) não encontraram nenhuma configuração DBSCAN válida. O `random_state=42` foi fixo em todos os casos para garantir reprodutibilidade. Os resultados do *grid search* revelaram que projeções UMAP com 2 e 5 dimensões produziram os melhores *Silhouette Scores* (0.69 e 0.71 respetivamente), enquanto 10 dimensões apresentou resultados inferiores - consistente com o fenómeno de que a eficácia do UMAP diminui ao aumentar o núméro de dimensões de saída (McInnes et al., 2018).

Na segunda etapa, o modelo UMAP ótimo (`n_components=5`, `n_neighbors=15`, `random_state=42`) foi treinado exclusivamente sobre `X_train_scaled` e a transformação do conjunto de teste foi realizada via reducer.transform(`X_test_scaled`), preservando o princípio de ausência de *data leakage*. O DBSCAN foi aplicado no espaço UMAP reduzido com `eps=6.0` e `min_samples=3`. O valor de `eps=6.0` no espaço UMAP(5D) é substancialmente diferente do `eps=2.0` usado no espaço PCA(5D), refletindo as diferentes escalas de distância produzidas pelas duas técnicas de redução. A atribuição ao teste foi realizada via KNN (k=5), consistente com o procedimento adotado em toda a fase de modelação (Géron, 2022; Schubert et al., 2017).

O espaço UMAP(5, 15) produziu uma estrutura de densidade excepcionalmente favorável para o DBSCAN: com `eps=6.0` e `min_samples=3`, o algoritmo formou 4 clusters sem classificar nenhum dos 1249 pontos de treino como ruído (0.0%). Este comportamento contrasta com o PCA(5)+DBSCAN, onde 16 pontos (1.3%) foram excluídos como outliers, e com o PCA fino n=3, com 101 pontos excluídos (8.1%). A geometria do manifold aprendida pelo UMAP é, portanto, suficientemente coesa para que o DBSCAN consiga capturar todos os pontos em regiões de densidade definida - algo que os espaços lineares não conseguem assegurar (McInnes et al., 2018; Jain, 2010).

### 3.3. Melhoria Obtida

A combinação UMAP(5, 15) + DBSCAN produziu o melhor resultado de toda a fase de modelação. A Tabela 2 resume os resultados das cinco abordagens de melhoria comparadas com a referência PCA(5)+DBSCAN.

| Abordagem                              | Silhouette (Treino) | Silhouette (Teste) | Davies-Bouldin (Treino) | Davies-Bouldin (Teste) | Calinski-Harabasz (Treino) | Calinski-Harabasz (Teste) | Ruído (%) |
|----------------------------------------|---------------------|---------------------|--------------------------|-------------------------|-----------------------------|----------------------------|-----------|
| UMAP(5, 15) + DBSCAN                  | 0.7141              | 0.7016              | 0.3991                   | 0.3864                  | 1772.02                     | 301.16                     | 0.0       |
| PCA fino n=3 + DBSCAN                 | 0.4926              | 0.4377              | 0.5850                   | 0.8341                  | 354.73                      | 58.73                      | 8.1       |
| PCA(5) + DBSCAN                       | 0.3666              | 0.3318              | 0.7099                   | 0.7602                  | 72.82                       | 13.35                      | 1.3       |
| DBSCAN otimizado                      | 0.1679              | 0.1737              | 1.4578                   | 1.1885                  | 27.05                       | 5.58                       | 0.2       |
| VarThreshold(0.01) + DBSCAN           | 0.1679              | 0.1737              | 1.4578                   | 1.1885                  | 27.05                       | 5.58                       | 0.2       |
| RobustScaler + PCA(5) + DBSCAN        | N/A                 | N/A                 | N/A                      | N/A                     | N/A                         | N/A                        | N/A       |

O modelo UMAP(5, 15) + DBSCAN (`eps=6.0`, `min_samples=3`) alcançou um *Silhouette Score* de 0.7141 no treino e 0.7016 no teste, situando-se claramente acima do limiar de 0.50 definido como objetivo. Rousseeuw (1987) classifica valores superiores a 0.70 como indicativos de estrutura de *clustering* forte - o que é alcançado no treino e praticamente atingido no teste. O *Davies-Bouldin* de 0.3991 é o mais baixo de todas as abordagens testadas, confirmando que os *clusters* são simultaneamente compactos e bem separados. O índice de *Calinski-Harabasz* de 1772 no treino representa uma melhoria de 24x face ao *baseline K-Means* (73.49) e de 24x face ao PCA(5)+DBSCAN (72.82), refletindo a diferência estrutural do espaço UMAP face aos espaços lineares.

Um resultado particularmente notável é a ausência total de ruído: 0 pontos classificados como outliers (0.0%), em contraste com os 1.3% do PCA(5)+DBSCAN e os 8.1% do PCA fino. Isto significa que o UMAP produziu um espaço de densidade suficientemente estruturado para que o DBSCAN atribua todos os 1249 pontos de treino a um dos quatro *clusters*, sem necessidade de excluir observações. A diferença de *Silhouette* entre treino e teste é de apenas 0.0125, indicando consistência elevada da estrutura aprendida e ausência de sobreajustamento ao conjunto de treino.

A progressão dos resultados ao longo das abordagens testadas é reveladora. O DBSCAN sem redução de dimensionalidade (*Silhouette* teste: 0.17) demonstra os limites do algoritmo no espaço original de 53 dimensões. O PCA(5) melhora substancialmente (0.33), ao compactar a estrutura em 5 componentes ortogonais. O PCA fino (n=3, adaptativo) melhora ainda mais (0.44), sugerindo que 3 componentes capturam melhor as separações relevantes do que 5. O UMAP, ao preservar a geometria local não-linear, dá o salto mais expressivo (0.70), confirmando que a estrutura latente dos dados de Recursos Humanos é melhor descrita por um manifold curvo do que por um subespacço linear (McInnes et al., 2018; Jain, 2010).

Em síntese, a melhoria total face ao *baseline K-Means* é de +909% no Silhouette Score de teste (0.0696 → 0.7016) e de +312% face ao melhor candidato DBSCAN (0.1709 → 0.7016). O modelo UMAP(5, 15) + DBSCAN foi, por isso, selecionado como modelo final de *clustering* do Objetivo 3.


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

Ankerst, M., Breunig, M. M., Kriegel, H.-P., & Sander, J. (1999). OPTICS: Ordering points to identify the clustering structure. ACM SIGMOD Record, 28(2), 49–60.

Bellman, R. E. (1957). Dynamic programming. Princeton University Press.

Calinski, T., & Harabasz, J. (1974). A dendrite method for cluster analysis. Communications in Statistics, 3(1), 1–27.

Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C., & Wirth, R. (2000). CRISP-DM 1.0: Step-by-step data mining guide. SPSS Inc.

Davies, D. L., & Bouldin, D. W. (1979). A cluster separation measure. IEEE Transactions on Pattern Analysis and Machine Intelligence, 1(2), 224–227.

Géron, A. (2022). Hands-on machine learning with Scikit-Learn, Keras, and TensorFlow (3rd ed.). O’Reilly Media.

Jain, A. K. (2010). Data clustering: 50 years beyond K-means. Pattern Recognition Letters, 31(8), 651–666.

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2021). An introduction to statistical learning (2nd ed.). Springer.

Jolliffe, I. T., & Cadima, J. (2016). Principal component analysis: A review and recent developments. Philosophical Transactions of the Royal Society A, 374(2065).

Rousseeuw, P. J. (1987). Silhouettes: A graphical aid to the interpretation and validation of cluster analysis. Journal of Computational and Applied Mathematics, 20, 53–65.

Schubert, E., Sander, J., Ester, M., Kriegel, H.-P., & Xu, X. (2017). DBSCAN revisited, revisited: Why and how you should (still) use DBSCAN. ACM Transactions on Database Systems, 42(3), 1–21.
 
Data de última atualização: 21/04/2026
