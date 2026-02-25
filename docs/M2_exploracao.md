# Milestone 2: Análise Exploratória e Engenharia de Atributos 
 
> **Nota de Revisão:** Este documento pressupõe que o dataset já foi identificado e descrito no 
ficheiro `docs/M1_iniciacao.md`. Caso precise de consultar o significado original das variáveis, 
deve consultar essa Milestone. 
 
## 1. Análise Exploratória de Dados (EDA) 
### 1.1. Distribuição da Variável Alvo 
*Descrevam como se comporta a variável que querem prever. Está equilibrada? Segue uma distribuição 
normal?* 
> **Factos importantes:** (Ex: "A nossa variável alvo 'Churn' está desequilibrada, com 80% de 
clientes ativos e 20% que saíram.") 
 
### 1.2. Correlações Relevantes 
*Quais as variáveis que têm maior relação com o problema? Incluam referências a gráficos que 
geraram no Kaggle.* 
* **Atributo A vs. Alvo:** (Ex: "Notámos que quanto maior a idade, menor a probabilidade de 
cancelamento.") 
* **Atributo B vs. Alvo:** (Ex: "O tipo de contrato mensal está fortemente ligado à saída de 
clientes.") 
 
## 2. Qualidade dos Dados e Limpeza 
### 2.1. Tratamento de Dados em Falta (Missing Data) 
* **Colunas afetadas:** [Lista de colunas] 
* **Estratégia adotada:** (Ex: "Substituímos os nulos da coluna 'Salário' pela mediana para 
evitar o impacto de outliers.") 
 
### 2.2. Outliers e Inconsistências 
*Descrevam se encontraram valores impossíveis (ex: idade = 200) e como os resolveram.* 
 
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
*Data de última atualização: 25/02/2026*
