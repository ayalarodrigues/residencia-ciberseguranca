# Aula 02 — Algoritmos Distribuídos

Disciplina: **Algoritmos Distribuídos e Blockchain**
Tema da aula: **Principais algoritmos distribuídos e problemas resolvidos por eles**
Data: **09/06**

---

## 1. Ideia geral da aula

Nesta aula, o objetivo foi compreender **o que são algoritmos distribuídos**, em quais ambientes eles são executados e quais problemas clássicos eles ajudam a resolver.

A ideia central é que, em vez de um único computador resolver tudo sozinho, vários computadores trabalham em conjunto. Porém, como esses computadores estão distribuídos, eles não compartilham memória, não possuem um relógio global perfeitamente sincronizado e precisam se comunicar por mensagens.

Em um sistema distribuído, cada máquina possui apenas uma visão parcial do sistema. Ela sabe o que está acontecendo localmente, mas não sabe automaticamente o que está ocorrendo nas demais. Para descobrir isso, precisa enviar e receber mensagens pela rede.

De forma simples:

```text
Algoritmo distribuído = processamento + comunicação entre máquinas
```

Ou seja, não basta que cada máquina execute uma parte do programa. É necessário que as máquinas cooperem corretamente, coordenem suas ações e mantenham o sistema funcionando de forma consistente.

---

## 2. Motivação: por que estudar algoritmos distribuídos?

A motivação principal da aula foi pensar em situações nas quais vários computadores precisam tomar decisões em conjunto.

Exemplo:

```text
Um conjunto de servidores precisa decidir se uma atualização será gravada
ou não em uma base de dados distribuída.
```

Se o sistema possui réplicas de dados, essas réplicas precisam manter uma visão coerente do estado. Caso uma máquina grave uma informação e as demais não gravem, o sistema pode ficar inconsistente.

Por isso, os algoritmos distribuídos são usados para resolver problemas como:

* coordenação entre computadores;
* ordenação de eventos;
* sincronização;
* replicação;
* consenso;
* eleição de líder;
* tolerância a falhas;
* detecção de problemas, como deadlock;
* manutenção de consistência entre réplicas.

O professor destacou que sistemas como **WhatsApp**, **Netflix**, **Google Docs**, **Pix**, **bancos digitais**, **blockchains**, **serviços em nuvem** e **bancos de dados distribuídos** dependem de algum tipo de coordenação distribuída.

A ideia é que, para o usuário, o sistema pareça único e funcione corretamente, mesmo que por trás existam muitos servidores envolvidos.

---

## 3. Diferença em relação a um programa tradicional

Em um programa tradicional centralizado, normalmente existe uma única máquina executando as instruções. A memória é local, o processador é local e a ordem de execução é controlada pelo próprio programa.

Em um sistema distribuído, a situação muda:

```text
Um programa pode ser dividido em várias partes,
e cada parte pode executar em uma máquina diferente.
```

Isso cria um problema importante:

> Como garantir que um programa distribuído produza o mesmo resultado que produziria se estivesse rodando em uma única máquina?

Essa é uma das grandes motivações dos algoritmos distribuídos. Eles tentam coordenar as partes do sistema para que a execução distribuída seja correta.

---

## 4. Características da infraestrutura distribuída

Os algoritmos distribuídos normalmente executam em ambientes com as seguintes características:

* vários computadores trabalhando ao mesmo tempo;
* ausência de relógio global;
* ausência de memória compartilhada;
* falhas independentes;
* mensagens sujeitas a atraso;
* mensagens que podem ser perdidas;
* máquinas com capacidades diferentes;
* rede com latência variável.

Essas características tornam o ambiente distribuído mais complexo do que o centralizado.

---

## 5. Ausência de relógio global

Uma das premissas mais importantes da aula foi:

> Em sistemas distribuídos, não existe um relógio global único e perfeitamente sincronizado.

Cada computador possui seu próprio relógio local. Um servidor pode estar com um horário ligeiramente diferente do outro. Por isso, não é seguro depender apenas do horário físico para dizer qual evento aconteceu primeiro.

Exemplo:

```text
Servidor A registra uma compra.
Servidor B registra o pagamento.
```

Se os relógios dos dois servidores não estiverem sincronizados, comparar apenas os horários pode gerar uma interpretação incorreta da ordem dos eventos.

Por isso, em sistemas distribuídos, muitas vezes o mais importante não é saber exatamente **que horas** algo aconteceu, mas sim saber **qual evento aconteceu antes de outro**.

---

## 6. Ausência de memória compartilhada

Outra premissa importante:

> Em sistemas distribuídos, os processos não compartilham memória.

Cada computador possui sua própria memória local. Um processo não consegue simplesmente acessar a variável de outro processo em outra máquina.

Para saber o que está acontecendo nos demais nós, ele precisa trocar mensagens.

Exemplo:

```text
Servidor A quer saber se Servidor B já concluiu uma operação.
```

Ele não acessa diretamente a memória do Servidor B. Ele envia uma mensagem perguntando ou aguardando uma resposta.

Isso significa que o estado global do sistema é formado por:

* o estado local de cada processo;
* as mensagens que estão em trânsito na rede.

As mensagens que ainda estão viajando também influenciam o sistema, porque podem carregar decisões, confirmações, operações ou atualizações.

---

## 7. Distância, atrasos e falhas

Em sistemas distribuídos, os computadores podem estar em locais diferentes. Isso significa que o tempo de envio de uma mensagem pode variar.

Uma mensagem pode:

* chegar rapidamente;
* demorar;
* chegar fora de ordem;
* ser perdida;
* não receber resposta;
* gerar timeout.

Um ponto importante da aula foi a dificuldade de distinguir se uma máquina realmente falhou ou se está apenas lenta.

Exemplo:

```text
Um servidor em São Paulo demora a responder a um servidor em Tóquio.
```

A dúvida é:

```text
O servidor caiu?
Ou a rede está congestionada?
Ou a máquina está sobrecarregada?
```

Muitos algoritmos tratam isso usando **timeouts**. Se o nó não responde dentro de um tempo definido, o sistema passa a suspeitar que ele falhou.

O problema é que essa suspeita pode estar errada. O nó pode estar vivo, mas lento.

---

## 8. Comunicação assíncrona

A comunicação em sistemas distribuídos pode ser assíncrona.

Na comunicação síncrona, um processo envia uma mensagem e fica esperando a resposta antes de continuar.

Na comunicação assíncrona, o processo envia a mensagem e continua executando outras tarefas. Quando a resposta chega, ela é tratada posteriormente.

Exemplo simplificado:

```text
Comunicação síncrona:
envia mensagem → espera resposta → continua

Comunicação assíncrona:
envia mensagem → continua executando → trata resposta quando chegar
```

A comunicação assíncrona ajuda a melhorar o desempenho, porque evita que o processo fique bloqueado esperando uma resposta.

Um exemplo citado em aula foi o funcionamento de servidores Web. Quando chega uma requisição, o servidor pode delegar o atendimento a uma thread ou subprocesso e continuar disponível para receber novas requisições.

---

## 9. Problemas clássicos em sistemas distribuídos

A aula apresentou diferentes classes de problemas e algoritmos distribuídos relacionados a cada uma delas.

As principais classes vistas foram:

| Classe de problema   | Algoritmo apresentado           | Ideia central                             |
| -------------------- | ------------------------------- | ----------------------------------------- |
| Ordenação de eventos | Relógio lógico de Lamport       | Saber qual evento ocorreu antes           |
| Sincronização        | Exclusão mútua baseada em token | Controlar acesso a recurso compartilhado  |
| Replicação           | Primary-Backup                  | Manter cópias sincronizadas               |
| Consenso             | Raft                            | Fazer vários nós chegarem à mesma decisão |

Um ponto importante é que esses algoritmos podem se combinar. Às vezes, um algoritmo resolve parte do problema, mas depende de outro para lidar com falhas, sincronização ou escolha de líder.

---

# 10. Ordenação de eventos

## 10.1 Problema

O primeiro problema apresentado foi a **ordenação de eventos**.

Em sistemas distribuídos, como não existe um relógio global confiável, surge a pergunta:

```text
Qual evento aconteceu antes?
```

Essa pergunta é importante porque a ordem dos eventos pode alterar o resultado final do sistema.

Exemplo:

```text
1. Debitar R$ 100 da conta A.
2. Creditar R$ 100 na conta B.
```

A ordem importa. O débito deve ocorrer antes do crédito. Se a ordem for invertida ou interpretada incorretamente, o sistema pode ficar inconsistente.

Outro exemplo é uma conversa em aplicativo de mensagens. Se uma pergunta chega depois da resposta, o sentido da conversa pode ser alterado.

---

## 10.2 Onde a ordenação de eventos aparece?

Algoritmos de ordenação de eventos são usados em:

* bancos de dados distribuídos;
* sistemas financeiros;
* aplicações colaborativas;
* blockchain;
* sistemas de mensagens;
* controle de transações;
* registros de operações.

Exemplos:

```text
Google Docs: ordenar alterações feitas por diferentes usuários.
WhatsApp: exibir mensagens em ordem coerente.
Blockchain: definir a sequência das transações.
Bancos digitais: registrar operações na ordem correta.
```

---

# 11. Algoritmo de Lamport

## 11.1 Ideia principal

O algoritmo de Lamport responde à seguinte pergunta:

```text
Se eu não posso confiar nos relógios físicos,
como posso estabelecer uma ordem entre os eventos?
```

A ideia de Leslie Lamport foi usar **relógios lógicos** em vez de relógios físicos.

Esse relógio lógico não representa horas, minutos ou segundos. Ele representa apenas um contador de eventos.

```text
Relógio lógico = contador de eventos
```

A ideia é simples e poderosa:

> Em vez de perguntar “que horas são?”, pergunta-se “qual evento ocorreu antes de outro?”.

---

## 11.2 Regras do relógio de Lamport

Cada processo mantém um contador local `L`.

Inicialmente:

```text
L = 0
```

### Regra 1 — Evento local

Sempre que ocorre um evento local, o contador é incrementado:

```text
L = L + 1
```

### Regra 2 — Envio de mensagem

Antes de enviar uma mensagem, o processo incrementa o contador e envia o valor junto com a mensagem:

```text
L = L + 1
enviar mensagem com L
```

### Regra 3 — Recebimento de mensagem

Ao receber uma mensagem, o processo compara:

* seu relógio local;
* o relógio recebido na mensagem.

Depois atualiza seu contador com:

```text
L = max(L_local, L_recebido) + 1
```

O objetivo é garantir que, se um evento influencia outro, então o primeiro evento tenha um número menor que o segundo.

---

## 11.3 Exemplo intuitivo com mensagens

Imagine dois servidores:

```text
Servidor A
Servidor B
```

Ambos começam com contador zero:

```text
A: L = 0
B: L = 0
```

O usuário no Servidor A envia uma mensagem.

Antes de enviar:

```text
A incrementa L.
A envia a mensagem com o valor do contador.
```

Quando B recebe:

```text
B compara seu L local com o L recebido.
B escolhe o maior valor e soma 1.
```

Assim, B consegue saber que a mensagem recebida veio de um evento anterior.

---

## 11.4 Analogia das senhas de atendimento

Uma analogia usada na aula foi pensar em senhas de atendimento.

Cada computador possui um bloco de senhas. Sempre que algo acontece, ele pega uma nova senha. Quando envia uma mensagem para outro computador, envia junto o número da senha.

Assim, mesmo sem saber exatamente o horário real, os computadores conseguem montar uma ordem lógica dos acontecimentos.

```text
Quem tem senha menor aconteceu antes.
Quem tem senha maior aconteceu depois.
```

---

## 11.5 Limitações e pontos de atenção

O relógio de Lamport ajuda a ordenar eventos em relação de causa e efeito, mas ele não resolve todos os problemas do sistema distribuído.

Algumas limitações:

* não impede perda de mensagens;
* não impede falhas de máquinas;
* não garante, sozinho, sincronização de acesso a recurso compartilhado;
* não resolve consenso;
* não garante segurança contra mensagens alteradas.

Por isso, a aula destacou que ordenar eventos é importante, mas não resolve tudo. Depois de ordenar eventos, ainda pode ser necessário controlar concorrência, sincronizar acesso, replicar dados ou chegar a consenso.

---

# 12. Sincronização distribuída

## 12.1 Problema

A sincronização distribuída busca coordenar processos que desejam acessar recursos compartilhados.

O problema aparece quando dois ou mais processos tentam acessar o mesmo recurso ao mesmo tempo, especialmente quando pelo menos um deles deseja modificar esse recurso.

Exemplo:

```text
Dois servidores acessam a mesma conta bancária.

Saldo inicial: R$ 1000

Servidor A realiza saque de R$ 200.
Servidor B realiza saque de R$ 300.
```

Se os dois atualizarem o saldo ao mesmo tempo, o resultado pode ficar incorreto.

O valor correto seria:

```text
1000 - 200 - 300 = 500
```

Mas, se ambos lerem o saldo inicial ao mesmo tempo e escreverem resultados diferentes, uma atualização pode sobrescrever a outra.

---

## 12.2 Quando há conflito?

Um ponto importante da aula:

> Conflito ocorre quando dois ou mais processos acessam o mesmo recurso e pelo menos um deles quer alterar esse recurso.

Leitura com leitura normalmente não gera conflito.

Exemplo:

```text
Processo A lê o dado.
Processo B lê o mesmo dado.
```

Não há problema.

Mas se um processo lê e outro escreve, ou se dois escrevem, pode haver conflito.

```text
Processo A lê o dado.
Processo B escreve no mesmo dado.
```

Nesse caso, pode haver leitura desatualizada ou inconsistência.

---

## 12.3 Objetivos da sincronização

Os algoritmos de sincronização buscam:

* evitar conflitos entre processos;
* coordenar o acesso a recursos compartilhados;
* garantir que ações ocorram na ordem correta;
* manter a consistência do sistema;
* impedir que dois processos entrem ao mesmo tempo em uma região crítica.

---

# 13. Exclusão mútua baseada em token

## 13.1 Ideia principal

Um dos algoritmos apresentados para sincronização foi a **exclusão mútua baseada em token**.

A ideia é simples:

```text
Existe um token circulando entre os processos.
Apenas quem possui o token pode acessar o recurso compartilhado.
```

O token funciona como uma chave, uma moeda ou um microfone.

Analogia:

```text
Em uma reunião, apenas quem está com o microfone pode falar.
Quando termina, passa o microfone para outra pessoa.
```

No algoritmo:

```text
Apenas quem está com o token pode entrar na região crítica.
```

---

## 13.2 Região crítica

A região crítica é a parte do sistema onde ocorre acesso a um recurso compartilhado.

Exemplo:

```text
Atualizar saldo em uma conta bancária.
Alterar uma linha em um banco de dados.
Modificar uma variável compartilhada.
Registrar uma operação em uma réplica.
```

Se dois processos entrarem na região crítica ao mesmo tempo, pode haver inconsistência.

---

## 13.3 Regras do algoritmo

As regras básicas são:

1. existe um único token no sistema;
2. apenas quem possui o token pode entrar na região crítica;
3. após terminar, o processo passa o token para outro processo;
4. quem não possui o token deve esperar.

Em pseudocódigo:

```text
se processo possui token:
    entra na região crítica
    executa operação
    sai da região crítica
    passa token para o próximo
senão:
    espera token chegar
```

---

## 13.4 Exemplo com três servidores

Imagine três servidores:

```text
A, B e C
```

Todos precisam acessar um banco de dados compartilhado.

Se A está com o token, apenas A pode acessar o banco. B e C esperam.

Quando A termina, passa o token. Se B recebe o token, B pode acessar. Depois passa para C, e assim por diante.

---

## 13.5 Limitações

O professor chamou atenção para dois problemas importantes:

### 1. E se o token for perdido?

Como o token é uma mensagem especial, ele pode se perder na rede. Se isso acontecer, nenhum processo consegue acessar a região crítica.

```text
Sem token, ninguém entra na região crítica.
```

### 2. E se o nó que possui o token falhar?

Se o processo que está com o token falha antes de repassá-lo, o sistema também pode ficar bloqueado.

Nesse caso, seria necessário algum mecanismo para detectar a falha e gerar um novo token de forma controlada.

Isso pode exigir outro algoritmo, como:

* eleição de líder;
* consenso;
* detecção de falhas;
* regeneração segura do token.

Essa observação reforça uma ideia importante da aula:

> Muitas soluções distribuídas combinam vários algoritmos.

---

# 14. Deadlock em sistemas distribuídos

Durante a aula, também foi discutida a ideia de deadlock.

Deadlock ocorre quando dois ou mais processos ficam esperando uns pelos outros indefinidamente.

Exemplo simplificado:

```text
Transação T1 segura o recurso A e espera o recurso B.
Transação T2 segura o recurso B e espera o recurso A.
```

Nesse caso:

```text
T1 espera T2.
T2 espera T1.
```

Nenhuma das duas consegue prosseguir.

Uma forma de representar isso é por um grafo de espera.

```text
T1 → T2
T2 → T1
```

Como existe um ciclo, há deadlock.

Uma solução comum é abortar uma das transações para liberar os recursos. Porém, isso pode gerar outro problema: uma transação pode ser abortada repetidamente. Para evitar injustiça, pode-se usar um contador de abortos e aumentar a prioridade de quem já foi abortado várias vezes.

Esse ponto foi importante porque mostra que sistemas distribuídos também precisam lidar com:

* concorrência;
* bloqueios;
* conflitos;
* justiça;
* reinício de operações.

---

# 15. Replicação distribuída

## 15.1 Ideia principal

A replicação distribuída consiste em manter cópias de dados ou serviços em vários computadores.

Essas cópias são chamadas de réplicas.

A ideia principal é:

```text
Ter várias cópias para aumentar disponibilidade, desempenho e tolerância a falhas.
```

Se uma réplica falhar, outra pode assumir.

Exemplos de replicação:

* bancos de dados distribuídos;
* sistemas de arquivos distribuídos;
* armazenamento em nuvem;
* redes sociais;
* e-commerce;
* streaming;
* servidores de aplicação;
* blockchain.

---

## 15.2 Benefícios da replicação

A replicação pode trazer:

### Alta disponibilidade

Se um nó falha, outro pode responder.

```text
Se S1 cair, S2 ou S3 podem continuar atendendo.
```

### Tolerância a falhas

O sistema não depende de uma única máquina.

### Melhor desempenho

Leituras podem ser distribuídas entre várias réplicas.

### Menor latência

Réplicas podem estar mais próximas dos usuários.

### Confiabilidade

Se uma cópia é perdida, outra pode ser usada para recuperação.

---

## 15.3 Leitura versus escrita

Um ponto importante da aula foi a diferença entre leitura e escrita.

Leitura normalmente é mais simples:

```text
Ler não altera o estado.
```

Por isso, várias réplicas podem atender leituras.

Já escrita é mais complexa:

```text
Escrever altera o estado.
```

Quando uma réplica recebe uma escrita, essa alteração precisa ser propagada para as demais réplicas.

---

## 15.4 Consistência forte e consistência eventual

A aula destacou a diferença entre dois modelos de consistência.

### Consistência forte

Todas as réplicas devem ver o mesmo dado ao mesmo tempo.

Exemplo:

```text
Internet banking
Pix
Sistemas financeiros
```

Nesses casos, não é aceitável que uma réplica mostre um saldo e outra mostre outro.

A desvantagem é o custo: para manter tudo igual, pode ser necessário bloquear operações, trocar mais mensagens e reduzir desempenho.

### Consistência eventual

As réplicas podem ficar temporariamente diferentes, mas convergem com o tempo.

Exemplo:

```text
Redes sociais
Cache
Conteúdos de baixa criticidade
```

Em uma rede social, pode ser aceitável que uma atualização demore alguns segundos para aparecer para todos.

A vantagem é o desempenho. A desvantagem é que, por um tempo, usuários diferentes podem ver estados diferentes.

---

## 15.5 Trade-off entre consistência e desempenho

Um ponto muito reforçado na aula foi que há sempre um equilíbrio a ser feito.

```text
Mais consistência → mais mensagens, mais bloqueios, menor desempenho.
Mais desempenho → maior chance de inconsistência temporária.
```

Portanto, a escolha depende da aplicação.

Para sistemas bancários, a consistência é mais importante.

Para redes sociais, o desempenho pode ser priorizado.

---

# 16. Algoritmo Primary-Backup

## 16.1 Ideia principal

O Primary-Backup é um modelo simples de replicação.

Ele possui:

* um nó primário (`primary`);
* um ou mais nós secundários (`backups`).

A ideia principal é:

```text
O primary decide as atualizações.
Os backups copiam.
```

O objetivo é manter os dados iguais em várias máquinas.

---

## 16.2 Funcionamento

O funcionamento básico é:

1. o cliente envia uma requisição de escrita;
2. o primary recebe e processa a requisição;
3. o primary envia a atualização para os backups;
4. os backups aplicam a atualização localmente;
5. o cliente recebe a confirmação.

Em pseudocódigo:

```text
cliente envia update para primary
primary aplica update localmente
primary envia update para backups
backups aplicam update
primary confirma ao cliente
```

---

## 16.3 Exemplo com três servidores

Imagine:

```text
S1 = Primary
S2 = Backup
S3 = Backup
```

Operação:

```text
depositar R$ 100
```

Fluxo:

```text
1. Cliente envia "depositar 100" para S1.
2. S1 atualiza seu estado.
3. S1 envia atualização para S2 e S3.
4. S2 e S3 atualizam seus estados.
```

---

## 16.4 Leitura e escrita no Primary-Backup

### Escrita

A escrita vai para o primary.

```text
Update → primary → backups
```

Isso ocorre porque a escrita altera o estado e precisa ser coordenada.

### Leitura

A leitura pode ser feita no primary ou em um backup, pois não altera o estado.

```text
Read → primary ou backup
```

---

## 16.5 Problema: falha do primary

O primary é um ponto central do algoritmo.

Se ele falha, o sistema pode parar temporariamente.

Por isso, é necessário escolher um novo primary entre os backups.

Esse processo é chamado de:

```text
eleição de líder
```

Aqui aparece novamente a ideia de combinação de algoritmos:

```text
Primary-Backup usa replicação,
mas pode depender de eleição de líder para lidar com falha do primary.
```

---

## 16.6 Vulnerabilidades e limitações

Alguns problemas possíveis no Primary-Backup:

* o primary pode falhar antes de replicar a atualização;
* um backup pode não receber a atualização;
* uma réplica pode ficar inconsistente;
* o sistema pode bloquear se exigir consistência forte;
* o primary pode se tornar gargalo;
* pode haver dificuldade para saber se um backup falhou ou está apenas lento;
* em caso de falha, é necessário eleger novo primary;
* logs precisam ser mantidos para recuperação.

O professor destacou que logs são importantes, mas também precisam ser armazenados de forma segura. Não adianta manter o log junto da máquina que pode falhar ou ser perdida. Em sistemas reais, logs também precisam ser protegidos, replicados e usados para recuperação.

---

# 17. Consenso distribuído

## 17.1 Problema

O consenso distribuído busca fazer vários computadores chegarem a uma decisão única.

A pergunta central é:

```text
Como fazer vários nós decidirem o mesmo valor final?
```

Mesmo com falhas, atrasos ou mensagens diferentes, os nós devem chegar a uma decisão comum.

Exemplos de decisão:

```text
Qual operação será aplicada?
Qual servidor será líder?
Qual bloco será adicionado à blockchain?
Qual estado do banco de dados é o correto?
```

---

## 17.2 Objetivos do consenso

Os objetivos principais são:

* garantir acordo;
* tolerar falhas;
* evitar inconsistência;
* coordenar ações;
* definir uma decisão única;
* manter uma visão comum do estado.

Em termos simples:

```text
Transformar várias máquinas independentes em um sistema de decisão única.
```

---

## 17.3 Maioria

Muitos algoritmos de consenso usam a ideia de maioria.

Exemplo com cinco nós:

```text
S1, S2, S3, S4, S5
```

A maioria é:

```text
3 nós
```

Assim, uma decisão pode ser aceita se pelo menos três nós confirmarem.

Dependendo da aplicação, pode-se exigir:

* maioria;
* todos os nós;
* quórum específico.

A escolha depende do nível de consistência e tolerância a falhas desejado.

---

## 17.4 Onde consenso é usado?

Algoritmos de consenso aparecem em:

* bancos de dados distribuídos;
* sistemas de armazenamento em nuvem;
* blockchain;
* sistemas financeiros;
* sistemas de coordenação de serviços;
* plataformas de grande escala;
* clusters de servidores;
* replicação de logs.

Na blockchain, por exemplo, os pares precisam concordar sobre qual bloco ou transação será aceito.

---

# 18. Algoritmo Raft

## 18.1 Ideia principal

O Raft é um algoritmo de consenso distribuído.

Seu objetivo é:

```text
Fazer vários servidores chegarem à mesma decisão.
```

Ele foi criado para ser mais simples de entender do que outros algoritmos de consenso, como Paxos.

A ideia principal é:

```text
Um servidor lidera, os demais seguem.
```

---

## 18.2 Papéis no Raft

No Raft, os servidores podem assumir papéis como:

| Papel     | Função                                           |
| --------- | ------------------------------------------------ |
| Líder     | Coordena as operações                            |
| Seguidor  | Recebe e replica o que o líder envia             |
| Candidato | Participa de uma eleição para tentar virar líder |

Em uma situação normal, existe apenas um líder por vez.

---

## 18.3 Funcionamento do Raft

O funcionamento simplificado pode ser dividido em cinco etapas.

### 1. Eleição de líder

Os servidores votam, e um deles é escolhido como líder.

```text
S1 solicita votos.
S2, S3, S4 e S5 votam.
Se S1 receber maioria, vira líder.
```

### 2. Recebimento de comando

O cliente envia uma operação para o líder.

```text
Cliente → Líder
```

Exemplo:

```text
depositar_100
```

### 3. Replicação do log

O líder registra a operação no seu log e envia a nova entrada aos seguidores.

```text
Líder → Seguidores
```

### 4. Confirmação da maioria

Os seguidores confirmam que receberam e registraram a operação.

Se a maioria confirmar, a operação é validada.

```text
Se maioria confirmou:
    operação é commitada
```

### 5. Tolerância a falhas

Se o líder falhar, uma nova eleição é iniciada.

```text
Líder falhou → seguidores suspeitam → nova eleição → novo líder
```

---

## 18.4 Heartbeats

O líder envia mensagens periódicas para mostrar que está ativo.

Essas mensagens são chamadas de forma geral de mensagens de vida ou `heartbeats`.

A ideia é:

```text
Líder envia sinais periódicos.
Seguidores recebem e sabem que ele continua ativo.
Se os sinais param, seguidores suspeitam de falha.
```

Esse mecanismo ajuda a detectar a falha do líder, mas também gera custo de comunicação.

Mais mensagens na rede podem significar mais overhead.

---

## 18.5 Analogia do grupo de trabalho

Uma analogia usada na aula foi pensar em um trabalho em grupo.

Um integrante organiza as decisões e repassa aos demais. Os demais seguem a orientação e confirmam.

```text
Professor/coordenador do grupo = líder
Integrantes = seguidores
Decisão repassada = entrada de log
Confirmações = votos/acks
```

No Raft, o líder coordena as decisões, mas depende das confirmações dos seguidores.

---

## 18.6 Potencialidades do Raft

O Raft possui algumas vantagens importantes:

* é mais fácil de entender que outros algoritmos de consenso;
* organiza claramente líder e seguidores;
* permite replicar logs;
* usa maioria para validar operações;
* tolera falha de alguns nós;
* permite nova eleição se o líder falhar;
* ajuda a manter uma decisão comum entre vários servidores.

---

## 18.7 Limitações do Raft

Algumas limitações:

* depende de troca constante de mensagens;
* o líder é um ponto de coordenação importante;
* se o líder falha, o sistema precisa detectar e eleger outro;
* se não houver maioria, o sistema não consegue progredir;
* mensagens atrasadas podem gerar suspeitas incorretas de falha;
* uma implementação completa é mais complexa do que a versão didática.

---

## 18.8 Vulnerabilidades e visão de segurança

Sob a perspectiva de cibersegurança, alguns pontos exigem atenção:

### Mensagens perdidas

Se mensagens se perdem, o consenso pode não ser alcançado.

```text
Sem maioria, não há decisão.
```

### Mensagens atrasadas

Atrasos podem fazer o sistema suspeitar que um nó falhou.

### Líder comprometido

Se o líder for comprometido, pode tentar coordenar operações indevidas.

### Mensagens modificadas

Se uma mensagem for alterada durante a comunicação, seguidores podem registrar dados incorretos.

### Nós maliciosos

O Raft tradicional não foi projetado para lidar com falhas bizantinas, isto é, nós que mentem, enviam informações contraditórias ou agem maliciosamente.

Nesse tipo de cenário, é preciso pensar em mecanismos adicionais, como:

* autenticação;
* assinatura digital;
* criptografia;
* hash;
* HMAC;
* TLS;
* controle de integridade;
* monitoramento;
* algoritmos tolerantes a falhas bizantinas.

---

# 19. Relação com blockchain

A aula também conectou os algoritmos distribuídos com blockchain.

Blockchain usa fortemente conceitos de sistemas distribuídos, como:

* rede P2P;
* replicação;
* consenso;
* ordenação de transações;
* tolerância a falhas;
* validação distribuída;
* ledger distribuído.

Em uma blockchain, não existe um servidor central único dizendo o que é verdadeiro. Os nós precisam chegar a um acordo sobre o estado da rede.

Isso torna o consenso essencial.

Exemplo:

```text
Qual bloco será aceito?
Qual transação é válida?
Qual cadeia representa o estado correto?
```

---

# 20. Relação com bancos digitais e Pix

A aula também usou exemplos de sistemas financeiros.

Em uma transferência bancária, a ordem das operações importa:

```text
1. Debitar da conta A.
2. Creditar na conta B.
```

Se a ordem for invertida ou se uma operação ocorrer sem a outra, o sistema pode ficar inconsistente.

Por isso, sistemas financeiros dependem de:

* ordenação de eventos;
* atomicidade;
* consistência;
* isolamento;
* durabilidade;
* consenso;
* replicação;
* logs;
* controle de concorrência.

---

# 21. Relação com Google Docs

O Google Docs foi usado como exemplo de aplicação colaborativa.

Várias pessoas podem editar o mesmo documento ao mesmo tempo. Cada pessoa pode ver atualizações em momentos diferentes, dependendo da rede, da latência e da sincronização.

Isso mostra que:

```text
Não existe uma visão instantânea e perfeita de tudo para todos.
```

O sistema precisa coordenar alterações, ordenar eventos e sincronizar estados para que o documento final permaneça consistente.

---

# 22. Relação com WhatsApp

O WhatsApp foi usado como exemplo para explicar mensagens e ordenação de eventos.

Em uma conversa, a ordem das mensagens importa.

Exemplo:

```text
Pessoa A: Tudo bem?
Pessoa B: Estou bem.
```

Se a resposta chegasse antes da pergunta, a conversa ficaria confusa.

Por isso, aplicações de mensagens precisam ordenar eventos e lidar com atrasos da rede.

---

# 23. Relação com Netflix e serviços em nuvem

Serviços como Netflix e plataformas em nuvem usam replicação e distribuição para atender muitos usuários ao mesmo tempo.

A replicação ajuda a:

* aumentar disponibilidade;
* reduzir latência;
* distribuir carga;
* manter serviço funcionando mesmo com falhas parciais.

Mas também cria o desafio de manter as cópias sincronizadas.

---

# 24. Relação com DDoS e segurança

A aula também abordou a dificuldade de saber se uma máquina falhou ou está apenas sobrecarregada.

Esse ponto se relaciona com ataques de negação de serviço.

Em um ataque DDoS, muitas requisições são enviadas a um servidor ao mesmo tempo. O servidor pode ficar tão sobrecarregado que deixa de responder corretamente.

Do ponto de vista de outro nó do sistema distribuído, pode parecer que o servidor falhou.

Mas, na verdade, ele pode estar apenas sobrecarregado.

Essa diferença é importante para segurança porque:

```text
Falha aparente nem sempre significa falha real.
```

Pode ser:

* congestionamento;
* lentidão;
* ataque;
* sobrecarga;
* falha de rede;
* falha de máquina.

---

# 25. Ponto importante: algoritmos se combinam

Um dos pontos mais importantes da aula foi perceber que um algoritmo sozinho nem sempre resolve tudo.

Exemplos:

```text
Lamport ordena eventos, mas não controla acesso concorrente.
Token controla região crítica, mas pode precisar de eleição se o token for perdido.
Primary-Backup replica dados, mas pode precisar de eleição se o primary falhar.
Raft faz consenso, mas pode precisar de mecanismos de segurança para mensagens maliciosas.
```

Ou seja:

```text
Problemas distribuídos frequentemente exigem combinação de algoritmos.
```

---

# 26. Atividade proposta pelo professor

A atividade proposta foi:

```text
Escolher um algoritmo distribuído qualquer,
implementar uma simulação do funcionamento
e fazer um relato destacando potencialidades, limitações e vulnerabilidades.
```

O professor enfatizou que a implementação não precisa ser complexa, com interface gráfica ou muitos nós.

O objetivo principal é:

* entender os passos do algoritmo;
* simular seu funcionamento;
* observar onde ele funciona bem;
* observar onde ele pode falhar;
* refletir sobre fragilidades e ameaças;
* pensar em possíveis mitigações.

A atividade deve seguir a lógica:

```text
1. Escolher um algoritmo.
2. Implementar uma simulação simples.
3. Observar o comportamento.
4. Relatar potencialidades.
5. Relatar limitações.
6. Relatar vulnerabilidades.
7. Sugerir possíveis melhorias ou mitigações.
```

---

# 27. Resumo final da aula

A aula mostrou que sistemas distribuídos trazem benefícios importantes, como disponibilidade, escalabilidade e tolerância a falhas. Porém, também criam vários problemas, como ausência de relógio global, ausência de memória compartilhada, mensagens atrasadas, mensagens perdidas, falhas independentes e dificuldade de manter consistência.

Para lidar com esses problemas, existem diferentes classes de algoritmos distribuídos:

```text
Ordenação de eventos → Lamport
Sincronização → Token
Replicação → Primary-Backup
Consenso → Raft
```

Cada algoritmo resolve um tipo de problema, mas também possui limitações. Por isso, em sistemas reais, é comum combinar diferentes algoritmos e acrescentar mecanismos de segurança.

A principal ideia da aula foi entender que:

> Sistemas distribuídos funcionam por cooperação entre máquinas independentes, e essa cooperação depende de mensagens, coordenação, consistência e tolerância a falhas.

Do ponto de vista de cibersegurança, é essencial observar que falhas de comunicação, mensagens alteradas, nós maliciosos, sobrecarga, ataques e ausência de autenticação podem comprometer o funcionamento correto desses algoritmos.

---

# 28. Referências indicadas no material da aula

* COULOURIS, G.; DOLLIMORE, J.; KINDBERG, T.; BLAIR, G. **Sistemas Distribuídos: Conceitos e Projeto**. 5. ed. Porto Alegre: Bookman, 2013.
* KSHEMKALYANI, A. D.; SINGHAL, M. **Distributed Computing: Principles, Algorithms, and Systems**. New York: Cambridge University Press, 2008.
* TANENBAUM, A. S.; VAN STEEN, M. **Sistemas Distribuídos: Princípios e Paradigmas**. 2. ed. São Paulo: Pearson Prentice Hall, 2007.
* TEL, G. **Introduction to Distributed Algorithms**. 2. ed. Cambridge: Cambridge University Press, 2000.
