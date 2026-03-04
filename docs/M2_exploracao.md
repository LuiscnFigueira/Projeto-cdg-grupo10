# Milestone 2: Análise Exploratória e Engenharia de Atributos 
 
## 1. Análise Exploratória de Dados (EDA) 
### 1.1. Distribuição da Variável Alvo 

Foi analisada a distribuição da variável alvo `Attrition`, que representa o estado de permanência ou saída de cada colaborador. Tratando-se de uma variável categórica binária, o conceito estatístico de distribuição normal não se aplica, baseando-se a nossa análise na proporção de frequências entre as duas categorias.

A análise exploratória revelou um cenário de forte desequilíbrio de classes (class imbalance), com aproximadamente 83.9% das observações correspondentes à classe negativa (permanência) e 16.1% à classe positiva (saída).

Este desequilíbrio constitui um fator crítico no contexto da modelação supervisionada, uma vez que algoritmos treinados diretamente sobre dados desequilibrados tendem a favorecer a classe maioritária. Isto pode apresentar um desempenho aparentemente elevado em termos de acurácia (Accuracy), mas com uma reduzida capacidade de identificação da classe minoritária. Este aspeto será estritamente considerado nas fases subsequentes do processo CRISP-DM, nomeadamente na seleção de métricas de avaliação apropriadas, como Precision, Recall, F1-score e Precision-Recall AUC, e na eventual aplicação de técnicas de balanceamento de classes (ex: SMOTE).
 
### 1.2. Correlações Relevantes

Nesta fase de análise bivariada, utilizámos gráficos de dispersão (scatter plots) para cruzar os atributos do dataset diretamente com a variável alvo `Attrition`, com o objetivo de identificar os principais preditores de rotatividade. Aplicámos um nível de transparência aos pontos do gráfico para revelar a densidade de colaboradores em cada eixo categórico ("Yes" / "No").


* **Atributo Idade (Age) vs. Alvo:** Notámos que a idade apresenta uma forte relação com a probabilidade de saída. Observando o scatter plot, a densidade de pontos na classe "Yes" (abandono) está claramente concentrada na faixa etária mais jovem (entre os 20 e os 35 anos). Por outro lado, a linha correspondente aos colaboradores que permanecem ("No") apresenta uma distribuição muito mais vasta e contínua ao longo de todas as idades, indicando que a retenção é superior em faixas etárias mais maduras.

* gráfico:
  
* referências:

* **Atributo Rendimento Mensal (MonthlyIncome) vs. Alvo:** O fator financeiro está fortemente ligado à saída de colaboradores. No gráfico de dispersão, a esmagadora maioria dos pontos de Attrition ("Yes") aglomera-se de forma densa no limite inferior do eixo X (salários mais baixos). À medida que o rendimento mensal aumenta, a presença de pontos na classe "Yes" torna-se cada vez mais rara, confirmando que pacotes salariais superiores atuam como um forte mecanismo de retenção.

*  gráfico:

*  referências:

### 1.3. Problema de Aprendizagem Supervisionada
A variável alvo do presente projeto, Attrition, indica se o colaborador abandonou a organização (Yes) ou permaneceu (No).

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

## 2. Qualidade dos Dados e Limpeza 
### 2.1. Tratamento de Dados em Falta 
Foi analisada a presença de valores em falta em todas as variáveis do dataset.

A análise revelou que não existem valores nulos nas variáveis consideradas, pelo que não foi necessária a aplicação de técnicas de imputação ou eliminação de observações.

Esta característica aumenta a robustez do processo de modelação, uma vez que reduz o risco de enviesamento introduzido por estratégias artificiais de preenchimento. 
 
### 2.2. Outliers e Inconsistências 

Com o objetivo de identificar possíveis valores atípicos que pudessem enviesar a modelação, foi aplicado o método do Intervalo Interquartil (IQR) às variáveis numéricas do dataset.

A análise revelou a existência de valores extremos em diversas variáveis, nomeadamente:

* TrainingTimesLastYear
* PerformanceRating
* MonthlyIncome
* YearsSinceLastPromotion
*YearsAtCompany
*TotalWorkingYears
* NumCompaniesWorked
* YearsInCurrentRole
* YearsWithCurrManager	

Contudo, após análise detalhada, verificou-se que:

Não foram identificados valores impossíveis (por exemplo: idades irrealistas ou salários negativos);

Algumas variáveis sinalizadas pelo método IQR são discretas ou ordinais (por exemplo: PerformanceRating), o que pode originar falsos positivos na deteção de outliers;

Os valores extremos observados em variáveis como MonthlyIncome ou TotalWorkingYears são plausíveis no contexto organizacional, podendo corresponder a colaboradores com maior antiguidade ou posições de maior responsabilidade.

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
 
## 4. Dicionário de Dados Final (Pós-Processamento) 
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
 
## 5. Conclusões da Fase de Exploração 
*O que aprenderam sobre o dataset que não sabiam no final do Milestone 1? Os dados são suficientes 
para avançar para a modelação?* 
 --- 
*Data de última atualização: 03/03/2026*
