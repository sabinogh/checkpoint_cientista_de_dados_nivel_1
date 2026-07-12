# Checkpoint Cientista de Dados - Nível 1 (Alura)

Projeto de Ciência de Dados desenvolvido como checkpoint de avaliação da trilha **Cientista de Dados** da Alura. O objetivo é analisar o perfil de clientes da (fictícia) empresa **Health&Life Analytics** e investigar como o consumo de café se relaciona com hábitos de saúde e qualidade do sono, culminando na construção de um modelo preditivo de classificação.

## Objetivo do projeto

O projeto está dividido em três entregas:

1. **Análise Exploratória de Dados (EDA)**: Entender a estrutura do dataset, identificar padrões, nulos e possíveis problemas nos dados.
2. **Visualização e Insights**: Construir gráficos comparativos e segmentados que relacionem café, estresse e sono, documentando os achados em uma seção de Principais Descobertas.
3. **Modelo Preditivo**: Pré-processar os dados e treinar modelos de classificação para prever a qualidade do sono (`Sleep_Quality`) dos clientes, com recomendações de negócio baseadas nos resultados.

## Dataset

O dataset utilizado é sintético, contendo **10.000 registros** e **16 colunas**, com informações demográficas (idade, gênero, país, ocupação), hábitos (consumo de café e cafeína, atividade física, tabagismo, álcool) e indicadores de saúde (IMC, frequência cardíaca, nível de estresse, qualidade do sono).

O arquivo CSV original está disponível na pasta [`/data`](./data) deste repositório.

## Estrutura do repositório

```
├── Checkpoint Cientista de Dados - Nível 1.ipynb   # Notebook principal com todas as etapas
├── data/
│   └── dataset_original.csv                        # Dataset original utilizado na análise
├── dataset_processado_final.csv                    # Dataset após pré-processamento (usado no modelo)
├── modelo_sleep_quality_rf.pkl                      # Modelo Random Forest treinado (salvo com joblib)
└── README.md
```

## Como rodar o projeto

Este notebook foi desenvolvido e testado no **Google Colab**, com o dataset carregado a partir do Google Drive. Para rodar:

1. Abra o notebook `Checkpoint Cientista de Dados - Nível 1.ipynb` no [Google Colab](https://colab.research.google.com/).
2. Faça upload do arquivo `dataset_original.csv` (disponível em `/data`) para o seu Google Drive, ajustando o caminho na célula de leitura do dataset:
   ```python
   dados = pd.read_csv('/content/drive/MyDrive/<seu-caminho>/dataset_original.csv')
   ```
3. Conecte o Google Drive quando solicitado (`from google.colab import drive`).
4. Execute as células em sequência: **Ambiente de execução → Executar tudo** (ou `Kernel → Restart and Run All`, se estiver usando Jupyter local).

> O notebook foi validado rodando do início ao fim sem erros de execução (Restart and Run All).

### Bibliotecas necessárias

```
pandas
numpy
matplotlib
seaborn
scikit-learn
joblib
```

Todas já vêm pré-instaladas no ambiente padrão do Google Colab. Para rodar localmente:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn joblib
```

## Entrega #1 — Análise Exploratória de Dados (EDA)

- Verificação de dimensões, tipos de dados, valores nulos e duplicados.
- Estatísticas descritivas gerais e segmentadas.
- Histogramas e boxplots para variáveis numéricas (idade, consumo de café, IMC, frequência cardíaca, cafeína).
- Gráficos de barras e pizza para variáveis categóricas (gênero, tabagismo, consumo de álcool, nível de estresse).
- Matriz de correlação (heatmap) entre variáveis numéricas.
- Identificação de um achado relevante: a coluna `Health_Issues` possui alto índice de valores nulos (~59%), tratado na etapa de modelagem.

## Entrega #2 — Visualização e Insights

- Gráficos comparativos segmentados por grupo: sono por gênero, sono por nível de estresse, café por nível de estresse, e a combinação idade/estresse/café.
- Cálculo de médias por grupo (`groupby`) para sustentar os insights com números exatos, não apenas leitura visual.
- Seção **Principais Descobertas** com a interpretação de cada gráfico.
- Seção **Conclusão Geral**, sintetizando que o nível de estresse é o fator mais relevante associado ao sono, com o café tendo papel secundário, e sem diferenças relevantes por gênero ou idade.

## Entrega #3 — Modelo Preditivo

**Pré-processamento:**
- Remoção de colunas irrelevantes (`ID`) e com excesso de nulos (`Health_Issues`).
- Codificação ordinal para variáveis com hierarquia natural (`Stress_Level`, `Sleep_Quality`).
- One-hot encoding para variáveis categóricas sem ordem (`Gender`, `Country`, `Occupation`).
- Criação de feature derivada (`Caffeine_per_Activity_Hour`).

**Um achado importante do processo:** a primeira versão do modelo apresentou acurácia de ~99%, o que indicou **vazamento de dado** (data leakage) — a coluna `Sleep_Hours` tinha relação quase determinística com o alvo `Sleep_Quality`. Após identificar e remover essa coluna (e refazer a feature derivada sem depender dela), a acurácia caiu para um patamar realista (~86%), refletindo um modelo que aprende de hábitos reais, não de uma fórmula disfarçada.

**Modelagem:**
- Divisão treino/teste (80/20, estratificada pelo alvo).
- Dois modelos de classificação treinados: **Regressão Logística** e **Random Forest**.
- Avaliação com acurácia, matriz de confusão e relatório de classificação.
- Identificação de viés por desbalanceamento de classes (nenhum modelo previa a classe "Excellent" inicialmente) e mitigação com `class_weight='balanced'`.
- Visualização de uma árvore de decisão do Random Forest para interpretabilidade.

**Resultado final:**
- Modelo escolhido: **Random Forest balanceado** (`class_weight='balanced'`), com acurácia de ~86,5%.
- Dataset final processado salvo em `dataset_processado_final.csv`.
- Modelo salvo em `modelo_sleep_quality_rf.pkl` (via `joblib`).

**Seção Recomendações para o Negócio** incluída no notebook, traduzindo a importância das variáveis do modelo (com destaque para o nível de estresse) em ações práticas para a Health&Life Analytics.

## Autor

Guilherme Henrique Sabino

Projeto de Ciências de Dados Nível 1 - Alura
