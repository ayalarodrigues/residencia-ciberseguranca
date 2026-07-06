# Segurança em Aplicações Web — Aula 1

## Fundamentos, mecanismos de defesa e tecnologias web

## 1. Visão geral da disciplina

A disciplina tem como objetivo apresentar os fundamentos, tecnologias, vulnerabilidades e mecanismos de defesa relacionados à segurança em aplicações web, preparando o estudante para compreender os principais riscos associados ao desenvolvimento e à operação desses sistemas.

Ao final da disciplina, espera-se que o estudante seja capaz de:

- compreender a arquitetura e as tecnologias que sustentam aplicações web;
- identificar e analisar vulnerabilidades;
- compreender formas de exploração controlada dessas falhas;
- conhecer mecanismos de defesa;
- utilizar ferramentas de análise de segurança;
- relacionar vulnerabilidades práticas com referências consolidadas da área, especialmente a OWASP.

A disciplina dá ênfase ao **OWASP Top 10**, uma lista periódica das categorias de riscos mais relevantes para aplicações web.

Também são abordadas três classes importantes de ferramentas:

### SAST — Static Application Security Testing

Ferramentas de análise estática examinam o código-fonte da aplicação sem que seja necessário executá-la.

O objetivo é encontrar problemas diretamente na implementação, como:

- uso inseguro de funções;
- validação insuficiente;
- padrões de código vulneráveis;
- falhas que podem ser identificadas antes da aplicação chegar à produção.

### DAST — Dynamic Application Security Testing

Ferramentas de análise dinâmica avaliam a aplicação em execução.

Elas interagem com a aplicação por meio do envio e recebimento de mensagens para observar como o sistema se comporta diante de diferentes entradas.

### SCA — Software Composition Analysis

Ferramentas de SCA analisam bibliotecas, componentes e dependências utilizados pela aplicação.

O objetivo é verificar, por exemplo:

- componentes desatualizados;
- bibliotecas obsoletas;
- versões com vulnerabilidades conhecidas;
- riscos introduzidos por dependências de terceiros.

## 2. Principais referências

As principais referências da disciplina são:

- *The Web Application Hacker's Handbook*;
- materiais e projetos da OWASP.

A **OWASP — Open Worldwide Application Security Project** é uma fundação sem fins lucrativos voltada para o aumento da segurança de aplicações.

Entre seus objetivos estão:

- produzir materiais de referência;
- ajudar desenvolvedores;
- apoiar profissionais de teste;
- divulgar boas práticas;
- sistematizar riscos e metodologias de avaliação de segurança.

Entre os materiais citados na disciplina estão:

- **OWASP Top 10**;
- padrões e controles de segurança;
- **WSTG — Web Security Testing Guide**, que reúne casos e metodologias para teste de aplicações web.

---

# Parte I — Fundamentos de segurança em aplicações web

## 3. Estudo de caso: o incidente da WebCorp

Para introduzir os principais problemas de segurança, considera-se o caso fictício de uma empresa chamada **WebCorp**.

A empresa fornece uma aplicação web aos seus usuários e sofre uma violação crítica.

Durante o incidente:

- sessões de usuários autenticados foram sequestradas;
- dados confidenciais foram extraídos dos bancos de dados;
- o painel de administração foi comprometido.

A aplicação possuía:

- usuários comuns;
- usuários administrativos;
- páginas destinadas à manutenção da plataforma;
- integração com outros sistemas internos;
- bancos de dados;
- infraestrutura de rede protegida.

A infraestrutura possuía mecanismos tradicionais de segurança, como:

- firewall;
- IDS;
- SIEM;
- monitoramento de logs.

Mesmo assim, a empresa foi atacada.

O ponto de entrada foi a própria **aplicação web exposta à internet**.

A principal conclusão é:

> Uma infraestrutura de rede pode estar bem protegida e, ainda assim, a aplicação web pode funcionar como uma grande porta de entrada para atacantes.

A aplicação web não deve ser vista apenas como uma página isolada. Ela normalmente está conectada a um ecossistema que pode incluir:

- bancos de dados;
- APIs;
- serviços internos;
- sistemas corporativos;
- infraestrutura local;
- serviços em nuvem.

Por isso, comprometer a aplicação pode permitir que o atacante alcance componentes muito além da interface que aparece no navegador.

---

## 4. Evolução da web e aumento da superfície de ataque

### 4.1 Web 1.0

No início da World Wide Web, as páginas eram predominantemente estáticas.

Um site era, essencialmente, um repositório de informações hospedadas em um servidor.

O conteúdo era composto principalmente por:

- texto;
- imagens;
- documentos HTML.

A interação era predominantemente unidirecional:

1. o navegador solicitava uma página;
2. o servidor retornava o HTML e os demais recursos;
3. o usuário apenas consumia o conteúdo.

Para alterar o site, o responsável precisava modificar diretamente os arquivos hospedados no servidor.

Os principais ataques da época estavam relacionados a:

### Defacing

Desfiguração do site.

O atacante alterava o conteúdo da página para:

- exibir mensagens;
- substituir imagens;
- provocar o proprietário;
- demonstrar que havia conseguido invadir o sistema.

### Indisponibilidade

O atacante podia derrubar o site e impedir seu funcionamento.

### Uso indevido da infraestrutura

O servidor comprometido também podia ser usado para:

- executar outras atividades;
- servir como ponte para ataques;
- enviar mensagens;
- consumir recursos computacionais.

---

### 4.2 Web 2.0 e aplicações modernas

Com a evolução da web, as aplicações se tornaram muito mais dinâmicas.

Os usuários passaram a fornecer dados continuamente.

Exemplos:

- Wikipédia;
- blogs;
- redes sociais;
- comércio eletrônico;
- Internet Banking;
- serviços de busca;
- e-mail;
- chats com inteligência artificial.

O fluxo de informações tornou-se bidirecional:

- o cliente envia dados ao servidor;
- o servidor processa e retorna dados ao cliente.

Um usuário pode:

- criar conteúdo;
- preencher formulários;
- realizar buscas;
- adicionar produtos a um carrinho;
- efetuar transações;
- enviar mensagens;
- alterar configurações;
- fornecer dados pessoais.

O navegador passou, portanto, a funcionar como um **cliente universal** para os mais diversos tipos de serviço.

Essa mudança aumentou muito a importância da segurança.

Aplicações modernas armazenam e processam:

- dados pessoais;
- informações sensíveis;
- dados financeiros;
- credenciais;
- sessões autenticadas;
- informações corporativas.

Consequentemente, um ataque pode causar:

- roubo de dados;
- fraude financeira;
- tomada da infraestrutura;
- movimentação indevida de valores;
- comprometimento de sistemas integrados;
- escalada para ambientes internos.

---

## 5. Nuvem e ampliação da superfície de ataque

A adoção da computação em nuvem ampliou ainda mais a superfície de ataque.

Antes, muitas organizações mantinham seus sistemas apenas em infraestrutura local, ou **on-premises**.

Com a nuvem, a organização passa a depender também de:

- provedores externos;
- serviços gerenciados;
- APIs;
- credenciais;
- integrações;
- configurações de acesso.

O provedor pode oferecer diversos mecanismos de segurança, mas uma integração ou configuração incorreta pode expor os sistemas.

Assim, uma aplicação moderna pode envolver:

```text
Usuário
   ↓
Aplicação Web
   ↓
APIs / Serviços
   ↓
Bancos de Dados
   ↓
Infraestrutura Local e/ou Nuvem
```

A aplicação exposta à internet é apenas a parte visível desse ecossistema.

---

# 6. Princípio fundamental: toda entrada do usuário é não confiável

O principal princípio apresentado na aula é:

> **Toda entrada de usuário deve ser considerada não confiável.**

Na web moderna, o usuário fornece uma grande quantidade de dados para a aplicação.

O problema é que um atacante também é um usuário.

Ele pode:

- estar autenticado;
- não estar autenticado;
- usar um navegador comum;
- usar o modo desenvolvedor;
- alterar elementos da página;
- modificar requisições;
- criar scripts;
- utilizar ferramentas automatizadas;
- enviar mensagens diretamente ao servidor.

Tudo que está do lado do cliente pode ser visto ou manipulado.

Isso inclui:

- HTML;
- JavaScript;
- campos de formulário;
- campos ocultos;
- cookies;
- parâmetros;
- cabeçalhos HTTP;
- URLs;
- dados enviados ao servidor.

Portanto:

> Toda lógica de segurança realmente importante deve ser aplicada e validada no servidor.

Uma validação feita apenas no navegador não é suficiente, porque o atacante controla o cliente.

---

## 7. Exemplo: manipulação de campo oculto

Imagine uma aplicação de comércio eletrônico com um campo oculto contendo o preço de um produto.

O atacante pode:

1. abrir as ferramentas de desenvolvimento do navegador;
2. localizar o campo oculto;
3. alterar o valor;
4. enviar a requisição.

Se o servidor confiar cegamente no valor recebido, poderá processar o preço modificado.

O problema não está no fato de o campo ser visível ou oculto.

O problema é o servidor confiar em um dado controlável pelo cliente.

Esse princípio se aplica também a:

- cupons;
- descontos;
- identificadores;
- preços;
- permissões;
- valores de transação;
- campos que não aparecem visualmente na interface.

---

# 8. Vulnerabilidades frequentes em aplicações web

Entre os tipos de vulnerabilidade destacados estão:

- **Cross-Site Scripting — XSS**;
- **Broken Access Control**;
- **SQL Injection**.

Essas vulnerabilidades representam exemplos de falhas que podem ocorrer quando:

- entradas não são validadas;
- controles de autorização falham;
- consultas ao banco são construídas de forma insegura;
- dados manipuláveis pelo cliente são tratados como confiáveis.

O OWASP Top 10 sistematiza categorias de riscos amplamente observadas no mundo real.

---

# 9. HTTP, HTTPS e a pilha de protocolos

## 9.1 Pilha tradicional

A comunicação web pode ser entendida por meio da pilha de protocolos.

De forma simplificada:

```text
Aplicação      → HTTP
Transporte     → TCP
Rede           → IP
Enlace/Física  → Ethernet, Wi-Fi etc.
```

Cliente e servidor possuem suas próprias pilhas.

Na camada de aplicação, a comunicação web utiliza HTTP.

Na camada de transporte, normalmente utiliza TCP.

O TCP é usado porque a aplicação web necessita de entrega confiável dos dados.

O objetivo é evitar que:

- partes de uma página sejam perdidas;
- imagens sejam corrompidas;
- dados cheguem incompletos;
- mensagens sejam entregues fora do controle esperado.

Na camada de rede, utiliza-se IP.

Na camada de enlace, a tecnologia pode variar.

Exemplos:

- cliente em Wi-Fi;
- servidor em Ethernet.

---

## 9.2 HTTP versus HTTPS

O HTTPS acrescenta uma camada de proteção à comunicação.

De forma conceitual:

```text
HTTP
↓
TLS
↓
TCP
↓
IP
```

Historicamente, também se utilizava o termo SSL.

### SSL

*Secure Sockets Layer*

### TLS

*Transport Layer Security*

O TLS é o protocolo moderno utilizado para proteger a comunicação.

Ele fornece mecanismos para estabelecer uma comunicação criptografada entre cliente e servidor.

---

## 9.3 Confidencialidade

A confidencialidade significa que o conteúdo das mensagens não deve ficar legível para terceiros não autorizados.

Sem a chave correta, um atacante que intercepte o tráfego não deveria conseguir visualizar facilmente os dados em texto claro.

---

## 9.4 Tríade CIA e propriedades ampliadas

As três propriedades clássicas de segurança são:

### Confidencialidade

Evitar acesso indevido à informação.

### Integridade

Garantir que os dados não sejam alterados indevidamente.

### Disponibilidade

Garantir que sistemas e informações permaneçam acessíveis quando necessários.

A aula também menciona duas propriedades adicionais:

### Autenticidade

Relacionada à verificação da identidade das entidades.

### Não repúdio ou irretratabilidade

Relacionada à capacidade de impedir que alguém negue posteriormente uma ação que realizou.

---

## 9.5 Criptografia simétrica e assimétrica

A comunicação segura utiliza conceitos de criptografia simétrica e assimétrica.

### Criptografia assimétrica

Utiliza um par de chaves:

- chave pública;
- chave privada.

Um algoritmo tradicionalmente associado a essa categoria é o RSA.

### Criptografia simétrica

Utiliza uma chave compartilhada para cifrar e decifrar dados.

Um exemplo é o AES.

A criptografia simétrica é mais eficiente para grandes volumes de dados.

Por isso, os mecanismos de comunicação segura utilizam um processo de estabelecimento de sessão e, em seguida, chaves simétricas eficientes para proteger o tráfego da sessão.

---

# 10. O mito: “se usa HTTPS, então a aplicação é segura”

O uso de HTTPS não torna uma aplicação automaticamente segura.

O TLS protege principalmente a comunicação em trânsito.

Ele pode impedir que terceiros visualizem ou alterem facilmente os dados durante o percurso.

No entanto, não impede que o próprio atacante envie uma entrada maliciosa para o servidor.

Exemplo:

```text
Atacante
   ↓
Entrada maliciosa
   ↓
Canal HTTPS válido e criptografado
   ↓
Servidor vulnerável
```

O ataque pode viajar normalmente por dentro do túnel criptografado.

Portanto:

> HTTPS protege o canal, mas não corrige vulnerabilidades na lógica da aplicação.

Um atacante pode estabelecer uma conexão TLS válida com o servidor e, em seguida:

- enviar SQL Injection;
- explorar XSS;
- alterar parâmetros;
- atacar a lógica de negócio;
- tentar sequestrar sessões.

A criptografia não distingue automaticamente uma entrada legítima de uma entrada maliciosa.

---

# 11. Defesa em profundidade

Como nenhuma medida isolada resolve todos os problemas, é necessário adotar **defesa em profundidade**.

Defesa em profundidade significa aplicar segurança em várias camadas.

Os quatro grandes pilares apresentados são:

1. proteger o acesso;
2. proteger e validar entradas;
3. lidar com atacantes;
4. administrar a aplicação de forma segura.

---

# Parte II — Mecanismos de defesa

## 12. Pilar 1: proteger o acesso

A proteção do acesso envolve três elementos principais:

- autenticação;
- autorização ou controle de acesso;
- gerenciamento de sessão.

---

## 12.1 Autenticação

A autenticação busca provar a identidade do usuário.

Ela pode envolver:

- login;
- senha;
- biometria;
- outros fatores de autenticação.

A pergunta respondida pela autenticação é:

> **Quem é você?**

---

## 12.2 Autorização e controle de acesso

Depois de autenticado, o usuário precisa ter suas permissões verificadas.

A pergunta respondida pela autorização é:

> **O que você pode fazer?**

Exemplo:

- um usuário comum pode consultar seus próprios dados;
- um administrador pode possuir permissões adicionais.

Os dois podem estar autenticados, mas não devem possuir o mesmo nível de acesso.

Uma falha de autorização pode permitir:

- escalada de privilégio;
- acesso a dados de outros usuários;
- execução de ações administrativas;
- acesso a funcionalidades restritas.

Um atacante pode:

1. autenticar-se legitimamente como usuário comum;
2. explorar uma falha de controle de acesso;
3. obter privilégios administrativos.

---

## 12.3 Gerenciamento de sessão

Depois do login, o usuário continua interagindo com a aplicação.

A aplicação precisa identificar que novas requisições pertencem ao mesmo usuário.

Para isso, são utilizados mecanismos como:

- cookies;
- tokens.

O gerenciamento de sessão evita que o usuário precise digitar login e senha em todas as páginas.

A pergunta central é:

> Como o servidor associa cada nova requisição ao usuário que já se autenticou?

---

## 13. HTTP é stateless

O HTTP é um protocolo **stateless**, ou seja, sem estado.

Cada requisição é, por padrão, independente.

O servidor não mantém automaticamente a memória das requisições anteriores.

Isso melhora a escalabilidade.

Se o servidor tivesse que manter estruturas permanentes na memória para rastrear individualmente milhões de usuários, o custo seria muito alto.

O lado negativo é que cada nova requisição precisa de algum mecanismo adicional para ser associada ao usuário correto.

Cookies e tokens resolvem esse problema.

---

## 14. Sequestro de sessão

O gerenciamento de sessão também introduz riscos.

Se um atacante obtiver ou prever o identificador de sessão de outro usuário, poderá tentar agir em nome dele.

Esse ataque é conhecido como:

**Session Hijacking — sequestro de sessão**

---

## 14.1 Exemplo: autenticação forte e sessão fraca

Considere uma aplicação com:

- senhas muito fortes;
- autenticação robusta;
- tokens previsíveis.

Exemplo:

```text
1234
1235
1236
1237
```

O atacante autentica-se com sua própria conta e recebe o token `1234`.

Ele percebe o padrão e tenta alterar manualmente o token para `1235`.

Se o token `1235` pertencer a outro usuário com uma sessão ativa e o servidor aceitar a troca, o atacante poderá sequestrar a sessão.

A partir desse momento, o servidor poderá interpretar as ações do atacante como se fossem do outro usuário.

A conclusão é:

> Uma autenticação forte não compensa um gerenciamento de sessão fraco.

Em segurança, o elo mais fraco pode comprometer todo o sistema.

---

# 15. Pilar 2: proteger as entradas

As principais estratégias discutidas para proteção de entradas são:

- blacklist;
- whitelist;
- sanitização;
- manuseio seguro de dados.

---

## 15.1 Blacklist

A blacklist, ou lista negra, define entradas conhecidas como perigosas.

A aplicação tenta bloquear elementos como:

- comandos;
- palavras reservadas;
- caracteres;
- padrões associados a ataques.

Exemplo:

```html
<script>
```

A aplicação pode tentar remover ou rejeitar essa entrada.

O problema é que atacantes podem criar variações.

Exemplos de técnicas de evasão:

- alterações de maiúsculas e minúsculas;
- uso de codificação;
- mudanças na sintaxe;
- concatenação;
- transformação da carga útil.

Por isso, a blacklist tende a ser menos eficaz quando utilizada isoladamente.

---

## 15.2 Whitelist

A whitelist, ou lista branca, adota a estratégia oposta.

Em vez de tentar listar tudo que é perigoso, define-se explicitamente o que é permitido.

Exemplo: data de nascimento.

Pode-se definir:

```text
DD/MM/AAAA
```

E verificar:

- quantidade de caracteres;
- presença apenas de números;
- posição das barras;
- intervalo possível para dia;
- intervalo possível para mês;
- formato do ano.

Esse modelo segue a ideia de negar por padrão e permitir apenas o que é conhecido como válido.

---

## 15.3 Por que nem sempre é possível usar whitelist?

Alguns campos aceitam um conjunto muito amplo de caracteres.

Exemplos:

- nome;
- endereço;
- texto livre.

Nomes legítimos podem conter:

- hífen;
- apóstrofo;
- caracteres especiais.

Uma política excessivamente restritiva pode impedir entradas válidas.

Portanto, whitelist é muito eficaz quando o formato é previsível, mas não pode ser aplicada da mesma maneira em todos os campos.

---

## 15.4 Sanitização

Sanitizar significa limpar, transformar ou codificar dados para reduzir o risco de interpretação maliciosa.

A sanitização pode envolver:

- remoção de caracteres;
- escape;
- codificação;
- normalização.

É necessário considerar o contexto em que o dado será utilizado.

Um dado pode ser interpretado por:

- HTML;
- JavaScript;
- SQL;
- XML;
- URL;
- outros interpretadores.

---

## 15.5 Manuseio seguro dos dados

Além de limpar entradas, é importante utilizar mecanismos seguros por design.

Um exemplo central é o uso de **consultas parametrizadas**.

Em vez de concatenar diretamente a entrada do usuário em um comando SQL, a aplicação utiliza parâmetros tratados separadamente.

Esse modelo ajuda a evitar SQL Injection.

---

# 16. Cross-Site Scripting — XSS

O XSS pode ocorrer quando dados controlados pelo atacante são armazenados ou refletidos e posteriormente interpretados pelo navegador como código.

Exemplo conceitual:

1. o atacante preenche um campo;
2. em vez de um nome legítimo, envia um trecho contendo código;
3. o servidor armazena a entrada sem tratamento adequado;
4. outro usuário acessa a página;
5. o conteúdo malicioso é retornado;
6. o navegador interpreta e executa o código.

Um exemplo de elemento associado a esse tipo de ataque é:

```html
<script>
```

O risco ocorre porque o navegador interpreta determinados conteúdos como instruções executáveis.

---

# 17. Fronteiras de confiança

Uma aplicação pode possuir várias fronteiras de confiança.

Exemplo:

```text
Usuário
   ↓
Aplicação Web
   ↓
Banco de Dados
   ↓
Outros Serviços
```

A fronteira entre o usuário e a aplicação é especialmente crítica.

No entanto, as fronteiras internas também não devem ser tratadas como totalmente confiáveis.

Uma entrada maliciosa pode atravessar várias camadas.

Exemplo:

1. o usuário envia um payload;
2. a aplicação não valida;
3. o payload chega ao banco de dados;
4. o ataque é processado no destino.

A existência de várias fronteiras reforça a importância da defesa em profundidade.

---

## 17.1 Validação na entrada e na saída

A aplicação deve tratar:

- dados que entram;
- dados que saem.

Fluxo conceitual:

```text
Usuário
   ↓
Validação da entrada
   ↓
Aplicação
   ↓
Consulta segura ao banco
   ↓
Processamento
   ↓
Tratamento da saída
   ↓
Navegador
```

A saída também precisa ser tratada porque será processada pelo navegador.

Se um conteúdo contaminado for retornado em HTML, JavaScript ou outro formato interpretável, o ataque pode se concretizar no cliente.

---

# 18. Outros serviços e XML

Uma aplicação pode se comunicar com serviços internos.

A aula cita o protocolo SOAP, que utiliza XML.

Como XML também possui uma sintaxe própria, entradas não tratadas podem explorar o modo como esse conteúdo é interpretado.

Assim, é necessário:

- validar;
- codificar;
- tratar caracteres especiais;
- impedir propagação de entradas maliciosas entre sistemas.

---

# 19. Falhas em filtros ingênuos

Filtros simples podem ser burlados.

Imagine um filtro que remove uma palavra perigosa apenas uma vez.

O atacante pode construir uma entrada de forma que, depois da remoção de uma parte, o restante volte a formar a expressão perigosa.

Esse problema ocorre quando a limpeza não considera:

- concatenação resultante;
- múltiplas etapas;
- recursividade;
- codificações;
- transformações posteriores.

A carga maliciosa enviada pelo atacante é chamada de **payload**.

Uma conclusão importante é:

> Se uma aplicação não consegue validar uma entrada de forma segura, rejeitar completamente a solicitação pode ser melhor do que tentar “consertá-la” de maneira ingênua.

---

# 20. Encoding e evasão de filtros

Um atacante pode codificar caracteres para tentar passar por filtros.

Entre os tipos de encoding mencionados estão:

- URL encoding;
- HTML encoding;
- Base64;
- hexadecimal.

---

## 20.1 URL encoding

No URL encoding, caracteres especiais podem ser representados por `%` seguido de um valor.

Exemplos comuns:

```text
%20 → espaço
%27 → apóstrofo
%22 → aspas
```

Um filtro que procura apenas os caracteres originais pode não reconhecer a versão codificada.

Por isso, a aplicação deve considerar:

1. normalização;
2. decoding apropriado;
3. validação;
4. sanitização.

A ordem dessas operações importa.

Um filtro pode se tornar vulnerável quando:

1. tenta remover padrões perigosos;
2. depois decodifica a entrada;
3. a decodificação recria o payload malicioso.

---

## 20.2 Encode e decode

### Encoding

Transforma o dado original em outra representação.

### Decoding

Faz o processo inverso.

Em segurança, é necessário considerar que o atacante pode utilizar múltiplas representações para esconder uma carga maliciosa.

---

# 21. Pilar 3: lidar com atacantes

Além de impedir a entrada, a aplicação precisa:

- detectar ataques;
- registrar eventos;
- responder de forma segura;
- evitar vazamento de informações.

---

## 21.1 Erros e vazamento de informações

Uma aplicação vulnerável pode retornar mensagens de erro com detalhes excessivos.

Exemplos de informações perigosas:

- estrutura interna;
- detalhes de SQL;
- caminhos de arquivos;
- versões;
- nomes de componentes;
- rastreamento de exceções.

Essas informações podem ajudar o atacante a mapear a aplicação.

---

## 21.2 Falha graciosa

Uma estratégia recomendada é a chamada **falha graciosa**.

Para o usuário externo, a aplicação retorna uma mensagem genérica.

Exemplo:

```text
Não foi possível concluir a operação.
```

Internamente, a aplicação registra os detalhes.

Exemplo:

```text
Data e hora
Usuário
Endereço de origem
Tipo de erro
Componente afetado
Indício de SQL Injection
```

Assim:

- externamente, pouca informação é exposta;
- internamente, há dados suficientes para análise.

---

## 21.3 Enumeração de usuários

Até mensagens aparentemente simples podem ajudar um atacante.

Compare:

```text
Usuário não existe.
```

com:

```text
Falha no login.
```

A primeira resposta permite descobrir se determinado usuário está cadastrado.

Um atacante pode automatizar tentativas com vários nomes e realizar enumeração de contas.

Por isso, respostas de autenticação devem evitar detalhes desnecessários.

---

# 22. Logs e investigação

Os logs são importantes para:

- investigação forense;
- monitoramento;
- detecção de comportamento suspeito;
- reconstrução de eventos;
- análise de incidentes.

A aplicação deve registrar internamente informações suficientes para que:

- administradores;
- equipes de segurança;
- testadores;

consigam compreender o que aconteceu.

---

# 23. Detecção de anomalias

A aplicação também pode detectar desvios em relação ao comportamento normal.

Exemplo:

```text
Requisições por segundo
        ↑
        │        ████
        │        ████
        │  ▂▂▂▂  ████
        └──────────────→ Tempo
```

Um aumento repentino pode indicar:

- força bruta;
- automação;
- abuso;
- tentativa de exploração.

A aplicação pode responder com:

- rate limiting;
- redução de velocidade;
- bloqueio;
- alertas;
- investigação.

---

## 23.1 Anomalias de negócio

Nem toda anomalia é puramente técnica.

Uma aplicação financeira pode detectar:

- compra fora do padrão;
- valor muito elevado;
- cidade incomum;
- transação incompatível com o histórico.

A resposta pode envolver:

- bloqueio temporário;
- confirmação com o usuário;
- geração de alerta.

---

# 24. Pilar 4: gestão segura da aplicação

A própria administração da aplicação é um ponto crítico.

Aplicações geralmente possuem interfaces destinadas a:

- administradores;
- operadores;
- funcionários internos.

Essas interfaces podem permitir:

- manutenção;
- consulta de dados;
- alteração de configurações;
- administração de usuários;
- operações privilegiadas.

---

## 24.1 Por que interfaces administrativas são perigosas?

Se comprometidas, elas podem fornecer ao atacante:

- privilégios elevados;
- acesso a dados;
- controle da aplicação;
- capacidade de realizar operações administrativas.

A URL do painel não deve ser considerada um mecanismo de segurança.

Mesmo uma página “secreta” pode ser:

- descoberta;
- enumerada;
- adivinhada;
- vazada.

---

## 24.2 XSS contra um administrador

Considere um sistema bancário.

Um administrador ou gerente possui uma página interna para consultar dados de clientes.

Fluxo do ataque:

1. um usuário malicioso envia um dado contendo código;
2. a aplicação não trata corretamente a entrada;
3. o dado é armazenado;
4. o administrador abre o painel;
5. o painel recupera o dado;
6. o navegador do administrador interpreta o conteúdo;
7. a sessão privilegiada pode ser comprometida.

Esse exemplo mostra que:

> Dados fornecidos por usuários comuns continuam sendo não confiáveis quando aparecem em interfaces administrativas.

O risco é ainda maior porque o navegador da vítima pode estar associado a uma conta de alto privilégio.

---

## 24.3 Interfaces administrativas podem ser menos testadas

Um motivo discutido é que organizações podem evitar fornecer acesso administrativo completo durante testes de intrusão.

Isso pode ocorrer por causa do alto nível de privilégio envolvido.

Como consequência, certas áreas críticas podem ser menos testadas do que a parte pública da aplicação.

---

# Parte III — Tecnologias que sustentam aplicações web

## 25. Visão geral de cliente e servidor

Uma aplicação web envolve dois grandes lados:

### Cliente

Normalmente o navegador do usuário.

### Servidor

Onde a aplicação web executa sua lógica principal.

O cliente e o servidor trocam mensagens HTTP ou HTTPS.

---

# 26. Tecnologias do lado do servidor

Uma pilha tradicional é a **LAMP**:

```text
L → Linux
A → Apache
M → MySQL
P → PHP
```

### Linux

Sistema operacional.

### Apache

Servidor web.

### MySQL

Banco de dados.

### PHP

Linguagem usada para implementar a aplicação.

Essa é apenas uma das possíveis combinações.

Aplicações modernas também podem utilizar:

- Java;
- Node.js;
- frameworks;
- outras linguagens;
- outros bancos de dados.

---

## 26.1 Frameworks e segurança

Frameworks podem ajudar a impor:

- padrões;
- organização;
- boas práticas;
- estruturas de desenvolvimento.

No entanto, um framework não corrige automaticamente uma lógica de negócio insegura.

Uma aplicação mal projetada ainda pode possuir:

- falhas estruturais;
- problemas de autorização;
- validações incorretas;
- regras de negócio vulneráveis.

A segurança não depende apenas da linguagem utilizada.

---

# 27. Tecnologias do lado do cliente

Entre as tecnologias mencionadas estão:

- HTML;
- JavaScript;
- Ajax;
- React.

Tudo que é enviado ao navegador deve ser considerado visível e potencialmente manipulável pelo usuário.

O atacante pode examinar:

- código;
- requisições;
- parâmetros;
- lógica implementada no cliente.

Por isso, a segurança não pode depender exclusivamente de controles no front-end.

---

# 28. Same-Origin Policy — SOP

A **Same-Origin Policy** é uma política de segurança do navegador.

Seu objetivo é impedir que um site de uma origem acesse livremente dados de outra origem.

Exemplo conceitual:

```text
Site malicioso
    ✕
Dados de outro site aberto no navegador
```

Sem esse tipo de isolamento, um site malicioso poderia tentar interagir diretamente com dados de outros serviços nos quais o usuário estivesse autenticado.

---

# Parte IV — HTTP em detalhes

## 29. HTTP Request e HTTP Response

A comunicação HTTP possui dois tipos centrais de mensagem:

### HTTP Request

Enviada pelo cliente ao servidor.

### HTTP Response

Enviada pelo servidor ao cliente.

---

# 30. Métodos HTTP

Entre os métodos mencionados estão:

- `GET`;
- `POST`;
- `PUT`;
- `DELETE`;
- `PATCH`;
- `HEAD`;
- `OPTIONS`.

---

## 30.1 GET

Usado principalmente para recuperar recursos.

Exemplo:

```http
GET /pagina HTTP/1.1
```

Parâmetros podem aparecer na URL.

Exemplo:

```text
?id=123
```

Como a URL pode ser registrada em:

- histórico;
- logs;
- ferramentas intermediárias;

não é apropriado enviar credenciais pela URL.

---

## 30.2 POST

Usado quando o cliente envia dados ao servidor.

Exemplos:

- formulários;
- login;
- envio de dados.

Os dados são colocados no corpo da requisição.

Para autenticação, deve-se utilizar HTTPS, pois o simples uso de `POST` não fornece criptografia.

---

## 30.3 PUT

Pode ser usado para enviar ou substituir um recurso no servidor.

É uma operação que exige cuidado, especialmente quando permite modificação de conteúdo.

---

## 30.4 DELETE

Usado para excluir recursos.

---

## 30.5 PATCH

Usado para atualizar apenas parte de um recurso.

---

## 30.6 HEAD

Semelhante ao `GET`, mas sem retorno do corpo completo do recurso.

Pode ser útil para:

- verificações;
- diagnóstico;
- análise de cabeçalhos.

---

## 30.7 OPTIONS

Pode ser utilizado para consultar capacidades ou métodos suportados.

---

# 31. Estrutura de uma requisição HTTP

Uma requisição possui, de forma simplificada:

1. linha inicial;
2. cabeçalhos;
3. linha em branco;
4. corpo, quando aplicável.

Exemplo conceitual:

```http
GET /pagina?userID=123 HTTP/1.1
Host: exemplo.com
Referer: https://exemplo.com/anterior
User-Agent: navegador
Cookie: session=abc123
Connection: keep-alive
```

---

## 31.1 Linha inicial

Pode conter:

- método;
- caminho ou URL;
- versão do HTTP.

---

## 31.2 Host

Identifica o destino.

```http
Host: exemplo.com
```

---

## 31.3 Referer

Indica de qual página a navegação partiu.

---

## 31.4 User-Agent

Identifica informações sobre o cliente.

Exemplos:

- navegador;
- versão;
- sistema.

---

## 31.5 Cookie

Carrega identificadores previamente definidos pelo servidor.

---

## 31.6 Connection: keep-alive

Solicita a manutenção da conexão para reutilização em novas requisições.

---

## 31.7 Corpo da mensagem

O corpo pode conter dados enviados pelo cliente.

É comum em requisições `POST`.

Exemplo:

```text
usuario=alice&senha=123
```

---

# 32. Tudo na requisição pode ser manipulado

Uma regra importante é:

> O atacante pode manipular praticamente qualquer parte da requisição que parte do cliente.

Isso inclui:

- método;
- parâmetros;
- URL;
- cookie;
- cabeçalhos;
- corpo;
- identificadores.

O servidor deve validar o que recebe.

---

# 33. GET versus POST

## GET

- utilizado principalmente para recuperar recursos;
- parâmetros aparecem normalmente na URL;
- dados podem ficar registrados em histórico e logs.

## POST

- utilizado para enviar dados;
- dados ficam no corpo da requisição;
- não aparecem diretamente na URL.

Entretanto:

> `POST` não é sinônimo de criptografia.

Para proteger os dados durante o transporte, é necessário HTTPS.

---

# 34. Estrutura de uma resposta HTTP

A resposta também possui:

- linha inicial;
- cabeçalhos;
- corpo.

Exemplo:

```http
HTTP/1.1 200 OK
Server: servidor-web
Content-Type: text/html

<html>
...
</html>
```

---

# 35. Códigos de status HTTP

Os códigos seguem famílias.

## 1xx — Informativos

Mensagens de informação.

## 2xx — Sucesso

Exemplo:

```text
200 OK
```

## 3xx — Redirecionamento

Exemplo:

```text
301 Moved Permanently
```

## 4xx — Erro relacionado à requisição do cliente

Exemplos:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
```

### 401

Relaciona-se à ausência ou falha de autenticação válida.

### 403

O servidor entendeu a requisição, mas não permite a operação.

## 5xx — Erro do servidor

Exemplo:

```text
500 Internal Server Error
```

Erros 5xx devem ser tratados com cuidado para evitar vazamento de informações internas.

---

# Parte V — Cookies e sessões

## 36. Como cookies permitem manter o estado

O funcionamento pode ser entendido como uma identificação entregue pelo servidor.

Fluxo:

```text
1. Usuário envia login e senha
2. Servidor valida
3. Servidor cria identificador de sessão
4. Servidor envia Set-Cookie
5. Navegador armazena o cookie
6. Requisições seguintes enviam Cookie
7. Servidor reconhece a sessão
```

---

## 36.1 Set-Cookie

O servidor pode enviar:

```http
Set-Cookie: session=abc123
```

---

## 36.2 Cookie

Nas requisições seguintes, o navegador envia:

```http
Cookie: session=abc123
```

O navegador normalmente realiza isso automaticamente.

---

# 37. Configurações importantes de cookies

## expires

Define a expiração.

## domain

Define em quais domínios o cookie é válido.

## path

Define em quais caminhos o cookie pode ser enviado.

## Secure

Indica que o cookie só deve ser enviado por uma conexão HTTPS.

## HttpOnly

Impede que scripts no lado do cliente acessem diretamente o cookie.

Isso ajuda a reduzir o risco de roubo do cookie por meio de XSS.

---

# 38. Back-end e front-end

## Back-end

O back-end é responsável por elementos como:

- lógica de negócio;
- armazenamento de dados;
- validações;
- autenticação;
- autorização;
- controles de segurança.

## Front-end

O front-end inclui o que chega ao navegador:

- HTML;
- JavaScript;
- imagens;
- componentes visuais.

O cliente está fora do controle da aplicação.

Por isso, qualquer validação crítica deve existir também no servidor.

---

# Parte VI — Análise de uma requisição suspeita

## 39. Exemplo conceitual de login interceptado

Considere uma requisição `POST` de login contendo:

- host;
- cookie;
- tamanho da mensagem;
- nome de usuário;
- senha.

A análise pode revelar diferentes problemas.

---

## 39.1 Senha fraca

Exemplo:

```text
123
```

É uma senha fácil de:

- adivinhar;
- testar;
- encontrar em listas comuns.

---

## 39.2 Cookie aparentemente codificado em Base64

Uma sequência terminando em:

```text
==
```

pode indicar Base64.

Isso não significa automaticamente que o cookie seja inseguro, mas pode indicar que vale a pena analisar sua estrutura.

Se a codificação apenas esconder informações previsíveis, o atacante pode tentar:

- decodificar;
- compreender a lógica;
- modificar valores;
- procurar padrões.

Codificação não é o mesmo que criptografia.

---

## 39.3 `%27` no nome do usuário

A sequência:

```text
%27
```

representa um apóstrofo em URL encoding.

Isso pode ser um indício de tentativa de manipulação de uma consulta SQL.

A análise de entradas codificadas exige:

1. decoding;
2. normalização;
3. validação;
4. interpretação do contexto.

---

# Parte VII — Síntese dos principais princípios

## 40. Regra central

> **Nunca confiar cegamente em dados vindos do cliente.**

---

## 41. Segurança deve estar no servidor

O atacante controla:

- navegador;
- HTML;
- JavaScript;
- requisições;
- parâmetros;
- cookies;
- ferramentas utilizadas para se comunicar com o servidor.

Portanto, a aplicação precisa validar tudo no lado do servidor.

---

## 42. HTTPS não elimina ataques contra a aplicação

HTTPS protege a comunicação, mas não impede:

- SQL Injection;
- XSS;
- manipulação de parâmetros;
- falhas de autorização;
- ataques de lógica de negócio.

---

## 43. Segurança depende do elo mais fraco

Uma aplicação pode ter:

- autenticação forte;
- criptografia;
- firewall;
- monitoramento.

Ainda assim, pode ser comprometida por:

- sessão previsível;
- autorização incorreta;
- entrada não validada;
- painel administrativo vulnerável.

---

## 44. Defesa em profundidade

É necessário combinar:

```text
Autenticação
+
Autorização
+
Gerenciamento de sessão
+
Validação de entrada
+
Tratamento de saída
+
Logs
+
Monitoramento
+
Gestão segura
```

---

# Parte VIII — Questões de revisão

## 45. Qual é a principal diferença entre sites antigos e aplicações web modernas?

As aplicações modernas possuem fluxo bidirecional constante de informações entre cliente e servidor.

Usuários não apenas recebem conteúdo: também enviam dados e executam operações.

---

## 46. Qual é o problema central da segurança em aplicações web?

O fato de o cliente estar fora do controle da aplicação e poder enviar entradas arbitrárias.

Todo dado recebido deve ser tratado como potencialmente malicioso.

---

## 47. Por que é enganoso dizer que um site é totalmente seguro apenas porque usa HTTPS?

Porque HTTPS protege o tráfego em trânsito, mas não impede ataques contra a lógica da aplicação.

Uma entrada maliciosa também pode atravessar um canal criptografado.

---

## 48. Qual é a principal função do gerenciamento de sessão?

Associar novas requisições ao usuário que já se autenticou.

Isso normalmente é feito com:

- cookies;
- tokens de sessão.

---

## 49. Por que blacklist é menos eficaz?

Porque atacantes podem contornar filtros utilizando:

- encoding;
- variações de sintaxe;
- mudanças triviais;
- transformações do payload.

---

## 50. Por que nem todos os campos podem usar whitelist rígida?

Porque alguns campos legítimos aceitam ampla variedade de caracteres.

Exemplos:

- nomes;
- endereços;
- textos livres.

---

## 51. Qual é o principal risco das interfaces administrativas?

Elas concentram funções privilegiadas.

Se comprometidas, podem permitir:

- acesso elevado;
- alteração de dados;
- controle da aplicação.

---

## 52. Por que POST é preferível ao GET para envio de dados sensíveis ou ações que alteram estado?

Porque os dados do `POST` ficam no corpo da requisição e não diretamente na URL.

Entretanto, informações sensíveis ainda precisam ser protegidas com HTTPS.

---

## 53. Qual é a função de `HttpOnly`?

Impedir que scripts do lado do cliente acessem diretamente o cookie.

Isso ajuda a mitigar o roubo de sessão via XSS.

---

## 54. O que significa HTTP 403?

O servidor entendeu a requisição, mas não permite a operação por falta de autorização.

---

## 55. Como a Same-Origin Policy protege o usuário?

Ela impede que um site acesse livremente dados pertencentes a outra origem.

---

## 56. Qual é a finalidade de `Set-Cookie`?

Permitir que o servidor envie ao navegador um cookie que será usado em requisições posteriores, possibilitando mecanismos como manutenção de sessão.

---

# 57. Mapa mental da aula

```text
Segurança em Aplicações Web
│
├── Evolução da Web
│   ├── Web 1.0
│   └── Web 2.0
│
├── Princípio Central
│   └── Entrada do usuário não é confiável
│
├── Comunicação
│   ├── HTTP
│   ├── TCP/IP
│   └── HTTPS/TLS
│
├── Defesa em Profundidade
│   ├── Acesso
│   │   ├── Autenticação
│   │   ├── Autorização
│   │   └── Sessão
│   │
│   ├── Entrada
│   │   ├── Blacklist
│   │   ├── Whitelist
│   │   ├── Sanitização
│   │   └── Métodos seguros
│   │
│   ├── Atacantes
│   │   ├── Logs
│   │   ├── Falha graciosa
│   │   ├── Monitoramento
│   │   └── Rate limiting
│   │
│   └── Gestão
│       └── Interfaces administrativas
│
├── Vulnerabilidades
│   ├── XSS
│   ├── SQL Injection
│   ├── Broken Access Control
│   └── Session Hijacking
│
└── Tecnologias Web
    ├── HTTP Requests
    ├── HTTP Responses
    ├── Cookies
    ├── Back-end
    ├── Front-end
    ├── SOP
    └── Encoding
```

---

# 58. Conclusão

A segurança de aplicações web não depende de um único mecanismo.

Firewalls, IDS, SIEM e HTTPS são importantes, mas não substituem a implementação segura da própria aplicação.

A aplicação precisa partir da premissa de que:

- o cliente pode ser controlado por um atacante;
- qualquer entrada pode ser manipulada;
- o atacante pode enviar requisições fora do fluxo normal da interface;
- dados precisam ser validados em cada fronteira de confiança;
- a segurança depende da combinação de várias camadas.

A ideia mais importante da aula pode ser resumida em uma frase:

> **Não confie no cliente. Valide, limite, monitore e trate tudo no servidor.**
