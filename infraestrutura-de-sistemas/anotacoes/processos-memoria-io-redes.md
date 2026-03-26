# Infraestrutura de Sistemas: Processos, Memória, E/S e Redes - 12/03

## Introdução

Hoje a gente vai continuar falando sobre alguns conceitos importantes de infraestrutura de sistemas, principalmente relacionados à memória, processos, entrada e saída e redes.

Esses conceitos são fundamentais para entender como um sistema operacional funciona internamente.

O sistema operacional precisa gerenciar vários recursos ao mesmo tempo. Entre os principais recursos que ele gerencia estão:

- processador
- memória
- dispositivos de entrada e saída
- armazenamento

Entre esses recursos, a memória é um dos mais importantes.

---

## Processos

Quando a gente fala de processo, a gente está falando de um programa em execução.

Um programa, quando está no disco, é só um arquivo. Quando ele vai ser executado, ele se transforma em um processo.

Esse processo passa a ter:

- área de memória
- estado de execução
- recursos associados
- contexto de execução

Os processos podem criar outros processos.

---

## Threads

As threads têm uma característica muito semelhante aos processos.

Mas elas são uma forma mais leve de execução.

Criar um novo processo tem um custo maior, porque envolve memória, contexto, estruturas internas e gerenciamento de recursos.

As threads surgem justamente para reduzir esse custo.

Elas ficam ligadas diretamente ao processo que as criou.

Assim, diferentes threads podem executar partes do mesmo processo, compartilhando os recursos dele.

Isso permite uma melhor gerência da CPU e evita que ela fique ociosa.

---

## Estados dos processos

Um processo pode passar por vários estados ao longo da sua execução.

Os estados mais comuns são:

- novo
- pronto
- executando
- esperando
- finalizado

### Novo

Quando o processo é criado, ele entra no estado novo.

### Pronto

Depois ele pode ir para o estado pronto, ou seja, está apto para executar, mas ainda não está usando a CPU.

### Executando

Quando o escalonador escolhe esse processo, ele vai para o estado executando.

### Esperando

Se ele precisar realizar alguma operação de entrada e saída, pode ir para o estado esperando.

### Finalizado

Quando termina sua execução, ele vai para o estado finalizado.

---

## Filas de processos

Os processos ficam organizados em filas dentro do sistema operacional.

Existe, por exemplo, a fila de prontos, que contém os processos que estão aptos a executar.

Também podem existir filas associadas a dispositivos de entrada e saída.

---

## Escalonadores

Existem diferentes tipos de escalonadores.

### Escalonador de curto prazo

É o que escolhe qual processo vai usar a CPU.

Ele atua sobre a fila de prontos.

### Escalonador de longo prazo

Gerencia a fila de jobs.

Decide quais processos entram no sistema e vão para a memória.

### Escalonador de médio prazo

Trabalha com um mix de processos na memória.

Ele pode retirar processos da memória principal e levá-los de volta para o disco.

Esse mecanismo está relacionado com swap.

---

## Chamadas de sistema e modos de execução

As chamadas de sistema permitem que a CPU saia do modo usuário para o modo kernel.

### Modo usuário

Nesse modo, a CPU está executando um processo criado pelo usuário.

Há limitação de privilégios.

### Modo kernel

Nesse modo, a CPU tem privilégios maiores e pode acessar diretamente recursos do sistema e dispositivos de hardware.

---

## Memória principal

A memória principal é a memória RAM.

É nela que os processos precisam estar carregados para executar.

Ela é uma memória volátil, ou seja, quando a energia é desligada, os dados são perdidos.

A memória é a área onde ficam armazenados dados e instruções dos processos que estão sendo executados pela CPU.

Essa memória precisa ser rápida para permitir a troca constante de informações entre memória e CPU.

Por ser mais rápida, ela também é mais cara.

Por isso, é preciso gerenciar essa limitação da memória.

---

## Inicialização do sistema operacional

Uma das características do sistema operacional é que ele precisa de um programa de inicialização.

Há uma área reservada que, quando o computador é ligado, carrega esse programa para inicializar o sistema operacional.

---

## Espaço de trabalho do sistema operacional

A memória consiste em um grande array de bytes, cada um com seu próprio endereço.

Os processos são carregados para serem executados nesse espaço.

Cada processo tem um espaço de memória separado.

Esse espaço define o intervalo de endereços que o processo pode acessar.

Isso garante proteção em relação ao acesso de um processo à área de memória de outro processo.

Há também um espaço sempre reservado para o sistema operacional.

---

## Ciclo de execução típico

A CPU busca uma instrução na memória.

Depois essa instrução é decodificada.

Em seguida, ela é executada.

Esse ciclo se repete continuamente durante a execução dos programas.

Uma forma de otimizar as consultas constantes à memória é o uso de memórias cache, que são mais velozes, mas também mais caras.

---

## Endereços lógicos e físicos

Os programas trabalham com endereços lógicos, também chamados de endereços simbólicos ou virtuais.

Esses endereços precisam ser convertidos em endereços físicos, que são os endereços reais na memória.

Quem faz esse gerenciamento é a MMU — Unidade de Gerenciamento de Memória.

### Exemplo

Se a base for 14000 e o programa acessar o endereço lógico 346, o endereço físico será:

14000 + 346 = 14346

Esse mecanismo permite que o processo seja movido de lugar na memória sem alterar o programa.

---

## Vinculação de endereços

O ciclo do processo é basicamente este:

1. o programa está no disco
2. quando ele precisa executar, vira um processo
3. esse processo vai para uma fila
4. quando houver espaço, ele vai para a memória
5. depois entra em execução

Os programas usam endereços lógicos, que depois são traduzidos para endereços físicos.

---

## Carga dinâmica

A memória é limitada.

Se um programa for maior do que a memória disponível, não é possível carregar tudo de uma vez.

Uma forma de resolver isso é a carga dinâmica.

Ela permite trazer para a memória apenas a parte do programa que está sendo usada naquele momento.

Os programas são muitas vezes modulares, então partes podem ficar no disco enquanto outras ficam na memória principal.

---

## Memória virtual

Na memória virtual, existe a memória principal e uma memória de retaguarda, normalmente no disco.

Essa memória de retaguarda funciona como extensão da memória principal.

Mesmo assim, tudo o que vai executar precisa ir primeiro para a memória principal e depois para a CPU.

---

## Swap

Um processo precisa estar na memória para ser executado.

Mas ele pode ser transferido temporariamente para a memória de retaguarda e depois trazido de volta à memória principal para continuar sua execução.

Isso é o swap.

O sistema mantém uma fila de prontos com processos que podem estar tanto na memória principal quanto na memória de retaguarda.

O despachante verifica se o processo escolhido pelo escalonador está na memória.

Se não estiver, ele pode remover outro processo da memória, trazer o processo desejado e então realizar a troca de contexto.

O tempo de mudança de contexto é alto e isso reduz efetivamente o grau de multiprogramação.

---

## Alocação contígua de memória

Na alocação contígua, cada processo ocupa uma única seção de memória que é contínua.

A memória principal precisa acomodar tanto o sistema operacional quanto os processos de usuário.

Ao longo do tempo, surgem brechas entre os processos.

Pode acontecer de um processo novo não caber em nenhum dos espaços livres isoladamente, mesmo que a soma deles seja suficiente.

Isso gera fragmentação externa.

---

## Fragmentação externa

A fragmentação externa acontece quando existem várias brechas de memória e, se forem somadas, elas seriam suficientes para um processo, mas como não são contíguas, o processo não cabe.

Uma solução seria a compactação, mas ela é muito cara.

---

## Fragmentação interna

A fragmentação interna acontece quando usamos partições de tamanho fixo.

Se um processo é menor que a partição alocada para ele, parte do espaço fica desperdiçada e não pode ser usada por outro processo.

---

## Estratégias de seleção de brechas livres

### First Fit

Aloca a primeira brecha que seja suficientemente grande.

A busca pode começar do início da lista ou de onde a busca anterior terminou.

A vantagem é que geralmente a busca é rápida.

### Best Fit

Escolhe a menor brecha possível que ainda comporte o processo.

A ideia é desperdiçar menos espaço.

### Worst Fit

Escolhe a maior brecha disponível.

A ideia é fazer sobrar uma brecha grande depois da alocação.

---

## Segmentação

Tanto a segmentação quanto a paginação são formas de tradução de endereços.

Na segmentação, o código é dividido em partes lógicas.

Quando o processo é criado, ele pode ser dividido em vários segmentos.

Esses segmentos representam partes diferentes do programa e podem ser distribuídos em diferentes regiões da memória.

---

## Paginação

A paginação é uma forma de endereçamento que permite que um processo não precise ficar em um único espaço contínuo da memória.

Ela evita o problema da fragmentação externa.

Na paginação:

- a memória lógica é dividida em páginas
- a memória física é dividida em quadros ou frames

As páginas do processo podem ser carregadas em qualquer frame disponível da memória.

Assim, o processo não precisa ocupar um único bloco contínuo.

### Tabela de páginas

A tabela de páginas mantém o mapeamento entre páginas do processo e frames da memória física.

### Vantagem da paginação

A principal vantagem é evitar a fragmentação externa.

---

## Memória de retaguarda e fragmentação do disco

A memória de retaguarda normalmente é o disco.

Quando processos e arquivos vão sendo gravados e apagados, também podem surgir buracos na memória secundária.

Isso lembra o processo de fragmentação de disco.

A desfragmentação reorganiza os dados, mas é um processo demorado.

---

## Entrada e Saída (I/O)

Quando a gente fala de entrada e saída, está falando da comunicação entre o computador e dispositivos externos.

Muitas vezes, quando um processo está esperando, ele está aguardando uma operação de entrada e saída.

Exemplos de dispositivos:

- teclado
- mouse
- monitor
- impressora
- disco
- rede

O sistema operacional precisa gerenciar todos esses dispositivos.

---

## Drivers

Os drivers são os softwares que permitem ao sistema operacional se comunicar com o hardware.

Hoje em dia muitos dispositivos funcionam automaticamente porque o sistema operacional já tem drivers adequados.

Mas alguns dispositivos ainda precisam de drivers específicos.

---

## Controladores de dispositivos

Os controladores fazem a comunicação entre o sistema operacional e o dispositivo físico.

O sistema operacional envia comandos, e o controlador executa as operações no dispositivo.

---

## Técnicas de I/O

### Programmed I/O (Polling)

A CPU envia um comando ao dispositivo e fica verificando o tempo todo se a operação terminou.

Isso é chamado de busy waiting.

A CPU fica ocupada esperando.

### I/O por interrupção

A CPU envia o comando e continua executando outras tarefas.

Quando o dispositivo termina, ele envia uma interrupção, e a CPU trata essa interrupção.

É mais eficiente que o polling.

### DMA — Direct Memory Access

No DMA, a transferência de dados ocorre diretamente entre o dispositivo e a memória.

A CPU apenas configura o controlador DMA.

Depois disso, o controlador faz a transferência.

Isso melhora bastante o desempenho.

---

## Redes de computadores

Uma rede de computadores é um conjunto de dispositivos interconectados que podem compartilhar informações, recursos e serviços.

Hoje praticamente todos os sistemas dependem de redes.

---

## Funções da rede

A rede permite:

- compartilhamento de recursos
- compartilhamento de informações
- comunicação entre usuários
- acesso remoto a sistemas

---

## Tipos de rede

### LAN — Local Area Network

Rede local, normalmente em uma casa, prédio, empresa ou laboratório.

### MAN — Metropolitan Area Network

Rede metropolitana, cobrindo uma área maior, como uma cidade.

### WAN — Wide Area Network

Rede de longa distância, cobrindo áreas muito maiores.

A internet é um exemplo.

---

## Dispositivos de rede

### Switch

Conecta dispositivos dentro da mesma rede local.

### Roteador

Conecta redes diferentes e encaminha os pacotes para o destino correto.

---

## Endereço IP

Cada dispositivo na rede possui um identificador chamado endereço IP.

---

## DNS

Quando a gente digita um endereço como `site.com`, o navegador precisa descobrir qual é o IP correspondente.

Isso é feito pelo DNS — Domain Name System.

O DNS traduz nomes de domínio em endereços IP.

---

## HTTPS e certificados

No HTTPS, o site utiliza um certificado digital.

Esse certificado garante:

- autenticidade
- criptografia
- integridade da comunicação

O navegador verifica o certificado antes de prosseguir com a comunicação.

---

## Encerramento

Esses conceitos são a base para entender o funcionamento de sistemas operacionais modernos.

Hoje a gente revisou:

- processos
- threads
- escalonadores
- memória principal
- endereços lógicos e físicos
- carga dinâmica
- memória virtual
- swap
- alocação contígua
- fragmentação
- segmentação
- paginação
- entrada e saída
- drivers
- redes

Na próxima aula, a gente continua a partir desses conceitos.
