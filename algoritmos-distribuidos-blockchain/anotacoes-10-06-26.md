# Aula 03 - Introdução à Tecnologia Blockchain

> **Disciplina:** Algoritmos Distribuídos e Blockchain  
> **Tema da aula:** Introdução à Tecnologia Blockchain  
> **Data:** 10/06  
> **Professor:** Leonardo Oliveira Moreira - LSBD/UFC  

---


## Sumário da aula

1. [Objetivo da aula](#objetivo-da-aula)
2. [Conexão com sistemas distribuídos](#conexão-com-sistemas-distribuídos)
3. [Conceitos introdutórios](#conceitos-introdutórios)
4. [O problema da confiança](#o-problema-da-confiança)
5. [Estrutura interna de um bloco](#estrutura-interna-de-um-bloco)
6. [Funções hash](#funções-hash)
7. [Árvore de Merkle](#árvore-de-merkle)
8. [Consenso em blockchain](#consenso-em-blockchain)
9. [Contratos inteligentes](#contratos-inteligentes)
10. [Aplicações distribuídas com blockchain](#aplicações-distribuídas-com-blockchain)
11. [Quando utilizar blockchain](#quando-utilizar-blockchain)
12. [Limitações e desafios](#limitações-e-desafios)
13. [Pontos enfatizados pelo professor](#pontos-enfatizados-pelo-professor)
14. [Referências dos slides](#referências-dos-slides)
15. [Transcrição integral da aula](#transcrição-integral-da-aula)

---

## Objetivo da aula

A aula foi apresentada como uma introdução à tecnologia blockchain, mas **não com foco exclusivo em criptomoedas**. O professor deixou claro que o objetivo principal da disciplina é entender blockchain como tecnologia para o desenvolvimento de **aplicações distribuídas**.

A ideia central da aula foi mostrar como uma blockchain consegue armazenar informações de maneira distribuída, manter registros auditáveis e permitir que computadores cooperem mesmo quando não existe uma autoridade central confiável.

O professor explicou que, para compreender blockchain, é necessário retomar alguns problemas clássicos de sistemas distribuídos, especialmente:

- consenso;
- sincronização;
- tolerância a falhas;
- replicação;
- comunicação entre nós;
- confiança em ambientes distribuídos.

Segundo a explicação da aula, blockchain pode ser entendida como uma aplicação prática desses conceitos, principalmente por combinar **consenso** e **criptografia** para permitir o funcionamento de uma rede distribuída em que os participantes não precisam confiar previamente uns nos outros.

---

## Conexão com sistemas distribuídos

O professor iniciou a aula relacionando blockchain com os temas vistos nas aulas anteriores de algoritmos distribuídos. A blockchain foi apresentada como uma tecnologia que depende diretamente de conceitos como consenso, sincronização e tolerância a falhas.

Em sistemas distribuídos tradicionais, é comum existir a preocupação com a consistência dos dados, a coordenação entre processos e a recuperação diante de falhas. Na blockchain, esses problemas continuam existindo, mas aparecem em um cenário em que:

- os dados são replicados entre vários participantes;
- não existe necessariamente um coordenador central;
- cada nó precisa manter uma cópia consistente do histórico;
- qualquer novo registro precisa ser validado pela rede;
- os participantes podem não conhecer ou não confiar uns nos outros.

Por isso, a blockchain foi apresentada como uma estrutura distribuída que precisa resolver um problema fundamental: **como manter um registro confiável sem depender de uma autoridade central?**

---

## Conceitos introdutórios

Os slides apresentaram um conjunto de conceitos básicos que servem como base para entender blockchain:

| Conceito | Ideia principal |
|---|---|
| Rede distribuída | Vários computadores participam da rede e mantêm cópias dos dados. |
| Nó | Computador participante da rede blockchain. |
| Bloco | Unidade de armazenamento que agrupa registros ou transações. |
| Cadeia de blocos | Sequência de blocos conectados por hashes. |
| Hash | Impressão digital criptográfica dos dados. |
| Transação | Operação registrada na blockchain. |
| Consenso | Mecanismo para que os participantes concordem sobre o estado da rede. |
| Validação | Verificação das regras antes de aceitar uma transação. |
| Imutabilidade | Registros não devem ser alterados após gravados. |
| Contrato inteligente | Programa armazenado na blockchain que executa regras automaticamente. |
| Descentralização | Não há um único responsável por controlar os dados. |

---

### Blockchain

Blockchain é uma forma de armazenar informações de maneira distribuída. Os dados são organizados em blocos ligados entre si, formando uma cadeia. Cada participante da rede mantém uma cópia dos dados, e não existe um único servidor central controlando tudo.

A ideia principal é que a blockchain funciona como um **livro de registros compartilhado** entre vários computadores. Esse livro registra tudo que aconteceu na rede, e os participantes utilizam mecanismos de consenso e validação para decidir se novas informações podem ser adicionadas.

O professor reforçou que, quando um dado entra na blockchain, ele não deve ser apagado nem alterado. A rede funciona de forma **append-only**, ou seja, novas informações são acrescentadas ao histórico, mas registros anteriores permanecem preservados.

---

### Rede distribuída

Na comparação feita em aula, em sistemas tradicionais normalmente existe um servidor central. Na blockchain, vários computadores participam da rede. Cada computador mantém uma cópia dos dados, e a falha de um participante não interrompe necessariamente o funcionamento do sistema.

O professor relacionou isso à tolerância a falhas: se um nó falhar, os demais continuam existindo e mantendo o histórico. Quando o nó volta, ele precisa passar por um processo de sincronização para atualizar sua cópia da blockchain.

---

### Nó

Um nó é um computador participante da rede blockchain. Ele:

- armazena uma cópia da blockchain;
- recebe informações de outros nós;
- envia informações para outros nós;
- participa da validação;
- ajuda a manter a rede funcionando.

Como a rede é do tipo **peer-to-peer (P2P)**, os nós se comunicam diretamente entre si. O professor comentou que, quando um novo nó entra na rede, não basta copiar os dados: também é necessário negociar as conexões e sincronizar o estado atual da blockchain.

---

### Ledger

O professor usou o termo **ledger** para se referir ao livro-razão da blockchain. Esse ledger contém o histórico das transações e registros da rede.

A explicação dada foi que cada nó mantém uma cópia desse livro. Assim, a blockchain possui **replicação total**: todos os nós devem ter a mesma visão do histórico.

Em termos de aplicação, o ledger pode ser visto como uma estrutura de armazenamento distribuído. Ele pode guardar ativos digitais, registros, documentos, eventos ou outras informações que a aplicação precise manter com propriedades como integridade, auditabilidade e imutabilidade.

---

### Bloco

Um bloco é um conjunto de registros ou transações. Antes de serem armazenadas, as transações são agrupadas em blocos.

Cada bloco contém:

- dados ou transações;
- hash do bloco anterior;
- informações de data e hora;
- raiz de Merkle;
- nonce e outras informações de controle.

O professor comparou o bloco a uma página de um livro de registros digitais. Cada página guarda informações e referencia a página anterior.

---

### Cadeia de blocos

Os blocos são conectados formando uma cadeia. Cada bloco aponta para o bloco anterior por meio do hash desse bloco anterior.

Uma representação simplificada é:

```text
Bloco 1 <- hash(Bloco 1) <- Bloco 2 <- hash(Bloco 2) <- Bloco 3 <- ...
```

Essa ligação é importante porque, se alguém alterar o conteúdo de um bloco, o hash daquele bloco muda. Se o hash muda, o bloco seguinte deixa de apontar corretamente para ele. Dessa forma, a cadeia é quebrada e a alteração indevida pode ser detectada.

---

### Hash

Hash foi explicado como uma espécie de **impressão digital dos dados**. Uma função hash recebe uma entrada e produz um código de tamanho fixo.

Características destacadas:

- entradas diferentes produzem hashes diferentes;
- pequenas alterações geram hashes completamente diferentes;
- é fácil calcular o hash;
- é extremamente difícil reconstruir os dados originais a partir do hash;
- o hash identifica os dados, mas não revela o conteúdo.

O professor usou o exemplo de `Blockchain` com B maiúsculo e `blockchain` com b minúsculo. Mesmo sendo uma diferença pequena, o hash produzido é completamente diferente.

---

### Imutabilidade

Imutabilidade significa que os registros não devem ser alterados depois de gravados.

Na fala do professor, se alguém ganhou uma moeda e depois gastou essa moeda, o registro de que ela foi recebida não desaparece. O que ocorre é a criação de uma nova transação indicando que aquela moeda foi gasta.

Assim, a blockchain não funciona como um banco de dados tradicional em que se faz `UPDATE` ou `DELETE` nos registros antigos. A ideia é sempre acrescentar novos registros, preservando o histórico.

---

### Transação

Uma transação representa uma operação registrada na blockchain. Pode ser:

- transferência de valores;
- registro de documento;
- venda de um ativo digital;
- execução de um contrato inteligente;
- registro de um evento;
- criação ou transferência de um ativo.

Depois de validadas, as transações passam a fazer parte permanente do histórico da blockchain.

---

### Consenso

Consenso é o mecanismo usado para que os participantes concordem sobre o estado da blockchain.

Na aula, o professor destacou que consenso não é apenas concordar por concordar. Na blockchain, o consenso também envolve verificar se a informação é válida, se as regras foram atendidas e se aquele novo bloco ou transação pode entrar na rede.

A pergunta central é:

> Como vários computadores conseguem concordar sem existir um chefe central?

A resposta envolve algoritmos de consenso, validação, contratos inteligentes e mecanismos criptográficos.

---

### Validação

Antes de serem registradas, as transações são verificadas. A rede verifica se as regras definidas pelo sistema foram atendidas.

O professor relacionou essa etapa aos contratos inteligentes. Em muitas blockchains, as regras de validação estão implementadas em programas que executam automaticamente.

Apenas informações que passam por esse processo de validação podem ser aceitas pela rede.

---

### Contratos inteligentes

Contratos inteligentes, ou **smart contracts**, são programas armazenados na blockchain. Eles executam regras de negócio de forma automática.

Exemplo simples apresentado nos slides:

```text
Se o pagamento for confirmado,
então liberar o acesso ao serviço.
```

O professor explicou que os contratos inteligentes podem conter variáveis, funções, procedimentos, estruturas de controle e regras de validação. Eles automatizam processos e reduzem a necessidade de intermediários.

Um ponto importante da aula foi a discussão sobre segurança: a blockchain pode garantir a integridade do que foi registrado, mas a qualidade da validação depende de contratos bem projetados. Se um contrato for mal escrito ou incompleto, um dado falso pode passar pela validação e ser registrado.

---

### Descentralização

Descentralização significa que não existe um único responsável pelos dados. O controle é compartilhado entre os participantes da rede.

Isso reduz pontos únicos de falha e aumenta a disponibilidade do sistema. Se um participante falha, a rede pode continuar funcionando, desde que existam outros nós mantendo a cópia do ledger e participando do consenso.

---

## Definições de blockchain apresentadas nos slides

Os slides trouxeram quatro definições principais de blockchain.

### Definição 1 - Bashir (2017)

Blockchain, em seu núcleo, é um **ledger peer-to-peer distribuído**, criptograficamente seguro, apenas anexado, imutável e atualizável apenas via consenso ou acordo entre os participantes.

Essa definição destaca:

- rede peer-to-peer;
- segurança criptográfica;
- característica append-only;
- imutabilidade;
- atualização por consenso.

---

### Definição 2 - Laurence (2017)

Blockchain é uma estrutura de dados que permite criar um livro-razão digital de dados e compartilhá-lo entre uma rede de partes independentes.

Essa definição enfatiza blockchain como:

- estrutura de dados;
- livro-razão digital;
- compartilhamento entre peers independentes.

---

### Definição 3 - Moreira (2019)

Blockchain é basicamente um banco de dados distribuído, compartilhado e imutável, que armazena registros de transações ou eventos digitais em uma rede P2P.

Essa definição aproxima blockchain de um banco de dados distribuído, mas com propriedades específicas de imutabilidade e compartilhamento.

---

### Definição 4 - Blockchain Expert (2026)

Blockchain é um livro-razão compartilhado e distribuído que facilita o processo de registro de transações e rastreamento de ativos em uma rede.

Essa definição amplia a visão de blockchain para o rastreamento de ativos, que podem ser moedas, documentos, joias, imóveis, diplomas, prontuários, produtos ou outros bens físicos e digitais.

---

## O problema da confiança

O problema da confiança foi um dos pontos mais importantes da aula.

Muitas aplicações precisam compartilhar informações entre várias pessoas ou organizações. Porém, nem sempre existe uma autoridade central confiável para controlar os dados. Além disso, os participantes podem não se conhecer ou não confiar uns nos outros.

A pergunta central é:

> Como manter um registro confiável sem depender de uma autoridade central?

---

### Solução tradicional

A solução tradicional é usar uma autoridade central. Exemplos:

- bancos;
- cartórios;
- plataformas digitais;
- órgãos governamentais.

Nesse modelo, uma entidade controla os registros e decide o que é válido. A limitação é que todos precisam confiar nessa única entidade. Além disso, essa entidade pode se tornar um gargalo ou ponto único de falha.

---

### Ideia da blockchain

A blockchain propõe outra abordagem:

- os registros são compartilhados por toda a rede;
- não existe um único controlador dos dados;
- os participantes verificam coletivamente as informações;
- os dados são protegidos por hashes;
- a entrada de novos blocos depende de validação e consenso.

O objetivo é construir confiança por meio da tecnologia, e não apenas por meio de uma autoridade central.

---

## Estrutura interna de um bloco

Um bloco é dividido em duas partes principais:

1. cabeçalho;
2. lista de transações.

### Cabeçalho do bloco

O cabeçalho contém informações que identificam o bloco e permitem conectá-lo ao restante da blockchain.

Elementos citados:

- hash do bloco anterior;
- data e hora;
- raiz de Merkle;
- nonce;
- outras informações de controle.

O cabeçalho funciona como a identidade do bloco.

---

### Hash do bloco anterior

Cada bloco guarda o hash do bloco anterior. Esse hash funciona como uma referência única.

Se o conteúdo do bloco anterior for alterado, seu hash muda. Com isso, o bloco seguinte deixa de apontar corretamente para ele. Essa quebra de ligação indica que houve violação da integridade.

---

### Transações do bloco

A principal função de um bloco é armazenar transações. Essas transações podem representar:

- transferências de valores;
- registros de documentos;
- execução de contratos inteligentes;
- eventos de uma aplicação;
- criação ou transferência de ativos digitais.

O professor reforçou que ativo digital pode representar várias coisas, não apenas moedas.

---

## Funções hash

Uma função hash é um algoritmo matemático que recebe uma entrada e produz um código de tamanho fixo.

Exemplo conceitual:

```text
Entrada: Blockchain
Hash: A94F...7C2D
```

O hash é usado para:

- identificar dados;
- verificar integridade;
- conectar blocos;
- detectar alterações indevidas;
- contribuir para a segurança e confiabilidade da rede.

Na blockchain, cada bloco possui seu próprio hash, e o bloco seguinte armazena o hash do bloco anterior.

---

## Árvore de Merkle

A Árvore de Merkle foi apresentada como uma forma de resumir muitas transações usando um único hash final, chamado **Merkle Root**.

Um bloco pode conter muitas transações. Verificar cada transação individualmente seria custoso. A Árvore de Merkle resolve esse problema criando uma estrutura em que:

1. calcula-se o hash de cada transação;
2. combina-se os hashes em pares;
3. repete-se o processo nos níveis superiores;
4. chega-se a um único hash final: a raiz de Merkle.

Representação simplificada:

```text
                         Merkle Root
                    Hash(H12 + H34)
                    /                             H12                  H34
          Hash(H1+H2)          Hash(H3+H4)
           /       \            /              H1          H2        H3          H4
    Hash(T1)   Hash(T2)  Hash(T3)   Hash(T4)
       |          |         |          |
      T1         T2        T3         T4
```

Se qualquer transação mudar, seu hash muda. Como os hashes são combinados em níveis, a alteração se propaga até a raiz. Assim, o Merkle Root final também muda.

A grande vantagem é que a rede consegue verificar rapidamente se as transações de um bloco continuam íntegras, sem comparar todas individualmente.

---

## Consenso em blockchain

Em uma blockchain, vários participantes mantêm uma cópia dos dados. Antes de adicionar um novo bloco, os participantes precisam concordar sobre sua validade.

O consenso é necessário porque:

- participantes podem ter versões diferentes dos dados;
- mensagens podem atrasar;
- mensagens podem se perder;
- nós podem falhar;
- alguns participantes podem agir de forma maliciosa ou incorreta.

A pergunta prática é:

> Se dois participantes apresentam versões diferentes dos dados, qual delas deve ser aceita?

Os algoritmos de consenso ajudam a responder essa pergunta.

---

### Algoritmos de consenso apresentados

| Algoritmo | Como a decisão é tomada | Observação |
|---|---|---|
| Proof of Work (PoW) | Competição computacional para resolver um desafio matemático. | Usado pelo Bitcoin; consome muita energia. |
| Proof of Stake (PoS) | Participação baseada na quantidade de ativos mantidos em garantia. | Consome menos energia que PoW. |
| PBFT | Votação entre participantes da rede. | Muito usado em blockchains permissionadas. |
| Raft | Um líder eleito temporariamente propõe decisões; os demais confirmam. | Algoritmo de consenso distribuído não criado originalmente para blockchain. |

---

### Proof of Work (PoW)

No Proof of Work, os participantes competem para resolver um problema matemático. No contexto do Bitcoin, os mineradores realizam muitas tentativas até encontrar um hash que satisfaça as regras da rede.

O primeiro que encontra a solução propõe o próximo bloco. Os demais participantes verificam se a solução está correta.

O professor comparou isso a um concurso de matemática: quem resolve primeiro ganha o direito de registrar a próxima página do livro.

A principal desvantagem é o alto consumo computacional e energético.

---

### Proof of Stake (PoS)

No Proof of Stake, os validadores são escolhidos com base na quantidade de moedas ou ativos que possuem em garantia.

A ideia é que quem tem maior participação no sistema tem maior chance de ser escolhido para validar o próximo bloco.

A vantagem é consumir muito menos energia do que o Proof of Work, pois não exige resolver problemas matemáticos complexos.

---

### PBFT

O PBFT, ou Practical Byzantine Fault Tolerance, trabalha com troca de mensagens e votação entre participantes.

Ele é muito utilizado em blockchains permissionadas, nas quais os participantes passam por algum controle de acesso ou autenticação. Como a confiança inicial é maior do que em redes totalmente públicas, a concordância pode depender de maioria necessária, e não necessariamente de todos.

---

### Raft

O Raft é um algoritmo de consenso distribuído. Ele não foi criado especificamente para blockchain, mas pode ser usado nesse contexto.

No Raft, um líder é eleito temporariamente. Esse líder coordena as atualizações, enquanto os demais participantes confirmam as decisões.

---

## Contratos inteligentes

Um smart contract é um programa executado na blockchain. Ele implementa regras e condições previamente definidas.

Fluxo simplificado:

```text
Condição -> Execução -> Registro
```

Exemplo:

```text
Pagamento realizado
       ↓
Smart Contract verifica a condição
       ↓
Acesso liberado automaticamente
```

Vantagens citadas:

- automatizam processos;
- reduzem intermediários;
- executam regras de forma previsível;
- aumentam a transparência;
- registram os resultados na blockchain;
- permitem a criação de aplicações descentralizadas.

O professor também explicou que contratos inteligentes são fundamentais para validar se uma informação pode ou não entrar na rede. Por isso, a segurança da aplicação depende muito da qualidade dessas regras.

---

## Aplicações distribuídas com blockchain

Uma aplicação distribuída com blockchain é uma aplicação executada por vários computadores conectados em rede, em que os dados são armazenados e compartilhados pela blockchain.

O fluxo apresentado nos slides foi:

```text
Usuário
  ↓
Aplicação
  ↓
Smart Contract
  ↓
Blockchain
```

Funcionamento geral:

1. o usuário realiza uma operação;
2. a aplicação envia uma solicitação para a blockchain;
3. o smart contract verifica as regras;
4. a rede valida a operação;
5. o resultado é registrado na blockchain.

---

### Exemplo: sistema de votação

No exemplo de votação:

- o eleitor acessa a aplicação;
- o voto é enviado para um smart contract;
- a blockchain registra o voto;
- os registros permanecem auditáveis;
- o resultado pode ser verificado por participantes autorizados.

O benefício seria maior transparência e rastreabilidade no processo.

---

### Outros exemplos comentados

Durante a aula, o professor citou diferentes usos possíveis para blockchain:

- criptomoedas, como Bitcoin;
- NFTs e ativos digitais;
- registro de documentos;
- cartórios digitais;
- diplomas acadêmicos;
- prontuários hospitalares;
- rastreamento de produtos;
- votação;
- aplicações clínicas;
- compartilhamento de dados entre organizações.

Um exemplo comentado com mais detalhes foi o de uma aplicação de análise clínica em que uma blockchain privada, baseada em Ethereum, era usada como mecanismo de armazenamento para dados que precisavam de propriedades como:

- imutabilidade;
- auditabilidade;
- irrefutabilidade;
- integridade.

Nessa arquitetura, dados que precisavam ser modificados podiam ficar em um banco relacional, enquanto dados que exigiam preservação e rastreabilidade eram enviados para a blockchain.

---

## Quando utilizar blockchain

A blockchain não deve ser usada em qualquer aplicação. O professor reforçou que é preciso avaliar se os benefícios justificam os custos e as limitações.

A tecnologia faz mais sentido quando:

- várias organizações precisam compartilhar informações;
- é importante manter histórico auditável;
- deseja-se reduzir dependência de uma autoridade central;
- transparência é requisito importante;
- rastreabilidade é requisito importante;
- integridade dos dados é essencial;
- os participantes precisam verificar informações registradas.

A frase central dos slides foi:

> Nem toda aplicação precisa de blockchain.

---

## Limitações e desafios

A blockchain oferece transparência, rastreabilidade e descentralização, mas possui limitações.

### Desempenho

Antes de registrar informações, a rede precisa validar os dados. O processo de consenso pode levar tempo. Em algumas blockchains, poucas transações são processadas por segundo.

Isso pode ser um problema para aplicações que exigem respostas imediatas ou tempo real.

---

### Crescimento dos dados

Novos blocos são adicionados continuamente. Como o histórico normalmente é preservado, a blockchain pode ocupar muito espaço de armazenamento com o passar do tempo.

Além disso, como existe replicação total, os participantes precisam armazenar uma quantidade crescente de informações.

---

### Consumo de recursos

Alguns mecanismos de consenso exigem muitos recursos computacionais. O Proof of Work foi citado como exemplo de algoritmo com alto consumo computacional e energético.

A rede também consome recursos de comunicação, pois precisa trocar mensagens de sincronização, validação e consenso.

---

### Complexidade de desenvolvimento

Desenvolver aplicações em blockchain pode ser mais complexo do que desenvolver aplicações tradicionais.

É necessário considerar:

- configuração da rede;
- escolha do algoritmo de consenso;
- criação de contratos inteligentes;
- comunicação entre aplicação e blockchain;
- segurança das regras;
- testes;
- manutenção;
- dificuldade de corrigir erros depois da implantação.

O professor comentou que montar uma rede blockchain privada pode ser trabalhoso mesmo com documentação, e que erros em smart contracts podem ser difíceis de corrigir depois que a aplicação já está implantada.

---

## Pontos enfatizados pelo professor

### 1. Blockchain não é apenas criptomoeda

O professor destacou que criptomoeda é apenas uma aplicação particular da tecnologia blockchain. O foco da aula foi entender blockchain como tecnologia para aplicações distribuídas.

---

### 2. A chave da blockchain é combinar consenso e criptografia

A rede consegue detectar alterações indevidas e validar novas entradas usando mecanismos criptográficos, funções hash, árvore de Merkle, contratos inteligentes e algoritmos de consenso.

---

### 3. O ledger é replicado entre os nós

Cada nó mantém uma cópia do livro-razão. Essa replicação total ajuda na disponibilidade e na tolerância a falhas, mas também aumenta custos de armazenamento e comunicação.

---

### 4. Imutabilidade não significa que nada novo acontece

A blockchain não altera registros antigos. Quando algo muda no mundo da aplicação, cria-se uma nova transação ou um novo bloco registrando o novo evento.

---

### 5. A blockchain preserva o que foi registrado, mas não garante sozinha que o dado de origem é verdadeiro

Na discussão em sala, foi levantada a dúvida sobre o envio de um dado falso que ainda assim satisfaça o contrato. O professor explicou que a segurança da entrada depende da qualidade do contrato inteligente e das regras de validação.

Ou seja, a blockchain ajuda a garantir integridade, rastreabilidade e imutabilidade do que foi registrado, mas a validação do significado do dado depende da aplicação.

---

### 6. A Árvore de Merkle acelera a verificação de integridade

Em vez de comparar todas as transações uma por uma, a rede pode verificar o Merkle Root. Se qualquer transação for alterada, o hash final muda.

---

### 7. O consenso tem custo

O consenso é essencial para manter uma visão única dos dados, mas traz custos de desempenho, mensagens e processamento.

---

### 8. O tipo de blockchain importa

O professor diferenciou blockchains públicas e privadas/permissionadas. Em redes públicas, como Bitcoin, o nível de desconfiança é maior. Em redes permissionadas, existe controle de acesso, e isso influencia a escolha dos mecanismos de consenso.

---

### 9. Smart contracts são regras de negócio executadas automaticamente

O professor resumiu smart contracts como regras de negócio automatizadas dentro da blockchain.

---

### 10. Blockchain deve ser usada quando os benefícios compensam as limitações

A conclusão da aula foi que blockchain é poderosa, mas não resolve todos os problemas. Deve ser usada quando transparência, rastreabilidade, confiança distribuída e auditabilidade forem realmente necessárias.

---

## Referências dos slides

As referências bibliográficas apresentadas nos slides foram:

- BASHIR, I. *Mastering Blockchain*. Packt Publishing, 2017.
- BLOCKCHAIN EXPERT. *Blockchain Book*. 2026.
- LAURENCE, T. *Blockchain for Dummies*. John Wiley & Sons, 2017.
- MOREIRA, K. B. *Blockchain: tecnologia, arquitetura e aplicações*. Monografia de Graduação. Universidade de Brasília, 2019.

---
