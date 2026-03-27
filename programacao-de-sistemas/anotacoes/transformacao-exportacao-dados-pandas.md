# Aula 27/03 — Transformação e Exportação de Dados com Pandas

> Anotações organizadas em **Markdown** a partir da transcrição completa da aula de 27/03 e dos slides de apoio.

---

## Sumário

- [Visão geral da aula](#visão-geral-da-aula)
- [Agenda da aula](#agenda-da-aula)
- [Parte 1 — Limpeza de dados](#parte-1--limpeza-de-dados)
  - [Inspeção inicial](#1-inspeção-inicial-dos-dados)
  - [Tratamento de valores nulos](#2-tratamento-de-valores-nulos)
  - [Substituições com `replace()`](#3-substituições-com-replace)
  - [Padronização de strings](#4-padronização-de-strings)
  - [Conversão de tipos](#5-conversão-de-tipos)
  - [Exemplo discutido em sala](#6-exemplo-discutido-em-sala)
- [Parte 2 — Transformações com Pandas](#parte-2--transformações-com-pandas)
  - [`apply`](#1-apply)
  - [`lambda`](#2-lambda)
  - [`groupby`](#3-groupby)
  - [`merge`](#4-merge)
  - [`concat`](#5-concat)
  - [`pivot_table`](#6-pivot_table)
- [Parte 3 — Exportação de dados](#parte-3--exportação-de-dados)
  - [CSV](#1-exportação-para-csv)
  - [JSON](#2-exportação-para-json)
  - [Outros formatos](#3-outros-formatos)
- [Parte 4 — Automação com scripts Python](#parte-4--automação-com-scripts-python)
- [Pipeline ETL com funções](#pipeline-etl-com-funções)
- [Estudo de caso apresentado](#estudo-de-caso-apresentado)
- [Exercícios citados na aula](#exercícios-citados-na-aula)
- [Principais ideias da aula](#principais-ideias-da-aula)
- [Conclusão](#conclusão)

---

## Visão geral da aula

Nesta aula, a professora retomou rapidamente o conceito de **ETL**, mas agora com foco maior em **Pandas** e na parte prática. Enquanto a aula anterior havia trabalhado mais os fundamentos, esta se concentrou principalmente em:

- **transformação de dados**;
- **limpeza e padronização**;
- **uso de funções importantes do Pandas**;
- **exportação para CSV e JSON**;
- **organização de um pipeline de ETL em funções Python**.

A ideia foi mostrar que, depois da etapa de extração, o trabalho mais importante costuma estar na **transformação**, isto é, na limpeza, tratamento, reorganização e preparação da base para análise ou geração de relatórios.

---

## Agenda da aula

Segundo os slides, a agenda foi a seguinte:

### Parte 1
- Limpeza de dados:
  - tratamento de valores nulos;
  - substituições;
  - formatação.
- Transformações com:
  - `apply`
  - `groupby`
  - `merge`
  - `pivot`

### Parte 2
- Exportação de dados:
  - salvando em CSV;
  - salvando em JSON.
- Automação básica de rotinas com scripts Python.
- Estudo de caso:
  - pipeline de ETL com Pandas.

---

# Parte 1 — Limpeza de dados

A professora destacou que a parte de transformação é a parte mais “chatinha” do processo, justamente porque exige correções manuais, inspeção constante e adaptação conforme os erros aparecem.

Ela reforçou que limpar dados não é apenas “embelezar” a tabela, mas sim **preparar a base para ser usada de forma confiável em análise, automação ou relatórios**.

---

## 1. Inspeção inicial dos dados

Antes de qualquer limpeza, a primeira etapa é sempre **olhar os dados**.

O objetivo dessa inspeção inicial é verificar:

- se há muita informação faltante;
- se existe despadronização;
- se algumas colunas estão com tipos inadequados;
- se há textos inconsistentes;
- se existem datas em formatos diferentes;
- se há registros obviamente problemáticos.

A professora resumiu essa etapa como:

> “dar uma olhadinha nos dados, ver a carinha deles”.

Isso é importante porque evita sair aplicando funções sem entender o problema real da base.

---

## 2. Tratamento de valores nulos

Um dos primeiros temas abordados foi o tratamento de **valores nulos**.

### Estratégias citadas

Foram apresentadas duas estratégias principais:

1. **Preencher o valor nulo** com algum valor estimado;
2. **Remover a linha ou a coluna** que contém o valor nulo.

### Quando preencher?

Faz sentido preencher quando:

- a linha possui outras informações relevantes;
- perder aquela linha inteira causaria perda importante de informação;
- o valor pode ser estimado de forma razoável.

Exemplo discutido em sala:

- uma idade ausente poderia ser preenchida com a **média** ou a **mediana** das demais idades.

A justificativa foi que, assim, o valor estimado não se afasta tanto da estatística da tabela.

### Quando remover?

Faz sentido remover quando:

- o registro tem informação demais faltando;
- a linha praticamente não agrega nada;
- não há como estimar o valor com confiança;
- o dado ausente compromete a utilidade do registro.

### Funções citadas

#### `fillna()`

Preenche valores nulos com algum valor fornecido.

Exemplo conceitual:

```python
df["idade"] = df["idade"].fillna(df["idade"].mean())
```

#### `dropna()`

Remove linhas ou colunas com valores nulos.

A professora comentou que é possível escolher o eixo:

- `axis=0` → linhas
- `axis=1` → colunas

Exemplo conceitual:

```python
df = df.dropna(axis=0)
```

### Observação importante

Foi discutido em sala que preencher valores nulos **não altera necessariamente a média**, mas pode alterar outras medidas estatísticas, como a variação e a distribuição dos dados.

Também foi lembrado que “chutar” qualquer valor aleatório é perigoso, porque isso pode introduzir valores muito distantes dos demais registros.

---

## 3. Substituições com `replace()`

Outro recurso importante da limpeza é a substituição de valores usando `replace()`.

### Exemplo citado

Na aula anterior, havia sido substituído:

- `"?"` → valor nulo.

A professora relembrou que o Pandas trabalha com representações como:

- `NA` → *Not Available*;
- `NaN` → *Not a Number*;
- `NaT` → *Not a Time*.

Exemplo conceitual:

```python
df["estado"] = df["estado"].replace("?", pd.NA)
```

### Replace com dicionário

A professora mostrou que o `replace()` também pode ser usado para mapeamento de valores, o que torna a leitura mais intuitiva.

Exemplo:

```python
df["estado"] = df["estado"].replace({
    "CE": "Ceará",
    "PI": "Piauí",
    "RJ": "Rio de Janeiro",
    "PB": "Paraíba"
})
```

Ela comentou que prefere essa abordagem porque o mapeamento de **chave → valor** fica mais claro.

### Ideia central

Nem sempre vale a pena decorar todos os argumentos das funções do Pandas. Quando houver uma forma mais intuitiva, como o uso de dicionário, ela tende a ser preferível.

---

## 4. Padronização de strings

A aula mostrou que bases com texto costumam ter muitos problemas de padronização, por exemplo:

- espaços no início ou no final;
- letras maiúsculas e minúsculas misturadas;
- nomes escritos de formas inconsistentes.

### Funções citadas

#### `strip()`

Remove espaços em branco do início e do fim da string.

#### `title()`

Coloca a primeira letra em maiúscula e o restante em minúsculo.

#### `upper()`

Converte tudo para maiúsculo.

#### `lower()`

Converte tudo para minúsculo.

### Exemplo conceitual

```python
df["Cliente"] = df["Cliente"].str.strip().str.title()
```

A professora chamou atenção para um detalhe importante:

- em colunas do Pandas, para usar funções de string, é necessário acessar o contexto de string com `.str`.

Por isso, em muitos casos, não basta usar:

```python
df["Cliente"].strip()
```

É preciso usar:

```python
df["Cliente"].str.strip()
```

E, se depois quiser chamar outra função de string, muitas vezes será necessário usar `.str` novamente:

```python
df["Cliente"] = df["Cliente"].str.strip().str.title()
```

### Observação discutida em sala

Houve uma conversa sobre o fato de `str.strip().title()` nem sempre funcionar como esperado diretamente em uma `Series`, porque o `.str` cria um “contexto” para tratar cada valor como string. Após aplicar uma função, esse contexto pode precisar ser chamado novamente.

A professora comentou que esse tipo de detalhe é algo que se aprende muito na prática e consultando:

- documentação;
- Stack Overflow;
- mensagens de erro.

---

## 5. Conversão de tipos

Outro ponto central da limpeza foi a conversão de colunas para os tipos corretos.

### 5.1 Números

Foi comentado que algumas colunas podem vir como string mesmo contendo números.

Exemplo clássico:

- valor monetário escrito com vírgula decimal.

Nesse caso, primeiro se substitui a vírgula por ponto e depois a coluna é convertida para tipo numérico.

Exemplo conceitual:

```python
df["valor_compra"] = df["valor_compra"].astype(str).str.replace(",", ".", regex=False)
df["valor_compra"] = pd.to_numeric(df["valor_compra"], errors="coerce")
```

### Discussão importante sobre `.str`

A professora explicou que, quando queremos substituir apenas uma parte do valor — por exemplo, a vírgula dentro de `"120,5"` —, não estamos substituindo o valor inteiro da `Series`, mas sim um pedaço da string de cada célula.

Por isso, muitas vezes não basta usar o `replace` do Pandas diretamente sobre a `Series`; é preciso trabalhar no contexto de string com `.str`.

### 5.2 Datas

As datas foram apresentadas como uma das partes mais problemáticas da limpeza.

O problema surge quando a coluna contém formatos heterogêneos, por exemplo:

- `01/02/2026`
- `2026-02-01`
- `1-2-26`

Nessas situações, o Pandas pode interpretar parte dos valores e falhar em outros.

A estratégia mostrada foi usar:

```python
df["data"] = pd.to_datetime(df["data"], errors="coerce")
```

O argumento `errors="coerce"` significa:

- se o Pandas não conseguir interpretar um valor como data, ele o transforma em `NaT`.

### Discussão em sala sobre datas

Foi comentado que:

- nem sempre é possível padronizar datas com total confiança;
- dias e meses entre `1` e `12` geram ambiguidades;
- anos podem aparecer com 2 ou 4 dígitos;
- mesmo uma API externa não garantiria total confiabilidade em todos os casos.

Nessas situações, as alternativas discutidas foram:

- generalizar a data;
- usar apenas o ano;
- usar mês e ano;
- descartar a informação;
- marcar como nulo.

A professora observou que, em alguns contextos, talvez o máximo confiável seja apenas o **ano**.

---

## 6. Exemplo discutido em sala

Foi apresentada uma tabela propositalmente despadronizada, e a turma precisou identificar o que a impedia de ser usada em análise ou exportação.

### Problemas apontados

- nomes com espaços extras;
- textos sem padronização;
- datas heterogêneas;
- valores nulos;
- cidade sem padronização;
- idade em formato inadequado;
- valor monetário com vírgula;
- linha com informação quase toda vazia.

### Soluções discutidas

#### Nomes
Padronizar com:

- `strip()`
- `title()`

#### Idade
Como estava em `float`, não era um problema tão grave, mas havia valor nulo.

Solução discutida:

- preencher com a média.

#### Cidade
Aplicar tratamento semelhante ao dos nomes.

#### Linha quase vazia
Remover, pois possuía informação insuficiente e agregava pouco à análise.

#### Valor de compra
Converter de string para número, substituindo vírgula por ponto.

#### Data
Converter para `datetime`, com `errors="coerce"`.

### Observação sobre idade

A professora comentou algo importante do ponto de vista de modelagem de dados:

> em muitos casos, nem é ideal armazenar idade; o ideal seria armazenar a data de nascimento.

A justificativa é que idade fica desatualizada com o tempo, enquanto a data de nascimento permanece correta e permite cálculo automático da idade.

---

# Parte 2 — Transformações com Pandas

Depois da limpeza, a aula passou para transformações mais voltadas à análise.

A professora resumiu bem a ideia dizendo que **transformar os dados é preparar a base para responder perguntas de negócio**.

---

## 1. `apply`

O `apply` foi apresentado como uma forma de aplicar regras customizadas em uma coluna ou estrutura do DataFrame.

### Ideia principal

Quando se usa `apply` em uma `Series`, o Pandas analisa **valor por valor** daquela coluna.

### Exemplo discutido

Se existe uma função `dobro`, que multiplica o valor por 2, então:

```python
df["coluna"].apply(dobro)
```

retorna uma nova `Series` com todos os valores transformados.

### Uso prático

O `apply` serve para:

- classificar valores;
- enriquecer colunas;
- aplicar regras personalizadas;
- construir novas variáveis.

---

## 2. `lambda`

A professora explicou que o `lambda` é uma função anônima de uma linha.

Exemplo citado:

```python
lambda x: x * 2
```

Esse tipo de construção é útil quando:

- a transformação é simples;
- não compensa criar uma função `def` separada;
- queremos deixar o fluxo mais rápido.

---

## 3. `groupby`

O `groupby` foi apresentado como uma das funções mais importantes do Pandas para análise de dados.

### Definição

Ele permite agrupar registros com base em uma ou mais colunas e, em seguida, aplicar uma função de agregação.

### Funções de agregação mencionadas

- média;
- soma;
- máximo;
- mínimo.

### Exemplo discutido

Se a base tem:

- vendedor;
- região;
- vendas.

E queremos saber o total de vendas por região, podemos agrupar pela coluna `Região` e somar a coluna `Vendas`.

Exemplo conceitual:

```python
df.groupby("Região")["Vendas"].sum()
```

A professora também citou a ideia de **Split-Apply-Combine**:

1. separar os dados em grupos;
2. aplicar uma função em cada grupo;
3. combinar os resultados.

---

## 4. `merge`

O `merge` foi comparado aos **joins do SQL**.

Ele é usado quando duas tabelas compartilham uma chave em comum, por exemplo:

- `id_cliente`
- `id_produto`

### Situação típica

Juntar:

- tabela de vendas
- tabela de clientes

ou

- tabela de vendas
- tabela de produtos

### Tipos mencionados

- `inner join`
- `left join`
- `right join`

### Observação importante

Se a chave não for única, pode haver **duplicação de linhas**. Isso foi explicitamente destacado nos slides como um risco importante.

---

## 5. `concat`

O `concat` foi apresentado como uma operação mais simples do que o `merge`.

Enquanto o `merge` junta tabelas por uma chave, o `concat` simplesmente:

- empilha uma tabela embaixo da outra; ou
- coloca uma tabela ao lado da outra.

### Exemplo citado

Unir:

- vendas de janeiro
- vendas de fevereiro

desde que tenham a mesma estrutura.

---

## 6. `pivot_table`

A `pivot_table` foi explicada como uma forma de reorganizar os dados para facilitar leitura gerencial e visualização rápida.

### Exemplo dado em aula

A partir de uma tabela com colunas como:

- Nome
- Idade
- Estado
- Curso

poderíamos reorganizar os dados para enxergar:

- quantidade de alunos por faixa etária e por estado.

Nesse caso:

- as linhas teriam os estados;
- as colunas teriam as idades ou faixas;
- o conteúdo poderia ser uma contagem de registros.

A professora comparou esse resultado a uma espécie de histograma em formato de tabela.

### Ideia central

A tabela base contém muita informação “crua”, mas a pivot permite reorganizar o que realmente interessa para o objetivo da análise.

---

# Parte 3 — Exportação de dados

A segunda metade da aula tratou de exportação de dados com Pandas.

A professora enfatizou que, em comparação com a transformação, essa etapa costuma ser bem mais simples.

Ela resumiu a lógica assim:

- para ler → `read_...`
- para exportar → `to_...`

---

## 1. Exportação para CSV

Para exportar uma base em CSV, usa-se:

```python
df.to_csv("arquivo.csv")
```

### Parâmetros comentados

#### `index=False`

Evita criar a coluna de índice no arquivo salvo.

Isso é importante para não gerar aquela coluna extra famosa:

- `Unnamed: 0`

Exemplo:

```python
df.to_csv("vendas_tratadas.csv", index=False, encoding="utf-8")
```

#### `encoding="utf-8"`

Foi recomendado para preservar corretamente caracteres especiais do português.

### Vantagens do CSV

Segundo a aula:

- ótimo para tabelas planas;
- fácil de abrir em planilhas;
- fácil de compartilhar entre equipes;
- simples de interpretar.

---

## 2. Exportação para JSON

A exportação para JSON segue a mesma lógica:

```python
df.to_json("arquivo.json")
```

### Parâmetro importante: `orient`

No JSON, um ponto importante é decidir como os dados serão organizados.

A professora explicou que o JSON pode representar:

- linhas;
- colunas.

Exemplo comum:

```python
df.to_json("vendas_tratadas.json", orient="records", indent=4)
```

### Sobre `indent`

Foi comentado que a identação serve mais para visualização humana do arquivo JSON.

Exemplo:

- `indent=4`

### Quando usar JSON

Segundo os slides:

- útil para APIs;
- útil para integrações web;
- boa opção para automação;
- estrutura flexível.

---

## 3. Outros formatos

A professora também comentou que, para bases maiores, CSV talvez não seja o formato ideal.

Foram mencionados:

- **Parquet**
- **Pickle**

A observação foi que esses formatos podem ser mais compactos e adequados para grandes volumes de dados.

Exemplo de leitura:

```python
pd.read_parquet("arquivo.parquet")
```

---

# Parte 4 — Automação com scripts Python

Depois da exportação, a professora explicou como organizar tudo isso em um pequeno script Python.

A ideia central foi:

- separar responsabilidades;
- colocar cada etapa em uma função;
- usar uma função principal `main()` para coordenar tudo.

### Benefícios disso

- melhora a organização;
- facilita manutenção;
- elimina trabalho manual repetitivo;
- permite reaproveitar o pipeline com novos dados.

### Comentário importante

Ela destacou que, se dados chegam de forma recorrente, esse tipo de organização já é um primeiro passo para automação.

---

# Pipeline ETL com funções

A professora sugeriu que o ideal é separar o código em funções como:

- carregar dados;
- transformar dados;
- salvar dados.

### Exemplo estrutural

```python
def carregar_dados(caminho):
    return pd.read_csv(caminho)

def transformar_dados(df):
    # limpeza, tratamento, novas colunas, padronização
    return df

def salvar_dados(df):
    df.to_csv("vendas_tratadas.csv", index=False, encoding="utf-8")
    df.to_json("vendas_tratadas.json", orient="records", indent=4)

def main():
    df = carregar_dados("vendas.csv")
    df = transformar_dados(df)
    salvar_dados(df)

if __name__ == "__main__":
    main()
```

### Ideia reforçada em aula

Antes de escrever tudo “na bagunça” no notebook, o ideal é pensar no **esqueleto do código**:

1. função para carregar;
2. função para transformar;
3. função para salvar;
4. função principal para integrar tudo.

---

# Estudo de caso apresentado

Nos slides, foi apresentado um estudo de caso em que uma empresa possui três arquivos:

- `clientes.csv`
- `produtos.csv`
- `vendas.csv`

## Objetivo

Gerar:

- uma base tratada para análise;
- um relatório consolidado por categoria e estado;
- arquivos finais em CSV e JSON.

A professora comentou que o “produto” final seria justamente a base já tratada, organizada e pronta para análise ou relatório.

---

# Exercícios citados na aula

## Exercício de fixação — limpeza

A turma deveria pensar:

- quais colunas têm maior risco de erro;
- onde faz sentido preencher nulos;
- onde faz sentido remover registros.

## Exercício de fixação — transformações

Foi proposto:

1. criar uma coluna `valor_total = valor * quantidade`;
2. agrupar por estado;
3. montar uma `pivot_table` com soma de `valor_total` por estado e categoria.

---

# Principais ideias da aula

## 1. Transformação é a parte mais trabalhosa do ETL
Ler e salvar costuma ser simples. O maior esforço geralmente está em:

- limpar;
- corrigir;
- padronizar;
- reorganizar.

## 2. Nem tudo pode ser resolvido automaticamente
Especialmente no caso de datas heterogêneas, pode não haver uma solução perfeita. Às vezes será preciso:

- generalizar;
- aceitar nulos;
- descartar parte da informação.

## 3. Pandas facilita muito o trabalho
Mesmo que algumas funções tenham muitos parâmetros, o Pandas fornece uma lógica muito consistente:

- `read_...` para ler;
- `to_...` para salvar;
- `groupby`, `merge`, `apply`, `pivot_table` para transformar.

## 4. Não é necessário decorar tudo
A professora reforçou várias vezes que:

- não é realista decorar todos os argumentos;
- documentação faz parte do processo;
- mensagens de erro e prática ajudam muito.

## 5. Organizar o código em funções é boa prática
Mesmo em scripts pequenos, separar etapas já melhora bastante a estrutura do trabalho.

---

# Conclusão

A aula de 27/03 aprofundou a parte prática do ETL com Pandas, concentrando-se naquilo que mais aparece em situações reais: **limpeza, transformação, exportação e automação básica**.

Os principais aprendizados foram:

- tratar valores nulos com critério;
- padronizar strings e formatos;
- converter corretamente números e datas;
- aplicar regras com `apply` e `lambda`;
- agrupar informações com `groupby`;
- juntar tabelas com `merge`;
- reorganizar resultados com `pivot_table`;
- exportar dados com `to_csv()` e `to_json()`;
- estruturar um pipeline simples com funções e `main()`.

No fim, a mensagem central da aula foi que **transformar dados é preparar a base para responder perguntas do negócio** — e que uma base bem tratada é o verdadeiro produto intermediário que torna análise, relatório e automação possíveis.

---
