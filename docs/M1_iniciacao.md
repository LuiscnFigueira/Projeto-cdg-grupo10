# Milestone 1: Iniciação e Definição do Projeto 
 
## 1. Descrição Detalhada do Problema

**Contexto do Setor: Gestão de Recursos Humanos (HR Analytics)** 

O capital humano ganhou uma relevância estratégica como nunca antes, transformando a forma como as organizações planeiam o seu futuro. No setor tecnológico e de serviços, como o representado pelo dataset IBM HR Analytics Employee Attrition & Performance, a saída voluntária de colaboradores (attrition) é um fenómeno complexo que vai além da simples rotatividade. A utilização de HR Analytics permite transformar dados brutos em decisões estratégicas, movendo os Recursos Humanos de uma função puramente administrativa para uma função preditiva e baseada em evidências.

**Relevância no Momento Atual**

A rotatividade voluntária representa um dos principais desafios estratégicos na gestão moderna, sendo a sua relevância justificada por fatores críticos que impactam diretamente a sustentabilidade das organizações. Em primeiro lugar, destaca-se o impacto financeiro direto e indireto, uma vez que a saída de um colaborador implica custos de recrutamento, seleção, anúncios e indemnizações, além de despesas com onboarding, formação e a perda inevitável de conhecimento organizacional e produtividade, valores que podem atingir o dobro do salário anual do cargo. Adicionalmente, este fenómeno é influenciado por fatores multidimensionais, onde o abandono é moldado por uma combinação de variáveis demográficas, comportamentais e organizacionais, tais como a satisfação no trabalho, o envolvimento e o equilíbrio entre a vida pessoal e profissional. Estas taxas elevadas de rotatividade geram ainda um impacto negativo no clima organizacional, afetando o moral das equipas restantes e criando um ciclo de instabilidade interna. Contudo, num mercado globalizado e altamente competitivo, a retenção de especialistas e talentos internos torna-se um fator decisivo de vantagem competitiva, essencial para manter a inovação e a eficiência operacional da empresa.
 
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
 
## 5. Cronograma Interno 
| Fase | Data Limite | Entregável Esperado | 
| :--- | :--- | :--- | 
| M1: Iniciação | [24/02/2026] | Repositório estruturado e Plano de Projeto. | 
| M2: Exploração | [24/03/2026] | Notebook de EDA e Dados Processados. | 
| M3: Modelação | [21/04/2026] | Comparação de algoritmos e métricas. | 
| M4: Finalização| [21/05/2026] | Pitch e Relatório Final. | 
 --- 
*Data de última atualização: [18/02/2026]*
