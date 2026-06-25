# Metodologia de Pesquisa — Aula 02

---

## Sumário

1. [Continuação da disciplina](#1-continuação-da-disciplina)
2. [Ciência da Computação: teoria, prática e maturidade da área](#2-ciência-da-computação-teoria-prática-e-maturidade-da-área)
3. [O método científico e suas influências](#3-o-método-científico-e-suas-influências)
4. [Empirismo](#4-empirismo)
5. [Positivismo](#5-positivismo)
6. [Filosofia como metaciência](#6-filosofia-como-metaciência)
7. [Pragmatismo, realismo científico e limites da percepção](#7-pragmatismo-realismo-científico-e-limites-da-percepção)
8. [Objetividade e ciência aberta](#8-objetividade-e-ciência-aberta)
9. [Indução, refutação e coerência](#9-indução-refutação-e-coerência)
10. [A ciência como processo conservador](#10-a-ciência-como-processo-conservador)
11. [Lâmina de Ockham](#11-lâmina-de-ockham)
12. [Natureza dos trabalhos de pesquisa](#12-natureza-dos-trabalhos-de-pesquisa)
13. [Hipótese, tese e teoria](#13-hipótese-tese-e-teoria)
14. [Objetivos da pesquisa: exploratória, descritiva e explicativa](#14-objetivos-da-pesquisa-exploratória-descritiva-e-explicativa)
15. [Procedimentos de pesquisa](#15-procedimentos-de-pesquisa)
16. [Ciência e tecnologia](#16-ciência-e-tecnologia)
17. [Estilos de pesquisa em computação](#17-estilos-de-pesquisa-em-computação)
18. [Pesquisas teóricas, formais, empíricas e exploratórias](#18-pesquisas-teóricas-formais-empíricas-e-exploratórias)
19. [Preparação de um trabalho de pesquisa](#19-preparação-de-um-trabalho-de-pesquisa)
20. [Revisão bibliográfica, literatura cinza e leitura crítica](#20-revisão-bibliográfica-literatura-cinza-e-leitura-crítica)
21. [Hipótese, resultados esperados, limitações e ameaças à validade](#21-hipótese-resultados-esperados-limitações-e-ameaças-à-validade)
22. [Ideias centrais da aula](#22-ideias-centrais-da-aula)

---

## 1. Continuação da disciplina

A aula dá continuidade ao conteúdo iniciado anteriormente e começa com a apresentação do **capítulo 3**, que deveria ter sido lido pelos alunos na aula passada. O professor comenta que, quando uma aula acontece imediatamente após a outra, fica mais difícil manter o ritmo de leitura.

O capítulo tratado é dedicado aos **princípios do método científico**.

O professor inicia com uma ironia comum na computação:

> “Na computação, a teoria é quando o fenômeno é compreendido, mas não funciona. A prática é quando funciona, mas não se sabe por quê.”

A partir disso, ele completa dizendo que, por muito tempo, na computação coexistiram teoria e prática: nada funcionava e ninguém sabia exatamente o porquê.

Essa provocação serve para introduzir a ideia de que a computação, durante muito tempo, teve uma relação complicada com o método científico. Muitas vezes, algo funcionava apenas em um ambiente específico — o famoso “funcionou no meu computador” — ou então havia uma explicação teórica, mas a implementação prática não funcionava.

---

## 2. Ciência da Computação: teoria, prática e maturidade da área

Segundo o professor, a geração atual encontra a Ciência da Computação em um estado mais maduro do que em décadas anteriores.

Um sinal dessa maturidade está no próprio nome da área. Em alguns países, como a França, a área é chamada de **informática** sem sentido pejorativo, da mesma forma que se fala em matemática ou química. No Brasil, por muito tempo, usou-se a expressão **Ciência da Computação** para reforçar que a computação também era uma ciência.

O professor observa que o departamento de computação fica no Centro de Ciências, mas poucos cursos ainda precisam carregar explicitamente a palavra “ciência” no nome. Isso indica que a área, à medida que amadurece, deixa de precisar se afirmar o tempo todo como ciência.

Um exemplo dessa mudança é o caso da CAPES: o comitê antes associado ao nome “Ciência da Computação” passou a ser chamado de **Comitê de Avaliação da Computação (CAC)**.

### 2.1 A juventude da área

A computação é uma área muito nova. Quando o termo **Ciência da Computação** foi definido como uma área separada da matemática e da engenharia elétrica, muitas das pessoas responsáveis por construir seus fundamentos ainda estavam vivas.

Além disso, muitas dessas pessoas não tinham formação originalmente em computação. Vieram de áreas como:

- matemática;
- engenharia elétrica;
- lógica;
- física;
- outras áreas formais ou aplicadas.

Isso fez com que, por muito tempo, a computação tivesse dificuldade em definir com clareza seu **objeto de estudo**.

### 2.2 Qual é o objeto de estudo da computação?

Durante muito tempo, a computação foi definida principalmente em termos de:

- algoritmos;
- teoria da computação;
- teoria da compilação;
- estruturas formais;
- fundamentos matemáticos da computação.

Com o amadurecimento da área, a computação passou a desenvolver disciplinas próprias, muitas delas interdisciplinares.

Exemplos citados pelo professor:

- **Redes de computadores** têm forte sinergia com telecomunicações e engenharia de redes.
- **Engenharia de software** tem interface com administração e sociologia, pois envolve aspectos humanos da produção de software.
- **Interação humano-computador** se aproxima das ciências comportamentais e da semiótica, pois estuda como as pessoas interagem com sistemas, como selecionam itens, como se cansam e como sua atenção pode ser capturada ou preservada.

À medida que surgem pessoas formadas diretamente na área de computação, o corpo de conhecimento vai se tornando mais concentrado e mais próprio da área.

Por isso, o método científico aplicado à computação hoje é mais ajustado aos tipos de pesquisa que a área realiza.

---

## 3. O método científico e suas influências

O professor destaca alguns termos e influências importantes para compreender o método científico:

- empirismo;
- positivismo;
- pragmatismo;
- objetividade;
- indução;
- refutação;
- contradição;
- coerencismo.

Antes de explicar essas correntes, ele comenta o uso da palavra **empírico**. Em português, às vezes o termo é usado de forma pejorativa, como se algo empírico fosse algo “sem rigor”. No contexto da aula, o professor usa o termo no sentido inglês de *empirical*, isto é, relacionado à experimentação, observação e coleta de evidências.

O método científico se desenvolve a partir de diferentes formas de buscar a verdade. Há correntes que acreditam que o universo possui leis gerais já existentes e que o trabalho da ciência seria simplesmente descobri-las. Outras correntes assumem que a ciência trabalha com fenômenos observados, modelos e explicações que podem ser substituídas quando deixam de explicar adequadamente a realidade.

### 3.1 Gödel, incompletude e limites da lógica

O professor menciona Gödel como exemplo de alguém que “bagunça” a visão de que tudo poderia ser explicado por uma lógica completa.

Gödel demonstrou a **incompletude da aritmética**: não é possível usar a lógica para descrever completamente a própria aritmética. Em termos simplificados, há limites internos nos sistemas formais.

O professor usa exemplos relacionados aos conjuntos numéricos:

- não se pode estabelecer completamente o “tamanho” dos inteiros usando apenas os próprios inteiros;
- para comparar certos conjuntos, é necessário usar uma linguagem ou estrutura maior;
- não é possível usar apenas os inteiros para contar tudo que existe entre 0 e 1 nos reais.

A partir disso, ele mostra que há problemas que não podem ser resolvidos dentro da própria linguagem do problema.

### 3.2 O paradoxo “esta frase é falsa”

Outro exemplo usado é o paradoxo:

> “Esta frase é falsa.”

Se a frase for verdadeira, então ela é falsa.  
Se ela for falsa, então o que ela afirma é verdadeiro.

Esse paradoxo não se resolve adequadamente na mesma linguagem em que foi formulado. É necessária uma **metalinguagem**, isto é, uma linguagem externa que consiga interpretar a linguagem original.

Esse exemplo ajuda a mostrar que nem tudo pode ser explicado internamente por uma única estrutura lógica.

### 3.3 Consequência para a ciência

Essas discussões abalaram a crença de que tudo teria uma explicação lógica, definitiva e universal. O professor cita a busca de Stephen Hawking por uma grande lei ou teoria unificada como exemplo desse desejo de encontrar uma explicação geral.

A aula, porém, enfatiza que a ciência passa a trabalhar muito mais com:

- observação;
- vivência;
- evidências;
- modelos explicativos;
- fenômenos temporais;
- teorias que permanecem válidas enquanto continuam explicando os fenômenos.

Se um método é usado, investigado e continua produzindo explicações consistentes, a comunidade científica passa a aceitar esse modelo como razoável. Mas, se em algum momento ele deixa de explicar novos fatos, a ciência pode substituí-lo por outro.

---

## 4. Empirismo

O empirismo é apresentado como uma das influências mais importantes do método científico.

Em termos simples, o empirismo parte da ideia de:

> “Só acredito vendo.”

Ou seja, o conhecimento precisa estar baseado na experiência, na observação e na evidência.

O professor brinca dizendo que talvez o primeiro “empírico” na Bíblia tenha sido São Tomé, porque ele quis ver as marcas para acreditar.

### 4.1 Empirismo e dogma

Em áreas dogmáticas, termos como empirismo ou materialismo às vezes são usados para indicar uma postura de afastamento do espiritual ou da crença no dogma. A ciência, porém, não assume nada como verdade absoluta.

O professor reforça que:

- a ciência não tem dogmas;
- ela não parte de verdades absolutas;
- ela trabalha com observações, hipóteses, métodos e evidências.

### 4.2 O empirismo nas ciências humanas e o exemplo da canonização

O professor faz uma observação curiosa: por contraditório que pareça, talvez um dos métodos empíricos mais robustos nas ciências humanas seja o processo de **canonização** na Igreja Católica.

Nesse processo, existe uma tentativa de investigar e eliminar explicações alternativas para verificar se algo pode ser considerado milagre. A lógica usada se aproxima da ideia de excluir o impossível.

### 4.3 Carl Sagan e a Terra curva

Carl Sagan é citado como alguém que explicava muito bem o raciocínio científico com base em observações simples.

Um exemplo mencionado é a demonstração de que a Terra é curva a partir da observação das sombras. Isso combina:

- observação;
- comparação;
- cálculo;
- abstração.

### 4.4 Astronomia e observação

Astrônomos conseguiam prever com precisão a passagem de cometas a partir da observação sistemática e da abstração matemática. Isso mostra como a observação repetida, quando organizada metodologicamente, permite explicar e prever fenômenos.

### 4.5 Descartes e o “Discurso do Método”

Descartes é citado como alguém que escreveu o **Discurso do Método** para apresentar uma forma de tratar a ciência de maneira aceitável para a sociedade ocidental da época.

O professor comenta que Descartes precisava escrever de uma forma que não o colocasse em conflito direto com a ordem religiosa de seu tempo. Ele começa tratando a curiosidade como uma busca pela verdade.

No modelo empírico, a base está em:

- observar;
- eliminar influências externas;
- controlar variáveis;
- chegar a explicações consistentes.

---

## 5. Positivismo

O positivismo aparece como outra influência importante.

A ideia central é que a ciência deve se basear em **valores humanos e observáveis**, deixando teologia, misticismo e metafísica em outra esfera.

O professor exemplifica com o contexto da Idade Média: se alguém dissesse que o Sol não girava em torno da Terra, poderia ser acusado de heresia.

O positivismo surge com força em períodos de mudança, defendendo que as explicações científicas devem se apoiar em fenômenos observáveis e não em explicações místicas.

Se existe uma relação de gravidade, por exemplo, essa relação deve ser investigada cientificamente, sem recorrer a explicações externas ao fenômeno.

### 5.1 Metafísica e ciência

O professor destaca que a metafísica depois retorna à discussão científica por meio da filosofia, pois a filosofia é a “mãe” da ciência. A filosofia permite discutir fundamentos, limites e métodos das ciências.

---

## 6. Filosofia como metaciência

A aula enfatiza que cada ciência se debruça sobre um objeto específico:

- a computação estuda objetos da computação;
- a medicina estuda objetos da medicina;
- a biologia estuda objetos da biologia;
- a física estuda objetos da física.

O professor comenta que não há conflito direto entre ciência e crença religiosa quando cada área respeita seu objeto de estudo. Deus, por exemplo, não é objeto de estudo da computação nem da medicina.

São citados exemplos de cientistas com crenças religiosas ou discussões relacionadas:

- Mendel, pai da genética, era monge;
- Darwin é citado no contexto da evolução;
- Gödel teria elaborado uma prova lógica da existência de Deus.

A questão principal é: uma ciência específica explica seu próprio objeto, mas não necessariamente explica o método de pesquisa em si.

O método científico, os critérios de validade e os fundamentos da pesquisa são objetos de reflexão da **filosofia**.

Por isso, o professor caracteriza a filosofia como uma espécie de **metaciência**: ela consegue falar sobre as outras ciências, seus métodos, seus limites e seus pressupostos.

Os filósofos podem observar os métodos de pesquisa usados em outras áreas e avaliar, por exemplo, se há viés, se há contradição ou se os pressupostos estão bem fundamentados.

---

## 7. Pragmatismo, realismo científico e limites da percepção

O professor contrapõe duas visões:

### 7.1 Realismo científico

O realismo científico tende a acreditar que a ciência descobre as leis que regem a realidade. Nessa visão, existe uma realidade objetiva, e a ciência vai se aproximando dela.

### 7.2 Pragmatismo

O pragmatismo, por outro lado, assume que talvez não seja possível saber exatamente o que é a realidade. A ciência explicaria fenômenos observados e construiria previsões úteis.

O foco pragmático está menos em afirmar que se descobriu a realidade “como ela é” e mais em construir modelos que funcionam para explicar e prever fenômenos dentro de um recorte.

### 7.3 Limites da percepção humana

O professor destaca que nossa percepção é limitada. Captamos apenas uma pequena parte da luz, do som e dos fenômenos existentes.

Somos máquinas de percepção limitadas. Para estudar fenômenos que não conseguimos perceber diretamente, criamos ferramentas de medição.

Exemplos:

- instrumentos para captar ondas;
- aceleradores de partículas;
- telescópios;
- equipamentos que detectam sinais vindos do Big Bang;
- cálculos baseados em velocidade, distância e tempo.

### 7.4 Carl Sagan e as dimensões

Carl Sagan é citado novamente por sua explicação sobre dimensões usando o exemplo de uma maçã.

Se um ser vivesse em um plano bidimensional, com apenas os eixos X e Y, ele não conseguiria perceber diretamente o eixo Z. Se uma maçã atravessasse esse plano, o ser perceberia apenas projeções ou sombras.

A partir disso, o professor mostra que a realidade pode ser mais ampla do que aquilo que conseguimos perceber diretamente.

Sagan também brincava com a ideia de que a quarta dimensão poderia ser percebida como algo que não conseguimos acessar diretamente. O professor menciona o som como exemplo em uma explicação intuitiva.

A ideia central é: talvez existam dimensões ou aspectos da realidade que não percebemos porque somos limitados biologicamente. A ciência cria ferramentas para capturar indícios desses fenômenos.

---

## 8. Objetividade e ciência aberta

A objetividade é apresentada como uma característica fundamental do método científico.

Em termos simples, a objetividade envolve a possibilidade de que duas pessoas competentes, analisando os mesmos dados e usando o mesmo método, cheguem às mesmas conclusões.

Isso ajuda a afastar opiniões puramente subjetivas.

### 8.1 Open Science

O professor menciona o movimento de **Open Science** ou **ciência aberta**.

A ciência aberta busca disponibilizar:

- dados;
- métodos;
- scripts;
- procedimentos;
- ambientes;
- pacotes de replicação.

A intenção é permitir que outros pesquisadores consigam reproduzir o experimento.

Na computação, isso é especialmente importante, porque muitos experimentos usam:

- código;
- scripts;
- datasets;
- ambientes de execução;
- parâmetros de configuração.

Se o script é determinístico e os dados são os mesmos, o resultado deveria ser o mesmo em outra máquina.

### 8.2 Reprodutibilidade

Se os resultados não batem, há algumas possibilidades:

- os dados foram alterados;
- o experimento não foi descrito adequadamente;
- há algum problema de ambiente;
- houve falha na reprodutibilidade;
- alguém pode ter falseado resultados.

Por isso, em computação, valoriza-se cada vez mais o uso de **pacotes de replicação**.

Ao submeter um artigo, muitas vezes o autor indica onde estão os dados, scripts e materiais para que revisores possam verificar os resultados.

### 8.3 Dados proprietários

Há exceções. Em projetos com empresas, como em contextos industriais, os dados podem ser proprietários. Nesse caso, nem sempre é possível abrir tudo, por questões de propriedade intelectual, sigilo ou segurança.

Mesmo assim, quando os dados são públicos ou o software é *open source*, espera-se que haja abertura dos materiais usados na pesquisa.

---

## 9. Indução, refutação e coerência

### 9.1 Indução

A indução é comparada às provas matemáticas no sentido de criar uma explicação para um fenômeno observado.

A lógica é observar um fenômeno em diferentes contextos e verificar se a explicação continua se sustentando.

Se o fenômeno se repete em contextos distintos e a explicação funciona, a hipótese ganha força. Dependendo do tipo de evento, podem ser usadas regras probabilísticas para avaliar se as observações são significativas o suficiente para sustentar uma teoria.

A indução ajuda a extrapolar um argumento com base em observações repetidas, mas ela não dá uma certeza absoluta. Ela fortalece uma explicação.

### 9.2 Refutação

O princípio da refutação afirma que qualquer teoria científica deve estar aberta à possibilidade de ser invalidada.

Se surgem novos fatos que a teoria não consegue explicar, ela pode ser substituída, revisada ou enfraquecida.

O professor menciona Louis Pasteur e os experimentos com balões de bico de cisne. Pasteur ajudou a refutar a ideia de **geração espontânea**, demonstrando que a vida não surgia do nada, mas de microrganismos já existentes.

### 9.3 Contradição

A ciência também avança quando teorias entram em contradição com novos fenômenos.

O professor menciona a tensão entre física clássica e física quântica. A física clássica funciona bem em determinados contextos, mas não explica adequadamente fenômenos em escala subatômica. Nesses casos, outras teorias e modelos são necessários.

### 9.4 Coerencismo

O coerencismo é associado ao pragmatismo. Uma teoria deve ser coerente e plausível para explicar o fenômeno.

O professor usa como exemplo uma pesquisadora que estudava recuperação de movimentos da medula. A crítica científica, nesse caso, não dizia respeito à seriedade da pesquisadora, mas ao rigor metodológico do estudo.

Ou seja: a intenção pode ser boa, o resultado pode ser promissor, mas, se o método não permite sustentar a conclusão, a ciência precisa ser cautelosa.

---

## 10. A ciência como processo conservador

A ciência é apresentada como conservadora no sentido metodológico: ela exige cautela antes de aceitar uma explicação ou elevar uma esperança.

No caso de medicamentos ou tratamentos, por exemplo, não basta observar que alguns pacientes melhoraram. É preciso investigar se a melhora foi de fato causada pelo medicamento.

Para isso, usam-se experimentos controlados, com:

- grupo controle;
- grupo de tratamento;
- controle de variáveis;
- aleatoriedade;
- comparação estatística;
- análise de efeitos colaterais.

### 10.1 Grupo controle e grupo de tratamento

O grupo controle é aquele que não recebe a intervenção principal, ou recebe placebo.

O grupo de tratamento é aquele que recebe a intervenção investigada.

A comparação entre os dois grupos ajuda a verificar se a diferença observada pode ser atribuída ao tratamento.

### 10.2 O problema de estudos não controlados

O professor comenta um caso em que um artigo foi rejeitado porque o experimento não era controlado. Sem grupo controle e sem desenho experimental adequado, não era possível afirmar com segurança que os resultados eram causados pela intervenção.

Mesmo quando um resultado parece promissor, a ciência evita transformá-lo em esperança pública sem evidências robustas.

### 10.3 Pesquisa farmacêutica

Na indústria farmacêutica, o custo de desenvolver uma droga nova é altíssimo. Empresas fazem apostas de centenas de milhões de reais.

Se a droga funciona, a empresa pode patentear e lucrar muito. Se falha, o prejuízo também é enorme.

O professor menciona:

- estudos de medicamentos contra câncer;
- um remédio para câncer de pâncreas que aumentou a sobrevida em cerca de seis meses e foi muito celebrado;
- o caso do Viagra, que teria surgido de forma acidental durante testes da Pfizer relacionados ao coração, como também é mostrado no filme *O Amor e Outras Drogas*.

Esses exemplos reforçam que a ciência exige muitos testes antes de aceitar uma relação causal.

---

## 11. Lâmina de Ockham

A **Lâmina de Ockham** é apresentada como o princípio segundo o qual, entre várias teorias que explicam o mesmo fenômeno, deve-se preferir a mais simples.

Isso não significa escolher uma explicação simplista ou incompleta, mas evitar complexidade desnecessária.

O professor menciona que cientistas como Newton e Einstein buscavam elegância e simplicidade nas fórmulas. A ciência valoriza explicações que sejam:

- simples;
- consistentes;
- elegantes;
- suficientes para explicar o fenômeno.

---

## 12. Natureza dos trabalhos de pesquisa

O professor apresenta duas grandes naturezas de trabalho:

1. **trabalhos originais**;
2. **trabalhos de resumo ou revisão**.

### 12.1 Trabalhos originais

Trabalhos originais envolvem:

- novas descobertas;
- novas hipóteses;
- novos experimentos;
- novos resultados científicos;
- avanço do estado da arte.

São os chamados **estudos primários**, pois apresentam resultados obtidos diretamente por investigação.

### 12.2 Trabalhos de resumo ou revisão

Trabalhos de resumo organizam o conhecimento já existente. Podem ser:

- revisões bibliográficas;
- revisões sistemáticas;
- estudos secundários;
- estudos terciários.

O professor reforça que a revisão é importante porque organiza o conhecimento e acelera a entrada de novas pessoas em uma área.

Uma revisão de um período longo, como dez anos, tende a representar um conhecimento mais consolidado, porque já passou por mais tempo de discussão e maturação.

### 12.3 Livros e conhecimento consolidado

Em graduações, os alunos geralmente estudam por livros que organizam conhecimento consolidado, como lógica de programação, estruturas de dados e fundamentos de computação.

Esses livros não apresentam necessariamente a fronteira do estado da arte, mas sistematizam aquilo que já foi aceito pela comunidade como conhecimento básico.

---

## 13. Hipótese, tese e teoria

O professor diferencia hipótese, tese e teoria.

### 13.1 Hipótese

A hipótese é uma afirmação que pode ser investigada, testada e eventualmente corroborada ou refutada.

Exemplo apresentado:

> “O uso de LLM aumenta a qualidade do código.”

Essa hipótese precisa ser testada por meio de um experimento.

### 13.2 Tese

A tese é uma explicação mais fundamentada para um fenômeno.

Se um experimento mostra que o uso de LLM aumentou a qualidade do código, ainda é necessário explicar **por que** isso aconteceu.

Uma tese possível seria:

> A automação reduz erros humanos e, por isso, melhora a qualidade do código.

Mas essa tese precisaria de sustentação empírica.

O professor destaca que, se o experimento foi feito em apenas um projeto, ele não permite generalizar para todos os contextos. Para sustentar uma tese mais ampla, seria necessário um experimento em larga escala, por exemplo, com milhares de repositórios.

### 13.3 Teoria

Uma tese corroborada ao longo do tempo, por diferentes estudos, pode ganhar força e eventualmente se tornar uma teoria.

Mas isso demora. A teoria depende de:

- repetição;
- evidências acumuladas;
- aceitação pela comunidade;
- capacidade de explicar fenômenos em diferentes contextos.

### 13.4 Doutorado

No doutorado, a missão é construir uma tese. O professor afirma que o doutoramento é um treinamento para formar pesquisador.

A tese é a aplicação do método científico para explicar um fenômeno. Para ganhar força, ela precisa ser divulgada e aceita pela comunidade científica, por meio de publicações, citações e uso por outros pesquisadores.

Publicar em lugares de prestígio, como conferências importantes da área, ajuda a tese a ganhar tração.

Às vezes, um trabalho só se torna influente anos depois, quando alguém precisa daquela solução ou daquele argumento.

### 13.5 Exemplo da vacina da Covid

O professor menciona que a vacina da Covid foi desenvolvida rapidamente porque a plataforma tecnológica já vinha sendo estudada há cerca de dez anos. Assim, não se começou do zero: a tecnologia foi adaptada para o novo vírus em um período mais curto.

---

## 14. Objetivos da pesquisa: exploratória, descritiva e explicativa

Quanto ao objetivo, a pesquisa pode ser:

1. exploratória;
2. descritiva;
3. explicativa.

### 14.1 Pesquisa exploratória

A pesquisa exploratória investiga relações, possibilidades e padrões iniciais.

Na ciência de dados, por exemplo, uma análise exploratória pode buscar relações entre variáveis sem necessariamente explicar causalmente o fenômeno.

A pesquisa exploratória ajuda a entender o terreno e levantar hipóteses.

### 14.2 Pesquisa descritiva

A pesquisa descritiva detalha como o fenômeno ocorre.

Ela pode descrever:

- categorias;
- frequência;
- evolução temporal;
- padrões observados;
- características de um processo.

Por exemplo, pode-se descrever como determinado processo ocorre ao longo do tempo ou como diferentes grupos se comportam diante de uma tecnologia.

### 14.3 Pesquisa explicativa

A pesquisa explicativa busca a causa.

Ela pergunta por que determinado fenômeno acontece.

Exemplo citado pelo professor:

- uma demora pode ocorrer por excesso de burocracia;
- ou por sobrecarga;
- ou por falta de automação;
- ou por outro fator causal.

No mestrado, muitas vezes o trabalho chega até o nível descritivo ou explicativo. No doutorado, espera-se normalmente propor uma solução e testá-la.

---

## 15. Procedimentos de pesquisa

O professor apresenta alguns procedimentos de pesquisa.

### 15.1 Pesquisa bibliográfica

A pesquisa bibliográfica consiste em ler e analisar o que já está consolidado em artigos, livros e outros materiais acadêmicos.

Ela é essencial para:

- entender o estado da arte;
- identificar lacunas;
- evitar resolver um problema já resolvido;
- construir o referencial teórico;
- definir melhor o objetivo da pesquisa.

### 15.2 Pesquisa documental

A pesquisa documental usa documentos como fonte primária.

Exemplo citado: documentos da NASA sobre métodos formais utilizados em projetos como a Curiosity.

Projetos da NASA duram décadas e produzem grande volume de documentação. Parte desse material, depois de certo tempo, é disponibilizada para universidades e pesquisadores.

Documentos oficiais podem ser usados para reconstruir decisões, métodos, práticas e evidências.

### 15.3 Pesquisa experimental

A pesquisa experimental envolve manipular variáveis em um ambiente controlado.

O professor distingue:

- **variáveis controladas**: permanecem fixas para não interferir no resultado;
- **variáveis livres**: são observadas ou manipuladas para analisar o fenômeno.

Exemplo simples:

- temperatura pode ser uma variável controlada;
- concentração dos alunos pode ser uma variável observada.

O objetivo do experimento é reduzir interferências e estabelecer relações mais fortes entre variáveis.

### 15.4 Pesquisa-ação

A pesquisa-ação ocorre quando o pesquisador interage com os sujeitos da pesquisa.

É comum na educação, pois o pesquisador pode atuar diretamente em uma turma, aplicar uma intervenção, observar efeitos e ajustar a prática.

Nesse tipo de pesquisa, o pesquisador não é totalmente externo ao fenômeno. Ele participa dele.

---

## 16. Ciência e tecnologia

O professor diferencia ciência e tecnologia.

### 16.1 Ciência

A ciência busca:

- conhecimento;
- explicações;
- teorias;
- compreensão de fenômenos.

A ciência tenta explicar o mundo.

### 16.2 Tecnologia

A tecnologia é a aplicação prática do conhecimento para transformar o mundo.

Ela pode ter finalidade:

- industrial;
- econômica;
- social;
- prática;
- cultural.

A tecnologia não necessariamente explica o mundo; ela o modifica.

O professor usa o livro como exemplo de tecnologia. O livro é uma tecnologia poderosa para armazenar e transmitir conhecimento.

### 16.3 Tecnologia além da ciência

Nem toda tecnologia nasce diretamente da ciência formal. Muitas tecnologias vêm do conhecimento humano acumulado, inclusive de povos originários e saberes tradicionais.

A indústria muitas vezes traduz esses saberes em produtos, processos e tecnologias aplicadas.

Na computação, trabalha-se muito com abstrações digitais, mas essas abstrações também são formas tecnológicas de organizar e transformar a realidade.

---

## 17. Estilos de pesquisa em computação

O professor apresenta diferentes estilos de pesquisa em computação.

### 17.1 Apresentação de um produto

Um trabalho pode apresentar um produto inovador, como uma ferramenta, um protótipo ou um sistema.

Nesse caso, a contribuição pode estar na criação de algo novo.

Workshops muitas vezes aceitam esse tipo de trabalho, especialmente quando a ferramenta é útil, mesmo sem um rigor científico extremo.

### 17.2 Algo diferente

Outro estilo é apresentar algo diferente do que já existe.

Nesse caso, a comparação pode ser qualitativa: mostra-se que a proposta tem características novas ou distintas em relação às anteriores.

### 17.3 Algo presumivelmente melhor

Nesse estilo, o trabalho compara sua proposta com a literatura e argumenta que ela é melhor em algum aspecto.

É necessário usar indicadores, métricas ou evidências.

Exemplo: se não há um *benchmark* padrão, o pesquisador pode comparar sua proposta com resultados reportados em artigos relacionados.

### 17.4 Algo reconhecidamente melhor

Esse é um nível mais forte de pesquisa.

A proposta é comparada com *benchmarks* globais ou *gold standards*.

Exemplos:

- datasets padronizados;
- competições científicas;
- rankings de algoritmos;
- métricas de referência;
- experimentos reproduzíveis.

Nesse caso, a avaliação tira parte da subjetividade, pois todos comparam suas soluções em condições semelhantes.

### 17.5 Exemplos da computação

O professor menciona o uso do PostgreSQL em pesquisas de banco de dados. Pesquisadores podem implementar novos algoritmos de indexação ou otimização dentro de um sistema real e medir:

- tempo;
- uso de memória;
- uso de processador;
- desempenho de consulta;
- escalabilidade.

O *benchmark* é importante porque permite comparar propostas de forma mais objetiva.

### 17.6 Indústria farmacêutica como analogia

Na indústria farmacêutica, simulações computacionais podem prever a atividade de moléculas antes de investir em laboratório.

Isso mostra como modelos computacionais e *benchmarks* ajudam a reduzir custo e risco em pesquisas aplicadas.

### 17.7 Datasets padronizados

O professor também menciona datasets padronizados, como coleções de imagens médicas, que permitem comparar algoritmos em rankings globais.

Quando todos usam o mesmo conjunto de dados, a comparação se torna mais justa.

---

## 18. Pesquisas teóricas, formais, empíricas e exploratórias

### 18.1 Computação teórica

Na computação teórica, faz-se demonstração de propriedades.

Exemplo:

> Demonstrar que um algoritmo tem complexidade `n log n`.

Esse tipo de trabalho usa:

- lógica;
- matemática;
- provas formais;
- indução;
- prova por absurdo;
- demonstração de propriedades.

### 18.2 Provas por indução e por absurdo

A pesquisa teórica pode usar técnicas como:

- prova por indução;
- prova por contradição ou absurdo;
- derivações lógicas.

O objetivo é mostrar formalmente que uma propriedade se sustenta.

### 18.3 Processos estocásticos em redes

Em redes de computadores, processos estocásticos podem ser usados para analisar entrega de pacotes, comportamento de tráfego, anomalias e ataques.

Esses modelos ajudam a lidar com fenômenos probabilísticos.

### 18.4 Classificação da pesquisa

O professor classifica a pesquisa, nesse trecho, como:

- **formal**: baseada em lógica, matemática e demonstrações;
- **empírica**: baseada em observação, dados, estatística e experimentação;
- **exploratória**: baseada em estudo de caso, investigação inicial ou levantamento de padrões.

### 18.5 Estudos de caso

Estudos de caso não provam de forma generalizada, mas podem ser úteis para convencer em um contexto específico.

Um estudo de caso pode usar:

- relato de experiência;
- questionários;
- entrevistas;
- análise de processo;
- métricas internas.

Um caso isolado não generaliza, mas vários casos podem fortalecer uma hipótese.

### 18.6 Google, Netflix e relatos industriais

O professor menciona que empresas como Google e Netflix publicam muitos artigos baseados em seus próprios casos internos.

Essas empresas têm sistemas complexos e ambientes de escala muito grande. Mesmo que os resultados sejam contextuais, podem inspirar práticas em outras organizações.

O valor científico está em mostrar:

- como chegaram ao resultado;
- qual era o problema;
- quais foram as decisões;
- quais evidências foram coletadas;
- quais limitações existem.

### 18.7 Chaos Monkey

O **Chaos Monkey**, da Netflix, é citado como exemplo de ferramenta para testar resiliência em ambientes caóticos de nuvem e microsserviços.

A ideia é derrubar máquinas ou serviços de forma controlada para verificar se o sistema continua funcionando.

Se o serviço permanece bom mesmo com falhas provocadas, isso indica que a engenharia de confiabilidade é robusta.

---

## 19. Preparação de um trabalho de pesquisa

A aula avança para o capítulo 6, que trata da preparação de um trabalho de pesquisa.

Um trabalho de pesquisa exige:

- definição de objetivo;
- revisão bibliográfica;
- identificação de lacuna;
- metodologia;
- resultados esperados;
- discussão de limitações.

### 19.1 Problema científico versus problema técnico

Na área aplicada, é necessário argumentar que o problema é científico e não apenas técnico.

Um problema técnico pode ser resolvido com manual, configuração ou aplicação direta de uma solução já conhecida.

Um problema científico exige investigação, pois ainda há algo não resolvido, não explicado ou não testado adequadamente.

A revisão bibliográfica é essencial para confirmar se o problema já foi resolvido ou se ainda existe uma lacuna.

### 19.2 Objetivo da pesquisa

O objetivo responde ao “para quê” do trabalho.

Exemplo dado pelo professor:

> Convencer a gerência a pagar tokens de LLM mostrando ganhos de qualidade no desenvolvimento de software.

Nesse caso, o objetivo não é apenas “usar LLM”, mas demonstrar algo que justifique uma decisão.

### 19.3 Objetivo versus hipótese

O objetivo é aquilo que a pesquisa pretende alcançar.

A hipótese é uma afirmação que será testada.

Exemplo:

- Objetivo: avaliar se o uso de LLM melhora a qualidade do código.
- Hipótese: o uso de LLM aumenta a qualidade do código produzido pelos desenvolvedores.

### 19.4 Justificativa

A justificativa explica por que vale a pena realizar a pesquisa.

Ela deve responder:

- por que o problema é relevante;
- quem se beneficia;
- qual lacuna será endereçada;
- por que a questão merece investigação;
- quais impactos podem surgir.

### 19.5 Método

O método detalha como a pesquisa será realizada.

Exemplo:

- dividir participantes em dois grupos;
- um grupo usa LLM;
- outro grupo não usa;
- ambos desenvolvem o mesmo projeto;
- critérios de qualidade são definidos;
- resultados são medidos e comparados.

O método precisa permitir que outra pessoa entenda como a conclusão foi obtida.

### 19.6 Resultados esperados

Resultados esperados indicam o que se pretende gerar, como:

- protocolo;
- ferramenta;
- dataset;
- análise;
- guia;
- modelo;
- relatório;
- evidências.

Os resultados esperados não precisam depender da hipótese ser confirmada. Mesmo uma hipótese refutada pode produzir resultado científico relevante.

---

## 20. Revisão bibliográfica, literatura cinza e leitura crítica

### 20.1 Background e trabalhos relacionados

A revisão bibliográfica pode ser dividida em duas partes:

1. **Background**  
   Apresenta conceitos fundamentais necessários para entender o trabalho.

2. **Related work** ou trabalhos relacionados  
   Apresenta pesquisas próximas, comparando o que já foi feito com o que o trabalho pretende fazer.

### 20.2 Objetivo da revisão

A revisão bibliográfica serve para:

- compreender o estado da arte;
- identificar lacunas;
- saber o que já foi feito;
- evitar duplicação;
- refinar o objetivo;
- encontrar métodos possíveis;
- construir uma teia de conexões entre trabalhos.

### 20.3 Knuth e a descrição do problema

O professor menciona uma orientação atribuída a Knuth sobre a descrição do problema em três partes:

1. enunciado preciso do problema;
2. explicação do problema;
3. referência mostrando que o problema ainda não foi tratado.

Essa estrutura ajuda a mostrar que o problema é real e que há espaço para contribuição.

### 20.4 “To the best of our knowledge”

O professor menciona a expressão:

> *To the best of our knowledge*

Ela é usada para indicar que, até onde os autores conseguiram verificar na literatura, não há trabalho igual ou solução equivalente.

Não é uma afirmação absoluta de inexistência; é uma afirmação baseada na revisão feita.

### 20.5 Ciclo de definição da pesquisa

O processo de definição da pesquisa envolve:

1. escolher um tema;
2. fazer uma revisão exploratória;
3. selecionar artigos relevantes;
4. refinar o objetivo;
5. identificar lacunas;
6. formular a metodologia;
7. definir resultados esperados.

Exemplo de tema mencionado:

> LGPD em ambientes acadêmicos.

A partir do tema, a revisão ajuda a transformar uma ideia ampla em um problema de pesquisa mais preciso.

### 20.6 Revisão sistemática e literatura cinza

O professor reforça a importância da revisão sistemática e da literatura cinza.

A literatura cinza inclui materiais que não necessariamente passaram por revisão por pares, como:

- blogs técnicos;
- white papers;
- documentação industrial;
- podcasts;
- documentos de associações;
- materiais de empresas;
- relatórios técnicos.

Ela é especialmente importante em temas que nascem primeiro na indústria e só depois chegam à academia.

Exemplos citados:

- Cloud;
- microsserviços;
- LLMs.

A literatura cinza não deve ser ignorada, mas precisa ser lida com criticidade.

### 20.7 Como ler um artigo científico

Ao ler um artigo, o professor orienta observar:

- de onde o autor tirou a ideia;
- qual problema ele está tentando resolver;
- como ele resolveu;
- qual foi o resultado;
- em que medida a solução funcionou;
- quais limitações foram reconhecidas;
- quais trabalhos futuros foram indicados.

A seção de **conclusões e trabalhos futuros** costuma dar boas dicas de próximos passos de pesquisa.

### 20.8 Teia de conexões

O pesquisador deve montar uma “teia de conexões” entre trabalhos relacionados.

Isso permite entender:

- quem conversa com quem;
- quais métodos aparecem;
- quais lacunas permanecem;
- quais soluções são parecidas;
- quais abordagens se repetem;
- onde ainda há espaço para contribuição.

O professor menciona que outras áreas podem agregar valor a um tema. Um exemplo citado é usar LLM para automatizar a rotulagem de código.

---

## 21. Hipótese, resultados esperados, limitações e ameaças à validade

### 21.1 Hipótese como elemento científico

A hipótese diferencia um trabalho científico de um trabalho apenas técnico.

Ela deve ser uma afirmação ou questão que possa ser investigada, confirmada ou refutada.

Uma hipótese científica precisa poder ser demonstrada como falsa ou verdadeira dentro do desenho de pesquisa.

### 21.2 Resultados esperados

Os resultados esperados indicam o que será produzido pela pesquisa.

Podem incluir:

- protocolos;
- datasets;
- ferramentas;
- modelos;
- métodos;
- análises;
- relatórios;
- recomendações.

Eles podem ir além do objetivo central.

Mesmo se a hipótese não for confirmada, o trabalho pode gerar conhecimento relevante, desde que o método seja adequado e as evidências sejam bem analisadas.

### 21.3 Limitações do trabalho

As limitações devem ser reconhecidas explicitamente.

Exemplo citado:

> O trabalho usou apenas Java.

Isso significa que os resultados podem não valer para outras linguagens sem novos experimentos.

Limitações não invalidam automaticamente o trabalho. Elas delimitam seu alcance.

### 21.4 Ameaças à validade

As ameaças à validade são circunstâncias que podem enfraquecer os resultados.

O professor menciona ameaças como:

- internas;
- externas;
- de construção.

A pesquisa deve relatar como tentou mitigar esses riscos.

Exemplo:

- usar coleta automática para evitar erro humano.

Ameaças à validade não são uma confissão de fracasso. Elas mostram maturidade metodológica e transparência científica.

---

## 22. Ideias centrais da aula

A aula enfatiza que a computação amadureceu como ciência e hoje possui métodos mais próprios para lidar com seus objetos de estudo.

Os pontos centrais foram:

- a computação combina teoria e prática, mas nem sempre soube explicar bem seus fenômenos;
- a maturidade da área permite discutir método científico com mais precisão;
- o empirismo, o positivismo, o pragmatismo e a objetividade influenciam a ciência;
- a ciência não trabalha com dogmas;
- teorias científicas são abertas à refutação;
- a objetividade depende de reprodutibilidade e transparência;
- a ciência aberta busca disponibilizar dados e métodos;
- uma hipótese precisa ser testável;
- uma tese é uma explicação fundamentada;
- uma teoria é uma explicação corroborada ao longo do tempo;
- trabalhos de pesquisa podem ser originais ou revisões;
- pesquisas podem ser exploratórias, descritivas ou explicativas;
- procedimentos incluem pesquisa bibliográfica, documental, experimental e pesquisa-ação;
- ciência busca explicar; tecnologia busca transformar;
- na computação, há pesquisas baseadas em produtos, estudos de caso, benchmarks, provas formais e experimentos empíricos;
- o problema de pesquisa precisa ser científico, não apenas técnico;
- revisão bibliográfica e literatura cinza ajudam a encontrar lacunas;
- limitações e ameaças à validade devem ser reconhecidas.

---

## Conclusão

A segunda aula aprofunda a relação entre **método científico e computação**, mostrando que a pesquisa em computação pode assumir diferentes formatos: formal, empírica, exploratória, aplicada, teórica ou baseada em estudos de caso.

O professor reforça que uma boa pesquisa depende de clareza sobre:

- qual problema está sendo investigado;
- qual hipótese está sendo testada;
- quais evidências serão coletadas;
- qual método será usado;
- quais limites o trabalho possui;
- como os resultados podem ser reproduzidos ou avaliados por outros pesquisadores.

A aula também destaca que a ciência é uma prática humana, portanto sujeita a vieses, mas possui mecanismos para reduzir esses vieses: método, objetividade, revisão por pares, reprodutibilidade, abertura de dados e explicitação das ameaças à validade.
