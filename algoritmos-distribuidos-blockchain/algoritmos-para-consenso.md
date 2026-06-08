# Algoritmos Distribuídos para Consenso

> Anotações de estudo para a disciplina **Algoritmos Distribuídos e Blockchain**.  


---

## Sumário

1. [Ideia central](#1-ideia-central)
2. [Por que consenso é necessário?](#2-por-que-consenso-é-necessário)
3. [O que é o problema do consenso?](#3-o-que-é-o-problema-do-consenso)
4. [Modelos de sistema e tipos de falha](#4-modelos-de-sistema-e-tipos-de-falha)
5. [Propriedades do consenso](#5-propriedades-do-consenso)
6. [Quórum e maioria](#6-quórum-e-maioria)
7. [O problema FLP](#7-o-problema-flp)
8. [Consenso, eleição, commit e replicação: não confundir](#8-consenso-eleição-commit-e-replicação-não-confundir)
9. [Paxos](#9-paxos)
10. [Multi-Paxos](#10-multi-paxos)
11. [Raft](#11-raft)
12. [PBFT e consenso bizantino](#12-pbft-e-consenso-bizantino)
13. [Consenso em Blockchain](#13-consenso-em-blockchain)
14. [HyperPaxos e a ideia de hierarquia](#14-hyperpaxos-e-a-ideia-de-hierarquia)
15. [Comparação entre algoritmos](#15-comparação-entre-algoritmos)
16. [Exemplo completo: consenso para uma transação bancária](#16-exemplo-completo-consenso-para-uma-transação-bancária)
17. [Análise de segurança](#17-análise-de-segurança)
18. [Sugestão de implementação para trabalho prático](#18-sugestão-de-implementação-para-trabalho-prático)
19. [Resumo para revisar antes da aula](#19-resumo-para-revisar-antes-da-aula)
20. [Glossário](#20-glossário)
21. [Referências](#21-referências)

---

## 1. Ideia central

Em sistemas distribuídos, não existe uma única máquina responsável por tudo. O sistema é formado por vários **nós** — servidores, processos, máquinas virtuais, containers ou dispositivos — que se comunicam pela rede.

O problema é que a rede não é perfeita:

- mensagens podem atrasar;
- mensagens podem se perder;
- mensagens podem chegar duplicadas;
- mensagens podem chegar fora de ordem;
- máquinas podem falhar;
- em alguns cenários, máquinas podem agir de forma maliciosa.

Diante disso, surge uma pergunta fundamental:

> **Como vários nós independentes conseguem decidir a mesma coisa?**

Essa pergunta é o coração do **consenso distribuído**.

De forma simples:

> **Consenso é o processo pelo qual vários nós de um sistema distribuído chegam a uma mesma decisão, mesmo diante de falhas, atrasos e incertezas.**

---

## 2. Por que consenso é necessário?

Consenso aparece sempre que um sistema distribuído precisa manter uma visão comum sobre alguma coisa.

### Exemplo intuitivo: grupo decidindo uma resposta

Imagine cinco pessoas trabalhando em grupo. Elas precisam entregar uma única resposta para a pergunta:

> “A transação deve ser confirmada ou cancelada?”

Cada pessoa pode ter recebido informações diferentes. Algumas podem estar sem internet. Outra pode ter recebido a mensagem atrasada. Mesmo assim, o grupo precisa entregar uma resposta única.

Em computação distribuída, os “alunos do grupo” são os servidores/processos, e a “resposta única” é o valor de consenso.

### Exemplos reais de uso

| Situação | O que precisa ser decidido? |
|---|---|
| Banco de dados replicado | Qual operação será aplicada primeiro? |
| Cluster de servidores | Quem é o líder atual? |
| Sistema de arquivos distribuído | Qual versão do arquivo é a correta? |
| Transação distribuída | A transação deve fazer `commit` ou `abort`? |
| Blockchain | Qual bloco será aceito como próximo bloco válido? |
| Kubernetes/etcd | Qual é o estado oficial do cluster? |
| Sistemas bancários | Qual é o saldo correto depois de uma operação? |

Sem consenso, cada nó poderia tomar uma decisão diferente. Isso causaria inconsistência.

Exemplo:

```text
Servidor 1 decide: COMMIT
Servidor 2 decide: COMMIT
Servidor 3 decide: ABORT
```

Esse cenário é perigoso, porque o sistema deixa de ter um estado único.

---

## 3. O que é o problema do consenso?

O problema do consenso pode ser descrito assim:

> Cada processo propõe um valor. Ao final, todos os processos corretos devem decidir o mesmo valor.

Exemplo:

```text
Nó A propõe: COMMIT
Nó B propõe: COMMIT
Nó C propõe: ABORT
```

O algoritmo de consenso precisa garantir que, ao final, os nós corretos decidam um único valor:

```text
Decisão final: COMMIT
```

ou:

```text
Decisão final: ABORT
```

Mas nunca:

```text
Nó A decide COMMIT
Nó B decide ABORT
Nó C decide COMMIT
```

Esse último caso viola o consenso.

---

## 4. Modelos de sistema e tipos de falha

Antes de estudar qualquer algoritmo de consenso, é preciso entender **qual tipo de falha ele tolera**.

Nem todo algoritmo resolve todos os problemas. Alguns algoritmos toleram apenas máquinas que param. Outros toleram máquinas maliciosas. Outros dependem de uma rede parcialmente estável.

### 4.1 Sistema síncrono

Em um sistema síncrono, existe um limite conhecido para o tempo de envio de mensagens e para o tempo de processamento.

Exemplo:

> Toda mensagem enviada deve chegar em até 2 segundos.

Isso facilita muito a vida do algoritmo. Se um nó não respondeu em 2 segundos, o sistema pode suspeitar que ele falhou.

### 4.2 Sistema assíncrono

Em um sistema assíncrono, não existe limite conhecido para atraso.

Uma mensagem pode chegar em:

```text
1 ms
10 ms
5 segundos
2 minutos
```

O grande problema é que não dá para saber se um nó morreu ou se apenas está lento.

Exemplo:

```text
Nó A envia mensagem para Nó B.
Nó B não responde.
```

O que aconteceu?

- Nó B caiu?
- A mensagem se perdeu?
- A rede está lenta?
- A resposta está atrasada?

Em sistemas distribuídos, essa dúvida é central.

### 4.3 Sistema parcialmente síncrono

É um meio-termo. O sistema pode se comportar mal por um tempo, mas assume-se que, em algum momento, a rede volta a ter limites razoáveis de atraso.

Muitos algoritmos práticos funcionam bem nesse modelo.

### 4.4 Falha por parada (*crash fault*)

O nó simplesmente para de funcionar.

Exemplo:

```text
Servidor S3 desligou.
Servidor S3 não envia mais mensagens.
Servidor S3 não responde mais requisições.
```

Algoritmos como **Paxos** e **Raft** normalmente trabalham com esse tipo de falha.

### 4.5 Falha por omissão

O nó está vivo, mas deixa de enviar ou receber algumas mensagens.

Exemplo:

```text
S1 enviou mensagem para S2.
S2 está ligado, mas a mensagem não chegou.
```

Pode acontecer por perda de pacote, timeout, falha temporária de rede ou congestionamento.

### 4.6 Falha bizantina

É a falha mais perigosa.

Um nó bizantino pode:

- mentir;
- mandar mensagens contraditórias;
- fingir que está correto;
- alterar dados;
- colaborar com atacantes;
- enviar uma resposta para um nó e outra resposta diferente para outro nó.

Exemplo:

```text
Nó malicioso M envia para A: "votei em X"
Nó malicioso M envia para B: "votei em Y"
```

Esse tipo de falha é importante em segurança e Blockchain.

---

## 5. Propriedades do consenso

Um algoritmo de consenso precisa satisfazer algumas propriedades fundamentais.

### 5.1 Acordo (*agreement*)

Nenhum par de processos corretos pode decidir valores diferentes.

Exemplo correto:

```text
Nó A decide COMMIT
Nó B decide COMMIT
Nó C decide COMMIT
```

Exemplo incorreto:

```text
Nó A decide COMMIT
Nó B decide ABORT
Nó C decide COMMIT
```

A propriedade de acordo impede que o sistema fique dividido.

### 5.2 Validade (*validity*)

O valor decidido precisa fazer sentido dentro do conjunto de valores propostos.

Exemplo:

```text
Valores propostos: COMMIT, ABORT
Valor decidido: FORMATAR_DISCO
```

Isso violaria a validade, porque `FORMATAR_DISCO` não foi uma proposta legítima.

### 5.3 Terminação (*termination*)

Todo processo correto deve eventualmente decidir algum valor.

Em outras palavras:

> O sistema não deve ficar esperando para sempre.

Na prática, essa propriedade depende muito das condições da rede.

### 5.4 Integridade

Um processo correto decide apenas uma vez.

Exemplo incorreto:

```text
Nó A decide COMMIT.
Depois muda de ideia e decide ABORT.
```

Depois que um nó correto decide, aquela decisão deve ser definitiva.

### 5.5 Safety e liveness

Duas palavras aparecem muito em consenso:

| Conceito | Ideia | Pergunta |
|---|---|---|
| **Safety** | Não fazer algo errado | “O sistema evita decisões conflitantes?” |
| **Liveness** | Continuar progredindo | “O sistema eventualmente decide?” |

Uma frase comum em sistemas distribuídos é:

> **É melhor não decidir do que decidir errado.**

Por isso, muitos algoritmos preservam a propriedade de safety mesmo quando a rede está ruim. Eles podem parar temporariamente, mas não devem confirmar dois valores conflitantes.

---

## 6. Quórum e maioria

Muitos algoritmos de consenso usam a ideia de **quórum**.

Um quórum é um subconjunto de nós suficiente para tomar uma decisão.

O tipo mais comum de quórum é a **maioria**.

### 6.1 Exemplo com 5 nós

Imagine um sistema com 5 servidores:

```text
S1, S2, S3, S4, S5
```

A maioria é 3.

```text
Maioria = 3 de 5
```

Se uma operação foi aceita por 3 servidores, ela foi aceita pela maioria.

### 6.2 Por que maioria é importante?

Porque duas maiorias sempre se cruzam em pelo menos um nó.

Exemplo:

```text
Maioria A: S1, S2, S3
Maioria B: S3, S4, S5
Interseção: S3
```

Esse ponto de interseção ajuda a impedir decisões contraditórias.

### 6.3 Exemplo intuitivo

Imagine uma turma com 5 alunos decidindo uma resposta. Para uma resposta ser oficial, precisa de pelo menos 3 votos.

Se um grupo de 3 já decidiu `COMMIT`, outro grupo de 3 não consegue decidir `ABORT` sem ter pelo menos uma pessoa que participou da decisão anterior.

Essa interseção é o que ajuda o sistema a “lembrar” decisões antigas.

### 6.4 Tolerância a falhas com maioria

Se o sistema tem `N` nós e usa maioria, ele normalmente tolera até:

```text
f = floor((N - 1) / 2)
```

Exemplos:

| Total de nós | Maioria | Falhas toleradas |
|---|---:|---:|
| 3 | 2 | 1 |
| 5 | 3 | 2 |
| 7 | 4 | 3 |

Isso vale para falhas do tipo parada, não para falhas bizantinas.

---

## 7. O problema FLP

O resultado conhecido como **FLP**, de Fischer, Lynch e Paterson, mostra uma limitação fundamental dos sistemas distribuídos.

A ideia é:

> Em um sistema completamente assíncrono, não existe algoritmo determinístico que garanta consenso se até mesmo um único processo puder falhar por parada.

Isso parece muito pessimista, mas é extremamente importante.

### 7.1 Tradução para linguagem simples

Se a rede pode atrasar mensagens por tempo indefinido, o sistema não consegue distinguir perfeitamente:

```text
Nó lento
```

de:

```text
Nó morto
```

Então, se o algoritmo precisa ter certeza absoluta antes de decidir, ele pode ficar esperando para sempre.

### 7.2 Isso significa que consenso é impossível na prática?

Não.

Significa que algoritmos práticos precisam fazer alguma concessão, por exemplo:

- usar timeouts;
- usar detectores de falha;
- assumir algum grau de sincronismo;
- eleger líderes;
- usar aleatoriedade;
- aceitar que o sistema pode parar temporariamente;
- exigir maioria para decidir.

Ou seja, o FLP não impede a existência de Paxos, Raft ou PBFT. Ele mostra quais suposições esses algoritmos precisam fazer para funcionar na prática.

---

## 8. Consenso, eleição, commit e replicação: não confundir

Esses conceitos são relacionados, mas não são exatamente a mesma coisa.

### 8.1 Consenso

É o problema geral de fazer vários nós concordarem sobre um valor.

Exemplo:

```text
Qual comando será aplicado na posição 10 do log?
```

### 8.2 Eleição de líder

É escolher um nó para coordenar o sistema.

Exemplo:

```text
S3 será o líder do cluster.
```

A eleição de líder pode usar consenso ou ser parte de um algoritmo de consenso.

### 8.3 Commit distribuído

É decidir se uma transação distribuída será confirmada ou abortada.

Exemplo:

```text
A transferência bancária será efetivada?
```

Protocolos como **Two-Phase Commit (2PC)** e **Three-Phase Commit (3PC)** tratam de commit distribuído, mas não resolvem todos os problemas de consenso em ambientes com falhas arbitrárias.

### 8.4 Replicação

É manter cópias de dados ou serviços em vários nós.

Exemplo:

```text
O mesmo banco de dados existe em S1, S2 e S3.
```

Para replicação ser segura, geralmente é necessário consenso sobre a ordem das operações.

---

## 9. Paxos

**Paxos** é um dos algoritmos clássicos de consenso distribuído.

Ele foi proposto por Leslie Lamport e é muito importante na teoria e na prática. Ao mesmo tempo, é conhecido por ser difícil de entender em uma primeira leitura.

### 9.1 Ideia principal

Paxos permite que um conjunto de processos escolha um único valor, mesmo que alguns processos falhem.

Ele não assume nós maliciosos. Ou seja, Paxos normalmente trabalha com falhas não bizantinas, como queda, atraso ou perda de mensagem.

### 9.2 Papéis no Paxos

Paxos possui três papéis principais:

| Papel | Função |
|---|---|
| **Proposer** | Propõe um valor. |
| **Acceptor** | Vota/aceita propostas. |
| **Learner** | Aprende qual valor foi escolhido. |

Na prática, o mesmo servidor pode exercer mais de um papel.

### 9.3 Exemplo intuitivo

Imagine que três servidores precisam decidir se uma transação será confirmada:

```text
A1, A2, A3
```

Dois clientes/proposers enviam propostas:

```text
P1 propõe: COMMIT
P2 propõe: ABORT
```

O Paxos precisa garantir que uma maioria não escolha `COMMIT` enquanto outra maioria escolhe `ABORT`.

### 9.4 Propostas numeradas

No Paxos, cada proposta tem um número.

Exemplo:

```text
Proposta 10: COMMIT
Proposta 11: ABORT
```

O número da proposta ajuda a ordenar tentativas concorrentes.

### 9.5 Fase 1: Prepare / Promise

O proposer escolhe um número de proposta e envia uma mensagem `PREPARE` para os acceptors.

Exemplo:

```text
P1 -> A1, A2, A3: PREPARE(10)
```

Se um acceptor ainda não prometeu aceitar uma proposta maior, ele responde:

```text
A1 -> P1: PROMISE(10)
A2 -> P1: PROMISE(10)
```

A promessa significa:

> “Prometo não aceitar propostas menores que 10.”

Se o proposer recebe promessa da maioria, ele pode avançar.

### 9.6 Fase 2: Accept / Accepted

Depois de receber promessas da maioria, o proposer envia a proposta com o valor:

```text
P1 -> A1, A2, A3: ACCEPT(10, COMMIT)
```

Se os acceptors ainda não receberam uma proposta maior, eles aceitam:

```text
A1 -> P1: ACCEPTED(10, COMMIT)
A2 -> P1: ACCEPTED(10, COMMIT)
```

Quando uma maioria aceita o mesmo valor, o valor é escolhido.

### 9.7 Fluxo simplificado do Paxos

```text
1. Proposer escolhe número n.
2. Proposer envia PREPARE(n) para os acceptors.
3. Acceptors respondem PROMISE(n), se puderem.
4. Proposer recebe promessa da maioria.
5. Proposer envia ACCEPT(n, valor).
6. Acceptors respondem ACCEPTED(n, valor).
7. Se maioria aceitou, o valor foi escolhido.
```

### 9.8 O que Paxos garante?

Paxos garante principalmente:

> **Não serão escolhidos dois valores diferentes.**

Ou seja, ele protege a propriedade de **safety**.

### 9.9 Dificuldade prática

Paxos é elegante, mas difícil de implementar corretamente.

Algumas dificuldades:

- várias propostas podem competir;
- mensagens podem atrasar;
- acceptors precisam guardar promessas;
- learners precisam descobrir o valor escolhido;
- progresso pode depender de escolher um proposer/líder estável.

Por isso, em implementações práticas, costuma-se usar variações como **Multi-Paxos** ou algoritmos mais didáticos, como **Raft**.

---

## 10. Multi-Paxos

O Paxos básico decide apenas um valor.

Mas sistemas reais precisam decidir uma sequência de valores.

Exemplo:

```text
Log[1] = criar conta
Log[2] = depositar R$ 100
Log[3] = transferir R$ 50
Log[4] = consultar saldo
```

Cada posição do log precisa de consenso.

Executar Paxos completo para cada posição seria custoso. O **Multi-Paxos** otimiza esse processo, geralmente usando um líder estável para coordenar várias decisões.

### 10.1 Ideia principal

Em vez de repetir toda a negociação para cada comando, o sistema tenta manter um líder que propõe os próximos comandos.

Isso reduz mensagens e melhora desempenho.

### 10.2 Relação com bancos de dados e logs replicados

Multi-Paxos é importante porque muitos sistemas distribuídos usam a ideia de **log replicado**.

O log é uma sequência ordenada de operações.

Se todos os nós aplicam o mesmo log na mesma ordem, todos chegam ao mesmo estado.

Exemplo:

```text
Estado inicial:
saldo_A = 500
saldo_B = 100

Log:
1. transferir A -> B: 100
2. depositar em A: 50

Estado final:
saldo_A = 450
saldo_B = 200
```

O consenso garante que todos os servidores concordem com a ordem das operações do log.

---

## 11. Raft

**Raft** é um algoritmo de consenso criado para ser mais fácil de entender do que Paxos.

Ele também serve para gerenciar um log replicado. A ideia é que vários servidores mantenham o mesmo log de comandos e apliquem esses comandos na mesma ordem.

### 11.1 Ideia principal

Raft divide o problema em partes mais intuitivas:

1. eleição de líder;
2. replicação de log;
3. garantia de segurança.

### 11.2 Estados dos nós

Cada nó em Raft pode estar em um dos três estados:

| Estado | Função |
|---|---|
| **Follower** | Segue o líder e responde mensagens. |
| **Candidate** | Está tentando virar líder. |
| **Leader** | Coordena o cluster e replica comandos. |

### 11.3 Termos

Raft organiza o tempo em **termos**.

Um termo é como uma “temporada” de liderança.

Exemplo:

```text
Termo 1: S1 foi líder.
Termo 2: S3 foi líder.
Termo 3: S3 continuou líder.
Termo 4: S2 virou líder.
```

Os termos ajudam a identificar mensagens antigas e evitar confusão.

### 11.4 Heartbeats

O líder envia mensagens periódicas para os seguidores dizendo:

> “Estou vivo. Continuem me seguindo.”

Essas mensagens são chamadas de **heartbeats**.

Se um seguidor fica muito tempo sem receber heartbeat, ele suspeita que o líder caiu e inicia uma eleição.

### 11.5 Eleição de líder

Imagine 5 servidores:

```text
S1, S2, S3, S4, S5
```

No começo, todos são followers.

Se ninguém recebe heartbeat, um deles vira candidate.

Exemplo:

```text
S3 vira candidato no termo 2.
S3 pede votos para S1, S2, S4 e S5.
```

Se S3 recebe votos da maioria, vira líder:

```text
S3 recebeu votos de S1, S2 e S3.
Maioria = 3.
S3 é o novo líder.
```

### 11.6 Replicação de log

Depois que existe um líder, os clientes enviam comandos para ele.

Exemplo:

```text
Cliente -> Líder S3: transferir(A, B, 100)
```

O líder adiciona a operação ao seu log:

```text
Log de S3:
1. transferir(A, B, 100)
```

Depois envia para os seguidores:

```text
S3 -> S1: AppendEntries(transferir A B 100)
S3 -> S2: AppendEntries(transferir A B 100)
S3 -> S4: AppendEntries(transferir A B 100)
S3 -> S5: AppendEntries(transferir A B 100)
```

Quando a maioria confirma, o líder considera a entrada como comprometida:

```text
S1 confirmou.
S2 confirmou.
S3 já possui a entrada.
Maioria alcançada: 3 de 5.
Operação commitada.
```

### 11.7 O que acontece se um seguidor cair?

Se S5 cair, ainda existem 4 servidores funcionando.

Como a maioria é 3, o sistema continua.

```text
S1, S2, S3, S4 vivos
S5 caiu
Maioria ainda possível
```

Quando S5 voltar, ele sincroniza o log com o líder.

### 11.8 O que acontece se o líder cair?

Se o líder S3 cair, os followers deixam de receber heartbeat.

Depois de um timeout, algum follower vira candidate e começa uma nova eleição.

Exemplo:

```text
S2 não recebeu heartbeat.
S2 vira candidate.
S2 pede votos.
S2 recebe maioria.
S2 vira líder.
```

### 11.9 Por que Raft é didático?

Raft é bom para estudar e implementar porque separa claramente:

- quem manda;
- quem segue;
- como votar;
- como replicar;
- como decidir por maioria;
- como lidar com queda do líder.

Por isso, para um trabalho prático introdutório, Raft simplificado costuma ser uma ótima escolha.

---

## 12. PBFT e consenso bizantino

Paxos e Raft toleram falhas como queda, atraso e perda de mensagens. Eles não foram projetados para lidar com nós maliciosos.

Quando há possibilidade de comportamento malicioso, entra o conceito de **tolerância a falhas bizantinas**, ou **BFT**.

Um dos algoritmos clássicos dessa área é o **PBFT**, de Castro e Liskov.

### 12.1 O que é uma falha bizantina?

Uma falha bizantina ocorre quando um nó pode se comportar de maneira arbitrária.

Ele pode:

- mentir;
- enviar mensagens falsas;
- responder valores diferentes para nós diferentes;
- fingir que está seguindo o protocolo;
- tentar causar inconsistência.

### 12.2 Exemplo dos generais bizantinos

Imagine generais cercando uma cidade. Eles precisam decidir juntos:

```text
ATACAR
```

ou:

```text
RECUAR
```

Se todos atacam juntos, vencem. Se metade ataca e metade recua, perdem.

O problema é que alguns generais podem ser traidores e podem enviar mensagens falsas.

Esse problema representa sistemas distribuídos com nós maliciosos.

### 12.3 Ideia geral do PBFT

No PBFT, existe um nó primário e várias réplicas.

O protocolo normalmente passa por fases como:

```text
REQUEST
PRE-PREPARE
PREPARE
COMMIT
REPLY
```

A ideia é que as réplicas troquem mensagens suficientes entre si antes de executar uma operação.

### 12.4 Quantidade de nós necessária

Para tolerar `f` nós bizantinos, PBFT normalmente precisa de:

```text
N = 3f + 1
```

Exemplos:

| Nós maliciosos tolerados | Total mínimo de réplicas |
|---:|---:|
| 1 | 4 |
| 2 | 7 |
| 3 | 10 |

Isso é mais caro do que tolerar apenas falhas por parada.

### 12.5 Exemplo com 4 réplicas

Imagine:

```text
R0, R1, R2, R3
```

R0 é o primário. R3 é malicioso.

O cliente envia:

```text
registrar_laudo("aprovado")
```

O primário propõe uma ordem para a requisição. As réplicas trocam mensagens e só executam quando há confirmações suficientes.

Mesmo que R3 tente atrapalhar, os nós corretos conseguem manter a consistência, desde que o número de maliciosos esteja dentro do limite tolerado.

### 12.6 Relação com segurança

PBFT é importante quando o sistema não pode assumir que todos os servidores são honestos.

Exemplos:

- redes permissionadas;
- sistemas interorganizacionais;
- blockchains privadas;
- serviços críticos com risco de invasão;
- sistemas que precisam tolerar comportamento arbitrário de alguns nós.

---

## 13. Consenso em Blockchain

Blockchain é uma aplicação importante de consenso distribuído.

Em uma blockchain, vários nós precisam concordar sobre:

```text
Qual é o próximo bloco válido?
```

ou:

```text
Qual é o estado atual do ledger?
```

### 13.1 Ledger distribuído

Um **ledger** é um livro-razão, isto é, um registro de transações.

Em blockchain, esse ledger é distribuído e replicado entre vários nós.

Exemplo:

```text
Bloco 1 -> Bloco 2 -> Bloco 3 -> Bloco 4
```

Cada bloco contém transações, e cada bloco aponta para o anterior por meio de hash.

### 13.2 O problema do gasto duplo

Em uma moeda digital, uma pessoa poderia tentar gastar a mesma moeda duas vezes.

Exemplo:

```text
Alice tem 1 moeda.
Alice envia 1 moeda para Bob.
Alice tenta enviar a mesma 1 moeda para Carol.
```

A blockchain precisa fazer a rede concordar sobre qual transação entrou primeiro e qual deve ser rejeitada.

### 13.3 Proof of Work

No **Proof of Work**, os nós competem para resolver um problema computacional.

Quem encontra uma solução válida propõe o próximo bloco.

A segurança vem do custo de produzir blocos falsos. Para reescrever a história, o atacante precisaria controlar uma quantidade enorme de poder computacional.

### 13.4 Consenso probabilístico

Em blockchains como Bitcoin, o consenso não é instantâneo como em muitos sistemas permissionados.

A cada novo bloco adicionado depois de uma transação, aumenta a confiança de que aquela transação permanecerá na cadeia.

Por isso se fala em confirmações:

```text
1 confirmação
2 confirmações
3 confirmações
...
```

Quanto mais confirmações, menor a chance de reversão.

### 13.5 Proof of Stake

No **Proof of Stake**, validadores bloqueiam capital como garantia. Se agirem de forma desonesta, podem ser punidos.

A lógica deixa de ser “quem gastou mais poder computacional” e passa a envolver participação econômica e regras de validação.

### 13.6 Blockchain pública versus privada

| Tipo | Características |
|---|---|
| Blockchain pública | Qualquer pessoa pode participar; ambiente mais hostil; usa mecanismos como PoW ou PoS. |
| Blockchain privada/permissionada | Participantes são conhecidos; pode usar PBFT, Raft ou variações BFT. |

### 13.7 Relação com a disciplina

Na disciplina, Blockchain não deve ser vista apenas como criptomoeda.

Ela pode ser analisada como uma tecnologia distribuída que envolve:

- rede P2P;
- replicação;
- consenso;
- criptografia;
- ledger distribuído;
- imutabilidade;
- não repúdio;
- tolerância a falhas;
- análise de ameaças.

---

## 14. HyperPaxos e a ideia de hierarquia

O artigo indicado sobre **HyperPaxos** apresenta uma versão hierárquica do Paxos em múltiplas instâncias sobre a LibPaxos.

A ideia geral é tentar organizar melhor a comunicação do Paxos para melhorar desempenho, especialmente quando há muitos nós.

### 14.1 Por que pensar em hierarquia?

No Paxos tradicional, muitas mensagens podem circular entre proposers, acceptors e learners.

Em sistemas maiores, esse volume de mensagens pode se tornar um gargalo.

Uma estratégia é organizar os nós em uma estrutura hierárquica.

Exemplo simplificado:

```text
                 Coordenador
                    /   \
             Grupo 1     Grupo 2
             /   \       /   \
           A1    A2    A3    A4
```

Em vez de todos falarem diretamente com todos, alguns nós podem agregar ou difundir mensagens dentro de grupos.

### 14.2 Relação com escalabilidade

A hierarquia tenta reduzir custos de comunicação e organizar melhor o fluxo de mensagens.

Isso conversa com um tema importante da aula anterior:

> Sistemas distribuídos precisam escalar, mas a comunicação pela rede tem custo.

Quanto maior o número de nós, maior o desafio de manter consistência e consenso sem sobrecarregar a rede.

### 14.3 Como estudar HyperPaxos agora?

Para este momento da disciplina, HyperPaxos deve ser visto como leitura complementar avançada.

Antes de se aprofundar nele, é melhor dominar:

1. consenso;
2. quórum;
3. Paxos básico;
4. Multi-Paxos;
5. gargalos de comunicação;
6. escalabilidade;
7. tolerância a falhas.

---

## 15. Comparação entre algoritmos

| Algoritmo | Tipo de ambiente | Falhas consideradas | Ideia principal | Nível de dificuldade |
|---|---|---|---|---|
| **Paxos** | Distribuído permissionado | Falhas por parada | Propostas numeradas e maioria | Alto |
| **Multi-Paxos** | Logs replicados | Falhas por parada | Repetir consenso com líder estável | Alto |
| **Raft** | Clusters replicados | Falhas por parada | Líder, eleição e replicação de log | Médio |
| **PBFT** | Ambientes com risco de nó malicioso | Falhas bizantinas | Fases de preparação e commit com várias réplicas | Alto |
| **Proof of Work** | Blockchain pública | Nós desconhecidos e potencialmente maliciosos | Trabalho computacional e cadeia mais forte | Médio/alto |
| **Proof of Stake** | Blockchain pública | Nós maliciosos com capital em risco | Validadores e punições econômicas | Alto |
| **HyperPaxos** | Variação otimizada de Paxos | Baseado na família Paxos | Organização hierárquica da comunicação | Alto |

---

## 16. Exemplo completo: consenso para uma transação bancária

Vamos imaginar três servidores replicando o estado de contas bancárias.

```text
Banco-1
Banco-2
Banco-3
```

Estado inicial:

```text
Conta A = R$ 500
Conta B = R$ 100
```

Um cliente solicita:

```text
transferir(A, B, 100)
```

O estado correto depois da operação deveria ser:

```text
Conta A = R$ 400
Conta B = R$ 200
```

### 16.1 Sem consenso

Se cada servidor agir sozinho, pode acontecer:

```text
Banco-1 aplica a transferência.
Banco-2 aplica a transferência.
Banco-3 não recebe a mensagem.
```

Resultado:

```text
Banco-1: A = 400, B = 200
Banco-2: A = 400, B = 200
Banco-3: A = 500, B = 100
```

O sistema ficou inconsistente.

### 16.2 Com consenso

Agora imagine que o sistema usa uma lógica parecida com Raft.

```text
1. O líder recebe a operação transferir(A, B, 100).
2. O líder grava a operação no log.
3. O líder envia a operação para os seguidores.
4. Os seguidores confirmam o recebimento.
5. Quando a maioria confirma, a operação é commitada.
6. Os servidores aplicam a operação na mesma ordem.
```

Resultado:

```text
Banco-1: A = 400, B = 200
Banco-2: A = 400, B = 200
Banco-3: A = 400, B = 200
```

Se Banco-3 estivesse fora do ar, Banco-1 e Banco-2 ainda formariam maioria em um sistema de 3 nós.

Quando Banco-3 voltasse, ele sincronizaria o log.

### 16.3 E se o líder cair no meio?

Imagine:

```text
Líder Banco-1 recebeu a operação.
Banco-1 caiu antes de replicar para a maioria.
```

A operação ainda não deve ser considerada confirmada.

Outro servidor será eleito líder, e o sistema continuará a partir do último log considerado seguro.

### 16.4 E se a mensagem for perdida?

```text
Banco-1 envia operação para Banco-3.
A mensagem se perde.
```

Se Banco-1 e Banco-2 formam maioria, a operação ainda pode ser commitada.

Depois, Banco-3 recebe a operação novamente e atualiza o seu log.

### 16.5 E se a mensagem for modificada por atacante?

Mensagem original:

```text
transferir(A, B, 100)
```

Mensagem alterada:

```text
transferir(A, C, 1000)
```

Esse problema não é resolvido apenas com consenso.

É necessário usar mecanismos de segurança, como:

- autenticação;
- assinatura digital;
- hash;
- MAC;
- TLS;
- controle de replay;
- autorização.

Consenso decide o valor. Criptografia e segurança ajudam a garantir que a mensagem não foi falsificada, adulterada ou enviada por alguém indevido.

---

## 17. Análise de segurança

A análise de segurança é essencial porque consenso distribuído não existe isolado. Ele depende de rede, autenticação, armazenamento, controle de acesso e tratamento de falhas.

### 17.1 Mensagem perdida

Situação:

```text
Líder envia AppendEntries para S4.
S4 não recebe.
```

Consequência:

- se ainda houver maioria, o sistema continua;
- se não houver maioria, o sistema para de decidir temporariamente.

Análise:

> A perda de mensagem afeta disponibilidade, mas não deve afetar a consistência.

### 17.2 Mensagem atrasada

Situação:

```text
Mensagem antiga chega depois de uma eleição nova.
```

Consequência:

- o nó pode receber informação obsoleta.

Defesas:

- usar número de termo;
- usar número de proposta;
- usar índice de log;
- descartar mensagens antigas.

### 17.3 Mensagem duplicada

Situação:

```text
O mesmo ACCEPT chega duas vezes.
```

Defesas:

- operações idempotentes;
- identificador único de requisição;
- controle de mensagens já processadas.

### 17.4 Mensagem modificada

Situação:

```text
COMMIT operação 10
```

vira:

```text
COMMIT operação 99
```

Defesas:

- assinatura digital;
- hash;
- HMAC/MAC;
- TLS;
- validação do remetente.

### 17.5 Replay attack

O atacante captura uma mensagem válida antiga e reenvia depois.

Exemplo:

```text
Mensagem antiga: transferir(A, B, 100)
Atacante reenvia a mesma mensagem depois.
```

Defesas:

- nonce;
- timestamp;
- número sequencial;
- identificador único da transação;
- janela de validade;
- registro de operações já executadas.

### 17.6 Nó malicioso

Situação:

```text
S3 envia informação falsa para S1.
S3 envia outra informação falsa para S2.
```

Consequência:

- Paxos/Raft puros não foram feitos para esse cenário.

Solução:

- usar protocolos BFT, como PBFT;
- usar assinaturas;
- usar auditoria;
- usar validação independente;
- limitar permissões dos nós.

### 17.7 Partição de rede

Imagine 5 nós:

```text
S1, S2, S3 | S4, S5
```

A rede se dividiu em dois grupos.

Grupo A:

```text
S1, S2, S3
```

Grupo B:

```text
S4, S5
```

Como a maioria é 3, apenas o grupo A pode continuar tomando decisões.

O grupo B não deve decidir sozinho.

Isso evita o problema de **split-brain**, em que duas partes do sistema acham que são o sistema principal.

### 17.8 Ataque de negação de serviço

Um atacante pode tentar impedir o consenso inundando a rede ou derrubando nós.

Consequência:

- o sistema pode perder disponibilidade;
- se não houver maioria, não há progresso.

Defesas:

- rate limiting;
- autenticação;
- balanceamento;
- monitoramento;
- isolamento de rede;
- redundância;
- mitigação de DDoS.

---

## 18. Sugestão de implementação para trabalho prático

Para uma primeira implementação, a melhor escolha costuma ser um **Raft simplificado**.

Ele permite demonstrar consenso de forma clara, sem entrar em todos os detalhes formais do Paxos.

### 18.1 Objetivo da simulação

Criar uma simulação com 5 nós que consiga:

- eleger um líder;
- enviar heartbeats;
- receber comandos de cliente;
- replicar comandos no log;
- confirmar operações por maioria;
- simular queda de nó;
- simular queda do líder;
- simular perda de mensagem;
- mostrar quando há ou não consenso.

### 18.2 Estrutura dos nós

Cada nó pode ter:

```python
id
estado  # follower, candidate, leader
termo_atual
votou_em
log
commit_index
vivo
```

### 18.3 Eventos possíveis

```text
start_election
request_vote
receive_vote
become_leader
send_heartbeat
append_entries
commit_entry
crash_node
recover_node
lose_message
```

### 18.4 Pseudocódigo simplificado

```text
iniciar cluster com 5 nós
escolher timeout aleatório para cada follower

se follower não recebe heartbeat:
    vira candidate
    incrementa termo
    vota em si mesmo
    pede votos aos outros

se candidate recebe maioria:
    vira leader
    começa a enviar heartbeats

quando cliente envia comando:
    leader adiciona comando ao log
    leader envia comando aos followers
    se maioria confirma:
        leader marca comando como commitado
        followers aplicam comando
```

### 18.5 Cenários de teste

#### Cenário 1: funcionamento normal

```text
5 nós vivos.
S1 é eleito líder.
Cliente envia comando.
Comando é replicado em maioria.
Comando é commitado.
```

#### Cenário 2: queda de seguidor

```text
S5 cai.
S1 continua líder.
S1 replica comando em S2 e S3.
Maioria alcançada.
Sistema continua.
```

#### Cenário 3: queda do líder

```text
S1 cai.
Followers param de receber heartbeat.
S3 inicia eleição.
S3 recebe maioria.
S3 vira novo líder.
```

#### Cenário 4: partição de rede

```text
Grupo A: S1, S2, S3
Grupo B: S4, S5
```

Resultado esperado:

```text
Grupo A pode decidir.
Grupo B não pode decidir.
```

#### Cenário 5: mensagem perdida

```text
S1 envia comando para S4.
Mensagem é perdida.
S1 ainda recebe confirmações de S2 e S3.
Maioria alcançada.
Comando é commitado.
```

### 18.6 O que analisar no relatório

No relatório, é importante explicar:

- qual algoritmo foi escolhido;
- qual problema ele resolve;
- quais falhas ele tolera;
- quais falhas ele não tolera;
- como ele usa maioria;
- quando ele para de progredir;
- como se comporta diante de perda de mensagens;
- o que aconteceria se uma mensagem fosse modificada;
- quais mecanismos de segurança seriam necessários.

### 18.7 Limitações de uma implementação simplificada

Uma versão didática provavelmente não implementará tudo, como:

- persistência em disco;
- reconfiguração do cluster;
- snapshots;
- compactação de log;
- TLS;
- autenticação forte;
- tolerância a falhas bizantinas.

Isso não é um problema, desde que o relatório deixe claro que é uma simulação educacional.

---

## 19. Resumo para revisar antes da aula

### O que é consenso?

> É fazer vários nós concordarem sobre um mesmo valor.

### Por que é difícil?

Porque em sistemas distribuídos:

- não há relógio global confiável;
- não há memória compartilhada;
- mensagens atrasam;
- mensagens se perdem;
- nós falham;
- a rede pode particionar;
- em alguns cenários, nós podem ser maliciosos.

### Quais são as propriedades principais?

- acordo;
- validade;
- terminação;
- integridade.

### O que é quórum?

É o conjunto mínimo de nós necessário para tomar uma decisão.

Normalmente usa-se maioria.

### O que é Paxos?

Um algoritmo clássico de consenso baseado em propostas numeradas e maioria de acceptors.

### O que é Raft?

Um algoritmo de consenso mais didático, baseado em líder, eleição e replicação de log.

### O que é PBFT?

Um algoritmo para consenso com tolerância a falhas bizantinas, ou seja, quando alguns nós podem agir de maneira maliciosa.

### O que é consenso em Blockchain?

É o mecanismo que permite que uma rede distribuída concorde sobre o estado do ledger e sobre quais blocos/transações são válidos.

### Qual algoritmo parece melhor para implementar primeiro?

Para uma atividade prática inicial:

```text
Raft simplificado
```

Motivos:

- é mais fácil de explicar;
- é mais fácil de simular;
- permite mostrar eleição de líder;
- permite mostrar replicação de log;
- permite mostrar maioria;
- permite testar queda de líder e perda de mensagem.

---

## 20. Glossário

| Termo | Significado |
|---|---|
| **Nó** | Máquina, processo ou servidor participante do sistema. |
| **Consenso** | Decisão comum entre vários nós. |
| **Quórum** | Quantidade mínima de votos/respostas para decidir. |
| **Maioria** | Mais da metade dos nós. |
| **Safety** | Garantia de que o sistema não decide algo errado. |
| **Liveness** | Garantia de que o sistema continua progredindo. |
| **Leader** | Nó coordenador em algoritmos como Raft. |
| **Follower** | Nó que segue o líder. |
| **Candidate** | Nó tentando se tornar líder. |
| **Log** | Sequência ordenada de operações. |
| **Commit** | Confirmação definitiva de uma operação. |
| **Heartbeat** | Mensagem periódica enviada pelo líder para indicar que está vivo. |
| **Timeout** | Tempo máximo de espera antes de suspeitar de falha. |
| **Crash fault** | Falha em que o nó para de funcionar. |
| **Falha bizantina** | Falha em que o nó pode agir de forma arbitrária ou maliciosa. |
| **Split-brain** | Situação em que dois grupos do sistema acham que podem decidir independentemente. |
| **Ledger** | Livro-razão distribuído, comum em blockchain. |
| **Proof of Work** | Consenso baseado em trabalho computacional. |
| **Proof of Stake** | Consenso baseado em validadores com capital em risco. |

---

## 21. Referências

### Livros-base

- TANENBAUM, Andrew S.; VAN STEEN, Maarten. **Distributed Systems: Principles and Paradigms**. 2. ed. Pearson, 2007.
- VAN STEEN, Maarten; TANENBAUM, Andrew S. **Distributed Systems**. 4. ed. distributed-systems.net, 2023.
- COULOURIS, George; DOLLIMORE, Jean; KINDBERG, Tim; BLAIR, Gordon. **Distributed Systems: Concepts and Design**. 5. ed. Addison-Wesley, 2011.
- KSHEMKALYANI, Ajay D.; SINGHAL, Mukesh. **Distributed Computing: Principles, Algorithms, and Systems**. Cambridge University Press, 2008.

### Artigos e materiais clássicos

- LAMPORT, Leslie. **Paxos Made Simple**. 2001.  
  https://lamport.azurewebsites.net/pubs/paxos-simple.pdf

- FISCHER, Michael J.; LYNCH, Nancy A.; PATERSON, Michael S. **Impossibility of Distributed Consensus with One Faulty Process**. Journal of the ACM, 1985.  
  https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf

- ONGARO, Diego; OUSTERHOUT, John. **In Search of an Understandable Consensus Algorithm (Raft)**. USENIX ATC, 2014.  
  https://www.usenix.org/conference/atc14/technical-sessions/presentation/ongaro

- CASTRO, Miguel; LISKOV, Barbara. **Practical Byzantine Fault Tolerance**. OSDI, 1999.  
  https://pdos.csail.mit.edu/6.824/papers/castro-practicalbft.pdf

- NAKAMOTO, Satoshi. **Bitcoin: A Peer-to-Peer Electronic Cash System**. 2008.  
  https://bitcoin.org/bitcoin.pdf

### Material indicado

- PEREIRA, Djenifer R.; KIOTHEKA, Fernando M.; DUARTE JR., Elias P. **HyperPaxos em Múltiplas Instâncias sobre a LibPaxos: Uma Versão Hierárquica do Algoritmo de Consenso Paxos**. REIC/SBC, 2023.  
  https://journals-sol.sbc.org.br/index.php/reic/article/view/3427

- Vídeo indicado: **Algoritmos de consenso em sistemas distribuídos**.  
  https://www.youtube.com/watch?v=NfhkCZUXM20

---

## Observação final

Para a próxima aula, o mais importante é chegar entendendo a intuição:

> Consenso não é simplesmente “votar”. Consenso é garantir que, mesmo com falhas, atrasos e mensagens problemáticas, os nós corretos não tomem decisões contraditórias.

Se for necessário implementar algo, **Raft simplificado** é provavelmente a opção mais didática. Se o foco for teoria, **Paxos** é obrigatório. Se o foco for segurança contra nós maliciosos, é preciso olhar para **PBFT** e protocolos BFT. Se o foco for Blockchain, o consenso precisa ser analisado junto com criptografia, rede P2P, incentivos e validação de blocos.
