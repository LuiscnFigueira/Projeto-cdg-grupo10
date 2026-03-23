# Milestone 2: Análise Exploratória e Engenharia de Atributos 
 
## 1. Análise Exploratória de Dados (EDA) 
### 1.1. Distribuição da Variável Alvo 

A variável alvo do presente projeto, `Attrition`, indica se o colaborador abandonou a organização (Yes) ou permaneceu na empresa (No). Trata-se de uma variável categórica binária que representa o fenómeno de rotatividade de colaboradores e constitui o principal objeto de análise deste estudo.

Foi realizada uma análise da distribuição desta variável com o objetivo de compreender a proporção de colaboradores que permanecem na organização face aos que abandonam a empresa. Dado tratar-se de uma variável categórica binária, o conceito estatístico de distribuição normal não é aplicável, sendo a análise baseada na proporção de frequências entre as duas categorias.

A análise revelou a seguinte distribuição:

* 83.9% dos colaboradores permaneceram na empresa

* 16.1% dos colaboradores abandonaram a organização

Estes resultados evidenciam um desequilíbrio significativo entre classes, sendo a classe “Yes” (saída) claramente minoritária.

### Desafios Técnicos Resultantes do Desequilíbrio de Classes

O desequilíbrio entre classes constitui um desafio relevante no contexto da modelação preditiva supervisionada. Em datasets desequilibrados, algoritmos de classificação podem tender a favorecer a classe maioritária, conduzindo a modelos que apresentam valores elevados de accuracy, mas com reduzida capacidade de identificar corretamente a classe minoritária.

Por exemplo, um modelo que classificasse todos os colaboradores como “No” obteria uma precisão aparente de aproximadamente 84%, apesar de não conseguir identificar corretamente os casos de abandono.

Do ponto de vista organizacional, esta limitação é particularmente crítica, uma vez que o abandono de colaboradores representa custos significativos associados ao recrutamento, integração e perda de conhecimento organizacional.

Deste modo, nas fases subsequentes do projeto serão adotadas estratégias adequadas para lidar com o desequilíbrio de classes, nomeadamente:

* utilização de métricas de avaliação apropriadas, como Precision, Recall, F1-score e ROC-AUC.

* eventual aplicação de técnicas de balanceamento de classes (por exemplo, SMOTE).

* análise detalhada da matriz de confusão para avaliar o desempenho do modelo na identificação da classe minoritária.
 
### 1.2. Correlações Relevantes

Nesta fase de análise bivariada, utilizámos gráficos de dispersão (scatter plots) para cruzar os atributos do dataset diretamente com a variável alvo `Attrition`, com o objetivo de identificar os principais preditores de rotatividade. Aplicámos um nível de transparência aos pontos do gráfico para revelar a densidade de colaboradores em cada eixo categórico ("Yes" / "No").


* Atributo Idade (`Age`) vs. Variável Alvo (`Attrition`): Notámos que a idade apresenta uma forte relação com a probabilidade de saída. Observando o scatter plot, a densidade de pontos na classe "Yes" (abandono) está claramente concentrada na faixa etária mais jovem (entre os 20 e os 35 anos). Por outro lado, a linha correspondente aos colaboradores que permanecem ("No") apresenta uma distribuição muito mais vasta e contínua ao longo de todas as idades, indicando que a retenção é superior em faixas etárias mais maduras.

* Atributo Rendimento Mensal (`MonthlyIncome`) vs. Variável Alvo (`Attrition`): O fator financeiro está fortemente ligado à saída de colaboradores. No gráfico de dispersão, a esmagadora maioria dos pontos de Attrition ("Yes") aglomera-se de forma densa no limite inferior do eixo X (salários mais baixos). À medida que o rendimento mensal aumenta, a presença de pontos na classe "Yes" torna-se cada vez mais rara, confirmando que pacotes salariais superiores atuam como um forte mecanismo de retenção.

* Atributo Experiência Total (`TotalWorkingYears`) vs. Variável Alvo (`Attrition`): Notámos que a senioridade e o tempo de carreira estão intimamente ligados à retenção. Observando o scatter plot, a grande mancha de densidade de abandonos ("Yes") concentra-se nos colaboradores com menos de 10 anos de experiência total. Em contrapartida, profissionais com carreiras mais longas (especialmente acima dos 15-20 anos) apresentam uma dispersão residual na linha de saída, provando que a consolidação da carreira reduz drasticamente a rotatividade.

*  gráficos:

*  referências:

### 1.3 Matriz de Correlação (Heatmap)

Foi gerada uma matriz de correlação para as variáveis numéricas, com o objetivo de identificar relações lineares relevantes e potenciais situações de multicolinearidade. A análise do heatmap evidenciou três padrões principais:

* Observou-se uma correlação muito elevada entre `JobLevel` e `MonthlyIncome` (≈ 0.95), sugerindo redundância informacional entre progressão hierárquica e remuneração.

* Verificou-se um conjunto de variáveis relacionadas com antiguidade na empresa com correlações elevadas, nomeadamente `YearsAtCompany` com `YearsInCurrentRole` (≈ 0.76) e com `YearsWithCurrManager` (≈ 0.77), indicando sobreposição na medição de estabilidade/tenure.

* Relativamente à variável-alvo (`Attrition_bin`), a associação linear mais evidente ocorre com `OverTime_bin` (≈ 0.25). Adicionalmente, `Age`, `MonthlyIncome`, `JobLevel` e `TotalWorkingYears` apresentam correlações negativas moderadas (≈ -0.16 a -0.17), sugerindo maior probabilidade de saída em colaboradores mais jovens e em níveis salariais/hierárquicos mais baixos.

Estes resultados serão considerados na fase de modelação, particularmente na seleção de atributos e na mitigação de multicolinearidade. Importa referir que correlação não implica causalidade, representando apenas associações lineares observadas nos dados.

 ### 1.4 Análise da Correlação com a Variável Alvo

Para compreender quais variáveis apresentam maior associação com a saída de colaboradores, foi analisada a correlação entre os atributos numéricos e a variável alvo `Attrition_bin`.

Os resultados evidenciam que `OverTime_bin` apresenta a maior correlação positiva (≈ 0.25), sugerindo que colaboradores que realizam horas extraordinárias apresentam maior probabilidade de abandonar a organização. Também se observa uma correlação positiva moderada em `DistanceFromHome`, indicando que colaboradores que vivem mais afastados podem ter maior propensão para saída.

Por outro lado, variáveis relacionadas com experiência profissional e progressão na carreira apresentam correlação negativa moderada com a variável alvo, incluindo `TotalWorkingYears`, `JobLevel`, `MonthlyIncome`, `Age` e `YearsInCurrentRole`. Este padrão sugere que colaboradores mais experientes, com maior remuneração ou posição hierárquica mais elevada tendem a permanecer na organização.

Por fim, algumas variáveis apresentam correlação praticamente nula com a saída, como `PerformanceRating`, `PercentSalaryHike` e `HourlyRate`, indicando que estes fatores não parecem influenciar diretamente a decisão de abandono.

Importa salientar que correlação mede apenas associações lineares e não implica causalidade.

Estes resultados sugerem que fatores relacionados com carga de trabalho, progressão na carreira e remuneração desempenham um papel relevante na retenção de talento.

### 1.5 Conclusões Visuais da Análise Bivariada

A análise dos gráficos de dispersão permitiu identificar alguns padrões visuais relevantes entre variáveis numéricas do dataset. Estes padrões ajudam a compreender melhor a estrutura dos dados e possíveis relações entre características dos colaboradores e o fenómeno de saída da organização.

**Relação Linear entre Idade e Experiência**

Existe uma relação linear positiva muito forte e perfeitamente visível no gráfico. Verificámos que à medida que a idade (`Age`) do colaborador aumenta, o seu total de anos de experiência (`TotalWorkingYears`) também cresce, formando uma linha diagonal densa. Curiosamente, notamos que a esmagadora maioria dos pontos vermelhos (saídas da empresa) concentra-se precisamente na base desta diagonal, ou seja, em colaboradores com baixos valores tanto em `Age` como em `TotalWorkingYears` (jovens com pouca experiência).

**Progressão Financeira por Experiência**

Observa-se uma clara tendência linear ascendente no gráfico cruzado, evidenciando que a empresa recompensa a senioridade. À medida que o total de anos de experiência (`TotalWorkingYears`) de um colaborador aumenta, o seu rendimento mensal (`MonthlyIncome`) acompanha esse crescimento de forma proporcional, formando um padrão visual semelhante a uma "escada". É também notório que a retenção (marcada pelos abundantes pontos azuis) é substancialmente mais robusta nos patamares superiores, quer de experiência, quer de salário.

**Relação entre Variáveis de Antiguidade na Empresa**

Os gráficos `YearsAtCompany` vs `YearsWithCurrManager` e `TotalWorkingYears` vs `YearsAtCompany` evidenciam uma relação positiva entre variáveis associadas à antiguidade do colaborador na organização.

Verifica-se que colaboradores com mais anos na empresa tendem também a apresentar mais anos na função atual ou com o mesmo gestor. Este padrão sugere que estas variáveis capturam dimensões semelhantes da estabilidade profissional dentro da organização.

Esta observação ajuda também a explicar as correlações elevadas identificadas anteriormente na matriz de correlação entre variáveis como  `YearsAtCompany`, `YearsInCurrentRole` e `YearsWithCurrManager`.

### 1.6 Distribuição das Variáveis Numéricas

A análise das variáveis numéricas através de histogramas, boxplots e estatística descritiva revelou diferentes padrões de distribuição no dataset. Dado que várias variáveis apresentam comportamentos semelhantes, optou-se por agrupá-las por tipo de distribuição, o que permite uma interpretação mais clara e evita descrições redundantes.

As variáveis `DailyRate`, `HourlyRate` e `MonthlyRate` apresentam distribuições relativamente uniformes ao longo do intervalo de valores observados, sem concentrações muito acentuadas em regiões específicas. Este comportamento sugere uma dispersão mais equilibrada das taxas administrativas associadas aos colaboradores.

As variáveis `Age`, `DistanceFromHome`, `PercentSalaryHike`, `TrainingTimesLastYear` e `YearsInCurrentRole` apresentam uma assimetria positiva ligeira ou moderada. Isto significa que a maioria das observações se concentra em valores baixos ou intermédios, enquanto valores mais elevados ocorrem com menor frequência.

Por outro lado, as variáveis `MonthlyIncome`, `TotalWorkingYears`, `YearsAtCompany`, `YearsSinceLastPromotion`, `YearsWithCurrManager` e `NumCompaniesWorked` apresentam uma assimetria positiva mais pronunciada, caracterizada por uma forte concentração de observações nos valores mais baixos e uma cauda longa à direita. Este padrão indica que apenas uma pequena fração de colaboradores apresenta salários elevados, muitos anos de experiência ou níveis elevados de antiguidade organizacional.

No conjunto, verifica-se que as variáveis relacionadas com remuneração, progressão de carreira e antiguidade são aquelas que apresentam maior grau de assimetria positiva, enquanto `DailyRate`, `HourlyRate` e `MonthlyRate` evidenciam distribuições mais homogéneas. Estes resultados são consistentes com estruturas organizacionais hierarquizadas, onde a maioria dos colaboradores se concentra em níveis intermédios de carreira, enquanto apenas uma pequena proporção ocupa posições mais séniores ou de maior remuneração.

### 1.7 Distribuição das Variáveis Categóricas

A análise das variáveis categóricas foi realizada através de gráficos de frequência, permitindo compreender a composição estrutural da força de trabalho representada no dataset. Tal como na análise das variáveis numéricas, optou-se por interpretar os padrões de forma agregada, agrupando variáveis com comportamentos semelhantes para facilitar a leitura e evitar descrições repetitivas.

Relativamente à variável alvo `Attrition`, observa-se um claro desequilíbrio entre classes, com predominância de colaboradores que permanecem na organização (`No`) face aos que abandonam a empresa (`Yes`). Este padrão confirma a presença de desbalanceamento de classes (`class imbalance`), um fator relevante que deverá ser considerado na fase de modelação.

No que diz respeito à mobilidade profissional, a variável `BusinessTravel` apresenta uma forte concentração na categoria `Travel_Rarely`, indicando que a maioria dos colaboradores realiza deslocações profissionais apenas ocasionalmente.

A distribuição da variável `Department` evidencia uma clara predominância do departamento Investigação e Desenvolvimento (`Research & Development`), seguido pelo departamento Vendas (`Sales`), enquanto Recursos Humanos (`Human Resources`) apresenta uma representação significativamente menor. Este padrão sugere uma estrutura organizacional fortemente orientada para atividades técnicas e de investigação.

Relativamente à formação académica, a variável `EducationField` mostra maior concentração nas áreas `Life Sciences` e `Medical`, indicando que a maioria dos colaboradores possui formação em áreas científicas ou técnicas.

No plano demográfico, a variável `Gender` apresenta uma distribuição relativamente equilibrada, embora com ligeira predominância de colaboradores do género masculino. Já a variável `MaritalStatus` revela maior presença de colaboradores `Married`, seguida das categorias `Single` e `Divorced`.

Por fim, a variável `OverTime` indica que a maioria dos colaboradores não realiza horas extraordinárias, embora exista uma proporção relevante que reporta trabalho extra. Este fator assume particular interesse para a análise da rotatividade, uma vez que poderá estar associado a níveis mais elevados de desgaste profissional.

No conjunto, as variáveis categóricas apresentam distribuições plausíveis e coerentes com a estrutura organizacional representada no dataset, não sendo identificadas categorias com frequências anómalas ou inconsistentes.

### 1.8 Resposta à 2ª Pergunta de Investigação

Para avaliar se existe uma associação estatisticamente significativa entre a realização de horas extraordinárias (`OverTime`) e a probabilidade de saída da organização (`Attrition`), foi aplicado o teste do qui-quadrado de independência, adequado para analisar relações entre variáveis categóricas.

A análise da tabela de contingência revelou diferenças relevantes na proporção de saída entre colaboradores que realizam horas extraordinárias e aqueles que não realizam.

Em particular, observou-se que:

* 10.44% dos colaboradores que não realizam horas extraordinárias abandonaram a organização;

* 30.53% dos colaboradores que realizam horas extraordinárias abandonaram a organização.

Este resultado sugere que colaboradores que realizam horas extraordinárias apresentam uma probabilidade substancialmente superior de saída da empresa.

Para verificar se esta diferença é estatisticamente significativa, foi aplicado o teste do qui-quadrado de independência, que produziu os seguintes resultados:

χ² = 87.56

p-value < 0.001

graus de liberdade = 1

Dado que o p-value é inferior ao nível de significância de 0.05, rejeita-se a hipótese nula de independência entre as variáveis. Assim, conclui-se que existe uma associação estatisticamente significativa entre a realização de horas extraordinárias (`OverTime`) e a variável alvo (`Attrition`).

Adicionalmente, foi calculado o tamanho do efeito através da medida Cramér’s V, que apresentou um valor de 0.24, indicando uma associação de intensidade fraca a moderada entre as duas variáveis. Isto significa que, embora a realização de horas extraordinárias esteja relacionada com o abandono da organização, este fenómeno é provavelmente influenciado também por outros fatores presentes no dataset.

Do ponto de vista organizacional, estes resultados sugerem que a realização frequente de horas extraordinárias pode estar associada a níveis mais elevados de desgaste profissional, carga de trabalho ou insatisfação laboral, fatores que podem contribuir para o aumento da rotatividade de colaboradores.

### 1.9 Análise Estatística Inferencial

Para além da análise exploratória, foi realizado um conjunto de testes estatísticos com o objetivo de verificar, de forma mais rigorosa, quais as variáveis que apresentam uma relação real com a probabilidade de saída dos colaboradores. A metodologia seguiu uma abordagem faseada, passando pela verificação prévia da normalidade dos dados, pela escolha do teste mais adequado com base nessa verificação e pelo cálculo do tamanho do efeito para avaliar a importância prática de cada resultado (James et al., 2021).

Esta última medida é importante porque um teste estatístico pode assinalar uma diferença como significativa apenas por a amostra ser grande, mesmo que essa diferença seja muito pequena na prática. O tamanho do efeito ajuda a perceber se a diferença encontrada é, de facto, relevante.

#### 1.9.1 Verificação de Normalidade - Teste de Shapiro-Wilk

Antes de aplicar os testes de comparação entre grupos, foi verificado se os dados de cada variável numérica seguem uma distribuição normal, analisando separadamente os colaboradores que saíram e os que ficaram. O teste de Shapiro-Wilk foi escolhido por ser considerado um dos mais adequados para este tipo de verificação.

Os resultados mostraram que nenhuma variável numérica segue uma distribuição normal em nenhum dos dois grupos, o que determinou a utilização exclusiva de testes não-paramétricos nas análises seguintes. Os testes não-paramétricos são uma alternativa que não exige que os dados sigam uma distribuição normal.

#### 1.9.2 Variáveis Numéricas - Teste de Mann-Whitney U

Uma vez que os dados não seguem uma distribuição normal, foi aplicado o teste de Mann-Whitney U para comparar cada variável numérica entre os colaboradores que saíram e os que ficaram. Este teste verifica se os dois grupos apresentam distribuições diferentes de forma estatisticamente significativa, sem exigir que os dados sigam uma distribuição normal (James et al., 2021). Para medir a importância prática das diferenças encontradas, foi calculado o d de Cohen, uma medida que indica se a diferença entre os dois grupos é grande ou pequena em termos reais.

As variáveis que melhor distinguem os colaboradores que saem dos que ficam são `TotalWorkingYears`, `YearsInCurrentRole`, `MonthlyIncome`, `Age`, `YearsWithCurrManager` e `YearsAtCompany`, todas com diferenças estatisticamente significativas e com um tamanho de efeito considerado pequeno. A variável `DistanceFromHome` apresentou também uma diferença significativa, com os colaboradores que saem a residir em média mais longe do local de trabalho. As variáveis `DailyRate`, `YearsSinceLastPromotion` e `TrainingTimesLastYear` mostraram diferenças estatisticamente significativas mas com um tamanho de efeito muito pequeno, o que limita a sua relevância do ponto de vista prático. Por fim, as variáveis `NumCompaniesWorked`, `PercentSalaryHike`, `MonthlyRate` e `HourlyRate` não apresentaram diferenças significativas entre os dois grupos, pelo que não parecem contribuir para explicar o fenómeno de attrition neste dataset.

#### 1.9.3 Variáveis Categóricas - Teste do Qui-Quadrado e Cramér's V

Para as variáveis categóricas, foi aplicado o teste do qui-quadrado de independência, que permite verificar se existe uma associação entre duas variáveis categóricas ou se essas variáveis são independentes entre si (James et al., 2021). Para medir a intensidade dessa associação, foi calculado o Cramér's V, uma medida que indica o grau de relação entre as duas variáveis de forma comparável entre diferentes pares.

Entre as variáveis categóricas, `OverTime` e `JobRole` apresentam as associações mais fortes com `Attrition`, ambas estatisticamente significativas e com uma força de associação classificada como fraca. `MaritalStatus` surge em terceiro lugar, seguida de `BusinessTravel` e `EducationField`, todas com associações estatisticamente significativas. O `Department` apresenta uma associação muito fraca, ainda que significativa. A variável `Gender` é a única que não revela qualquer associação estatisticamente significativa com a variável alvo, confirmando que o género não é um fator relevante para explicar a saída de colaboradores neste contexto.

#### 1.9.4 Variáveis Ordinais - Teste de Kruskal-Wallis

As variáveis com escalas ordenadas, como os níveis de satisfação e o nível hierárquico, foram analisadas com o teste de Kruskal-Wallis, que permite comparar as distribuições de mais de dois grupos sem exigir que os dados sigam uma distribuição normal (James et al., 2021).

`JobLevel` e `StockOptionLevel` revelaram-se as variáveis com maior diferença entre os grupos, sugerindo que tanto a posição hierárquica como o acesso a opções sobre ações estão associados à probabilidade de saída. A dimensão `EnvironmentSatisfaction` apresentou também uma diferença significativa entre os dois grupos. `JobSatisfaction` e `WorkLifeBalance` revelaram igualmente diferenças estatisticamente significativas, embora o tamanho do efeito seja muito reduzido em ambos os casos, o que limita a sua relevância prática.

Em sentido contrário, `RelationshipSatisfaction` e `Education` não mostraram diferenças relevantes entre os colaboradores que saem e os que ficam. O resultado de `PerformanceRating` é particularmente expressivo, com o valor do teste praticamente nulo, confirmando que a avaliação de desempenho não tem qualquer relação com a probabilidade de abandono, o que é consistente com o que foi observado anteriormente na análise de correlação e com o que a literatura refere sobre a baixa capacidade preditiva das avaliações de desempenho formais neste tipo de contexto (James et al., 2021).

#### 1.9.5 Síntese dos Testes Estatísticos

Os testes realizados permitiram identificar quais as variáveis com maior relevância para prever a saída de colaboradores. As que apresentam uma associação mais forte e consistente com `Attrition` são `TotalWorkingYears`, `MonthlyIncome`, `YearsInCurrentRole`, `Age`, `YearsWithCurrManager`, `OverTime` e `JobRole`, que deverão ter um papel central nos modelos a desenvolver na fase seguinte.

As variáveis que não apresentaram associação significativa com a saída em nenhum dos testes - `Gender`, `PerformanceRating`, `PercentSalaryHike`, `MonthlyRate`, `HourlyRate`, `RelationshipSatisfaction` e `Education` - serão avaliadas com maior cuidado na fase de modelação. A sua relevância não deve ser descartada com base apenas nos testes de associação bivariada, uma vez que uma variável pode não mostrar relação direta com a variável alvo mas ainda assim contribuir em combinação com outras variáveis dentro de um modelo (Géron, 2022). A decisão sobre quais variáveis incluir será tomada com base na importância que os próprios modelos atribuírem a cada variável durante o treino, na Milestone 3.

Para além da seleção de variáveis, há outros desafios a resolver na fase de modelação: o desequilíbrio entre classes da variável alvo, que pode enviesar os modelos para a classe maioritária, e a multicolinearidade elevada entre algumas variáveis, que pode afetar o desempenho de certos algoritmos. Ambos serão tratados com estratégias adequadas ao tipo de modelo utilizado.

## 2. Qualidade dos Dados e Limpeza 
### 2.1. Tratamento de Dados em Falta 

Com o objetivo de avaliar a qualidade do dataset, foi realizada uma análise à presença de valores em falta em todas as variáveis consideradas.

Os resultados obtidos indicaram que não existem valores em falta no dataset, não sendo, portanto, necessária a aplicação de técnicas de imputação ou a eliminação de observações.

Esta ausência constitui uma vantagem para o processo de preparação e modelação dos dados, uma vez que reduz a necessidade de intervenções artificiais no dataset e minimiza o risco de introdução de enviesamento associado a estratégias de imputação. Em contextos de aprendizagem automática, a presença de valores em falta pode comprometer a qualidade das inferências e afetar o desempenho dos modelos preditivos, exigindo a aplicação de técnicas específicas de tratamento e pré-processamento dos dados (Géron, 2022; James et al., 2021).

Assim, foi possível prosseguir diretamente para as etapas subsequentes de análise de outliers e verificação da consistência dos dados.

### 2.2 Outliers

Com o objetivo de identificar possíveis valores atípicos nas variáveis numéricas do dataset, foi aplicado o método do Intervalo Interquartil (_Interquartile Range_ – IQR). Este método permite detetar observações que se encontram significativamente afastadas da distribuição central dos dados, sendo amplamente utilizado em análise exploratória para a identificação de valores extremos (Rousseeuw & Hubert, 2011).

A aplicação do método sinalizou potenciais outliers em várias variáveis, nomeadamente: `TrainingTimesLastYear`, `PerformanceRating`, `MonthlyIncome`, `YearsSinceLastPromotion`, `YearsAtCompany`, `TotalWorkingYears`, `NumCompaniesWorked`, `YearsInCurrentRole` e `YearsWithCurrManager`.

Contudo, uma análise mais detalhada revelou que estes valores não correspondem necessariamente a erros de registo ou inconsistências nos dados. Em primeiro lugar, não foram identificados valores impossíveis, como idades irrealistas ou valores negativos de rendimento. Em segundo lugar, algumas das variáveis identificadas são de natureza discreta ou ordinal, como `PerformanceRating`, o que pode originar deteções de outliers que não representam necessariamente anomalias reais.

Adicionalmente, valores elevados em variáveis como `MonthlyIncome` ou `TotalWorkingYears` são plausíveis no contexto organizacional, podendo corresponder a colaboradores com maior experiência profissional ou posições hierárquicas mais elevadas. Em contextos organizacionais, a presença de valores extremos pode refletir a heterogeneidade natural da população analisada e não erros nos dados (Rousseeuw & Hubert, 2011).

Deste modo, concluiu-se que os valores identificados refletem a variabilidade natural da população organizacional representada no dataset. Assim, optou-se por não remover estas observações, preservando a integridade e a representatividade dos dados para as etapas subsequentes de análise e modelação.

### 2.3 Verificação da Qualidade e Consistência dos Dados

Antes de proceder às etapas de transformação das variáveis e de criação de novos atributos, foi realizada uma verificação preliminar da qualidade e consistência dos dados presentes no dataset. Esta etapa tem como objetivo identificar possíveis valores inválidos, inconsistências lógicas entre variáveis ou padrões anómalos que possam comprometer a fiabilidade das análises subsequentes. A verificação da qualidade dos dados constitui uma fase essencial no processo de preparação de dados, uma vez que modelos de aprendizagem automática são particularmente sensíveis a erros ou incoerências presentes no conjunto de dados (Géron, 2022).

Numa primeira fase, foi analisada a presença de valores iguais a zero em variáveis associadas à experiência profissional dos colaboradores. Foram consideradas as variáveis de experiência profissional `TotalWorkingYears`, `YearsAtCompany`, `YearsInCurrentRole`, `YearsSinceLastPromotion` e `YearsWithCurrManager`. A análise revelou a existência de algumas observações com valor zero nestas variáveis. Em particular, foram identificadas 11 ocorrências em `TotalWorkingYears`, 44 em `YearsAtCompany`, 244 em `YearsInCurrentRole`, 581 em `YearsSinceLastPromotion` e 263 em `YearsWithCurrManager`. Estes valores não representam necessariamente erros nos dados, podendo corresponder a situações plausíveis, como colaboradores recentemente contratados, colaboradores que iniciaram recentemente uma nova função ou casos em que ocorreu uma promoção recente.

De seguida, foi analisada a variável `Age`, com o objetivo de identificar possíveis idades inválidas. Em particular, verificou-se a existência de colaboradores com idade inferior a 18 anos, uma vez que tal situação seria inconsistente com o contexto laboral considerado. Não foram identificadas observações com idade inferior a este limiar, indicando que os valores registados para a idade se encontram dentro de intervalos plausíveis.

Foi também analisada a coerência entre a idade dos colaboradores e a sua experiência profissional total. Para tal, verificaram-se situações em que `TotalWorkingYears` seria superior ao valor registado em `Age`, o que representaria uma inconsistência lógica. Os resultados obtidos indicam que não existem ocorrências deste tipo no dataset, sugerindo que os valores relativos à experiência profissional são compatíveis com a idade dos colaboradores.

Adicionalmente, foram avaliadas possíveis inconsistências entre variáveis temporais relacionadas com o percurso profissional dos colaboradores. Em particular, verificou-se se o número de anos na função atual `YearsInCurrentRole`, o número de anos desde a última promoção `YearsSinceLastPromotion` ou o número de anos com o gestor atual `YearsWithCurrManager` excediam o número total de anos na empresa `YearsAtCompany`. Foi igualmente analisada a relação entre estas variáveis e a experiência profissional total `TotalWorkingYears`, dado que o tempo em funções específicas ou na própria organização não pode exceder a experiência profissional total de um colaborador. Não foram identificadas observações que violem estas relações lógicas, indicando consistência global entre as variáveis de experiência presentes no dataset.

Por fim, foi realizada uma análise descritiva da variável `MonthlyIncome` em função do nível hierárquico `JobLevel`. Esta análise teve como objetivo avaliar a coerência da estrutura salarial presente no dataset. Os resultados mostram uma progressão consistente da remuneração média à medida que aumenta o nível hierárquico. O nível 1 apresenta um rendimento médio mensal de aproximadamente 2786.92, enquanto os níveis superiores apresentam valores progressivamente mais elevados, atingindo cerca de 19191.83 no nível 5. Esta distribuição confirma a existência de uma estrutura salarial coerente com a hierarquia organizacional representada nos dados.

Em síntese, a verificação realizada não revelou inconsistências críticas ou valores inválidos que comprometam a análise subsequente. As variáveis temporais apresentam relações lógicas coerentes e a distribuição salarial mostra-se consistente com a estrutura hierárquica da organização. Deste modo, foi possível avançar para a etapa seguinte de preparação dos dados, nomeadamente a transformação e codificação das variáveis categóricas.

## 3. Engenharia de Atributos (Feature Engineering) 
### 3.1 Transformação e Codificação das Variáveis Categóricas

No âmbito da fase de preparação dos dados, foi necessário proceder à transformação das variáveis categóricas presentes no dataset para representações numéricas. Esta transformação é essencial no contexto da modelação preditiva, uma vez que a maioria dos algoritmos de aprendizagem automática opera exclusivamente sobre variáveis quantitativas e não consegue processar diretamente valores de natureza textual (Géron, 2022).

A estratégia adotada teve em consideração a natureza das variáveis categóricas existentes no dataset, distinguindo entre variáveis binárias e variáveis nominais com múltiplas categorias.

No caso das variáveis categóricas binárias, foi aplicada uma codificação numérica simples baseada em variáveis indicadoras. Este procedimento consistiu na conversão das categorias de texto em valores binários (0 e 1), permitindo representar cada variável de forma compacta e interpretável. Foram transformadas três variáveis deste tipo: `Attrition`, `OverTime` e `Gender`.

A variável `Attrition`, que constitui a variável alvo do estudo, foi convertida numa nova variável denominada `Attrition_bin`. Nesta representação, o valor 1 indica que o colaborador abandonou a organização (`Yes`), enquanto o valor 0 indica que o colaborador permaneceu na empresa (`No`). Esta transformação permite representar diretamente o fenómeno de rotatividade de colaboradores numa escala binária adequada para problemas de classificação supervisionada.

De forma semelhante, a variável `OverTime` foi convertida na variável `OverTime_bin`, assumindo o valor 1 quando o colaborador realiza horas extraordinárias (`Yes`) e 0 quando não realiza (`No`). Esta transformação permite representar a presença ou ausência de trabalho extraordinário de forma quantitativa, possibilitando a sua utilização em análises estatísticas e modelos preditivos.

Por sua vez, a variável `Gender` foi transformada na variável `Gender_bin`, assumindo o valor 1 para colaboradores do género masculino (`Male`) e 0 para colaboradores do género feminino (`Female`). Embora esta variável seja categórica, a sua natureza binária permite uma representação numérica direta sem perda de informação.

Para as restantes variáveis categóricas, que apresentam mais de duas categorias e não possuem uma ordem natural entre si, foi aplicada a técnica de One-Hot Encoding. Este método consiste em criar uma variável binária independente para cada categoria existente, permitindo representar cada categoria de forma explícita no espaço de características (James et al., 2021).

A aplicação desta técnica foi realizada nas variáveis `BusinessTravel`, `Department`, `EducationField`, `JobRole` e `MaritalStatus`. Neste processo, cada categoria passou a ser representada por uma nova variável indicadora que assume o valor 1 quando a observação pertence à categoria correspondente e 0 caso contrário.

Por exemplo, a variável `MaritalStatus`, originalmente composta pelas categorias `Single`, `Married` e `Divorced`, foi transformada em três variáveis binárias distintas que indicam explicitamente se o colaborador pertence a cada uma destas categorias. De forma análoga, as restantes variáveis categóricas foram expandidas em múltiplas variáveis indicadoras, permitindo representar adequadamente as diferentes categorias presentes no dataset.

A utilização do One-Hot Encoding evita a introdução de relações ordinais artificiais entre categorias, assegurando que os modelos de aprendizagem automática tratam cada categoria como uma entidade independente (Géron, 2022).

Como consequência deste processo de codificação, o número total de variáveis do dataset aumentou, uma vez que cada categoria passou a ser representada por uma variável binária própria. Apesar deste aumento na dimensionalidade do espaço de características, esta abordagem permite preservar integralmente a informação contida nas variáveis categóricas e garante que o dataset se encontra preparado para a aplicação de técnicas estatísticas, análise de correlação e algoritmos de aprendizagem automática nas fases subsequentes do projeto.

**Escalonamento**

Nesta fase não foi aplicado qualquer método de escalonamento às variáveis numéricas. Esta decisão deve-se ao facto de, nas etapas subsequentes de modelação, serem utilizados diferentes algoritmos de aprendizagem automática, os quais podem apresentar requisitos distintos em termos de pré-processamento dos dados.

Em particular, alguns algoritmos (como modelos baseados em árvores de decisão) são invariantes à escala das variáveis, enquanto outros (como regressão logística, SVM ou métodos baseados em distância) beneficiam da aplicação de técnicas de normalização ou padronização. Deste modo, o escalonamento será realizado numa fase posterior do projeto, de forma específica e adequada a cada modelo considerado (Géron, 2022).
 
### 3.2 Criação de Novos Atributos 

No âmbito da fase de preparação dos dados, foi realizada uma etapa de engenharia de atributos (*feature engineering*) com o objetivo de enriquecer o espaço de características do dataset. Esta abordagem consiste na criação de novas variáveis derivadas a partir de variáveis já existentes, permitindo representar de forma mais direta determinadas dimensões organizacionais e comportamentais potencialmente associadas ao fenómeno de attrition.

A criação de atributos derivados constitui uma prática comum em projetos de ciência de dados, uma vez que a transformação e combinação de variáveis existentes pode aumentar a capacidade explicativa dos modelos e facilitar a identificação de padrões relevantes nos dados (Géron, 2022; James et al., 2021).

Neste contexto, foram criadas quatro novas variáveis: `RatioYearsInRole`, `SatisfactionIndex`, `CareerStagnation` e `IncomePerLevel`.

**RatioYearsInRole**

A variável `RatioYearsInRole` representa a proporção do tempo que um colaborador permanece na sua função atual relativamente ao tempo total de permanência na empresa. A literatura empírica demonstra que a mobilidade interna está associada a menores níveis de turnover, uma vez que a diversidade de experiências e oportunidades de progressão contribui para maior envolvimento organizacional (Dalton & Todor, 1987).

Com base neste princípio, foi construída uma medida objetiva de mobilidade interna, calculada como a razão entre `YearsInCurrentRole` e `YearsAtCompany`. Valores próximos de 1 indicam que o colaborador passou a maior parte do seu tempo na empresa na mesma função, enquanto valores mais baixos refletem maior progressão ou mudança funcional. Para evitar situações de divisão por zero, sempre que `YearsAtCompany` assume valor igual a zero, o indicador é definido como 0.

A operacionalização específica deste rácio foi desenvolvida para este projeto, não existindo na literatura uma fórmula padronizada para medir diretamente esta proporção com base em dados administrativos de recursos humanos.

**SatisfactionIndex**

Foi construído um índice composto designado `SatisfactionIndex`, com o objetivo de sintetizar o nível global de satisfação profissional do colaborador. A literatura demonstra que a satisfação no trabalho está negativamente associada à intenção de saída, sendo relevante considerar múltiplas dimensões de satisfação de forma integrada (Tett & Meyer, 1993). Com base neste fundamento, o índice combina as quatro variáveis presentes no dataset que captam estas dimensões: `JobSatisfaction`, `EnvironmentSatisfaction`, `RelationshipSatisfaction` e `WorkLifeBalance`.

Estas variáveis utilizam uma escala ordinal de quatro níveis, em que valores mais elevados correspondem a níveis superiores de satisfação. A construção do índice baseou-se na contagem da frequência de cada nível de satisfação nas quatro variáveis e na aplicação de regras de decisão que permitem classificar o nível global de satisfação do colaborador. As regras de agregação foram definidas com base na lógica ordinal das variáveis originais.

O índice assume valor 4 (Muito Alto) quando existem três ou mais valores iguais a 4 e nenhuma variável apresenta valor inferior a 3. Assume valor 3 (Alto) quando existem pelo menos três valores iguais a 3 ou 4 e apenas uma variável assume valor 2. O valor 2 (Moderado) é atribuído quando existem dois ou mais valores iguais a 2 ou quando existe exatamente um valor igual a 1. Por fim, o índice assume valor 1 (Baixo) quando existem duas ou mais variáveis com valor igual a 1. Nos restantes casos, o índice assume, por defeito, o valor 2.

A agregação destas variáveis permite sintetizar diferentes dimensões de satisfação organizacional numa única variável interpretável, reduzindo a complexidade associada à análise isolada de cada indicador.

**CareerStagnation**

Foi criada a variável `CareerStagnation`, destinada a identificar possíveis situações de estagnação na progressão profissional. O conceito de *career plateau*, introduzido por Ference et al. (1977), descreve o ponto a partir do qual a progressão hierárquica de um colaborador deixa de ser provável, podendo esta situação assumir uma dimensão hierárquica ou de conteúdo. Adicionalmente, Yang et al. (2019), numa revisão de 40 anos de investigação, reforçam a relevância deste fenómeno na compreensão de comportamentos organizacionais como a intenção de saída.

Com base nesta conceptualização, a variável foi construída para captar simultaneamente as duas dimensões, assumindo valor 1 quando um colaborador não recebeu qualquer promoção há mais de cinco anos (`YearsSinceLastPromotion > 5`) e permanece na mesma função há mais de cinco anos (`YearsInCurrentRole > 5`). Este limiar foi definido com base em critérios frequentemente utilizados na literatura para operacionalizar situações de estagnação prolongada na carreira (Yang et al., 2019). Caso contrário, a variável assume o valor 0.

Este indicador procura captar situações em que a ausência prolongada de progressão na carreira pode estar associada a níveis mais elevados de insatisfação profissional e a uma maior propensão para a saída da organização (Ference et al., 1977).

**IncomePerLevel**

Por fim, foi criada a variável `IncomePerLevel`, com o objetivo de identificar possíveis discrepâncias entre o rendimento auferido e a responsabilidade organizacional associada ao nível hierárquico do colaborador. A literatura na área da compensação sugere que práticas de remuneração podem influenciar o comportamento de saída dos colaboradores, nomeadamente quando afetam a perceção da relação entre desempenho, recompensa e reconhecimento organizacional (Pohler & Schmidt, 2016). Com base neste princípio, foi construída uma medida objetiva desta discrepância, calculada como a razão entre `MonthlyIncome` e `JobLevel`.

A operacionalização através deste rácio foi desenvolvida como uma aproximação quantitativa à relação entre remuneração e posição hierárquica.

Em síntese, a introdução destas variáveis derivadas permite enriquecer o espaço de características do dataset, captando de forma mais direta dimensões relacionadas com mobilidade profissional, satisfação no trabalho, progressão na carreira e estrutura salarial. Estes indicadores adicionais contribuem para aprofundar a análise exploratória dos dados e poderão reforçar o poder explicativo dos modelos de aprendizagem automática a desenvolver nas fases subsequentes do projeto.

### 3.3 Relação das Novas Variáveis com Variável Alvo

Após a criação de novas variáveis, procedeu-se à análise da sua relação com a variável alvo (`Attrition`), com o objetivo de avaliar o seu contributo potencial para a modelação preditiva.

A análise foi conduzida através de uma abordagem complementar, combinando a observação da correlação linear com a variável alvo, a comparação dos valores médios entre classes e a análise visual das distribuições através de boxplots. Esta abordagem permite não só identificar associações estatísticas, mas também avaliar a capacidade discriminativa de cada variável.

Os resultados evidenciam que a variável `SatisfactionIndex` apresenta a associação mais relevante com `Attrition` (correlação ≈ -0.13). Verifica-se que colaboradores com níveis mais elevados de satisfação global tendem a apresentar menor probabilidade de abandono da organização, sendo esta diferença também visível na separação das distribuições entre as classes. Esta relação é consistente com a interpretação dos indicadores individuais de satisfação e reforça a importância desta dimensão no contexto da retenção de talento.

A variável `RatioYearsInRole` apresenta igualmente uma associação negativa moderada (≈ -0.13), sugerindo que colaboradores com maior proporção do tempo passado na função atual tendem a apresentar menor probabilidade de abandono. Este resultado pode refletir tanto padrões de estabilidade funcional como possíveis limitações na mobilidade interna, dependendo do contexto organizacional, captando assim dinâmicas que não são diretamente observáveis nas variáveis originais.

Relativamente à variável `IncomePerLevel`, observa-se também uma associação negativa (≈ -0.11), evidenciando que colaboradores com menor rendimento relativo ao nível hierárquico apresentam maior propensão para abandonar a empresa. Este resultado sugere que a perceção de desajuste entre remuneração e responsabilidade poderá influenciar a decisão de saída, reforçando a relevância de políticas salariais ajustadas à estrutura organizacional.

Por outro lado, a variável `CareerStagnation` apresenta uma correlação praticamente nula com a variável alvo (≈ -0.01), não evidenciando qualquer relação estatisticamente relevante com o atrito. Este resultado sugere que a forma como a variável foi definida poderá não capturar adequadamente o fenómeno de estagnação na carreira, ou que este fator, isoladamente, não tem um impacto significativo na decisão de abandono no contexto do dataset analisado.

De forma global, as novas variáveis - em particular `SatisfactionIndex`, `RatioYearsInRole` e `IncomePerLevel` - demonstram capacidade para captar dimensões relevantes do comportamento organizacional, contribuindo para enriquecer o conjunto de atributos disponíveis. Estas variáveis apresentam potencial para melhorar a capacidade preditiva dos modelos a desenvolver nas fases subsequentes do projeto.

## 4. Dicionário de Dados Final (Pós-Processamento) 
*Listagem final das variáveis que serão entregues ao modelo na Fase 3. - por confirmar no fim da análise exploratória* 


| Variável | Tipo Estatístico | Domínio | Classes / Escala Semântica | Definição Operacional | Papel Analítico |
|----------|------------------|---------|-----------------------------|----------------------|----------------|
| Attrition_bin | Categórica binária | {0,1} | 1 = Abandonou<br>0 = Permaneceu | Codificação binária da variável Attrition. | Variável alvo |
| OverTime_bin | Categórica binária | {0,1} | 1 = Realiza horas extraordinárias<br>0 = Não realiza horas extra | Codificação binária da variável OverTime. | Condições de trabalho |
| Gender_bin | Categórica binária | {0,1} | 1 = Homem<br>0 = Mulher | Codificação binária da variável Gender. | Demográfica |
| CareerStagnation | Categórica binária | {0,1} | 1 = Estagnação de carreira<br>0 = Progressão normal | Assume valor 1 quando YearsSinceLastPromotion > 5 e YearsInCurrentRole > 5; caso contrário assume 0. | Progressão profissional |
| IncomePerLevel | Numérica contínua | ℝ⁺ | - | Rácio entre MonthlyIncome e JobLevel, representando o rendimento relativo ao nível hierárquico. | Compensação |
| RatioYearsInRole | Numérica contínua | [0,1] | - | Proporção entre YearsInCurrentRole e YearsAtCompany; assume 0 quando YearsAtCompany = 0 para evitar divisão por zero. | Mobilidade organizacional |
| SatisfactionIndex | Categórica ordinal | {1,2,3,4} | 1 Baixo<br>2 Moderado<br>3 Alto<br>4 Muito Alto | Índice composto construído a partir de JobSatisfaction, EnvironmentSatisfaction, RelationshipSatisfaction e WorkLifeBalance. | Psicométrica |
 
## 5. Conclusões da Fase de Exploração 
*O que aprenderam sobre o dataset que não sabiam no final do Milestone 1? Os dados são suficientes 
para avançar para a modelação?* 
 --- 

 ## 6. Metodologia de Gestão (PBL) 
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
     
 ## 7. Referências

Ference, T. P., Stoner, J. A. F., & Warren, E. K. (1977).  
*Managing the career plateau*. *Academy of Management Review*.

Géron, A. (2022).  
*Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow* (3ª ed.). O’Reilly Media.

James, G., Witten, D., Hastie, T., & Tibshirani, R. (2021).  
*An Introduction to Statistical Learning: with Applications in R* (2ª ed.). Springer.

Yang, W., Niven, K., & Johnson, S. (2019).  
*Career plateau: A review of 40 years of research*. *Journal of Vocational Behavior*.

Rousseeuw, P. J., & Hubert, M. (2011).  
*Robust statistics for outlier detection*. *Wiley Interdisciplinary Reviews: Data Mining and Knowledge Discovery*.

Pohler, D., & Schmidt, J. A. (2016).  
*Does pay-for-performance strain the employment relationship? The effect of manager bonus eligibility on nonmanagement employee turnover*. *Personnel Psychology*.

Dalton, D. R., & Todor, W. D. (1987).  
*The attenuating effects of internal mobility on employee turnover: Multiple field assessments*. *Journal of Management*.

Tett, R. P., & Meyer, J. P. (1993).  
*Job satisfaction, organizational commitment, turnover intention, and turnover: Path analyses based on meta-analytic findings*. *Personnel Psychology*.
 
*Data de última atualização: 23/03/2026*
