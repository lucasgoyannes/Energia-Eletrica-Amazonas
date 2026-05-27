# Acesso à Energia Elétrica no Amazonas: Análise Geoespacial

Projeto acadêmico com foco em **inclusão energética** no estado do Amazonas. A análise cobre todo o pipeline de dados: desde o ETL com dados reais do Censo IBGE 2022 até visualizações voltadas para tomadores de decisão públicos.

---

## Objetivo

Mapear e analisar o acesso à energia elétrica nos 62 municípios do Amazonas, identificando padrões de exclusão energética por município, por zona rural, urbana e por mesorregião, com foco em subsidiar políticas públicas da ANEEL e do Governo do Estado.

---

## Estrutura do projeto

```
📁 projeto/
├── 01_etl.ipynb                    # Pipeline de ETL
├── 02_analise.ipynb                # Análise exploratória e correlação
├── 03_visualizacao.ipynb           # 5 visualizações para gestores públicos
├── energia-am-municipios.csv       # Dados de energia por município
├── energia-am-rural-urbano.csv     # Dados por município e zona (rural/urbana)
└── geojs-13-mun.json               # Geometria dos municípios do AM (IBGE)
```

---

## Notebooks

### 01: ETL
- Leitura dos dados de domicílios por município
- Reprojeção do sistema de coordenadas para **EPSG:31980** (SIRGAS 2000 / UTM zona 20S)
- Join espacial entre dados tabulares e geometria dos municípios
- Cálculo da distância de cada município até Manaus (em km)
- Exportação dos arquivos `municipios-am.geojson` e `energia-am.geojson`

### 02: Análise Exploratória
Responde 3 perguntas quantitativas:
1. Qual o percentual de domicílios sem acesso à energia por município?
2. Quais os 15 municípios com maior exclusão energética? (ranking)
3. Como a exclusão varia entre zona rural e urbana no estado?

Além da análise de **correlação de Pearson** entre distância à capital e nível de exclusão energética.

### 03: Visualizações
5 visualizações desenvolvidas para um público não-técnico (gestores públicos):

| # | Visualização |
|---|---|
| 1 | Mapa coroplético: % sem energia por município |
| 2 | Gráfico de barras: top 15 municípios com maior exclusão |
| 3 | Scatter: distância a Manaus × % sem energia |
| 4 | Mapa de bolhas: porte do município × exclusão energética |
| 5 | Gráfico agrupado: rural vs urbano por mesorregião |

---

## Principais achados

- Municípios do **interior remoto** do Amazonas concentram os maiores índices de exclusão energética, especialmente aqueles fora do Sistema Interligado Nacional (SIN) e dependentes de Sistemas Isolados (SISOL) a diesel
- Municípios como **São Gabriel da Cachoeira, Ipixuna, Barcelos e Atalaia do Norte** apresentam os piores indicadores, com mais de 40% dos domicílios rurais sem acesso adequado
- A **zona rural** do AM tem índice de exclusão energética aproximadamente 8x maior que a zona urbana
- Municípios conectados ao SIN (Manaus, Iranduba, Itacoatiara, Parintins) apresentam os menores índices do estado
- A correlação entre distância à capital e exclusão energética é positiva, mas moderada: o principal fator determinante é a integração ou não ao SIN, não apenas a distância geográfica

---

## Tecnologias utilizadas

| Biblioteca | Uso |
|---|---|
| `pandas` | Manipulação e agregação de dados tabulares |
| `geopandas` | Dados geoespaciais, reprojeção e join espacial |
| `matplotlib` | Geração de todos os gráficos e mapas |
| `scipy` | Cálculo de correlação estatística (Pearson) |
| `numpy` | Operações numéricas auxiliares |
| `shapely` | Manipulação de geometrias (pontos, polígonos) |

---

## Como rodar

### Pré-requisitos

```bash
pip install pandas geopandas matplotlib scipy numpy
```

### Execução

Abra os notebooks **na ordem** dentro da mesma pasta:

```
01_etl.ipynb  →  02_analise.ipynb  →  03_visualizacao.ipynb
```

O notebook 01 gera os arquivos `municipios-am.geojson` e `energia-am.geojson`, que são utilizados pelos notebooks seguintes. Os arquivos CSV e o GeoJSON já estão incluídos no repositório.

---

## Fontes de dados

- **Censo Demográfico 2022**: IBGE (Agregados por Setores Censitários)
- **Malha municipal do Amazonas**: IBGE (GeoJSON)
- **Dados de cobertura energética**: ANEEL / IEMA (estimativas por município)

---

## Contexto

O Amazonas possui 62 municípios, dos quais apenas 6 estão integrados ao **Sistema Interligado Nacional (SIN)**. Os demais dependem de **Sistemas Isolados (SISOL)**, majoritariamente movidos a diesel: o que resulta em energia mais cara, menos confiável e ambientalmente mais impactante. O estado concentra o maior número de SISOL da Amazônia Legal, com cerca de 97 sistemas isolados em operação.

---

## Gestor-alvo

**ANEEL / Governo do Amazonas**  
Projeção utilizada: EPSG:31980 (SIRGAS 2000 / UTM zona 20S)
