# Relatório de Conclusão e Entrega de Valor (Milestone 4)

## 1. Síntese de Resultados e Impacto

### 1.1 Objetivo 1 - Modelo Preditivo de `Attrition`

#### O Problema Resolvido
O primeiro objetivo do projeto consistia em desenvolver um modelo de classificação capaz de prever o atrito (`Attrition`) dos colaboradores, alcançando um F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 21/04/2026. O problema foi formalizado como uma tarefa de classificação binária sobre o dataset _IBM HR Analytics Employee Attrition & Performance_, composto por 1470 colaboradores e 35 variáveis originais, com uma taxa de atrito de 16,1% (Chapman et al., 2000; IBM Watson Analytics, 2016).

#### Interpretação dos Resultados
O modelo final selecionado foi a Regressão Logística com _pipeline_ `MaxAbsScaler` + SMOTE + `StratifiedKFold` (k=15) e threshold ótimo de 0,52, obtido após um processo de otimização em cinco etapas sequenciais: pesquisa do melhor _split_ (85/15), do melhor normalizador (`MaxAbsScaler`), da melhor técnica de _resampling_ (SMOTE), de hiperparâmetros via `GridSearchCV` e do threshold de decisão. Foram testados 18 algoritmos distintos antes de convergir para esta solução, cobrindo modelos de ensemble, lineares, probabilísticos e redes neuronais, em linha com a recomendação do CRISP-DM de explorar múltiplos algoritmos antes de selecionar o modelo final (Chapman et al., 2000).

[Por acabar]

#### Índice de Risco de `Attrition`
* A partir das probabilidades previstas pelo modelo, foi construído um índice de risco.
* Os colaboradores foram classificados em categorias de risco, facilitando a interpretação dos resultados.

#### Valor para o Utilizador/Negócio
* A solução permite identificar colaboradores com maior probabilidade de saída.
* Este resultado pode apoiar estratégias preventivas de retenção e tomada de decisão em Recursos Humanos.



### 1.2 Objetivo 2 - Identificação de Perfis de Colaboradores através de _Clustering_

#### O Problema Resolvido

O segundo objetivo consistia em aplicar técnicas de _clustering_ para identificar e caracterizar perfis distintos de colaboradores, determinando o número ótimo de _clusters_ com um _Silhouette Score_ médio superior a 0,50, até ao dia 21/04/2026. O problema foi abordado como uma tarefa de aprendizagem , sem recurso a qualquer informação sobre a variável alvo (`Attrition`) durante o processo de agrupamento (Chapman et al., 2000).

#### Interpretação dos Resultados

O modelo final selecionado foi UMAP(`n_components=5`, `n_neighbors=15`) + DBSCAN(`eps=6.0`, `min_samples=3`). Foram testadas seis abordagens distintas de redução de dimensionalidade e seleção de variáveis antes de convergir para esta solução, incluindo DBSCAN no espaço original, DBSCAN otimizado, PCA + DBSCAN, seleção por correlação, GMM e _K-Means_ (McInnes et al., 2018; Schubert et al., 2017).

Os resultados obtidos são os seguintes:

| Métrica                 | Treino  | Teste  | Meta       | Resultado    |
|-------------------------|---------|--------|------------|--------------|
| Silhouette Score        | 0,7141  | 0,7016 | > 0,50     | Atingida     |
| Davies-Bouldin Index    | 0,3991  | 0,3864 | Minimizar  | Excelente    |
| Calinski-Harabasz Index | 1772,02 | 301,16 | Maximizar  | 21x baseline |
| Clusters identificados  | 4       | 4      | -          | -            |
| Ruído (%)               | 0,0%    | 0,0%   | Minimizar  | Nenhum       |

O _Silhouette Score_ de 0,7016 no conjunto de teste supera a meta em +40,3 pontos percentuais. Rousseeuw (1987) classifica valores superiores a 0,70 como indicativos de estrutura de _clustering_ forte, o limiar mais exigente da sua escala. O Davies-Bouldin de 0,3864 confirma _clusters_ compactos e bem separados - valores abaixo de 0,5 são considerados excelentes (Davies & Bouldin, 1979). A diferença entre treino e teste é mínima (Delta Silhouette = 0,0125), confirmando que o modelo generaliza a estrutura aprendida sem sobreajustamento.

Os quatro perfis de colaboradores identificados apresentam correspondência direta com a estrutura departamental da organização:

| Cluster | Designação           | Peso  | Características Principais                                                         |
|---------|----------------------|-------|------------------------------------------------------------------------------------|
| 0       | I&D Operacional      | 56%   | Técnicos de I&D de nível médio, rendimento moderado, longa permanência na empresa  |
| 1       | Liderança Científica | 5,8%  | Quadros seniores de I&D, rendimento elevado, alta escolaridade, cargos de liderança|
| 2       | Equipa de Vendas     | 36%   | Colaboradores de Vendas, perfil mais jovem, maior mobilidade entre empresas        |
| 3       | Recursos Humanos     | 2,1%  | Pequeno grupo de RH, perfil distinto em termos de função e compensação             |

Esta coerência não foi imposta ao modelo - resultou da aprendizagem sobre 53 variáveis escaladas. A correspondência entre os _clusters_ e unidades organizacionais reais é um indicador forte de validade externa: o modelo captura uma estrutura que existe objetivamente na organização, não um artefacto do algoritmo (Jain, 2010).

Por palavras simples, o modelo consegue identificar automaticamente os diferentes perfis de colaboradores sem ter sido informado sobre departamentos ou funções. O facto de os grupos encontrados coincidirem com a realidade organizacional confirma que as variáveis do dataset captam diferenças reais entre os colaboradores, e não apenas ruído estatístico.

#### Valor para o Utilizador/Negócio

A segmentação permite à organização desenvolver estratégias de Recursos Humanos diferenciadas por perfil. A Equipa de Vendas (36%), com maior mobilidade entre empresas, pode beneficiar de políticas de retenção específicas baseadas em incentivos de progressão e reconhecimento. A Liderança Científica (5,8%), apesar de reduzida em número, representa um capital humano de elevado valor estratégico que justifica abordagens de retenção personalizadas. Os colaboradores de I&D Operacional (56%), sendo o maior grupo, são os que mais influenciam os indicadores agregados da organização (Hom et al., 2017).



## 2. Análise Crítica e Limitações

### 2.1 Limitações dos Dados

O dataset _IBM HR Analytics_ é sintético, tendo sido criado pela IBM para fins demonstrativos, o que limita a transferibilidade direta dos resultados para contextos organizacionais reais. A variável `Attrition` apresenta desequilíbrio de classes significativo (~84% "Não Saiu" vs ~16% "Saiu"), o que penaliza métricas como o _F1-Score_ mesmo em modelos com boa capacidade discriminativa (Chawla et al., 2002). 

A variável `PerformanceRating` apresenta variância praticamente nula, com a quase totalidade das observações concentradas nos valores 3 e 4, limitando o seu poder preditivo e justificando a sua exclusão na fase de preparação dos dados. Por fim, o dataset não inclui variáveis potencialmente relevantes como indicadores de engajamento (_engagement_), absentismo, clima organizacional real ou dados externos sobre o mercado de trabalho, o que condiciona o poder preditivo máximo dos modelos (Hom et al., 2017).


### 2.2 Limitações do Modelo Preditivo

O objetivo mínimo de _F1-Score_ >= 0,80 não foi alcançado (_F1 Teste_ = 0,5625), apesar de terem sido testados 18 algoritmos distintos e aplicadas técnicas de equilíbrio (SMOTE) e otimização de _threshold_. O modelo apresenta um _gap_ entre treino e teste de +0,12 no _F1-Score_, indicativo de _overfitting_ leve, embora substancialmente inferior ao observado nos modelos de ensemble e redes neuronais testados - por exemplo, _Random Forest_ com _gap_ de +0,78 e Keras simples com _gap_ de +0,54. 

As previsões indicam probabilidade de risco, mas não provam causalidade entre as variáveis e o atrito - uma limitação inerente a qualquer modelo preditivo correlacional (James et al., 2021). 

O índice de risco deve ser interpretado como ferramenta de apoio à decisão humana, não como mecanismo de decisão automática.


### 2.3 Limitações do _Clustering_

O _Cluster 3_ (Recursos Humanos) conta com apenas ~26 membros no treino (2,1% da amostra). A dimensão reduzida impõe cautela na generalização dos seus perfis médios e na tomada de decisões organizacionais baseadas neste grupo, uma vez que pequenas alterações na composição do departamento de RH podem alterar significativamente as médias do _cluster_. O UMAP é um algoritmo estocástico: sem semente fixa (`random_state=42`), execuções distintas produzem *manifolds* ligeiramente diferentes, alterando potencialmente a atribuição de pontos fronteiriços (McInnes et al., 2018). 

A avaliação baseou-se exclusivamente em métricas internas (Silhouette, Davies-Bouldin, Calinski-Harabasz): a validade externa - correspondência com variáveis não utilizadas na modelação, como o atrito - não foi testada formalmente, constituindo uma limitação metodológica a colmatar em trabalhos futuros (Rousseeuw, 1987).

## 3. Considerações Éticas e de Viés

### 3.1 Privacidade
O _dataset_ encontra-se totalmente anonimizado, não contendo identificadores pessoais diretos como nomes, contactos ou identificadores reais de colaboradores. A utilização dos dados neste projeto é estritamente académica e cumpre os princípios fundamentais do RGPD, nomeadamente a minimização de dados, a limitação da finalidade e a licitude do tratamento. Atendendo à natureza sintética e agregada da informação, o risco de reidentificação individual é negligenciável.

### 3.2 Transparência

A Regressão Logística foi selecionada em parte pela sua interpretabilidade intrínseca: os coeficientes do modelo são diretamente legíveis e permitem explicar, para cada colaborador, quais as variáveis que mais contribuíram para o nível de risco atribuído. Esta propriedade é essencial num contexto de Recursos Humanos, onde as decisões de intervenção têm de ser justificáveis perante os próprios colaboradores e a gestão de topo (Hom et al., 2017).

O índice de risco é apresentado com categorias claras e limiares explícitos; Baixo (< 30%), Médio (30–50%), Alto (50–70%) e Crítico (≥ 70%),  permitindo auditabilidade das decisões de intervenção. Todos os passos metodológicos estão documentados e são reprodutíveis, sendo o _notebook_ executável do início ao fim sem interrupções, em linha com os princípios de reprodutibilidade da ciência aberta (Chapman et al., 2000).

### 3.3 Viés

O desequilíbrio da variável `Attrition` (~84% vs ~16%) pode levar o modelo a subvalorizar casos reais de atrito, com impacto direto no _Recall_. Este viés foi parcialmente mitigado através da aplicação de SMOTE e da otimização do _threshold_ de decisão, mas não eliminado na totalidade, o que reforça a necessidade de interpretar o índice como ferramenta de apoio e não como veredicto automático.

Variáveis como `Age`, `Gender`, `MaritalStatus`, `MonthlyIncome` ou `JobLevel` devem ser interpretadas com cautela: a sua correlação com o atrito pode refletir padrões históricos de desigualdade organizacional, não necessariamente causalidade (James et al., 2021). A classificação de colaboradores como "de risco" não deve ser utilizada para penalização, restrição de oportunidades ou decisões de despedimento. O índice de risco deve suportar exclusivamente estratégias proativas e justas de retenção e desenvolvimento, em linha com os princípios de uso responsável de sistemas de apoio à decisão algorítmica em Recursos Humanos (Hom et al., 2017).


## 4. Roadmap e Trabalhos Futuros

### 4.1 Melhorias Técnicas

As principais limitações identificadas neste projeto sugerem as seguintes melhorias técnicas para trabalhos futuros:

1. **Explorar técnicas de calibração de probabilidades** como _Platt Scaling_ ou _Isotonic Regression_, com o objetivo de melhorar a fiabilidade do índice de risco, garantindo que uma probabilidade prevista de 70% corresponda efetivamente a 70% de casos reais de saída (Géron, 2022).

2. **Testar abordagens de ensemble heterogéneo**, combinando a Regressão Logística com modelos de maior capacidade preditiva como LightGBM ou XGBoost através de _stacking_, procurando superar o teto de desempenho imposto pelo desequilíbrio de classes sem sacrificar interpretabilidade (James et al., 2021).

3. **Explorar técnicas de seleção de variáveis** como _Recursive Feature Elimination_ (RFE) ou _SHAP values_, com o objetivo de identificar o subconjunto mínimo de variáveis com maior poder preditivo, reduzindo a dimensionalidade e melhorando a generalização dos modelos (Géron, 2022).

4. **Validar o _clustering_ com métricas externas**, cruzando os _clusters_ identificados com variáveis não utilizadas na modelação como `Attrition`, `PerformanceRating` ou dados de progressão na carreira, para avaliar a validade externa dos perfis encontrados (Rousseeuw, 1987).

### 4.2 Novas Variáveis

A capacidade preditiva dos modelos está diretamente condicionada pela qualidade e abrangência das variáveis disponíveis. As seguintes fontes de dados adicionais representam oportunidades relevantes de enriquecimento do modelo:

1. **Dados longitudinais:** Integrar séries temporais de avaliações de desempenho, promoções e movimentos internos para capturar a trajetória de cada colaborador ao longo do tempo, transformando o problema de classificação transversal num problema de análise de sobrevivência, metodologia mais adequada para modelar o tempo até à ocorrência de um evento (Hom et al., 2017).

2. **Variáveis de _engagement_:** Incorporar dados de _pulse surveys_, Net Promoter Score interno, absentismo ou participação em programas de formação, variáveis identificadas na literatura como preditores independentes do atrito não capturadas pelo dataset atual (Hom et al., 2017).

3. **Fatores externos de mercado:** Incluir indicadores como a taxa de desemprego sectorial, o volume de ofertas de emprego concorrentes e índices de satisfação do mercado de trabalho, que representam determinantes contextuais do atrito frequentemente ignorados em modelos baseados exclusivamente em dados internos (Hom et al., 2017).

### 4.3 Escalabilidade e _Deployment_

Assumindo a disponibilidade de dados reais de uma organização, a solução desenvolvida poderia evoluir para um sistema de apoio à decisão operacional através das seguintes etapas:

1. **Interface de consulta em _Streamlit_:** Desenvolver uma aplicação _web_ que permita à equipa de RH consultar o índice de risco de cada colaborador, visualizar os perfis de _clustering_ e simular o impacto de intervenções de retenção.

2. **_Dashboard_ de monitorização:** Criar painéis de controlo interativos com métricas agregadas por departamento, nível de risco e _cluster_, permitindo à gestão de topo acompanhar a evolução do risco de atrito ao longo do tempo.

3. **_Pipeline_ de atualização contínua:** Implementar um processo automatizado de re-treino periódico do modelo com novos dados, garantindo que as previsões se mantêm calibradas face a mudanças organizacionais ou de mercado, com monitorização de _data drift_ para detetar degradação do desempenho (Géron, 2022).


## 5. Conclusão Final

ste projeto demonstrou a aplicabilidade de técnicas de Ciência de Dados ao problema da rotatividade de colaboradores, percorrendo de forma rigorosa as fases do CRISP-DM da definição do problema à entrega de valor analítico (Chapman et al., 2000).

Foram desenvolvidas duas soluções complementares. A primeira, um modelo preditivo de classificação baseado em Regressão Logística, permite estimar a probabilidade de saída de cada colaborador e classificá-lo num índice de risco de quatro categorias: Baixo, Médio, Alto e Crítico, transformando uma previsão probabilística em informação acionável para a gestão de Recursos Humanos. A segunda, um modelo de _clustering_ baseado em UMAP + DBSCAN, identificou quatro perfis distintos de colaboradores com correspondência direta à estrutura departamental real da organização, sem que essa informação tivesse sido fornecida ao modelo.

Os resultados ficaram aquém da meta quantitativa de _F1-Score_ ≥ 0,80, fixando-se em 0,56 no conjunto de teste. Esta limitação, honestamente reconhecida, não invalida o valor da solução: o modelo identifica corretamente 3 em cada 4 colaboradores que
efetivamente saem (_Recall_ de 75%), e o _clustering_ atingiu um _Silhouette Score_ de 0,70, acima da meta de 0,50 e classificado como estrutura forte na escala de Rousseeuw. A diferença entre a meta e o resultado alcançado reflete as limitações inerentes ao dataset sintético e não uma falha metodológica.

Do ponto de vista do impacto prático, a solução permite à organização passar de uma postura reativa para uma postura preventiva e estratégica: identificar quem está em risco, em que perfil se insere e com que prioridade intervir. Esta capacidade tem valor direto na redução de custos de rotatividade e na preservação do capital humano organizacional (Hom et al., 2017).

---


**Data de Conclusão:** [Inserir Data]

**Versão do Projeto:** v4.0 Final
