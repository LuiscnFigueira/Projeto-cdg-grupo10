# Milestone 1: Iniciação e Definição do Projeto 
 
## 1. Descrição Detalhada do Problema

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

## 2. Objetivos SMART 
 
1.  **Objetivo 1:** Desenvolver um modelo de classificação supervisionado para prever o attrition, alcançando um
F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 28/02/2026
(Milestone 3).
 
2.  **Objetivo 2:** Construir um índice de risco de attrition baseado nas probabilidades previstas pelo modelo,
classificando os colaboradores em categorias de baixo risco (<30%), médio risco (30–60%) e alto
risco (>60%), até ao dia 07/03/2026.

3. **Objetivo 3:** Aplicar técnicas de clustering não supervisionado para identificar e caracterizar perfis distintos de
colaboradores com base nas variáveis relevantes do dataset, determinando o número ótimo de
clusters através do método do cotovelo e do Silhouette Score, garantindo um valor médio de
Silhouette superior a 0,50, e descrevendo estatisticamente cada perfil identificado, até ao dia
07/03/2026.

### 2.1 Perguntas de Investigação

As perguntas de investigação estruturam o enquadramento científico do estudo, orientando a análise empírica dos dados com o objetivo de identificar os principais determinantes do attrition, avaliar a sua relevância estatística e preditiva e verificar a viabilidade de desenvolver modelos capazes de prever este fenómeno de forma fiável.

1. **Quais são as variáveis com maior poder explicativo e preditivo do attrition dos colaboradores?**

2. **Existe uma associação estatisticamente significativa entre a realização de horas extraordinárias (OverTime) e a probabilidade de attrition?**

3. **O nível de satisfação no trabalho (JobSatisfaction) e o equilíbrio entre vida pessoal e profissional (WorkLifeBalance) influenciam significativamente o risco de attrition?**

4. **O rendimento mensal (MonthlyIncome) tem impacto significativo na probabilidade de attrition, mesmo após controlo multivariável?**

5. **Qual dos algoritmos de classificação testados apresenta melhor desempenho e maior estabilidade na previsão do attrition?**

6. **O desequilíbrio da variável alvo influencia o desempenho dos modelos preditivos e pode ser mitigado através da aplicação da técnica SMOTE?**

7. **É possível construir um índice de risco de attrition interpretável e fiável que permita classificar os colaboradores de acordo com o seu nível de risco?**

8. **Que fatores distinguem os colaboradores com maior risco de attrition dos restantes, e como podem ser utilizados para apoiar estratégias de retenção?**

 
## 3. Metodologia de Gestão (PBL) 
* **Divisão de Tarefas:** 
  * **Membro A:** Responsável pela Engenharia de Dados. 
  * **Membro B:** Responsável pela Modelação Estatística. 
  * **Membro C:** Responsável pela Visualização e Documentação. 
* **Ferramentas de Colaboração:** Kaggle Notebooks (ambiente de desenvolvimento), GitHub (controlo de versões e partilha de código), GitHub Projects (gestão de tarefas) e reuniões semanais (Google Meet).
 
## 4. Análise de Viabilidade dos Dados 
* **Disponibilidade:** O dataset "IBM HR Analytics Employee Attrition & Performance" encontra-se disponível publicamente na plataforma Kaggle em formato CSV. Os dados já foram importados para o ambiente Kaggle Notebook, estando estruturados de forma tabular com 1470 observações e 35 variáveis. A estrutura é ideal para análise estatística e modelação supervisionada, sem necessidade de integração com bases de dados relacionais externas nesta fase. 
* **Qualidade Inicial:** A análise exploratória preliminar (EDA) indica uma excelente qualidade inicial, com ausência de valores nulos. No entanto, identificámos a necessidade de limpeza de dados, uma vez que existem variáveis sem valor preditivo (variância zero ou identificadores únicos) foram removidas e não estarão na Milestone 2, nomeadamente: EmployeeNumber, EmployeeCount, Over18 e StandardHours. O dataset apresenta um mix de variáveis categóricas, numéricas e ordinais que exigirão técnicas específicas de codificação (encoding).
* **Ética:** O dataset está totalmente anonimizado, não contendo identificadores pessoais diretos (nomes, contactos ou IDs reais). A utilização do projeto é estritamente académica, cumprindo os princípios do RGPD, designadamente a minimização de dados, finalidade específica e utilização legítima. Não existe risco de identificação individual, garantindo a integridade ética da investigação.

### Dicionário das variáveis
* **Age (Idade):** idade do funcionário, variável numérica com intervalo (18-60).
* **Attrition (Atrito):** indica se o funcionário teve atrito (saída) da empresa (“Sim” ou “Não”),cesta variável é classificada como uma string.
* **BusinessTravel (Viagem de Negócios):** O tipo de viagem de negócios frequentemente realizada pelo funcionário (“Travel_Rarely”, “Travel_Frequently” ou “Non-Travel”).
* **DailyRate (Diária):** Diárias entre (102-1499).

### Fonte de Dados
* **Dataset:** https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset

* **Fonte do Código:** https://www.kaggle.com/code/lusfigueira/m1-inicia-o
 
## 5. Cronograma Interno 
| Fase | Data Limite | Entregável Esperado | 
| :--- | :--- | :--- | 
| M1: Iniciação | [24/02/2026] | Repositório estruturado e Plano de Projeto. | 
| M2: Exploração | [24/03/2026] | Notebook de EDA e Dados Processados. | 
| M3: Modelação | [21/04/2026] | Comparação de algoritmos e métricas. | 
| M4: Finalização| [21/05/2026] | Pitch e Relatório Final. | 
 --- 
*Data de última atualização: 20/02/2026*
