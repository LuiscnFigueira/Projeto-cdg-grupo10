# Relatório de Conclusão e Entrega de Valor (Milestone 4)

## 1. Síntese de Resultados e Impacto

### 1.1 Objetivo 1 - Modelo Preditivo de Attrition

#### O Problema Resolvido
O primeiro objetivo do projeto consistia em desenvolver um modelo de classificação supervisionado capaz de prever o atrito (`Attrition`) dos colaboradores, alcançando um F1-Score mínimo de 0,80 em validação cruzada estratificada (k=5), até ao dia 21/04/2026. O problema foi formalizado como uma tarefa de classificação binária sobre o dataset IBM HR Analytics Employee Attrition & Performance, composto por 1470 colaboradores e 35 variáveis originais, com uma taxa de atrito de 16,1% (Chapman et al., 2000; IBM Watson Analytics, 2016).

#### Interpretação dos Resultados
O modelo final selecionado foi a Regressão Logística com pipeline `MaxAbsScaler` + SMOTE + `StratifiedKFold` (k=15) e threshold ótimo de 0,52, obtido após um processo de otimização em cinco etapas sequenciais: pesquisa do melhor split (85/15), do melhor normalizador (`MaxAbsScaler`), da melhor técnica de resampling (SMOTE), de hiperparâmetros via `GridSearchCV` e do threshold de decisão. Foram testados 18 algoritmos distintos antes de convergir para esta solução, cobrindo modelos de ensemble, lineares, probabilísticos e redes neuronais, em linha com a recomendação do CRISP-DM de explorar múltiplos algoritmos antes de selecionar o modelo final (Chapman et al., 2000).

[Por acabar]

#### Índice de Risco de Attrition
* A partir das probabilidades previstas pelo modelo, foi construído um índice de risco.
* Os colaboradores foram classificados em categorias de risco, facilitando a interpretação dos resultados.

#### Valor para o Utilizador/Negócio
* A solução permite identificar colaboradores com maior probabilidade de saída.
* Este resultado pode apoiar estratégias preventivas de retenção e tomada de decisão em Recursos Humanos.



### 1.2 Objetivo 2 - Identificação de Perfis de Colaboradores através de Clustering

#### O Problema Resolvido

O segundo objetivo consistia em aplicar técnicas de clustering não supervisionado para identificar e caracterizar perfis distintos de colaboradores, determinando o número ótimo de clusters com um Silhouette Score médio superior a 0,50, até ao dia 21/04/2026. O problema foi abordado como uma tarefa de aprendizagem não supervisionada, sem recurso a qualquer informação sobre a variável alvo `Attrition` durante o processo de agrupamento (Chapman et al., 2000).

#### Interpretação dos Resultados
* [Inserir número de clusters identificados.]
* [Inserir Silhouette Score.]
* [Explicar por palavras simples os perfis encontrados.]

#### Valor para o Utilizador/Negócio
* A segmentação permite compreender melhor diferentes grupos de colaboradores.
* Os perfis identificados podem apoiar estratégias de gestão, retenção e acompanhamento mais direcionadas.



## 2. Análise Crítica e Limitações

### 2.1 Limitações dos Dados
* O dataset é sintético, tendo sido criado pela IBM para fins demonstrativos.
* A variável `Attrition` apresenta desequilíbrio de classes.
* Algumas variáveis apresentam pouca variabilidade ou reduzido valor preditivo.
* O dataset não inclui fatores externos, como mercado de trabalho, clima organizacional real ou histórico detalhado de desempenho.

### 2.2 Limitações do Modelo Preditivo
* O modelo pode apresentar maior dificuldade em identificar corretamente colaboradores da classe minoritária.
* As previsões indicam probabilidade de risco, mas não provam causalidade.
* O índice de risco deve ser interpretado como ferramenta de apoio à decisão, não como decisão automática.

### 2.3 Limitações do Clustering
* Os clusters identificam grupos com características semelhantes, mas não demonstram causalidade.
* O resultado depende das variáveis selecionadas, da escala dos dados e do número de clusters escolhido.
* Caso o Silhouette Score seja inferior ao esperado, a separação entre perfis pode ser considerada limitada.



## 3. Considerações Éticas e de Viés

### 3.1 Privacidade
* O dataset encontra-se anonimizado e não contém identificadores pessoais diretos.
* A utilização dos dados é exclusivamente académica.

### 3.2 Transparência
* As decisões do modelo devem ser explicadas através da análise da importância das variáveis.
* O índice de risco deve ser apresentado como uma ferramenta de apoio à decisão humana.

### 3.3 Viés
* O desequilíbrio da variável `Attrition` pode influenciar o desempenho do modelo.
* Variáveis como idade, género, estado civil ou rendimento devem ser interpretadas com cautela.
* A classificação de colaboradores como “de risco” não deve ser usada para penalização, mas sim para apoiar estratégias justas de retenção.



## 4. Roadmap e Trabalhos Futuros

### 4.1 Melhorias Técnicas
1. Testar modelos adicionais e técnicas de otimização de hiperparâmetros.
2. Avaliar técnicas de balanceamento, como SMOTE.
3. Melhorar a calibração das probabilidades do modelo preditivo.
4. Testar outros algoritmos de clustering além do K-Means.

### 4.2 Novas Variáveis
1. Integrar dados reais de engagement, absentismo e avaliações de desempenho.
2. Adicionar variáveis temporais para acompanhar a evolução do risco ao longo do tempo.
3. Incluir fatores externos, como mercado de trabalho, políticas internas de retenção e clima organizacional.

### 4.3 Escalabilidade e Deployment
1. Criar uma interface em Streamlit para consulta dos resultados.
2. Desenvolver dashboards para Recursos Humanos com métricas de risco e perfis de colaboradores.
3. Implementar monitorização contínua do desempenho do modelo.



## 5. Conclusão Final

* O projeto permitiu aplicar técnicas de Ciência de Dados ao problema da rotatividade de colaboradores.
* Foram trabalhadas duas dimensões principais: previsão de `Attrition` e identificação de perfis através de clustering.
* O índice de risco funciona como uma extensão interpretável do modelo preditivo.
* Apesar das limitações do dataset, a solução demonstra potencial para apoiar decisões estratégicas em Recursos Humanos.

---

**Data de Conclusão:** [Inserir Data]

**Versão do Projeto:** v4.0 Final
