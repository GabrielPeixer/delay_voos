# Análise e Predição de Atrasos de Voos nos EUA

Tech Challenge - MLET Fase 3

## Descrição

Este projeto desenvolve um pipeline completo de ciência de dados para analisar e prever atrasos de voos nos Estados Unidos, aplicando técnicas de Machine Learning supervisionado e não supervisionado. O estudo abrange desde a exploração e engenharia de features até a detecção de anomalias e interpretação crítica dos resultados.

## Base de Dados

Datasets públicos referentes ao ano de 2015, compostos por três arquivos:

| Arquivo | Descrição |
|---|---|
| `flights.csv` | Registros detalhados de voos nos EUA (aprox. 5,8 milhões de linhas) |
| `airlines.csv` | Código IATA e nome das companhias aéreas |
| `airports.csv` | Informações dos aeroportos (código, nome, cidade, estado, coordenadas) |

> O arquivo `flights.csv` possui 564 MB e é armazenado via Git LFS.

## Estrutura do Projeto

```
tech_challenge_3.ipynb   # Notebook principal com toda a análise
flights.csv              # Dataset de voos (Git LFS)
airlines.csv             # Dataset de companhias aéreas
airports.csv             # Dataset de aeroportos
```

## Conteúdo do Notebook

### 1. Importação de Bibliotecas e Dados
Carregamento dos três datasets e enriquecimento dos voos com o nome da companhia aérea via merge por código IATA.

### 2. Exploração dos Dados (EDA)
- Visão geral e estatísticas descritivas
- Análise de valores ausentes
- Distribuição dos atrasos na partida e na chegada
- Atrasos por companhia aérea, mês, dia da semana e horário
- Top aeroportos com maior atraso médio
- Análise das causas de atraso (sistema aéreo, clima, companhia, aeronave, segurança)
- Matriz de correlação entre variáveis numéricas

### 3. Feature Engineering
Criação de variáveis derivadas com sinal preditivo confirmado graficamente:
- `PERIOD_OF_DAY` — período do dia (madrugada / manhã / tarde / noite)
- `SEASON` — estação do ano (hemisfério norte)
- `IS_WEEKEND` — indicador de fim de semana
- `IS_HOLIDAY` — indicador de feriado federal dos EUA em 2015 e períodos de alto tráfego

### 4. Tratamento e Preparação para Modelagem
- Remoção de voos cancelados
- Remoção de linhas sem informação de atraso
- Criação do target binário `IS_DELAYED` (chegada com >= 15 min de atraso)
- Target encoding com smoothing para a variável de alta cardinalidade `ORIGIN_AIRPORT`
- Codificação one-hot para variáveis categóricas nominais

### 5. Modelagem Supervisionada

**5.1 Classificação — Prever se um voo vai atrasar (IS_DELAYED)**
- Regressão Logística (com `class_weight='balanced'`)
- Random Forest Classifier (com `class_weight='balanced'`)
- Métricas: accuracy, precision, recall, F1-score, ROC-AUC, matriz de confusão, curva ROC
- Importância de features
- Validação cruzada estratificada (5-fold)
- Comparação com split temporal (treino em meses 1-9, teste em 10-12)

**5.2 Regressão — Prever a magnitude do atraso (ARRIVAL_DELAY)**
- Random Forest Regressor
- Gradient Boosting Regressor
- Métricas: MAE, RMSE, R²
- Clip de outliers em [-30, 300] minutos

### 6. Modelagem Não Supervisionada

**6.1 Clusterização (K-Means) — Companhias Aéreas**
- Agrupamento das 14 companhias por perfil operacional (atraso médio, distância, taxa de cancelamento, taxi time, volume de voos)
- Método do cotovelo para seleção de K
- Visualização dos clusters via PCA 2D com rótulos

**6.2 Redução de Dimensionalidade (PCA) — Voos**
- Análise de variância explicada por componente
- Loadings dos principais componentes
- Visualização 2D colorida por nível de atraso (adiantado / leve / moderado / severo)

### 7. Bônus: Detecção de Anomalias (Isolation Forest)
- Identificação de voos com perfil operacional atípico
- Features: `DEPARTURE_DELAY`, `TAXI_OUT`, `TAXI_IN`, `AIR_TIME`, `DISTANCE`, `SCHEDULED_TIME`
- Contaminação de 1%; comparação da distribuição de atraso entre anomalias e voos normais

### 8. Conclusões, Limitações e Próximos Passos
- Principais achados sobre padrões de atraso e desempenho dos modelos
- Limitações (único ano, ausência de dados externos, amostragem)
- Sugestões de melhorias: XGBoost/LightGBM, histórico da aeronave, dados de clima, deploy via FastAPI

## Principais Insights

- Voos da manhã apresentam os menores índices de atraso; o pico ocorre no período noturno
- Feriados concentram atrasos médios superiores aos dias normais
- `ORIGIN_ENC` (target encoding do aeroporto de origem), `HOUR` e `DISTANCE` são as features mais importantes para classificação
- K-Means (K=3) separou as companhias em perfis distintos de pontualidade e operação
- Isolation Forest identificou ~1% dos voos com perfil anômalo, com médias de atraso e taxi time substancialmente maiores

## Tecnologias Utilizadas

- Python 3
- pandas, numpy
- matplotlib, seaborn
- scikit-learn (LogisticRegression, RandomForestClassifier, RandomForestRegressor, GradientBoostingRegressor, IsolationForest, KMeans, PCA, StandardScaler, StratifiedKFold)
- Jupyter Notebook

## Como Executar

1. Clone o repositório (requer Git LFS instalado para baixar o arquivo `flights.csv`):

```bash
git lfs install
git clone https://github.com/GabrielPeixer/delay_voos.git
cd delay_voos
```

2. Instale as dependências:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

3. Execute o notebook:

```bash
jupyter notebook tech_challenge_3.ipynb
```

## Observação sobre o Dataset

O arquivo `flights.csv` possui aproximadamente 565 MB e é armazenado com Git Large File Storage (LFS). Para baixá-lo corretamente, certifique-se de ter o Git LFS instalado antes de clonar o repositório (`git lfs install`).
