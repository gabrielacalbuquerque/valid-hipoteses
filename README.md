# valid-hipoteses
Validação de hipóteses através da análise de dados e fornecimento de recomendações estratégicas com base nas descobertas, com dashboards interativos.

# **✔Ficha Técnica**: Projeto de Análise de Dados

---

## **📰** Análise Exploratória e validação de hipóteses baseadas na preferência musical de usuários da plataforma de streamings Spotify

---

**🎯 Objetivo:**

Realizar a limpeza, preparação, e validação de hipóteses através da análise de dados, fornecendo recomendações estratégicas com base nas descobertas. O objetivo principal da análise é a tomada de decisão direcionada pelos resultados para que uma gravadora e seu novo artista possam ter mais chances de alcançar o “sucesso”

---

**👩🏽‍🦱 Equipe:** O projeto foi feito em dupla, mas as etapas aqui relatadas foram realizadas individualmente

---

**⚙ Ferramentas e Tecnologias**:

- BigQuery: Importação, limpeza de dados, criação de variáveis e tabelas dinâmicas.
- PowerBi: Importação, construção de gráficos de barra, linha e de dispersão, visualização de dados e construção de dashboard.
- Google Colab: Importação, análise, aplicação de técnica de regressão linear simples, construção de gráficos de dispersão a partir da linguagem Python.

---

**💻 Processamento e análises:**

1. Criação do projeto, dataset e tabelas carregadas no BigQuery.
2. Foi utilizada a função COALESCE para substituir valores nulos (NULL) nas colunas por valores padrão:

     - Para colunas numéricas, valores NULL foram substituídos por 0.

-Para colunas de texto, valores NULL foram substituídos por "Unknown" ou "No Info", de  acordo com o contexto.

1. A função REGEXP_REPLACE foi aplicada nas colunas track_name e artists_name para remover caracteres especiais, como acentos e símbolos não alfanuméricos, utilizando a expressão regular r'[^A-Za-z0-9 ]'.
2. Utilizou-se a função NORMALIZE para aplicar uma forma normal de caracteres, especialmente importante para remover acentos e garantir que caracteres especiais fossem tratados de forma consistente.
3. Foram Identificados e tratados dados discrepantes em variáveis categóricas e numéricas.
4. Utilizou-se o JOINS para juntar tabelas.
5. Foi usada a estrutura de tabela temporária WITH.
6. Utilizou-se a função NTILE(4) OVER (ORDER BY streams) para dividir os dados de streams em quatro grupos (quartis), ajudando a entender a distribuição dos dados.
7. Baseado nos quartis de streams, foi criada a coluna classificacao_streams, que classifica as músicas como "alto" ou "baixo" de acordo com o quartil de cada música.
8. Foi calculada a correlação entre streams e in_spotify_playlists, utilizando a função CORR, e o valor da correlação foi armazenado na coluna correlacao_valor.
9. A tabela final tabela_spotify_unificada_tratada foi importada para o PowerBi e utilizada para realizar gráficos e análises exploratórias sobre a popularidade das músicas, a relação entre o número de streams e a presença nas playlists, e para avaliar as métricas técnicas de cada música (como dançabilidade, energia, acusticidade, etc.), bem como segmentar músicas com base em quartis de streams.
10. 

| Nº | Variáveis Comparadas | Coef. de Correlação (r) | Classificação | Interpretação |
| --- | --- | --- | --- | --- |
| Hipótese 1 | BPM × Streams | -0.0046 | Muito fraca | Praticamente nenhuma relação entre o BPM e a quantidade de streams. |
| Hipótese 2 | In_spotify_charts × In_deezer_charts | 0.5545 | Forte | Músicas populares no Spotify também tendem a ser populares no Deezer. |
| Hipótese 3 | In_spotify_playlists × Streams | 0.7897 | Muito forte | Músicas em playlists do Spotify têm forte relação com o número de streams. |
| Hipótese 4 | Total_musicas × Streams | -0.0099 | Muito fraca | Quase nenhuma relação entre o total de músicas por artista e seus streams. |
| Hipótese 5.1 | BPM × Streams | -0.0046 | Muito fraca | Nenhuma relação linear perceptível entre BPM e streams. |
| Hipótese 5.2 | Danceability × Streams | -0.0046 | Muito fraca | O quão dançante é a música não afeta diretamente os streams. |
| Hipótese 5.3 | Valence × Streams | -0.0577 | Muito fraca | O “tom emocional” da música tem pouquíssima relação com os streams. |
| Hipótese 5.4 | Energy × Streams | -0.0371 | Muito fraca | A energia percebida na música pouco influencia os streams. |
| Hipótese 5.5 | Acousticness × Streams | -0.0213 | Muito fraca | O quão acústica é a música quase não afeta sua popularidade. |
| Hipótese 5.6 | Instrumentalness × Streams | 0.0091 | Muito fraca | Não há relação significativa com o número de streams. |
| Hipótese 5.7 | Liveness × Streams | 0.0091 | Muito fraca | Presença de som ao vivo não influencia os streams. |
| Hipótese 5.8 | Speechiness × Streams | -0.0494 | Muito fraca | Presença de fala na música tem impacto praticamente nulo nos streams. |
1.  No PowerBI foi feito um dashboard com filtro por ano, Scorecards com dados de streamings, músicas e playlists, além de gráficos de coluna, linha e de dispersão.
2. No Google Colab, a partir da linguagem Python, foi aplicada a técnica de regressão linear simples

| Nº | Hipótese | Descrição | Coeficiente(s) | Conclusão (com interpretação) |
| --- | --- | --- | --- | --- |
| Hipótese 1 | BPM × Streams | Músicas com BPM mais alto têm mais streams | -0.0145 | **Refutada** — A correlação é muito fraca e negativa, indicando ausência de relação significativa. |
| Hipótese 2 | Popularidade se repete em outras plataformas | Sucesso no Spotify implica sucesso em outras plataformas | +0.7898 (Spotify), +0.1443 (Deezer) | **Parcialmente validada** — Relação muito forte com Spotify, mas fraca com Deezer. |
| Hipótese 3 | Mais playlists = mais streams | Quanto mais playlists a música aparece, mais ela é ouvida | +0.7898 | **Validada** — Correlação muito forte, indicando forte associação entre playlists e streams. |
| Hipótese 4 | Mais músicas por artista = mais streams | Artistas com mais músicas terão mais streams totais | -0.1365 | **Refutada** — Correlação fraca e negativa, indicando ausência de relação direta. |
| Hipótese 5 | Características técnicas explicam sucesso | Sucesso da música pode ser explicado por seus atributos técnicos | Todos negativos e muito fracos | **Refutada** — As correlações são todas muito fracas, sem evidência de relação com streams. |

---

**📑 Resultados e Conclusões:**

Hipóteses a partir das correlações do BigQuery:

1. Correlação hipótese 1 - Muito fraca
2. Correlação hipótese 2 - Forte
3. Correlação hipótese 3 - Muito fraca
4. Correlação hipótese 4 - Muito fraca
5. Correlação hipótese 5 - Muito fraca

Hipóteses a partir da técnica de regressão linear simples, no Google Colab

1. Hipótese 1 - Refutada
2. Hipótese 2 - Parcialmente validada
3. Hipótese 3 - Validada
4. Hipótese 4 - Refutada
5. Hipótese 5 - Refutada

---

**🔐Limitações/Próximos Passos/recomendações**:

### **Limitações**

- **Dados Incompletos ou Imperfeitos**: Possíveis lacunas e inconsistências nos dados podem impactar a precisão da análise.
- **Exclusão de Variáveis Potenciais**: Algumas variáveis relevantes para o sucesso das músicas podem não ter sido incluídas.
- **Viés nos Dados**: A análise pode ser influenciada por uma seleção limitada de dados (ano específico ou plataforma única).
- **Interpretação de Correlações**: A correlação não implica causalidade; interpretações devem ser feitas com cautela.

### **Próximos Passos**

- **Aprofundamento na Causalidade**: Investigar relações causais por meio de modelos de regressão ou séries temporais.
- **Exploração de Novas Variáveis**: Incluir fatores adicionais como gênero musical, estratégias de marketing e data de lançamento.
- **Análise Comparativa entre Plataformas**: Avaliar o impacto da presença em diferentes plataformas sobre o sucesso das músicas.
- **Teste de Novas Hipóteses**: Explorar novas relações, como a influência das playlists no sucesso das faixas.

### **Recomendações**

- **Aprimorar Visualizações**: Continuar a evolução das visualizações no Power BI para reforçar a narrativa dos dados, utilizando diferentes tipos de gráficos.
- **Refinar Métricas de Sucesso**: Definir claramente o critério de "sucesso" (ex.: streams, playlists) para uma análise mais focada.
- **Validação de Resultados**: Validar conclusões com dados adicionais ou por meio de amostras diferentes para garantir a robustez dos resultados.

---

**🔗Links de interesse:**

- Gráficos, resultados de dashboard no PoweBI- [**LINK**](https://drive.google.com/file/d/1cSMvAo5meAHjwTrKqrbDLxwRR6sSBSAj/view?usp=sharing)
- Dashboard no Looker Studio - [**LINK**](https://lookerstudio.google.com/u/0/reporting/2e80321b-b8ac-49c4-a07f-9eeef6b1f186)
- Apresentação da análise dos resultados - [**LINK**](https://docs.google.com/presentation/d/1Db0Ver6PCbylMGAcBsBiHv4-hXowptFWHwDq0yv1saU/edit)
