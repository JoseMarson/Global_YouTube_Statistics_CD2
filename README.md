# Global YouTube Statistics — Análise Exploratória & Pré-processamento

> Trabalho desenvolvido no âmbito de disciplina de Ciência de Dados 2.  
> **Grupo:**  Eduardo Araujo · Eduardo Marson ·José Vitor Marson · Hugo Viana · Maria Clara

---

## Sumário

1. [Visão Geral](#-visão-geral)
2. [Dataset](#-dataset)
3. [Estrutura do Projeto](#-estrutura-do-projeto)
4. [Requisitos e Instalação](#-requisitos-e-instalação)
5. [Como Reproduzir](#-como-reproduzir)
6. [Etapas do Notebook](#-etapas-do-notebook)
   - [0. Configuração do Ambiente](#etapa-0--configuração-do-ambiente)
   - [1. Exploração Inicial](#etapa-1--exploração-inicial)
   - [2. Análise Exploratória de Dados (EDA)](#etapa-2--análise-exploratória-de-dados-eda)
   - [3. Pré-processamento](#etapa-3--pré-processamento)
   - [4. Resumo e Artefatos Gerados](#etapa-4--resumo-e-artefatos-gerados)
7. [Decisões Técnicas](#-decisões-técnicas)
8. [Artefatos de Saída](#-artefatos-de-saída)
9. [Integrantes](#-integrantes)

---

## Visão Geral

Este notebook realiza a **análise exploratória completa e o pré-processamento** do dataset *Global YouTube Statistics*, que reúne métricas de engajamento, monetização e dados demográficos dos 995 maiores canais do YouTube no mundo.

O pipeline cobre desde a carga e inspeção inicial dos dados até a entrega de três DataFrames prontos para modelagem:

| DataFrame | Escalonamento | Uso recomendado |
|-----------|---------------|-----------------|
| `df_tratado` | Sem escala (log1p aplicado) | Análises exploratórias, referência canônica |
| `df_std` | StandardScaler | PCA, SVM, Regressão Logística |
| `df_mm` | MinMaxScaler | KNN, Redes Neurais |

---

## Dataset

| Propriedade | Valor |
|-------------|-------|
| Arquivo | `Global YouTube Statistics.csv` |
| Registros | 995 canais |
| Variáveis originais | 28 colunas |
| Variáveis finais | 23 colunas |
| Fonte original | [Kaggle – Global YouTube Statistics 2023](https://www.kaggle.com/datasets/nelgiriyewithana/global-youtube-statistics-2023) |

**Principais colunas do dataset:**

| Coluna | Descrição |
|--------|-----------|
| `Youtuber` | Nome do canal |
| `subscribers` | Total de inscritos |
| `video views` | Total de visualizações |
| `uploads` | Número de vídeos publicados |
| `category` / `channel_type` | Categoria e tipo do canal |
| `Country` | País de origem |
| `lowest/highest_monthly_earnings` | Faixa de ganhos mensais estimados |
| `lowest/highest_yearly_earnings` | Faixa de ganhos anuais estimados |
| `created_year` | Ano de criação do canal |
| `subscribers_for_last_30_days` | Novos inscritos nos últimos 30 dias |
| `video_views_for_the_last_30_days` | Visualizações nos últimos 30 dias |
| `Population` / `Urban_population` | Dados demográficos do país |
| `Unemployment rate` | Taxa de desemprego do país |
| `Gross tertiary education enrollment (%)` | Matrícula no ensino superior |
| `Latitude` / `Longitude` | Coordenadas geográficas do país |

---

## Estrutura do Projeto

```
.
├── GlobalYoutubeStatistics.ipynb   # Notebook principal
├── Global YouTube Statistics.csv  # Dataset de entrada (necessário)
├── df_tratado.csv                  # Saída: dados limpos (log1p, sem escala)
├── df_std.csv                      # Saída: dados com StandardScaler
├── df_mm.csv                       # Saída: dados com MinMaxScaler
└── README.md                       # Este arquivo
```


---

## ⚙️ Requisitos e Instalação

### Python

Recomenda-se **Python 3.9+**.

### Dependências

Instale todas as dependências com um único comando:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn chardet
```

Ou, se houver um `requirements.txt`:

```bash
pip install -r requirements.txt
```

**Lista detalhada de pacotes:**

| Pacote | Versão testada | Finalidade |
|--------|---------------|------------|
| `numpy` | ≥ 1.23 | Operações numéricas e transformações |
| `pandas` | ≥ 1.5 | Manipulação de DataFrames |
| `matplotlib` | ≥ 3.6 | Visualizações estáticas |
| `seaborn` | ≥ 0.12 | Gráficos estatísticos (heatmaps, boxplots) |
| `scikit-learn` | ≥ 1.1 | `StandardScaler` e `MinMaxScaler` |
| `chardet` | ≥ 5.0 | Detecção automática de encoding do CSV |

### Ambientes compatíveis

| Ambiente | Suporte | Observação |
|----------|---------|------------|
| **Google Colab** | ✅ Recomendado | Upload automático do CSV via `files.upload()` |
| **Jupyter Lab / Notebook** | ✅ | Coloque o CSV na mesma pasta do `.ipynb` |
| **VS Code (Jupyter)** | ✅ | Idem ao Jupyter local |

---

## Como Reproduzir

Siga o roteiro abaixo na ordem indicada para reproduzir completamente a análise:

---

### Passo 1 — Clonar ou baixar o repositório

```bash
git clone https://github.com/JoseMarson/Global_YouTube_Statistics_CD2.git
cd <seu-repositorio>
```

Ou simplesmente baixe e descompacte o ZIP do projeto.

---

### Passo 2 — Obter o dataset

1. Acesse [https://www.kaggle.com/datasets/nelgiriyewithana/global-youtube-statistics-2023](https://www.kaggle.com/datasets/nelgiriyewithana/global-youtube-statistics-2023)
2. Faça o download do arquivo `Global YouTube Statistics.csv`
3. Coloque o CSV **na mesma pasta** do notebook `GlobalYoutubeStatistics.ipynb`

---

### Passo 3 — Instalar as dependências

```bash
pip install numpy pandas matplotlib seaborn scikit-learn chardet
```

---

### Passo 4 — Abrir o notebook

**Localmente (Jupyter):**
```bash
jupyter notebook GlobalYoutubeStatistics.ipynb
```

**Google Colab:**
1. Acesse [colab.research.google.com](https://colab.research.google.com)
2. Faça o upload do arquivo `.ipynb`
3. Na primeira célula de carregamento do CSV, o notebook detectará automaticamente o Colab e solicitará o upload do arquivo via `files.upload()`

---

### Passo 5 — Executar todas as células

No Jupyter ou Colab, use **`Kernel → Restart & Run All`** (ou `Runtime → Run All` no Colab).

> O notebook foi projetado para execução linear, de cima para baixo, sem necessidade de ajuste manual entre células.

---

### Passo 6 — Verificar as saídas

Ao final da execução, os seguintes arquivos serão gerados na pasta do projeto (ou baixados automaticamente no Colab):

```
df_tratado.csv   # 995 linhas × 23 colunas — versão log1p sem escala
df_std.csv       # 995 linhas × 23 colunas — StandardScaler
df_mm.csv        # 995 linhas × 23 colunas — MinMaxScaler
```

---

## Etapas do Notebook

### Etapa 0 — Configuração do Ambiente

- Importação de todas as bibliotecas necessárias
- Configuração do backend `Agg` do Matplotlib (garante renderização mesmo em ambientes sem display)
- Override de `plt.show()` para exibição inline como imagem PNG
- Supressão de warnings irrelevantes
- Detecção automática do encoding do CSV via `chardet`
- Lógica de carregamento adaptável: detecta o caminho do arquivo localmente ou solicita upload no Google Colab

---

### Etapa 1 — Exploração Inicial

**Objetivo:** compreender a estrutura e qualidade bruta dos dados.

| Sub-etapa | Descrição |
|-----------|-----------|
| **Shape e tipos** | Verificação das dimensões (`995 × 28`) e tipos de cada coluna (`df.dtypes`) |
| **Estatísticas descritivas** | `df.describe(include='all')` com todas as colunas, incluindo categóricas |
| **Mapa de valores faltantes** | Tabela com contagem e percentual de nulos por coluna |
| **Heatmap de missings** | Visualização espacial com `seaborn.heatmap` + paleta viridis para identificar padrões de ausência |

**Principais achados:** `subscribers_for_last_30_days` apresentou 337 valores ausentes (34%), sinalizando potencial padrão informativo (canais inativos). Demais faltâncias concentradas em colunas geográficas (~122 registros).

---

### Etapa 2 — Análise Exploratória de Dados (EDA)

**Objetivo:** entender distribuições, relações e padrões entre variáveis.

#### 2.1 Histogramas de distribuição

- 14 colunas numéricas plotadas em grade (4 colunas × N linhas)
- Colunas com distribuições assimétricas (`subscribers`, `video views`, `earnings` etc.) recebem escala logarítmica no eixo X automaticamente
- Permite identificar visualmente a forte assimetria positiva típica de dados de plataformas digitais

#### 2.2 Boxplots (escala log)

- 9 colunas de engajamento e monetização
- Eixo X em escala log para evidenciar outliers superiores reais (canais como T-Series, MrBeast, Cocomelon)

#### 2.3 Gráficos de barras horizontais

- **Top 15 Categorias** de canal (ex: Entertainment, Music, Gaming)
- **Top 15 Tipos de canal** (channel_type)
- **Top 20 Países** com mais canais representados

#### 2.4 Matriz de Correlação de Spearman — dados brutos

- Cobertura de 14 variáveis numéricas
- Método Spearman escolhido por ser **robusto a distribuições assimétricas** e capturar relações **monotônicas não-lineares**
- Máscara triangular superior para leitura limpa
- Formato: heatmap anotado `coolwarm` com valores entre −1 e +1

#### 2.5 Matriz de Correlação de Pearson — dados brutos

- Complementar à de Spearman para contraste de relações lineares
- Evidencia como coeficientes de Pearson são inflados ou distorcidos pelas caudas longas das distribuições

#### 2.6 Scatter plots log-log

6 pares de variáveis plotados em escala log-log:

| Par | Insight esperado |
|-----|-----------------|
| `subscribers` × `video views` | Correlação positiva forte |
| `lowest` × `highest_monthly_earnings` | Proporcionalidade direta |
| `lowest` × `highest_yearly_earnings` | Idem anual |
| `subscribers` × `highest_monthly_earnings` | Ganhos crescem com audiência |
| `video views` × `highest_monthly_earnings` | Visualizações como proxy de receita |
| `subscribers` × `subscribers_for_last_30_days` | Canais maiores crescem mais em absoluto |

---

### Etapa 3 — Pré-processamento

#### 3.1 Tratamento de Valores Faltantes

Estratégia diferenciada por coluna, justificada pela natureza de cada variável:

| Coluna(s) | Estratégia | Justificativa |
|-----------|-----------|---------------|
| `category`, `channel_type` | Moda | Classe dominante; baixa faltância |
| `Country`, `Abbreviation` | Sentinela `'Unknown'` / `'UNK'` | Sem dado confiável; preserva o grupo para análise |
| Andorra (6 variáveis geográficas) | Lookup manual com dados de 2023 | Único caso com `Country` preenchido mas sem dados demográficos |
| `Population`, `Urban_population`, `Unemployment rate`, `Gross tertiary education (%)`, `Latitude`, `Longitude` | Mediana | Distribuições assimétricas; mediana é robusta a outliers |
| `subscribers_for_last_30_days` | Flag binária + Mediana | Alta faltância (34%) potencialmente informativa; coluna `subscribers_for_last_30_days_missing` criada antes da imputação |
| `country_rank` | Flag binária + Sentinela −1 | Ordinal; −1 inequívoco como "sem rank"; `country_rank_missing` criado antes |
| `video_views_rank`, `channel_type_rank`, `video_views_for_the_last_30_days`, `created_year`, `created_date` | Mediana | Baixa faltância; distribuições assimétricas |
| `created_month` | Moda | Categórica (nome do mês) |

> **Nota sobre outliers:** identificados via IQR mas **intencionalmente preservados**. Registros como T-Series (245M inscritos), MrBeast (166M) e Cocomelon (162M) são dados reais e válidos. Removê-los enviaria modelos futuros na direção de canais de médio porte, perdendo informação valiosa sobre o comportamento do topo do mercado.

---

#### 3.2 Feature Engineering

8 novas variáveis derivadas criadas para enriquecer o poder explicativo do dataset:

**Indicadores de ausência (flags binárias)**

Criadas *antes* da imputação, de modo a preservar a informação de que o valor original estava faltante — ausência que pode ser, por si só, um sinal relevante para modelos futuros:

| Feature | Origem | Interpretação |
|---------|--------|---------------|
| `subscribers_for_last_30_days_missing` | `1` se `subscribers_for_last_30_days` era nulo, `0` caso contrário | Potencial indicador de canal inativo ou sem crescimento recente (34% de ausência) |
| `country_rank_missing` | `1` se `country_rank` era nulo, `0` caso contrário | Indica canal sem ranking nacional registrado |

**Features contínuas derivadas**

| Feature | Fórmula | Interpretação |
|---------|---------|---------------|
| `channel_age` | `2023 − created_year` | Maturidade do canal em anos (referência: 2023) |
| `views_per_subscriber` | `video views ÷ subscribers` | Eficiência de consumo: quantas views cada inscrito gera em média |
| `views_per_upload` | `video views ÷ uploads` (0 se uploads = 0) | Performance média por vídeo publicado |
| `earnings_spread` | `highest_yearly_earnings − lowest_yearly_earnings` | Amplitude/volatilidade da monetização anual |
| `urban_ratio` | `Urban_population ÷ Population` | Taxa de urbanização do país de origem do canal |
| `subscriber_growth_rate` | `subscribers_for_last_30_days ÷ subscribers` | Crescimento mensal relativo ao tamanho do canal |

---

#### 3.3 Remoção de Features Redundantes

1. Correlações de Spearman calculadas entre todas as colunas numéricas
2. Pares com correlação absoluta ≥ 0.97 identificados e exibidos
3. Remoção de 13 colunas redundantes ou de baixo valor analítico:

```python
DROP_COLS = [
    'rank', 'video_views_rank', 'channel_type_rank',
    'Abbreviation',
    'lowest_yearly_earnings', 'highest_yearly_earnings', 'lowest_monthly_earnings',
    'created_year', 'created_month', 'created_date',
    'Urban_population', 'Latitude', 'Longitude',
]
```

**Resultado:** `995 linhas × 23 colunas` (de 28 + 8 novas − 13 removidas).

---

#### 3.4 Transformação Logarítmica (log1p)

11 colunas com distribuição altamente assimétrica transformadas com `numpy.log1p` para aproximar normalidade:

```
subscribers, video views, uploads,
video_views_for_the_last_30_days, highest_monthly_earnings,
earnings_spread, subscribers_for_last_30_days, Population,
views_per_subscriber, views_per_upload, subscriber_growth_rate
```

> **Por que `log1p` e não `log`?** `log1p(x) = log(1 + x)` é seguro para valores zero, evitando erros ou infinitos negativos — crítico para colunas como `subscriber_growth_rate` e `views_per_upload`.

Histogramas pós-transformação são plotados para validação visual da redução de assimetria.

---

#### 3.5 Normalização

Dois scalers aplicados sobre 13 colunas contínuas (`SCALE_COLS`), gerando cópias independentes:

| Scaler | DataFrame | Resultado | Ideal para |
|--------|-----------|-----------|------------|
| `StandardScaler` | `df_std` | μ = 0, σ = 1 | PCA, SVM, Regressão Logística, Lasso/Ridge |
| `MinMaxScaler` | `df_mm` | [0, 1] | KNN, Redes Neurais, algoritmos baseados em distância |

O DataFrame `df_tratado` permanece como **versão canônica não normalizada** (mas com log1p aplicado), recomendada para análises exploratórias e interpretabilidade.

Comparativo de distribuição de `subscribers` nas três versões plotado para validação.

---

### Etapa 4 — Resumo e Artefatos Gerados

- Tabela-resumo de todas as estratégias de imputação
- Tabela de features engineered (2 flags binárias de ausência + 6 features contínuas derivadas)
- Comparativo de normalização
- Matrizes de correlação de Spearman e Pearson do dataset final
- Exportação dos três DataFrames como `.csv`

---

##  Decisões Técnicas

| Decisão | Alternativa considerada | Motivo da escolha |
|---------|------------------------|-------------------|
| Correlação de **Spearman** como padrão | Pearson | Dados com caudas longas; Spearman é robusto a outliers e distribucional |
| **Preservar** outliers | Remoção por IQR/Z-score | Outliers são canais reais e válidos; remoção introduziria viés de seleção |
| Flag binária para `subscribers_for_last_30_days` | Apenas imputar mediana | Alta taxa de ausência (34%) pode ser informativa — canais inativos têm comportamento distinto |
| **`log1p`** em vez de `log` | `log`, Box-Cox | Segurança para zeros; simples; amplamente interpretável |
| Dois DataFrames normalizados (`std` + `mm`) | Apenas um | Diferentes algoritmos de ML exigem diferentes escalas; entregar os dois evita reprocessamento futuro |

---

##  Artefatos de Saída

| Arquivo | Linhas | Colunas | Escalonamento |
|---------|--------|---------|---------------|
| `df_tratado.csv` | 995 | 23 | log1p (sem normalização) |
| `df_std.csv` | 995 | 23 | log1p + StandardScaler |
| `df_mm.csv` | 995 | 23 | log1p + MinMaxScaler |

---

## Integrantes

| Nome | 
|------|
| Eduardo de Oliveira Araujo |
| Eduardo Oliveira Marson |
| José Vitor Oliveira Marson |
| Hugo Alves Viana |
| Maria Clara Sailva Borges|

---
