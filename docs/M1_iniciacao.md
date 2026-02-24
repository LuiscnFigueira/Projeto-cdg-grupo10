# Milestone 1: Iniciação e Definição do Projeto 
 
## 1. Descrição Detalhada do Problema

### Contexto do Setor: Gestão de Capital Humano e HR Analytics 

O capital humano constitui um dos ativos mais críticos para o desempenho, inovação e sustentabilidade das organizações modernas, particularmente em setores intensivos em conhecimento, como o setor tecnológico e de serviços. Neste contexto, a área de HR Analytics (People Analytics) tem emergido como uma disciplina estratégica que utiliza dados e técnicas de análise quantitativa para apoiar a tomada de decisão na gestão de recursos humanos.

Tradicionalmente, a gestão de recursos humanos baseava-se em abordagens maioritariamente reativas e qualitativas. No entanto, com o aumento da disponibilidade de dados organizacionais e o desenvolvimento de técnicas de Ciência de Dados e Machine Learning, tornou-se possível adotar uma abordagem orientada por dados, permitindo analisar padrões comportamentais, identificar fatores de risco e antecipar fenómenos relevantes. Esta evolução permite transformar os Recursos Humanos numa função estratégica e preditiva, contribuindo para uma gestão mais eficiente e fundamentada do capital humano.

Uma das aplicações mais relevantes neste domínio é a análise da rotatividade de colaboradores (Employee Attrition), que corresponde à saída de colaboradores de uma organização e constitui um indicador crítico da estabilidade e eficiência organizacional.

### Relevância do Problema no Contexto Atual

A rotatividade de colaboradores representa um desafio estratégico significativo devido ao seu impacto direto e indireto nas organizações. Do ponto de vista financeiro, a saída de um colaborador implica custos associados ao recrutamento, seleção, integração e formação de novos colaboradores, bem como custos indiretos relacionados com a perda de produtividade e transferência de conhecimento. Em funções especializadas, estes custos podem atingir valores substanciais.

Para além do impacto económico, a rotatividade afeta também o desempenho organizacional, podendo comprometer a continuidade operacional, reduzir a coesão das equipas e afetar negativamente o clima organizacional. Num contexto de elevada competitividade e escassez de talento qualificado, a capacidade de reter colaboradores constitui um fator determinante para a vantagem competitiva e sustentabilidade das organizações.

A saída de colaboradores é influenciada por múltiplos fatores, incluindo características demográficas, condições de trabalho, remuneração, progressão na carreira e níveis de satisfação profissional. A identificação e análise destes fatores constitui um passo fundamental para permitir às organizações desenvolver estratégias eficazes de retenção e otimizar a gestão do seu capital humano.

### Formulação do Problema no Contexto da Ciência de Dados

Para analisar este problema, será utilizado o dataset IBM HR Analytics Employee Attrition & Performance, disponibilizado na plataforma Kaggle, que contém dados estruturados relativos a 1470 colaboradores e 35 variáveis que descrevem características demográficas, profissionais, salariais e níveis de satisfação.

A variável central deste estudo é Attrition, uma variável categórica binária que indica se um colaborador abandonou a organização ("Yes") ou permaneceu na empresa ("No"). Esta variável constitui a variável alvo do projeto.

O problema pode ser formalizado como um problema de análise exploratória de dados e classificação supervisionada, em que o objetivo consiste em analisar a relação entre um conjunto de variáveis explicativas e a variável alvo Attrition. Esta análise permitirá identificar padrões e fatores associados à rotatividade de colaboradores, bem como compreender a influência relativa das diferentes variáveis disponíveis no dataset.

Através da aplicação de técnicas de análise de dados, será possível transformar dados organizacionais em conhecimento relevante, permitindo compreender melhor os determinantes da rotatividade e apoiar futuras abordagens preditivas.

### Objetivo Analítico do Projeto

O objetivo analítico principal deste projeto consiste em aplicar metodologias de Ciência de Dados ao dataset IBM HR Analytics Employee Attrition & Performance, com o propósito de identificar, analisar e compreender os fatores associados à rotatividade de colaboradores.

Especificamente, pretende-se realizar uma análise exploratória detalhada do dataset, avaliar a relação entre as variáveis explicativas e a variável alvo Attrition, e estabelecer uma base analítica robusta que suporte o desenvolvimento e validação de modelos preditivos em fases posteriores do projeto.

Este projeto demonstra a aplicação prática de técnicas de análise de dados ao domínio da gestão de recursos humanos, evidenciando o potencial da Ciência de Dados como ferramenta de suporte à tomada de decisão estratégica e à otimização da gestão do capital humano.

## 2. Objetivos SMART
 
1.  **Objetivo 1:** Desenvolver um modelo de classificação supervisionado para prever o attrition, alcançando um
F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 21/04/2026
(Milestone 3).
 
2.  **Objetivo 2:** Construir um índice de risco de attrition baseado nas probabilidades previstas pelo modelo,
classificando os colaboradores em categorias de baixo risco (<30%), médio risco (30–60%) e alto
risco (>60%), até ao dia 21/04/2026.

3. **Objetivo 3:** Aplicar técnicas de clustering não supervisionado para identificar e caracterizar perfis distintos de
colaboradores com base nas variáveis relevantes do dataset, determinando o número ótimo de
clusters através do método do cotovelo e do Silhouette Score, garantindo um valor médio de
Silhouette superior a 0,50, e descrevendo estatisticamente cada perfil identificado, até ao dia
21/04/2026.

### 2.1 Perguntas de Investigação

As perguntas de investigação estruturam o enquadramento científico do estudo, orientando a análise empírica dos dados com o objetivo de identificar os principais determinantes do attrition, avaliar a sua relevância estatística e preditiva e verificar a viabilidade de desenvolver modelos capazes de prever este fenómeno de forma fiável.

**1.** Quais são as variáveis com maior poder explicativo e preditivo do attrition dos colaboradores?

**2.** Existe uma associação estatisticamente significativa entre a realização de horas extraordinárias (OverTime) e a probabilidade de attrition?

**3.** O nível de satisfação no trabalho (JobSatisfaction) e o equilíbrio entre vida pessoal e profissional (WorkLifeBalance) influenciam significativamente o risco de attrition?

**4.** O rendimento mensal (MonthlyIncome) tem impacto significativo na probabilidade de attrition, mesmo após controlo multivariável?

**5.** Qual dos algoritmos de classificação testados apresenta melhor desempenho e maior estabilidade na previsão do attrition?

**6.** O desequilíbrio da variável alvo influencia o desempenho dos modelos preditivos e pode ser mitigado através da aplicação da técnica SMOTE?

**7.** É possível construir um índice de risco de attrition interpretável e fiável que permita classificar os colaboradores de acordo com o seu nível de risco?

**8.** Que fatores distinguem os colaboradores com maior risco de attrition dos restantes, e como podem ser utilizados para apoiar estratégias de retenção?

## 2.2 Operacionalização dos Objetivos

Os objetivos definidos para este projeto foram operacionalizados em termos técnicos, especificando os métodos, métricas de avaliação, critérios de validação e resultados esperados. Esta operacionalização assegura que os objetivos são mensuráveis, reprodutíveis e cientificamente verificáveis, estabelecendo uma ligação clara entre o enquadramento conceptual e a sua implementação prática.

### Objetivo 1 — Desenvolvimento de um Modelo Preditivo de Attrition

O primeiro objetivo consiste no desenvolvimento e validação de um modelo de classificação supervisionado capaz de prever a variável alvo Attrition, utilizando as variáveis explicativas disponíveis no dataset. O problema é formalizado como um problema de classificação binária, no qual cada observação é representada por um conjunto de atributos e uma variável alvo que indica o estado de permanência ou saída.

Serão avaliados diferentes algoritmos de Machine Learning, incluindo métodos lineares e baseados em árvores de decisão, sendo o desempenho avaliado através de validação cruzada estratificada com k = 5 folds. A métrica principal de avaliação será o F1-Score, devido à sua adequação em contextos de desequilíbrio de classes, sendo definido como objetivo atingir um valor mínimo de 0,80. Adicionalmente, serão consideradas métricas complementares, como Precision, Recall e AUC-ROC, de forma a garantir uma avaliação robusta e abrangente do desempenho dos modelos.

### Objetivo 2 — Construção de um Índice de Risco de Attrition

O segundo objetivo consiste na construção de um índice de risco de attrition baseado nas probabilidades previstas pelo modelo desenvolvido. A probabilidade estimada de saída será utilizada como medida quantitativa de risco individual, permitindo transformar a previsão do modelo numa métrica interpretável e operacional.

Com base nesta probabilidade, os colaboradores serão classificados em três categorias de risco: baixo risco (probabilidade inferior a 30%), risco intermédio (probabilidade entre 30% e 60%) e alto risco (probabilidade superior a 60%). Esta segmentação permitirá identificar diferentes níveis de vulnerabilidade e suportar a análise diferenciada dos perfis de risco presentes no dataset.

### Objetivo 3 — Identificação de Perfis de Colaboradores através de Clustering

O terceiro objetivo consiste na aplicação de técnicas de aprendizagem não supervisionada com o propósito de identificar perfis distintos de colaboradores com base nas suas características. Será utilizado o algoritmo K-Means como método principal de clustering, sendo o número ótimo de clusters determinado com recurso ao método do cotovelo e ao coeficiente de Silhouette.

O coeficiente de Silhouette será utilizado como critério quantitativo de validação, sendo definido como objetivo alcançar um valor médio superior a 0,50, indicando uma estrutura de clusters bem definida. Após a identificação dos clusters, será realizada uma caracterização estatística detalhada de cada perfil, permitindo identificar diferenças estruturais entre segmentos e compreender melhor a heterogeneidade presente no dataset.

Esta operacionalização assegura que os objetivos do projeto são implementados de forma rigorosa e mensurável, estabelecendo uma base metodológica sólida para as fases subsequentes de preparação dos dados, modelação e avaliação.

### 2.3 Operacionalização das Perguntas de Investigação

1. **Identificação das variáveis com maior poder explicativo e preditivo**

Será realizada uma análise exploratória e estatística com o objetivo de identificar as variáveis com maior associação à variável alvo `Attrition`. Esta análise incluirá o cálculo de medidas de associação estatística, análise comparativa entre classes e avaliação da importância das variáveis em modelos de classificação supervisionada, permitindo identificar os principais fatores associados à saída dos colaboradores.

2. **Avaliação da associação entre OverTime e Attrition**

A relação entre a variável `OverTime` e a variável alvo será analisada através de testes estatísticos de independência, nomeadamente o teste do qui-quadrado, bem como através da comparação das proporções de attrition entre grupos. Esta análise permitirá avaliar se a realização de horas extraordinárias está estatisticamente associada a um aumento da probabilidade de saída.

3. **Análise do impacto de variáveis psicométricas ordinais**

Será analisada a relação entre variáveis ordinais, como `JobSatisfaction`, `EnvironmentSatisfaction`, `RelationshipSatisfaction` e `WorkLifeBalance`, e a variável alvo. Serão utilizadas técnicas de análise estatística e comparação de distribuições entre classes, bem como medidas de correlação não paramétrica, de forma a avaliar se níveis reduzidos de satisfação constituem fatores preditivos de attrition.

4. **Avaliação do impacto do rendimento mensal na probabilidade de attrition**

Será investigada a relação entre a variável `MonthlyIncome` e a variável alvo, analisando a sua distribuição entre colaboradores que permanecem e colaboradores que saem. Esta análise será complementada por modelos multivariáveis, permitindo avaliar se o rendimento apresenta capacidade preditiva independente após controlo de outras variáveis relevantes.

5. **Comparação do desempenho de diferentes algoritmos de classificação**

Serão treinados e avaliados múltiplos algoritmos de classificação supervisionada, incluindo Regressão Logística, Random Forest e Gradient Boosting. O desempenho será avaliado através de validação cruzada estratificada, com base em métricas como F1-score, Precision, Recall e AUC-ROC, permitindo identificar o modelo com melhor capacidade de generalização.

6. **Avaliação do impacto do desequilíbrio de classes e aplicação de SMOTE**

Será analisado o impacto do desequilíbrio da variável alvo no desempenho dos modelos, comparando os resultados obtidos antes e após a aplicação da técnica SMOTE. Esta análise permitirá avaliar se o balanceamento das classes melhora a capacidade do modelo de identificar corretamente observações pertencentes à classe minoritária.

7. **Construção e validação de um índice de risco de attrition**

Será desenvolvido um índice de risco baseado nas probabilidades previstas pelo modelo de classificação. As observações serão segmentadas em categorias de risco, e será avaliada a capacidade deste índice para discriminar diferentes níveis de probabilidade de attrition, garantindo a sua interpretabilidade e utilidade analítica.

8. **Identificação de perfis distintos através de clustering**

Serão aplicadas técnicas de clustering, nomeadamente o algoritmo K-Means, com o objetivo de identificar perfis distintos de colaboradores com base nas suas características. Os clusters obtidos serão caracterizados estatisticamente, permitindo identificar segmentos com diferentes padrões estruturais e níveis de risco associados ao attrition.

 
## 3. Metodologia de Gestão (PBL) 
O projeto segue uma abordagem baseada no modelo CRISP-DM (Cross-Industry Standard Process for Data Mining), que estrutura o desenvolvimento em seis fases principais: compreensão do problema, compreensão dos dados, preparação dos dados, modelação, avaliação e implementação. Embora estas fases tenham uma ordem lógica, o processo é iterativo e permite regressar a fases anteriores sempre que necessário. Esta metodologia assegura uma abordagem estruturada, sistemática e rigorosa ao desenvolvimento de projetos de Ciência de Dados.

**Divisão de Tarefas:** 

 **Luís Figueira:** 
   *  Supervisão geral do projeto
   *  Preparação e estruturação do projeto
   *  Coordenação da comunicação entre membros
    
 **Martim Ferreira:**
   *  Verificação da qualidade e integridade dos dados
   *  Planeamento do cronograma interno
   *  Análise da existência de valores nulos
  
 **Mateus Afonso:** 
  *  Análise preliminar da distribuição da variável alvo Attrition
  *  Compreensão inicial das variáveis e tipos de dados
  *  Preparação de entregáveis de Milestone 1 (relatório e estrutura de projeto)

  **Tarefas conjuntas**
   *  Análise do dataset IBM HR Analytics Employee Attrition & Performance
   *  Definição clara do problema e objetivos SMART
   *  Identificação de limitações e riscos do dataset
   *  Discussão da abordagem metodológica (CRISP-DM)
     
**Ferramentas de Colaboração:** Kaggle Notebooks (ambiente de desenvolvimento), GitHub (controlo de versões e partilha de código), GitHub Projects (gestão de tarefas), Whatsapp (mensagens diárias), Notion (distribuição de tarefas) e Google Meet (reuniões de dois em dois dias).
 
## 4. Análise de Viabilidade dos Dados 
* **Disponibilidade:** O dataset IBM HR Analytics Employee Attrition & Performance encontra-se disponível publicamente na plataforma Kaggle em formato CSV, sob licença de utilização para fins académicos. Os dados foram já importados para o ambiente Kaggle Notebook e encontram-se organizados numa estrutura tabular com 1470 observações e 35 variáveis, o que facilita a aplicação de técnicas de análise estatística e de modelação supervisionada, sem necessidade de integração adicional com bases de dados relacionais nesta fase.
* **Qualidade Inicial:** A análise exploratória preliminar (EDA) revela uma qualidade estrutural elevada, evidenciada pela ausência de valores nulos em todas as variáveis. Ainda assim, foi identificada a necessidade de limpeza de dados, uma vez que o dataset inclui variáveis sem valor preditivo relevante (variância zero ou meros identificadores únicos). Estas variáveis - EmployeeNumber, EmployeeCount, Over18 e StandardHours - foram marcadas para remoção e não serão consideradas a partir da Milestone 2. Adicionalmente, o dataset apresenta uma combinação de variáveis categóricas, numéricas e ordinais, o que exigirá a aplicação de estratégias específicas de codificação (encoding) e transformação durante a fase de preparação dos dados.
* **Ética:** O dataset encontra-se totalmente anonimizado, não contendo identificadores pessoais diretos (como nomes, contactos ou identificadores reais de colaboradores). A utilização dos dados neste projeto é estritamente académica e cumpre os princípios fundamentais do RGPD, nomeadamente a minimização de dados, a limitação da finalidade e a licitude do tratamento. Atendendo à natureza sintética e agregada da informação, o risco de reidentificação individual é negligenciável, garantindo a conformidade ética e a proteção dos participantes.

### Unidade de Análise

Cada observação corresponde a uma instância independente no espaço de características, identificada unicamente pela variável EmployeeNumber, que funciona como identificador técnico sem significado analítico. Cada registo contém os valores observados para as variáveis que caracterizam a entidade observada, incluindo atributos demográficos, profissionais, salariais, organizacionais e níveis de satisfação, bem como o respetivo valor da variável Attrition.

O dataset é composto por 1470 observações, correspondendo cada linha a um registo distinto. A variável Attrition está associada a cada observação e indica o estado de permanência ou saída da entidade observada no contexto organizacional.

Importa referir que o dataset é de natureza sintética, tendo sido gerado pela IBM para fins de demonstração do IBM Watson Analytics, não correspondendo a dados reais de colaboradores.

### Dicionário das variáveis
| Variável                 | Tipo Estatístico      | Domínio                     | Classes / Escala Semântica                                                                                                                                                                       | Definição Operacional                                | Papel Analítico       |
| ------------------------ | --------------------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------- | --------------------- |
| Attrition                | Categórica binária    | {Yes, No}                   | Yes = abandonou<br>No = permaneceu                                                                                                                                                               | Estado de permanência do colaborador na organização. | Variável alvo         |
| Age                      | Numérica discreta     | [18, 60]                    | —                                                                                                                                                                                                | Idade cronológica do colaborador (anos).             | Demográfica           |
| BusinessTravel           | Categórica nominal    | 3 classes                   | Non-Travel<br>Travel_Rarely<br>Travel_Frequently                                                                                                                                                 | Frequência de deslocações profissionais.             | Organizacional        |
| DailyRate                | Numérica discreta     | [102, 1499]                 | —                                                                                                                                                                                                | Taxa de remuneração diária.                          | Compensação           |
| Department               | Categórica nominal    | 3 classes                   | • Sales<br> • Research & Development<br> • Human Resources                                                                                                                                               | Departamento organizacional.                         | Organizacional        |
| DistanceFromHome         | Numérica discreta     | [1, 29]                     | —                                                                                                                                                                                                | Distância entre residência e local de trabalho.      | Geográfica            |
| Education                | Categórica ordinal    | {1,2,3,4,5}                 | 1 Below College<br>2 College<br>3 Bachelor<br>4 Master<br>5 Doctor                                                                                                                               | Nível de qualificação académica.                     | Capital humano        |
| EducationField           | Categórica nominal    | 6 classes                   | • Life Sciences<br> • Medical<br> • Marketing<br> • Technical Degree<br> • Human Resources<br> • Other                                                                                                            | Área disciplinar da formação académica.              | Capital humano        |
| EmployeeCount            | Constante             | {1}                         | 1                                                                                                                                                                                                | Variável administrativa sem variabilidade.           | Não informativa       |
| EmployeeNumber           | Identificador nominal | Valores únicos              | —                                                                                                                                                                                                | Identificador único do colaborador.                  | Não informativa       |
| EnvironmentSatisfaction  | Categórica ordinal    | {1,2,3,4}                   | 1 Low<br>2 Medium<br>3 High<br>4 Very High                                                                                                                                                       | Grau de satisfação com o ambiente organizacional.    | Psicométrica          |
| Gender                   | Categórica nominal    | {Male, Female}              | Male<br>Female                                                                                                                                                                                   | Género do colaborador.                               | Demográfica           |
| HourlyRate               | Numérica discreta     | [30, 100]                   |  —                                                                                                                                                                                              | Taxa de remuneração horária.                         | Compensação           |
| JobInvolvement           | Categórica ordinal    | {1,2,3,4}                   | 1 Low<br>2 Medium<br>3 High<br>4 Very High                                                                                                                                                       | Grau de envolvimento no trabalho.                    | Psicométrica          |
| JobLevel                 | Categórica ordinal    | {1,2,3,4,5}                 | 1–5 níveis hierárquicos                                                                                                                                                                          | Nível hierárquico organizacional.                    | Organizacional        |
| JobRole                  | Categórica nominal    | 9 classes                   | • Sales Executive<br> • Research Scientist<br> • Laboratory Technician<br> • Manufacturing Director<br> • Healthcare Representative<br> • Manager<br> • Sales Representative<br> • Research Director<br> • Human Resources | Função ocupacional.                                  | Organizacional        |
| JobSatisfaction          | Categórica ordinal    | {1,2,3,4}                   | 1 Low<br>2 Medium<br>3 High<br>4 Very High                                                                                                                                                       | Grau de satisfação com o trabalho.                   | Psicométrica          |
| MaritalStatus            | Categórica nominal    | {Single, Married, Divorced} | Single<br>Married<br>Divorced                                                                                                                                                                    | Estado civil.                                        | Demográfica           |
| MonthlyIncome            | Numérica discreta     | [1009, 19999]               | —                                                                                                                                                                                                | Rendimento mensal.                                   | Compensação           |
| MonthlyRate              | Numérica discreta     | [2094, 26999]               | —                                                                                                                                                                                                | Taxa mensal administrativa.                          | Compensação           |
| NumCompaniesWorked       | Numérica discreta     | [0, 9]                      | —                                                                                                                                                                                                | Número de empregadores anteriores.                   | Capital humano        |
| Over18                   | Constante             | {Y}                         | Y                                                                                                                                                                                                | Indica idade superior a 18 anos.                     | Não informativa       |
| OverTime                 | Categórica binária    | {Yes, No}                   | Yes<br>No                                                                                                                                                                                        | Indicador de trabalho extraordinário.                | Condições de trabalho |
| PercentSalaryHike        | Numérica discreta     | [11, 25]                    | —                                                                                                                                                                                                | Percentagem de aumento salarial recente.             | Compensação           |
| PerformanceRating        | Categórica ordinal    | {1,2,3,4}                  | 1 Low<br>2 Good<br>3 Excellent<br>4 Outstanding                                                                                                                                                  | Avaliação formal de desempenho.                      | Desempenho            |
| RelationshipSatisfaction | Categórica ordinal    | {1,2,3,4}                   | 1 Low<br>2 Medium<br>3 High<br>4 Very High                                                                                                                                                       | Grau de satisfação com relações profissionais.       | Psicométrica          |
| StandardHours            | Constante             | {80}                        | 80                                                                                                                                                                                               | Número padrão de horas de trabalho.                  | Não informativa       |
| StockOptionLevel         | Categórica ordinal    | {0,1,2,3}                   | 0 None<br>1 Low<br>2 Medium<br>3 High                                                                                                                                                            | Nível de compensação em ações.                       | Compensação           |
| TotalWorkingYears        | Numérica discreta     | [0, 40]                     | —                                                                                                                                                                                                | Experiência profissional total.                      | Capital humano        |
| TrainingTimesLastYear    | Numérica discreta     | [0, 6]                      | —                                                                                                                                                                                                | Número de formações realizadas.                      | Desenvolvimento       |
| WorkLifeBalance          | Categórica ordinal    | {1,2,3,4}                   | 1 Bad<br>2 Good<br>3 Better<br>4 Best                                                                                                                                                            | Equilíbrio trabalho–vida pessoal.                    | Psicométrica          |
| YearsAtCompany           | Numérica discreta     | [0, 40]                     | —                                                                                                                                                                                                | Antiguidade organizacional.                          | Organizacional        |
| YearsInCurrentRole       | Numérica discreta     | [0, 18]                     | —                                                                                                                                                                                                | Tempo na função atual.                               | Organizacional        |
| YearsSinceLastPromotion  | Numérica discreta     | [0, 15]                     | —                                                                                                                                                                                                | Tempo desde última promoção.                         | Organizacional        |
| YearsWithCurrManager     | Numérica discreta     | [0, 17]                     | —                                                                                                                                                                                                | Tempo com gestor atual.                              | Organizacional        |
## 4.1 Compreensão e Avaliação Inicial dos Dados

### 4.1.1 Estrutura e Composição do Dataset

No âmbito da fase de Data Understanding do modelo CRISP-DM, foi realizada uma análise sistemática da estrutura do dataset com o objetivo de caracterizar formalmente o espaço de características, avaliar a sua adequação ao problema de classificação supervisionada e identificar propriedades com implicações nas fases subsequentes de preparação e modelação.

O dataset foi carregado na sua forma bruta e convertido numa estrutura do tipo DataFrame, permitindo a inspeção programática da sua composição e propriedades estatísticas. A análise estrutural confirmou que o dataset é composto por 1470 observações e 35 variáveis, em conformidade com a documentação original. Cada observação corresponde a um registo independente no espaço de características, descrito por um conjunto de variáveis demográficas, organizacionais, profissionais e psicométricas, bem como pelo respetivo valor da variável alvo `Attrition`.

A inspeção dos tipos de dados revelou a presença de variáveis numéricas inteiras e variáveis categóricas codificadas como texto, refletindo a natureza discreta e categórica das dimensões representadas. Esta estrutura é consistente com um problema de classificação supervisionada, em que cada observação constitui uma instância independente caracterizada por um vetor de atributos e uma variável alvo associada.



### 4.1.2 Avaliação da Qualidade e Integridade dos Dados

Foi realizada uma verificação formal da integridade do dataset com o objetivo de avaliar a sua completude, consistência e unicidade. Esta análise confirmou a ausência total de valores nulos em todas as variáveis, garantindo que o dataset se encontra completo e eliminando a necessidade de aplicar técnicas de imputação. Esta propriedade permite preservar as distribuições empíricas observadas e evita a introdução de viés decorrente de estimativas artificiais.

Adicionalmente, verificou-se a inexistência de registos duplicados, garantindo que cada observação representa uma entidade distinta no espaço de dados. Esta condição é fundamental para preservar a independência das observações, um pressuposto assumido pela maioria dos algoritmos de Machine Learning e essencial para assegurar a validade estatística do processo de modelação.



### 4.1.3 Identificação e Remoção de Variáveis Não Informativas

A análise da cardinalidade e variabilidade das variáveis permitiu identificar a presença de variáveis sem capacidade discriminativa ou relevância preditiva para o problema em estudo. Em particular, verificou-se que as variáveis `EmployeeCount`, `Over18` e `StandardHours` apresentam valores constantes em todas as observações, não contribuindo para a explicação da variabilidade da variável alvo. Adicionalmente, a variável `EmployeeNumber` constitui um identificador único atribuído a cada observação, não possuindo qualquer significado estatístico ou relação causal com o fenómeno em análise.

Do ponto de vista da teoria da informação, estas variáveis apresentam entropia nula ou irrelevante, sendo incapazes de fornecer informação útil ao processo de aprendizagem. A sua inclusão aumentaria artificialmente a dimensionalidade do espaço de características, podendo introduzir ruído e reduzir a eficiência dos algoritmos de modelação. Por este motivo, estas variáveis foram removidas programaticamente, reduzindo o número de variáveis de 35 para 31 e melhorando a qualidade analítica do dataset.

### 4.1.4 Caracterização Estatística das Variáveis

Foi realizada uma análise estatística descritiva das variáveis numéricas com o objetivo de caracterizar a sua distribuição, amplitude e variabilidade. Esta análise incluiu o cálculo de estatísticas fundamentais, como média, mediana, desvio padrão, valores mínimos e máximos e percentis.

Os resultados evidenciaram diferenças significativas na escala e dispersão das variáveis, refletindo a heterogeneidade das características representadas. Estas diferenças são particularmente relevantes no contexto da modelação preditiva, uma vez que determinados algoritmos, como regressão logística, máquinas de vetores de suporte e redes neuronais, são sensíveis à escala das variáveis. Esta observação justifica a eventual aplicação de técnicas de normalização ou padronização na fase de preparação dos dados, com o objetivo de garantir estabilidade numérica e melhorar o desempenho dos modelos.

A análise confirmou ainda que as variáveis apresentam variabilidade suficiente para suportar o processo de aprendizagem automática, não tendo sido identificadas anomalias estruturais ou inconsistências evidentes.


### 4.1.5 Distribuição da Variável Alvo

Foi analisada a distribuição da variável alvo `Attrition`, que representa o estado de permanência ou saída de cada colaborador. A análise revelou um cenário de desequilíbrio de classes (*class imbalance*), com aproximadamente 83.9% das observações correspondentes à classe negativa (permanência) e 16.1% à classe positiva (saída).

Este desequilíbrio constitui um fator crítico no contexto da modelação supervisionada, uma vez que algoritmos treinados diretamente sobre dados desequilibrados tendem a favorecer a classe maioritária, podendo apresentar desempenho aparentemente elevado em termos de acurácia, mas reduzida capacidade de identificação da classe minoritária. Este aspeto será considerado nas fases subsequentes do processo CRISP-DM, nomeadamente na seleção de métricas de avaliação apropriadas, como Precision, Recall, F1-score e Precision-Recall AUC, e na eventual aplicação de técnicas de balanceamento de classes.


### 4.1.6 Síntese da Compreensão dos Dados

A análise realizada permitiu confirmar que o dataset apresenta elevada qualidade estrutural, caracterizada pela ausência de valores em falta, inexistência de registos duplicados e presença de variáveis com variabilidade adequada ao problema em estudo. A identificação e remoção de variáveis não informativas contribuiu para melhorar a qualidade do espaço de características e reduzir a complexidade do processo de modelação.

Adicionalmente, a caracterização estatística das variáveis e a análise da distribuição da variável alvo permitiram identificar propriedades relevantes, incluindo diferenças na escala das variáveis e a presença de desequilíbrio entre classes, fatores com implicações diretas na preparação dos dados e no desenvolvimento de modelos preditivos.

Esta fase estabelece uma base metodológica sólida para as etapas subsequentes do processo CRISP-DM, nomeadamente a preparação dos dados, transformação das variáveis e desenvolvimento de modelos de classificação supervisionada robustos e generalizáveis.

## 4.2 Limitações do Dataset

Apesar da elevada qualidade estrutural do dataset, existem limitações metodológicas que devem ser consideradas na interpretação dos resultados e no desenvolvimento dos modelos preditivos.

Em primeiro lugar, o dataset é de natureza sintética, tendo sido gerado pela IBM para fins de demonstração do IBM Watson Analytics. Consequentemente, as relações entre variáveis podem não refletir integralmente os padrões observados em contextos organizacionais reais, o que, aliado à estrutura transversal do dataset, limita simultaneamente a generalização externa dos resultados e a inferência de relações causais dinâmicas.

Em segundo lugar, a variável `PerformanceRating` apresenta variância praticamente nula, com a quase totalidade das observações concentradas nos valores 3 e 4, o que limita o seu poder discriminativo e poderá justificar a sua exclusão ou tratamento específico na fase de preparação dos dados.

Adicionalmente, o dataset não inclui determinadas variáveis potencialmente relevantes, como indicadores de desempenho histórico detalhado, métricas de engagement ou fatores externos ao contexto organizacional, o que pode condicionar o poder preditivo máximo dos modelos.

Estas limitações serão consideradas na interpretação dos resultados e na avaliação da capacidade de generalização dos modelos desenvolvidos.

### Fonte de Dados
* **Dataset:** https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset
* **Fonte do Código:** https://www.kaggle.com/code/lusfigueira/m1-inicia-o

## 5. Cronograma Interno 
| Fase | Data Limite | Entregável Esperado | 
| :--- | :--- | :--- | 
| M1: Iniciação | 24/02/2026 | Repositório estruturado e Plano de Projeto. | 
| M2: Exploração | [24/03/2026] | Notebook de EDA e Dados Processados. | 
| M3: Modelação | [21/04/2026] | Comparação de algoritmos e métricas. | 
| M4: Finalização| [21/05/2026] | Pitch e Relatório Final. | 
 --- 
 ### Referência do Dataset
IBM Watson Analytics. (2016). IBM HR Analytics Employee Attrition & Performance Dataset. IBM Corporation.  
Disponível em: https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset

*Data de última atualização: 24/02/2026*
