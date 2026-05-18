# Q&A - People Analytics & Employee Turnover Prediction

Esta secção reúne as perguntas mais prováveis numa defesa ou sessão de Q&A sobre este projeto.
As respostas foram preparadas para ser dadas oralmente, de forma clara e sem jargão desnecessário.
A organização segue a lógica do próprio projeto: dados, modelação, clustering e impacto.

## Dados e Exploração

### "O OverTime é o fator mais importante. Mas isso não é óbvio? Qual é o valor analítico?"

**Resposta:** A correlação de 0,25 era expectável, mas o valor analítico está na quantificação. Sem o modelo, um gestor sabe vagamente que quem faz horas a mais está mais insatisfeito. Com o modelo, sabe que a taxa de saída de quem faz OverTime é de 30,5%, contra 10,4% de quem não faz - três vezes maior. E sabe que o coeficiente de Regressão Logística para OverTime é +0,944, o mais alto de todas as variáveis. Isso permite priorizar: se a organização quer reduzir attrition, a primeira medida a avaliar é a política de horas extraordinárias. Transforma intuição em evidência acionável.

### "Como trataram as variáveis categóricas? E as variáveis com distribuições muito assimétricas?"

**Resposta:** As variáveis categóricas nominais - como Departamento, Género e EstadoCivil - foram convertidas com one-hot encoding. As ordinais - como JobLevel e Education - foram mantidas como numéricas porque a ordem é significativa. Para as distribuições assimétricas, o StandardScaler normaliza todas as variáveis numéricas antes de entrar no modelo. No caso da Regressão Logística, a normalização é essencial porque o modelo é sensível à escala. O pipeline garante que o StandardScaler é aplicado sempre antes do SVMSMOTE e do modelo, incluindo dentro da validação cruzada, para evitar data leakage.

## Modelação - Classificação

### "O modelo prevê quem vai sair - mas não quando. Para uma decisão de retenção, o timing importa. Como tratam isso?"

**Resposta:** É uma limitação real e assumimos que existe. O modelo classifica cada colaborador com uma probabilidade de saída, mas não estima se isso acontece daqui a um mês ou daqui a dois anos. Num contexto real, isso faz diferença: uma intervenção feita tarde pode ser inútil. Para resolver isto seria necessário reformular o problema como um modelo de sobrevivência - por exemplo com Cox Proportional Hazards ou modelos de tempo até evento - que estimam não só se alguém vai sair mas em que janela temporal. Essa abordagem exige dados longitudinais, com registos históricos por colaborador ao longo do tempo, que o dataset IBM não tem na forma necessária. É um próximo passo concreto e relevante para um deployment real.

### "Porquê Regressão Logística e não um modelo mais sofisticado?"

**Resposta:** Porque os modelos mais sofisticados não generalizaram. O Random Forest, por exemplo, atingiu F1 próximo de 1,0 no treino e colapsou no teste - overfitting claro. A Regressão Logística obteve o melhor equilíbrio entre desempenho no teste e diferença treino/teste. Além disso, tem uma vantagem decisiva neste contexto: interpretabilidade. Conseguimos dizer ao gestor de RH que um coeficiente de +0,944 no OverTime significa que fazer horas extraordinárias aumenta significativamente a probabilidade de saída. Num modelo que afeta decisões sobre pessoas, ser capaz de explicar o porquê é tão importante quanto a precisão.

### "Porquê o split 65/35 e não o tradicional 80/20?"

**Resposta:** Testámos vários splits durante a otimização com GridSearchCV. O split 65/35 produziu o melhor equilíbrio entre estabilidade no treino e desempenho no teste. Com 80/20, o conjunto de teste ficava com apenas 294 observações e cerca de 47 casos positivos - demasiado pequeno para ser representativo. Com 65/35, o conjunto de teste tem 515 observações, o que dá estimativas mais fiáveis das métricas reais. O GridSearchCV testou 63 configurações diferentes, incluindo splits, e esta foi a vencedora.

### "O dataset IBM é sintético. O que isso implica para os vossos resultados?"

**Resposta:** Implica que os resultados não podem ser diretamente generalizados para organizações reais. O IBM HR Analytics foi construído para fins educacionais e as distribuições são artificialmente controladas. Isso significa que o nosso modelo pode ter aprendido padrões que não existem em dados reais. Declaramos isto como limitação honesta. O valor do projeto está na metodologia - o pipeline CRISP-DM, a escolha e avaliação de 18 algoritmos, a construção do índice de risco - que é diretamente transferível para um contexto com dados reais. Com dados de uma organização real, esperamos que o F1 melhore porque os padrões serão mais consistentes.

## Clustering

### "Como sabem que os clusters são válidos e não um artefacto do algoritmo?"

**Resposta:** Temos duas formas de validação. A primeira é quantitativa: Silhouette Score de 0,70 indica separação forte entre clusters; Davies-Bouldin de 0,39 confirma compacidade interna; e o delta entre treino e teste é de apenas 0,01, o que descarta overfitting. A segunda é qualitativa e é a mais convincente: o modelo foi treinado sem qualquer informação sobre departamentos ou funções. Quando analisámos os perfis resultantes, cada cluster corresponde a um departamento real - I&D, Vendas, Liderança Científica, RH. Isso é validação externa. Se os clusters fossem artefacto, não encontraríamos essa correspondência.

### "Os quatro perfis de clustering - que intervenções concretas sugerem para cada um?"

**Resposta:** Para o Cluster 2 - Equipa de Vendas - o perfil mais jovem e mais móvel, a intervenção prioritária é progressão de carreira e reconhecimento rápido, porque são pessoas que saem quando não veem futuro a curto prazo. Para o Cluster 0 - I&D Operacional, o maior grupo - a estabilidade é um fator de retenção, pelo que mudanças abruptas de função ou projeto são arriscadas. Para o Cluster 1 - Liderança Científica, apenas 5,8% - são de alto valor estratégico e difíceis de substituir, por isso merecem atenção individualizada. Para o Cluster 3 - RH, o mais pequeno - o perfil distinto sugere que as motivações são diferentes e merecem análise separada.

## Aplicação e Ética

### "O índice de risco é uma ferramenta de apoio à decisão. Mas não pode ser usado para discriminar colaboradores?"

**Resposta:** É uma preocupação legítima e importante. O índice não deve ser usado para penalizar colaboradores, para condicionar promoções ou para tomar decisões unilaterais. A sua função é orientar conversas - o gestor sabe onde concentrar atenção e pode ter uma conversa proativa com um colaborador antes que ele decida sair. A decisão final é sempre humana. Isto está alinhado com os princípios de ética em IA: usar o modelo como apoio ao julgamento humano, não como substituto. Num contexto real, recomendaríamos políticas claras de governação sobre como e por quem o índice pode ser consultado.

### "Se tivessem que apresentar este projeto a um CEO em dois minutos, o que diriam?"

**Resposta:** Um em cada seis colaboradores desta organização vai sair no próximo ano - e não sabem qual. Cada saída tem um custo real: recrutamento, formação, perda de conhecimento. Desenvolvemos um sistema que analisa os dados de RH e classifica cada colaborador em quatro níveis de risco: verde, amarelo, laranja e vermelho. Quase 500 pessoas estão hoje nos níveis de alerta. Identificámos também quatro perfis distintos de colaboradores, cada um com motivações e fatores de risco diferentes, o que permite intervir de forma personalizada. O próximo passo é um dashboard em Streamlit que qualquer gestor de RH consegue usar sem formação técnica. A ferramenta está pronta. O que falta são dados reais da vossa organização.
