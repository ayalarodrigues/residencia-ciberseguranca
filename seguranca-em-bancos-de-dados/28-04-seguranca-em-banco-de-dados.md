# Segurança em Banco de Dados — Aula 28/04

> **Disciplina:** Segurança em Banco de Dados  
> **Tema da aula:** arquivo de log, recuperação, vulnerabilidades internas do SGBD, gerenciador de buffers, armazenamento físico, arquiteturas distribuídas, replicação, protocolos de consenso, falhas bizantinas, RAID, disponibilidade, auditoria e criptografia.  

---

## 1. Retomada da aula anterior

A aula iniciou com uma retomada dos componentes de um sistema de banco de dados e da importância de conhecer a arquitetura interna do SGBD para identificar vulnerabilidades.

O professor reforçou que, para trabalhar com segurança em banco de dados, não basta conhecer ataques que já aconteceram. É necessário entender os componentes internos do sistema para conseguir imaginar **novas possibilidades de ataque**.

A lógica apresentada foi:

1. conhecer a arquitetura e os componentes do SGBD;
2. entender como esses componentes manipulam, armazenam e recuperam dados;
3. identificar pontos de vulnerabilidade;
4. imaginar ataques possíveis;
5. propor mecanismos de mitigação.

Foram citados exemplos de incidentes reais e empresas envolvidas em problemas de disponibilidade, vazamento ou ataques, como Uber, Facebook e Dyn. O professor também comentou que ataques a provedores de infraestrutura, como provedores de DNS, podem derrubar indiretamente serviços como Netflix, GitHub e Twitter, pois esses serviços dependem da infraestrutura atacada.

Também foi citada a configuração incorreta de serviços em nuvem como uma fonte importante de vulnerabilidade. Muitas vezes, o problema não está necessariamente no provedor de nuvem, mas na forma como o cliente configurou o serviço. Mesmo assim, o professor observou que, ao oferecer um serviço, o provedor também precisa se preocupar em orientar e viabilizar configurações seguras.

---

## 2. Benchmark e comparação de desempenho

Antes de retomar os componentes do SGBD, o professor explicou o conceito de **benchmark**.

Um benchmark é um recurso usado para fazer comparações entre sistemas computacionais. Existem benchmarks para diferentes áreas, como redes e bancos de dados.

No contexto de banco de dados, um benchmark permite comparar a performance de diferentes SGBDs ou diferentes configurações de um mesmo SGBD diante de uma determinada carga de trabalho.

O professor mencionou o **TPC-H**, um benchmark conhecido para consultas analíticas. Ele foi usado como base em exemplos de consultas e comparação de desempenho.

Na aula anterior, uma consulta no PostgreSQL havia sido executada em aproximadamente 13 segundos. No SQL Server, entretanto, a execução estava mais lenta. O professor explicou que isso não necessariamente significava que o SQL Server era pior, pois havia fatores do ambiente influenciando o desempenho, como:

- o SQL Server estava rodando em um ambiente virtualizado/emulado;
- a máquina utilizada era uma máquina Azure;
- havia uso de emulador para compatibilização com arquitetura ARM;
- mesmo com 10 cores e 10 GB de memória, a camada de emulação impactava o desempenho.

Apesar da lentidão, o professor destacou que gosta de usar o SQL Server em aula porque suas ferramentas gráficas e construções didáticas ajudam a mostrar muitos detalhes internos do SGBD.

Também foi reforçada a ideia de que uma subconsulta pode ser tratada, do ponto de vista do processamento, como uma forma de junção. No exemplo citado, o SQL Server estava sofrendo para executar uma consulta que envolvia `orders` e `lineitem`, como se fosse uma junção mais custosa.

---

## 3. Gerenciador de transações e vulnerabilidades de recuperação

Na aula anterior, o professor havia apresentado o **gerenciador de transações** como um componente fundamental do SGBD.

Esse componente está diretamente ligado à garantia de:

- **consistência** dos dados;
- **integridade**;
- **recuperação** em caso de falhas.

Nesta aula, o professor retomou dois pontos importantes:

1. é possível quebrar a integridade dos dados por problemas de concorrência;
2. é possível tornar o banco de dados não recuperável se o arquivo de log for corrompido.

O arquivo de log, portanto, é ao mesmo tempo:

- um mecanismo fundamental de recuperação;
- um ponto de vulnerabilidade.

Se alguém conseguir corromper, manipular ou remover o arquivo de log, pode comprometer a capacidade do banco de dados de se recuperar corretamente após uma falha.

---

## 4. Arquivo de log como mecanismo de recuperação e vulnerabilidade

O professor mostrou o arquivo de log de um banco de dados no SQL Server.

O arquivo de log registra operações necessárias para que o SGBD consiga recuperar o banco de dados após uma falha. Ele não é um arquivo de auditoria no sentido de registrar todos os acessos. Sua finalidade principal é **recuperação**.

Mesmo assim, o arquivo de log pode revelar muitas informações sobre o banco de dados.

O professor explicou que, ao acessar informações do log, é possível observar elementos como:

- início e fim de checkpoints;
- início de transações;
- operações de modificação de linhas;
- identificadores de transações;
- páginas alteradas;
- registros anteriores da mesma transação;
- informações sobre bloqueios;
- dados em hexadecimal relacionados a comandos ou alterações.

A partir dessas informações, um atacante poderia conhecer parte do funcionamento e do estado do banco de dados.

### 4.1 Exemplo didático mostrado em aula

O professor usou um banco de dados pequeno, com uma tabela de aproximadamente 200 tuplas. Mesmo assim, o arquivo de log era grande, pois muitas transações de atualização haviam sido executadas sobre ele.

Isso demonstra que o tamanho do banco de dados não é o único fator relevante para o tamanho do log. Um banco pequeno pode ter um log grande se houver muitas operações de escrita.

---

## 5. Checkpoint no arquivo de log

O professor explicou o conceito de **checkpoint**.

Um checkpoint é um ponto gravado no arquivo de log que indica a partir de onde o processo de recuperação pode começar em caso de falha.

Em termos práticos:

- o SGBD grava um ponto de controle no log;
- esse ponto informa que determinadas alterações anteriores já foram tratadas de forma segura;
- se ocorrer uma falha, o SGBD não precisa reprocessar o log desde o início absoluto;
- ele pode começar a recuperação a partir do último checkpoint relevante.

O professor destacou que, se alguém conseguir manipular o registro de checkpoint, pode inviabilizar ou comprometer o processo de recuperação.

Essa é uma vulnerabilidade importante porque o checkpoint orienta o SGBD sobre onde iniciar a recuperação. Se esse ponto for adulterado, o banco pode não conseguir recuperar corretamente o estado anterior à falha.

---

## 6. Registros de log e operação `Modify Row`

Durante a demonstração, o professor apontou registros de log contendo a operação **Modify Row**.

Essa operação indica que uma linha, ou seja, uma tupla do banco de dados, foi alterada.

A associação feita foi:

```text
Modify Row → uma tupla foi alterada → houve uma operação de UPDATE
```

O professor explicou que o registro de log também pode conter informações em hexadecimal. Se alguém conseguir interpretar esse hexadecimal, poderá descobrir qual comando foi executado ou quais dados foram modificados.

Essa informação é importante para recuperação, mas também cria uma vulnerabilidade, porque permite inferir atividades realizadas no banco.

---

## 7. Log Sequence Number — LSN

Todo registro de log possui um **Log Sequence Number (LSN)**.

O LSN é o identificador do registro de log. Ele também pode aparecer em formato hexadecimal.

A ideia do LSN é representar a ordem de gravação dos registros no log.

Exemplo:

```text
LSN 5 foi gravado antes do LSN 10
```

Com isso, o SGBD consegue saber a ordem em que as operações foram registradas.

O professor reforçou que o arquivo de log é gravado em modo **append**, isto é, os registros são sempre adicionados ao final do arquivo. O log funciona como um arquivo sequencial de registros de operações.

---

## 8. ID da transação e Page ID

Cada operação registrada no log pertence a uma transação. Por isso, o log contém o **ID da transação**.

Quando uma aplicação executa, por exemplo, uma operação de `UPDATE`, essa operação chega ao SGBD associada a uma transação. O gerenciador de transações é responsável por criar e gerenciar esse identificador.

Além do ID da transação, o log também registra o **Page ID**, ou seja, o identificador da página do banco de dados que foi alterada.

Isso é importante porque o SGBD enxerga o banco de dados como um conjunto de páginas. Quando uma alteração ocorre, o log precisa saber em qual página houve modificação para que seja possível recuperar ou desfazer essa alteração posteriormente.

---

## 9. `Prev LSN` e encadeamento dos registros de uma transação

Um campo importante no log é o **Prev LSN**.

Ele aponta para o registro de log anterior da mesma transação.

O professor explicou que, embora o arquivo de log seja sequencial, os registros de uma mesma transação não aparecem necessariamente um ao lado do outro. Isso acontece porque várias transações podem estar sendo executadas concorrentemente.

Exemplo didático:

```text
LSN 5  → escrita da transação T1 sobre a página X
LSN 6  → escrita da transação T2 sobre o objeto Y
LSN 7  → escrita da transação T3 sobre o objeto Z
LSN 8  → nova escrita da transação T1 sobre o objeto W
```

Nesse caso, os registros `LSN 5` e `LSN 8` pertencem à transação `T1`, mas estão separados por registros de outras transações.

Por isso, no registro `LSN 8`, o campo `Prev LSN` apontaria para o `LSN 5`.

A ideia é semelhante a uma lista encadeada para trás:

```text
Registro atual da transação → registro anterior da mesma transação → registro anterior da mesma transação → ...
```

Esse encadeamento é fundamental para o processo de recuperação, especialmente para desfazer operações.

---

## 10. Operações registradas no log: escrita, não leitura

O professor destacou que, no arquivo de log de recuperação, são registradas apenas operações que alteram o estado do banco de dados.

Ou seja, operações de leitura não interessam ao log de recuperação.

```text
SELECT → não altera o estado do banco → normalmente não entra no log de recuperação
INSERT → altera o estado do banco → entra no log
UPDATE → altera o estado do banco → entra no log
DELETE → altera o estado do banco → entra no log
```

A razão é simples: o arquivo de log existe para permitir recuperação. Se uma operação não altera o estado do banco, ela não precisa ser refeita ou desfeita em caso de falha.

Mesmo registrando apenas operações de escrita, o log cresce rapidamente.

---

## 11. Diferença entre log de recuperação e log de auditoria

Uma dúvida levantada em aula foi se seria possível forçar o registro de operações de leitura, especialmente quando envolvem dados sensíveis.

O professor respondeu que o arquivo de log de recuperação não tem essa finalidade. Ele não é um mecanismo de auditoria.

A diferença é:

| Tipo de log | Finalidade principal | Registra leitura? |
|---|---|---|
| Log de recuperação | Recuperar o banco após falhas | Normalmente não |
| Log de auditoria | Rastrear quem acessou ou modificou informações | Pode registrar, se configurado |

O log de auditoria deve ser implementado ou configurado para registrar ações relevantes, como:

- autenticações;
- acessos a dados sensíveis;
- alterações em tabelas críticas;
- comandos administrativos;
- operações suspeitas.

O professor citou um caso de vazamento de dados sensíveis na Receita Federal. A identificação do responsável teria sido possível por meio de auditoria, e não pelo log de recuperação.

---

## 12. `Redo` e `Undo`

O professor explicou que existem duas operações fundamentais no processo de recuperação:

- **Redo**: refazer operações;
- **Undo**: desfazer operações.

### 12.1 Redo

O `Redo` refaz operações já confirmadas que ainda não haviam sido completamente descarregadas em disco no momento da falha.

Ele geralmente trabalha a partir do último checkpoint para frente.

```text
Checkpoint → operações confirmadas depois do checkpoint → refazer no banco
```

### 12.2 Undo

O `Undo` desfaz operações de transações que não chegaram ao `COMMIT`.

Para desfazer, o SGBD precisa percorrer o log de trás para frente. É por isso que o `Prev LSN` é importante.

```text
Último registro da transação → Prev LSN → Prev LSN → ...
```

Assim, o SGBD consegue desfazer as alterações na ordem correta.

### 12.3 Vulnerabilidade associada

As informações necessárias para `Redo` e `Undo` são essenciais para recuperação. Porém, como elas ficam registradas no log, também podem expor detalhes internos do banco.

Por isso, o log é um componente crítico tanto para recuperação quanto para segurança.

---

## 13. Leitura do log no SQL Server

O professor observou que, na demonstração, o que estava sendo lido não era diretamente o arquivo físico de log, mas uma **visão sobre o arquivo de log**.

Isso é um mecanismo de segurança do SQL Server: permitir leitura controlada de determinadas informações sem permitir atualização direta do arquivo.

A ideia é:

- o SGBD precisa proteger o arquivo de log contra alteração direta;
- permitir atualização direta do log comprometeria a recuperação;
- por isso, a visualização é mediada por uma interface/visão.

Mesmo assim, se um atacante conseguir acesso ao sistema operacional e aos arquivos em repouso, ele pode tentar manipular ou remover arquivos do banco.

---

## 14. Dados em repouso e arquivos físicos do banco

O professor retomou a ideia de **dados em repouso**.

Quando o SGBD está em execução, os arquivos físicos do banco estão em uso e, normalmente, não podem ser removidos ou manipulados diretamente.

Quando o SGBD está desligado, ou se o servidor for derrubado, esses arquivos ficam em repouso. Nesse momento, se um atacante tiver acesso ao sistema operacional, ele pode tentar:

- ler arquivos físicos;
- copiar arquivos;
- remover arquivos;
- inserir registros indevidos;
- corromper o arquivo de log;
- comprometer o processo de recuperação.

No SQL Server, foram citados os arquivos:

```text
ESTOQUE.mdf → arquivo de dados
ESTOQUE.ldf → arquivo de log
```

O professor destacou que `.mdf` e `.ldf` são extensões específicas do SQL Server. Outros SGBDs também possuem arquivos de dados e arquivos de log, mas podem usar formatos, extensões e estruturas diferentes.

A ideia geral, porém, vale para qualquer SGBD: se existe mecanismo de recuperação, é necessário haver algum tipo de arquivo de log.

---

## 15. Arquivos de dados e arquivos de log em diferentes modelos de banco

O professor destacou que todo SGBD, independentemente do modelo de dados, precisa lidar com:

- arquivos de dados;
- arquivos de log;
- mecanismo de recuperação.

Isso não vale apenas para bancos relacionais.

Exemplos citados:

- SQL Server;
- PostgreSQL;
- Oracle;
- MongoDB;
- Neo4j.

A diferença está na forma como o SGBD interpreta os dados armazenados:

| Modelo | Como os dados são interpretados |
|---|---|
| Relacional | tabelas, tuplas e atributos |
| Documentos | documentos |
| Grafos | nós, arestas e propriedades |

O mecanismo físico pode envolver arquivos e logs, mas o modelo lógico muda a forma de interpretação.

---

## 16. DoS por bloqueios e tabela interna de locks

O professor apresentou outra vulnerabilidade: a possibilidade de parar o sistema de banco de dados por meio de uma transação que cause muitos bloqueios.

A ideia é a seguinte:

- o SGBD possui estruturas internas para gerenciar bloqueios;
- se uma transação espúria gerar uma quantidade enorme de bloqueios, ela pode inundar essas estruturas;
- outras transações precisam consultar essas estruturas para executar;
- se a estrutura de bloqueios estiver sobrecarregada, o banco pode parar de responder.

Mesmo uma tabela pequena pode ser usada em um ataque se a operação for repetida muitas vezes.

O professor descreveu isso como uma forma de **DoS — Denial of Service**.

```text
Transação espúria → muitos bloqueios → tabela/estrutura de locks inundada → outras transações não conseguem executar → indisponibilidade
```

---

## 17. Gerenciador de buffers

Outro componente estudado foi o **gerenciador de buffers**.

Quando o servidor de banco de dados é iniciado, ele solicita ao sistema operacional uma área de memória principal para carregar seu código e trabalhar. Além disso, ele reserva uma área de memória para uso próprio: a área de buffers.

O professor destacou que o sistema operacional não tem ingerência direta sobre essa área de buffer do SGBD. Ela é gerenciada pelo próprio banco.

A função do gerenciador de buffers é manter em memória principal as páginas do banco de dados que estão sendo mais referenciadas.

A lógica é:

- o banco de dados completo é muito maior que a memória disponível;
- o acesso à memória principal é muito mais rápido que o acesso ao disco;
- logo, o SGBD mantém em memória as páginas mais usadas;
- quando uma página necessária não está no buffer, ela precisa ser lida do disco.

---

## 18. Páginas quentes e política LRU

O SGBD enxerga o banco de dados como um conjunto de **páginas**. A página é a unidade de acesso do banco de dados.

Quando uma página é muito acessada, ela tende a permanecer no buffer. Essas páginas mais referenciadas podem ser chamadas de **páginas quentes**.

Se o buffer estiver cheio e uma nova página precisar ser carregada, o SGBD deve escolher uma página para remover da memória.

A política citada foi a **LRU — Least Recently Used**.

A política LRU remove a página que foi usada há mais tempo.

```text
Buffer cheio + nova página necessária → remover página menos recentemente usada → carregar nova página
```

Essa política é uma estratégia de otimização para reduzir acessos a disco.

---

## 19. Buffer como ponto de vulnerabilidade

O professor explicou que o gerenciador de buffers também pode ser explorado como ponto de vulnerabilidade.

Se um atacante souber quais páginas são muito referenciadas, pode tentar atacá-las.

Exemplos de estratégias citadas:

- fazer muitas requisições a uma página específica;
- manter bloqueios por muito tempo sobre páginas importantes;
- criar transações espúrias com esperas artificiais;
- usar comandos como `WAITFOR DELAY` para manter bloqueios ativos por horas.

Exemplo conceitual:

```sql
-- Exemplo didático de espera artificial em uma transação
WAITFOR DELAY '05:00:00';
```

Se a transação mantém um bloqueio de escrita por muito tempo, outras transações ficam impedidas de acessar o recurso bloqueado.

Isso pode afetar a disponibilidade do SGBD.

---

## 20. Buffer do SGBD e paginação do sistema operacional

O professor comparou o uso de buffer no SGBD com mecanismos do sistema operacional.

No sistema operacional, existe a ideia de paginação e de swap. O sistema operacional também tenta otimizar o uso de memória e disco, mantendo em memória o que é mais utilizado e movendo dados conforme necessário.

A ideia geral de manter dados frequentemente acessados em memória não é exclusiva de SGBDs.

O professor também comentou que, ao formatar um disco, nem todo o espaço fica disponível para o usuário. Parte do espaço é reservado para uso do sistema operacional e do sistema de arquivos.

No caso de discos magnéticos, foi mencionado que as primeiras trilhas têm importância especial, pois o cabeçote fica posicionado no início quando não está acessando dados. Essas áreas podem ser reservadas para estruturas do sistema.

---

## 21. Acesso ao banco de dados por aplicações

A aula também abordou como aplicações acessam o banco de dados.

O professor retomou o exemplo de uma aplicação em C# acessando SQL Server e comparou com Java.

Em Java, por exemplo, é comum usar `ResultSet` para lidar com o resultado de consultas. A consulta SQL pode ser passada como uma string para uma camada de acesso.

Exemplo conceitual:

```java
String sql = "SELECT * FROM aluno WHERE matricula = ?";
ResultSet rs = statement.executeQuery(sql);
```

Essa abordagem exige camadas intermediárias para mapear a consulta da aplicação até o driver nativo do banco.

---

## 22. JDBC, ODBC e drivers de acesso

Foram citadas duas tecnologias importantes de acesso a banco:

- **JDBC**;
- **ODBC**.

### 22.1 JDBC

O JDBC é usado em aplicações Java para acessar bancos de dados. A aplicação envia a consulta como uma string e o JDBC faz o mapeamento para o driver adequado do SGBD.

Exemplo:

```text
Aplicação Java → JDBC → driver do banco → SGBD
```

Se o banco for SQL Server, o JDBC mapeia para o driver do SQL Server. Se for Oracle, mapeia para o driver Oracle.

### 22.2 ODBC

O ODBC tem função semelhante e foi criado pela Microsoft. É um padrão de acesso utilizado em produtos Microsoft e também em diversos outros ambientes.

Exemplo:

```text
Aplicação → ODBC → driver nativo → SGBD
```

### 22.3 DBeaver

O professor também comentou que o DBeaver é uma ferramenta gráfica para acessar bancos de dados. Ele funciona como uma interface semelhante à ferramenta gráfica do SQL Server, mas com suporte a vários SGBDs, como PostgreSQL, MySQL, Oracle e outros.

---

## 23. SQL embutido e pré-compilador

Além da estratégia de passar consultas como strings para métodos da aplicação, o professor apresentou outra forma de acesso: **SQL embutido**.

Nessa abordagem, consultas SQL são colocadas diretamente dentro do código da aplicação.

Isso é comum em sistemas legados, por exemplo, programas em C que acessam SQL Server, Oracle ou outro SGBD.

O problema é que um compilador C não entende SQL diretamente. Se uma consulta SQL for colocada dentro de um programa C, o compilador acusará erro.

Para resolver isso, existe o **pré-compilador**.

A função do pré-compilador é:

1. localizar consultas SQL embutidas no código;
2. transformar essas consultas em chamadas de funções de bibliotecas;
3. gerar um código que o compilador da linguagem consiga compilar;
4. permitir acesso direto ao driver nativo do banco.

A vantagem é ganho de performance, porque algumas camadas intermediárias podem ser eliminadas.

A desvantagem é que esse modelo torna o código mais dependente do SGBD e do processo de pré-compilação.

---

## 24. SQLJ

O professor citou o **SQLJ**, que permite embutir consultas SQL em programas Java.

A ideia é semelhante ao SQL embutido em C, mas integrada ao ambiente Java.

Como Java já passa por um processo de compilação para bytecode executado pela JVM, o SQLJ consegue fazer o mapeamento necessário dentro desse fluxo.

O professor observou que pouca gente conhece esse recurso, mas ele existe como alternativa de acesso a banco de dados em sistemas Java.

---

## 25. Armazenamento físico: páginas, tablespaces e filegroups

O professor retomou a ideia de que o SGBD enxerga o banco de dados como um conjunto de páginas.

Além das tabelas, existem outros objetos importantes no armazenamento físico.

Em SGBDs como Oracle, PostgreSQL e DB2, há o conceito de **tablespace**.

No SQL Server, a funcionalidade equivalente é chamada de **filegroup**.

A ideia é associar objetos lógicos do banco, como tabelas e índices, a áreas de armazenamento físico.

```text
Tabela / Índice → Tablespace ou Filegroup → Arquivo físico em disco
```

### 25.1 Tablespace

Uma tablespace pode estar associada a um ou mais arquivos físicos.

Exemplo conceitual:

```text
Tablespace 2 → arquivo físico A → tabelas associadas
```

Se duas tabelas estão associadas a uma tablespace que possui apenas um arquivo físico, os dados dessas tabelas estarão naquele arquivo.

### 25.2 Filegroup

No SQL Server, o conceito equivalente é o filegroup.

Existe um filegroup padrão chamado **PRIMARY**.

Quando uma tabela é criada, ela pode ser associada a um filegroup. Esse filegroup, por sua vez, está associado a arquivos físicos.

---

## 26. Tablespaces e filegroups como pontos de vulnerabilidade

O professor explicou que conhecer a relação entre objetos lógicos e arquivos físicos pode ser usado em um ataque.

Se um atacante souber que determinadas tabelas estão armazenadas em um arquivo específico, ele não precisa executar um `DROP TABLE` para remover os dados. Ele pode tentar remover diretamente o arquivo físico, caso tenha acesso ao sistema operacional e os dados estejam em repouso.

Exemplo conceitual:

```text
Tabela A e Tabela B → Tablespace X → arquivo dados01.dbf
```

Se o arquivo `dados01.dbf` for removido ou corrompido, os dados dessas tabelas são afetados.

Isso mostra que a segurança do banco de dados não depende apenas de permissões SQL. Também depende de:

- segurança do sistema operacional;
- controle de acesso aos arquivos físicos;
- criptografia dos dados em repouso;
- política de backup;
- monitoramento de alterações nos arquivos do banco.

---

## 27. Página, bloco e setor

O professor diferenciou três unidades de acesso:

| Camada | Unidade de acesso |
|---|---|
| Controladora de disco | Setor |
| Sistema de arquivos | Bloco |
| SGBD | Página |

A controladora de disco trabalha com setores.

O sistema de arquivos trabalha com blocos, que agrupam setores.

O SGBD trabalha com páginas, que são unidades lógicas de acesso aos dados do banco.

Para que o acesso seja eficiente, o tamanho da página do SGBD precisa ser compatível com o tamanho do bloco do sistema de arquivos.

A ideia é evitar situações em que uma página precise ser lida de forma desalinhada em relação aos blocos físicos.

---

## 28. Tamanho de páginas nos SGBDs

No SQL Server, a página tem tamanho fixo de **8 KB**.

O professor comentou que isso se integra bem ao ambiente Windows/NTFS, pois o SQL Server e o sistema de arquivos foram desenvolvidos dentro do ecossistema Microsoft.

No Oracle, como o SGBD pode rodar em diferentes sistemas operacionais, ele permite diferentes tamanhos de página, como:

- 2 KB;
- 4 KB;
- 8 KB;
- 16 KB;
- 32 KB.

A escolha do tamanho da página depende do ambiente, do sistema operacional, do sistema de arquivos e da carga de trabalho.

---

## 29. Tuplas, páginas e BLOBs

Dentro das páginas, em um banco relacional, ficam as tuplas, isto é, as linhas das tabelas.

Normalmente, uma página contém várias tuplas.

No entanto, algumas tuplas podem ser grandes o suficiente para ocupar várias páginas. Isso é comum quando a tabela possui atributos do tipo **BLOB — Binary Large Object**.

Exemplos de BLOBs:

- imagens;
- vídeos;
- documentos binários;
- arquivos grandes armazenados dentro do banco.

Quando uma tupla ocupa várias páginas, há impacto na performance, pois o SGBD precisa acessar mais páginas para recuperar ou manipular o mesmo registro lógico.

O professor também comentou que algumas otimizações não funcionam bem quando uma tabela é pequena demais, por exemplo, quando ela cabe em uma única página.

---

## 30. Object Storage, Block Storage e relação com banco de dados

Durante a aula, surgiu uma pergunta sobre serviços de nuvem que oferecem **Object Storage** e **Block Storage**.

O professor comentou que não conhecia profundamente o conceito no contexto específico de contratação de nuvem, mas relacionou a pergunta com conceitos de armazenamento em banco de dados orientado a objetos e armazenamento em blocos.

A explicação feita em aula foi aproximada:

- em banco de dados orientado a objetos, objetos podem ser grandes e compostos por outros objetos;
- em vez de armazenar o objeto diretamente em uma única página, pode haver uso de identificadores ou ponteiros para localizar os objetos;
- em sistemas distribuídos como Google File System, blocos/chunks podem ter identificadores globais para localização na infraestrutura.

A associação feita foi que o conceito de bloco se aproxima mais da ideia de unidade de armazenamento em baixo nível, enquanto o armazenamento de objetos envolve tratar o dado como uma entidade identificável, possivelmente grande e composta.

---

## 31. Arquiteturas distribuídas e banco de dados centralizado

O professor explicou que, em um banco de dados centralizado, todos os componentes do SGBD estão em uma única máquina.

Já em arquiteturas distribuídas, há componentes ou dados espalhados em diferentes nós.

Uma das arquiteturas distribuídas mais comuns é a **cliente-servidor**.

---

## 32. Arquitetura cliente-servidor e arquitetura em três camadas

Na Engenharia de Software, é comum falar em arquitetura de três camadas:

1. cliente;
2. servidor de aplicação;
3. servidor de banco de dados.

Do ponto de vista do SGBD, o servidor de aplicação é um cliente.

```text
Usuário / Browser → Servidor de aplicação → Servidor de banco de dados
```

O professor explicou que aplicações Web são exemplos cotidianos de arquitetura cliente-servidor.

Quando alguém acessa um serviço da Google pelo navegador, parte da funcionalidade está no cliente, mas o serviço principal roda em servidores remotos.

Em aplicações Web:

- o front-end atua como cliente;
- o back-end atua como servidor;
- o banco de dados fica em outro servidor ou serviço.

A ideia da arquitetura cliente-servidor é distribuir a carga de trabalho entre cliente e servidor.

---

## 33. Vulnerabilidades na arquitetura cliente-servidor

A arquitetura cliente-servidor cria pontos de ataque específicos.

O professor destacou principalmente a **conexão** entre cliente e servidor.

Se o atacante quer descobrir dados processados entre aplicação e banco, ele não precisa necessariamente atacar o cliente nem o servidor diretamente. Ele pode tentar atacar a conexão.

```text
Cliente / aplicação ↔ conexão ↔ SGBD
```

Por isso, a criptografia de dados em trânsito é importante.

O professor mencionou que o SQL Server possui serviço de criptografia para proteger os dados que trafegam na rede.

Outro ponto de ataque é a sobrecarga de requisições.

Se muitos clientes, ou um mesmo cliente, fizerem muitas solicitações ao banco, o SGBD pode ficar sobrecarregado e deixar de atender usuários legítimos.

Isso também caracteriza um ataque de disponibilidade.

---

## 34. Banco de dados paralelo

O professor também citou sistemas de banco de dados paralelos.

A ideia do banco paralelo é usar processadores com muitos núcleos para distribuir a carga de trabalho entre diferentes cores.

Foi mencionada uma arquitetura de processador em “ladrilhos”, em que há vários blocos de processamento, cada um com caches e múltiplos cores.

Para aproveitar esse tipo de arquitetura, o SGBD precisa ser capaz de enxergar e usar paralelismo.

No SQL Server, existe um parâmetro de configuração chamado **grau de paralelismo**.

Exemplo conceitual:

```text
Grau de paralelismo = 5 → o SGBD pode usar até 5 cores para distribuir tarefas
```

O objetivo é melhorar o desempenho da execução de consultas e operações pesadas.

---

## 35. Banco de dados distribuído e banco de dados replicado

O professor diferenciou dois conceitos:

- banco de dados distribuído;
- banco de dados replicado.

### 35.1 Banco de dados distribuído

No banco de dados distribuído, os dados são espalhados entre diferentes nós da rede. Um dado específico fica em um nó, e não necessariamente em todos.

```text
Fragmento A → nó 1
Fragmento B → nó 2
Fragmento C → nó 3
```

### 35.2 Banco de dados replicado

No banco de dados replicado, o mesmo dado pode estar presente em vários nós.

```text
Dado X → réplica 1
Dado X → réplica 2
Dado X → réplica 3
```

A replicação aumenta a disponibilidade e pode ajudar no balanceamento de carga.

---

## 36. Fragmentação por agência: exemplo do Banco do Brasil

Para explicar banco de dados distribuído, o professor usou o exemplo de uma tabela de correntistas do Banco do Brasil.

Não faria sentido manter todos os dados centralizados se eles podem ser fragmentados por critérios relevantes.

Um critério intuitivo citado foi a **agência**.

```text
Correntistas da agência Benfica → nó da agência Benfica
Correntistas da agência Centro → nó da agência Centro
Correntistas da agência Aldeota → nó da agência Aldeota
```

A razão é que a maioria das transações de uma agência tende a envolver clientes daquela própria agência.

Com isso, os dados ficam fisicamente mais próximos das aplicações que os utilizam.

Vantagens:

- redução de latência de rede;
- melhor desempenho;
- menor exposição a ataques pela rede externa;
- melhor distribuição da carga.

O professor também observou que, quando uma transação distribuída precisa acessar dados em vários nós, o nível de complexidade e de vulnerabilidade aumenta.

---

## 37. Vantagens da replicação

A replicação tem duas grandes vantagens citadas em aula:

1. **Disponibilidade**: se uma réplica estiver indisponível, outra pode atender.
2. **Balanceamento de carga**: se uma réplica estiver sobrecarregada, requisições podem ser direcionadas para outras.

```text
Réplica 1 indisponível → usar réplica 2
Réplica 1 sobrecarregada → distribuir requisições entre réplica 2 e réplica 3
```

O professor comentou que existem middlewares capazes de receber requisições e distribuí-las entre réplicas de banco de dados.

Nem sempre o próprio sistema de banco replicado faz balanceamento de carga de forma eficiente. Por isso, ferramentas intermediárias podem ser usadas.

---

## 38. Vantagens da distribuição sobre a replicação

A distribuição também tem vantagens em relação à replicação.

Uma das principais é o custo.

Na replicação, manter várias cópias do mesmo dado exige mais armazenamento e maior controle de atualização.

Na distribuição, os dados são divididos entre nós, permitindo crescimento horizontal.

```text
Crescimento vertical → aumentar memória, CPU e disco de uma máquina
Crescimento horizontal → adicionar mais máquinas ao sistema
```

A distribuição permite escalar adicionando novos nós, em vez de depender apenas de máquinas cada vez mais potentes.

---

## 39. Problema da replicação: propagação de atualizações

O grande problema da replicação aparece nas operações de escrita.

Quando um dado é alterado em uma réplica, essa alteração precisa ser propagada para as demais réplicas.

Exemplo:

```text
Dado X alterado na réplica 1
→ precisa propagar para réplica 2
→ precisa propagar para réplica 3
→ precisa propagar para réplica 4
→ precisa propagar para réplica 5
```

Se a propagação for feita durante a execução da transação, a transação só poderá terminar depois que todas as réplicas forem atualizadas.

Isso aumenta o tempo de execução da transação.

---

## 40. Propagação ávida e propagação preguiçosa

O professor apresentou dois modos de propagação de atualizações em bancos replicados:

- propagação ávida;
- propagação preguiçosa.

### 40.1 Propagação ávida

Na propagação ávida, a atualização é propagada para as outras réplicas durante a execução da transação.

A transação só termina quando a propagação é efetivada nas demais réplicas.

```text
Atualiza X na réplica 1
→ propaga para réplica 2
→ propaga para réplica 3
→ propaga para réplica 4
→ confirma transação
```

Vantagem:

- maior consistência entre as réplicas.

Desvantagem:

- maior tempo de execução da transação;
- menor desempenho;
- maior custo de comunicação.

### 40.2 Propagação preguiçosa

Na propagação preguiçosa, a atualização é aceita primeiro e propagada depois.

```text
Atualiza X na réplica 1
→ confirma transação
→ propaga depois para as outras réplicas
```

Vantagem:

- transação termina mais rapidamente;
- maior disponibilidade;
- menor espera para o usuário.

Desvantagem:

- durante algum tempo, as réplicas podem ficar com valores diferentes;
- há risco de inconsistência temporária.

O professor destacou que a escolha depende da carga de trabalho e do tipo de aplicação.

Se a aplicação pode tolerar inconsistência temporária em troca de disponibilidade, a propagação preguiçosa pode ser adequada. Se a consistência forte for obrigatória, será necessário usar propagação ávida.

---

## 41. Protocolos de consenso

Em sistemas distribuídos, quando uma transação envolve vários nós, é necessário que os participantes cheguem a uma decisão comum.

Essa decisão pode ser:

- confirmar a transação (`COMMIT`);
- abortar a transação (`ABORT`).

Protocolos que permitem essa decisão comum são chamados de **protocolos de consenso**.

O professor citou exemplos:

- **2PC — Two-Phase Commit**;
- **3PC — Three-Phase Commit**;
- **Paxos**;
- **Raft**.

A função desses protocolos é coordenar processos distribuídos para que eles cheguem a uma decisão consistente.

---

## 42. Teorema CAP

O professor relacionou protocolos de consenso com o **Teorema CAP**.

CAP significa:

- **Consistency** — consistência;
- **Availability** — disponibilidade;
- **Partition Tolerance** — tolerância a partição.

O teorema afirma que, em um sistema distribuído, não é possível garantir simultaneamente as três propriedades em todos os cenários, especialmente quando ocorre falha de rede.

Se houver partição de rede, o sistema terá que sacrificar consistência ou disponibilidade.

O professor comentou que o 2PC falha fortemente nesse cenário, enquanto protocolos como Paxos e Raft buscam oferecer garantias mais robustas em grande parte dos casos.

A ideia apresentada foi que Paxos e Raft não eliminam completamente todos os cenários problemáticos, mas reduzem muito a probabilidade de falha em ambientes distribuídos.

---

## 43. Two-Phase Commit — 2PC

O professor explicou o funcionamento do **2PC — Two-Phase Commit**.

Esse protocolo é usado para coordenar uma transação distribuída envolvendo um coordenador e vários participantes.

### 43.1 Coordenador e participantes

O coordenador é o nó que iniciou a transação e recebeu a operação de `COMMIT`.

Os participantes são os nós que executaram operações daquela transação.

Exemplo:

```text
Coordenador → recebeu o COMMIT
Participante P1 → alterou objeto X
Participante P2 → alterou objeto Y
Participante PN → alterou objeto Z
```

Cada nó possui seu próprio arquivo de log. Portanto, a recuperação também é distribuída.

---

## 44. Primeira fase do 2PC: votação

Quando o coordenador recebe a solicitação de `COMMIT`, ele inicia a primeira fase do protocolo.

Ele grava em seu log um registro de início do 2PC, contendo a lista de participantes.

Depois, envia uma mensagem a todos os participantes solicitando voto.

```text
Coordenador → “votem: posso commitar?” → participantes
```

Cada participante recebe a mensagem, grava informações em seu próprio log e responde com seu voto:

- `SIM`, se conseguiu executar sua parte da transação;
- `NÃO`, se não conseguiu.

A mensagem do coordenador também inclui a lista de participantes, para que cada nó conheça os demais envolvidos.

Essa informação é importante porque, em caso de timeout ou falha do coordenador, um participante pode tentar consultar outros participantes para descobrir a decisão.

---

## 45. Segunda fase do 2PC: decisão

A segunda fase é a fase de decisão.

A regra é:

- se todos os participantes votarem `SIM`, o coordenador pode decidir por `COMMIT`;
- se pelo menos um participante votar `NÃO`, o coordenador deve decidir por `ABORT`.

```text
Todos votaram SIM → COMMIT
Pelo menos um votou NÃO → ABORT
```

Após decidir, o coordenador envia a decisão para todos os participantes.

Cada participante executa a decisão recebida e envia um acknowledgment ao coordenador, informando que recebeu a decisão e executou a ação correspondente.

---

## 46. Período de incerteza no 2PC

O professor destacou uma observação fundamental:

> Depois que um participante vota `SIM`, ele não pode mais tomar decisão localmente.

Isso significa que, após votar `SIM`, o participante não pode:

- abortar localmente;
- commitar localmente;
- liberar bloqueios por conta própria;
- decidir sozinho o destino da transação.

Ele precisa aguardar a decisão do coordenador.

Esse intervalo é chamado de **período de incerteza**.

O problema ocorre quando o participante vota `SIM`, mas não recebe a decisão do coordenador dentro do tempo esperado.

Nesse caso, ele pode atingir timeout e ficar sem saber o que fazer.

Pior cenário:

- o coordenador cai;
- nenhum participante conhece a decisão;
- todos ficam bloqueados aguardando uma decisão que não chega.

Esse é um dos problemas do 2PC.

---

## 47. Falha Bizantina

O professor explicou a **Falha Bizantina** como uma falha em sistemas distribuídos que pode ser provocada por um atacante ou nó malicioso.

Em uma falha bizantina, um participante pode se comportar de forma arbitrária ou maliciosa.

Exemplos:

- um nó intruso se passa por participante e envia voto falso ao coordenador;
- um nó intruso se passa pelo coordenador e envia decisão falsa aos participantes;
- diferentes participantes recebem decisões diferentes;
- um nó tenta confundir o processo de consenso.

Isso pode quebrar o protocolo de consenso e causar execução parcial da transação.

### 47.1 Problema dos Generais Bizantinos

O nome vem do problema clássico dos Generais Bizantinos.

A ideia é:

- vários exércitos bizantinos cercam uma cidade;
- para vencer, todos precisam atacar ao mesmo tempo;
- se alguns atacarem e outros não, eles perdem;
- os generais precisam chegar a um consenso;
- porém, mensagens podem ser adulteradas e generais podem ser traidores.

A pergunta central é:

> Como chegar a um consenso confiável na presença de participantes maliciosos?

Protocolos como Paxos e Raft tentam lidar melhor com falhas distribuídas, mas o 2PC não resolve falhas bizantinas.

---

## 48. Falha Bizantina e violação de integridade

A falha bizantina pode violar a integridade dos dados.

Exemplo explicado em aula:

1. alguns participantes recebem a decisão de `COMMIT`;
2. outros recebem a decisão de `ABORT`;
3. parte da transação é confirmada;
4. outra parte é desfeita;
5. ocorre execução parcial.

Execução parcial de transação não pode acontecer, pois quebra a atomicidade.

O professor usou o exemplo de uma transferência bancária:

```text
Retirar R$ 100 da conta A
Depositar R$ 100 na conta B
```

Se apenas a retirada for executada e o depósito não, houve execução parcial. Isso viola a integridade dos dados.

Em transações distribuídas, esse risco é ainda mais grave porque as partes da transação estão em nós diferentes.

---

## 49. Mitigação de Falha Bizantina no 2PC

O professor explicou que, se fosse necessário evitar falhas bizantinas usando 2PC, seria preciso implementar extensões ao protocolo.

O SGBD normalmente implementa apenas o 2PC básico. Logo, esse controle adicional teria que ser feito em nível de aplicação.

Uma estratégia citada foi a verificação baseada em **quórum**.

A ideia é que os participantes comparem a decisão recebida com as decisões recebidas pelos outros participantes.

```text
Participante A recebeu COMMIT
Participante B recebeu ABORT
→ divergência detectada
```

Essa comparação pode ajudar a detectar que há um comportamento inconsistente ou malicioso.

O professor observou que, se o objetivo for evitar esse risco, é comum a recomendação de evitar o 2PC puro em cenários com possibilidade de falha bizantina.

---

## 50. Vulnerabilidades comuns em bancos de dados

Em outro momento da aula, o professor passou a discutir vulnerabilidades em bancos de dados.

Foram citadas como principais vulnerabilidades:

- senhas padrão;
- SQL Injection;
- falta de criptografia;
- privilégios excessivos;
- falta de auditoria;
- vulnerabilidades no próprio software do SGBD;
- configurações incorretas;
- exposição desnecessária do banco em rede pública;
- serviços desnecessários habilitados.

### 50.1 Senhas padrão

Senhas padrão são um problema recorrente. Se o banco ou serviço é instalado e a senha padrão não é alterada, um atacante pode tentar autenticação com credenciais conhecidas.

### 50.2 SQL Injection

O professor chamou SQL Injection de uma das principais vulnerabilidades relacionadas a banco de dados.

Embora a falha normalmente esteja na aplicação, o alvo do ataque é o banco.

### 50.3 Falta de criptografia

A falta de criptografia pode ocorrer:

- nos dados em repouso;
- nos dados em trânsito.

### 50.4 Privilégios excessivos

Um exemplo citado foi um usuário com papel de estagiário tendo privilégios equivalentes aos de DBA.

Outro exemplo foi a aplicação usar `sa`, `root` ou outro usuário administrativo para se conectar ao banco.

Se a aplicação for comprometida, o atacante herda todos os privilégios dessa conta.

### 50.5 Falta de auditoria

Sem auditoria, não é possível saber quem acessou o quê, quando e como.

A ausência de auditoria prejudica a investigação de incidentes.

---

## 51. Controle de acesso

O professor apresentou o controle de acesso como uma das primeiras linhas de defesa.

Ele é baseado em três pilares:

1. **Identificação**;
2. **Autenticação**;
3. **Autorização**.

### 51.1 Identificação

Identificação é o ato de informar uma identidade ao sistema.

Exemplo:

```text
Usuário informa: “sou Marcos”
```

### 51.2 Autenticação

Autenticação é provar que a identidade informada é verdadeira.

Pode envolver:

- senha;
- token;
- biometria.

### 51.3 Autorização

Autorização é definir o que o usuário autenticado pode fazer.

Exemplo:

```text
Marcos pode executar SELECT na tabela Salários?
Marcos pode executar UPDATE?
Marcos pode executar DROP TABLE?
```

---

## 52. RBAC, GRANT, REVOKE e DENY

O modelo de autorização citado foi o **RBAC — Role-Based Access Control**.

Nesse modelo, permissões são associadas a papéis, e usuários são associados a papéis.

Exemplos de roles:

- `Vendedor`;
- `Gerente`;
- `Auditor`;
- `Analista_Financeiro`.

A vantagem é facilitar a administração.

Em vez de conceder permissões usuário por usuário, o administrador concede permissões à role.

```sql
GRANT SELECT ON Vendas TO Vendedor;
```

Depois, basta associar usuários à role.

Comandos citados:

```sql
GRANT  -- concede permissão
REVOKE -- remove permissão
DENY   -- nega explicitamente uma permissão, no SQL Server
```

No SQL Server, o `DENY` tem precedência sobre o `GRANT`.

Se um usuário recebe `GRANT` por meio de uma role, mas recebe `DENY` diretamente, a negação explícita prevalece.

---

## 53. Princípio do Menor Privilégio

O professor reforçou o **Princípio do Menor Privilégio**.

Esse princípio afirma que um usuário ou processo deve ter apenas as permissões estritamente necessárias para realizar sua função.

Exemplo:

```text
Se o sistema só precisa gerar relatório, conceda apenas SELECT.
Não conceda UPDATE, DELETE ou DROP.
```

A razão é limitar o estrago caso a conta seja comprometida.

Se a aplicação usa uma conta com privilégios administrativos, um SQL Injection bem-sucedido pode permitir:

- apagar tabelas;
- alterar dados críticos;
- criar usuários;
- executar comandos perigosos;
- comprometer o servidor.

No SQL Server, foi citado o comando `xp_cmdshell`, que permite executar comandos de shell do Windows a partir do SQL Server. Hoje, por padrão, esse recurso vem desabilitado, mas em versões antigas ou por má configuração pode estar ativo.

Nas mãos de um atacante, esse recurso pode permitir ações graves, como baixar arquivos maliciosos, criar usuários no sistema operacional ou apoiar um ataque de ransomware.

---

## 54. SQL Injection: exemplo básico

O professor explicou o funcionamento básico de um SQL Injection.

Imagine uma aplicação que monta uma consulta concatenando texto digitado pelo usuário:

```sql
SELECT * FROM produtos WHERE nome = 'texto_digitado';
```

Se o atacante digitar:

```sql
' OR 1=1 --
```

A consulta pode se transformar em algo como:

```sql
SELECT * FROM produtos WHERE nome = '' OR 1=1 --';
```

Como `1=1` é sempre verdadeiro, o banco pode retornar todos os produtos.

O `--` comenta o restante da linha, evitando erro de sintaxe.

O professor explicou que esse é apenas o básico. Um atacante pode usar técnicas como `UNION` para tentar extrair dados de outras tabelas, como usuários e senhas, além de descobrir versão do banco, nomes de colunas e estrutura do esquema.

---

## 55. Ataques de integridade por abuso de privilégio

O professor citou um exemplo clássico de ataque à integridade envolvendo frações de centavos.

A ideia é:

- transações bancárias geravam frações de centavos;
- essas frações eram arredondadas;
- um funcionário criou um script para coletar os “restos” de centavos de muitas contas;
- para cada cliente, o valor era imperceptível;
- para o atacante, a soma de muitos pequenos valores gerava um montante significativo.

Esse tipo de ataque altera o estado do banco de forma maliciosa e compromete a integridade e a conformidade dos dados.

---

## 56. Defesa em profundidade

O professor reforçou a ideia de **defesa em profundidade**.

Não basta ter apenas um firewall. A proteção deve ocorrer em camadas.

Camadas citadas:

- criptografia na rede, com TLS/SSL;
- controle de acesso rigoroso;
- RBAC;
- princípio do menor privilégio;
- auditoria;
- backups regulares;
- backups protegidos;
- criptografia de dados sensíveis;
- proteção dos dados em repouso;
- proteção dos logs.

A lógica é que, mesmo se o atacante passar por uma camada, ainda encontrará outras barreiras.

Exemplo:

```text
Firewall falhou → controle de acesso ainda limita ações
Controle de acesso falhou → dados sensíveis podem estar criptografados
Dados foram apagados → backup permite recuperação
```

O professor também destacou que o elo mais fraco muitas vezes é o humano. Engenharia social para obter senha de um DBA pode ser mais fácil que quebrar uma criptografia forte.

---

## 57. Backup, múltiplas cópias e locais diferentes

O professor explicou que, para garantir recuperação, é mais seguro manter várias cópias de backup.

A ideia é que, se uma cópia for destruída, corrompida ou inacessível, outras cópias ainda estarão disponíveis.

Na prática, empresas normalmente mantêm backups em locais diferentes.

Isso aumenta a chance de recuperação em caso de desastre.

Porém, o professor observou que isso também cria outro risco: quanto mais cópias existem, maior a superfície de exposição. Uma cópia mal protegida pode ser roubada ou acessada indevidamente.

A decisão envolve trade-off:

```text
Mais cópias → maior chance de recuperação
Mais cópias → mais pontos que precisam ser protegidos
```

Por isso, backups devem ser armazenados em locais seguros e preferencialmente criptografados.

---

## 58. Segurança nunca é 100%

O professor reforçou que não existe sistema 100% seguro.

Mesmo organizações com alto nível de segurança podem sofrer vazamentos ou incidentes por falhas humanas, ameaças internas ou eventos improváveis.

Foram usados exemplos para ilustrar a ideia:

- vazamento de informações por pessoas com acesso autorizado;
- dados copiados em mídias removíveis;
- cópias físicas ou persistentes manuseadas por seres humanos;
- ambientes espelhados que podem ser afetados pelo mesmo desastre físico.

O exemplo das Torres Gêmeas foi usado para mostrar que, mesmo com dois ambientes espelhados, é possível perder ambos se eles estiverem em locais sujeitos ao mesmo evento extremo.

A lição é:

> Segurança é redução de risco, não eliminação absoluta de risco.

---

## 59. Mitigações para ataques de integridade

Para mitigar ataques contra a integridade dos dados, foram citadas estratégias como:

- replicação de backups;
- replicação do arquivo de log, quando aplicável;
- criptografia de backups;
- criptografia do log;
- mecanismos de RAID;
- controle físico e lógico de acesso aos arquivos;
- armazenamento seguro de mídias de backup.

A replicação do backup é comum. Já a replicação do arquivo de log não é tão comum, segundo o professor.

---

## 60. RAID — Redundant Array of Independent Disks

O professor apresentou o mecanismo de **RAID — Redundant Array of Independent Disks**.

A ideia do RAID é combinar vários discos para obter:

- melhor performance;
- maior confiabilidade;
- recuperação em caso de falha de disco.

O RAID trabalha com dois princípios principais:

1. **distribuição** dos dados;
2. **redundância**.

A distribuição melhora performance, pois permite ler ou escrever partes diferentes dos dados em discos diferentes.

A redundância permite recuperar dados caso um disco falhe.

---

## 61. RAID 0, RAID 1, RAID 10 e RAID 01

O professor citou diferentes níveis de RAID.

### 61.1 RAID 0

No RAID 0, há distribuição dos dados, mas não há redundância.

```text
RAID 0 → distribuição → performance
RAID 0 → sem redundância → sem proteção contra falha de disco
```

Se um disco falhar, os dados podem ser perdidos.

### 61.2 RAID 1

No RAID 1, há espelhamento, ou seja, replicação de disco.

Se há cinco discos de dados, seriam necessários mais cinco discos para espelhar, totalizando dez discos.

```text
RAID 1 → replicação → maior segurança
RAID 1 → custo alto → dobra a quantidade de discos
```

### 61.3 RAID 10 e RAID 01

O professor também citou combinações como RAID 10 e RAID 01, que variam a ordem entre distribuição e replicação.

A ideia geral é combinar performance e redundância.

---

## 62. RAID 5

O RAID 5 foi apresentado como um dos níveis mais utilizados.

Ele combina distribuição dos dados com paridade distribuída.

Exemplo conceitual:

```text
Arquivo A dividido em blocos:
A1 → disco 1
A2 → disco 2
A3 → disco 3
A4 → disco 4
Paridade de A → disco 5
```

Ao ler o arquivo, o sistema pode ler vários blocos em paralelo, aumentando a performance.

A paridade permite recuperar dados caso um disco falhe.

Diferentemente de um modelo em que um disco é dedicado apenas à paridade, no RAID 5 a paridade é distribuída entre os discos.

Isso evita concentrar toda a redundância em um único disco.

O professor destacou que, com RAID 5, se um disco for perdido, é possível recuperar os dados daquele disco.

A recuperação ocorre de forma transparente para a aplicação. A aplicação não precisa perceber que houve falha de disco.

---

## 63. RAID 6 e Reed-Solomon

O professor explicou que, no RAID 6, é possível tolerar a perda de mais de um disco, dependendo da quantidade de redundância.

A ideia é aumentar a redundância para aumentar a capacidade de recuperação.

Também foi citado o uso de códigos de Reed-Solomon em sistemas de arquivos distribuídos, como Google File System e Hadoop.

Exemplo apresentado:

```text
10 MB de dados divididos em 10 blocos de 1 MB
+ 4 blocos adicionais de paridade
→ permite recuperar dados mesmo com perda de até 4 blocos/discos, conforme a configuração
```

A lógica geral é:

```text
Mais redundância → maior capacidade de recuperação
Mais redundância → maior custo de armazenamento
```

---

## 64. Performance, redundância e tempo de recuperação

O professor enfatizou que diferentes níveis de RAID variam em:

- grau de distribuição;
- grau de redundância;
- performance;
- confiabilidade;
- tempo de recuperação;
- custo.

A replicação é mais cara, pois exige mais discos. Porém, a recuperação pode ser mais rápida.

A paridade economiza discos em comparação com replicação total, mas a recuperação pode ser mais demorada.

Exemplo discutido:

- em uma aplicação bancária crítica, talvez não seja aceitável esperar muito tempo pela recuperação;
- nesse caso, um RAID com replicação pode ser mais adequado, apesar do custo maior.

A escolha do nível de RAID depende da criticidade da aplicação.

---

## 65. Backup frequente, backup diferencial e recuperação

O professor relacionou backup com tempo de recuperação.

Quanto mais frequente for o backup, menor tende a ser o tempo necessário para recuperar até o ponto da falha.

Exemplo:

```text
Backup a cada 1 hora → em caso de falha, recupera o backup e aplica no máximo 1 hora de log
```

O professor comentou que, antigamente, gerar backup podia exigir parar o banco. Hoje, existe o conceito de backup diferencial e backup “a quente”.

### 65.1 Backup diferencial

No backup diferencial, depois de um backup completo, os próximos backups registram apenas o que foi alterado desde o backup anterior.

Isso reduz o volume de dados copiados e permite gerar backup sem parar o banco em muitos cenários.

```text
Backup completo → imagem inicial
Backup diferencial → alterações desde o backup anterior
```

---

## 66. Disponibilidade e ataques DoS em banco de dados

O professor discutiu ataques contra disponibilidade.

Se o objetivo é manter o banco sempre disponível, é necessário usar mecanismos como replicação, balanceamento e limites de recursos.

Um ataque de **DoS — Denial of Service** contra banco de dados pode ocorrer de várias formas.

Exemplos citados:

- inundar o banco com conexões;
- esgotar o limite máximo de conexões simultâneas;
- disparar consultas extremamente pesadas;
- consumir 100% de CPU e memória;
- executar várias consultas com joins pesados e sem índices;
- travar recursos internos do banco com bloqueios.

Exemplo conceitual:

```text
Consulta pesada demora 5 minutos
Atacante dispara 50 execuções simultâneas
→ CPU e memória são consumidas
→ usuários legítimos não conseguem acesso
```

Segurança não envolve apenas sigilo. Também envolve disponibilidade.

---

## 67. Mitigações contra DoS no banco de dados

Foram citadas algumas mitigações para reduzir ataques de DoS:

- limite máximo de conexões por usuário;
- limite de recursos por usuário ou role;
- tempo máximo de execução de consultas;
- timeout de conexões inativas;
- encerramento automático de sessões ociosas;
- controle de consultas pesadas;
- criação adequada de índices;
- monitoramento de consumo de CPU e memória;
- balanceamento de carga entre réplicas.

Exemplo conceitual:

```text
Usuário Relatório → no máximo 10% de CPU
Consulta ultrapassou limite → SGBD encerra o processo
```

A ideia é impedir que um único usuário, aplicação ou processo consuma todos os recursos do SGBD.

---

## 68. Auditoria em banco de dados

A auditoria serve para rastrear o que aconteceu depois que uma ação ocorreu.

O professor explicou que auditoria responde perguntas como:

- quem acessou determinada informação?;
- quem alterou determinado salário?;
- quando a alteração aconteceu?;
- qual comando foi executado?;
- houve login com falha?;
- quem criou ou apagou uma tabela?

O banco pode auditar diferentes eventos:

- logins bem-sucedidos;
- logins falhos;
- comandos DDL, como `CREATE`, `ALTER` e `DROP`;
- comandos DML, como `INSERT`, `UPDATE` e `DELETE`;
- ações de administradores;
- acesso a tabelas críticas.

O professor citou o **SQL Server Audit**, recurso que permite criar uma especificação de auditoria e definir o que será monitorado.

---

## 69. Impacto da auditoria

Auditar tudo pode gerar alto impacto em performance e armazenamento.

Se cada comando executado por todos os usuários for auditado, o banco pode gerar muitos gigabytes de log por dia.

Além disso, cada comando pode demorar mais, pois o SGBD precisa registrar a ação auditada.

Por isso, a auditoria deve ser seletiva.

Normalmente, audita-se:

- ações de DBAs;
- acessos a dados financeiros;
- acessos a dados de RH;
- acessos a dados de clientes;
- alterações em tabelas críticas;
- comandos administrativos sensíveis.

---

## 70. Integridade dos logs de auditoria

Um aluno perguntou se logs poderiam ser usados como prova judicial.

O professor respondeu que sim, podem ser usados.

Por isso, a integridade do log de auditoria é fundamental.

Se o log puder ser alterado livremente, ele perde valor legal e investigativo.

A recomendação apresentada foi exportar os logs para um ambiente seguro, preferencialmente externo ao banco.

Exemplos:

- servidor Syslog;
- SIEM;
- ambiente onde o DBA do banco não tenha permissão de escrita.

A ideia é impedir que o próprio administrador do banco apague ou altere rastros de uma fraude.

---

## 71. Criptografia em trânsito e em repouso

O professor explicou dois grandes contextos de criptografia:

- criptografia em trânsito;
- criptografia em repouso.

### 71.1 Criptografia em trânsito

Protege dados trafegando pela rede.

Exemplo:

```text
Aplicação ↔ conexão TLS/SSL ↔ SGBD
```

Ela protege contra interceptação da comunicação.

### 71.2 Criptografia em repouso

Protege dados armazenados em disco.

Exemplos de dados em repouso:

- arquivos de dados;
- arquivos de log;
- backups;
- dumps;
- mídias de armazenamento.

---

## 72. TDE — Transparent Data Encryption

O professor citou o **TDE — Transparent Data Encryption**.

O TDE criptografa os arquivos físicos do banco de dados, como:

```text
.mdf → arquivo de dados
.ldf → arquivo de log
```

A proteção é transparente para a aplicação. Ou seja, a aplicação não precisa ser alterada.

A lógica é:

- quando o dado vai para o disco, o banco criptografa;
- quando o dado vai para a memória/buffer, o banco descriptografa.

O TDE protege contra roubo físico da mídia ou cópia indevida dos arquivos.

Se alguém roubar o HD, copiar o arquivo do banco ou tentar abrir o backup em outro servidor, não conseguirá ler os dados sem a chave adequada.

---

## 73. Limitação do TDE

O TDE não protege contra usuários com privilégios dentro do banco.

Se um DBA autenticado executar um `SELECT`, ele verá o dado descriptografado, pois o SGBD descriptografa o dado para processá-lo.

Portanto, o TDE protege contra roubo físico de arquivos, mas não impede que usuários autorizados vejam dados aos quais têm permissão.

Para proteger dados até mesmo contra o DBA, é necessário outro mecanismo, como criptografia de coluna ou Always Encrypted.

---

## 74. Always Encrypted

O professor explicou o recurso **Always Encrypted**.

A ideia é que o dado seja criptografado no lado do cliente, e não no servidor.

Nesse modelo:

- a chave de criptografia fica na aplicação ou no cliente;
- o dado chega criptografado ao banco;
- o dado é armazenado criptografado;
- o DBA vê apenas dados ilegíveis;
- apenas quem possui a chave no lado da aplicação consegue ler.

```text
Aplicação com chave → criptografa → envia ao banco → banco armazena criptografado
Banco → retorna dado criptografado → aplicação com chave descriptografa
```

Esse mecanismo permite separação de deveres:

- o DBA administra o banco;
- mas não necessariamente consegue ver dados sensíveis dos clientes.

---

## 75. Trade-off do Always Encrypted

O Always Encrypted aumenta a segurança, mas reduz a capacidade de processamento no servidor.

Se o banco não possui a chave, ele não consegue interpretar o valor criptografado.

Isso pode limitar operações como:

- filtros com `WHERE`;
- comparações;
- ordenações;
- buscas por padrões;
- processamento de colunas criptografadas.

O professor destacou o trade-off:

```text
Mais segurança → menos processamento no servidor
Mais funcionalidade no servidor → maior exposição dos dados
```

Por isso, não se criptografa necessariamente a tabela inteira. Normalmente, criptografam-se apenas colunas extremamente sensíveis, como CPF, número de cartão de crédito ou outros identificadores críticos.

---

## 76. OLTP, OLAP e Data Warehouse

O professor também comentou sobre bancos voltados para análise, como ambientes **OLAP — Online Analytical Processing**.

Esses bancos alimentam BI e Data Warehouse.

Em um Data Warehouse, informações são consolidadas para tomada de decisão.

O professor destacou que um atacante interessado em informações estratégicas da empresa pode preferir atacar o Data Warehouse em vez do banco transacional.

Exemplos de informações sensíveis em ambiente analítico:

- faturamento estratégico;
- margem de lucro;
- desempenho de produtos;
- indicadores de vendas;
- dados consolidados de clientes.

As tabelas de fatos são especialmente críticas, pois armazenam os números e indicadores centrais da análise.

Se um concorrente obtém acesso a uma tabela de fatos de vendas, pode descobrir onde a empresa ganha ou perde dinheiro.

Logo, segurança em ambientes analíticos é tão importante quanto em ambientes transacionais.

---

## 77. Relação com o triângulo CID

Ao tratar de disponibilidade, integridade e confidencialidade, o professor relacionou os temas ao triângulo CID:

- **Confidencialidade**: impedir acesso indevido aos dados;
- **Integridade**: impedir alteração indevida ou inconsistência;
- **Disponibilidade**: manter o banco acessível para usuários legítimos.

Exemplos:

| Propriedade | Exemplo de ameaça |
|---|---|
| Confidencialidade | leitura indevida de dados sensíveis |
| Integridade | alteração maliciosa de valores ou execução parcial de transação |
| Disponibilidade | DoS por conexões ou consultas pesadas |

A segurança em banco de dados precisa proteger as três propriedades.

---

## 78. Síntese dos principais pontos da aula

A aula aprofundou os componentes internos do SGBD e mostrou como cada um pode representar tanto um mecanismo de proteção quanto um ponto de vulnerabilidade.

Principais ideias:

- o arquivo de log é essencial para recuperação, mas revela informações e pode ser alvo de ataque;
- checkpoints orientam a recuperação e, se manipulados, podem inviabilizar o processo;
- `LSN`, `Prev LSN`, ID da transação e Page ID são informações fundamentais para recuperação;
- log de recuperação é diferente de log de auditoria;
- `Redo` refaz operações confirmadas; `Undo` desfaz operações não confirmadas;
- dados em repouso são vulneráveis quando arquivos físicos podem ser acessados fora do SGBD;
- bloqueios podem ser explorados para causar DoS;
- o gerenciador de buffers melhora performance, mas páginas quentes e locks longos podem ser explorados;
- aplicações acessam bancos por drivers, como JDBC e ODBC, ou por SQL embutido com pré-compilação;
- tablespaces e filegroups associam objetos lógicos a arquivos físicos;
- página, bloco e setor pertencem a camadas diferentes;
- arquiteturas cliente-servidor criam vulnerabilidades na conexão e na sobrecarga de requisições;
- bancos distribuídos fragmentam dados; bancos replicados mantêm cópias;
- replicação melhora disponibilidade, mas aumenta o custo de propagação de atualizações;
- propagação ávida favorece consistência; propagação preguiçosa favorece disponibilidade;
- protocolos de consenso, como 2PC, Paxos e Raft, são fundamentais em sistemas distribuídos;
- o Teorema CAP mostra o trade-off entre consistência, disponibilidade e tolerância a partição;
- falhas bizantinas podem quebrar o consenso e violar integridade;
- RAID combina distribuição e redundância para performance e recuperação;
- backups frequentes e diferenciais reduzem tempo de recuperação;
- auditoria precisa ser seletiva e protegida contra alteração;
- criptografia em trânsito, TDE e Always Encrypted protegem em níveis diferentes;
- segurança deve ser pensada em camadas, com defesa em profundidade.

---

## 79. Termos importantes

| Termo | Ideia principal |
|---|---|
| Benchmark | Recurso para comparar desempenho de sistemas |
| TPC-H | Benchmark conhecido para consultas analíticas |
| Arquivo de log | Arquivo usado para recuperação de transações |
| Checkpoint | Ponto de controle usado para iniciar recuperação |
| LSN | Identificador sequencial de registro de log |
| Prev LSN | Ponteiro para registro anterior da mesma transação |
| Redo | Refazer operações confirmadas |
| Undo | Desfazer operações não confirmadas |
| Log de auditoria | Registro de ações para rastreabilidade |
| Buffer | Área de memória usada pelo SGBD para páginas frequentes |
| LRU | Política que remove a página menos recentemente usada |
| Página | Unidade de acesso do SGBD |
| Bloco | Unidade de acesso do sistema de arquivos |
| Setor | Unidade de acesso da controladora de disco |
| Tablespace | Área lógica de armazenamento associada a arquivos físicos |
| Filegroup | Equivalente do SQL Server para agrupamento de arquivos |
| BLOB | Objeto binário grande armazenado no banco |
| Cliente-servidor | Arquitetura que distribui carga entre cliente e servidor |
| Banco distribuído | Dados fragmentados entre vários nós |
| Banco replicado | Dados copiados em vários nós |
| Propagação ávida | Atualização propagada antes do fim da transação |
| Propagação preguiçosa | Atualização propagada depois da confirmação local |
| 2PC | Protocolo de commit em duas fases |
| Paxos | Protocolo de consenso distribuído |
| Raft | Protocolo de consenso semelhante ao Paxos, com foco em compreensibilidade |
| Teorema CAP | Trade-off entre consistência, disponibilidade e tolerância a partição |
| Falha Bizantina | Falha distribuída com comportamento malicioso ou arbitrário |
| RAID | Arranjo redundante de discos independentes |
| TDE | Criptografia transparente dos arquivos do banco |
| Always Encrypted | Criptografia no lado do cliente |
| RBAC | Controle de acesso baseado em papéis |
| DoS | Ataque de negação de serviço |
| SIEM | Sistema de centralização e correlação de eventos de segurança |

---

## 80. Conclusão da aula

A aula mostrou que a segurança em banco de dados depende diretamente do entendimento dos componentes internos do SGBD.

O arquivo de log, o gerenciador de transações, o gerenciador de buffers, os arquivos físicos, os mecanismos de armazenamento, a arquitetura cliente-servidor, a replicação, os protocolos de consenso, o RAID, a auditoria e a criptografia não são apenas recursos técnicos. Todos eles têm impacto direto em confidencialidade, integridade e disponibilidade.

O professor reforçou que cada mecanismo criado para garantir eficiência, recuperação ou disponibilidade também pode abrir uma nova superfície de ataque.

Por isso, proteger um banco de dados exige uma visão em camadas:

- proteger a aplicação;
- proteger a conexão;
- proteger o SGBD;
- proteger os arquivos físicos;
- proteger os logs;
- proteger backups;
- limitar privilégios;
- auditar ações críticas;
- criptografar dados sensíveis;
- planejar recuperação e disponibilidade.

A ideia central é que segurança em banco de dados não é apenas impedir acesso externo. É compreender como o banco funciona internamente e proteger cada ponto em que os dados podem ser lidos, alterados, corrompidos, indisponibilizados ou recuperados de forma indevida.
