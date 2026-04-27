# Segurança em Banco de Dados — Aula 27/04

> **Disciplina:** Segurança em Banco de Dados  
> **Tema da aula:** fundamentos de sistemas de banco de dados, vulnerabilidades, arquitetura, processamento de consultas, transações, concorrência, deadlock, log e recuperação.  


---

## 1. Objetivo do curso e foco da disciplina

O curso de **Segurança em Banco de Dados** tem como objetivo estudar quais são as vulnerabilidades existentes dentro de um **Sistema Gerenciador de Banco de Dados (SGBD)** e quais mecanismos podem ser usados para mitigar essas vulnerabilidades.

Para compreender segurança em banco de dados, o professor enfatizou que primeiro é necessário entender **como um sistema de banco de dados funciona internamente**. Sem esse conhecimento, não é possível identificar corretamente onde estão os pontos de vulnerabilidade.

A disciplina será conduzida tratando os conceitos de banco de dados já associados à segurança. A ideia é estudar:

- o que é um sistema de banco de dados;
- como os dados são armazenados;
- como o SGBD acessa e manipula dados;
- como o SGBD garante consistência e recuperação;
- onde aparecem vulnerabilidades em cada nível de abstração;
- como ataques podem ocorrer, especialmente:
  - **injeção de SQL**;
  - ataques contra **dados em repouso**;
  - ataques envolvendo permissões, catálogo, arquivos e código armazenado.

O professor também comentou que pretende reservar uma parte prática para ataques, principalmente com **SQL Injection** e acesso aos arquivos do banco de dados, isto é, ataques contra dados em repouso.


### 1.1 Contexto técnico citado na apresentação inicial

Embora a parte mais pessoal da apresentação tenha sido removida, alguns pontos técnicos mencionados pelo professor ajudam a contextualizar a disciplina:

- em sistemas antigos, funcionalidades que hoje são automáticas no SGBD precisavam ser implementadas manualmente;
- um exemplo citado foi a **detecção e resolução de deadlock**, que em uma experiência profissional precisou ser resolvida manualmente por meio da serialização de transações que acessavam os mesmos dados;
- foram mencionados bancos de dados antigos baseados em **modelo de rede**, como IMS/IMS2 e IDS;
- também foi citado o DB2 como uma tecnologia relacional mais moderna em relação a esses bancos legados;
- o professor mencionou o conceito de **banco de dados temporal**, isto é, banco com dimensão temporal associada aos dados;
- bancos relacionais atuais incorporam parte dessa ideia por meio de tipos como `datetime`;
- foi citado um banco de dados orientado a objetos extensível, no qual era possível criar módulos dentro do motor do banco, inclusive usando linguagem C;
- também foi mencionado o **System R**, sistema relacional desenvolvido pela IBM e importante historicamente para a chegada da tecnologia relacional ao mercado;
- a linha atual de pesquisa mencionada envolve trazer recursos de **inteligência artificial para dentro do núcleo de um SGBD**.

Esses pontos foram usados para mostrar que segurança em banco de dados depende de entender a implementação interna do SGBD: concorrência, recuperação, armazenamento, metadados, processamento de consultas e controle de acesso.

---

## 2. Segurança de dados no cenário atual

A aula iniciou com a discussão de um relatório sobre ameaças a dados. O professor destacou que, atualmente, muitas preocupações das empresas com segurança estão ligadas ao uso de **nuvem** e de **inteligência artificial**.

Entre os pontos de preocupação citados estavam:

- **armazenamento em nuvem** (*cloud-based storage*);
- aplicações em nuvem, associadas ao modelo **Software as a Service (SaaS)**;
- contratação de infraestrutura computacional em nuvem, associada ao modelo **Infrastructure as a Service (IaaS)**;
- segurança relacionada ao uso de **IA**.

A ideia é que muitas empresas defendem que, do ponto de vista econômico, pode ser mais interessante não manter infraestrutura própria de TI e colocar tudo na nuvem. Porém, isso cria novas preocupações de segurança.

Quando a infraestrutura está na rede interna da empresa, há mais controle direto sobre os recursos. Quando os serviços estão na nuvem, a empresa contratante não tem acesso direto à infraestrutura física nem controle total sobre os mecanismos de proteção usados pelo provedor.

O professor também observou que, tradicionalmente, quando se fala em segurança, muita gente pensa primeiro no **gateway** ou na borda da rede. Porém, hoje esse já não é necessariamente o único ou principal ponto de preocupação. A segurança evoluiu bastante nesse ponto, e os riscos estão mais distribuídos.

---

## 3. Vazamento de dados e ameaças internas

Ao discutir vazamento de dados, o professor perguntou qual tipo de ataque poderia provocar o maior vazamento. A resposta central foi: **vulnerabilidades internas**.

Segundo o professor, um dos maiores riscos está em algo difícil de controlar apenas computacionalmente: **pessoas que trabalham dentro da própria organização**.

Esse tipo de ameaça envolve funcionários, prestadores de serviço ou pessoas com acesso legítimo ao ambiente, mas que podem usar esse acesso para copiar ou vazar informações.

Como exemplo, foi citado o caso **Snowden**, em que uma pessoa com acesso autorizado conseguiu copiar dados sensíveis usando um cartão de memória. O professor usou o caso para mostrar que nem todo vazamento ocorre por meio de ataque externo sofisticado. Muitas vezes, o estrago pode ser maior quando o atacante já possui algum nível de confiança dentro da organização.

A lição principal foi:

> Em segurança de banco de dados, não basta se preocupar apenas com ataques externos via rede. É necessário considerar também ameaças internas, acessos autorizados mal utilizados e ataques contra dados fora do caminho normal do SGBD.

---

## 4. O que é um sistema de banco de dados?

Um sistema de banco de dados possui dois componentes principais:

1. **Componente estático:** os dados armazenados.
2. **Componente de software:** o Sistema Gerenciador de Banco de Dados, responsável por manipular, controlar, proteger e recuperar esses dados.

O professor destacou que não basta dizer que banco de dados é apenas “armazenamento de dados”, porque sistemas de arquivos também armazenam dados. Hadoop e Google File System, por exemplo, também são mecanismos de armazenamento.

A diferença central entre um simples sistema de arquivos e um sistema de banco de dados está nos serviços adicionais que o SGBD oferece para gerenciar os dados de forma segura, consistente e eficiente.

Um banco de dados pode ser entendido como um sistema de armazenamento que garante:

- armazenamento persistente dos dados;
- acesso eficiente aos dados;
- controle de concorrência;
- consistência;
- recuperação em caso de falhas;
- independência entre os dados e os programas que os acessam;
- uso de metadados para descrever a estrutura dos dados;
- controle de acesso e segurança.

---

## 5. Arquivos, dados relacionados e metadados

Uma definição comum de banco de dados é “um conjunto de dados estruturados que possuem significado sobre uma determinada organização”. Porém, o professor mostrou que essa definição também pode valer para arquivos.

Por exemplo, é possível ter:

- um arquivo de estudantes da UFC;
- um arquivo de cursos;
- um arquivo de matrículas.

Esses arquivos podem conter dados relacionados e com significado. Mesmo assim, isso não significa automaticamente que há um sistema de banco de dados.

A diferença fundamental aparece quando existe um SGBD oferecendo mecanismos como:

- catálogo;
- metadados;
- esquema;
- restrições de integridade;
- controle de concorrência;
- recuperação;
- independência de dados;
- controle de acesso.

### 5.1 Catálogo e metadados

O professor destacou que uma parte fundamental do conceito de sistema de banco de dados é o **catálogo**.

O catálogo armazena os **metadados**, isto é, os dados sobre os próprios dados.

Exemplos de metadados:

- nomes das tabelas;
- nomes das colunas;
- tipos de dados de cada coluna;
- restrições de integridade;
- chaves primárias;
- chaves estrangeiras;
- visões;
- índices;
- usuários;
- permissões;
- objetos armazenados no banco.

O catálogo é importante porque permite que o SGBD saiba como os dados estão organizados e como devem ser acessados sem que cada aplicação precise conhecer fisicamente a estrutura de armazenamento.

---

## 6. Google File System, chunks e diferença para um SGBD

O professor comparou o **Google File System (GFS)** com sistemas de banco de dados como PostgreSQL, MySQL, SQL Server e DB2.

O GFS trabalha com arquivos organizados em **chunks**, que podem ser entendidos como grandes blocos de armazenamento.

### 6.1 Blocos, setores e mídia de armazenamento

Antes de explicar os chunks, o professor retomou a diferença entre conceitos físicos da mídia e abstrações criadas pelo sistema de arquivos.

Em um **disco magnético**, os dados são armazenados por meio da direção de campos magnéticos. Zeros e uns são representados por orientações diferentes desses campos.

O acesso físico ao disco não ocorre bit a bit. A unidade física de acesso é o **setor**.

Os dados em um disco magnético são organizados em:

- trilhas concêntricas;
- setores dentro dessas trilhas.

O professor comparou o disco magnético a um disco de vinil: ambos possuem trilhas. No HD, porém, as trilhas são subdivididas em setores.

Já o conceito de **bloco** não pertence diretamente à controladora de disco, mas ao sistema de arquivos. O bloco é uma abstração usada para agrupar setores.

Exemplo dado em aula:

- um setor pode ter 4 KB;
- um bloco pode reunir vários setores;
- se um bloco tiver quatro setores, ao posicionar o cabeçote no primeiro setor, os próximos setores também pertencem ao mesmo bloco.

Isso reduz a latência, porque evita que o braço do disco precise se deslocar muitas vezes para ler setores espalhados pela superfície.

### 6.2 Chunks no Google File System

No Google File System, a unidade de acesso é o **chunk**, semelhante ao conceito de bloco, mas pensado para armazenamento distribuído nos data centers da Google.

Cada chunk precisa ter um identificador global único. Assim, o sistema consegue saber:

- onde o chunk está armazenado;
- em qual data center ele está;
- em qual storage físico ele se encontra.

Esses chunks podem estar espalhados em diferentes locais do mundo.

### 6.3 Atualização de chunks

Para aumentar a performance, o GFS evita realizar muitas operações de escrita diretamente sobre o mesmo chunk.

Exemplo explicado:

1. Um chunk com identificador 1 é lido.
2. Se for necessário atualizá-lo, o sistema não reescreve diretamente o mesmo chunk.
3. Ele cria um novo chunk, por exemplo com identificador 99.
4. Se houver nova atualização, pode criar outro chunk, por exemplo 159.
5. Depois, será necessário consolidar esses chunks.

A vantagem é reduzir bloqueios sobre dados muito acessados. A desvantagem é que, em algum momento, será necessário consolidar os dados e lidar com possíveis inconsistências.

### 6.4 Replicação e disponibilidade

O GFS também usa replicação de chunks. Isso significa que o mesmo chunk pode ter cópias em vários data centers.

A replicação aumenta a disponibilidade dos dados. Se um data center falhar, os dados podem ser acessados em outro local.

### 6.5 Relaxamento das propriedades ACID

O professor explicou que o Google File System relaxa algumas propriedades ACID, especialmente o **isolamento**.

Isso significa que ele não garante 100% de consistência da mesma forma que um SGBD voltado a aplicações OLTP.

Em sistemas de banco de dados, especialmente os transacionais, as propriedades ACID são fundamentais. No GFS, por sua finalidade e escala, algumas garantias são relaxadas para favorecer desempenho e disponibilidade.

### 6.6 Relação com segurança

Se um sistema perde dados ou não consegue recuperá-los integralmente, isso afeta a **integridade** dos dados.

Portanto, recuperação e consistência também são temas de segurança em banco de dados.

---

## 7. Sistema de arquivos versus sistema de banco de dados

O professor explicou que, em um sistema de arquivos tradicional, cada aplicação precisa conhecer a estrutura dos arquivos que acessa.

Isso significa que a descrição dos dados fica embutida no código da aplicação.

Exemplo:

- se há um arquivo de alunos, a aplicação precisa saber quais campos existem no arquivo;
- precisa saber a ordem dos campos;
- precisa saber o tipo e o tamanho de cada campo;
- precisa saber onde o arquivo está armazenado fisicamente;
- precisa controlar abertura, leitura, escrita, posição no arquivo e fim de arquivo.

Em um banco de dados, essas responsabilidades são assumidas pelo SGBD.

### 7.1 Dependência entre aplicação e estrutura do arquivo

Quando uma aplicação acessa diretamente arquivos, há forte dependência entre a estrutura física do arquivo e o código da aplicação.

Se a estrutura do arquivo muda, todas as aplicações que acessam esse arquivo precisam ser alteradas e recompiladas.

Em banco de dados, essa dependência é reduzida. O aplicativo acessa o banco por meio de comandos SQL, e o SGBD se responsabiliza por localizar e manipular fisicamente os dados.

### 7.2 Valor dos dados

O professor reforçou uma ideia central:

> O que tem mais valor em uma empresa são os dados dela.

Não adianta ter o melhor software, a melhor rede ou a melhor infraestrutura se a organização não consegue garantir a consistência, a integridade e a recuperação dos seus dados.

---

## 8. Aplicação acessando arquivo: complexidade e vulnerabilidade

O professor mostrou um exemplo de programa em C acessando um arquivo chamado `aluno.dat`.

A transcrição não contém o código completo exibido, mas o professor descreveu a lógica.

### 8.1 Estrutura de dados no código

No programa em C, era necessário definir uma estrutura, por exemplo usando `typedef struct`, para representar os registros do arquivo.

Essa estrutura descreve os campos do registro. Em banco de dados, essa descrição estaria no esquema do banco. Em arquivo, ela fica dentro da própria aplicação.

Em uma tabela de banco de dados, por exemplo, definimos:

- nome da tabela;
- colunas;
- tipos de dados;
- restrições de integridade;
- restrições de domínio.

Exemplos de restrições citadas:

- atributo não pode ser nulo (`NOT NULL`);
- salário deve ser maior que o salário mínimo (`CHECK`).

### 8.2 Endereço físico do arquivo

No acesso direto a arquivos, a aplicação precisa informar fisicamente onde o arquivo está armazenado.

Isso cria uma vulnerabilidade: se todos os programadores conhecem o caminho físico do arquivo, alguém mal-intencionado pode informar esse caminho a um atacante.

Em um SGBD, os arquivos físicos existem, mas normalmente não são conhecidos nem manipulados diretamente por quem acessa os dados via SQL. Quem conhece esses arquivos é o SGBD ou o DBA que configurou os espaços de armazenamento.

### 8.3 Leitura, ponteiro de arquivo e fim de arquivo

Ao abrir um arquivo, há um ponteiro interno indicando a posição atual de leitura ou escrita.

No exemplo:

1. O arquivo é aberto.
2. O ponteiro fica antes do primeiro registro.
3. Ao executar `fread`, o primeiro registro é lido.
4. O programa precisa verificar se chegou ao fim do arquivo.
5. O programa precisa construir um laço para percorrer os registros.
6. Ao encontrar o registro desejado, precisa reposicionar o ponteiro.
7. Para alterar o registro, usa-se algo como `fseek` para voltar à posição correta.
8. Depois, usa-se `fwrite` para gravar a alteração.

O professor destacou que, mesmo com poucas linhas de código, a manipulação direta de arquivos tem complexidade e exige muito cuidado.

### 8.4 Comparação com SQL

Toda a lógica de localizar um aluno pela matrícula e atualizar seu nome poderia ser expressa em SQL de forma muito mais simples:

```sql
UPDATE aluno
SET nome = :nome_informado
WHERE matricula = :matricula_informada;
```

O professor destacou que a linguagem SQL é próxima da linguagem natural:

- atualize a tabela `aluno`;
- altere o atributo `nome`;
- apenas na tupla cuja matrícula seja igual à matrícula informada.

Com isso, o programador não precisa lidar diretamente com ponteiros, posição física do arquivo, `fread`, `fseek` ou `fwrite`.

---

## 9. Dados em repouso

Todos os dados de qualquer sistema de banco de dados estão armazenados fisicamente em arquivos.

A diferença é que o SGBD cria uma camada acima desses arquivos. Quem acessa o banco via SQL não precisa saber onde os arquivos estão.

Quando o SGBD está em execução, os arquivos estão em uso, e normalmente não podem ser manipulados diretamente por outras aplicações.

Porém, se o SGBD for desligado, os arquivos deixam de estar em uso. Nesse momento, os dados ficam em repouso.

**Dados em repouso** são dados armazenados fisicamente, mas não sendo acessados naquele momento pelo SGBD.

O professor destacou que ataques a dados em repouso são relevantes em segurança de banco de dados, porque o atacante pode tentar acessar diretamente os arquivos do banco, sem passar pelo SGBD.

---

## 10. Arquitetura de três níveis de abstração

A tecnologia de banco de dados foi pensada com três níveis de abstração:

1. **Esquema interno**
2. **Esquema conceitual**
3. **Esquemas externos**

Essa arquitetura reduz a dependência entre aplicações e armazenamento físico, além de abrir possibilidades de controle de acesso.

### 10.1 Esquema interno

O esquema interno representa o nível mais próximo do armazenamento físico.

Ele descreve como os dados são armazenados internamente:

- arquivos;
- páginas;
- índices;
- estruturas físicas;
- localização dos dados;
- formas de acesso.

No acesso direto a arquivos, o programador precisa lidar com detalhes equivalentes ao esquema interno. Em banco de dados, essa responsabilidade fica com o SGBD.

### 10.2 Esquema conceitual

O esquema conceitual descreve logicamente o banco de dados.

É nesse nível que o usuário ou aplicação trabalha ao executar comandos como:

```sql
SELECT * FROM aluno;
```

ou:

```sql
UPDATE aluno
SET nome = :nome
WHERE matricula = :matricula;
```

Nesse nível, não é necessário saber onde os dados estão armazenados fisicamente. O usuário informa **quais dados** quer acessar e **o que deseja fazer** com eles.

### 10.3 Esquema externo

O esquema externo corresponde às formas específicas pelas quais certos usuários ou aplicações enxergam o banco de dados.

Esse nível está diretamente relacionado à segurança, porque permite restringir o que cada usuário pode ver.

O principal conceito associado ao esquema externo é a **visão** (*view*).

---

## 11. Visões em banco de dados

Uma **visão** é uma janela definida sobre o banco de dados.

Por meio dela, certos usuários conseguem enxergar apenas uma parte dos dados.

O professor explicou que quem acessa o banco por meio de uma visão não vê todos os dados armazenados. Vê apenas os dados definidos naquela janela.

Por isso, uma das principais funções de uma visão é **restringir o acesso aos dados**.

### 11.1 Exemplo de visão

O professor mostrou uma visão chamada `V1`. A ideia era criar uma janela por meio da qual o usuário pudesse ver apenas:

- o nome de uma filial;
- o nome de um vendedor.

O usuário não veria, por exemplo:

- salário do vendedor;
- outros atributos sensíveis;
- demais dados das tabelas envolvidas.

Um exemplo conceitual seria:

```sql
CREATE VIEW V1 AS
SELECT filial.nome AS nome_filial,
       vendedor.nome AS nome_vendedor
FROM filial
JOIN vendedor ON vendedor.id_filial = filial.id;
```

A consulta exata exibida em aula não aparece integralmente na transcrição, mas o conceito explicado foi esse: a visão é definida por uma consulta SQL e limita os dados visíveis ao usuário.

### 11.2 Visão como mecanismo de segurança

A visão foi apresentada como uma das primeiras preocupações da tecnologia de banco de dados com segurança.

Ao criar uma visão, é possível permitir que usuários tenham acesso apenas a determinados atributos ou tuplas, sem acesso direto às tabelas completas.

### 11.3 Criptografia da definição da visão

O professor explicou que, em alguns SGBDs, é possível proteger a definição da visão usando uma opção como `WITH ENCRYPTION`.

A ideia é que, mesmo que alguém acesse o esquema do banco, não consiga visualizar a definição da visão.

Exemplo conceitual:

```sql
CREATE VIEW V1
WITH ENCRYPTION
AS
SELECT ...;
```

Esse tipo de recurso depende do SGBD. Nem todos trabalham da mesma forma.

### 11.4 Visão virtual e visão materializada

O professor diferenciou dois tipos de visão:

#### Visão virtual

Na visão virtual, os dados da visão não ficam armazenados fisicamente como uma nova tabela.

Eles só existem enquanto a visão está sendo acessada.

Quando a consulta sobre a visão é executada, o SGBD calcula o resultado naquele momento.

#### Visão materializada

Na visão materializada, o resultado da visão é materializado, isto é, armazenado fisicamente.

Nesse caso, podem existir dados em repouso referentes à própria visão.

Isso tem impacto de segurança, porque não basta proteger apenas as tabelas originais. Também é necessário proteger a visão materializada.

---

## 12. Vulnerabilidades nos níveis de abstração

A arquitetura de três níveis trouxe benefícios, mas também abriu pontos de vulnerabilidade.

### 12.1 Vulnerabilidade no nível externo/conceitual: SQL Injection

No nível em que aplicações enviam comandos SQL para o banco, existe o risco de **injeção de SQL**.

A injeção de SQL ocorre quando a aplicação permite que dados fornecidos pelo usuário sejam inseridos diretamente em comandos SQL sem validação adequada.

Se a aplicação não filtrar ou parametrizar corretamente a entrada, o usuário pode enviar comandos SQL inesperados ou maliciosos.

O SGBD executa comandos SQL válidos. Portanto, se a aplicação monta uma consulta vulnerável, o SGBD pode executar algo que o programador não pretendia.

Exemplo clássico citado:

```sql
OR 1 = 1
```

Esse tipo de condição pode alterar a lógica de uma consulta e permitir acesso indevido.

O professor citou que recursos como **Prepared Statements** ajudam a mitigar esse problema.

### 12.2 Vulnerabilidade no esquema interno: dados em repouso

No nível interno, a vulnerabilidade está associada ao acesso direto aos arquivos do banco de dados.

Se alguém consegue acesso ao servidor onde o banco está instalado, pode tentar ler ou copiar os arquivos diretamente, sem passar pelo SGBD.

Esse é o ataque aos **dados em repouso**.

### 12.3 Segurança em todos os níveis

A segurança precisa ser pensada em todos os níveis:

- no nível externo, com visões;
- no nível conceitual, com permissões e controle de acesso;
- no nível interno, com criptografia de arquivos, proteção do sistema operacional, backup, log e recuperação.

---

## 13. Catálogo do banco de dados

O **catálogo do banco de dados** é o local onde o SGBD armazena a descrição de todos os objetos do banco.

Ele contém os metadados do sistema.

No catálogo, ficam informações como:

- tabelas existentes;
- colunas de cada tabela;
- tipos de dados;
- restrições de integridade;
- índices;
- visões;
- funções;
- procedimentos;
- gatilhos;
- usuários;
- privilégios;
- informações de segurança.

O professor destacou que, se um atacante obtém acesso total ao catálogo, ele tem o “mapa da mina”. Ele passa a saber onde estão as informações sensíveis e quem pode acessá-las.

Por isso, o acesso ao catálogo deve ser restrito e protegido pelo SGBD.

Em bancos relacionais, o catálogo também é organizado em tabelas. Em alguns SGBDs, é possível consultar metadados por meio de estruturas como:

```sql
SELECT *
FROM information_schema.tables;
```

Esse tipo de consulta depende das permissões do usuário.

---

## 14. Propriedades ACID

As propriedades **ACID** são fundamentais em sistemas de banco de dados transacionais.

ACID significa:

- **Atomicidade**;
- **Consistência**;
- **Isolamento**;
- **Durabilidade**.

### 14.1 Atomicidade

A atomicidade garante que uma transação seja executada por completo ou não seja executada.

Exemplo:

Em uma transferência bancária, se o dinheiro sai de uma conta, ele precisa entrar na outra. Se houver falha no meio da operação, o sistema não pode deixar o dinheiro desaparecer.

### 14.2 Consistência

A consistência garante que o banco saia de um estado consistente e vá para outro estado consistente.

Isso envolve respeitar as restrições de integridade.

Exemplo:

Se uma regra diz que o saldo não pode ser negativo, o SGBD deve impedir operações que violem essa regra.

### 14.3 Isolamento

O isolamento garante que transações simultâneas não interfiram indevidamente umas nas outras.

Isso evita que uma transação veja dados parciais ou inconsistentes de outra transação ainda em execução.

Do ponto de vista de segurança, o isolamento também evita vazamentos temporários de informações durante o processamento.

### 14.4 Durabilidade

A durabilidade garante que, depois de um `COMMIT`, os efeitos da transação não serão perdidos, mesmo que ocorra uma falha logo em seguida.

Essa propriedade é garantida por mecanismos como o **arquivo de log**.

---

## 15. Controle de acesso

O controle de acesso responde a duas perguntas:

1. **Quem é você?**
2. **O que você pode fazer?**

A primeira pergunta está relacionada à **autenticação**. A segunda está relacionada à **autorização**.

### 15.1 Autenticação

Autenticação é o processo de verificar a identidade do usuário.

Pode envolver:

- algo que o usuário sabe, como uma senha;
- algo que o usuário possui, como um token ou cartão;
- algo que o usuário é, como biometria.

Em banco de dados, ainda é comum o uso de usuário e senha, mas também há integração com mecanismos como:

- Kerberos;
- LDAP;
- autenticação centralizada.

### 15.2 Autorização

Depois de autenticar o usuário, o sistema precisa determinar o que ele pode fazer.

Exemplos:

- pode ler determinada tabela?
- pode alterar dados?
- pode excluir registros?
- pode alterar o próprio salário?
- pode apagar o banco?

### 15.3 Roles ou papéis

Para facilitar a administração de permissões, usa-se o conceito de **roles** ou papéis.

Em vez de conceder permissões individualmente a cada usuário, cria-se um papel, por exemplo:

```text
Analista_Financeiro
```

Depois, concedem-se permissões ao papel e associa-se o usuário a esse papel.

Isso reduz a chance de erro e facilita a manutenção das permissões.

### 15.4 GRANT e REVOKE

Para conceder permissões, usa-se o comando `GRANT`.

Exemplo:

```sql
GRANT SELECT, INSERT ON Aluno TO Joao;
```

Esse comando concede ao usuário João permissão para consultar e inserir dados na tabela `Aluno`.

Para remover uma permissão, usa-se `REVOKE`:

```sql
REVOKE INSERT ON Aluno FROM Joao;
```

### 15.5 Princípio do menor privilégio

O professor destacou o **Princípio do Menor Privilégio**.

Esse princípio diz que o usuário deve receber apenas o acesso estritamente necessário para executar seu trabalho.

Se o usuário só precisa ler relatórios, não deve ter permissões de `UPDATE` ou `DELETE`.

O professor alertou sobre o uso de `GRANT ALL`. Muitas vezes, por pressa ou comodidade, desenvolvedores concedem permissões totais à aplicação. Se essa aplicação sofrer SQL Injection, o atacante herdará os privilégios excessivos da conexão da aplicação.

---

## 16. Índices e arquivos de índices

O professor explicou que o banco de dados pode armazenar diferentes tipos de estruturas, incluindo arquivos de dados e arquivos de índices.

Os **arquivos de índices** armazenam estruturas usadas para acelerar buscas.

Em uma analogia, o índice funciona como um índice remissivo: em vez de procurar em todo o conteúdo, o sistema consulta uma estrutura auxiliar que aponta para onde o dado está.

### 16.1 Índices e vulnerabilidades

O professor comentou que, quando se observa um acesso não especializado ou muito frequente a determinada tabela, pode-se inferir informações sobre a estrutura dos dados, como a existência de uma chave primária ou de um índice importante.

Isso também pode ser tratado como ponto de vulnerabilidade, porque estruturas internas do banco revelam informações sobre a organização dos dados.

### 16.2 Índices em áreas de armazenamento diferentes

Na criação do banco, é possível especificar áreas de armazenamento diferentes para arquivos de dados e arquivos de índices.

Isso é útil para performance.

Exemplo:

- uma tabela possui um índice muito utilizado;
- esse índice pode ser colocado em outro arquivo;
- esse arquivo pode estar em um disco menos concorrido;
- o acesso ao índice fica mais eficiente.

Essa separação também tem impacto em segurança, porque aumenta a distribuição dos dados e, com isso, também aumenta a área de vulnerabilidade.

---

## 17. Código armazenado no banco de dados

Além de dados e índices, bancos de dados modernos podem armazenar fragmentos de código.

O professor citou três tipos principais:

1. **Gatilhos (triggers)**
2. **Funções**
3. **Procedimentos armazenados (stored procedures)**

### 17.1 Gatilhos ou triggers

Um gatilho é um código executado automaticamente pelo SGBD quando ocorre determinado evento.

A ideia geral é:

> Na ocorrência de um evento, opcionalmente satisfeita uma condição, dispare uma ação.

Em banco de dados, eventos comuns são operações como:

- `INSERT`;
- `UPDATE`;
- `DELETE`.

Um gatilho pode ser disparado:

- antes do evento;
- depois do evento.

Cada opção possui vantagens e desvantagens.

### 17.2 Triggers e regras de negócio

Muitas restrições de integridade podem ser definidas diretamente na criação da tabela, usando DDL.

Porém, algumas regras estão mais próximas das regras de negócio da empresa e não conseguem ser expressas apenas com comandos simples como `NOT NULL`, `CHECK` ou chave estrangeira.

Nesses casos, uma regra que antes ficaria em uma aplicação pode ser trazida para dentro do banco de dados por meio de um gatilho.

Vantagem:

- se a regra de negócio mudar, altera-se em um único lugar: no banco.

Desvantagem:

- código dentro do banco pode afetar performance e aumentar a superfície de ataque.

### 17.3 Funções

Uma função em banco de dados é semelhante a uma função em uma linguagem de programação.

Características:

- pode receber parâmetros de entrada;
- retorna um valor de saída;
- pode retornar um valor escalar ou uma tabela;
- pode aparecer dentro de consultas SQL;
- pode ser chamada dentro de gatilhos ou procedimentos.

Exemplo conceitual:

```sql
SELECT minha_funcao(coluna)
FROM tabela;
```

### 17.4 Stored procedures

Stored procedures são procedimentos armazenados no banco.

Podem ter:

- parâmetros de entrada;
- parâmetros de saída;
- nenhum parâmetro, dependendo do caso.

A diferença destacada pelo professor é que uma stored procedure não é chamada dentro de uma consulta da mesma forma que uma função. Ela normalmente é executada por comando próprio.

### 17.5 Diferença entre função e stored procedure

Segundo a explicação da aula:

| Recurso | Característica principal |
|---|---|
| Função | Pode ser chamada dentro de consultas SQL e retorna valor |
| Stored procedure | É executada diretamente e não é usada como expressão dentro de uma consulta comum |
| Trigger | É disparada automaticamente pelo SGBD quando ocorre um evento |

### 17.6 Código armazenado como vulnerabilidade

O professor alertou que código armazenado também cria vulnerabilidades.

Se um atacante consegue executar um comando como `CREATE FUNCTION`, pode criar uma função maliciosa que retorna dados indevidos ou executa ações não autorizadas.

Triggers também podem ser perigosos, embora sejam mais difíceis de criar sem acesso autorizado.

### 17.7 Cuidado com excesso de código no banco

Houve uma época em que muitas pessoas defendiam colocar praticamente todo o código de aplicação dentro do banco.

O professor criticou esse uso excessivo.

O SGBD deve ser eficiente na manipulação de dados. Se muito código de aplicação é colocado dentro dele, o SGBD também passa a gerenciar a execução desses códigos, o que pode impactar a performance.

Conclusão:

> Código armazenado é um recurso útil, mas deve ser usado com cautela.

---

## 18. O que pode ser armazenado em um banco de dados

A partir da explicação do professor, o banco de dados não armazena apenas tuplas de tabelas. Ele pode armazenar diferentes tipos de estruturas e objetos, cada um com implicações de performance e segurança.

Entre os principais elementos citados estão:

- **arquivos de dados**, que armazenam as tabelas e seus registros;
- **arquivos de índices**, que aceleram o acesso aos dados;
- **arquivo de log**, usado para registrar operações de transações e permitir recuperação;
- **catálogo**, que contém metadados sobre o banco;
- **visões**, que funcionam como janelas de acesso aos dados;
- **fragmentos de código**, como gatilhos, funções e stored procedures.

A separação desses elementos pode melhorar desempenho, mas também precisa ser analisada do ponto de vista da segurança. Quanto mais distribuídos os componentes, maior a quantidade de pontos que precisam ser protegidos.

---

## 19. Distribuição, performance e segurança

O professor resumiu duas lições importantes sobre armazenamento em banco de dados.

### 18.1 Distribuição como estratégia de performance

Para melhorar performance, a distribuição é uma estratégia importante.

É possível espalhar estruturas de armazenamento em:

- arquivos diferentes;
- discos diferentes;
- nós de rede diferentes.

Isso pode reduzir concorrência por recursos e melhorar o desempenho.

### 18.2 Separação do arquivo de log

Com relação à segurança e à integridade, a separação do arquivo de log dos arquivos de dados é fundamental.

Se o log e os dados ficam no mesmo disco e esse disco falha, a recuperação fica comprometida.

Por isso, em sistemas robustos, é comum separar:

- arquivos de dados;
- arquivos de log;
- arquivos de índice;
- backups.

### 18.3 Distribuição aumenta a superfície de ataque

A distribuição melhora desempenho e disponibilidade, mas aumenta a área de vulnerabilidade.

Quanto mais componentes, discos, arquivos, nós e caminhos de acesso, maior o número de pontos que precisam ser protegidos.

---

## 20. Componentes de um SGBD

O professor passou então a explicar a componente de software do sistema de banco de dados.

Um sistema de banco de dados possui:

- a componente estática: os dados;
- a componente de software: o SGBD.

A componente de software é responsável por toda a manipulação dos dados.

Do ponto de vista de entrada no sistema, há dois grandes caminhos:

1. **Interpretador DDL**
2. **Compilador DML**

---

## 21. DDL e DML

Toda linguagem de acesso a banco de dados possui duas grandes componentes:

- **DDL — Data Definition Language**
- **DML — Data Manipulation Language**

### 20.1 DDL — linguagem de definição de dados

A DDL contém comandos relacionados ao esquema do banco de dados.

Exemplos:

```sql
CREATE TABLE
CREATE VIEW
CREATE FUNCTION
ALTER TABLE
DROP TABLE
```

Esses comandos criam, alteram ou removem objetos do banco.

Na DDL, o comando usado para remover uma tabela, por exemplo, é `DROP`.

### 20.2 DML — linguagem de manipulação de dados

A DML contém comandos usados para manipular dados.

Exemplos:

```sql
SELECT
INSERT
UPDATE
DELETE
```

Na DML, o comando usado para remover tuplas é `DELETE`.

O professor chamou atenção para a diferença:

- `DROP` remove objeto do esquema;
- `DELETE` remove dados.

---

## 22. Interpretador DDL e compilador DML

O professor explicou a diferença entre interpretador e compilador usando a comparação entre Python e C.

### 21.1 Interpretador

Em linguagens interpretadas, como Python, erros de sintaxe podem aparecer quando a execução chega naquela linha.

O interpretador traduz e executa linha por linha.

O professor comparou isso a processos burocráticos: o processo chega a um setor, o setor encontra o primeiro erro, devolve; depois que esse erro é corrigido, aparece outro.

### 21.2 Compilador

Em linguagens compiladas, como C, o programa é traduzido antes da execução.

Se houver erro de sintaxe, ele aparece na compilação.

A compilação gera um código objeto antes de o programa ser executado.

### 21.3 Por que DDL pode ser interpretada?

Comandos DDL costumam ser executados uma única vez.

Exemplo:

- cria-se uma tabela uma vez;
- cria-se uma visão uma vez;
- cria-se uma função uma vez.

Mesmo que uma tabela seja alterada várias vezes, cada `ALTER TABLE` é um comando diferente.

Por isso, faz sentido que a DDL passe por um interpretador.

### 21.4 Alteração de esquema é complexa

O professor observou que manutenção no esquema do banco pode ser muito complexa.

Exemplo:

- uma tabela foi criada com uma coluna permitindo `NULL`;
- dados foram inseridos;
- depois, tenta-se alterar essa coluna para `NOT NULL`.

Dependendo dos dados existentes, o SGBD pode impedir a alteração.

Nesses casos, pode ser necessário criar outra tabela, mover os dados e ajustar a estrutura.

Por isso, a fase de projeto do banco de dados é fundamental.

### 21.5 Por que DML é compilada?

Consultas e comandos DML precisam de um plano de execução eficiente.

O compilador DML gera um “código objeto” da consulta, isto é, um plano de execução.

Esse plano define como o SGBD acessará fisicamente os dados.

---

## 23. Processamento de consultas

O professor explicou que comandos como `SELECT`, `INSERT`, `UPDATE` e `DELETE` passam por uma componente chamada **processador de consultas** ou **máquina de consulta**.

O processador de consultas é responsável por transformar o SQL em um plano de execução.

### 22.1 Etapas do processamento

As etapas citadas foram:

1. Receber a consulta SQL.
2. Fazer o parser.
3. Gerar a árvore de parse.
4. Reescrever a consulta.
5. Gerar um plano lógico.
6. Selecionar um plano físico de execução.
7. Executar a consulta.

Em várias dessas fases há oportunidade de otimização.

### 22.2 Vulnerabilidade no processamento

O ponto de entrada da consulta é também um ponto de vulnerabilidade.

É nesse momento que pode ocorrer injeção de SQL, caso a aplicação envie comandos malformados ou manipulados pelo usuário.

---

## 24. Plano lógico e plano físico

### 23.1 Plano lógico

O plano lógico trabalha com operações da álgebra relacional, como:

- seleção;
- projeção;
- junção.

Ele não define ainda qual algoritmo físico será usado para executar essas operações.

### 23.2 Plano físico

O plano físico define quais operações físicas serão executadas.

Exemplos:

- `table scan`;
- `index seek`;
- `hash join`;
- `merge join`;
- `nested loop join`;
- `block nested loop join`;
- `index nested loop join`.

O professor mostrou que o plano exibido pelo SGBD normalmente é o plano físico.

---

## 25. Table scan e index seek

### 24.1 Table scan

`Table scan` significa ler a tabela inteira.

O professor explicou que o SGBD faz `table scan` quando não há um índice adequado para localizar rapidamente os dados desejados.

### 24.2 Index seek

Se houver índice, o SGBD pode executar um `index seek`, lendo apenas um subconjunto das tuplas.

Isso tende a ser mais eficiente do que ler a tabela inteira.

---

## 26. Junção: operação fundamental em banco de dados

O professor destacou que a operação de **junção** (*join*) é fundamental em banco de dados, independentemente do modelo.

No modelo relacional, a junção foi formalizada como uma operação da álgebra relacional.

A junção associa duas tabelas por meio de uma condição.

Exemplo conceitual:

```sql
SELECT *
FROM lineitem, partsupp, part, supplier
WHERE lineitem.l_partkey = partsupp.ps_partkey
  AND ...
  AND lineitem.l_quantity = 1;
```

A consulta discutida em aula envolvia quatro tabelas do benchmark **TPC-H**.

O professor explicou que, embora a consulta envolvesse quatro tabelas, a junção física ocorre sempre de duas em duas tabelas.

O processador de consultas precisa decidir a sequência de junções.

### 25.1 Junção não precisa envolver chave primária

Um aluno perguntou se a junção exigia que a chave primária fosse a mesma nas duas tabelas.

O professor explicou que não.

A junção pode ser feita com qualquer atributo de cada tabela. Na prática, muitas junções usam chave primária e chave estrangeira, mas isso não é uma regra obrigatória.

Também não é obrigatório usar igualdade. É possível usar outros operadores de comparação na condição de junção.

### 25.2 Junção é uma operação cara

O professor afirmou que a maior parte das consultas computadas envolve junção e que a junção é uma das operações mais caras.

Encontrar a melhor sequência de junções é um problema difícil.

Para reduzir a complexidade, o SGBD aplica técnicas como:

- heurísticas;
- programação dinâmica;
- redução do espaço de busca.

---

## 27. Algoritmos de junção

Foram citados vários algoritmos físicos de junção.

### 26.1 Hash join

O professor mostrou um `hash join` no plano de execução.

Segundo a explicação da aula, o custo do hash join pode ser estimado como aproximadamente três vezes o somatório do tamanho das tabelas, considerando páginas como unidade:

```text
Custo ≈ 3 × (B(R) + B(S))
```

Em que:

- `B(R)` é o tamanho da tabela R em páginas;
- `B(S)` é o tamanho da tabela S em páginas.

O SGBD escolheu hash join porque as tabelas eram grandes e não havia índice adequado.

### 26.2 Merge join

No `merge join`, a estimativa de custo é menor quando as tabelas já estão ordenadas pelos atributos de junção.

A ideia é ler as duas tabelas uma única vez.

Para o SGBD escolher merge join, as tabelas precisam estar fisicamente ordenadas pelo atributo de junção ou ser ordenadas antes.

### 26.3 Nested loop join

No `nested loop join`, a tabela externa é lida uma vez, e a tabela interna pode ser lida repetidamente para cada página ou tupla da tabela externa.

Por isso, o custo pode ser maior, especialmente quando as tabelas são grandes e não há índice.

---

## 28. Escolha do plano de execução

Na fase de seleção do plano mais eficiente, o SGBD considera vários planos físicos possíveis.

O conjunto de todos os planos possíveis é chamado de **espaço de busca**.

O problema é difícil porque há muitas possibilidades:

- diferentes ordens de junção;
- diferentes algoritmos de junção;
- diferentes formas de acesso;
- diferentes índices disponíveis.

O SGBD não testa tudo de forma ingênua. Ele reduz o espaço de busca usando heurísticas e programação dinâmica.

A expectativa é encontrar um subconjunto de planos em que o melhor plano seja igual ou muito próximo do melhor plano possível no conjunto completo.

### 27.1 Modelo de custo

O SGBD usa um modelo de custo.

Esse modelo pode considerar:

- CPU;
- memória principal;
- acesso a disco;
- tamanho das tabelas;
- quantidade de páginas;
- cardinalidade intermediária.

O professor ressaltou que o acesso a disco costuma ter o maior peso.

Assim, otimizar uma consulta geralmente significa reduzir a quantidade de acessos a disco.

---

## 29. Heurística da árvore de profundidade à esquerda

O professor mostrou que o SGBD construiu uma árvore de junção de profundidade à esquerda.

Exemplo simplificado:

1. Primeiro faz a junção entre `supplier` e `partsupp`.
2. O resultado é juntado com `part`.
3. O resultado é juntado com `lineitem`.

Essa estrutura é chamada de árvore de profundidade à esquerda (*left-deep tree*).

O SGBD poderia considerar outras formas, como juntar `supplier` com `partsupp`, juntar `part` com `lineitem`, e depois juntar os dois resultados.

Mas isso aumenta muito a complexidade.

Ao restringir a busca a árvores de profundidade à esquerda, o SGBD reduz o espaço de busca.

---

## 30. Heurística: seleção antes da junção

Outra heurística explicada foi executar a **seleção** antes da junção.

Seleção é o filtro de tuplas.

Exemplo:

```sql
WHERE L_QUANTITY > 9
```

O atributo `L_QUANTITY` pertence à tabela `lineitem`.

Se o SGBD filtra as tuplas de `lineitem` antes da junção, menos tuplas seguem para a operação de junção.

Isso reduz o custo.

A tabela `lineitem` tinha mais de 6 milhões de tuplas. Após o filtro, fluíam cerca de 4,9 milhões de tuplas.

Como a junção é cara, reduzir a quantidade de tuplas antes dela gera ganho significativo.

---

## 31. Heurística: projeção antes da junção

Além da seleção, o SGBD também pode aplicar **projeção** antes da junção.

Projeção é o filtro de colunas.

Se o usuário precisa apenas de algumas colunas, o SGBD pode evitar carregar todas as colunas da tabela nos resultados intermediários.

### 30.1 Exemplo de impacto

O professor comparou duas consultas que retornavam o mesmo resultado lógico, mas com quantidade diferente de colunas.

Uma usava `SELECT *`, retornando todos os atributos.

A outra retornava apenas os atributos necessários.

O tempo de execução mudou de aproximadamente:

- **1 hora e 10 minutos** para a consulta com mais colunas;
- **17 minutos** para a consulta com projeção reduzida.

### 30.2 Tamanho das tuplas intermediárias

O professor mostrou que, em um plano, cada tupla intermediária tinha cerca de **19 bytes**.

No outro, a mesma quantidade de tuplas fluía, mas cada tupla tinha cerca de **127 bytes**.

A diferença é importante porque resultados intermediários muito grandes podem ser gravados em disco.

Se a tupla é menor, cabem mais tuplas por página.

No SQL Server, a página tem tamanho fixo de **8 KB**.

A quantidade de tuplas por página depende do tamanho da tupla.

Assim:

- tuplas menores;
- mais tuplas por página;
- menos páginas;
- menos acesso a disco;
- melhor desempenho.

### 30.3 Projeção precisa manter atributos de junção

Um aluno perguntou se a projeção é o filtro aplicado às colunas. O professor confirmou.

Mas destacou um detalhe importante: o SGBD não pode simplesmente passar apenas as colunas que serão exibidas ao usuário.

Ele também precisa manter os atributos usados nas condições de junção.

Por exemplo, se a consulta exibe apenas `S_NAME`, mas precisa juntar `supplier` com outra tabela usando `S_SUPPKEY`, esse atributo de junção também precisa seguir no fluxo intermediário.

---

## 32. Refatoração de consultas SQL

O professor explicou que otimização de consulta pode envolver várias estratégias, como criação de índices e refatoração do SQL.

No primeiro exemplo, a refatoração foi reduzir a projeção, evitando `SELECT *`.

Depois, ele mostrou uma refatoração mais sofisticada envolvendo subconsultas.

### 31.1 Exemplo com subconsulta

As duas consultas retornavam o mesmo resultado, mas tinham tempos diferentes.

A ideia da consulta era retornar pedidos da tabela `ORDERS` cujo `TOTALPRICE` fosse menor que determinados valores associados a pedidos com prioridade `2-HIGH`.

A consulta mais lenta comparava a tabela externa com uma subconsulta que era executada repetidamente.

A consulta mais rápida usava uma forma em que a subconsulta era executada uma única vez, por exemplo usando um valor agregado como `MIN`.

Tempos citados no experimento:

- consulta mais lenta: **13,68 segundos**;
- consulta mais rápida: **1,82 segundos**.

A diferença principal:

- em uma forma, a subconsulta é executada para cada tupla da consulta externa;
- na outra, a subconsulta é executada uma única vez.

### 31.2 Text-to-SQL e otimização

O professor comentou um trabalho de mestrado sobre uma plataforma de **Text-to-SQL**.

A ideia é permitir que o usuário escreva consultas em linguagem natural, e o sistema traduza para SQL.

Além de gerar o SQL correto, a plataforma busca:

- produzir código mais eficiente;
- observar se vale a pena criar índices;
- auxiliar na otimização da consulta.

O professor destacou que o problema não é apenas traduzir linguagem natural para SQL, mas garantir que a tradução esteja correta e eficiente.

---

## 33. Gerenciador de transações

O próximo componente estudado foi o **gerenciador de transações**.

Ele é fundamental para garantir a integridade dos dados.

O gerenciador de transações possui duas grandes funcionalidades:

1. garantir a **consistência**;
2. garantir a **recuperação**.

Essas duas propriedades estão diretamente relacionadas à integridade dos dados.

---

## 34. Experimento de concorrência: movimentação de estoque

O professor demonstrou duas transações simples simulando movimentações de estoque.

A primeira transação representava uma **entrada** de produto no estoque.

A segunda representava uma **saída** por venda.

As duas transações operavam sobre o produto com `ID = 1`.

### 33.1 Transações do experimento

- Transação T1: entrada de 20 unidades.
- Transação T2: saída de 5 unidades.

Se o estoque inicial fosse 3890, o valor final correto deveria ser:

```text
3890 + 20 - 5 = 3905
```

### 33.2 Uso de WAITFOR DELAY

O professor usou um comando como:

```sql
WAITFOR DELAY '00:00:10';
```

Esse comando faz a execução esperar 10 segundos.

Ele explicou que isso é artificial no experimento, mas representa algo possível em sistemas reais. Em vez de um `WAITFOR`, poderia haver uma função que demorasse 10 segundos para executar.

O objetivo era forçar a concorrência. Se T1 terminasse antes de T2 começar, não haveria concorrência real.

As duas transações precisavam disputar o mesmo recurso: a tupla do produto com `ID = 1`.

### 33.3 Execução sem controle de concorrência

Sem ativar explicitamente um mecanismo de controle de concorrência, as duas transações foram executadas.

O resultado final foi **3910**, quando deveria ser **3905**.

Isso representou quebra de integridade dos dados.

O erro foi provocado pela concorrência.

---

## 35. Lost update ou atualização perdida

O professor explicou o problema usando as operações de leitura e escrita.

Para controle de concorrência, interessam principalmente duas operações:

- leitura: `R(X)`;
- escrita: `W(X)`.

O que acontece dentro do programa aplicativo entre uma leitura e uma escrita não importa diretamente para o protocolo de controle de concorrência.

### 34.1 Escalonamento sem controle adequado

A ordem das operações foi aproximadamente:

```text
T1: R(X)
T1: espera 10 segundos
T2: R(X)
T2: W(X)
T2: COMMIT
T1: W(X)
T1: COMMIT
```

T1 e T2 leram o mesmo valor inicial de `X`.

T2 subtraiu 5 e gravou.

Depois T1, que ainda tinha lido o valor antigo, somou 20 e gravou por cima.

O valor gravado por T2 foi perdido.

Esse problema é chamado de **lost update**, ou **atualização perdida**.

O entrelaçamento das operações de T1 e T2 provocou a inconsistência.

O papel do mecanismo de controle de concorrência é impedir esse tipo de ordem de execução.

---

## 36. Controle de concorrência e protocolo 2PL

Depois, o professor ativou o mecanismo de controle de concorrência do SQL Server.

Com os comandos configurados, o SQL Server passou a executar um protocolo chamado **2PL — Two-Phase Locking**.

O 2PL usa mecanismos de bloqueio, ou **locking**.

O professor comparou o conceito com semáforos estudados em Sistemas Operacionais.

Quando dois processos ou transações disputam o mesmo recurso, um precisa esperar o outro.

### 35.1 Resultado com controle de concorrência

Partindo do estado incorreto anterior de 3910, as mesmas operações deveriam produzir:

```text
3910 + 20 - 5 = 3925
```

Com o mecanismo de controle de concorrência ativado e evitando deadlock, o resultado final foi **3925**.

Isso mostrou que o controle de concorrência garantiu a integridade dos dados.

---

## 37. Bloqueios: leitura e escrita

O professor explicou que, para cada operação sobre o banco, há um tipo de bloqueio.

### 36.1 Bloqueio de leitura

O bloqueio de leitura é compatível com ele mesmo.

Isso significa que várias transações podem ler o mesmo objeto ao mesmo tempo.

Exemplo:

- T1 tem bloqueio de leitura sobre `X`;
- T2 também pode receber bloqueio de leitura sobre `X`.

### 36.2 Bloqueio de escrita

O bloqueio de escrita é exclusivo.

Apenas uma transação pode reter um bloqueio de escrita sobre determinado objeto em determinado intervalo de tempo.

Se outra transação possui bloqueio de leitura, a escrita precisa esperar.

### 36.3 Liberação dos bloqueios

Os bloqueios são liberados durante o `COMMIT`.

Enquanto a transação não executa `COMMIT` ou `ROLLBACK`, os bloqueios necessários podem continuar retidos.

---

## 38. Deadlock

Ao remover a estratégia usada para evitar deadlock, o professor mostrou o deadlock acontecendo.

Uma das transações foi cancelada pelo SGBD.

Ela foi escolhida como vítima do deadlock e sofreu `ROLLBACK`.

O SGBD informou que a transação deveria ser reexecutada.

O professor destacou que, em uma aplicação real, esse erro precisa ser tratado pelo programador. O SGBD não necessariamente reexecuta a transação automaticamente.

### 37.1 Como o deadlock ocorreu

Com controle de concorrência ativado:

1. T1 solicita bloqueio de leitura sobre `X`.
2. O bloqueio é concedido.
3. T2 solicita bloqueio de leitura sobre `X`.
4. Como bloqueios de leitura são compatíveis, o bloqueio também é concedido.
5. T2 tenta obter bloqueio de escrita sobre `X`.
6. Não consegue, porque T1 possui bloqueio de leitura.
7. T2 fica esperando T1 liberar o bloqueio.
8. T1 tenta obter bloqueio de escrita sobre `X`.
9. Não consegue, porque T2 possui bloqueio de leitura.
10. T1 fica esperando T2 liberar o bloqueio.

Resultado:

```text
T1 espera T2
T2 espera T1
```

Isso é um deadlock.

### 37.2 Grafo de espera

A detecção de deadlock é feita pela detecção de ciclo em um **grafo de espera**.

- vértices representam transações;
- arestas representam espera por recursos.

Se existe um ciclo, há deadlock.

Para eliminar o ciclo, o SGBD remove um vértice, isto é, aborta uma transação.

Ao abortar uma transação, as arestas associadas a ela desaparecem e o deadlock é resolvido.

O professor observou que o deadlock pode envolver mais de duas transações. Pode haver deadlock com cinco transações, por exemplo. Ainda assim, abortar uma transação envolvida no ciclo pode ser suficiente para quebrá-lo.

O SGBD escolhe a vítima usando algum critério de prioridade ou custo.

---

## 39. UPDLOCK e prevenção de deadlock

Para evitar o deadlock no experimento, o professor usou `UPDLOCK`.

A ideia do `UPDLOCK` é dizer ao SGBD:

> Esta operação é uma leitura, mas futuramente essa mesma transação pretende escrever sobre esse objeto. Portanto, não conceda apenas um bloqueio de leitura comum; conceda um bloqueio de atualização.

O bloqueio de atualização é mais restritivo que o bloqueio de leitura, mas menos restritivo que o bloqueio de escrita.

Ele deve ser usado quando uma transação lê um objeto e depois pretende atualizá-lo.

### 38.1 Upgrade de bloqueio

O professor explicou que muitos deadlocks são provocados por **upgrade de bloqueio**.

Isso ocorre quando uma transação possui um bloqueio menos restritivo, como leitura, e depois tenta elevá-lo para um bloqueio mais restritivo, como escrita.

Segundo o professor, empiricamente se observou que grande parte dos deadlocks em banco de dados ocorre por esse tipo de situação.

### 38.2 Diferença entre UPDLOCK e XLOCK

O professor também citou `XLOCK`, que corresponde a um bloqueio exclusivo de escrita.

Seria possível colocar um bloqueio de escrita logo no início, mas isso seria muito restritivo.

O `UPDLOCK` é uma solução intermediária:

- mais forte que leitura;
- menos forte que escrita;
- ajuda a evitar deadlock quando a transação vai atualizar o objeto depois.

---

## 40. Recuperação do banco de dados

Depois de tratar consistência, o professor passou para a segunda funcionalidade do gerenciador de transações: **recuperação**.

A recuperação também está ligada à integridade dos dados.

A ideia é que, em caso de falha, o SGBD consiga reconstruir um estado consistente do banco.

### 39.1 Falha de memória principal

Durante o funcionamento normal, se o sistema perde o conteúdo da memória principal, o SGBD consegue recuperar operações de transações que executaram `COMMIT`, mas ainda não tinham sido descarregadas em disco.

Isso é feito usando o **arquivo de log**.

### 39.2 Arquivo de log

O arquivo de log registra as operações das transações.

Ele sofre muitas escritas e cresce rapidamente.

Por isso, periodicamente, o log precisa ser descarregado para uma memória terciária, como fita magnética.

O professor destacou que o arquivo de log registra operações, e não uma imagem completa do banco.

### 39.3 Backup

O backup é diferente do log.

O backup é uma imagem do banco de dados em determinado momento.

No PostgreSQL, por exemplo, o termo `dump` é usado para esse tipo de descarga da imagem do banco.

Resumo da diferença:

| Recurso | O que representa |
|---|---|
| Backup | Imagem do banco de dados em um momento específico |
| Log | Registro das operações/transações executadas |

---

## 41. Fita magnética e storage tape

O professor explicou que fita magnética ainda é muito usada em armazenamento corporativo.

Existem sistemas chamados **storage tape**, que são armários com fitas magnéticas e braços robóticos.

O braço robótico:

1. localiza a fita;
2. pega a fita;
3. encaixa a fita no drive;
4. permite a leitura ou gravação;
5. devolve a fita ao local correto.

O professor comparou com a controladora de disco: assim como a controladora sabe onde o dado está no disco, o sistema de storage tape sabe onde cada fita está.

### 40.1 Por que usar fita magnética?

A principal razão é a capacidade de armazenamento.

O professor citou fitas com capacidade extremamente alta, como 185 TB em um cartucho pequeno.

Fitas são úteis para dados que não precisam ser acessados rapidamente, mas precisam ser preservados por muito tempo.

### 40.2 Exemplos de uso

Foram citados exemplos como:

- imagens de satélite da NASA;
- dados gerados continuamente, por exemplo uma imagem a cada dois minutos;
- grandes bancos de dados de empresas como Mastercard ou Visa;
- backups de bancos de dados com volume na ordem de petabytes.

Para esses cenários, armazenar tudo em disco pode exigir data centers enormes. A fita magnética oferece grande capacidade com custo e espaço físico menores.

O acesso é mais lento, mas aceitável para recuperação histórica. Se a organização precisa recuperar uma imagem de satélite de 10 anos atrás, pode esperar alguns minutos enquanto o robô localiza e monta a fita.

---

## 42. Conteúdo do arquivo de log

O professor comentou que depois mostraria o conteúdo do arquivo de log.

O log permite enxergar operações realizadas sobre os dados.

Muita coisa aparece em hexadecimal, mas com ferramentas adequadas é possível converter e interpretar parte das informações.

Isso mostra que o log também precisa ser protegido, porque ele pode revelar operações feitas no banco de dados.

---

## 43. Exemplo de recuperação com backup e logs

O professor apresentou um cenário de recuperação.

### 42.1 Situação inicial

Suponha que foi feito um backup do banco de dados em:

```text
24/05, às 23:59
```

Esse backup representa a imagem do banco naquele momento.

Depois, o arquivo de log em disco continuou recebendo registros de transações.

Quando o log encheu, foi descarregado para fita magnética em:

```text
26/05, às 10:00
```

Após descarregar o log para fita, o log em disco foi zerado e passou a registrar as transações atuais.

### 42.2 Falha de disco

Suponha que houve uma falha e o disco do banco de dados foi perdido.

A explicação assume que:

- o banco de dados estava em um disco;
- o arquivo de log estava em outro disco.

Essa separação é essencial para a recuperação.

### 42.3 Processo de recuperação

O processo seria:

1. Subir o backup de 24/05 às 23:59.
2. Recuperar o estado do banco naquele momento.
3. Aplicar as operações do log descarregado para fita em 26/05 às 10:00.
4. Com isso, recuperar o banco até o momento do descarregamento do log.
5. Aplicar as operações do log atual que estava em disco.
6. Recuperar o banco até o momento imediatamente anterior à falha.

Esse processo permite garantir a integridade dos dados mesmo após perda de disco.

---

## 44. Ideias centrais da aula

A aula apresentou uma visão ampla de segurança em banco de dados a partir da arquitetura e funcionamento interno do SGBD.

Os pontos centrais foram:

1. Segurança em banco de dados exige entender como o SGBD funciona.
2. O maior valor de uma empresa está nos seus dados.
3. Sistemas de arquivos armazenam dados, mas não oferecem os mesmos serviços de um SGBD.
4. O SGBD oferece metadados, catálogo, controle de concorrência, recuperação, restrições de integridade e independência entre dados e aplicações.
5. A arquitetura de três níveis permite abstração e controle de acesso.
6. Visões são mecanismos importantes de segurança.
7. SQL Injection aparece como vulnerabilidade no ponto de entrada das consultas.
8. Dados em repouso são vulneráveis quando arquivos físicos podem ser acessados diretamente.
9. Índices, logs, código armazenado e catálogo também são objetos sensíveis.
10. Otimização de consultas busca reduzir principalmente acesso a disco.
11. Seleção e projeção antes da junção podem reduzir muito o tempo de execução.
12. O gerenciador de transações garante consistência e recuperação.
13. Concorrência sem controle pode gerar atualização perdida.
14. Bloqueios e protocolo 2PL ajudam a garantir integridade.
15. Deadlocks são detectados por ciclos no grafo de espera.
16. UPDLOCK pode evitar deadlocks causados por upgrade de bloqueio.
17. Backup e log são diferentes e complementares.
18. A recuperação depende da combinação entre backup, logs arquivados e log atual.

---

## 45. Glossário rápido

| Termo | Significado |
|---|---|
| SGBD | Sistema Gerenciador de Banco de Dados |
| Dados em repouso | Dados armazenados fisicamente, sem acesso ativo pelo SGBD naquele momento |
| Catálogo | Área do SGBD que armazena metadados |
| Metadados | Dados sobre os dados |
| Chunk | Bloco de armazenamento usado em sistemas como o Google File System |
| Setor | Unidade física de acesso em disco magnético |
| Bloco | Abstração do sistema de arquivos que agrupa setores |
| Visão | Janela lógica que restringe o acesso aos dados |
| Visão virtual | Visão cujo resultado é calculado no momento do acesso |
| Visão materializada | Visão cujo resultado fica armazenado fisicamente |
| SQL Injection | Ataque em que comandos SQL são injetados via entrada da aplicação |
| DDL | Linguagem de definição de dados |
| DML | Linguagem de manipulação de dados |
| Table scan | Leitura completa de uma tabela |
| Index seek | Busca usando índice |
| Join | Operação de junção entre tabelas |
| Seleção | Filtro de tuplas/linhas |
| Projeção | Filtro de atributos/colunas |
| Transação | Unidade lógica de trabalho no banco |
| COMMIT | Confirmação de uma transação |
| ROLLBACK | Desfazimento de uma transação |
| 2PL | Two-Phase Locking, protocolo de bloqueio em duas fases |
| Lock | Bloqueio sobre objeto do banco |
| Deadlock | Situação em que transações esperam umas pelas outras indefinidamente |
| UPDLOCK | Bloqueio de atualização, usado quando uma leitura será seguida de escrita |
| XLOCK | Bloqueio exclusivo |
| Log | Registro das operações das transações |
| Backup | Imagem do banco em determinado momento |
| Storage tape | Sistema de armazenamento em fita magnética, geralmente robotizado |

---

## 46. Resumo para revisão

Um banco de dados não é apenas um conjunto de arquivos com dados relacionados. O diferencial do SGBD está nos mecanismos que ele oferece: catálogo, metadados, controle de acesso, visões, independência de dados, controle de concorrência, recuperação e otimização de consultas.

Esses mecanismos aumentam a capacidade de proteger e gerenciar dados, mas também criam pontos de vulnerabilidade. A entrada de comandos SQL pode permitir SQL Injection. O catálogo revela a estrutura do banco. Arquivos físicos podem ser atacados quando os dados estão em repouso. Código armazenado pode ser abusado se permissões forem mal configuradas. Logs e backups precisam ser protegidos porque são essenciais para recuperação e também podem revelar informações sensíveis.

A integridade dos dados depende fortemente do gerenciador de transações. Sem controle de concorrência, duas transações podem ler o mesmo valor e sobrescrever alterações uma da outra, gerando atualização perdida. Com protocolos como 2PL e mecanismos de bloqueio, o SGBD controla o acesso concorrente. Porém, bloqueios podem gerar deadlocks, que são detectados por ciclos no grafo de espera. O SGBD resolve o problema abortando uma transação.

A recuperação depende da combinação entre backup e arquivo de log. O backup representa uma imagem do banco em um momento. O log registra as operações executadas depois. Em caso de falha, o SGBD restaura o backup e reaplica os logs até chegar ao estado anterior à falha.
