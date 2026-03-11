# Milestone 2: Análise Exploratória e Engenharia de Atributos 
 
## 1. Análise Exploratória de Dados (EDA) 
### 1.1. Distribuição da Variável Alvo 

Foi analisada a distribuição da variável alvo `Attrition`, que representa o estado de permanência ou saída de cada colaborador. Tratando-se de uma variável categórica binária, o conceito estatístico de distribuição normal não se aplica, baseando-se a nossa análise na proporção de frequências entre as duas categorias.

A análise exploratória revelou um cenário de forte desequilíbrio de classes (class imbalance), com aproximadamente 83.9% das observações correspondentes à classe negativa (permanência) e 16.1% à classe positiva (saída).

Este desequilíbrio constitui um fator crítico no contexto da modelação supervisionada, uma vez que algoritmos treinados diretamente sobre dados desequilibrados tendem a favorecer a classe maioritária. Isto pode apresentar um desempenho aparentemente elevado em termos de acurácia (Accuracy), mas com uma reduzida capacidade de identificação da classe minoritária. Este aspeto será estritamente considerado nas fases subsequentes do processo CRISP-DM, nomeadamente na seleção de métricas de avaliação apropriadas, como Precision, Recall, F1-score e Precision-Recall AUC, e na eventual aplicação de técnicas de balanceamento de classes (ex: SMOTE).
 
### 1.2. Correlações Relevantes

Nesta fase de análise bivariada, utilizámos gráficos de dispersão (scatter plots) para cruzar os atributos do dataset diretamente com a variável alvo `Attrition`, com o objetivo de identificar os principais preditores de rotatividade. Aplicámos um nível de transparência aos pontos do gráfico para revelar a densidade de colaboradores em cada eixo categórico ("Yes" / "No").


* **Atributo Idade (`Age`) vs. Alvo:** Notámos que a idade apresenta uma forte relação com a probabilidade de saída. Observando o scatter plot, a densidade de pontos na classe "Yes" (abandono) está claramente concentrada na faixa etária mais jovem (entre os 20 e os 35 anos). Por outro lado, a linha correspondente aos colaboradores que permanecem ("No") apresenta uma distribuição muito mais vasta e contínua ao longo de todas as idades, indicando que a retenção é superior em faixas etárias mais maduras.

* gráfico:
  
* referências:

* **Atributo Rendimento Mensal (`MonthlyIncome`) vs. Alvo:** O fator financeiro está fortemente ligado à saída de colaboradores. No gráfico de dispersão, a esmagadora maioria dos pontos de Attrition ("Yes") aglomera-se de forma densa no limite inferior do eixo X (salários mais baixos). À medida que o rendimento mensal aumenta, a presença de pontos na classe "Yes" torna-se cada vez mais rara, confirmando que pacotes salariais superiores atuam como um forte mecanismo de retenção.

*  gráfico:

*  referências:

* **Atributo Experiência Total (`TotalWorkingYears`) vs. Alvo:** Notámos que a senioridade e o tempo de carreira estão intimamente ligados à retenção. Observando o scatter plot, a grande mancha de densidade de abandonos ("Yes") concentra-se nos colaboradores com menos de 10 anos de experiência total. Em contrapartida, profissionais com carreiras mais longas (especialmente acima dos 15-20 anos) apresentam uma dispersão residual na linha de saída, provando que a consolidação da carreira reduz drasticamente a rotatividade.

*  gráfico:

*  referências:

### 1.3. Problema de Aprendizagem Supervisionada
A variável alvo do presente projeto, `Attrition`, indica se o colaborador abandonou a organização (Yes) ou permaneceu (No).

A análise da distribuição revelou:

* 83.9% dos colaboradores permaneceram na empresa

* 16.1% dos colaboradores abandonaram a empresa

Esta distribuição demonstra um desequilíbrio significativo entre classes, sendo a classe “Yes” (saída) claramente minoritária.

### Desafios Técnicos Resultantes do Desequilíbrio de Classes

O desequilíbrio poderá influenciar negativamente modelos que utilizem métricas globais como a accuracy, uma vez que um modelo que classifique sempre como “No” obteria uma precisão aparente elevada (cerca de 84%), mas sem qualquer utilidade prática.

Dado que o abandono de colaboradores representa um custo elevado para a organização (recrutamento, integração e perda de conhecimento), a correta identificação da classe minoritária é crítica.

Assim, nas fases seguintes será necessário:

* Utilizar métricas adequadas (Precision, Recall, F1-Score, ROC-AUC)
* Considerar técnicas de balanceamento (SMOTE)
* Avaliar cuidadosamente a matriz de confusão

### 1.4 Matriz de Correlação (Heatmap)

Foi gerada uma matriz de correlação para as variáveis numéricas, com o objetivo de identificar relações lineares relevantes e potenciais situações de multicolinearidade. A análise do heatmap evidenciou três padrões principais:

* Observou-se uma correlação muito elevada entre `JobLevel` e `MonthlyIncome` (≈ 0.95), sugerindo redundância informacional entre progressão hierárquica e remuneração.

* Verificou-se um conjunto de variáveis relacionadas com antiguidade na empresa com correlações elevadas, nomeadamente `YearsAtCompany` com `YearsInCurrentRole` (≈ 0.76) e com `YearsWithCurrManager` (≈ 0.77), indicando sobreposição na medição de estabilidade/tenure.

* Relativamente à variável-alvo (`Attrition_bin`), a associação linear mais evidente ocorre com `OverTime_bin` (≈ 0.25). Adicionalmente, `Age`, `MonthlyIncome`, `JobLevel` e `TotalWorkingYears` apresentam correlações negativas moderadas (≈ -0.16 a -0.17), sugerindo maior probabilidade de saída em colaboradores mais jovens e em níveis salariais/hierárquicos mais baixos.

Estes resultados serão considerados na fase de modelação, particularmente na seleção de atributos e na mitigação de multicolinearidade. Importa referir que correlação não implica causalidade, representando apenas associações lineares observadas nos dados.

 ### 1.5 Análise da Correlação com a Variável Alvo

Para compreender quais variáveis apresentam maior associação com a saída de colaboradores, foi analisada a correlação entre os atributos numéricos e a variável alvo `Attrition_bin`.

Os resultados evidenciam que `OverTime_bin` apresenta a maior correlação positiva (≈ 0.25), sugerindo que colaboradores que realizam horas extraordinárias apresentam maior probabilidade de abandonar a organização. Também se observa uma correlação positiva moderada em `DistanceFromHome`, indicando que colaboradores que vivem mais afastados podem ter maior propensão para saída.

Por outro lado, variáveis relacionadas com experiência profissional e progressão na carreira apresentam correlação negativa moderada com a variável alvo, incluindo `TotalWorkingYears`, `JobLevel`, `MonthlyIncome`, `Age` e `YearsInCurrentRole`. Este padrão sugere que colaboradores mais experientes, com maior remuneração ou posição hierárquica mais elevada tendem a permanecer na organização.

Por fim, algumas variáveis apresentam correlação praticamente nula com a saída, como `PerformanceRating`, `PercentSalaryHike` e `HourlyRate`, indicando que estes fatores não parecem influenciar diretamente a decisão de abandono.

Importa salientar que correlação mede apenas associações lineares e não implica causalidade.

Estes resultados sugerem que fatores relacionados com carga de trabalho, progressão na carreira e remuneração desempenham um papel relevante na retenção de talento.

### 1.6 Conclusões Visuais da Análise Bivariada

A análise dos gráficos de dispersão permitiu identificar alguns padrões visuais relevantes entre variáveis numéricas do dataset. Estes padrões ajudam a compreender melhor a estrutura dos dados e possíveis relações entre características dos colaboradores e o fenómeno de saída da organização.

**Relação Linear entre Idade e Experiência**

Existe uma relação linear positiva muito forte e perfeitamente visível no gráfico. Como é lógico, à medida que a idade (`Age`) do colaborador avança, o seu total de anos de experiência (`TotalWorkingYears`) também cresce, formando uma linha diagonal densa. Curiosamente, notamos que a esmagadora maioria dos pontos vermelhos (saídas da empresa) concentra-se precisamente na base desta diagonal, ou seja, em colaboradores com baixos valores tanto em `Age` como em `TotalWorkingYears` (jovens com pouca experiência).

**Progressão Financeira por Experiência**

Observa-se uma clara tendência linear ascendente no gráfico cruzado, evidenciando que a empresa recompensa a senioridade. À medida que o total de anos de experiência (`TotalWorkingYears`) de um colaborador aumenta, o seu rendimento mensal (`MonthlyIncome`) acompanha esse crescimento de forma proporcional, formando um padrão visual semelhante a uma "escada". É também notório que a retenção (marcada pelos abundantes pontos azuis) é substancialmente mais robusta nos patamares superiores, quer de experiência, quer de salário.

**Relação entre Variáveis de Antiguidade na Empresa**

Os gráficos `YearsAtCompany` vs `YearsWithCurrManager` e `TotalWorkingYears` vs `YearsAtCompany` evidenciam uma relação positiva entre variáveis associadas à antiguidade do colaborador na organização.

Verifica-se que colaboradores com mais anos na empresa tendem também a apresentar mais anos na função atual ou com o mesmo gestor. Este padrão sugere que estas variáveis capturam dimensões semelhantes da estabilidade profissional dentro da organização.

Esta observação ajuda também a explicar as correlações elevadas identificadas anteriormente na matriz de correlação entre variáveis como  `YearsAtCompany `, `YearsInCurrentRole` e `YearsWithCurrManager`.

### 1.7 Distribuição das Variáveis Numéricas

A análise das variáveis numéricas através de histogramas, boxplots e estatística descritiva revelou diferentes padrões de distribuição no dataset. Dado que várias variáveis apresentam comportamentos semelhantes, optou-se por agrupá-las por tipo de distribuição, o que permite uma interpretação mais clara e evita descrições redundantes.

As variáveis `DailyRate`, `HourlyRate` e `MonthlyRate` apresentam distribuições relativamente uniformes ao longo do intervalo de valores observados, sem concentrações muito acentuadas em regiões específicas. Este comportamento sugere uma dispersão mais equilibrada das taxas administrativas associadas aos colaboradores.

As variáveis `Age`, `DistanceFromHome`, `PercentSalaryHike`, `TrainingTimesLastYear` e `YearsInCurrentRole` apresentam uma assimetria positiva ligeira ou moderada. Isto significa que a maioria das observações se concentra em valores baixos ou intermédios, enquanto valores mais elevados ocorrem com menor frequência.

Por outro lado, as variáveis `MonthlyIncome`, `TotalWorkingYears`, `YearsAtCompany`, `YearsSinceLastPromotion`, `YearsWithCurrManager` e `NumCompaniesWorked` apresentam uma assimetria positiva mais pronunciada, caracterizada por uma forte concentração de observações nos valores mais baixos e uma cauda longa à direita. Este padrão indica que apenas uma pequena fração de colaboradores apresenta salários elevados, muitos anos de experiência ou níveis elevados de antiguidade organizacional.

No conjunto, verifica-se que as variáveis relacionadas com remuneração, progressão de carreira e antiguidade são aquelas que apresentam maior grau de assimetria positiva, enquanto `DailyRate`, `HourlyRate` e `MonthlyRate` evidenciam distribuições mais homogéneas. Estes resultados são consistentes com estruturas organizacionais hierarquizadas, onde a maioria dos colaboradores se concentra em níveis intermédios de carreira, enquanto apenas uma pequena proporção ocupa posições mais séniores ou de maior remuneração.

### 1.8 Distribuição das Variáveis Categóricas

A análise das variáveis categóricas foi realizada através de gráficos de frequência, permitindo compreender a composição estrutural da força de trabalho representada no dataset. Tal como na análise das variáveis numéricas, optou-se por interpretar os padrões de forma agregada, agrupando variáveis com comportamentos semelhantes para facilitar a leitura e evitar descrições repetitivas.

Relativamente à variável alvo 'Attrition', observa-se um claro desequilíbrio entre classes, com predominância de colaboradores que permanecem na organização ('No') face aos que abandonam a empresa ('Yes'). Este padrão confirma a presença de **class imbalance**, um fator relevante que deverá ser considerado na fase de modelação.

No que diz respeito à mobilidade profissional, a variável 'BusinessTravel' apresenta uma forte concentração na categoria 'Travel_Rarely', indicando que a maioria dos colaboradores realiza deslocações profissionais apenas ocasionalmente.

A distribuição da variável 'Department' evidencia uma clara predominância do departamento 'Research & Development', seguido pelo departamento 'Sales', enquanto 'Human Resources' apresenta uma representação significativamente menor. Este padrão sugere uma estrutura organizacional fortemente orientada para atividades técnicas e de investigação.

Relativamente à formação académica, a variável 'EducationField' mostra maior concentração nas áreas 'Life Sciences' e 'Medical', indicando que a maioria dos colaboradores possui formação em áreas científicas ou técnicas.

No plano demográfico, a variável 'Gender' apresenta uma distribuição relativamente equilibrada, embora com ligeira predominância de colaboradores do género masculino. Já a variável 'MaritalStatus' revela maior presença de colaboradores 'Married', seguida das categorias 'Single' e 'Divorced'.

Por fim, a variável 'OverTime' indica que a maioria dos colaboradores não realiza horas extraordinárias, embora exista uma proporção relevante que reporta trabalho extra. Este fator assume particular interesse para a análise da rotatividade, uma vez que poderá estar associado a níveis mais elevados de desgaste profissional.

No conjunto, as variáveis categóricas apresentam distribuições plausíveis e coerentes com a estrutura organizacional representada no dataset, não sendo identificadas categorias com frequências anómalas ou inconsistentes.

## 2. Qualidade dos Dados e Limpeza 
### 2.1. Tratamento de Dados em Falta 
Foi analisada a presença de valores em falta em todas as variáveis do dataset.

A análise revelou que não existem valores nulos nas variáveis consideradas, pelo que não foi necessária a aplicação de técnicas de imputação ou eliminação de observações.

Esta característica aumenta a robustez do processo de modelação, uma vez que reduz o risco de enviesamento introduzido por estratégias artificiais de preenchimento. 
 
### 2.2. Outliers e Inconsistências 

Com o objetivo de identificar possíveis valores atípicos que pudessem enviesar a modelação, foi aplicado o método do Intervalo Interquartil (IQR) às variáveis numéricas do dataset.

A análise revelou a existência de valores extremos em diversas variáveis, nomeadamente:

* `TrainingTimesLastYear`
* `PerformanceRating`
* `MonthlyIncome`
* `YearsSinceLastPromotion`
* `YearsAtCompany`
* `TotalWorkingYears`
* `NumCompaniesWorked`
* `YearsInCurrentRole`
* `YearsWithCurrManager`	

Contudo, após análise detalhada, verificou-se que:

Não foram identificados valores impossíveis (por exemplo: idades irrealistas ou salários negativos);

Algumas variáveis sinalizadas pelo método IQR são discretas ou ordinais (por exemplo: `PerformanceRating`), o que pode originar falsos positivos na deteção de outliers;

Os valores extremos observados em variáveis como `MonthlyIncome` ou `TotalWorkingYears` são plausíveis no contexto organizacional, podendo corresponder a colaboradores com maior antiguidade ou posições de maior responsabilidade.

Deste modo, concluiu-se que os valores identificados representam variação real da população organizacional e não erros de registo ou inconsistências nos dados.

Assim, optou-se por não eliminar observações, preservando a integridade e representatividade do dataset. O eventual impacto destes valores será mitigado na fase de modelação através de técnicas de normalização e da utilização de modelos robustos a valores extremos.

## 3. Engenharia de Atributos (Feature Engineering) 
### 3.1. Transformações Realizadas 
* **Encoding:** (Ex: "Convertemos a variável 'Género' em numérica usando One-Hot Encoding.") 
* **Escalonamento:** (Ex: "Aplicámos o StandardScaler nas variáveis numéricas para que todas 
fiquem na mesma escala.") 
 
### 3.2. Criação de Novos Atributos 
*Descrevam as variáveis que criaram para ajudar o modelo.* 
* **Nova Variável [Nome]:** (Ex: "Criámos a 'Tenure_Per_Year' que divide o tempo de contrato 
pela idade do cliente.") 

 ## 4. Metodologia de Gestão (PBL) 
O projeto segue uma abordagem baseada no modelo CRISP-DM (Cross-Industry Standard Process for Data Mining), que estrutura o desenvolvimento em seis fases principais: compreensão do problema, compreensão dos dados, preparação dos dados, modelação, avaliação e implementação. Embora estas fases tenham uma ordem lógica, o processo é iterativo e permite regressar a fases anteriores sempre que necessário. Esta metodologia assegura uma abordagem estruturada, sistemática e rigorosa ao desenvolvimento de projetos de Ciência de Dados.

**Divisão de Tarefas:** 

 **Luís Figueira:** 
   *  tarefa1
   *  tarefa2
   *  tarefa3
    
 **Martim Ferreira:**
   *  Análise da qualidade dos dados e limpeza
   *  Análise da correlação das variáveis com a variável alvo
   *  tarefa4
  
 **Mateus Afonso:** 
  *  tarefa5
  *  tarefa6
  *  tarefa7

  **Tarefas conjuntas**
   *  tarefa8
   *  tarefa9
   *  tareefa10
   *  tarefa11
     
**Ferramentas de Colaboração:** Kaggle Notebooks (ambiente de desenvolvimento), GitHub (controlo de versões e partilha de código), GitHub Projects (gestão de tarefas), Whatsapp (mensagens diárias), Notion (distribuição de tarefas) e Google Meet (reuniões de dois em dois dias).
 
## 5. Dicionário de Dados Final (Pós-Processamento) 
*Listagem final das variáveis que serão entregues ao modelo na Fase 3. - por confirmar no fim da análise exploratória* 


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
 
## 6. Conclusões da Fase de Exploração 
*O que aprenderam sobre o dataset que não sabiam no final do Milestone 1? Os dados são suficientes 
para avançar para a modelação?* 
 --- 

 ## 7. Referências

 
*Data de última atualização: 11/03/2026*
