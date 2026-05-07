# Analise e Predicao de Atrasos de Voos nos EUA

Tech Challenge - MLET Fase 3

## Descricao

Este projeto desenvolve um pipeline completo de ciencia de dados para analisar e prever atrasos de voos nos Estados Unidos, aplicando tecnicas de Machine Learning supervisionado e nao supervisionado. O estudo abrange desde a exploracao dos dados ate a interpretacao critica dos resultados.

## Base de Dados

Datasets publicos referentes ao ano de 2015, compostos por tres arquivos:

| Arquivo | Descricao |
|---|---|
| `flights.csv` | Registros detalhados de voos nos EUA (aprox. 5.8 milhoes de linhas) |
| `airlines.csv` | Codigo IATA e nome das companhias aereas |
| `airports.csv` | Informacoes dos aeroportos (codigo, nome, cidade, estado, coordenadas) |

> O arquivo `flights.csv` possui 564 MB e e armazenado via Git LFS.

## Estrutura do Projeto

```
tech_challenge_flights.ipynb   # Notebook principal com toda a analise
flights.csv                    # Dataset de voos (Git LFS)
airlines.csv                   # Dataset de companhias aereas
airports.csv                   # Dataset de aeroportos
```

## Conteudo do Notebook

### 1. Importacao de Bibliotecas e Dados
Carregamento e enriquecimento dos datasets com join entre voos e companhias aereas.

### 2. Exploracao dos Dados (EDA)
- Estatisticas descritivas das variaveis principais
- Analise de valores ausentes
- Distribuicao dos atrasos na partida e na chegada
- Atrasos por companhia aerea
- Atrasos por mes e dia da semana
- Atrasos por horario de partida
- Top aeroportos com maior atraso medio
- Analise das causas de atraso (sistema aereo, clima, companhia, aeronave, seguranca)
- Matriz de correlacao entre variaveis numericas

### 3. Tratamento de Dados
- Remocao de voos cancelados e desviados
- Imputacao de valores ausentes
- Engenharia de features (faixa horaria, variavel target binaria)
- Codificacao de variaveis categoricas

### 4. Modelagem Supervisionada

**4.1 Classificacao - Prever se um voo vai atrasar (atraso >= 15 min)**
- Regressao Logistica
- Random Forest Classifier
- Metricas: accuracy, precision, recall, F1-score, ROC-AUC, matriz de confusao

**4.2 Regressao - Prever a duracao do atraso**
- Random Forest Regressor
- Gradient Boosting Regressor
- Metricas: MAE, RMSE, R2

### 5. Modelagem Nao Supervisionada

**5.1 Clusterizacao (K-Means)**
- Agrupamento de aeroportos por perfil de atraso
- Metodo do cotovelo para selecao do numero de clusters
- Visualizacao e interpretacao dos grupos

**5.2 Reducao de Dimensionalidade (PCA)**
- Analise de variancia explicada pelos componentes principais
- Visualizacao dos dados em espaco reduzido

### 6. Conclusoes e Proximos Passos
- Principais achados sobre padroes de atraso
- Limitacoes dos modelos
- Sugestoes de melhorias e proximas iteracoes

## Principais Insights

- Voos programados para o inicio da manha apresentam os menores indices de atraso
- Os atrasos aumentam progressivamente ao longo do dia, atingindo pico no fim da tarde e no inicio da noite
- Aeronave atrasada e a principal causa de propagacao de atrasos na rede
- Ha variacao sazonal significativa: junho e julho concentram os maiores atrasos medios
- Sexta-feira e o dia da semana com maior atraso medio

## Tecnologias Utilizadas

- Python 3
- pandas, numpy
- matplotlib, seaborn
- scikit-learn (LogisticRegression, RandomForestClassifier, RandomForestRegressor, GradientBoostingRegressor, KMeans, PCA)
- Jupyter Notebook

## Como Executar

1. Clone o repositorio (requer Git LFS instalado para baixar o arquivo `flights.csv`):

```bash
git lfs install
git clone https://github.com/GabrielPeixer/delay_voos.git
cd delay_voos
```

2. Instale as dependencias:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

3. Execute o notebook:

```bash
jupyter notebook tech_challenge_flights.ipynb
```

## Observacao sobre o Dataset

O arquivo `flights.csv` possui aproximadamente 565 MB e e armazenado com Git Large File Storage (LFS). Para baixa-lo corretamente, certifique-se de ter o Git LFS instalado antes de clonar o repositorio (`git lfs install`).
