# People Analytics and Employee Turnover Prediction
## Identificação da Equipa
* **Grupo nº:** 10
* **Membros:**
* Luís Figueira - 2022160309 (a2022160309@alumni.iscac.pt)
* Martim Ferreira - 2023132459 (a2023132459@alumni.iscac.pt)
* Mateus Afonso - 2023142275 (a2023142275@alumni.iscac.pt)
## Organização do Repositório
A estrutura deste projeto segue as boas práticas de Ciência de Dados e Engenharia de Software:
* **`data/`**: Armazenamento de dados (dados brutos em `raw/` e processados em `processed/`).
* **`docs/`**: Documentação técnica detalhada dividida por Milestones (M1, M2, M3 e M4).
* **`notebooks/`**: Jupyter Notebooks para experimentação, limpeza e modelação.
* **`reports/`**: Relatórios finais, apresentações e exportação de figuras (`figures/`).
* **`src/`**: Código-fonte modular (scripts `.py`) para funções reutilizáveis.
* **`requirements.txt`**: Ficheiro de configuração com as bibliotecas necessárias.
## 1. Iniciação (Milestone 1)

Este projeto aplica metodologias de Ciência de Dados ao problema da rotatividade de colaboradores (Employee Attrition) no contexto de HR Analytics. A retenção de talento constitui um desafio estratégico para organizações modernas, com impacto direto nos custos operacionais, produtividade e sustentabilidade organizacional.

Com base no dataset IBM HR Analytics Employee Attrition & Performance (1470 colaboradores, 35 variáveis), o objetivo consiste em analisar os fatores associados ao atrito (`Attrition`) e desenvolver modelos preditivos capazes de estimar o risco de saída de colaboradores.

O projeto segue a metodologia CRISP-DM e encontra-se estruturado em quatro milestones: iniciação, exploração, modelação e finalização.

### Objetivos do Projeto (SMART)
* **Objetivo 1:** Desenvolver um modelo de classificação supervisionado para prever o atrito (`Attrition`), alcançando um F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 23/04/2026 (Milestone 3).
* **Objetivo 2:** Aplicar técnicas de clustering não supervisionado para identificar e caracterizar perfis distintos de colaboradores com base nas variáveis relevantes do dataset, determinando o número ótimo de agrupamentos (clusters) através do método do cotovelo e do Silhouette Score, garantindo um valor médio de Silhouette superior a 0,50, e descrevendo estatisticamente cada perfil identificado, até ao dia 23/04/2026.
### Perguntas de Investigação

As perguntas de investigação estruturam o enquadramento científico do estudo, orientando a análise empírica dos dados com o objetivo de identificar os principais determinantes do atrito (`Attrition`), avaliar a sua relevância estatística e preditiva e verificar a viabilidade de desenvolver modelos capazes de prever este fenómeno de forma fiável.

**1.** Quais são as variáveis com maior poder explicativo e preditivo do atrito (`Attrition`) dos colaboradores?

**2.** Existe uma associação estatisticamente significativa entre a realização de horas extraordinárias (`OverTime`) e a probabilidade de atrito (`Attrition`)?

**3.** O nível de satisfação no trabalho (`JobSatisfaction`) e o equilíbrio entre vida pessoal e profissional (`WorkLifeBalance`) influenciam significativamente o risco de atrito (`Attrition`)?

**4.** O rendimento mensal (`MonthlyIncome`) tem impacto significativo na probabilidade de atrito (`Attrition`), mesmo após controlo multivariável?

**5.** Qual dos algoritmos de classificação testados apresenta melhor desempenho e maior estabilidade na previsão do atrito (`Attrition`)?

**6.** O desequilíbrio da variável alvo influencia o desempenho dos modelos preditivos e pode ser mitigado através da aplicação da técnica SMOTE?

**7.** É possível construir um índice de risco de atrito (`Attrition`) interpretável e fiável, baseado nas probabilidades previstas pelo modelo, que permita classificar os colaboradores em quatro categorias de risco (Baixo: prob < 30%, Médio: 30% ≤ prob < 50%, Alto: 50% ≤ prob < 70% e Crítico: prob ≥ 70%) e apoiar a tomada de decisão em contexto organizacional?

**8.** Que fatores distinguem os colaboradores com maior risco de atrito (`Attrition`) dos restantes, e como podem ser utilizados para apoiar estratégias de retenção?

## 2. Exploração (Milestone 2)

### Limpeza e Preparação

Foi realizada uma análise da qualidade dos dados com o objetivo de garantir a consistência e fiabilidade do dataset antes da modelação.

* **Valores em falta:** Não foram identificados valores nulos, não sendo necessária a aplicação de técnicas de imputação ou remoção de observações.
* **Outliers:** Foram detetados valores extremos em várias variáveis numéricas através do método IQR. No entanto, estes valores foram considerados plausíveis no contexto organizacional, tendo-se optado por não remover observações, de forma a preservar a representatividade dos dados.
* **Consistência dos dados:** Foi validada a coerência entre variáveis relacionadas com experiência profissional e idade, não tendo sido identificadas inconsistências lógicas. A estrutura salarial revelou-se consistente com os níveis hierárquicos.
* **Encoding:** Codificação binária (0/1) aplicada a `Attrition`, `OverTime` e `Gender`; One-Hot Encoding aplicado a `BusinessTravel`, `Department`, `EducationField`, `JobRole` e `MaritalStatus`.
* **Escalonamento:** Adiado para a Milestone 3, onde será integrado nas pipelines de cada modelo para evitar *data leakage*.

De forma geral, o dataset apresenta boa qualidade e consistência, permitindo avançar para as etapas de transformação e modelação sem necessidade de intervenções corretivas significativas.

> Detalhes completos em [`docs/M2_exploracao.md`](docs/M2_exploracao.md)

---

### Principais Conclusões (EDA)

<p align="center">
  <img src="reports/figures/CorrelaçãoVariávelAlvo.png" alt="Correlação com Variável Alvo" width="700"/>
</p>

O gráfico apresenta a correlação entre diversas variáveis e a variável alvo `Attrition` (saída de colaboradores).
À esquerda, o mapa de calor mostra a intensidade e direção das correlações (valores entre -1 e 1), onde tons azuis indicam correlação negativa e tons vermelhos indicam correlação positiva. À direita, o gráfico de barras destaca as variáveis com maior impacto na rotatividade.

Observa-se que OverTime (horas extra) apresenta a correlação positiva mais forte com o `Attrition`, indicando que colaboradores que fazem mais horas extra têm maior probabilidade de sair da empresa. Variáveis como JobLevel, TotalWorkingYears, MonthlyIncome e Age apresentam correlações negativas, sugerindo que maior experiência, senioridade e rendimento estão associados a menor rotatividade.

Outras variáveis, como JobRole (Sales Representative) e BusinessTravel (Travel Frequently), também demonstram associação positiva com o Attrition, enquanto fatores relacionados com satisfação (ex.: JobSatisfaction e EnvironmentSatisfaction) tendem a reduzir a probabilidade de saída.

* **Ponto-chave 1 - Horas Extraordinárias:** Colaboradores que realizam horas extra apresentam uma taxa de saída de 30.5%, face a 10.4% nos restantes. O teste qui-quadrado confirmou esta associação (χ² = 87.56, p < 0.001, Cramér's V = 0.24).
* **Ponto-chave 2 - Perfil de risco:** Os colaboradores com maior probabilidade de abandono tendem a ser jovens (20–35 anos), com baixo rendimento mensal e menos de 10 anos de experiência total.
* **Ponto-chave 3 - Desequilíbrio de classes:** A variável alvo `Attrition` apresenta *class imbalance* significativo: 83.9% (No) vs 16.1% (Yes). A Milestone 3 adotará métricas adequadas (F1-Score, ROC-AUC) e técnicas de balanceamento (SMOTE).
* **Ponto-chave 4 - Engenharia de atributos:** Criadas 4 novas variáveis - `SatisfactionIndex`, `RatioYearsInRole`, `IncomePerLevel` e `CareerStagnation`.

## 3. Modelação (Milestone 3)

### Abordagem Técnica

A fase de modelação cobre dois objetivos: classificação supervisionada do atrito (Objetivo 1 e 2) e segmentação não supervisionada de colaboradores (Objetivo 3).

**Objetivo 1 - Classificação Supervisionada**
- **Modelos testados:** 18 algoritmos (ensemble, lineares, redes neuronais) + baseline (Árvore de Decisão)
- **Modelo final:** Regressão Logística com pipeline `StandardScaler` + SMOTE + `StratifiedKFold` (k=15) + threshold ótimo
- **Métrica principal:** F1-Score
- **Resultado:** F1 Teste = 0.56 | AUC-ROC = 0.8308

**Objetivo 2 - Clustering Não Supervisionado**
- **Modelos testados:** K-Means, DBSCAN, GMM, Agglomerative Clustering, OPTICS, MiniBatch K-Means
- **Modelo final:** UMAP(`n_components=5`, `n_neighbors=15`) + DBSCAN(`eps=6.0`, `min_samples=3`)
- **Métrica principal:** Silhouette Score 
- **Resultado:** Silhouette Teste = 0.702 | 4 clusters identificados (I&D Operacional, Liderança Científica, Equipa de Vendas, Recursos Humanos)

> Detalhes completos em [`docs/M3_modelacao_O1.md`](docs/M3_modelacao_O1.md) e [`docs/M3_modelacao_O2.md`](docs/M3_modelacao_O2.md)

## 4. Finalização (Milestone 4)
### Resposta ao Problema
[Resumo da solução e como ela gera valor para o negócio.]
### Recomendações de Inovação
1. [Sugestão prática baseada nos resultados]
## Como Reproduzir este Projeto
1. Clone o repositório: `git clone https://github.com/LuiscnFigueira/Projeto-cdg-grupo10`
2. Instale as dependências: `pip install -r requirements.txt`
3. Execute os notebooks na pasta `notebooks/` seguindo a ordem numérica.
   
**Instituição:** Coimbra Business School | ISCAC

**Curso:** Licenciatura em Ciência de Dados para a Gestão

**Unidade Curricular:** Projeto em Ciência de Dados

**Professor Responsável:** Dora Melo (dmelo@iscac.pt)


## Fonte de Dados

* **Dataset:** https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset  
* **Dimensão:** 1470 linhas, 35 colunas  
* **Fonte do Código:** https://www.kaggle.com/lusfigueira/code 


## Referências

IBM Watson Analytics. (2016). *IBM HR Analytics Employee Attrition & Performance Dataset*. IBM Corporation.  
Disponível em: https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset
