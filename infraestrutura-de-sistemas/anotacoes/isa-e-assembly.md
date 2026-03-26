# Infraestrutura de Sistemas — 13/03

## Visão geral

Foi visto anteriormente como os componentes de hardware são abstraídos. Agora, o foco é descer um pouco mais o nível de abstração, indo para uma camada abaixo do sistema operacional, para entender como as coisas acontecem “debaixo do capô”.

Quando se fala em linguagem de alto nível, não se está falando necessariamente de Python, Low Code ou No Code, mas de linguagens como C e talvez Pascal. Essas linguagens são mais próximas da linguagem humana, mais legíveis e mais fáceis de entender.

Já a linguagem Assembly é uma linguagem de montagem que modela e reflete como os comandos são enviados de forma mais direta ao hardware.

Diferente das linguagens de alto nível, em que normalmente usamos bibliotecas e recursos pré-programados do sistema operacional, em baixo nível a manipulação do hardware é mais direta.

## Ponteiros

Um ponteiro aponta para um endereço de memória.

Na prática, ele armazena o endereço para o qual aponta, permitindo acessar ou alterar o conteúdo daquela posição de memória.

## Compilação e linking

O processo de compilação transforma o código-fonte em código objeto binário.

Depois disso, ocorre a etapa de **linkagem**, que une os objetos gerados para criar um executável de fato, que fará referência às syscalls do sistema operacional.

## Compilação vs. interpretação

Também existe o processo de interpretação.

Ele é parecido com a compilação no sentido de traduzir instruções, mas não acontece de uma vez do início ao fim. A interpretação ocorre instrução por instrução, em tempo de execução.

### Diferença principal

- **Compilação:** converte o programa inteiro de um nível para outro antes da execução.
- **Interpretação:** converte e executa linha por linha, durante a execução.

### Qual tem mais overhead durante a execução?

A interpretação.

A interpretação tem mais overhead durante a execução porque a tradução acontece em tempo real, instrução por instrução.

Já a compilação desloca essa sobrecarga para antes da execução.

### Comparação prática

- Em um programa interpretado, o início da execução é quase imediato.
- Porém, cada instrução executada sofre o custo da interpretação.
- Em um programa compilado, há um tempo maior de preparação antes da execução.
- Em compensação, durante a execução o custo é menor, pois o código já está traduzido.

## Arquitetura multinível

Quando falamos em arquitetura multinível, estamos falando de camadas de abstração.

Cada camada esconde a complexidade da camada inferior e fornece uma interface mais simples para a camada superior.

### Camadas

#### Hardware
Na base, temos o hardware propriamente dito:
- circuitos
- registradores
- memória
- barramentos

#### Microarquitetura
Logo acima, temos a microarquitetura, que corresponde à forma como o hardware implementa o conjunto de instruções.

#### ISA
Depois, temos a **ISA** (*Instruction Set Architecture*), ou Arquitetura de Conjunto de Instruções.

A ISA define:
- quais instruções o processador entende
- como essas instruções são codificadas

Exemplos de instruções:
- `ADD`
- `SUB`
- `MOV`

#### Assembly
Acima da ISA, temos a linguagem Assembly, que é uma representação simbólica dessas instruções.

#### Linguagens de alto nível
Depois vêm as linguagens de alto nível:
- C
- Python
- Java

Essas linguagens abstraem ainda mais as operações realizadas pela máquina.

#### Aplicações
No topo, está o usuário interagindo com aplicações.

### Ideia central

Cada nível abstrai o nível inferior.

Por isso, ao programar em Python, por exemplo, não é necessário saber como funciona um registrador.

Mas, ao programar em Assembly, esse conhecimento já passa a ser importante.

Cada nível se comunica com o outro por meio de um contrato, chamado de **interface**.

Para sair de linguagens orientadas a problema, como C e Java, é necessário traduzir as instruções para linguagem de montagem e, depois, para código de máquina.

Em alguns casos, como em sistemas embarcados, pode ser possível acessar o hardware sem passar totalmente pelo sistema operacional. Ainda assim, esse não é o cenário ideal, porque o sistema operacional existe para gerenciar e proteger o hardware. :contentReference[oaicite:1]{index=1}

## ISA

A ISA é extremamente importante porque define o contrato entre hardware e software.

Se um programa em Assembly for escrito para a arquitetura **x86**, ele não rodará em **ARM**, por exemplo, porque o conjunto de instruções é diferente.

Mesmo linguagens de alto nível dependem disso, porque, no final, tudo precisa ser traduzido para instruções compatíveis com a ISA da máquina.

O Assembly é, portanto, uma representação quase direta dessas instruções.

### Exemplos

- `MOV`: move dados entre registradores ou entre memória e registradores
- `ADD`: soma valores

Essas operações geralmente acontecem em **registradores**, que são áreas de memória extremamente rápidas dentro do processador.

Diferente de variáveis em C, que podem estar na RAM, em Assembly frequentemente trabalhamos diretamente com registradores.

Isso oferece mais controle, mas também mais responsabilidade. :contentReference[oaicite:2]{index=2}

## Níveis da arquitetura

### Nível 5 — Linguagem de alto nível

Nesse nível, não há preocupação direta com:
- registradores
- endereços de memória
- detalhes internos do hardware

### Nível 4 — Assembly

No Assembly, trabalhamos com operações mais próximas do hardware, como:

- carregar valor
- somar
- armazenar

Não existem estruturas como:
- `if`
- `for`
- `while`

Tudo é feito com:
- comparações
- saltos (`jumps`)

Isso se aproxima da lógica de um `goto`, o que pode gerar o chamado **código espaguete**, isto é, um fluxo difícil de entender e manter.

### Nível 3 — Sistema Operacional

Aqui entram as chamadas de sistema, como:

- criar processos
- manipular arquivos
- acessar dispositivos
- comunicação entre processos

Nesse nível, o programa não acessa diretamente a memória ou os dispositivos: ele faz solicitações ao sistema operacional.

### Nível 2 — ISA

Aqui temos as instruções em binário.

Exemplos:
- operação → sequência de bits
- endereço → sequência de bits
- dados → sequência de bits

Cada arquitetura interpreta esses bits de forma própria.

### Nível 1 — Microarquitetura

Aqui entram:
- CPU
- ALU
- registradores
- caminhos de dados (*datapath*)

#### Clock da CPU
- `1 Hz` → 1 instrução por segundo
- `1 MHz` → 1 milhão de instruções por segundo

### Nível 0 — Físico

No nível físico estão:
- transistores
- portas lógicas (`AND`, `OR`, `XOR` etc.) :contentReference[oaicite:3]{index=3}

## Registradores e pilha

### Registradores

Algumas arquiteturas possuem diferentes quantidades de registradores visíveis:

- **x86/x64** → 16 registradores visíveis
- **ARM** → 31 registradores

Também temos registradores especiais:

- **Program Counter (PC):** guarda a próxima instrução a ser executada
- **Stack Pointer (SP):** aponta para o topo da pilha

### Pilha

A pilha guarda o estado da execução.

Exemplos do que pode ficar armazenado nela:
- retorno de função
- endereço de retorno
- variáveis locais

## Tipos de instrução

- **Movimentação:** `load`, `store`, `move`
- **Aritméticas:** `add`, `sub`
- **Lógicas:** `and`, `or`
- **Comparação:** `cmp`

## Modos de endereçamento

- **Imediato:** o valor está na própria instrução
- **Direto:** o valor está em registrador
- **Indireto:** há um ponteiro para uma posição de memória

## CISC vs. RISC

### CISC
- instruções complexas
- pode operar diretamente na memória
- instruções de tamanho variável

### RISC
- instruções simples
- tamanho fixo
- opera apenas com registradores

### Comparação geral

- **RISC** tende a ser mais eficiente energeticamente e, por isso, é muito usado em celulares
- **CISC** domina desktops por questões de compatibilidade histórica

## SIMD — Single Instruction, Multiple Data

SIMD permite aplicar uma mesma instrução sobre vários dados ao mesmo tempo, aumentando o desempenho.

### Usos comuns
- vídeo
- imagem
- inteligência artificial
- cálculos matriciais

## Ciclo de instrução

O ciclo de instrução pode ser resumido em quatro etapas:

1. **Fetch** → busca a instrução na memória
2. **Decode** → interpreta a instrução
3. **Execute** → executa a operação
4. **Store** → salva o resultado

## Controle direto e responsabilidade

Os registradores são recursos limitados.

Por isso, em baixo nível, é preciso saber exatamente:
- onde cada dado está
- quando um valor será sobrescrito
- quando um valor precisa ser preservado

Não é como em Python, em que podemos criar variáveis livremente e contar com muita abstração.

Essa responsabilidade também se relaciona com a pilha de execução.

Se a pilha for corrompida ou “bagunçada”, o fluxo do programa pode ser quebrado completamente, o que abre espaço para vulnerabilidades como **buffer overflow**. :contentReference[oaicite:4]{index=4}

## Interrupções

Interrupções são sinais que fazem o processador parar temporariamente o que está fazendo para tratar um evento.

### Exemplos
- entrada de teclado
- operação de I/O
- eventos internos de execução

Quando ocorre uma interrupção:
1. o processador salva o estado atual
2. desvia para uma rotina específica
3. trata o evento
4. retorna ao ponto em que estava

Isso é fundamental para:
- concorrência
- resposta a eventos externos
- eficiência do sistema

Sem interrupções, o processador teria que verificar constantemente se algo aconteceu, o que seria ineficiente.

## Syscalls e kernel

O sistema operacional usa interrupções para gerenciar recursos.

Quando um programa quer acessar um arquivo, por exemplo, ele não acessa diretamente o hardware.

Em vez disso, ele faz uma **syscall**, isto é, uma chamada ao sistema operacional.

Essa syscall gera uma interrupção controlada, transferindo o controle para o **kernel**.

O kernel executa a operação e depois retorna o resultado ao programa.

Isso garante:
- segurança
- controle
- proteção de recursos críticos :contentReference[oaicite:5]{index=5}

## Segurança em baixo nível

Quando se trabalha em baixo nível, há muito poder de controle, mas esse poder também traz riscos.

Se um programa escreve fora dos limites de memória, ele pode sobrescrever dados importantes.

Por exemplo, pode sobrescrever o endereço de retorno de uma função na pilha.

Se isso acontecer, quando a função terminar, em vez de voltar para o ponto correto, o programa pode desviar para um endereço malicioso.

Esse é um tipo clássico de ataque.

## Buffer overflow

Um dos principais exemplos de problema de segurança em baixo nível é o **buffer overflow**.

Isso acontece quando se escreve mais dados do que o espaço de memória reservado para aquela variável suporta.

### Exemplo

Se um vetor possui 10 posições e o programa escreve 20 valores, os dados excedentes começam a sobrescrever regiões adjacentes da memória.

Essas regiões podem conter informações muito sensíveis, como:
- variáveis locais
- dados de controle
- endereço de retorno de funções

Se um atacante sobrescreve o endereço de retorno, ele consegue alterar o fluxo de execução do programa.

Em vez de retornar ao ponto correto, o programa pode ser desviado para executar código malicioso. :contentReference[oaicite:6]{index=6}

## Execução de código malicioso

Nesse cenário, o atacante sobrescreve a memória de tal forma que um dado passa a ser interpretado como instrução.

Ou seja:
- ele injeta código em uma região de memória
- altera o fluxo do programa para pular para aquela região
- o processador passa a executar aquele conteúdo como se fosse código legítimo

Isso é extremamente perigoso.

## Proteções modernas

Atualmente, existem várias proteções no sistema operacional e no hardware para mitigar esse tipo de problema.

### Áreas executáveis e não executáveis

Uma proteção importante é a separação entre áreas de memória executáveis e não executáveis.

Por exemplo, a pilha normalmente não deve ser executável.

Assim, mesmo que alguém injete código nela, o processador não deveria executá-lo.

### Canários de pilha

Os **canários de pilha** funcionam como um mecanismo de detecção de corrupção de memória.

O nome vem da analogia dos canários levados para minas de carvão: se o ambiente estivesse tóxico, o canário morreria antes e serviria como alerta.

No contexto computacional:
- o compilador insere um valor especial na pilha antes do endereço de retorno
- se ocorrer um buffer overflow, esse valor tende a ser sobrescrito
- antes de retornar da função, o programa verifica se o valor continua intacto
- se não estiver, significa que houve corrupção de memória

Nesse caso, o programa pode ser interrompido antes que o fluxo seja desviado silenciosamente. :contentReference[oaicite:7]{index=7}

## Outras proteções

### ASLR

**ASLR** (*Address Space Layout Randomization*) embaralha os endereços de memória a cada execução do programa.

Isso dificulta a exploração de vulnerabilidades, porque o atacante não consegue prever exatamente onde cada elemento estará carregado em memória.

### NX Bit

O **NX bit** marca certas páginas de memória como não executáveis.

Assim, mesmo que haja injeção de código, a CPU pode impedir sua execução.

Essas técnicas são camadas adicionais de proteção. :contentReference[oaicite:8]{index=8}

## Engenharia reversa

Outra preocupação importante em baixo nível é a **engenharia reversa**.

Quando um programa compilado é distribuído, alguém pode tentar analisar o binário para entender como ele funciona.

Ferramentas de **disassembly** conseguem converter o binário de volta para Assembly.

O resultado não será igual ao código-fonte original, mas ainda assim pode revelar bastante coisa.

Isso pode ser um problema para:
- proteção de propriedade intelectual
- descoberta de vulnerabilidades
- análise indevida do funcionamento do software

## Mitigação de engenharia reversa

Algumas técnicas podem dificultar a engenharia reversa, embora não a impeçam totalmente.

### Exemplos

- ofuscação de código
- embaralhamento de nomes
- embaralhamento de fluxo
- inserção de instruções desnecessárias
- criptografia de partes do código
- carregamento dinâmico

Essas técnicas servem para aumentar a dificuldade de análise. :contentReference[oaicite:9]{index=9}

## Relação com linguagens de alto nível

Voltando à comparação entre alto nível e baixo nível:

Linguagens como C oferecem muito poder e muito controle, mas também exigem muito mais responsabilidade do programador.

Linguagens mais modernas, como Python e Java, já possuem mecanismos internos que evitam muitos desses problemas, como gerenciamento automático de memória.

Por exemplo, em Python, o programador normalmente não precisa se preocupar diretamente com buffer overflow.

Por outro lado, isso vem com custo de desempenho e menor controle sobre a máquina.

## Conclusão

Tudo é uma questão de **trade-off**.

- Mais abstração → mais segurança e facilidade
- Mais baixo nível → mais desempenho e controle, mas também mais responsabilidade

A escolha depende do que se precisa fazer no sistema. :contentReference[oaicite:10]{index=10}
