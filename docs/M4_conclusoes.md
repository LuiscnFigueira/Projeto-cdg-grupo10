# Relatório de Conclusão e Entrega de Valor (Milestone 4)

## 1. Síntese de Resultados e Impacto

### 1.1 Objetivo 1 - Modelo Preditivo de Attrition

#### O Problema Resolvido
O primeiro objetivo do projeto consistia em desenvolver um modelo de classificação supervisionado capaz de prever o atrito (`Attrition`) dos colaboradores, alcançando um F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 21/04/2026. O problema foi formalizado como uma tarefa de classificação binária sobre o dataset IBM HR Analytics Employee Attrition & Performance, composto por 1470 colaboradores e 35 variáveis originais, com uma taxa de atrito de 16,1% (Chapman et al., 2000; IBM Watson Analytics, 2016).

#### Interpretação dos Resultados
O modelo final selecionado foi a Regressão Logística com pipeline `MaxAbsScaler` + SMOTE + `StratifiedKFold` (k=15) e threshold ótimo de 0,52, obtido após um processo de otimização em cinco etapas sequenciais: pesquisa do melhor split (85/15), do melhor normalizador (`MaxAbsScaler`), da melhor técnica de resampling (SMOTE), de hiperparâmetros via `GridSearchCV` e do threshold de decisão. Foram testados 18 algoritmos distintos antes de convergir para esta solução, cobrindo modelos de ensemble, lineares, probabilísticos e redes neuronais, em linha com a recomendação do CRISP-DM de explorar múltiplos algoritmos antes de selecionar o modelo final (Chapman et al., 2000).

[Por acabar]

#### Índice de Risco de Attrition
* A partir das probabilidades previstas pelo modelo, foi construído um índice de risco.
* Os colaboradores foram classificados em categorias de risco, facilitando a interpretação dos resultados.

#### Valor para o Utilizador/Negócio
* A solução permite identificar colaboradores com maior probabilidade de saída.
* Este resultado pode apoiar estratégias preventivas de retenção e tomada de decisão em Recursos Humanos.



### 1.2 Objetivo 2 - Identificação de Perfis de Colaboradores através de Clustering

#### O Problema Resolvido

O segundo objetivo consistia em aplicar técnicas de clustering não supervisionado para identificar e caracterizar perfis distintos de colaboradores, determinando o número ótimo de clusters com um Silhouette Score médio superior a 0,50, até ao dia 21/04/2026. O problema foi abordado como uma tarefa de aprendizagem não supervisionada, sem recurso a qualquer informação sobre a variável alvo `Attrition` durante o processo de agrupamento (Chapman et al., 2000).

#### Interpretação dos Resultados

O modelo final selecionado foi UMAP(`n_components=5`, `n_neighbors=15`) + DBSCAN(`eps=6.0`, `min_samples=3`). Foram testadas seis abordagens distintas de redução de dimensionalidade e seleção de variáveis antes de convergir para esta solução, incluindo DBSCAN no espaço original, DBSCAN otimizado, PCA + DBSCAN, seleção por correlação, GMM e K-Means (McInnes et al., 2018; Schubert et al., 2017).

Os resultados obtidos são os seguintes:

| Métrica                 | Treino  | Teste  | Meta       | Resultado    |
|-------------------------|---------|--------|------------|--------------|
| Silhouette Score        | 0,7141  | 0,7016 | > 0,50     | Atingida     |
| Davies-Bouldin Index    | 0,3991  | 0,3864 | Minimizar  | Excelente    |
| Calinski-Harabasz Index | 1772,02 | 301,16 | Maximizar  | 21x baseline |
| Clusters identificados  | 4       | 4      | -          | -            |
| Ruído (%)               | 0,0%    | 0,0%   | Minimizar  | Nenhum       |

O Silhouette Score de 0,7016 no conjunto de teste supera a meta em +40,3 pontos percentuais. Rousseeuw (1987) classifica valores superiores a 0,70 como indicativos de estrutura de clustering forte, o limiar mais exigente da sua escala. O Davies-Bouldin de 0,3864 confirma clusters compactos e bem separados - valores abaixo de 0,5 são considerados excelentes (Davies & Bouldin, 1979). A diferença entre treino e teste é mínima (Delta Silhouette = 0,0125), confirmando que o modelo generaliza a estrutura aprendida sem sobreajustamento.

Os quatro perfis de colaboradores identificados apresentam correspondência direta com a estrutura departamental da organização:

| Cluster | Designação           | Peso  | Características Principais                                                         |
|---------|----------------------|-------|------------------------------------------------------------------------------------|
| 0       | I&D Operacional      | 56%   | Técnicos de I&D de nível médio, rendimento moderado, longa permanência na empresa  |
| 1       | Liderança Científica | 5,8%  | Quadros seniores de I&D, rendimento elevado, alta escolaridade, cargos de liderança|
| 2       | Equipa de Vendas     | 36%   | Colaboradores de Vendas, perfil mais jovem, maior mobilidade entre empresas        |
| 3       | Recursos Humanos     | 2,1%  | Pequeno grupo de RH, perfil distinto em termos de função e compensação             |

Esta coerência não foi imposta ao modelo - resultou da aprendizagem não supervisionada sobre 53 variáveis escaladas. A correspondência entre os clusters e unidades organizacionais reais é um indicador forte de validade externa: o modelo captura uma estrutura que existe objetivamente na organização, não um artefacto do algoritmo (Jain, 2010).

Por palavras simples, o modelo consegue identificar automaticamente os diferentes perfis de colaboradores sem ter sido informado sobre departamentos ou funções. O facto de os grupos encontrados coincidirem com a realidade organizacional confirma que as variáveis do dataset captam diferenças reais entre os colaboradores, e não apenas ruído estatístico.

#### Valor para o Utilizador/Negócio

A segmentação permite à organização desenvolver estratégias de Recursos Humanos diferenciadas por perfil. A Equipa de Vendas (36%), com maior mobilidade entre empresas, pode beneficiar de políticas de retenção específicas baseadas em incentivos de progressão e reconhecimento. A Liderança Científica (5,8%), apesar de reduzida em número, representa um capital humano de elevado valor estratégico que justifica abordagens de retenção personalizadas. Os colaboradores de I&D Operacional (56%), sendo o maior grupo, são os que mais influenciam os indicadores agregados da organização (Hom et al., 2017).



## 2. Análise Crítica e Limitações

### 2.1 Limitações dos Dados

O dataset IBM HR Analytics é sintético, tendo sido criado pela IBM para fins demonstrativos, o que limita a transferibilidade direta dos resultados para contextos organizacionais reais. A variável `Attrition` apresenta desequilíbrio de classes significativo (~84% "Não Saiu" vs ~16% "Saiu"), o que penaliza métricas como o F1-Score mesmo em modelos com boa capacidade discriminativa (Chawla et al., 2002). 

A variável `PerformanceRating` apresenta variância praticamente nula, com a quase totalidade das observações concentradas nos valores 3 e 4, limitando o seu poder preditivo e justificando a sua exclusão na fase de preparação dos dados. Por fim, o dataset não inclui variáveis potencialmente relevantes como indicadores de engagement, absentismo, clima organizacional real ou dados externos sobre o mercado de trabalho, o que condiciona o poder preditivo máximo dos modelos (Hom et al., 2017).


### 2.2 Limitações do Modelo Preditivo

O objetivo mínimo de F1-Score >= 0,80 não foi alcançado (F1 Teste = 0,5625), apesar de terem sido testados 18 algoritmos distintos e aplicadas técnicas de balanceamento (SMOTE) e otimização de threshold. O modelo apresenta um gap entre treino e teste de +0,12 no F1-Score, indicativo de overfitting leve, embora substancialmente inferior ao observado nos modelos de ensemble e redes neuronais testados - por exemplo, Random Forest com gap de +0,78 e Keras simples com gap de +0,54. 

As previsões indicam probabilidade de risco, mas não provam causalidade entre as variáveis e o atrito - uma limitação inerente a qualquer modelo preditivo correlacional (James et al., 2021). 

O índice de risco deve ser interpretado como ferramenta de apoio à decisão humana, não como mecanismo de decisão automática.


### 2.3 Limitações do Clustering

O Cluster 3 (Recursos Humanos) conta com apenas ~26 membros no treino (2,1% da amostra). A dimensão reduzida impõe cautela na generalização dos seus perfis médios e na tomada de decisões organizacionais baseadas neste grupo, uma vez que pequenas alterações na composição do departamento de RH podem alterar significativamente as médias do cluster. O UMAP é um algoritmo estocástico: sem semente fixa (`random_state=42`), execuções distintas produzem *manifolds* ligeiramente diferentes, alterando potencialmente a atribuição de pontos fronteiriços (McInnes et al., 2018). 

A avaliação baseou-se exclusivamente em métricas internas (Silhouette, Davies-Bouldin, Calinski-Harabasz): a validade externa - correspondência com variáveis não utilizadas na modelação, como o atrito - não foi testada formalmente, constituindo uma limitação metodológica a colmatar em trabalhos futuros (Rousseeuw, 1987).

## 3. Considerações Éticas e de Viés

### 3.1 Privacidade
* O dataset encontra-se anonimizado e não contém identificadores pessoais diretos.
* A utilização dos dados é exclusivamente académica.

### 3.2 Transparência

A Regressão Logística foi selecionada em parte pela sua interpretabilidade intrínseca: os coeficientes do modelo são diretamente legíveis e permitem explicar, para cada colaborador, quais as variáveis que mais contribuíram para o nível de risco atribuído. Esta propriedade é essencial num contexto de Recursos Humanos, onde as decisões de intervenção têm de ser justificáveis perante os próprios colaboradores e a gestão de topo.

O índice de risco é apresentado com categorias claras e limiares explícitos, permitindo auditabilidade das decisões de intervenção. Todos os passos metodológicos estão documentados e são reprodutíveis, sendo o notebook executável do início ao fim sem interrupções, em linha com os princípios de reprodutibilidade da ciência aberta (Chapman et al., 2000).

### 3.3 Viés

O desequilíbrio da variável `Attrition` pode levar o modelo a subvalorizar casos reais de atrito (classe minoritária), com impacto direto no Recall. Variáveis como `Age`, `Gender`, `MaritalStatus`, `MonthlyIncome` ou `JobLevel` devem ser interpretadas com cautela: a sua correlação com o atrito pode refletir padrões históricos de desigualdade organizacional, não necessariamente causalidade. A classificação de colaboradores como "de risco" não deve ser utilizada para penalização, restrição de oportunidades ou decisões de despedimento. O índice de risco deve suportar estratégias proativas e justas de retenção e desenvolvimento, em linha com os princípios de uso responsável de sistemas de apoio à decisão em Recursos Humanos (Hom et al., 2017).


## 4. Roadmap e Trabalhos Futuros

### 4.1 Melhorias Técnicas
1. Testar modelos adicionais e técnicas de otimização de hiperparâmetros.
2. Avaliar técnicas de balanceamento, como SMOTE.
3. Melhorar a calibração das probabilidades do modelo preditivo.
4. Testar outros algoritmos de clustering além do K-Means.

### 4.2 Novas Variáveis

1. Dados longitudinais: Integrar séries temporais de avaliações de desempenho, promoções e movimentos internos para capturar a trajetória de cada colaborador ao longo do tempo, transformando o problema de classificação transversal num problema de análise de sobrevivência (Hom et al., 2017).

2. Variáveis de engagement: Incorporar dados de pulse surveys, Net Promoter Score interno, absentismo ou participação em programas de formação, variáveis identificadas na literatura como preditores independentes do atrito (Hom et al., 2017).

3. Fatores externos: Incluir indicadores de mercado de trabalho (taxa de desemprego sectorial, volume de ofertas de emprego concorrentes) e clima organizacional, que representam determinantes contextuais do atrito não captados pelo dataset atual.

### 4.3 Escalabilidade e Deployment
1. Criar uma interface em Streamlit para consulta dos resultados.
2. Desenvolver dashboards para Recursos Humanos com métricas de risco e perfis de colaboradores.
3. Implementar monitorização contínua do desempenho do modelo.



## 5. Conclusão Final

* O projeto permitiu aplicar técnicas de Ciência de Dados ao problema da rotatividade de colaboradores.
* Foram trabalhadas duas dimensões principais: previsão de `Attrition` e identificação de perfis através de clustering.
* O índice de risco funciona como uma extensão interpretável do modelo preditivo.
* Apesar das limitações do dataset, a solução demonstra potencial para apoiar decisões estratégicas em Recursos Humanos.

---


**Data de Conclusão:** [Inserir Data]

**Versão do Projeto:** v4.0 Final
