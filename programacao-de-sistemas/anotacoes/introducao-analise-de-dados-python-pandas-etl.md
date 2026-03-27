# Aula 26/03 — Introdução à Análise de Dados com Python, ETL e Pandas

> Anotações organizadas em Markdown a partir da transcrição da aula.

## Sumário

- [Visão geral da aula](#visão-geral-da-aula)
- [Objetivo da aula](#objetivo-da-aula)
- [Tecnologias mencionadas](#tecnologias-mencionadas)
- [ETL: conceito e etapas](#etl-conceito-e-etapas)
  - [Extração](#1-extração-extract)
  - [Transformação](#2-transformação-transform)
  - [Carga / Exportação](#3-carga--exportação-load)
- [Introdução ao Python para análise de dados](#introdução-ao-python-para-análise-de-dados)
  - [Variáveis](#1-variáveis-no-python)
  - [Listas](#2-listas)
  - [Dicionários](#3-dicionários)
  - [Leitura de arquivos](#4-leitura-de-arquivos)
- [Introdução ao Pandas](#introdução-ao-pandas)
  - [DataFrame](#1-o-que-é-um-dataframe)
  - [Leitura de arquivos com Pandas](#2-leitura-de-arquivos-com-pandas)
  - [Inspeção inicial dos dados](#3-primeira-inspeção-de-um-dataframe)
  - [Seleção e filtragem](#4-seleção-e-filtragem-de-dados)
- [Exemplo prático com a base `adult.csv`](#exemplo-prático-com-a-base-adultcsv)
- [Relação com machine learning](#relação-com-machine-learning)
- [Principais ideias da aula](#principais-ideias-da-aula)
- [Conclusão](#conclusão)

---

## Visão geral da aula

A aula apresentou uma introdução à **análise de dados**, com foco no processo de **ETL** e no uso de **Python** e **Pandas** para manipulação, organização e interpretação de dados.

A proposta central foi entender como transformar **dados brutos** em **informações úteis para a tomada de decisão**, além de revisar conceitos básicos da linguagem Python e iniciar o uso da biblioteca Pandas.

---

## Objetivo da aula

O objetivo principal foi desenvolver uma visão **analítica e crítica** sobre os dados, de modo que seja possível:

- tratar, organizar e interpretar bases de dados;
- formular perguntas relevantes a partir dos dados;
- interpretar resultados;
- extrair insights;
- utilizar dados como base para decisões mais seguras, eficientes e embasadas.

### Exemplo discutido em aula

Um exemplo citado foi o seguinte:

> Por que mulheres entram menos em cursos de tecnologia e engenharia?

A base de dados pode mostrar esse comportamento e, a partir disso, permitir investigações mais profundas sobre suas possíveis causas. Assim, a análise de dados ajuda a construir um raciocínio crítico sobre fenômenos sociais, educacionais e profissionais.

---

## Tecnologias mencionadas

As ferramentas citadas na aula foram:

- **Python**
- **Pandas**
- **Matplotlib**
- **Google Colab**
- **VS Code**

> **Observação:** o Colab foi apresentado como um ambiente semelhante ao **Jupyter Notebook**, funcionando com execução por células.

### Sobre o Matplotlib

Foi comentado que o **Matplotlib** é uma biblioteca de gráficos que, em muitos casos, aparece de forma integrada ao uso do Pandas. Assim, algumas visualizações podem ser feitas sem que o usuário perceba explicitamente todas as chamadas da biblioteca.

---

# ETL: conceito e etapas

**ETL** significa:

- **E**xtract
- **T**ransform
- **L**oad

Em português, pode ser entendido como:

- **Extração**
- **Transformação**
- **Carga / Carregamento / Exportação**

O ETL é o processo que permite obter os dados, tratá-los e salvá-los para uso posterior.

---

## 1. Extração (Extract)

A etapa de extração consiste em **obter os dados a partir de alguma fonte**.

### Fontes citadas em aula

- CSV
- JSON
- TXT
- PDF
- tabelas
- sites
- bases de dados

Também foi comentado que os dados podem ser extraídos por meio de **scraping**, inclusive em páginas web e arquivos PDF.

### Exemplo citado

- extrair uma planilha com registros de vendas de uma loja.

> **Importante:** nesta etapa, **não se espera que os dados já estejam limpos ou organizados**. O objetivo inicial é apenas obter os dados.

---

## 2. Transformação (Transform)

Na etapa de transformação, os dados extraídos passam por **limpeza, tratamento, padronização e reorganização**, para que possam ser analisados corretamente.

### Operações citadas

#### Remoção de duplicatas

Registros duplicados podem distorcer métricas e produzir análises incorretas.

#### Tratamento de valores ausentes

Valores faltantes podem ser:

- removidos;
- substituídos por média;
- substituídos por mediana;
- tratados de outra forma adequada ao contexto.

#### Padronização de formatos

Exemplos:

- notas como `7,5` e `7.5`;
- diferenças de escrita;
- valores monetários escritos de formas diferentes.

#### Conversão de tipos

É importante garantir que cada coluna tenha o tipo adequado.

Exemplos:

- nome → `string`
- idade → `int`
- altura → `float`
- datas → `datetime`

#### Padronização de datas

Foi mencionado que datas podem aparecer em formatos diferentes, como:

- `ano-mês-dia`
- `dia/mês/ano`

Isso dificulta a análise quando os formatos não estão unificados.

#### Filtragem de registros

Podem ser removidos:

- registros irrelevantes;
- dados fora do escopo da análise;
- erros humanos;
- outliers.

#### Junção de tabelas

Foi citado o uso de **merge** para combinar diferentes conjuntos de dados e obter uma tabela mais completa.

Exemplo discutido em aula:

- juntar dados de alunos, notas, frequência e outras informações acadêmicas.

#### Criação de indicadores

Exemplo citado:

- índice de reprovação por falta.

### Outliers

Foi explicado que **outliers** são valores muito fora do padrão esperado.

#### Exemplo

- uma idade `500` em uma base em que os demais valores estão entre `1` e `80`.

Esses valores podem distorcer a média, o desvio padrão e outras estatísticas.

### Como identificar outliers

Foi mencionado que isso pode ser feito por:

- inspeção visual;
- amostragem;
- **boxplot**.

---

## 3. Carga / Exportação (Load)

Depois de extrair e transformar os dados, a última etapa é salvar ou armazenar os dados tratados.

### Formatos citados

- CSV
- Excel
- TXT
- base de dados

### Exemplo

- salvar os dados tratados em CSV para uso posterior em relatórios ou análises estatísticas.

---

## Por que o ETL é importante?

Sem ETL, a análise pode ser feita sobre dados:

- inconsistentes;
- incompletos;
- desorganizados.

O ETL ajuda a garantir:

- qualidade dos dados;
- confiabilidade;
- padronização;
- melhor base para decisões futuras.

---

# Introdução ao Python para análise de dados

Após a parte conceitual sobre ETL, a aula passou para uma introdução rápida ao **Python**, com foco no que seria necessário para começar a trabalhar com análise de dados.

## Por que usar Python?

Segundo a professora, Python foi escolhido por ser:

- fácil;
- intuitivo;
- de alto nível;
- amplamente utilizado;
- rico em bibliotecas prontas para análise de dados.

Também foi observado que, embora não seja tão eficiente quanto C em desempenho computacional, ele facilita bastante a implementação.

---

## 1. Variáveis no Python

Diferentemente de linguagens como C, em Python não é necessário declarar explicitamente o tipo da variável antes de atribuir um valor.

### Exemplo

```python
nome = "Carol"
idade = 28
altura = 1.65
aprovado = True
```

### Observação feita em aula

Isso pode ser bom pela praticidade, mas também pode gerar inconsistências, porque o Python aceita facilmente mudanças inesperadas de tipo.

Exemplo conceitual:

```python
nome = 2
```

Nesse caso, o nome da variável sugere uma string, mas o valor atribuído é numérico.

---

## 2. Listas

As listas foram apresentadas como estruturas fundamentais para armazenar vários valores.

### Exemplo

```python
nomes = ["Heron", "Félix", "Gabriel", "Ayalla", "Alícia", "Felipe", "Eduarda", "Isis"]
```

Elas podem representar, por exemplo:

- uma coluna de uma base de dados;
- uma linha de uma base;
- um conjunto qualquer de informações.

### Exemplo de linha representada como lista

```python
linha = ["Carol", 28, "Matemática Industrial", "Mestrado em Ciência da Computação"]
```

### Funções úteis com listas

```python
notas = [8.5, 7.0, 9.0, 6.5]
```

**Maior valor:**

```python
max(notas)
```

**Menor valor:**

```python
min(notas)
```

**Quantidade de elementos:**

```python
len(notas)
```

**Média:**

```python
sum(notas) / len(notas)
```

---

## 3. Dicionários

Outra estrutura importante apresentada foi o **dicionário**, que trabalha com pares de **chave** e **valor**.

### Exemplo

```python
aluno = {
    "nome": "Daniel Rezende",
    "idade": 28,
    "curso": "Matemática"
}
```

### Acesso por chave

```python
aluno["nome"]
aluno["curso"]
```

Foi comentado que essa estrutura se parece bastante com **JSON**, sendo muitas vezes mais intuitiva para representar dados.

### Dicionário com listas

```python
alunos = {
    "nome": ["Heron", "Gabriel", "Ayalla"],
    "curso": ["Matemática", "TI", "Telecomunicações"]
}
```

Essa estrutura é muito útil para a criação de DataFrames no Pandas.

---

## 4. Leitura de arquivos

Foi apresentada a leitura de arquivos usando Python puro.

### Exemplo

```python
with open("dados_venda.txt", "r") as arquivo:
    conteudo = arquivo.read()
```

Também foi comentado que é possível fazer leitura linha por linha, usando estruturas como `for`.

### Funções úteis para strings

#### `strip()`

Remove espaços em branco no início e no fim da string.

#### `split()`

Divide a string a partir de um separador.

### Exemplo

```python
linha = "Heron,Matemática"
linha.split(",")
```

Saída esperada:

```python
["Heron", "Matemática"]
```

Se os dados forem separados por ponto e vírgula:

```python
linha.split(";")
```

> **Observação:** foi comentado que o uso de `;` pode ser melhor quando o conteúdo textual já possui vírgulas.

---

# Introdução ao Pandas

Na segunda parte da aula, foi iniciada a introdução ao **Pandas**, biblioteca muito utilizada na análise de dados em Python.

## Por que usar Pandas?

Porque o Pandas:

- facilita a leitura de dados tabulares;
- possui muitas funções prontas;
- organiza os dados em estruturas práticas;
- permite inspeção rápida;
- é amplamente utilizado em projetos de análise de dados.

> **Observação:** também foi citado que o Pandas consome bastante memória e que, para bases muito grandes, existem bibliotecas mais otimizadas. Ainda assim, ele é excelente para aprendizado e uso inicial.

---

## 1. O que é um DataFrame?

O **DataFrame** foi definido como uma estrutura de dados em formato de tabela, contendo:

- linhas;
- colunas;
- cabeçalhos;
- valores organizados.

Essa estrutura facilita a leitura, a filtragem, a inspeção e a transformação dos dados.

---

## 2. Leitura de arquivos com Pandas

### Importação

```python
import pandas as pd
```

O `pd` é apenas um apelido para a biblioteca.

### Leitura de CSV

```python
df = pd.read_csv("arquivo.csv")
```

### Outros formatos mencionados

- `pd.read_json()`
- `pd.read_parquet()`
- `pd.read_sql()`
- `pd.read_excel()`

### Parâmetros do `read_csv`

Foi comentado que a função pode receber argumentos como:

- separador;
- codificação;
- cabeçalho;
- nomes de colunas;
- índice.

### Exemplo

```python
df = pd.read_csv("arquivo.csv", sep=";", encoding="utf-8")
```

> **Observação:** a professora reforçou a importância de consultar a documentação oficial das funções.

---

## 3. Primeira inspeção de um DataFrame

Antes de transformar os dados, é importante entender a estrutura da base.

### Funções citadas

#### `df.info()`

Mostra:

- número de linhas;
- colunas;
- valores não nulos;
- tipos de dados.

#### `df.describe()`

Mostra estatísticas das colunas numéricas, como:

- contagem;
- média;
- desvio padrão;
- mínimo;
- percentis;
- máximo.

#### `df.head()`

Mostra as primeiras linhas.

#### `df.tail()`

Mostra as últimas linhas.

#### `df.sample()`

Mostra linhas aleatórias.

#### `df.shape`

Mostra a quantidade de:

- linhas;
- colunas.

#### `df.columns`

Mostra os nomes das colunas.

### Atenção ao tipo `object`

Foi destacado que, quando uma coluna aparece como `object`, isso pode indicar inconsistência nos dados.

#### Exemplo citado

Uma mesma coluna contendo:

- `26` → inteiro
- `"26"` → string
- `26.2` → float

Nessa situação, o Pandas pode classificar a coluna como `object`.

---

## 4. Seleção e filtragem de dados

### Seleção de colunas

**Uma coluna:**

```python
df["nome"]
```

**Múltiplas colunas:**

```python
df[["nome", "nota"]]
```

### Seleção de linhas

**Por posição com `iloc`:**

```python
df.iloc[1]
df.iloc[3]
```

**Por índice com `loc`:**

```python
df.loc["Gabriel"]
```

### Fatiamento com `iloc`

```python
df.iloc[0:2]
```

Retorna da posição `0` até a posição anterior à `2`.

### Seleção de linhas e colunas ao mesmo tempo

```python
df.loc[linhas, colunas]
```

ou

```python
df.iloc[linhas, colunas]
```

### Filtragem com condições

**Notas maiores que 9:**

```python
df[df["nota"] > 9]
```

**Idade maior ou igual a 21 e nota menor que 8:**

```python
df[(df["idade"] >= 21) & (df["nota"] < 8)]
```

### Operadores lógicos mencionados

- `&` → and
- `|` → or

> **Importante:** quando há mais de uma condição, cada comparação deve ser colocada entre parênteses.

---

# Exemplo prático com a base `adult.csv`

Na parte final da aula, foi feito um exemplo prático com o dataset `adult.csv`.

## Etapas observadas

### 1. Leitura da base

A base foi carregada e inspecionada com funções como:

- `sample()`
- `info()`
- `describe()`
- `columns`

### 2. Identificação de dados faltantes

Foi observado que alguns valores ausentes estavam representados por `"?"`.

Isso significa que o `df.info()` não trataria automaticamente esses valores como nulos.

### 3. Seleção de colunas relevantes

Foram destacadas colunas como:

- `age`
- `workclass`
- `education`
- `marital.status`
- `relationship`
- `race`
- `gender`
- `hours.per.week`
- `native.country`
- `income`

Algumas outras colunas foram descartadas por serem menos úteis naquele contexto inicial de análise.

### 4. Substituição de `"?"` por valor nulo

Exemplo conceitual:

```python
df_filtrado = df_filtrado.replace("?", pd.NA)
```

Depois disso, a inspeção dos dados passou a mostrar corretamente a presença de valores ausentes.

### 5. Filtro: jovens com alta renda

Foi criado um filtro para selecionar pessoas com:

- idade menor ou igual a 30;
- renda maior que 50 mil por ano.

Exemplo conceitual:

```python
df_jovens_alta_renda = df_filtrado[
    (df_filtrado["age"] <= 30) &
    (df_filtrado["income"] == ">50K")
]
```

Foi mencionado que, entre cerca de 48 mil registros, apenas **993** se encaixavam nesse filtro.

### 6. Possíveis análises a partir do filtro

A professora comentou que, a partir desse subconjunto, seria possível investigar:

- escolaridade;
- gênero;
- ocupação;
- situação conjugal;
- carga horária de trabalho;
- desigualdades sociais.

### 7. Gráficos mencionados

Foram citadas visualizações como:

- distribuição por escolaridade;
- distribuição por gênero.

#### Insight citado

Entre os jovens com alta renda, havia predominância de pessoas com **bacharelado**.

Também foi observada uma diferença significativa por gênero, com maior presença de homens nesse grupo, o que pode abrir discussões sobre:

- viés da base;
- desigualdades estruturais;
- distribuição social da renda.

---

# Relação com machine learning

Foi comentado que a coluna `income` pode ser utilizada como **label** em tarefas de machine learning.

Nesse caso, seria possível treinar um modelo para prever se uma pessoa pertence ao grupo de:

- renda alta;
- renda baixa.

A partir desse tipo de problema, também se pode discutir:

- viés algorítmico;
- discriminação nos dados;
- impacto social das previsões.

---

# Principais ideias da aula

## Em resumo

A aula apresentou:

- o conceito de análise de dados;
- a importância do ETL;
- as etapas de extração, transformação e carga;
- fundamentos iniciais de Python;
- listas e dicionários;
- leitura de arquivos;
- introdução ao Pandas;
- DataFrames;
- inspeção inicial de dados;
- seleção e filtragem;
- tratamento de valores ausentes;
- interpretação inicial de uma base real.

---

# Conclusão

A principal mensagem da aula foi que **dados brutos, sozinhos, não bastam**.

É necessário extrair, limpar, organizar e interpretar os dados para que eles realmente possam gerar conhecimento e orientar decisões.

Além disso, a aula mostrou que **Python** e **Pandas** oferecem recursos muito práticos para esse processo, facilitando tanto a exploração inicial quanto a transformação e análise de dados.

---
