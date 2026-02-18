# [Título do Projeto de Ciência de Dados]
## Identificação da Equipa
* **Grupo nº:** 10
* **Membros:**
* Luís Figueira - 2022160309
* Martim Ferreira - 2023132459
* Mateus Afonso - 2023142275
## Organização do Repositório
A estrutura deste projeto segue as boas práticas de Ciência de Dados e Engenharia de Software:
* **`data/`**: Armazenamento de dados (dados brutos em `raw/` e processados em `processed/`).
* **`docs/`**: Documentação técnica detalhada dividida por Milestones (M1, M2 e M3).
* **`notebooks/`**: Jupyter Notebooks para experimentação, limpeza e modelação.
* **`src/`**: Código-fonte modular (scripts `.py`) para funções reutilizáveis.
* **`reports/`**: Relatórios finais, apresentações e exportação de figuras (`figures/`).
* **`requirements.txt`**: Ficheiro de configuração com as bibliotecas necessárias.
## 1. Iniciação (Milestone 1)
### Contexto e Problema de Negócio
A empresa enfrenta custos elevados associados à rotatividade voluntária de colaboradores.
Pretende-se desenvolver um modelo preditivo capaz de identificar colaboradores com maior probabilidade de abandono, permitindo priorizar intervenções de retenção, e consequentemente reduzir custos de substituição (recrutamento, onboarding e perda de produtividade) e melhorar a retenção de talento.
### Objetivos do Projeto
* **Objetivo 1:** Desenvolver um modelo de classificação supervisionado para prever o attrition, alcançando um F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 28/02/2026 (Milestone 3).
* **Objetivo 2:** Construir um índice de risco de attrition baseado nas probabilidades previstas pelo modelo, classificando os colaboradores em categorias de baixo risco (<30%), médio risco (30–60%) e alto risco (>60%), até ao dia 07/03/2026.
* **Objetivo 3:** Aplicar técnicas de clustering não supervisionado para identificar e caracterizar perfis distintos de colaboradores com base nas variáveis relevantes do dataset, determinando o número ótimo de clusters através do método do cotovelo e do Silhouette Score, garantindo um valor médio de Silhouette superior a 0,50, e descrevendo estatisticamente cada perfil identificado, até ao dia 07/03/2026.
### Fonte de Dados
* **Dataset:** [https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset]
* **Dimensão:** [1470 linhas, 35 colunas]
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
