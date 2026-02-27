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

Este projeto aplica metodologias de Ciência de Dados ao problema da rotatividade de colaboradores (Employee Attrition) no contexto de HR Analytics. A retenção de talento constitui um desafio estratégico para organizações modernas, com impacto direto nos custos operacionais, produtividade e sustentabilidade organizacional.

Com base no dataset IBM HR Analytics Employee Attrition & Performance (1470 colaboradores, 35 variáveis), o objetivo consiste em analisar os fatores associados ao attrition e desenvolver modelos preditivos capazes de estimar o risco de saída de colaboradores.

O projeto segue a metodologia CRISP-DM e encontra-se estruturado em quatro milestones: iniciação, exploração, modelação e finalização.

### Objetivos do Projeto
* **Objetivo 1:** Desenvolver um modelo de classificação supervisionado para prever o attrition, alcançando um F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 21/04/2026 (Milestone 3).
* **Objetivo 2:** Construir um índice de risco de attrition baseado nas probabilidades previstas pelo modelo, classificando os colaboradores em categorias de baixo risco (<30%), médio risco (30–60%) e alto risco (>60%), até ao dia 21/04/2026.
* **Objetivo 3:** Aplicar técnicas de clustering não supervisionado para identificar e caracterizar perfis distintos de colaboradores com base nas variáveis relevantes do dataset, determinando o número ótimo de clusters através do método do cotovelo e do Silhouette Score, garantindo um valor médio de Silhouette superior a 0,50, e descrevendo estatisticamente cada perfil identificado, até ao dia 21/04/2026.
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
* **Fonte do Código:** https://www.kaggle.com/code/lusfigueira/m1-inicia-o  


## Referências

IBM Watson Analytics. (2016). *IBM HR Analytics Employee Attrition & Performance Dataset*. IBM Corporation.  
Disponível em: https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset
