# People Analytics and Employee Turnover Prediction
## Identificação da Equipa
* **Grupo nº:** 10
* **Membros:**
* Luís Figueira - 2022160309 - a2022160309@alumni.iscac.pt 
* Martim Ferreira - 2023132459 - a2023132459@alumni.iscac.pt
* Mateus Afonso - 2023142275 - a2023142275@alumni.iscac.pt
## Organização do Repositório
A estrutura deste projeto segue as boas práticas de Ciência de Dados e Engenharia de Software:
* **`data/`**: Armazenamento de dados (dados brutos em `raw/` e processados em `processed/`).
* **`docs/`**: Documentação técnica detalhada dividida por Milestones (M1, M2 e M3).
* **`notebooks/`**: Jupyter Notebooks para experimentação, limpeza e modelação.
* **`src/`**: Código-fonte modular (scripts `.py`) para funções reutilizáveis.
* **`reports/`**: Relatórios finais, apresentações e exportação de figuras (`figures/`).
* **`requirements.txt`**: Ficheiro de configuração com as bibliotecas necessárias.
## 1. Iniciação (Milestone 1)

**Contexto do Setor: Gestão de Capital Humano e HR Analytics** 

O capital humano constitui um dos ativos mais críticos para o desempenho, inovação e sustentabilidade das organizações modernas, particularmente em setores intensivos em conhecimento, como o setor tecnológico e de serviços. Neste contexto, a área de HR Analytics (People Analytics) tem emergido como uma disciplina estratégica que utiliza dados e técnicas de análise quantitativa para apoiar a tomada de decisão na gestão de recursos humanos.

Tradicionalmente, a gestão de recursos humanos baseava-se em abordagens maioritariamente reativas e qualitativas. No entanto, com o aumento da disponibilidade de dados organizacionais e o desenvolvimento de técnicas de Ciência de Dados e Machine Learning, tornou-se possível adotar uma abordagem orientada por dados, permitindo analisar padrões comportamentais, identificar fatores de risco e antecipar fenómenos relevantes. Esta evolução permite transformar os Recursos Humanos numa função estratégica e preditiva, contribuindo para uma gestão mais eficiente e fundamentada do capital humano.

Uma das aplicações mais relevantes neste domínio é a análise da rotatividade de colaboradores (Employee Attrition), que corresponde à saída de colaboradores de uma organização e constitui um indicador crítico da estabilidade e eficiência organizacional.


**Relevância do Problema no Contexto Atual**

A rotatividade de colaboradores representa um desafio estratégico significativo devido ao seu impacto direto e indireto nas organizações. Do ponto de vista financeiro, a saída de um colaborador implica custos associados ao recrutamento, seleção, integração e formação de novos colaboradores, bem como custos indiretos relacionados com a perda de produtividade e transferência de conhecimento. Em funções especializadas, estes custos podem atingir valores substanciais.

Para além do impacto económico, a rotatividade afeta também o desempenho organizacional, podendo comprometer a continuidade operacional, reduzir a coesão das equipas e afetar negativamente o clima organizacional. Num contexto de elevada competitividade e escassez de talento qualificado, a capacidade de reter colaboradores constitui um fator determinante para a vantagem competitiva e sustentabilidade das organizações.

A saída de colaboradores é influenciada por múltiplos fatores, incluindo características demográficas, condições de trabalho, remuneração, progressão na carreira e níveis de satisfação profissional. A identificação e análise destes fatores constitui um passo fundamental para permitir às organizações desenvolver estratégias eficazes de retenção e otimizar a gestão do seu capital humano.

**Formulação do Problema no Contexto da Ciência de Dados**

Para analisar este problema, será utilizado o dataset IBM HR Analytics Employee Attrition & Performance, disponibilizado na plataforma Kaggle, que contém dados estruturados relativos a 1470 colaboradores e 35 variáveis que descrevem características demográficas, profissionais, salariais e níveis de satisfação.

A variável central deste estudo é Attrition, uma variável categórica binária que indica se um colaborador abandonou a organização ("Yes") ou permaneceu na empresa ("No"). Esta variável constitui a variável alvo do projeto.

O problema pode ser formalizado como um problema de análise exploratória de dados e classificação supervisionada, em que o objetivo consiste em analisar a relação entre um conjunto de variáveis explicativas e a variável alvo Attrition. Esta análise permitirá identificar padrões e fatores associados à rotatividade de colaboradores, bem como compreender a influência relativa das diferentes variáveis disponíveis no dataset.

Através da aplicação de técnicas de análise de dados, será possível transformar dados organizacionais em conhecimento relevante, permitindo compreender melhor os determinantes da rotatividade e apoiar futuras abordagens preditivas.

**Objetivo Analítico do Projeto**

O objetivo analítico principal deste projeto consiste em aplicar metodologias de Ciência de Dados ao dataset IBM HR Analytics Employee Attrition & Performance, com o propósito de identificar, analisar e compreender os fatores associados à rotatividade de colaboradores.

Especificamente, pretende-se realizar uma análise exploratória detalhada do dataset, avaliar a relação entre as variáveis explicativas e a variável alvo Attrition, e estabelecer uma base analítica robusta que suporte o desenvolvimento e validação de modelos preditivos em fases posteriores do projeto.

Este projeto demonstra a aplicação prática de técnicas de análise de dados ao domínio da gestão de recursos humanos, evidenciando o potencial da Ciência de Dados como ferramenta de suporte à tomada de decisão estratégica e à otimização da gestão do capital humano.

### Objetivos do Projeto
* **Objetivo 1:** Desenvolver um modelo de classificação supervisionado para prever o attrition, alcançando um F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 28/02/2026 (Milestone 3).
* **Objetivo 2:** Construir um índice de risco de attrition baseado nas probabilidades previstas pelo modelo, classificando os colaboradores em categorias de baixo risco (<30%), médio risco (30–60%) e alto risco (>60%), até ao dia 07/03/2026.
* **Objetivo 3:** Aplicar técnicas de clustering não supervisionado para identificar e caracterizar perfis distintos de colaboradores com base nas variáveis relevantes do dataset, determinando o número ótimo de clusters através do método do cotovelo e do Silhouette Score, garantindo um valor médio de Silhouette superior a 0,50, e descrevendo estatisticamente cada perfil identificado, até ao dia 07/03/2026.
### Perguntas de Investigação

As perguntas de investigação estruturam o enquadramento científico do estudo, orientando a análise empírica dos dados com o objetivo de identificar os principais determinantes do attrition, avaliar a sua relevância estatística e preditiva e verificar a viabilidade de desenvolver modelos capazes de prever este fenómeno de forma fiável.

**1.** Quais são as variáveis com maior poder explicativo e preditivo do attrition dos colaboradores?

**2.** Existe uma associação estatisticamente significativa entre a realização de horas extraordinárias (OverTime) e a probabilidade de attrition?

**3.** O nível de satisfação no trabalho (JobSatisfaction) e o equilíbrio entre vida pessoal e profissional (WorkLifeBalance) influenciam significativamente o risco de attrition?

**4.** O rendimento mensal (MonthlyIncome) tem impacto significativo na probabilidade de attrition, mesmo após controlo multivariável?

**5.** Qual dos algoritmos de classificação testados apresenta melhor desempenho e maior estabilidade na previsão do attrition?

**6.** O desequilíbrio da variável alvo influencia o desempenho dos modelos preditivos e pode ser mitigado através da aplicação da técnica SMOTE?

**7.** É possível construir um índice de risco de attrition interpretável e fiável que permita classificar os colaboradores de acordo com o seu nível de risco?

**8.** Que fatores distinguem os colaboradores com maior risco de attrition dos restantes, e como podem ser utilizados para apoiar estratégias de retenção?

### Fonte de Dados
* **Dataset:** https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset
* **Dimensão:** 1470 linhas, 35 colunas
* **Fonte do Código:** https://www.kaggle.com/code/lusfigueira/m1-inicia-o
## 2. Exploração (Milestone 2)
### Limpeza e Preparação
* [Breve resumo das ações de limpeza tomadas. Detalhes em `docs/M2_exploracao.md`]
### Principais Conclusões (EDA)
> *Dica: Insere aqui o gráfico mais importante do projeto.*
* **Ponto-chave:** [Ex: Identificámos que o fator X influencia em 40% o resultado Y, por aplicação
do método ganho de informação]
## 3. Modelação (Milestone 3)
### Abordagem Técnica
* **Modelos:** [Ex: Random Forest e XGBoost]
* **Métrica Principal:** [Ex: F1-Score ou RMSE]
## 4. Finalização (Milestone 4)
### Resposta ao Problema
[Resumo da solução e como ela gera valor para o negócio.]
### Recomendações de Inovação
1. [Sugestão prática baseada nos resultados]
## Como Reproduzir este Projeto
1. Clone o repositório: `git clone [url-do-repo]`
2. Instale as dependências: `pip install -r requirements.txt`
3. Execute os notebooks na pasta `notebooks/` seguindo a ordem numérica.
**Instituição:** Coimbra Business School | ISCAC
**Curso:** Licenciatura em Ciência de Dados para a Gestão
**Unidade Curricular:** Projeto em Ciência de Dados
**Professor Responsável:** Dora Melo (dmelo@iscac.pt)

## Referências
IBM Watson Analytics. (2016). *IBM HR Analytics Employee Attrition & Performance Dataset*.  
Disponível em: https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset
