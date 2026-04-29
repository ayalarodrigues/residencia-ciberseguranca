# Segurança em Banco de Dados — Aula 29/04

> **Disciplina:** Segurança em Banco de Dados  
> **Tema da aula:** replicação em bancos de dados, disponibilidade, estratégias de master, propagação de atualizações, SQL Injection, prevenção, detecção de ameaças, auditoria, monitoramento, vazamentos em nuvem, dados em repouso e casos reais de exposição de dados.  

---

## 1. Orientação para a prática da aula seguinte

O professor iniciou a aula orientando sobre a prática do dia seguinte.

A prática seria realizada com **PostgreSQL**, preferencialmente instalado em ambiente Linux. O objetivo seria permitir que os alunos visualizassem, na prática, ataques relacionados a bancos de dados.



---

## 2. Retomada da tríade de segurança: confidencialidade, integridade e disponibilidade

O professor retomou o conceito de segurança em banco de dados a partir da tríade conhecida como **CIA**:

- **Confidencialidade**;
- **Integridade**;
- **Disponibilidade**.

Na fala do professor, houve momentos em que os termos **consistência** e **confidencialidade** apareceram de forma próxima. No contexto da tríade de segurança, o termo esperado é **confidencialidade**. Porém, no contexto de banco de dados, a ideia de **consistência** também aparece como parte da integridade dos dados.

A explicação foi organizada da seguinte forma:

### 2.1 Confidencialidade

A confidencialidade está relacionada a impedir que pessoas ou sistemas não autorizados acessem os dados.

Um ataque contra a confidencialidade ocorre quando um atacante consegue invadir o banco ou acessar os arquivos do banco e visualizar dados que não deveria visualizar.

### 2.2 Integridade

A integridade está relacionada a impedir alteração, dano ou corrupção indevida dos dados.

Um ataque contra a integridade pode ocorrer quando o atacante:

- altera dados indevidamente;
- remove dados;
- corrompe arquivos do banco;
- danifica o arquivo de log;
- torna o banco não recuperável;
- provoca uma execução parcial de transação.

### 2.3 Disponibilidade

A disponibilidade está relacionada a garantir que os dados e serviços estejam acessíveis quando usuários legítimos precisarem deles.

Um ataque contra a disponibilidade não necessariamente altera ou remove os dados. O banco pode continuar íntegro, mas o usuário legítimo não consegue acessá-lo.

Esse é o caso de ataques de **negação de serviço**.

---

## 3. Disponibilidade e ataques DDoS

Ao tratar da disponibilidade, o professor retomou o caso da empresa **Dyn**, um provedor de DNS que sofreu um ataque de **DDoS**.

DDoS significa **Distributed Denial of Service**, ou seja, **negação de serviço distribuída**.

A ideia do ataque é inundar um servidor ou serviço com uma quantidade muito grande de mensagens, requisições ou dados. O objetivo é sobrecarregar o serviço de forma que ele não consiga mais atender usuários legítimos.

No caso citado, o ataque à Dyn afetou serviços como:

- Twitter;
- Netflix;
- GitHub.

Esses serviços não foram necessariamente atacados diretamente em seus próprios bancos de dados. Eles foram afetados porque dependiam da infraestrutura de DNS da Dyn.

O professor reforçou que, em segurança, muitas vezes um serviço é derrubado por dependências externas. Atacar uma infraestrutura intermediária pode causar indisponibilidade em vários serviços que dependem dela.

---

## 4. RAID e replicação como estratégias de disponibilidade

Como forma de mitigar problemas de disponibilidade, o professor retomou o mecanismo de **RAID**.

O RAID ajuda porque trabalha com:

- **distribuição** dos dados entre discos;
- **redundância**;
- em alguns níveis, **replicação**.

Se um disco for atacado ou falhar, outros discos podem permitir a recuperação ou continuidade do acesso aos dados, dependendo do nível de RAID adotado.

O professor também destacou que a replicação não depende necessariamente de RAID. Um sistema de banco de dados pode ter mecanismos próprios de replicação.

Ou seja, existem duas ideias diferentes:

```text
RAID → replicação ou redundância no nível de discos
Replicação do SGBD → cópias do banco ou de objetos do banco em vários nós
```

---

## 5. Replicação em bancos de dados

A replicação em bancos de dados consiste em manter cópias dos dados em diferentes nós de uma rede.

O professor explicou que a replicação pode ser feita de várias formas.

### 5.1 Replicação de todo o banco

Uma primeira estratégia é replicar o banco de dados inteiro.

Nesse caso, diferentes nós da rede possuem cópias completas do banco.

Essa estratégia pode aumentar a disponibilidade, pois, se uma réplica estiver indisponível, outra pode responder às consultas.

Também pode ajudar na performance de leitura, pois as requisições podem ser distribuídas entre várias réplicas.

### 5.2 Replicação de objetos específicos

Outra estratégia é replicar apenas alguns objetos do banco, como uma tabela muito acessada.

O professor deu o exemplo de uma tabela mais utilizada. Em vez de replicar todo o banco, a empresa pode replicar somente essa tabela para distribuir a carga de leitura sobre ela.

Essa estratégia existe porque replicação tem custo computacional alto. Sempre que um dado replicado é alterado, essa alteração precisa ser propagada para as demais réplicas.

Portanto, replicar apenas objetos específicos pode reduzir o custo de manter as réplicas consistentes.

---

## 6. Estratégia com um único Master

Uma estratégia comum é definir que apenas uma réplica permite operações de escrita.

Essa réplica é chamada de **Master**.

Nesse modelo:

```text
Leitura  → pode ocorrer em qualquer réplica
Escrita  → só pode ocorrer no Master
```

O professor explicou que isso reduz a complexidade de manter a consistência entre as réplicas, pois todas as escritas entram por um ponto único.

Por outro lado, essa arquitetura cria uma desvantagem:

- todas as operações de escrita ficam concentradas no Master;
- a carga de trabalho de escrita deixa de ser balanceada;
- o Master pode se tornar gargalo.

Mesmo assim, o custo computacional de manter a consistência é menor do que permitir escrita em qualquer réplica.

---

## 7. Replicação de objetos com Masters diferentes

O professor explicou que também é possível definir Masters diferentes para objetos diferentes do banco.

Exemplo:

```text
Tabela 1 → Master na réplica 2
Tabela 2 → Master na réplica 3
```

Nesse caso:

- as escritas sobre a tabela 1 só podem ocorrer na réplica 2;
- as escritas sobre a tabela 2 só podem ocorrer na réplica 3;
- as leituras podem continuar ocorrendo em várias réplicas.

Essa estratégia distribui melhor a carga de escrita, pois não concentra todas as escritas em um único Master.

O professor destacou que essa arquitetura ainda é mais controlável do que permitir escrita livre em qualquer réplica.

---

## 8. Escrita em qualquer réplica e custo de consistência

A possibilidade mais complexa é permitir que qualquer réplica receba operações de escrita.

O professor afirmou que nunca ouviu falar de uma grande empresa ou de um grande banco de dados replicado trabalhando com essa estratégia de forma ampla, porque o custo computacional é muito alto.

O problema central é a consistência.

Se duas transações alteram o mesmo objeto `X` em réplicas diferentes, o sistema precisa garantir que todas as réplicas cheguem ao mesmo valor correto.

O professor destacou um ponto importante:

> O bloqueio sempre é local, nunca global.

Isso significa que uma transação em uma réplica pode bloquear um objeto localmente, mas outra transação em outra réplica pode estar alterando o mesmo objeto em outro nó.

Como os bloqueios são locais, garantir consistência global entre réplicas se torna muito caro.

---

## 9. Propagação das atualizações entre réplicas

Quando uma transação altera um dado no Master, essa alteração precisa ser propagada para as demais réplicas.

O professor explicou três estratégias gerais.

### 9.1 Propagação imediata durante a transação

Uma possibilidade é propagar a atualização para as outras réplicas enquanto a transação ainda está em execução.

Nesse caso, a transação só termina depois que as demais réplicas confirmarem que receberam e aplicaram a atualização.

Essa estratégia garante mais consistência, mas pode deixar a transação muito lenta.

### 9.2 Propagação no commit usando 2PC

A estratégia mais utilizada, segundo o professor, é propagar as alterações na hora do **commit**.

Nesse caso, o sistema usa a ideia do protocolo **2PC — Two-Phase Commit**.

A lógica é:

1. a transação altera o dado no Master;
2. no momento do commit, o Master propaga a alteração para as demais réplicas;
3. as réplicas votam informando se conseguiram aplicar a alteração;
4. se tudo estiver correto, a transação é efetivada;
5. a transação que escreveu fica esperando a confirmação das réplicas.

Essa estratégia aumenta a consistência, mas também aumenta o tempo de conclusão da transação.

### 9.3 Propagação preguiçosa

Outra estratégia é a **propagação preguiçosa**.

Nesse modelo:

1. a transação termina no Master;
2. o commit é feito localmente;
3. em algum momento futuro, o sistema propaga a alteração para as demais réplicas.

A vantagem é que a transação termina mais rapidamente.

A desvantagem é que, durante algum tempo, as réplicas podem ficar com valores diferentes.

O professor explicou que essa estratégia aceita a ideia de consistência eventual:

```text
Em algum momento futuro, as réplicas deverão ficar consistentes.
```

Mas não há garantia de consistência imediata.

---

## 10. Leitura no Master e nas demais réplicas

Durante a discussão, surgiu a pergunta sobre o que acontece quando a propagação é preguiçosa e as demais réplicas ainda não receberam a atualização.

O professor explicou que o Master pode receber tanto leitura quanto escrita.

As demais réplicas, no modelo com Master único, normalmente recebem apenas leitura.

Assim:

```text
Master → leitura e escrita
Réplicas secundárias → leitura
```

Na propagação preguiçosa, pode ocorrer de uma leitura feita em uma réplica secundária retornar um valor ainda desatualizado.

Esse é o preço pago pela maior disponibilidade e menor tempo de resposta das transações de escrita.

---

## 11. Instalação e funcionamento de um banco replicado

Um aluno perguntou como a replicação funciona na instalação: se o banco cria arquivos em outra máquina, se cria pastas, se guarda arquivos em outro nó etc.

O professor explicou que, para existir um banco de dados replicado, é necessário instalar o servidor de banco de dados nos vários nós participantes.

Esses nós precisam se comunicar entre si.

Na instalação, o sistema precisa conhecer quais máquinas fazem parte da rede replicada.

O professor explicou que, embora os bancos usem protocolos de rede tradicionais, também existem protocolos próprios do SGBD para permitir a comunicação entre os nós.

No caso da Oracle, por exemplo, há protocolos e mecanismos específicos para que os nós do banco consigam conversar.

A ideia é:

```text
Cada nó possui o SGBD instalado
Os nós conhecem uns aos outros
O sistema sabe quem é Master
O sistema sabe quem participa da replicação
As mensagens entre nós seguem protocolos próprios do SGBD
```

---

## 12. Introdução à SQL Injection

Após a discussão sobre replicação, o professor iniciou o tema principal da aula: **SQL Injection**.

SQL Injection, ou injeção de SQL, consiste em inserir código SQL dentro de uma consulta que será enviada ao banco de dados.

O objetivo do atacante é manipular a consulta original construída pela aplicação.

A ideia básica é:

```text
Consulta original da aplicação + entrada maliciosa do usuário = consulta SQL alterada
```

O professor explicou que o atacante tenta incluir código SQL em campos de entrada da aplicação, como:

- tela de login;
- campo de busca;
- campo de código de cliente;
- formulário de consulta;
- qualquer campo que seja usado para montar uma consulta SQL.

Quando a injeção funciona, o atacante pode conseguir:

- consultar dados indevidamente;
- acessar tabelas que não deveria acessar;
- alterar dados;
- remover dados;
- descobrir informações sobre o esquema do banco;
- testar a vulnerabilidade da aplicação.

O professor reforçou que o atacante precisa conhecer SQL e precisa inferir detalhes sobre a aplicação.

---

## 13. Consulta construída com entrada do usuário

O ponto de partida do SQL Injection é a construção de uma consulta SQL usando dados digitados pelo usuário.

Em uma tela de login, por exemplo, o usuário informa:

- username;
- senha.

A aplicação pode montar internamente uma consulta parecida com:

```sql
SELECT *
FROM users
WHERE username = '<input_do_usuario>';
```

O problema ocorre quando a aplicação concatena diretamente o que o usuário digitou dentro da string SQL.

Se a entrada não for validada, o usuário pode digitar um trecho que altera a lógica da consulta.

---

## 14. Injeção com condição sempre verdadeira: `OR 1=1`

A estratégia mais simples apresentada pelo professor foi inserir uma condição sempre verdadeira.

Exemplo de entrada maliciosa:

```sql
' OR 1=1 --
```

A ideia é transformar a condição da consulta em algo que sempre seja verdadeiro.

Se a consulta original fosse:

```sql
SELECT *
FROM users
WHERE username = '<input>';
```

A entrada do atacante poderia fazer a consulta se transformar em algo como:

```sql
SELECT *
FROM users
WHERE username = '' OR 1=1;
```

Como `1=1` é sempre verdadeiro, a consulta pode retornar todos os registros da tabela.

O professor mostrou essa lógica usando uma tabela de clientes. Primeiro, executou uma consulta que não retornava nada porque o nome informado não existia. Depois, ao adicionar uma condição sempre verdadeira, a consulta passou a retornar todos os clientes.

A conclusão foi:

```text
Se uma condição falsa é combinada com OR 1=1, o resultado lógico passa a ser verdadeiro.
```

---

## 15. Por que o atacante não precisa saber o nome da coluna da consulta original

Durante a aula, um aluno perguntou se o atacante precisaria saber o nome da coluna, por exemplo `username`.

O professor explicou que, nesse tipo de ataque, o atacante não precisa necessariamente saber o nome da coluna usada internamente pela aplicação.

Isso acontece porque o nome da coluna já está na consulta escrita pelo desenvolvedor dentro da aplicação.

O atacante controla apenas o trecho correspondente ao input.

Exemplo:

```text
A aplicação já possui:
SELECT * FROM users WHERE username = ...

O atacante controla apenas:
<input>
```

Então, mesmo sem saber o nome do atributo, o atacante pode tentar inserir um trecho de SQL no campo de entrada.

---

## 16. Uso de comentários para neutralizar o restante da consulta

Um aluno perguntou o que aconteceria se a consulta tivesse outras condições depois do campo injetado.

Exemplo:

```sql
SELECT *
FROM users
WHERE username = '<input>'
AND password = '<senha>';
```

A dúvida era se o atacante poderia inserir algo para comentar o restante da consulta.

O professor explicou que o atacante precisa conhecer SQL para construir uma entrada que mantenha a consulta sintaticamente correta.

Em ataques de SQL Injection, é comum o uso de comentários para ignorar o restante da consulta original.

Exemplo conceitual:

```sql
' OR 1=1 --
```

O `--` pode comentar o que vier depois, dependendo do SGBD e da sintaxe aceita.

O professor reforçou que o atacante precisa entender a sintaxe SQL, porque, se montar uma entrada inválida, a consulta pode gerar erro.

---

## 17. Erros como fonte de informação para o atacante

O professor explicou que, em alguns casos, provocar erro faz parte da estratégia do atacante.

Se a aplicação exibe diretamente a mensagem de erro retornada pelo banco de dados, o atacante pode descobrir informações úteis, como:

- nome de coluna;
- tipo de dado esperado;
- nome de tabela;
- estrutura parcial da consulta;
- comportamento do SGBD.

Exemplo de informação que pode aparecer em uma mensagem de erro:

```text
Coluna inválida
Tipo de dado incompatível
Objeto não definido
```

Se o banco informa que determinada coluna é inválida, o atacante aprende que aquele nome não existe e pode testar outro.

Se o banco informa que o tipo enviado é incompatível, o atacante aprende o tipo esperado.

O professor destacou que uma aplicação segura deve tratar os erros. Ela não deve simplesmente repassar para o usuário final a mensagem completa retornada pelo SGBD.

---

## 18. Tamanho do retorno e conhecimento da aplicação

Um aluno perguntou se uma aplicação conseguiria receber uma resposta muito grande, por exemplo todos os registros de uma tabela.

O professor respondeu que isso depende da aplicação.

O atacante precisa conhecer ou testar o comportamento da aplicação:

- se ela mostra várias linhas;
- se limita a quantidade de registros;
- se pagina o resultado;
- se retorna erro;
- se exibe mensagens do banco.

A exploração depende das brechas existentes.

O professor destacou que o atacante não procura uma aplicação que tenha previsto todos os problemas de segurança. Ele procura justamente a aplicação que tem falhas.

---

## 19. Aplicação autenticada e acesso indireto ao banco

Um ponto importante explicado pelo professor foi que, em muitos casos, quem está autenticado no banco de dados é a aplicação, e não o usuário final.

O usuário acessa a aplicação. A aplicação, por sua vez, já possui uma conexão legítima com o banco.

Assim, se o atacante consegue injetar SQL através da aplicação, ele aproveita a conexão legítima que a aplicação já possui.

O banco enxerga a consulta como vindo de uma aplicação autorizada.

Isso dificulta a defesa diretamente pelo SGBD, porque, do ponto de vista do banco, a requisição veio por uma conexão válida.

---

## 20. SQL Injection baseada em tempo

O professor explicou uma técnica de teste chamada **time-based SQL Injection**.

A ideia é inserir um comando que provoque atraso na execução, como `WAIT FOR DELAY`, e observar o tempo de resposta da aplicação.

Se a aplicação demora exatamente o tempo esperado, o atacante infere que conseguiu injetar código SQL.

A lógica é:

```text
1. O atacante envia uma entrada normal e mede o tempo de resposta.
2. Depois envia uma entrada com WAIT FOR DELAY.
3. Se a resposta demora mais, há indício de que o SQL foi executado.
4. A partir daí, o atacante sabe que aquele campo é uma possível brecha.
```

O professor explicou que, antes de tentar extrair dados, o atacante normalmente tenta identificar se algum ponto da aplicação permite injeção.

---

## 21. SQL Injection out-of-band e portas do banco

O professor também mencionou a categoria **out-of-band**.

A ideia envolve conexões externas ou canais alternativos usados para obter retorno sobre a exploração.

Ele destacou que todo banco de dados possui uma porta em que fica ouvindo conexões.

Se o atacante descobre a porta e ela está acessível, pode tentar se conectar diretamente ao banco.

O professor comentou que portas padrão de SGBDs são amplamente conhecidas. Também são conhecidos muitos usuários administrativos padrão.

Isso torna perigoso manter configurações default.

Exemplos de riscos citados:

- porta padrão exposta;
- usuário administrativo padrão;
- senha padrão;
- credenciais não alteradas após instalação;
- serviço de banco acessível pela rede sem necessidade.

---

## 22. SQL Injection baseada em erro

Outra estratégia apresentada foi provocar erros para obter informações.

O atacante injeta um trecho de SQL e observa se a aplicação retorna mensagens do banco.

Se a aplicação não tratar o erro, o atacante pode receber mensagens como:

```text
Invalid column name
Objeto não encontrado
Tipo incompatível
```

A partir disso, ele pode inferir:

- quais colunas existem;
- quais colunas não existem;
- quais tipos de dados são esperados;
- qual SGBD está sendo usado;
- quais tabelas podem estar envolvidas.

O professor reforçou que esse comportamento ocorre quando a aplicação não filtra nem trata adequadamente os erros vindos do banco.

---

## 23. SQL Injection com `UNION`

O professor explicou outra estratégia muito utilizada: injeção usando a cláusula `UNION`.

O `UNION` permite unir o resultado de duas consultas SQL.

Exemplo conceitual:

```sql
SELECT coluna1, coluna2
FROM tabela1
UNION
SELECT coluna1, coluna2
FROM tabela2;
```

O resultado de uma consulta SQL é uma tabela. Quando se usa `UNION`, o banco tenta unir duas tabelas resultantes.

Para que isso funcione, as duas consultas precisam ser compatíveis.

Segundo o professor, a compatibilidade exige:

1. o mesmo número de atributos;
2. tipos de dados compatíveis em cada posição.

Exemplo:

```text
Consulta 1 retorna 2 colunas → Consulta 2 também precisa retornar 2 colunas
Coluna 1 da consulta 1 precisa ser compatível com coluna 1 da consulta 2
Coluna 2 da consulta 1 precisa ser compatível com coluna 2 da consulta 2
```

Se a quantidade ou o tipo de atributos não forem compatíveis, o banco retorna erro.

Esse erro, se exibido pela aplicação, pode ajudar o atacante a ajustar a injeção.

---

## 24. Objetivo do `UNION` no ataque

O professor explicou que o atacante pode encontrar uma brecha em uma tela ou campo que consulta uma determinada tabela, mas seu interesse real pode ser outra tabela.

Por exemplo:

- a aplicação consulta uma tabela de clientes;
- o atacante quer acessar a tabela de usuários;
- ele tenta usar `UNION` para combinar a consulta original com uma consulta sobre a tabela de usuários.

Exemplo conceitual:

```sql
SELECT coluna1, coluna2
FROM tabela_original
WHERE condição
UNION
SELECT usuario, senha
FROM users;
```

Para isso, o atacante precisa inferir:

- nome da tabela desejada;
- nomes das colunas;
- quantidade de colunas da consulta original;
- tipos das colunas.

O professor ressaltou que esse processo pode envolver muitas tentativas.

---

## 25. Dificuldade do SGBD para impedir SQL Injection

Um aluno perguntou se os SGBDs possuem mecanismos próprios para prevenir SQL Injection ou DDoS.

O professor explicou que, no caso de SQL Injection, o problema geralmente está na aplicação.

O SGBD recebe um comando SQL válido. Ele não tem como saber se aquele comando representa uma lógica legítima da aplicação ou uma entrada maliciosa digitada por um atacante.

Se a aplicação envia:

```sql
SELECT * FROM users WHERE username = '' OR 1=1;
```

Do ponto de vista do banco, é apenas uma consulta SQL válida.

Portanto, a principal defesa precisa estar na aplicação.

O professor explicou que a aplicação deve:

- validar entrada;
- filtrar comandos indevidos;
- tratar erros;
- não montar SQL por concatenação simples;
- limitar permissões da conexão usada;
- evitar repassar mensagens internas do banco ao usuário.

---

## 26. DDoS contra banco de dados

Sobre DDoS, o professor comentou que é muito difícil para o SGBD identificar sozinho uma tentativa de negação de serviço.

Um ataque pode ocorrer por:

- muitas conexões simultâneas;
- muitas requisições feitas pelo mesmo cliente;
- consultas muito pesadas;
- junções complexas;
- consultas OLAP demoradas;
- consumo excessivo de CPU, memória ou disco.

O problema é que consultas legítimas também podem ser pesadas.

Em ambientes OLAP, por exemplo, consultas podem demorar horas ou até dias. Portanto, o banco não pode simplesmente assumir que uma consulta demorada é maliciosa.

O professor reforçou que, para o SGBD, é difícil distinguir automaticamente uma consulta legítima pesada de uma consulta maliciosa feita para consumir recursos.

---

## 27. WAF e defesa no caminho da aplicação

Durante a discussão, foi citado o uso de mecanismos intermediários, como firewall e **WAF — Web Application Firewall**.

O WAF atua na camada da aplicação web e pode identificar padrões suspeitos antes que eles cheguem ao banco.

Porém, o professor ressaltou que, quando a injeção ocorre por meio de uma aplicação legítima, o banco tende a confiar naquela aplicação.

A aplicação já tem credenciais e conexão autorizada.

Se o SGBD tentasse verificar constantemente se aquela aplicação é confiável a cada consulta, haveria grande impacto de performance.

Por isso, a defesa precisa ser dividida entre:

- aplicação;
- infraestrutura de rede;
- controle de acesso;
- monitoramento;
- auditoria;
- configuração segura do SGBD.

---

## 28. Prevenção de SQL Injection

O professor destacou algumas estratégias de prevenção.

### 28.1 Validação de entrada

A principal forma destacada foi validar a entrada do usuário na aplicação.

Se o campo espera um número, a aplicação deve aceitar apenas números.

Se o campo espera um nome, a aplicação deve tratar caracteres especiais, aspas, operadores e palavras reservadas.

O professor enfatizou que evitar SQL Injection tem muita relação com **código seguro**.

### 28.2 Prepared Statements e consultas parametrizadas

Segundo a transcrição, também foi destacada a ideia de **Prepared Statements** ou consultas parametrizadas.

Nesse modelo, o comando SQL e os dados do usuário são enviados separadamente.

Em vez de concatenar a entrada diretamente na string SQL, a aplicação usa parâmetros.

A ideia é que, se o usuário digitar `OR 1=1`, isso será tratado como valor literal, e não como parte do comando SQL.

Exemplo conceitual:

```sql
SELECT *
FROM users
WHERE username = ?;
```

O valor do `?` é tratado como dado, não como código.

### 28.3 Stored Procedures

Também foi mencionada a possibilidade de usar **Stored Procedures**, desde que sejam bem escritas.

Elas podem reduzir a concatenação direta de SQL na aplicação, mas não resolvem o problema se internamente também montarem SQL dinâmico sem validação.

### 28.4 Tratamento de erros

A aplicação não deve exibir diretamente mensagens internas do banco.

Mensagens detalhadas podem ajudar o atacante a descobrir a estrutura do banco.

### 28.5 Restrição de permissões

Mesmo que ocorra uma injeção, as permissões da conexão usada pela aplicação devem ser limitadas.

Se a aplicação só precisa consultar a tabela de clientes, ela não deve ter permissão para acessar outras tabelas.

---

## 29. Restringir permissões ao banco de dados

O professor explicou que uma forma importante de reduzir o impacto da SQL Injection é restringir as permissões da aplicação no banco de dados.

Exemplo:

```text
A aplicação só precisa acessar a tabela cliente.
Logo, a conexão dessa aplicação só deve ter permissão nessa tabela.
```

Se um atacante tentar usar `UNION` para acessar outra tabela, a consulta não funcionará se a aplicação não tiver privilégio sobre essa outra tabela.

Essa restrição pode ser feita com comandos como:

```sql
GRANT SELECT ON cliente TO usuario_aplicacao;
REVOKE SELECT ON outra_tabela FROM usuario_aplicacao;
```

O professor destacou o papel do DBA na concessão e revogação de privilégios.

---

## 30. Uso de visões como mecanismo de restrição

Além de permissões diretas em tabelas, o professor explicou que é possível usar **visões**.

A aplicação pode ser autorizada a acessar apenas determinadas visões, e não as tabelas completas.

Exemplo:

```text
Tabela cliente possui: nome, endereço, CPF, renda, limite de crédito.
Visão cliente_publico possui apenas: nome e endereço.
```

Se a aplicação acessa somente a visão, mesmo que ocorra injeção, o atacante fica limitado aos dados expostos por essa visão.

O professor explicou que isso reduz a chance de acessar atributos sensíveis.

---

## 31. DCL, GRANT, REVOKE e Roles

Segundo a transcrição, o professor retomou o controle de acesso usando a linguagem **DCL — Data Control Language**.

Os comandos principais são:

```sql
GRANT
REVOKE
```

O `GRANT` concede permissões.

Exemplo:

```sql
GRANT SELECT ON tabela TO usuario;
```

O `REVOKE` remove permissões.

Exemplo:

```sql
REVOKE SELECT ON tabela FROM usuario;
```

Também foi retomado o conceito de **Roles**, ou papéis.

Em vez de conceder permissões individualmente para cada usuário, cria-se um papel com permissões específicas e associa-se usuários a esse papel.

Exemplo:

```text
Role: Vendedores
Permissões: SELECT e INSERT na tabela de vendas
Usuários: João, Maria, Ana
```

Isso facilita a administração e reduz o risco de permissões residuais quando um funcionário muda de função.

---

## 32. Princípio do menor privilégio

O professor retomou o princípio do menor privilégio.

A ideia é conceder apenas as permissões necessárias para que um usuário, aplicação ou processo execute sua função.

Exemplo:

```text
Se a aplicação só precisa ler dados, ela recebe apenas SELECT.
Se não precisa alterar, não recebe UPDATE.
Se não precisa remover, não recebe DELETE.
Se não administra objetos, não recebe DROP.
```

Esse princípio reduz o impacto de uma invasão.

Se uma conta de aplicação for comprometida, o atacante ficará limitado ao conjunto de permissões daquela conta.

---

## 33. Casos reais de SQL Injection

O professor apresentou casos reais de ataques envolvendo SQL Injection.

### 33.1 Yahoo

O Yahoo foi citado como uma empresa que sofreu ataque por injeção de SQL.

O professor destacou que o Yahoo era uma grande empresa de tecnologia e, mesmo assim, deixou uma brecha explorável.

No caso citado, aproximadamente **450 mil dados de usuários** foram expostos, incluindo senhas.

A vulnerabilidade apontada foi **validação de entrada fraca**.

A conclusão do professor foi que validar entradas continua sendo uma das formas mais eficientes de prevenir SQL Injection.

### 33.2 TalkTalk

A empresa TalkTalk, do setor de telecomunicações, também foi citada.

O ataque expôs dados de mais de **157 mil usuários**, incluindo dados financeiros.

Segundo o professor, a vulnerabilidade estava em um sistema legado, provavelmente antigo, que não havia sido revisado adequadamente do ponto de vista de segurança.

---

## 34. Sistemas legados como fonte de vulnerabilidades

O professor dedicou uma parte importante da aula ao problema dos sistemas legados.

Ele explicou que grandes empresas muitas vezes funcionam como uma **colcha de retalhos** de sistemas.

Isso acontece porque empresas grandes compram empresas menores e incorporam seus sistemas, dados e aplicações.

Como há pressão para continuar gerando valor e receita, muitas vezes não há tempo para:

- migrar os sistemas;
- reescrever aplicações;
- revisar código;
- fazer auditoria de segurança;
- validar corretamente o acesso ao banco de dados.

O resultado é a permanência de sistemas antigos em produção.

Esses sistemas podem conter vulnerabilidades antigas, inclusive SQL Injection.

---

## 35. Exemplo do governo e sistemas integrados

O professor citou sistemas governamentais como exemplo de integração de sistemas legados.

Ele mencionou que plataformas como `gov.br` e `Sou Gov` funcionam, em grande parte, como camadas de identificação e acesso a vários sistemas.

O risco é que, se o usuário consegue se autenticar e acessar um sistema legado que possui vulnerabilidade no acesso ao banco de dados, ele pode explorar essa vulnerabilidade.

A preocupação central é:

```text
Um sistema de autenticação moderno pode estar por cima de sistemas antigos vulneráveis.
```

---

## 36. COBOL e manutenção de sistemas antigos

O professor citou sistemas de compra de passagem aérea e sistemas como o Amadeus como exemplos de aplicações antigas ainda usadas em larga escala.

Ele afirmou que muitos desses sistemas são escritos em **COBOL**.

Segundo o professor, grandes empresas ainda executam muito código em linguagens antigas, não necessariamente Java, Python ou tecnologias mais modernas.

COBOL foi descrito como uma linguagem relativamente fácil de ler, por ter seções bem definidas, mas difícil de manter porque há poucos profissionais com domínio prático da linguagem.

A dificuldade é que mexer em sistemas críticos antigos pode gerar alto risco.

Por isso, muitas empresas evitam alterar o código legado, mesmo quando ele pode conter vulnerabilidades.

---

## 37. Segurança e desenvolvimento terceirizado

O professor também relacionou vulnerabilidades a serviços terceirizados.

Quando uma empresa contrata outra para desenvolver um serviço, ela pode não conhecer a qualidade do código produzido.

A empresa contratante pode não ter controle suficiente sobre:

- práticas de desenvolvimento seguro;
- validação de entrada;
- tratamento de credenciais;
- permissões em nuvem;
- testes de segurança;
- revisão de código.

Esse ponto foi retomado mais adiante no caso do Facebook, em que uma aplicação terceirizada expôs dados por má configuração.

---

## 38. Detecção de ameaças em bancos de dados

O professor passou a discutir estratégias de detecção de ameaças em bancos de dados.

Ele reforçou que o objetivo não é eliminar completamente a vulnerabilidade, mas **mitigar**.

Não existe eliminação total de vulnerabilidades, porque novas formas de ataque podem surgir.

As principais estratégias citadas foram:

- monitoramento;
- detecção de anomalias;
- detecção baseada em assinaturas;
- sistemas de detecção de intrusão;
- auditoria e alertas em tempo real;
- análise de comportamento com Machine Learning;
- monitoramento de controle de acesso;
- checagens de integridade;
- integração com inteligência de ameaças.

---

## 39. Monitoramento de transações e consultas

A primeira estratégia é monitorar as transações e consultas executadas sobre o banco.

A ideia é conhecer a carga de trabalho normal do banco de dados.

A partir desse padrão, o sistema pode identificar requisições que fogem do comportamento esperado.

Exemplo:

```text
Uma aplicação normalmente só consulta dados.
De repente, começa a executar DELETE em várias tabelas.
```

Esse comportamento pode indicar anomalia.

O professor destacou que uma ferramenta assim não é perfeita. Ela só conhece os padrões já observados. Se surgir algo novo, pode ser difícil saber se é legítimo ou malicioso.

---

## 40. Detecção de anomalias

A detecção de anomalias procura identificar mudanças de comportamento.

Exemplo dado pelo professor:

```text
Uma transação ou aplicação que sempre lia dados passa a remover tudo.
```

Isso pode indicar comportamento malicioso ou comprometimento.

O desafio é que o sistema precisa conhecer o comportamento esperado de cada aplicação ou transação.

Esse monitoramento tem custo e pode impactar a performance do banco.

O professor comentou que usar o arquivo de log para isso também não resolve completamente, porque o log de recuperação registra operações de escrita, mas não registra operações de leitura.

Assim, se uma transação estava apenas lendo e depois começa a remover tudo, o log de recuperação mostrará a remoção, mas não necessariamente permitirá reconstruir todo o comportamento anterior de leitura.

---

## 41. Necessidade de um profissional híbrido: DBA + Segurança

O professor refletiu sobre a necessidade de um profissional que una conhecimento de banco de dados e segurança.

Ele comentou que o DBA tradicional costuma estar muito focado em performance:

- por que o banco está lento;
- como otimizar consulta;
- como melhorar tempo de resposta;
- como atender demanda de gestores por consultas rápidas.

Por outro lado, profissionais de cibersegurança muitas vezes focam muito em rede, serviços de nuvem e infraestrutura, deixando banco de dados em segundo plano.

O professor sugeriu que existe espaço para um profissional especializado em segurança de banco de dados, alguém que conheça tanto cibersegurança quanto os componentes internos de SGBDs.

A mensagem principal foi:

```text
Para identificar vulnerabilidades em SGBDs, é necessário conhecer SGBDs.
Para identificar risco de SQL Injection, é necessário conhecer SQL.
```

---

## 42. Detecção baseada em assinatura

A detecção baseada em assinatura consiste em reconhecer padrões conhecidos de ataque.

Se o sistema conhece um padrão de SQL Injection, por exemplo, pode gerar alerta quando uma requisição semelhante aparece.

O professor ressaltou que essa estratégia é limitada, pois atacantes eficientes tentam criar novas formas de ataque justamente para escapar de assinaturas conhecidas.

Assim, assinaturas ajudam, mas não resolvem o problema sozinhas.

---

## 43. Sistemas de detecção de intrusão

O professor citou sistemas de detecção de intrusão.

Eles podem ajudar em alguns cenários, mas não resolvem necessariamente SQL Injection.

O motivo é que, em um ataque de SQL Injection, a requisição pode passar por uma aplicação legítima.

A aplicação não é um intruso. Ela está conectada normalmente ao banco.

Logo, o sistema de detecção pode não perceber que o comando enviado ao banco foi manipulado por um atacante.

---

## 44. Auditoria e alertas em tempo real

O professor comparou auditoria e alertas em tempo real com sistemas usados por empresas de cartão de crédito.

Quando um cartão é usado de forma fora do padrão, o sistema pode identificar uma possível fraude e suspender a transação até confirmar com o cliente.

Exemplo dado:

```text
O usuário passou o dia usando o cartão em Fortaleza.
Uma hora depois aparece uma transação em Recife.
O sistema pode suspeitar de fraude.
```

A estratégia é proativa: bloquear ou suspender antes de efetivar a transação.

Porém, o professor destacou um efeito colateral: falsos positivos.

Um uso legítimo pode ser interpretado como fraude.

Ele citou o caso de um colega que estava no exterior e teve uma transação suspensa ao tentar pagar um hotel. O uso era legítimo, mas fugia do padrão esperado.

---

## 45. Machine Learning e análise de comportamento

Segundo o professor, técnicas de Machine Learning podem ser usadas para identificar padrões de comportamento de usuários ou aplicações.

A ideia é detectar comportamentos que fogem do padrão esperado.

Isso pode ser aplicado a:

- usuários individuais;
- aplicações;
- conexões;
- transações;
- consultas;
- tentativas de login.

A transcrição também menciona a ideia de **UEBA — User and Entity Behavior Analytics**, que é uma abordagem baseada na análise de comportamento de usuários e entidades para detectar atividades suspeitas.

---

## 46. Monitoramento de acesso e controle

Outra estratégia citada é monitorar quem tenta se logar no banco de dados.

Isso inclui observar:

- tentativas de login bem-sucedidas;
- tentativas de login falhas;
- horários incomuns;
- origem das conexões;
- mudanças no padrão de acesso;
- usuários com comportamento anômalo.

Esse monitoramento pode ajudar a identificar tentativas de invasão, abuso de credenciais ou uso indevido de contas legítimas.

---

## 47. Checagem de integridade e checksum

O professor explicou a ideia de checagem de integridade dos dados.

Uma estratégia é usar mecanismos como **checksum** para identificar se houve alteração indevida.

O professor comparou o checksum com a ideia de bit de paridade, já discutida na aula sobre RAID.

### 47.1 Bit de paridade

A ideia do bit de paridade é adicionar redundância para detectar erro.

Se a paridade for par, a quantidade de bits `1` deve ser par.

Se a paridade for ímpar, a quantidade de bits `1` deve ser ímpar.

Assim, se um bit for perdido ou alterado, é possível detectar que há inconsistência.

Em alguns casos, pode ser possível corrigir o erro.

### 47.2 Checksum no banco

No banco de dados, a ideia é semelhante: verificar se os dados mantêm uma determinada propriedade de integridade.

Porém, o professor destacou que isso não garante 100% de proteção.

É necessário definir qual técnica será usada, e mesmo assim ainda podem existir formas de alteração inconsistente que não sejam detectadas.

---

## 48. Integração com inteligência de ameaças

A transcrição menciona uma estratégia de integração com inteligência de ameaças.

Um exemplo seria manter listas de IPs associados a ataques conhecidos.

Se uma requisição vier de um IP presente em uma lista negra, o sistema pode bloquear ou gerar alerta.

O professor observou que essa estratégia também tem limitações, pois atacantes podem mudar de IP, usar proxies, botnets ou infraestrutura comprometida.

---

## 49. Complexidade da detecção de ameaças

O professor concluiu que a detecção de ameaças em banco de dados é extremamente complexa.

Mesmo combinando várias ferramentas, não há garantia de segurança total da:

- confidencialidade;
- integridade;
- disponibilidade.

Ele destacou duas ameaças particularmente difíceis de detectar:

1. **SQL Injection**, porque muitas vezes entra pela aplicação;
2. **Falha bizantina**, porque o nó malicioso pode ser um nó legítimo comprometido.

No caso de falha bizantina, um nó legítimo pode ter sido infectado por vírus e passar a votar incorretamente em uma transação distribuída, por exemplo, sempre votando pelo abort.

A estratégia ideal de detecção deve combinar:

- monitoramento;
- análise de comportamento;
- identificação de padrões;
- alertas ao DBA;
- resposta proativa.

O professor reforçou que identificar depois do ataque não é suficiente. O ideal é agir antes da efetivação do dano.

---

## 50. Ataques a dados em repouso

O professor afirmou que muitos vazamentos de dados ocorrem sobre dados em repouso.

Dados em repouso são dados gravados em arquivos, discos, backups ou buckets de armazenamento, fora do fluxo normal de execução.

Quando o SGBD está ativo, seus arquivos normalmente estão em uso e protegidos pelo próprio sistema.

Quando o SGBD está desligado, os arquivos ficam mais vulneráveis a acesso direto pelo sistema operacional.

Na prática do dia seguinte, os alunos veriam um exemplo com PostgreSQL:

- o PostgreSQL desligado;
- acesso ao arquivo pelo sistema operacional;
- leitura de dados em arquivo binário;
- interpretação da estrutura interna da página do banco.

---

## 51. Estrutura de páginas no banco de dados

Para conseguir ler dados diretamente de um arquivo binário do banco, é necessário entender como o SGBD organiza suas páginas.

O professor explicou que uma página não contém apenas tuplas.

Ela também contém informações de controle, como:

- cabeçalho da página;
- localização das tuplas;
- deslocamento das tuplas dentro da página;
- espaço livre;
- informações de organização interna.

O atacante precisa conhecer essa estrutura para recuperar os dados corretamente.

---

## 52. Espaço livre na página e tipos `char` e `varchar`

O professor explicou que, em alguns SGBDs, é possível reservar espaço livre dentro da página.

Exemplo:

```text
Página de 8 KB
2 KB reservados como espaço livre
6 KB usados para dados
```

Esse espaço pode ser útil quando um atributo cresce.

O professor explicou a diferença entre `char` e `varchar`.

### 52.1 `char`

Quando um atributo é definido como `char(30)`, sempre são alocados 30 bytes para ele, mesmo que o valor armazenado tenha menos caracteres.

### 52.2 `varchar`

Quando um atributo é definido como `varchar(30)`, ele pode ocupar apenas o espaço necessário para o valor armazenado.

Se o valor tiver 1 byte, ocupa 1 byte.

Se depois for atualizado para 30 bytes, será necessário haver espaço na página para acomodar o crescimento.

Se não houver espaço, a tupla pode acabar espalhada em várias páginas, o que prejudica a performance.

Por isso, deixar espaço livre pode ser uma estratégia de organização física.

---

## 53. Vazamentos em nuvem e má configuração

O professor afirmou que provedores de nuvem frequentemente dizem que a maioria dos vazamentos ocorre por má configuração feita pelo usuário do serviço.

Ou seja, o problema não estaria necessariamente no provedor, mas em como o cliente configurou:

- permissões;
- buckets;
- APIs;
- firewall;
- credenciais;
- autenticação;
- exposição pública de dados.

O professor, porém, problematizou essa visão. Em alguns casos, a má configuração do cliente permitiu acesso a metadados ou recursos do próprio ambiente de nuvem, o que também indica fragilidade do ecossistema.

---

## 54. Caso Capital One — 2019

O professor citou o caso da **Capital One**, ocorrido em 2019.

Segundo a explicação, uma ex-funcionária da AWS explorou uma configuração incorreta no firewall de uma aplicação web.

A técnica citada foi **Server-Side Request Forgery — SSRF**.

Por meio dela, foi possível acessar o serviço de metadados da AWS.

Ao acessar metadados, o atacante consegue conhecer informações importantes sobre o ambiente, como:

- onde dados estão armazenados;
- quais recursos existem;
- credenciais temporárias;
- papéis e permissões;
- caminhos para acessar buckets.

No caso citado, isso permitiu acesso a buckets S3 e download de registros de aproximadamente **100 milhões de usuários**.

Os dados acessados incluíam:

- nomes;
- endereços;
- informações de crédito;
- Social Security Number, equivalente aproximado ao CPF nos Estados Unidos.

O professor ressaltou que o ataque começou com uma configuração equivocada do sistema de identificação e acesso da aplicação.

---

## 55. Metadados em nuvem

O professor fez uma analogia entre metadados em banco de dados e metadados em serviços de nuvem.

Em um banco de dados, metadados descrevem os dados:

- tabelas;
- colunas;
- tipos;
- restrições;
- permissões.

Em serviços de nuvem, metadados podem indicar onde recursos e arquivos estão armazenados dentro de uma infraestrutura distribuída.

Como os serviços de nuvem são distribuídos, acessar os metadados pode permitir descobrir em quais nós ou buckets determinados dados estão armazenados.

O professor destacou que, se um atacante acessa um nó sem metadados, talvez não saiba a que arquivo pertencem os dados. Com metadados, ele consegue localizar e acessar com muito mais precisão.

---

## 56. Caso Uber — 2016

O professor citou o ataque à Uber em 2016.

O ataque envolveu credenciais da AWS armazenadas em código no GitHub.

Segundo a explicação:

1. o atacante acessou ou invadiu um repositório no GitHub;
2. encontrou credenciais da AWS em hardcode;
3. usou essas credenciais para acessar o S3;
4. conseguiu acessar dados de aproximadamente **57 milhões de usuários**.

A lição principal foi:

```text
Qualquer mecanismo de segurança pode falhar se o atacante obtém credenciais válidas.
```

O professor reforçou que armazenar credenciais diretamente no código é uma prática extremamente perigosa.

---

## 57. GitHub como fonte de vulnerabilidades

O professor discutiu o risco de colocar código em repositórios, especialmente quando desenvolvedores mantêm a mentalidade de ambiente fechado.

Dentro de uma empresa, inserir credenciais no código já era uma má prática, mas o risco podia parecer menor porque o ambiente era interno.

No GitHub, mesmo em repositórios privados, a exposição aumenta.

O professor destacou que pessoas podem procurar por termos como:

```text
api_key
secret
password
access_key
```

E encontrar chaves ou credenciais publicadas indevidamente.

Ele também relacionou o GitHub ao treinamento de ferramentas de IA, destacando que o repositório de código público se tornou uma grande fonte de aprendizado para modelos capazes de programar.

---

## 58. Exposição por buckets públicos

O professor citou casos em que dados foram expostos porque buckets de armazenamento em nuvem estavam públicos.

A explicação foi que alguns desenvolvedores configuram arquivos no S3 imaginando que apenas seus próprios serviços irão acessá-los. Porém, se o bucket está público, um atacante pode acessá-lo diretamente.

Essa é uma má configuração clássica:

```text
Bucket público + dados sensíveis = vazamento
```

O professor destacou que a mentalidade de ambiente interno não pode ser simplesmente transferida para a nuvem.

Na nuvem, configurações permissivas tornam os dados muito mais expostos.

---

## 59. Caso Facebook Data Exposure — 2019

O professor citou o caso do Facebook, envolvendo aplicação terceirizada e buckets públicos na AWS.

A falha não estava diretamente no Facebook, mas em uma empresa terceirizada contratada para desenvolver ou operar algum serviço.

Essa empresa configurou incorretamente buckets onde os dados estavam armazenados, deixando-os com acesso público.

O professor usou esse caso para reforçar o risco de terceirização sem controle adequado de segurança.

Quando uma empresa compra um serviço, ela pode não saber:

- como o código foi implementado;
- quais permissões foram configuradas;
- como os dados foram armazenados;
- se houve testes de segurança;
- se boas práticas foram seguidas.

---

## 60. Caso Microsoft Power Apps — 2021

O professor citou um caso envolvendo o **Microsoft Power Apps**, em 2021.

Segundo a explicação, uma API estava com configuração equivocada de permissões.

Isso permitiu acesso sem autenticação a dados que deveriam estar protegidos.

O caso expôs cerca de **38 milhões de registros** de clientes ou usuários de serviços ligados à Microsoft.

O professor ressaltou que esse tipo de falha, embora pareça absurdo, é comum e pode ocorrer por:

- falta de atenção do desenvolvedor;
- ausência de controle de qualidade;
- ausência de testes adequados;
- falta de engenheiros de teste;
- pressão por redução de custo no desenvolvimento.

---

## 61. Caso Imperva — 2019

A transcrição cita o caso da Imperva em 2019 como outro exemplo de exposição envolvendo chaves de API e serviços da Amazon.

Segundo a explicação, foram acessadas chaves de API na Amazon, permitindo baixar backups de dados armazenados na AWS.

Esse caso foi classificado como ataque a dados em repouso, pois envolveu acesso a backups.

---

## 62. Caso CodeSpaces

O professor citou o caso da CodeSpaces como exemplo extremo.

O atacante obteve contas de acesso à AWS usadas pela empresa.

Com esse acesso, conseguiu deletar infraestrutura e backups.

O impacto foi tão grave que levou ao fechamento da empresa.

A lição destacada foi que proteger backups é tão importante quanto proteger a infraestrutura principal.

Se o atacante remove dados e também remove backups, a empresa pode não conseguir se recuperar.

---

## 63. Caso Pegasus Airlines — 2022

O professor citou o caso da Pegasus Airlines.

Segundo a explicação, a empresa utilizava serviço de nuvem pública, novamente com referência à AWS.

A má configuração permitiu acesso a dados sensíveis.

O professor ressaltou que os dados expostos eram particularmente críticos, pois envolviam:

- dados de voos;
- horários;
- rotas;
- possíveis procedimentos de segurança;
- sistema usado por pilotos;
- código-fonte de sistemas da Pegasus;
- dados pessoais de empregados;
- dados de passageiros.

O professor destacou que, se esse tipo de acesso fosse explorado por um grupo terrorista, poderia ser usado para planejar ataques, conhecer rotas e tentar burlar procedimentos de segurança.

---

## 64. Nuvem não torna automaticamente o sistema seguro

Na discussão com os alunos, o professor reforçou que subir um sistema para a nuvem não o torna automaticamente seguro.

Muitas empresas migram sistemas on-premises para a nuvem mantendo as mesmas práticas inseguras.

Exemplo:

```text
Permissões abertas dentro da empresa pareciam menos perigosas.
Na nuvem, a mesma permissão aberta fica exposta a muito mais atacantes.
```

O aluno comentou que a nuvem oferece elasticidade e facilidade de disponibilidade, mas a configuração e o funcionamento da aplicação continuam sendo responsabilidade da empresa.

O professor concordou e reforçou que vulnerabilidades que existiam internamente podem ficar ainda mais expostas na nuvem.

---

## 65. Decisão gerencial e desconhecimento técnico

O professor criticou o fato de muitas decisões de migração para nuvem serem tomadas por gestores que conhecem pouco de computação.

A decisão costuma ser baseada em custo:

- economizar equipamento;
- reduzir manutenção;
- diminuir equipe de TI;
- evitar compra de infraestrutura;
- terceirizar operação.

O problema é que a redução de custo pode vir acompanhada de aumento de risco.

O gestor pode acreditar que, por estar na nuvem, os dados estarão automaticamente seguros.

Mas, se o sistema legado possui vulnerabilidades, elas podem se tornar ainda mais exploráveis.

---

## 66. Responsabilidade compartilhada e discurso dos provedores

O professor afirmou que provedores de nuvem vendem segurança, mas geralmente garantem a segurança do ponto de vista deles.

Ou seja, protegem a infraestrutura da nuvem, mas não necessariamente a aplicação mal configurada pelo cliente.

A responsabilidade é compartilhada:

```text
Provedor → infraestrutura da nuvem
Cliente → configuração, aplicação, permissões, credenciais e dados
```

O professor, porém, questionou se os provedores não têm também responsabilidade quando a má configuração do cliente permite acesso a recursos sensíveis dentro da própria nuvem.

---

## 67. Privacidade, LGPD, Cloud Act e nuvem soberana

O professor discutiu também questões de privacidade e soberania de dados.

Ele citou a LGPD no Brasil e mencionou o **Cloud Act** dos Estados Unidos.

Segundo a explicação, empresas americanas que prestam serviço de nuvem podem ser obrigadas a fornecer dados ao governo americano quando solicitadas.

Isso levanta preocupações sobre serviços de nuvem usados para armazenar dados sensíveis de órgãos públicos ou empresas brasileiras.

O professor também criticou o marketing de “nuvem soberana”, argumentando que, se a empresa está sujeita a leis estrangeiras, a soberania prometida pode ser limitada.

---

## 68. Dados pessoais, vigilância e grandes empresas de tecnologia

O professor relacionou a discussão de nuvem com o filme e o caso Snowden.

Ele mencionou que empresas americanas de hardware, software e serviços digitais podem fornecer dados ao governo americano.

Também comentou exemplos cotidianos de percepção de vigilância, como falar sobre determinado produto e depois receber propagandas relacionadas.

A discussão foi usada para reforçar a ideia de que segurança em nuvem não envolve apenas disponibilidade técnica, mas também privacidade, controle e soberania dos dados.

---

## 69. Lock-in em nuvem

Um aluno mencionou o problema de ficar dependente de um provedor de nuvem.

Segundo o comentário, subir dados para a nuvem pode ser fácil e barato, mas retirar grandes volumes de dados pode ser caro.

Isso cria dependência do provedor.

O professor comparou essa estratégia com a dependência criada por grandes fornecedores de SGBD.

Empresas começam com condições comerciais favoráveis, mas depois ficam dependentes de atualizações, licenças e custos crescentes.

---

## 70. Provedores locais e confiança nas grandes marcas

A aula terminou com uma discussão sobre provedores locais de nuvem e data centers regionais.

Um aluno citou uma empresa local com data center em Fortaleza, com estrutura classificada como Tier 3, que tenta vender serviços de nuvem com maior proximidade e menos dificuldades de saída.

O professor e os alunos discutiram que muitas empresas preferem contratar grandes provedores globais, como AWS, Google ou Oracle, mesmo quando existem alternativas locais.

Também foi mencionado que algumas empresas vendem “serviço de nuvem”, mas usam infraestrutura ou gerenciamento de grandes provedores por trás.

---

## 71. Fechamento da disciplina

Ao final, o professor afirmou que, com essa aula, o programa teórico da disciplina estava encerrado.

A aula seguinte seria dedicada à prática.

A prática envolveria:

- SQL Injection;
- exploração de aplicação vulnerável;
- ataque a dados em repouso;
- leitura de arquivos binários do PostgreSQL;
- compreensão da estrutura interna de páginas do banco.

---

## 72. Resumo dos principais pontos da aula

A aula consolidou os seguintes pontos:

- segurança em banco de dados envolve confidencialidade, integridade e disponibilidade;
- replicação aumenta disponibilidade, mas traz alto custo de consistência;
- arquiteturas com Master reduzem complexidade, mas podem criar gargalos;
- propagação ávida garante maior consistência, mas aumenta o tempo de transação;
- propagação preguiçosa melhora desempenho, mas aceita inconsistência temporária;
- SQL Injection ocorre quando entrada do usuário é interpretada como código SQL;
- `OR 1=1`, `WAIT FOR DELAY`, erros e `UNION` são estratégias usadas para testar ou explorar vulnerabilidades;
- o SGBD tem dificuldade de impedir SQL Injection sozinho, porque ele recebe SQL válido vindo de uma aplicação legítima;
- a prevenção depende principalmente de código seguro, validação de entrada, parametrização e controle de permissões;
- views, GRANT, REVOKE, Roles e menor privilégio reduzem o impacto de uma exploração;
- detecção de ameaças em banco de dados é complexa e exige monitoramento, auditoria e análise de comportamento;
- ataques a dados em repouso são comuns, especialmente em arquivos, backups e buckets de nuvem;
- má configuração de serviços em nuvem é uma das principais causas de vazamento;
- casos como Capital One, Uber, Facebook, Microsoft Power Apps, Imperva, CodeSpaces e Pegasus mostram que credenciais, APIs, buckets públicos e backups expostos podem causar grandes incidentes;
- migrar para nuvem não elimina vulnerabilidades existentes e pode aumentar a exposição;
- segurança em banco de dados exige conhecimento combinado de SGBD, SQL, infraestrutura, aplicação e cibersegurança.
