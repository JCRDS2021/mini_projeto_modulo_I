# Análise Exploratória de Dados: Base Varejo

Mini projeto do Módulo I com foco em análise exploratória de dados (EDA) de um conjunto de dados de varejo, utilizando Python e pandas.

## Objetivo

Compreender a estrutura da base de vendas, garantir a qualidade dos dados (tipos, duplicatas e valores não informados) e investigar o comportamento de compra dos clientes, relacionando o perfil (gênero, segmento, estado civil e número de filhos) com os produtos adquiridos.

## Fonte dos dados

- **Dataset:** [Base Varejo](https://www.kaggle.com/datasets/namespaiva/base-varejo) (Kaggle, `namespaiva/base-varejo`)
- **Arquivo:** `Base Varejo.csv` (separador `;`, codificação `latin-1`)
- **Volume:** 733.447 linhas e 11 colunas no DataFrame consolidado (após a etapa de tratamento das duplicatas)
- **Período:** transações a partir de fevereiro de 2019, com registros até agosto de 2022

### Dicionário de colunas

| Coluna | Descrição |
|---|---|
| `DATA` | Data da compra |
| `CO_ID` | Identificador da compra (pedido) |
| `CL_ID` | Identificador do cliente |
| `CL_GENERO` | Gênero do cliente (`M` / `F`) |
| `CL_EC` | Estado civil do cliente (codificado) |
| `CL_FHL` | Número de filhos do cliente (0 a 4) |
| `CL_SEG` | Segmento do cliente (A, B, C...) |
| `PR_ID` | Identificador do produto |
| `PR_CAT` | Categoria do produto (ex.: ALIMENTOS, LIMPEZA) |
| `PR_NOME` | Nome do produto |
| `PR_QTD` | Quantidade do produto na compra (coluna criada na consolidação, ver etapa 3) |

## Estrutura do notebook

O notebook está organizado em quatro seções:

### 1. Importação dos dados e transformação em DataFrame

- Download do dataset via `kagglehub`.
- Leitura do CSV com `sep=';'` e `encoding='latin-1'`.

### 2. Análises básicas: conhecendo o DataFrame

- Visualização das primeiras e últimas linhas (`head` e `tail`).
- Dimensões do DataFrame (`shape`) e informações gerais (`info`).
- Estatísticas descritivas (`describe`).
- Contagem de linhas duplicadas.

### 3. Tratamento e limpeza dos dados

- **Colunas desnecessárias:** remoção das colunas vazias geradas na importação (`Unnamed: 10` a `Unnamed: 13`).
- **Datas:** `DATA` convertida para `datetime` (formato `dd/mm/aaaa`).
- **Tipos de dados:**
  - `CL_GENERO` e `CL_SEG` convertidas para `category`.
  - `PR_CAT` e `PR_NOME` convertidas para `string`.
- **Duplicatas:** investigação das linhas 100% idênticas, para entender se representam compras repetidas do mesmo produto na mesma transação ou registros duplicados por erro.
- **Consolidação e criação de `PR_QTD`:** em vez de excluir as duplicatas e perder informação de vendas, as linhas idênticas foram agrupadas (`groupby` em todas as colunas) e o tamanho de cada grupo passou a ser a quantidade do produto na compra (`PR_QTD`). O resultado é o DataFrame `df_consolidado`, ordenado por `CO_ID` e `PR_ID`, com o índice reiniciado. Como verificação, a soma de `PR_QTD` é comparada com o número de linhas do DataFrame original: se forem iguais, nenhuma linha foi perdida na consolidação.
- **Valores não informados:** nas colunas `PR_CAT` e `PR_NOME`, o valor `#N/D` (erro do Excel gravado como texto no CSV, que o pandas não reconhece como nulo) foi substituído por `Não informado`. Na coluna `PR_CAT`, eram 3.228 ocorrências.

### 4. Insights

- **Número de filhos por cliente:** como cada cliente (`CL_ID`) aparece em várias compras, foi criado um DataFrame com uma linha por cliente, usando `groupby('CL_ID')` com `.first()`. A premissa de que o número de filhos é constante para cada cliente é validada contando os clientes com mais de um valor distinto de `CL_FHL` (resultado `0` indica que a premissa se sustenta).
- **Compras por gênero:** agrupamento por `CL_GENERO` com soma, média, contagem, mínimo e máximo da quantidade comprada (`PR_QTD`).

## Premissas e limitações

- **Linhas idênticas = compras repetidas.** Linhas totalmente iguais foram tratadas como o mesmo produto levado mais de uma vez na mesma compra, e não como erro de registro. Essa interpretação não pode ser confirmada apenas com os dados.
- **Número de filhos constante por cliente.** Assume-se que `CL_FHL` é sempre o mesmo para um mesmo `CL_ID`, por isso o uso de `.first()`. O notebook contém uma célula que valida essa premissa.

## Tecnologias utilizadas

- Python 3
- pandas
- kagglehub
- Google Colab / Jupyter Notebook

## Estrutura do repositório

```
.
├── mini_projeto_modulo_I.ipynb   # Notebook com a análise
├── Base Varejo.csv               # Conjunto de dados
└── README.md
```

## Como executar

1. Clone o repositório:
   ```bash
   git clone https://github.com/SEU_USUARIO/NOME_DO_REPOSITORIO.git
   cd NOME_DO_REPOSITORIO
   ```
2. Instale as dependências:
   ```bash
   pip install pandas numpy kagglehub
   ```
3. Abra o notebook no Jupyter, VS Code ou Google Colab e execute as células em ordem.

> O notebook baixa o dataset automaticamente pelo `kagglehub`. Caso prefira usar o arquivo local, ajuste a variável `arquivo_csv` para o caminho do `Base Varejo.csv`.

## Autor

**Jean Carlos Rodrigues**
[LinkedIn](https://www.linkedin.com/in/jeanrodrigues97/) · [GitHub](https://github.com/JCRDS2021)
