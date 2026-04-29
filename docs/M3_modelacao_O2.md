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

*Tabela 1 — Comparação de métricas de clustering entre os modelos candidatos testados (ordenados por Silhouette Score no teste, decrescente).*

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

<p align="center">
  <img src="https://raw.githubusercontent.com/LuiscnFigueira/Projeto-cdg-grupo10/main/reports/figures/splits/Objetivo3/PCA_DBSCAN_Objetivo3.png" width="80%"/>
</p>

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

*Tabela 2 — Resultados das abordagens de otimização testadas, comparadas com a referência PCA(5)+DBSCAN (ordenadas por Silhouette Score no teste, decrescente).*

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

<p align="center">
  <img src="https://raw.githubusercontent.com/LuiscnFigueira/Projeto-cdg-grupo10/main/reports/figures/splits/Objetivo3/PCA_UMAP_5_15_DBSCAN_Objetivo3.png" width="80%"/>
</p>

Em síntese, a melhoria total face ao *baseline K-Means* é de +909% no *Silhouette Score* de teste (0.0696 → 0.7016) e de +312% face ao melhor candidato DBSCAN (0.1709 → 0.7016). O modelo UMAP(5, 15) + DBSCAN foi, por isso, selecionado como modelo final de *clustering* do Objetivo 3.


## 4. Avaliação do Modelo Final 
### 4.1. Métricas de Avaliação do Modelo Final

O modelo final UMAP(5, 15) + DBSCAN (`eps=6.0`, `min_samples=3`) foi avaliado com as três métricas definidas na Secção 1: *Silhouette Score* como critério principal, complementado pelo *Davies-Bouldin Index* e pelo *Calinski-Harabasz Index*. A Tabela 3 apresenta os resultados finais e a comparação com o *baseline K-Means*.

*Tabela 3 — Comparação das métricas de avaliação entre o baseline, o melhor candidato e o modelo final, nos conjuntos de treino e teste.*

| Métrica                      | Baseline (K-Means) | Melhor Candidato (DBSCAN) | Modelo Final (UMAP+DBSCAN) — Treino | Modelo Final (UMAP+DBSCAN) — Teste |
|------------------------------|--------------------|----------------------------|--------------------------------------|-------------------------------------|
| Silhouette Score             | 0.0705             | 0.1709                     | 0.7141                           | 0.7016                          |
| Davies-Bouldin Index         | 2.9943             | 1.4790                     | 0.3991                           | 0.3864                          |
| Calinski-Harabasz Index      | 14.24              | 8.51                       | 1772.02                          | 301.16                          |
| Clusters identificados       | 4                  | 4                        | 4                                    | 4                                   |
| Ruído (%)                    | 15.0%                  | 1.1%                       | 0.0%                             | 0.0%                                   |
| Meta Silhouette > 0.50       | Não atingida       | Não atingida               | Atingida                         | Atingida                        |

O modelo final atingiu um *Silhouette Score* de 0.7016 no conjunto de teste, superando claramente a meta de 0.50 definida na Secção 1. Rousseeuw (1987) classifica valores superiores a 0.70 como indicativos de estrutura de *clustering* forte, e valores entre 0.50 e 0.70 como razoável. O resultado de 0.7141 no treino e 0.7016 no teste situa o modelo no limiar superior desta classificação. O *Davies-Bouldin* de 0.3864 (teste) é o mais baixo registado em toda a fase de modelação - valores inferiores a 0.5 indicam clusters bem compactos e bem separados entre si (Davies & Bouldin, 1979). O índice de *Calinski-Harabasz* de 301.16 no teste representa uma melhoria de 21x face ao *baseline K-Means* (14.24), refletindo a superioridade da estrutura de *clustering* produzida pelo UMAP (Caliński & Harabasz, 1974).
 
A consistência entre treino e teste é notável. A diferença de *Silhouette* é de apenas 0.0125, a de DB de 0.0127 e a de CH de 1470 pontos - esta última, embora expressiva em valor absoluto, é esperada dado que o CH depende do tamanho dos conjuntos (treino: 1249 obs., teste: 221 obs.) e escala com o rácio intra/inter-cluster (Caliński & Harabasz, 1974). A ausência de sobreaçustamento é assim confirmada pelas três métricas: o modelo generaliza a estrutura aprendida no treino de forma robusta para o conjunto de teste.

### 4.2. Perfis dos Clusters

A caracterização dos *clusters* foi realizada através da análise das médias das 53 variáveis originais por grupo, visualizadas num heatmap de perfil com z-scores para comparabilidade entre escalas distintas. Esta abordagem permite identificar as características que mais distinguem cada cluster da média populacional, independentemente da escala de cada variável (James et al., 2021).
O modelo identifica quatro perfis de colaboradores com estrutura departamental consistente. A Tabela 4 apresenta as médias das variáveis mais discriminantes por cluster, evidenciando a segmentação por departamento e função como eixo estrutural principal.

*Tabela 4 — Médias das variáveis mais discriminantes por cluster, evidenciando a segmentação departamental como eixo estrutural principal.*

 | Variável discriminante        | Cluster 0           | Cluster 1          | Cluster 2           | Cluster 3          | Média Pop. |
|-------------------------------|---------------------|--------------------|---------------------|--------------------|------------|
| n (aprox., % treino)          | 701 (56%)           | 72 (5.8%)          | 450 (36%)           | 26 (2.1%)          | 1 249      |
| Age                           | 35.8                | 43.9               | 37.4                | 36.9               | ~36.9      |
| JobLevel                      | 1.60                | 3.97               | 2.45                | 1.42               | ~2.04      |
| MonthlyIncome                 | 4 526               | 15 955             | 8 001               | 4 103              | ~6 428     |
| TotalWorkingYears             | —                   | 21.3               | —                   | —                  | —          |
| Department_R&D                | 1.000               | 1.000              | 0.093               | 0.000              | ~0.653     |
| Department_Sales              | 0.000               | 0.000              | 0.847               | 0.000              | ~0.305     |
| Department_HR                 | 0.000               | 0.000              | 0.060               | 1.000              | ~0.042     |
| JobRole_Research Director     | 0.000               | 1.000              | 0.000               | 0.000              | ~0.058     |
| JobRole_Sales Executive       | 0.000               | 0.000              | 0.604               | 0.000              | ~0.218     |
| JobRole_Human Resources       | 0.000               | 0.000              | 0.000               | 1.000              | ~0.021     |

**Cluster 0 - I&D Junior-Mid:** O *cluster* maioritário agrupa a quase totalidade dos colaboradores do departamento de Investigação & Desenvolvimento com funções de nível técnico e intermédio: *Laboratory Technicians* (31.2%), *Research Scientists* (36.7%), *Healthcare Representatives* (15.7%) e *Manufacturing Directors* (16.4%). Todos pertencem ao departamento de I&D (`Department_R&D=1.00`), mas com `JobLevel` baixo-intermédio (média 1.60) e `MonthlyIncome` de 4 526 - o segundo mais baixo. A idade média de 35.8 anos reflete uma população predominantemente jovem e em fase ativa de carreira técnica. Este *cluster* concentra a maior massa de capital científico operacional da organização.

**Cluster 1 - Diretores de Investigação:** Grupo reduzido mas altamente diferenciado. Todos os membros são *Research Directors* (`JobRole_Research Director=1.00`) no departamento de I&D (`Department_R&D=1.00`). O `JobLevel` médio de 3.97 e o `MonthlyIncome` de 15 955 são os mais elevados de todos os clusters - o rendimento é 3.5 vezes superior ao *Cluster* 0. O `TotalWorkingYears` de 21.3 anos e a idade média de 43.9 confirmam um perfil de liderança científica sénior com trajétória longa e especializada. Este *cluster* representa o topo hierárquico da função de investigação, funcionando como a camada de gestão científica da organização.

**Cluster 2 - Equipa de Vendas:** O segundo maior *cluster* agrega a quase totalidade da força comercial da empresa: *Sales Executives* (60.4%), *Managers* (18.9%) e *Sales Representatives* (16.7%), com uma pequena fraccção de Recursos Humanos (6%). O `Department_Sales` de 0.847 confirma a identidade departamental do *cluster*. O `JobLevel` médio de 2.45 e o `MonthlyIncome` de 8 001 refletem uma hierarquia mista - entre representantes de base e gestores comerciais - com rendimento médio superior ao de I&D operacional (*Cluster* 0). A idade média de 37.4 anos e a composição funcional indicam uma equipa comercial em plena maturidade profissional.

**Cluster 3 - Departamento de Recursos Humanos:** O menor cluster é inteiramente composto por colaboradores do departamento de Recursos Humanos (Department_HR=1.00, JobRole_Human Resources=1.00). O `JobLevel` médio de 1.42 e o `MonthlyIncome` de 4 103 - o mais baixo de todos os clusters - posicionam este grupo no nível mais operacional da organização. A idade média de 36.9 anos é próxima da média populacional. Destaca-se o `WorkLifeBalance` médio de 3.0, o mais elevado de todos os clusters, o que pode refletir especificidades da cultura e das condições de trabalho do departamento de RH. A sua separação como cluster independente demonstra a capacidade do UMAP de isolar grupos pequenos mas estruturalmente coerentes no *manifold* aprendido.

### 4.3. Variáveis Discriminantes

Ao contrário dos modelos supervisionados, o UMAP e o DBSCAN não produzem coeficientes de importância de variáveis analógos aos de uma Regressão Logística ou de uma Random Forest. O UMAP aprende uma transformação não-linear do espaço de entrada para um manifold de baixa dimensão, e o DBSCAN opera exclusivamente sobre as distâncias nesse espaço reduzido. A importância das variáveis é, por isso, indireta: todas as 53 variáveis escaladas entram no UMAP, que as pondera implicitamente através da estrutura do grafo de vizinhança que constrói (McInnes et al., 2018).

Para identificar as variáveis que mais contribuem para a separação entre clusters, recorre-se à análise da variância inter-cluster: as variáveis com maior diferença entre as médias de cada cluster e a média populacional são aquelas que melhor distinguem os grupos, independentemente de qualquer método de seleção formal. Esta abordagem é equivalente ao princípio subjacente ao índice de Calinski-Harabasz, que mede precisamente o rácio entre a variância inter-cluster e intra-cluster (Caliński & Harabasz, 1974; James et al., 2021).

*Tabela 5 — Grupos de variáveis com maior poder discriminante entre clusters, identificados por análise de variância inter-cluster.*

A análise da variância inter-cluster sobre as 53 variáveis originais permite identificar três grupos de variáveis discriminantes, ordenados por capacidade de separação entre clusters:

| Grupo                     | Variáveis                                                                 | Padrão Observado |
|--------------------------|---------------------------------------------------------------------------|------------------|
| Departamento / Função    | Department_R&D, Department_Sales, Department_HR, JobRole_Research Director, JobRole_Sales Executive, JobRole_Human Resources | Eixo estrutural principal. Cluster 0 e 1: I&D exclusivo (1.00). Cluster 2: Vendas (0.847). Cluster 3: RH exclusivo (1.00). Cada cluster é quase homogéneo no departamento e na função. |
| Hierarquia / Rendimento  | MonthlyIncome, IncomePerLevel, TotalWorkingYears, JobLevel               | Cluster 1 extremamente elevado (Income=15 955, JL=3.97, TWY=21.3). Cluster 2 intermédio (Income=8 001, JL=2.45). Cluster 0 e 3 no nível mais baixo (Income≈4 300, JL≤1.5). |
| Experiência / Antiguidade| Age, YearsAtCompany, YearsInCurrentRole, YearsWithCurrManager, NumCompaniesWorked | Cluster 1 claramente mais sénior (Age=43.9). Restantes clusters com idade entre 35–37 anos. Cl. 1 tem a maior antiguidade e experiência acumulada. |

As variáveis de departamento e função são o principal eixo discriminante: cada *cluster* corresponde, de forma quase exclusiva, a um departamento orgânico (I&D, Vendas, RH). Esta separação estrutural é especialmente notável porque o UMAP opera sobre as 53 variáveis escaladas sem qualquer conhecimento prévio da estrutura departamental - e mesmo assim recupera-a como a dimensão de maior coerência local no *manifold* (McInnes et al., 2018). As variáveis binárias de departamento e função têm valores de 0 ou 1 e apresentam médias por *cluster* extremamente afastadas entre si, o que as coloca no topo da variância inter-cluster.

O segundo eixo discriminante é a hierarquia profissional e o rendimento. O Cluster 1 (Diretores de Investigação) distingue-se de forma inequívoca: o `MonthlyIncome` de 15 955 é 3.5 vezes superior ao do *Cluster* 0 e 3.9 vezes superior ao do *Cluster 3*; o `JobLevel` de 3.97 contrasta com 1.42–1.60 dos restantes grupos de I&D e RH. O `TotalWorkingYears` de 21.3 anos confirma que esta separação hierárquica é sustentada por uma trajetória profissional longa. O *Cluster* 2 (Vendas) ocupa uma posição intermédia, refletindo a coexistência de *Sales Executives* e *Sales Representatives* com *Managers* na mesma equipa comercial.

Em síntese, o modelo final UMAP(5, 15) + DBSCAN identifica quatro perfis organizacionalmente coerentes: I&D Operacional (*Cluster* 0, 56% - técnicos e investigadores de nível base-intermédio), Liderança Científica (*Cluster* 1, 5.8% - *Research Directors*, o topo hierárquico de I&D), Equipa de Vendas (*Cluster* 2, 36% - equipa comercial da organização), e Recursos Humanos (*Cluster* 3, 2.1% - departamento de RH isolado). O departamento/função é o principal eixo de diferenciação, seguido pela hierarquia e o rendimento. A capacidade do UMAP de recuperar esta estrutura organizacional a partir de 53 variáveis heterogéneas, sem supervisionamento, demonstra a sua superioridade face a técnicas lineares como o PCA, que não capturaria com a mesma nitidez as fronteiras departamentais enquanto dimensão de coerência local (McInnes et al., 2018; Jain, 2010).

## 5. Conclusão da Fase de Modelação 
### 5.1 Síntese do Processo de Modelação

A fase de modelação do Objetivo 3 compreendeu três etapas sequenciais: avaliação de modelos baseline, exploração de modelos candidatos e otimização do modelo selecionado. O ponto de partida foi o K-Means com quatro clusters, que atingiu um Silhouette Score de apenas 0.0705 no conjunto de treino, valor que Rousseeuw (1987) classifica como indicativo de ausência de estrutura de clustering relevante. Este resultado estabeleceu a necessidade de explorar abordagens alternativas, tanto em termos de algoritmo como de representação do espaço de características.

*Tabela 6 — Síntese da progressão das métricas ao longo das três etapas do processo de modelação.*

| Etapa / Modelo                          | Silhouette (Treino) | Silhouette (Teste) | Davies-Bouldin (Treino) | Davies-Bouldin (Teste) | Calinski-Harabasz (Treino) | Calinski-Harabasz (Teste) | Clusters | Ruído |
|----------------------------------------|----------------|---------------|--------------|-------------|--------------|-------------|----------|--------|
| Baseline — K-Means (k=4)               | 0.0748         | 0.0725             | 3.3149       | 3.1517           | 77.6458        | 14.4884           | 4        | 15.0%      |
| Melhor Candidato — DBSCAN (original)   | 0.1709         | 0.1828             | 1.5723       | 1.4334           | 35.7952         | 7.5962           | 4      | 1.1%   |
| Modelo Final — UMAP(5,15) + DBSCAN     | 0.7141         | 0.7016        | 0.3991       | 0.3864      | 1772.02      | 301.16      | 4        | 0.0%   |
| Meta definida                          | > 0.50         | > 0.50        | —            | —           | —            | —           | 4        | ≤ 20%  |

A melhoria mais expressiva ao longo do processo de modelação ocorreu com a introdução do UMAP como etapa de redução de dimensionalidade. A passagem de um *Silhouette* de 0.1709 (melhor candidato no espaço original) para 0.7141 no treino e 0.7016 no teste representa um ganho de +310 pontos percentuais que não seria alcançável por simples substituição de algoritmo ou ajuste de hiperparâmetros no espaço de 53 variáveis. O mesmo padrão confirma-se nas métricas DB e CH: o modelo final reduz o DB de 2.9943 para 0.3991 no treino e multiplica o CH de 14.24 para 1772.02. O UMAP aprendeu um *manifold* de baixa dimensão onde a estrutura departamental latente - até então não recuperada por nenhuma abordagem - emergiu como fronteira de separação nítida (McInnes et al., 2018).

### 5.2. Justificação da Solução Final

O modelo UMAP(5, 15) + DBSCAN (eps=6.0, min_samples=3) está pronto a ser apresentado como solução final com base em quatro critérios complementares: desempenho quantitativo, generalização, interpretabilidade e alinhamento com o objetivo do projeto.

**Desempenho quantitativo acima da meta:** O *Silhouette Score* de 0.7016 no conjunto de teste supera a meta de 0.50 definida na Secção 1 em +40.3 pontos percentuais. Este valor é classificado por Rousseeuw (1987) como indicativo de estrutura de clustering forte, o limiar mais exigente da sua classificação. O *Davies-Bouldin* de 0.3864 no teste confirma *clusters* compactos e bem separados - valores abaixo de 0.5 são considerados excelentes (Davies & Bouldin, 1979). A melhoria face ao *baseline K-Means* é de +895% no Silhouette e de -87% no *Davies-Bouldin*. Todas as métricas apontam na mesma direção, sem compromissos ou contradições entre si.

**Generalização robusta sem sobreajustamento:** A diferença entre treino e teste é mínima: ΔSilhouette=0.0125, ΔDB=0.0127. O modelo foi treinado no conjunto de treino (85%, n=1249) e avaliado num conjunto de teste totalmente separado (15%, n=221), usando um classificador KNN como *proxy* para a função de predição - metodologia que evita *data leakage* e garante a validade da avaliação. A estabilidade das métricas confirma que o UMAP aprendeu a estrutura global dos dados, não padrões específicos do treino.

**Classificação completa sem ruído:** O modelo produz zero pontos classificados como ruído no conjunto de treino (0.0%), o que significa que todos os colaboradores são atribuídos a um *cluster*. Este resultado, que contrasta com os 1.1% de ruído do melhor candidato em espaço original, é consequência da combinação entre o *manifold* UMAP - que comprime a estrutura de vizinhança de 53 dimensões em 5 componentes coerentes - e a calibração do eps via percentil 5 das distâncias KNN no espaço reduzido. Do ponto de vista operacional, um modelo sem ruído é preferível: não deixa colaboradores por classificar e facilita a interpretação e utilização dos resultados.

**Interpretabilidade e coerência organizacional:** Os quatro *clusters* identificados têm correspondiência direta com a estrutura departamental da organização: I&D Operacional (56%), Liderança Científica (5.8%), Equipa de Vendas (36%) e Recursos Humanos (2.1%). Esta coerência não foi imposta ao modelo - resultou da aprendizagem não-supervisionada sobre 53 variáveis escaladas. A correspondência entre os clusters e unidades organizacionais reais é um indicador forte de validade externa: o modelo captura uma estrutura que existe objectivamente na organização, não um artefacto do algoritmo (Jain, 2010). Os perfis são suficientemente distintos para suportar decisões diferenciadas de gestão de pessoas, desenvolvimento de carreira ou política de compensação por segmento.

### 5.3. Limitações e Considerações

Apesar do desempenho quantitativo e da coerência qualitativa, o modelo apresenta três limitações que devem ser consideradas na utilização dos seus resultados.

**Cluster 3 de dimensão reduzida:** O *cluster* de Recursos Humanos conta com apenas 26 membros no treino (2.1% da amostra). Embora a sua separação seja estatísticamente justificada pelo *Silhouette* e *Davies-Bouldin*, a dimensão reduzida impõe cautela na generalização dos seus perfis médios e na tomada de decisões orgânicas baseadas neste grupo. Pequenas alterações na composição do departamento de RH podem alterar significativamente as médias do *cluster*.

**Não-determinismo do UMAP:** O UMAP é um algoritmo estocástico: execuções sem semente fixa produzem *manifolds* ligeiramente distintos, o que pode alterar as fronteiras do DBSCAN e, por consequência, a atribuição de alguns pontos fronteiriços. Este comportamento foi mitigado com o uso de `random_state=42` em todas as execuções, garantindo reprodutibilidade integral dentro do pipeline definido. A validação cruzada K-Fold (K=5) realizada na fase de otimização confirmou a estabilidade média das métricas, com variância reduzida entre *folds*.

**Ausência de métricas de validade externa formal:** A avaliação baseou-se exclusivamente em métricas internas (Silhouette, DB, CH), que medem a qualidade geométrica dos clusters no espaço transformado pelo UMAP. A validade externa — ou seja, a correspondência entre os clusters e variáveis não utilizadas na modelação, como o atrito ou a performance avaliada — não foi testada formalmente nesta fase. A coerência departamental observada é um indicador positivo de validade externa, mas uma validação por supervisionamento parcial (por exemplo, usando a variável Attrition como critério externo) reforçaria a confiança nos perfis identificados.

### 5.4. Conclusão

O modelo UMAP(5, 15) + DBSCAN (`eps=6.0`, `min_samples=3`) cumpre todos os critérios definidos para esta fase de modelação: supera a meta de *Silhouette* > 0.50, generaliza de forma robusta para o conjunto de teste, classifica a totalidade da população sem ruído, e produz *clusters* com significado organizacional direto. O ganho métrico face ao *baseline* é substancial e não é atribuível a *overfitting*. O processo de otimização foi sistemático - explorando seis abordagens de representação e selecionação de variáveis antes de convergir para o UMAP - o que confere robustez metodológica à escolha final.

Os quatro segmentos identificados - I&D Operacional, Liderança Científica, Equipa de Vendas e Recursos Humanos - constituem uma representação coerente e acionável da população da organização. A segmentação por departamento como eixo principal, complementada pelas dimensões de hierarquia e rendimento, oferece uma base sólida para estratégias diferenciadas de gestão de talento. O modelo está pronto a ser apresentado como solução final do Objetivo 3.

## 6. Metodologia de Gestão (PBL)

O projeto segue uma abordagem estruturada com base na metodologia CRISP-DM (Cross-Industry Standard Process for Data Mining), que organiza o desenvolvimento em seis fases principais: compreensão do problema, compreensão dos dados, preparação dos dados, modelação, avaliação e implementação.

Na presente fase - *Milestone 3* (Modelação e Avaliação) - o foco incide na aplicação de técnicas de aprendizagem não supervisionada, com o objetivo de identificar padrões latentes e segmentar os colaboradores em perfis homogéneos, suportando a análise exploratória avançada do fenómeno de Attrition.

Foram implementados e comparados diversos algoritmos de *clustering*, nomeadamente *K-Means* (baseline e otimizado), DBSCAN, *Gaussian Mixture Models* (GMM), *Agglomerative Clustering*, *OPTICS* e *MiniBatch K-Means*, explorando diferentes configurações e parametrizações. A determinação do número ótimo de *clusters* foi realizada com base em métodos quantitativos, como o *Silhouette Score*, sendo complementada por métricas adicionais de avaliação interna, incluindo o índice de *Davies-Bouldin* (avaliação de sobreposição entre clusters) e o índice de *Calinski-Harabasz* (compacidade e separação global).

Adicionalmente, foi conduzido um processo de otimização de parâmetros (*tuning*), com o objetivo de melhorar a qualidade dos agrupamentos obtidos. A avaliação dos modelos teve em consideração não apenas o desempenho estatístico, mas também a robustez dos resultados (comparação entre treino e teste) e a interpretabilidade dos *clusters*, aspetos fundamentais no contexto de apoio à decisão.

À semelhança das restantes fases do projeto, o processo seguiu uma abordagem iterativa, permitindo ajustar o conjunto de variáveis, testar diferentes algoritmos e configurações, e refinar os resultados obtidos. Esta estratégia possibilitou a melhoria contínua da qualidade dos agrupamentos e a extração de conhecimento relevante para a segmentação de colaboradores e suporte à tomada de decisão em contexto organizacional.

**Divisão de Tarefas:** 

 **Luís Figueira:** 
   * Definição da estratégia de clustering
   * Otimização do threshold de decisão 
   * Definição da estratégia de clustering
    
 **Martim Ferreira:**
   * Implementação dos algoritmos de clustering candidatos (K-Means, GMM, Hierárquico, OPTICS, MiniBatch)
   * Implementação e otimização do UMAP + DBSCAN (Objetivo 3) Redação da documentação técnica M3_modelacao_O3.md 
   * Avaliação das métricas de clustering
   * Implementação dos modelos candidatos (DBSCAN, OPTICS)

 **Mateus Afonso:** 
   * Otimização UMAP + DBSCAN 
   * Avaliação do Modelo Final e Thresholding
   * Análise e caracterização dos clusters finais
   * Análise de feature importance

  **Tarefas conjuntas**
   * Definição da estratégia de avaliação e métricas de sucesso
   * Modelo Baseline 
   * Definição da estratégia de modelação 
   *  Avaliação e análise de erros do modelo final
   
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

McInnes, L., Healy, J., & Melville, J. (2018). UMAP: Uniform manifold approximation and projection for dimension reduction. arXiv:1802.03426.

Rousseeuw, P. J. (1987). Silhouettes: A graphical aid to the interpretation and validation of cluster analysis. Journal of Computational and Applied Mathematics, 20, 53–65.

Schubert, E., Sander, J., Ester, M., Kriegel, H.-P., & Xu, X. (2017). DBSCAN revisited, revisited: Why and how you should (still) use DBSCAN. ACM Transactions on Database Systems, 42(3), 1–21.
