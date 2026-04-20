# Milestone 3: Modelação e Avaliação 
 
## 1. Estratégia de Modelação 

### 1.1 Divisão do Dataset (Treino/Teste)

O dataset foi dividido em dois subconjuntos distintos: treino (80%) e teste (20%), utilizando uma semente aleatória fixa (`random_state=42`) para garantir a reprodutibilidade dos resultados.

Apesar de se tratar de um problema de aprendizagem não supervisionada, foi adotada uma estratégia de amostragem estratificada com base na variável `Attrition_bin`, assegurando que a proporção de colaboradores com e sem atrito se mantém consistente entre os dois subconjuntos. Esta decisão é particularmente relevante do ponto de vista de negócio, uma vez que permite avaliar se os padrões identificados nos clusters são estáveis face à distribuição real do fenómeno de `Attrition`.

Os resultados da divisão demonstram essa consistência:

- Conjunto de treino: 1176 observações (80%)
- Conjunto de teste: 294 observações (20%)
- Proporção de *Attrition* no treino: 16.2%
- Proporção de *Attrition* no teste: 16.0%

Esta abordagem permite uma avaliação mais robusta da capacidade de generalização dos modelos, verificando se os agrupamentos identificados no treino se mantêm quando aplicados a novos dados.

### 1.2 Normalização dos Dados

Antes da aplicação dos algoritmos de clustering, foi realizada a normalização das variáveis numéricas utilizando o método `StandardScaler`.

Esta etapa é crítica porque algoritmos comoK-Means, DBSCAN e GMM baseiam-se em distâncias (nomeadamente distância euclidiana). Sem normalização, variáveis com maior escala (por exemplo, rendimento mensal) dominariam o cálculo das distâncias, distorcendo os resultados e comprometendo a qualidade dos clusters.

Para garantir rigor metodológico e evitar data leakage, o processo foi conduzido da seguinte forma:

- O `StandardScaler` foi ajustado (fit) apenas no conjunto de treino;
- Posteriormente, os parâmetros aprendidos foram aplicados (transform) ao conjunto de teste.

Após a normalização, verificou-se que:

- Média das variáveis ≈ 0
- Desvio padrão ≈ 1

Este procedimento assegura que todas as variáveis contribuem de forma equilibrada para a formação dos clusters, aumentando a fiabilidade dos modelos.

### 1.3 Métricas de Sucesso

A avaliação dos modelos de clustering foi realizada com base em métricas internas, uma vez que não existem rótulos reais que permitam uma validação supervisionada.

A métrica principal definida foi o Silhouette Score, que mede simultaneamente:

- A coesão interna dos clusters
- A separação entre clusters

Os seus valores variam entre -1 e 1, sendo valores mais elevados indicativos de melhor qualidade de agrupamento. Foi definido como objetivo atingir um valor superior a 0.50, correspondente a clusters bem definidos e separados.

Contudo, reconhecendo que o Silhouette Score, isoladamente, pode não captar toda a complexidade dos dados, foram também consideradas métricas complementares:

- Davies-Bouldin Index (DB): avalia a sobreposição entre clusters (quanto menor, melhor)
- Calinski-Harabasz Index (CH): mede a separação global entre clusters e a sua compacidade (quanto maior, melhor)

Para além destas métricas, foi igualmente analisada a estabilidade dos modelos, através da comparação dos resultados obtidos nos conjuntos de treino e teste. Um modelo robusto deverá apresentar métricas consistentes entre ambos, indicando capacidade de generalização.

Desta forma, a seleção do modelo final baseou-se numa abordagem multi-critério, combinando desempenho quantitativo, robustez estatística e interpretabilidade dos resultados no contexto do problema de negócio, nomeadamente a identificação de padrões associados ao `Attrition` de colaboradores.
 
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
