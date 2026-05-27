# Aula de Aprendizado de Máquina

**Disciplina:** Residência em Segurança Cibernética — Módulo Aprendizado de Máquina  
**Professor:** César Lincoln Cavalcante Mattos  
**Data da aula organizada:** 25/05  

> Esta anotação foi organizada para preservar a lógica da explicação do professor. A ideia não é apenas listar fórmulas, mas reconstruir o caminho da aula: por que cada conceito apareceu, qual problema ele resolve e como ele se conecta com os próximos assuntos.

---

## 1. Visão geral da disciplina

O professor começou explicando que o módulo é curto, mas cobre uma quantidade grande de temas. 

A disciplina não é apenas uma disciplina de aplicação de ferramentas. O objetivo principal é entender **o que existe por trás das ferramentas** de aprendizado de máquina. Ou seja, não basta usar uma biblioteca pronta: é importante compreender os modelos, os parâmetros, as funções custo, as hipóteses probabilísticas e os critérios de avaliação.

O módulo cobre, ao longo da semana:

- conceitos básicos de IA e aprendizagem de máquina;
- revisão de probabilidade e estatística;
- regressão linear;
- regressão polinomial;
- regularização;
- regressão logística;
- classificadores estatísticos;
- KNN;
- árvores de decisão;
- avaliação de modelos;
- redes neurais;
- SVM;
- comitês/ensembles;
- agrupamento;
- PCA;
- projeto de sistemas de aprendizagem de máquina.

A avaliação será feita por **listas de implementação**, de forma individual. O professor reforçou que haverá matemática, mas o foco avaliativo é implementar e entender os métodos.

### Ferramentas sugeridas

A linguagem mais recomendada é **Python**, por ser a linguagem de fato mais usada em ciência de dados, aprendizagem de máquina e deep learning. O professor comentou que outras linguagens podem ser usadas, como R, Julia, C/C++ ou Octave, mas Python é a escolha mais natural por causa do ecossistema de pacotes como Jupyter, NumPy, SciPy, Matplotlib e scikit-learn.

---

## 2. IA, Aprendizagem de Máquina e produtos baseados em IA

O professor chamou atenção para a necessidade de usar melhor o jargão técnico. No uso cotidiano, muita gente chama tudo de “IA”, mas, em uma formação técnica, é importante diferenciar:

### Inteligência Artificial

A IA é a grande área que busca **automatizar tarefas intelectuais normalmente associadas à capacidade humana**, como tomar decisões, resolver problemas, planejar, aprender e otimizar.

Um ponto importante da explicação: **a máquina não precisa imitar a forma como o ser humano resolve o problema**. Ela precisa imitar a capacidade de resolver a tarefa. O exemplo implícito é semelhante ao avião: ele voa, mas não voa exatamente como um pássaro.

### Aprendizagem de Máquina

A aprendizagem de máquina é um subconjunto da IA. Ela busca algoritmos capazes de **aprender uma tarefa a partir de dados disponíveis**.

A diferença para um programa tradicional é central:

- em um algoritmo clássico, como ordenar números ou fazer uma consulta SQL, programamos explicitamente as regras;
- em aprendizagem de máquina, muitas vezes não sabemos escrever essas regras manualmente.

Exemplo usado pelo professor: reconhecer uma pessoa em uma imagem. Nós reconhecemos rostos com facilidade, mas é muito difícil dizer uma regra do tipo “se este pixel estiver assim, então é o Félix”. Por isso, em vez de programar todas as regras, fornecemos exemplos para que o algoritmo encontre padrões.

### Produtos baseados em IA

O professor também separou IA/AM de **produtos baseados em IA**.

ChatGPT, Gemini ou Claude não são “a IA” em si. Eles são produtos que usam técnicas de IA e aprendizagem de máquina. O modelo é apenas uma parte pequena do produto. Ao redor dele existem interface, processamento da entrada, processamento da saída, gerenciamento de recursos, filtros, infraestrutura e outros componentes.

A analogia usada foi: chamar um produto de IA de “a IA” é como chamar um carro de “a mecânica”. O carro existe porque usa princípios da mecânica, mas a mecânica é uma área de estudo, não o produto.

---

## 3. O problema central de aprendizagem de máquina

O problema central é aprender uma relação entre entrada e saída a partir de dados.

De maneira geral, queremos aprender uma função:

$$
f: x \mapsto y
$$

em que:

- $x$ é a entrada;
- $y$ é a saída desejada;
- o modelo tenta produzir uma predição $\hat{y}$ próxima de $y$.

A ideia mais importante é que o modelo não deve apenas decorar os dados vistos. Ele deve funcionar bem para dados novos. O professor enfatizou isso com a ideia do **paciente de amanhã**: não queremos apenas acertar os pacientes já vistos na tabela, queremos prever corretamente o comprimento do cateter para um paciente novo, que ainda não apareceu nos dados.

---

## 4. No Free Lunch

O teorema do **No Free Lunch** foi apresentado como justificativa para estudar várias técnicas diferentes.

A ideia é:

> Não existe um algoritmo de aprendizagem que seja superior a todos os outros em todas as distribuições de dados possíveis.

Se um método é muito bom em uma aplicação específica, ele pode não ser tão bom em outra. Por isso, é necessário ter um “arsenal” de técnicas e entender em que tipo de situação cada uma funciona melhor.

A consequência prática é que não existe “o melhor algoritmo universal”. Existe o método mais adequado para determinado problema, com determinadas restrições, dados, hipóteses e objetivos.

---

## 5. Categorias de aprendizagem de máquina

O professor usou a taxonomia clássica:

1. aprendizagem supervisionada;
2. aprendizagem não supervisionada;
3. aprendizagem por reforço.

### 5.1 Aprendizagem supervisionada

Ocorre quando temos pares observáveis de entrada e saída:

$$
(x_i, y_i)
$$

A palavra **observável** foi destacada. Não basta existir uma entrada e uma saída em sentido abstrato: para treinar supervisionadamente, precisamos ter acesso aos pares de dados alinhados.

Duas tarefas supervisionadas principais:

#### Regressão

A saída é uma quantidade numérica, geralmente contínua, mas pode ser discreta quando existe noção de grandeza.

Exemplos:

- preço;
- tempo;
- temperatura;
- umidade;
- comprimento de um cateter;
- número de alarmes em um sistema;
- número de chamadas em um call center.

Mesmo quando a saída é discreta, como número de chamadas, ainda pode ser regressão, pois existe uma noção de quantidade.

#### Classificação

A saída pertence a um conjunto finito de classes ou rótulos.

Exemplos:

- spam ou não spam;
- saudável ou doente;
- gato ou cachorro;
- dígitos de 0 a 9;
- espécies de flores;
- objetos em imagens.

Na classificação, em geral, não existe uma relação de grandeza natural entre as classes. Por exemplo, “gato” não é maior ou menor que “cachorro” no sentido do rótulo.

O professor também observou que algumas tarefas misturam regressão e classificação. Em detecção de objetos, por exemplo, o modelo precisa prever:

- a classe do objeto: pessoa, cadeira, bolo, taça etc.;
- a posição da caixa delimitadora, isto é, coordenadas numéricas.

Logo, há classificação e regressão no mesmo problema.

### 5.2 Aprendizagem não supervisionada

Nesse caso, não temos pares entrada-saída. O objetivo é descobrir estrutura nos dados: padrões, grupos, densidades, anomalias ou representações úteis.

Exemplos:

- agrupamento de perfis de usuários;
- sistemas de recomendação;
- visualização de dados de alta dimensão;
- compressão;
- remoção de ruído;
- detecção de anomalias;
- estimação de densidade;
- geração de dados.

O professor destacou os sistemas de recomendação como uma das aplicações mais rentáveis. Quando se fala “o algoritmo do Instagram”, “o algoritmo do TikTok” ou “o algoritmo da plataforma de streaming”, normalmente estamos falando de algoritmos de recomendação.

#### Modelos generativos

A geração de dados foi tratada como uma forma de aprendizagem não supervisionada ou, em modelos mais complexos, como uma abordagem híbrida/auto-supervisionada.

A ideia é aprender a estrutura geral de uma distribuição de dados e gerar novos exemplos plausíveis. Por exemplo, um modelo aprende características gerais de rostos humanos e depois gera rostos que nunca existiram.

Quando a geração é guiada por texto, como em modelos texto-imagem, a geração é **condicionada** por uma entrada textual.

### 5.3 Aprendizagem por reforço

A aprendizagem por reforço foi apresentada como uma categoria diferente das anteriores. Ela envolve um **agente** interagindo com um **ambiente**.

O agente realiza ações, observa consequências e recebe recompensas. O objetivo é aprender uma política de ações que maximize a recompensa ao longo do tempo.

Exemplos mencionados:

- robótica;
- braço robótico empilhando blocos;
- jogos como AlphaGo;
- agentes jogando Mario a partir dos pixels da tela;
- LLMs e agentes como sistemas híbridos.

O ponto mais difícil destacado pelo professor é que a recompensa costuma ser:

- **tardia**, porque aparece apenas no fim de uma tentativa;
- **fracamente informativa**, porque muitas vezes só diz “conseguiu” ou “não conseguiu”.

No exemplo do Mario, o modelo recebe apenas os pixels da tela e comandos possíveis. Ele não sabe inicialmente qual boneco controla, qual botão pula, qual é o objetivo ou que cair no buraco é ruim. Ele precisa descobrir isso pela recompensa.

---

## 6. Probabilidade, Estatística e Aprendizagem de Máquina

O professor argumentou de que aprendizagem de máquina e probabilidade são quase inseparáveis.

A probabilidade e a estatística são formas de lidar com incerteza. E aprendizagem de máquina também lida com incerteza: dados incompletos, ruído, fenômenos não observáveis e predições sobre casos futuros.

Uma distinção apresentada:

- **Probabilidade:** parte de modelos/regras para fazer predições sobre dados;
- **Estatística:** parte de dados para analisar, inferir ou ajustar modelos/regras.

Na prática, aprendizagem de máquina usa as duas perspectivas: dados, modelos, predição e inferência aparecem o tempo todo.

O professor também comentou que AM é interdisciplinar: vem da estatística, matemática aplicada, engenharia elétrica, processamento de sinais, ciência da computação, ciência de dados e até da física em alguns ramos.

---

## 7. Variáveis aleatórias

Variável aleatória foi apresentada como um conceito básico, mas com uma observação importante: apesar do nome, ela não é exatamente uma “variável” comum. Ela é uma função que mapeia resultados de um fenômeno incerto em valores numéricos.

Também foi feita uma distinção entre “aleatório” e “arbitrário”. Aleatório, no contexto probabilístico, significa incerto, mas não necessariamente sem estrutura.

Classificações importantes:

- **contínua:** assume valores em um domínio contínuo, como os reais;
- **discreta:** assume valores específicos;
- **univariada:** possui uma dimensão;
- **multivariada:** possui várias dimensões.

Exemplo:

- temperatura isolada: variável contínua univariada;
- temperatura + umidade: variável contínua multivariada.

---

## 8. Interpretação frequentista e bayesiana

### Interpretação frequentista

A probabilidade está associada à frequência relativa de ocorrência de um evento em muitas repetições.

Na visão frequentista:

- parâmetros são valores fixos, determinísticos;
- eles podem ser desconhecidos, mas não são tratados como variáveis aleatórias.

O professor resumiu de forma bem intuitiva: se você não sabe o valor do parâmetro, “a culpa é sua”; o parâmetro não está mudando por isso.

### Interpretação bayesiana

A probabilidade quantifica incerteza sobre um evento ou parâmetro.

Na visão bayesiana:

- parâmetros podem ser tratados como variáveis aleatórias;
- observar dados atualiza o grau de incerteza;
- a incerteza antes dos dados é representada por uma priori;
- a incerteza depois dos dados é representada pela posteriori.

O professor explicou que aprendizagem de máquina usa as duas visões conforme a conveniência, embora nesse módulo a abordagem frequentista apareça com mais frequência.

---

## 9. Regra de Bayes

A regra de Bayes foi apresentada como uma ferramenta para relacionar uma variável observada com uma variável não observada.

Forma geral:

$$
p(x|y) = \frac{p(y|x)p(x)}{p(y)}
$$

Componentes:

- $p(x|y)$: posteriori;
- $p(y|x)$: verossimilhança;
- $p(x)$: priori;
- $p(y)$: evidência.

A interpretação central da aula:

> A regra de Bayes permite calcular a incerteza sobre algo que eu não observei a partir de algo que eu observei.

### 9.1 Exemplo dos baús

Existem dois baús:

- baú A: 100 moedas de ouro;
- baú B: 50 moedas de ouro e 50 de prata.

Escolhe-se um baú aleatoriamente e retira-se uma moeda. Se a moeda é de ouro, qual a probabilidade de o baú escolhido ter sido o A?

Eventos:

- $A$: escolheu o baú A;
- $B$: escolheu o baú B;
- $G$: pegou uma moeda de ouro.

Aplicando Bayes:

$$
P(A|G) = \frac{P(G|A)P(A)}{P(G|A)P(A) + P(G|B)P(B)}
$$

$$
P(A|G) = \frac{1 \cdot 0{,}5}{1 \cdot 0{,}5 + 0{,}5 \cdot 0{,}5} = \frac{2}{3}
$$

Interpretação do professor: a cor da moeda foi observada, mas o baú escolhido não. Bayes liga essas duas informações.

### 9.2 Exemplo do teste de Covid

O exemplo foi usado para mostrar algo muito próximo do raciocínio de aprendizagem de máquina.

Dados:

- prevalência: 1 em cada 10 pessoas tem a doença, isto é, $P(X=1)=0{,}1$;
- sensibilidade: se a pessoa tem a doença, o teste dá positivo com probabilidade 87,5%;
- especificidade: se a pessoa não tem a doença, o teste dá negativo com probabilidade 97,5%.

Pergunta:

> Se o teste deu positivo, qual a probabilidade de a pessoa estar doente?

O professor destacou que muitos alunos chutariam 87,5%, mas isso é $P(Y=1|X=1)$, ou seja, a probabilidade de o teste dar positivo dado que a pessoa está doente. A pergunta quer o contrário: $P(X=1|Y=1)$.

A variável não observada é o estado de saúde. A variável observada é o resultado do teste. O professor usou a imagem de um “bit escondido” no peito da pessoa: o estado real de saúde está lá, mas não temos acesso direto. Temos exames, sintomas e medições, que são proxies.

Pela regra de Bayes:

$$
P(X=1|Y=1) = \frac{P(Y=1|X=1)P(X=1)}{P(Y=1)}
$$

O denominador inclui dois caminhos:

1. pessoa doente e teste positivo;
2. pessoa saudável e teste falso positivo.

$$
P(Y=1) = P(X=1)P(Y=1|X=1) + P(X=0)P(Y=1|X=0)
$$

$$
P(Y=1) = 0{,}1 \cdot 0{,}875 + 0{,}9 \cdot (1 - 0{,}975)
$$

$$
P(Y=1) = 0{,}11
$$

Logo:

$$
P(X=1|Y=1) = \frac{0{,}875 \cdot 0{,}1}{0{,}11} \approx 0{,}795
$$

A probabilidade final é aproximadamente **79,5%**.

O professor conectou isso com aprendizagem de máquina dizendo que as taxas usadas no cálculo vieram de dados. A prevalência, a sensibilidade e a especificidade não caíram do céu: foram estimadas por coleta de dados.

O “pulo do gato” para aprendizagem de máquina é que, em vez de taxas fixas iguais para todo mundo, podemos aprender taxas que dependem dos atributos de cada indivíduo. Ou seja, a probabilidade passa a variar conforme a entrada $x$.

---

## 10. Estatísticas de caracterização

O professor revisou estatísticas que resumem variáveis aleatórias.

### 10.1 Populacional × empírica/amostral

- **Populacional:** calculada com acesso a toda a população.
- **Empírica ou amostral:** calculada a partir de uma amostra dos dados.

A palavra **empírica** foi reforçada como “a partir dos dados”.

### 10.2 Média / esperança

A média está ligada ao operador esperança. O professor afirmou que, nesse contexto, média e esperança estão essencialmente conectadas: esperança é o operador usado para calcular a média esperada de uma variável aleatória.

Estimador comum:

$$
\hat{\mu} = \frac{1}{N}\sum_{i=1}^{N} x_i
$$

O chapeuzinho indica que é uma estimativa calculada a partir dos dados.

### 10.3 Mediana

É o valor central dos dados ordenados. Se houver número par de elementos, geralmente usa-se a média dos dois valores centrais.

### 10.4 Moda

É o valor mais frequente ou o ponto de maior densidade local.

O professor chamou atenção para distribuições multimodais. Em uma distribuição com dois picos, há duas modas locais. Isso pode indicar múltiplas formas plausíveis de explicar um fenômeno.

### 10.5 Variância

Mede dispersão em relação à média.

Forma populacional:

$$
\sigma^2 = E[(X - \mu)^2]
$$

Estimador amostral:

$$
\hat{\sigma}^2 = \frac{1}{N-1}\sum_{i=1}^{N}(x_i - \hat{\mu})^2
$$

O uso de $N-1$ aparece quando usamos a média amostral em vez da média populacional, como correção de viés.

### 10.6 Matriz de covariância

Para variáveis multivariadas, a variância vira matriz de covariância.

Ela informa:

- como cada variável varia em relação a si mesma;
- como duas variáveis variam juntas.

Exemplo dado: temperatura e pressão. A matriz permite representar tanto a variação da temperatura quanto a relação entre temperatura e pressão.

A matriz de covariância é simétrica, porque:

$$
\mathrm{cov}(x_1, x_2) = \mathrm{cov}(x_2, x_1)
$$

---

## 11. Distribuições importantes

### 11.1 Bernoulli

Modela variáveis binárias, como 0/1, sim/não, positivo/negativo.

$$
p(y|q)=q^y(1-q)^{1-y}
$$

Foi usada depois para justificar a regressão logística binária.

### 11.2 Categórica / Multinoulli

Modela uma variável discreta com mais de duas classes.

Exemplo: classe 1, 2 ou 3 em um problema multiclasse.

### 11.3 Gaussiana

A gaussiana apareceu como: uma distribuição que volta o tempo todo por conveniência matemática.

Ela é usada em:

- ruído gaussiano;
- erro quadrático médio;
- regressão linear probabilística;
- regularização L2 via priori gaussiana;
- análise do discriminante gaussiano;
- Naive Bayes Gaussiano;
- aproximações em modelos mais complexos.

A gaussiana é matematicamente conveniente, mesmo quando os dados reais não são perfeitamente gaussianos.

---

## 12. Regressão linear simples

O exemplo base foi o cateterismo cardíaco.

Problema:

> Dada a altura de um paciente, qual o comprimento do cateter necessário para alcançar o coração?

Temos uma tabela com pares:

$$
(x_i, y_i)
$$

em que:

- $x_i$: altura do paciente;
- $y_i$: comprimento do cateter.

O objetivo é encontrar uma relação entre altura e comprimento para predizer $\hat{y}_i$ próximo de $y_i$.

Modelo linear simples:

$$
\hat{y}_i = w_0 + w_1x_i
$$

- $w_0$: intercepto/bias;
- $w_1$: inclinação da reta.

---

## 13. Terminologia importante

- **Atributo / feature:** característica de um padrão.
- **Padrão / pattern:** vetor de atributos que representa um exemplo.
- **Modelo:** função que relaciona entrada e saída.
- **Função custo / função objetivo:** mede o quão mal ou bem o modelo aproxima os dados.
- **Parâmetros:** variáveis que caracterizam o modelo.
- **Risco empírico:** estimativa do custo obtida a partir dos dados disponíveis.
- **Otimização / treinamento / aprendizagem:** processo de encontrar parâmetros que minimizem o custo ou maximizem um objetivo.

---

## 14. Erro Quadrático Médio — MSE

A função custo escolhida foi o erro quadrático médio:

$$
J(w_0,w_1)=\frac{1}{2N}\sum_{i=1}^{N}e_i^2
$$

com:

$$
e_i=y_i-\hat{y}_i
$$

O erro é elevado ao quadrado para penalizar erros grandes e evitar cancelamento entre erros positivos e negativos.

Mas o professor foi além da intuição: o MSE tem uma interpretação probabilística.

### De onde vem o MSE?

O MSE equivale a assumir que o ruído de observação é gaussiano.

Modelo:

$$
y_i = w^Tx_i + \epsilon_i
$$

com:

$$
\epsilon_i \sim \mathcal{N}(0,\sigma^2)
$$

Ou seja, toda vez que usamos MSE, estamos, por baixo dos panos, assumindo que a diferença entre o valor real e a predição se comporta como ruído normal/gaussiano.

---

## 15. Gradiente Descendente — GD

Queremos minimizar:

$$
J(w_0,w_1)
$$

O gradiente indica a direção de maior crescimento da função custo. Para minimizar, andamos no sentido contrário ao gradiente.

Atualizações:

$$
w_0 \leftarrow w_0 + \alpha \frac{1}{N}\sum_{i=1}^{N}e_i
$$

$$
w_1 \leftarrow w_1 + \alpha \frac{1}{N}\sum_{i=1}^{N}e_ix_i
$$

O professor explicou isso visualmente como uma bacia/paraboloide. O objetivo é chegar ao fundo da bacia, onde o custo é menor.

O $\alpha$ é o **passo de aprendizado**.

- Se $\alpha$ é grande demais, o algoritmo oscila e pode não convergir.
- Se $\alpha$ é pequeno demais, o algoritmo converge muito devagar.

---

## 16. Gradiente Descendente Estocástico — SGD

O GD usa todos os dados para calcular cada atualização. O SGD usa um exemplo por vez, ou pequenos lotes.

Atualização típica:

$$
w \leftarrow w + \alpha e_ix_i
$$

A explicação do professor: o GD calcula um gradiente mais “correto”, usando todos os dados, então caminha de forma mais suave. O SGD usa uma estimativa ruidosa do gradiente, então seu caminho é mais errático, mas ele escala melhor para grandes quantidades de dados.

O professor comparou o SGD com o LMS da engenharia elétrica/filtragem de sinais. A estrutura é essencialmente a mesma, embora o contexto de aplicação possa ser diferente.

Um ponto destacado: muitos modelos paramétricos modernos são treinados, no fundo, com variações de SGD. Por isso, entender SGD é entender um dos algoritmos de treinamento mais usados do mundo.

### Época

Uma época ocorre quando o algoritmo passa por todos os dados de treinamento uma vez. No SGD, normalmente há várias épocas.

---

## 17. Regressão linear múltipla

Quando há mais de uma entrada, por exemplo altura e peso, o modelo passa a ter mais parâmetros:

$$
\hat{y}_i = w_0 + w_1x_{i1} + w_2x_{i2}
$$

Para evitar escrever uma equação enorme para muitos atributos, usamos notação vetorial.

Criamos uma entrada artificial fixa igual a 1 para representar o intercepto:

$$
x_i = [1, x_{i1}, x_{i2}, ..., x_{iD}]^T
$$

$$
w = [w_0, w_1, w_2, ..., w_D]^T
$$

Modelo:

$$
\hat{y}_i = w^Tx_i
$$

O professor reforçou que a implementação deve ser vetorial. Fazer parâmetro por parâmetro é lento, sequencial e pouco escalável.

---

## 18. Forma matricial e OLS

Agrupando todos os exemplos:

$$
\hat{y}=Xw
$$

A solução analítica por mínimos quadrados ordinários é:

$$
w=(X^TX)^{-1}X^Ty
$$

Essa solução é chamada de **OLS — Ordinary Least Squares**, ou mínimos quadrados ordinários.

A expressão:

$$
(X^TX)^{-1}X^T
$$

é associada à **pseudo-inversa**, também conhecida como inversa de Moore-Penrose.

### Por que não usar sempre OLS?

Apesar de elegante, OLS não escala tão bem.

Problemas:

- exige inversão de matriz;
- pode ter alto custo computacional;
- pode gerar problemas numéricos;
- exige que os dados caibam de forma adequada na memória;
- não é prático para bases muito grandes.

Por isso, em problemas grandes, usa-se mais GD/SGD.

---

## 19. Regressão polinomial

A regressão polinomial foi apresentada como uma forma de criar novos atributos a partir de um atributo existente.

Exemplo: a partir da altura $x$, criamos:

$$
x^2, x^3, ..., x^P
$$

Para um polinômio de ordem $P$:

$$
x_i = [1, x_i, x_i^2, ..., x_i^P]^T
$$

$$
w = [w_0, w_1, w_2, ..., w_P]^T
$$

$$
\hat{y}_i = w^Tx_i
$$

O ponto importante: o modelo é **não linear nos dados**, mas continua **linear nos parâmetros**. Por isso, ainda podemos usar GD, SGD ou OLS.

### Problema da ordem do polinômio

A pergunta principal foi:

> Como escolher a ordem $P$ do polinômio?

Polinômios de ordem baixa podem ser simples demais. Polinômios de ordem alta podem se ajustar demais aos dados observados, criando curvas biologicamente pouco plausíveis no exemplo do cateter.

---

## 20. Generalização

Generalização é a capacidade do modelo de predizer corretamente dados que não foram usados para ajustar seus parâmetros.

Procedimento básico:

1. separar dados em treinamento e teste;
2. treinar apenas com o conjunto de treinamento;
3. avaliar a capacidade de generalização no conjunto de teste.

A ideia central do professor: não queremos apenas acertar os dados que já vimos. Queremos ser bons no dado novo, no paciente novo, na situação nova.

### Overfitting

Ocorre quando o modelo se ajusta demais aos dados de treinamento.

Características:

- erro de treino baixo;
- erro de teste alto;
- modelo muito flexível;
- baixa capacidade de generalização.

### Underfitting

Ocorre quando o modelo é simples demais para capturar a estrutura dos dados.

Características:

- erro de treino alto;
- erro de teste também ruim;
- modelo pouco expressivo.

### Dilema viés-variância

- Underfitting: alto viés, baixa variância.
- Overfitting: baixo viés, alta variância.
- O ideal é equilibrar viés e variância.

---

## 21. Regularização L2

O professor explicou que modelos com parâmetros grandes em módulo tendem a generalizar menos, porque ficam muito sensíveis e flexíveis.

A regularização L2 adiciona um termo à função custo:

$$
J(w)=\frac{1}{2}(y-Xw)^T(y-Xw)+\frac{\lambda}{2}\|w\|^2
$$

com:

$$
\|w\|^2 = w^Tw = \sum_{d=1}^{D}w_d^2
$$

Interpretação:

- o primeiro termo mede o ajuste aos dados;
- o segundo termo penaliza parâmetros grandes.

$\lambda$ é um hiperparâmetro. Quanto maior $\lambda$, mais forte é a penalização.

O parâmetro $w_0$, associado ao bias/intercepto, normalmente não é regularizado.

A regularização L2 também é chamada de:

- ridge regression;
- weight decay.

### Atualização regularizada

No SGD, a regularização adiciona um termo de “decaimento” dos pesos:

$$
w(t)=w(t-1)+\alpha[e_i(t-1)x_i-\lambda w(t-1)]
$$

### Interpretação probabilística da regularização

O professor explicou que a regularização L2 também vem da gaussiana.

Se queremos parâmetros próximos de zero, podemos assumir uma priori gaussiana centrada em zero:

$$
w \sim \mathcal{N}(0, \sigma^2)
$$

A variância controla o quanto permitimos que os parâmetros se afastem de zero:

- variância maior: priori mais espalhada, regularização mais fraca;
- variância menor: priori mais concentrada em zero, regularização mais forte.

Isso conecta regularização com a solução **MAP — Maximum a Posteriori**.

- máxima verossimilhança: considera apenas a verossimilhança;
- MAP: considera verossimilhança + priori;
- solução bayesiana completa: considera a posteriori inteira, sem apenas maximizar.

---

## 22. Seleção de hiperparâmetros

Parâmetros são aprendidos pelo modelo. Hiperparâmetros são escolhidos antes ou fora do treinamento direto.

Exemplos de hiperparâmetros:

- ordem do polinômio $P$;
- taxa de aprendizado $\alpha$;
- regularização $\lambda$;
- número de vizinhos no KNN;
- profundidade de árvore;
- número de neurônios em uma rede neural.

### Hold-out validation

A separação passa a ter três conjuntos:

1. treinamento;
2. validação;
3. teste.

O conjunto de validação é usado para escolher hiperparâmetros. O conjunto de teste deve ser usado apenas no final, para avaliar o modelo escolhido.

### Grid search

Procedimento:

1. separar dados em treinamento, validação e teste;
2. construir uma lista de valores candidatos para os hiperparâmetros;
3. treinar o modelo para cada candidato;
4. avaliar no conjunto de validação;
5. escolher o candidato com melhor desempenho;
6. retreinar usando treinamento + validação;
7. avaliar uma única vez no teste.

O professor usou a ideia dos “hiperparâmetros campeões”: escolhemos os que tiveram melhor desempenho na validação e só depois fazemos a “prova final” no teste.

### K-fold cross-validation

O conjunto de treino é dividido em $K$ partições. Em cada rodada:

- uma partição fica para teste/validação;
- as outras $K-1$ são usadas para treino.

O processo se repete até que todas as partições tenham sido usadas para avaliação. Depois, calcula-se a média dos resultados.

A analogia do professor foi com uma prova mais justa. Uma prova única pode favorecer ou prejudicar dependendo do conteúdo sorteado. Várias avaliações cobrindo partes diferentes dão uma visão mais estável do desempenho.

---

## 23. Normalização dos dados

A normalização foi apresentada como muito importante para a maior parte dos modelos vistos.

Modelos que precisam de normalização, segundo a revisão do professor:

- regressão linear;
- regressão polinomial;
- regressão logística;
- regressão softmax.

Exceções comentadas:

- Naive Bayes Gaussiano;
- Análise do Discriminante Gaussiano.

No Naive Bayes Gaussiano, os cálculos são feitos separadamente por atributo, então diferenças de escala interferem menos. Na Análise do Discriminante Gaussiano, a inversa da matriz de covariância compensa a escala dos dados, funcionando quase como uma normalização interna.

---

## 24. Regressão logística binária

A regressão logística foi introduzida a partir de uma pergunta:

> Podemos usar regressão linear para classificação?

No exemplo da Íris, o objetivo era classificar flores entre Setosa e Versicolor a partir de medidas das sépalas.

Se codificarmos as classes como $-1$ e $1$, a regressão linear ainda retorna valores reais, como 0,3 ou 25. Isso não combina naturalmente com uma saída categórica.

Uma ideia seria usar:

$$
\hat{y}=\mathrm{sign}(w^Tx)
$$

Mas a função sinal não é diferenciável, o que dificulta o treinamento por gradiente.

A solução é usar uma função diferenciável entre 0 e 1: a **sigmoide logística**.

$$
\sigma(z)=\frac{1}{1+e^{-z}}
$$

Modelo:

$$
\hat{y}_i=\sigma(w^Tx_i)
$$

A saída pode ser interpretada como probabilidade:

$$
P(y=1|x,w)=\sigma(w^Tx)
$$

$$
P(y=0|x,w)=1-\sigma(w^Tx)
$$

### Bernoulli e regressão logística

Como a saída é 0 ou 1, usamos a distribuição de Bernoulli:

$$
p(y|x,w)=\sigma(w^Tx)^y(1-\sigma(w^Tx))^{1-y}
$$

### Função custo: entropia cruzada

A função custo surge como o negativo da log-verossimilhança:

$$
J(w)=-\frac{1}{N}\sum_{i=1}^{N}\left[y_i\log\sigma(w^Tx_i)+(1-y_i)\log(1-\sigma(w^Tx_i))\right]
$$

Essa é a **binary cross-entropy**.

O gradiente leva a uma forma muito parecida com a regressão linear:

$$
w(t)=w(t-1)+\alpha\frac{1}{N}\sum_{i=1}^{N}e_i(t-1)x_i
$$

com:

$$
e_i=y_i-\sigma(w^Tx_i)
$$

A diferença é que agora a predição passa pela sigmoide.

### Fronteira de decisão

A classe é decidida por um limiar, normalmente 0,5:

$$
\sigma(w^Tx) \geq 0{,}5 \Rightarrow y=1
$$

Isso equivale a:

$$
w^Tx \geq 0
$$

Logo, a fronteira de decisão da regressão logística padrão é linear.

### Dados não linearmente separáveis

Assim como na regressão polinomial, podemos criar atributos não lineares, como $x^2$, $x^3$, interações etc. Isso permite fronteiras não lineares. Se o modelo ficar flexível demais, usamos regularização.

---

## 25. De onde vem a função logística?

O professor derivou a sigmoide a partir do conceito de **odds**.

Se $p$ é a probabilidade da classe 1:

$$
\frac{p}{1-p}
$$

é a razão de chances.

O log-odds é:

$$
\log\frac{p}{1-p}
$$

A regressão logística assume que o log-odds é linear:

$$
\log\frac{p}{1-p}=w^Tx
$$

Manipulando:

$$
p=\frac{1}{1+e^{-w^Tx}}=\sigma(w^Tx)
$$

Assim, a função logística surge naturalmente quando modelamos o logaritmo da razão de chances como uma função linear dos atributos.

---

## 26. Regressão logística multiclasse e softmax

Quando há mais de duas classes, a saída precisa representar probabilidades para cada classe.

Usa-se **one-hot encoding**:

- classe 1: $[1,0,0]^T$;
- classe 2: $[0,1,0]^T$;
- classe 3: $[0,0,1]^T$.

A predição do modelo não é exatamente um vetor one-hot, mas um vetor de probabilidades, por exemplo:

$$
\hat{y}=[0{,}2,0{,}7,0{,}1]^T
$$

A classe escolhida é a de maior probabilidade.

### Por que não treinar várias regressões logísticas separadas?

Porque as probabilidades precisam ser consistentes:

$$
\sum_{k=1}^{K}p(y=k|x)=1
$$

Se treinarmos classificadores separados, não há garantia de que as probabilidades somem 1.

### Softmax

A função softmax transforma as saídas lineares em probabilidades normalizadas:

$$
\hat{y}_{ik}=\frac{\exp(w_k^Tx_i)}{\sum_{j=1}^{K}\exp(w_j^Tx_i)}
$$

Propriedades:

- cada saída fica entre 0 e 1;
- a soma das saídas é 1;
- pode ser interpretada probabilisticamente.

Também é chamada de:

- regressão softmax;
- regressão logística multinomial.

### Multiclass cross-entropy

A função custo multiclasse é:

$$
J(W)=-\frac{1}{N}\sum_{i=1}^{N}\sum_{k=1}^{K}y_{ik}\log\hat{y}_{ik}
$$

De novo, a atualização por GD/SGD fica parecida com os modelos anteriores:

$$
w_k(t)=w_k(t-1)+\alpha\frac{1}{N}\sum_{i=1}^{N}e_{ik}(t-1)x_i
$$

com:

$$
e_{ik}=y_{ik}-\hat{y}_{ik}
$$

O professor destacou uma ideia importante: em muitos modelos, inclusive redes neurais grandes, troca-se a função $w^Tx$ por uma função mais complexa, mas continua-se usando cross-entropy e SGD. Ou seja, o “maquinário” visto nessa aula reaparece em modelos muito maiores.

---

## 27. Classificadores estatísticos

A parte final apresentou classificadores estatísticos, especialmente os bayesianos.

A distinção central:

### Modelos discriminantes

Modelam diretamente a fronteira de decisão ou a probabilidade da classe dada a entrada:

$$
p(y|x)
$$

Exemplo:

- regressão logística.

### Modelos generativos

Modelam como os dados de cada classe são gerados:

$$
p(x|y)
$$

Depois usam a regra de Bayes para obter:

$$
p(y|x)
$$

Exemplos:

- classificadores bayesianos;
- Naive Bayes;
- Análise do Discriminante Gaussiano.

---

## 28. Classificadores Bayesianos

Problema:

> Dado um vetor de atributos $x$, a qual classe o padrão pertence?

Pela regra de Bayes:

$$
p(C_k|x)=\frac{p(x|C_k)p(C_k)}{p(x)}
$$

Como $p(x)$ é comum a todas as classes, para escolher a classe mais provável podemos usar proporcionalidade:

$$
p(C_k|x) \propto p(x|C_k)p(C_k)
$$

A decisão é:

$$
\hat{y}=\arg\max_k p(C_k|x)
$$

ou:

$$
\hat{y}=\arg\max_k p(x|C_k)p(C_k)
$$

As probabilidades a priori $p(C_k)$ podem ser:

- iguais para todas as classes;
- proporcionais à quantidade de exemplos por classe;
- conhecidas pela natureza do problema.

---

## 29. Análise do Discriminante Gaussiano — ADG

Na ADG, assumimos que os dados de cada classe foram gerados por uma distribuição gaussiana multivariada:

$$
p(x|C_k)=\mathcal{N}(x|\mu_k,\Sigma_k)
$$

Estimamos, para cada classe:

- média $\mu_k$;
- matriz de covariância $\Sigma_k$;
- priori $p(C_k)$.

Para classificar um novo ponto $x^*$, calculamos a probabilidade de ele ter sido gerado por cada gaussiana de classe e escolhemos a classe mais provável.

O professor destacou que a matriz de covariância influencia o formato das fronteiras:

- covariâncias diferentes entre classes podem gerar fronteiras quadráticas;
- covariâncias iguais podem gerar fronteiras lineares;
- covariância diagonal indica atributos sem correlação;
- covariância esférica indica mesma variância em todas as direções.

---

## 30. Naive Bayes

O Naive Bayes faz uma hipótese simplificadora:

> Os atributos são independentes entre si, dada a classe.

Assim:

$$
p(x|C_k)=\prod_{d=1}^{D}p(x_d|C_k)
$$

A predição fica:

$$
\hat{y}=\arg\max_k p(C_k)\prod_{d=1}^{D}p(x_d|C_k)
$$

Na prática, usa-se log para evitar problemas numéricos:

$$
\hat{y}=\arg\max_k \left[\log p(C_k)+\sum_{d=1}^{D}\log p(x_d|C_k)\right]
$$

### Naive Bayes Gaussiano

Quando cada atributo contínuo é modelado por uma gaussiana:

$$
p(x_d|C_k)=\mathcal{N}(x_d|\mu_{dk},\sigma^2_{dk})
$$

O modelo estima média e variância de cada atributo dentro de cada classe.

O professor apresentou esses classificadores como os primeiros modelos generativos vistos na aula. Em tese, eles permitem gerar novos dados: sorteia-se uma classe e depois sorteia-se uma amostra da distribuição associada a essa classe.

---

## 31. Observações finais da aula

No fim do dia, o professor recapitulou que, em um único dia, a turma viu:

- regressão linear;
- regressão polinomial;
- regressão logística;
- regressão softmax;
- Naive Bayes Gaussiano;
- Análise do Discriminante Gaussiano;
- grid search;
- hold-out validation;
- K-fold cross-validation;
- normalização de dados;
- conexão entre MSE, cross-entropy, gaussianas, Bernoulli, Bayes e SGD.

Também houve uma discussão final sobre identificação de imagens geradas por IA. O professor explicou que, em geral, problemas causados por IA acabam sendo tratados com outros modelos de IA. Para detectar imagens falsas, normalmente é necessário treinar outro modelo. Também comentou a ideia de marcas d’água invisíveis em imagens ou textos gerados por IA, mas apontou que esse ainda é um problema difícil e parcialmente em aberto.

---

# Mapa rápido dos modelos vistos

| Modelo | Tipo de tarefa | Saída | Distribuição/ideia probabilística | Função custo típica | Treinamento |
|---|---|---|---|---|---|
| Regressão linear | Regressão | Valor contínuo | Ruído gaussiano | MSE | GD, SGD, OLS |
| Regressão polinomial | Regressão | Valor contínuo | Ruído gaussiano + atributos não lineares | MSE | GD, SGD, OLS |
| Ridge/L2 | Regressão regularizada | Valor contínuo | Priori gaussiana nos pesos | MSE + penalização L2 | GD, SGD, OLS regularizado |
| Regressão logística binária | Classificação binária | Probabilidade de classe 1 | Bernoulli | Binary cross-entropy | GD, SGD |
| Regressão softmax | Classificação multiclasse | Vetor de probabilidades | Categórica/Multinoulli | Multiclass cross-entropy | GD, SGD |
| Naive Bayes Gaussiano | Classificação | Classe mais provável | Gaussianas por atributo e classe | Máxima posteriori | Estimação de médias/variâncias |
| ADG | Classificação | Classe mais provável | Gaussiana multivariada por classe | Máxima posteriori | Estimação de médias/covariâncias |

---

# Ideias que o professor reforçou bastante

1. **Aprendizagem de máquina não é só aplicar biblioteca.** É preciso entender o que está por trás do modelo.
2. **Probabilidade é central.** Vários modelos aparecem como formas de modelar incerteza.
3. **O dado novo importa mais que o dado visto.** A palavra-chave é generalização.
4. **Overfitting é perigoso.** Um modelo pode parecer excelente no treino e ruim no teste.
5. **Regularização controla flexibilidade.** Penalizar parâmetros grandes ajuda a reduzir sobreajuste.
6. **SGD é fundamental.** É simples, ruidoso, mas escalável e aparece em modelos modernos.
7. **A gaussiana aparece por conveniência matemática.** Mesmo quando não é perfeita, ela simplifica muita coisa.
8. **Cross-entropy + SGD reaparece em redes neurais.** O que foi visto em regressão logística é uma base para modelos mais complexos.
9. **Teste não é validação.** O conjunto de teste deve ser guardado para a avaliação final.
10. **Não existe algoritmo universalmente melhor.** O No Free Lunch justifica estudar várias ferramentas.

---

