# Segurança em Redes — Aula de 07/04

## Criptografia assimétrica, ICP/PKI, certificados digitais, TLS e função hash

> **Observação:** estas anotações foram organizadas a partir da transcrição da aula e dos slides utilizados pelo professor. O objetivo aqui é registrar o conteúdo em formato de estudo, preservando os conceitos, exemplos e observações feitos em sala.
## 1. Retomada: por que a criptografia assimétrica surge?

A aula parte de uma retomada da criptografia simétrica. Na simétrica, uma única chave é usada tanto para cifrar quanto para decifrar. O grande ponto positivo desse modelo é o desempenho. O grande problema é a **distribuição da chave**.

Se essa chave for exposta, perde-se a confidencialidade. Além disso, existe uma dificuldade prática: **como entregar essa chave ao destinatário de forma segura?**. Em algum momento, essa informação precisa passar por um canal que, em geral, é inseguro, ou então depender de um conhecimento prévio, de uma entrega física ou de um meio externo.

A criptografia assimétrica surge justamente para lidar com esse problema.

## 2. Chaves pública e privada

Na criptografia assimétrica, cada ente pode possuir um **par de chaves**:

- **chave pública**;
- **chave privada**.

Essas chaves possuem uma relação matemática entre si. A chave pública deriva da privada, no sentido de que existe um relacionamento computacional entre ambas, mas deve ser **computacionalmente inviável derivar a chave privada a partir da chave pública**.

### Ideia central

- a **chave pública** pode — e deve — ser exposta;
- a **chave privada** deve ser mantida em total sigilo.

O professor reforçou que a chave pública não é apenas algo que “pode” ser publicada. Ela **precisa** ser publicada, porque sem isso não se estabelece o uso da criptografia assimétrica.

Já a chave privada, se comprometida, compromete toda a proteção oferecida por aquele par de chaves. Se um atacante obtiver a chave privada, ele pode:

- decifrar mensagens;
- acessar conteúdos que deveriam ser sigilosos;
- modificar dados;
- cifrar novamente o conteúdo alterado.

## 3. Base matemática e custo computacional

Para que seja computacionalmente inviável derivar a chave privada a partir da pública, os algoritmos trabalham com operações matemáticas complexas e números grandes.

A aula cita, entre outros pontos:

- uso de números grandes;
- uso de números primos em alguns algoritmos;
- curvas elípticas sobre corpos finitos em outros cenários.

Essa base matemática mais pesada explica por que a criptografia assimétrica é **mais lenta** que a simétrica.

## 4. Uso para confidencialidade

Quando o objetivo é **proteger o conteúdo da mensagem**, usa-se a chave pública do destinatário.

### Exemplo conceitual

Se Alice deseja enviar uma mensagem para Bob:

1. Alice obtém a chave pública de Bob;
2. cifra a mensagem com essa chave pública;
3. envia o conteúdo cifrado;
4. somente Bob, com a chave privada correspondente, poderá decifrar a mensagem.

Mesmo que haja um **man-in-the-middle** capturando o tráfego, ele não conseguirá ler o texto plano se não tiver a chave privada do destinatário.

### Ponto importante

Se Bob quiser responder a Alice, ele **não pode** cifrar com a própria chave privada para obter confidencialidade. Se fizer isso, qualquer pessoa que tenha acesso à chave pública dele conseguirá ler a informação. Portanto, para resposta com confidencialidade, Bob precisa da **chave pública de Alice**.

## 5. Uso para assinatura digital

Quando o objetivo não é apenas confidencialidade, mas sim **autenticidade** e **integridade**, o uso se inverte.

Nesse caso:

- assina-se com a **chave privada**;
- verifica-se com a **chave pública**.

A assinatura deve ser feita **na origem dos dados**.

### Ideia central

Ao trocar a ordem de uso das chaves, não se está mais tentando esconder a mensagem em si, mas sim comprovar que:

- ela veio realmente de quem diz ter enviado;
- não foi alterada no caminho.

### Exemplo citado em aula

O professor comenta o caso de sincronismo de tempo e menciona o **NTS (Network Time Security)**. O dado de tempo, por si só, pode não ser sensível. Nesse caso, nem sempre é necessário cifrar toda a informação. Mas é importante assiná-la para que o destinatário consiga verificar que o dado realmente veio do servidor correto e não foi adulterado.

## 6. Assinatura digital com uso de hash

Na prática, em vez de cifrar todo o conteúdo com a chave privada, muitas vezes o processo funciona assim:

1. gera-se um **hash** da mensagem;
2. esse hash é cifrado com a chave privada do emissor;
3. a mensagem segue junto com essa assinatura;
4. o destinatário calcula novamente o hash da mensagem recebida;
5. decifra a assinatura com a chave pública do emissor;
6. compara os dois valores.

Se os hashes coincidirem:

- a mensagem está íntegra;
- a origem é autêntica.

Se forem diferentes, pode ter ocorrido:

- erro na transmissão;
- alteração maliciosa;
- tentativa de fraude.

O professor reforça que **um único bit alterado** já pode modificar completamente o hash.

## 7. Observação prática: sincronismo de relógio e certificados

Um exemplo importante da aula foi a relação entre **tempo do sistema** e **validação de certificados digitais**.

O professor relata um caso real em que uma máquina “não conseguia acessar o Facebook”. O problema não era o site, e sim a bateria da placa-mãe exaurida, o que fazia o relógio ficar incorreto. Como certificados possuem **data de início de validade** e **data de expiração**, o navegador comparava o certificado com o relógio local da máquina. Como a data estava errada, o certificado era interpretado como inválido.

Isso mostra que:

- a validação de certificados depende de tempo correto;
- sincronismo de relógio é um requisito de segurança e disponibilidade;
- falhas simples de infraestrutura podem impactar o acesso a serviços seguros.

## 8. Vantagens e desvantagens da criptografia assimétrica

### Vantagens

- maior segurança na transmissão da informação;
- possibilidade de garantir **autenticidade**;
- possibilidade de garantir **não-repúdio**;
- melhor escalabilidade quanto à distribuição de chaves.

### Desvantagens

- menor desempenho;
- maior custo computacional;
- limitações relacionadas ao tamanho dos dados;
- chaves maiores em alguns algoritmos.

O professor observou que, em um cenário ideal, seria ótimo usar apenas assimétrica, mas isso não é prático em razão do custo computacional. Por isso, diversos protocolos utilizam criptografia assimétrica apenas para **troca de chaves** ou autenticação inicial, e depois passam a trabalhar com **criptografia simétrica por sessão**.

## 9. Criptografia assimétrica em protocolos reais

A aula cita o caso do **SSH**. Mesmo quando o acesso é feito com autenticação por senha, o SSH trabalha com mecanismos de troca de chaves usando criptografia assimétrica. O professor menciona, inclusive, o parâmetro relacionado ao algoritmo de troca de chaves no OpenSSH, como `KexAlgorithms`.

O ponto principal é:

- a criptografia assimétrica pode ser usada para **estabelecer uma chave simétrica segura**;
- depois disso, a comunicação segue de forma mais eficiente.

## 10. Principais algoritmos abordados

### RSA (Rivest, Shamir, Adleman)

O RSA foi apresentado como um dos algoritmos mais utilizados.

Características destacadas:

- baseia-se na fatoração de grandes números primos;
- é usado para:
  - criptografia de dados;
  - autenticação;
  - assinatura digital;
- o professor reforça o tamanho mínimo recomendado de **2048 bits**.

Também foi mostrado que o RSA ainda aparece amplamente em certificados e equipamentos de rede.

### DSA e ECDSA

Foram citados como algoritmos **voltados apenas para assinatura digital**.

- **DSA**: proposto pelo NIST, faz uso de SHA e possui tamanhos mínimos específicos para chaves pública e privada;
- **ECDSA**: versão baseada em curvas elípticas, também para assinatura digital, com chaves menores.

### Curva elíptica / ECC

A criptografia de curva elíptica foi apresentada como baseada em operações matemáticas sobre curvas elípticas em corpos finitos.

Pontos enfatizados:

- permite trabalhar com chaves menores;
- mantém alto nível de segurança;
- apresenta ganho de desempenho em comparação com certos cenários de RSA;
- o tamanho mínimo mencionado em aula foi **256 bits**, com destaque para a curva **P-256** como referência segura em recomendações do NIST.

### Diffie-Hellman-Merkle

Foi apresentado como um mecanismo muito importante para **troca de chaves**.

## 11. Diffie-Hellman-Merkle

O professor explica que, no uso prático, muita gente chama apenas de **Diffie-Hellman (DH)**, mas há contribuição do trabalho de Merkle.

### Ideia do algoritmo

Alice e Bob escolhem parâmetros públicos, como:

- um número primo grande;
- um valor inteiro associado à operação.

Cada um combina esses parâmetros com sua informação privada e gera um valor público. Depois da troca desses valores públicos, ambos conseguem chegar a uma mesma **chave simétrica compartilhada**, mesmo usando um canal inseguro.

### Limitação importante

Se houver um **man-in-the-middle** no momento da troca e ele conseguir substituir as chaves públicas, pode passar a intermediar a comunicação. Ou seja, o Diffie-Hellman resolve vários problemas de troca de chave, mas **não elimina sozinho todos os riscos**, especialmente sem autenticação adicional.

## 12. Comparação: criptografia simétrica x assimétrica

A comparação feita em aula pode ser resumida assim:

### Criptografia simétrica

- mais rápida;
- mesma chave para cifrar e decifrar;
- não possui mecanismo próprio de autenticação;
- não fornece não-repúdio;
- enfrenta o problema da distribuição da chave.

### Criptografia assimétrica

- mais lenta;
- usa chave pública e privada;
- possibilita autenticação por assinatura digital;
- possibilita não-repúdio;
- simplifica a distribuição de chaves;
- quando usada para confidencialidade, o acesso fica restrito ao proprietário da chave privada correspondente.

## 13. Recomendações do NIST

O professor cita recomendações do NIST para tamanhos mínimos de chaves e observa que essas recomendações mudam com o tempo.

Entre os pontos destacados:

- RSA de 2048 bits é aceito até certo período, mas há tendência de exigir **3072 bits** em horizontes posteriores;
- 3DES já se encontra em descontinuação/desuso;
- AES continua relevante com 128, 192 e 256 bits;
- curvas elípticas permanecem como alternativa forte;
- há preocupação crescente com algoritmos resistentes à computação quântica.

O professor observa que já existem propostas pós-quânticas e até suporte experimental em alguns contextos, mas o tema ainda não está consolidado em larga escala.

## 14. Exemplo prático de impacto no desempenho

Foi citado o caso de switches Huawei usados no SMD. Segundo o professor, ao configurar SSH com RSA no limite suportado pelo equipamento, o login ficava perceptivelmente mais lento — a ponto de parecer que o equipamento estava fora do ar — enquanto, após o estabelecimento da sessão, os comandos passavam a responder normalmente.

Esse exemplo foi usado para mostrar que:

- o custo da criptografia assimétrica é real;
- muitas vezes a maior demora está na fase inicial de troca/estabelecimento seguro;
- depois disso, a operação fica mais fluida.

## 15. Introdução à Infraestrutura de Chaves Públicas (ICP / PKI)

Depois da base de criptografia assimétrica, a aula avança para a **Infraestrutura de Chaves Públicas**, também chamada de **ICP** ou **PKI**.

A ideia central é a seguinte: não basta apenas ter pares de chaves. É preciso ter uma forma confiável de:

- criar;
- distribuir;
- validar;
- revogar;
- gerenciar chaves públicas e certificados.

A ICP/PKI é, portanto, o conjunto de ferramentas, entidades e processos usados para dar **confiança operacional** à criptografia assimétrica.

## 16. Objetivo da PKI

Com a PKI, busca-se viabilizar:

- confidencialidade;
- integridade;
- autenticidade;
- não-repúdio.

Mas tudo isso depende de um ponto essencial: **estabelecer confiança**.

Em algum momento, é preciso confiar em uma entidade raiz ou em uma cadeia de confiança previamente estabelecida.

## 17. Entidades da PKI

A aula aborda três papéis centrais:

- **Autoridade Certificadora (CA)**;
- **Autoridade de Registro (RA)**;
- **Autoridade de Verificação (VA)**.

### Autoridade Certificadora (CA)

É responsável por:

- emitir certificados;
- validar certificados;
- revogar certificados;
- assinar certificados;
- gerenciar repositórios;
- manter CRLs;
- manter ou oferecer suporte a OCSP.

### Autoridade de Registro (RA)

Funciona como interface entre usuário e autoridade certificadora.

É a entidade que verifica a identidade do solicitante antes de a CA emitir o certificado.

Na explicação do professor, cartórios e algumas empresas funcionam como esse ponto de validação, verificando documentos físicos antes de liberar a emissão.

### Autoridade de Verificação (VA)

Fornece informações sobre a validade do certificado, inclusive por mecanismos como:

- **CRL**;
- **OCSP**.

## 18. Modelos de confiança

A aula menciona que a confiança em PKI não é “mágica”; ela é **pré-estabelecida**.

Se o certificado raiz estiver confiado no sistema ou no navegador, a confiança é transmitida para certificados subordinados dentro da cadeia.

O professor chama atenção para o fato de que não é necessário “confiar manualmente” em cada certificado da cadeia, desde que a raiz esteja confiada.

## 19. ICP-Brasil

No contexto brasileiro, a aula destaca a **ICP-Brasil**.

### Ponto central

- a autoridade máxima está associada ao **ITI / ICP-Brasil**;
- abaixo dela existem ACs de níveis inferiores;
- essas ACs podem emitir certificados para pessoas, empresas, serviços e outros usos específicos.

Foram citados exemplos como:

- SERPRO;
- Serasa;
- Certisign;
- Caixa;
- Ministério da Justiça;
- outros órgãos com ACs próprias em determinados contextos.

O professor reforça que a raiz **não entrega certificados diretamente a usuários ou dispositivos**. Ela autoriza e estrutura a cadeia de confiança.

## 20. Tipos de autoridades certificadoras citados

Foram mencionados diferentes tipos de AC:

- AC raiz;
- AC intermediária;
- AC comercial;
- AC de domínio;
- AC de e-mail;
- AC de assinatura de código;
- AC de identidade;
- AC de dispositivo.

Isso mostra que a PKI não se resume a certificados para sites.

## 21. Certificado digital: conceito

O certificado digital foi apresentado como um **documento eletrônico** que liga uma **chave pública** a uma **identidade**.

Ele pode comprovar a identidade de:

- pessoa física;
- empresa;
- site;
- dispositivo;
- serviço.

O certificado contém, entre outras informações:

- chave pública;
- dados do titular;
- período de validade;
- emissor;
- algoritmo de assinatura;
- assinatura digital da autoridade certificadora.

## 22. Exemplo em navegador

O professor mostra certificados em navegador e explica que, ao clicar no cadeado, é possível verificar:

- quem emitiu o certificado;
- para quem ele foi emitido;
- a validade;
- o algoritmo usado;
- a chave pública;
- nomes alternativos do certificado.

Foi comentado, por exemplo, o caso do Google e de certificados usando **curva P-256**.

## 23. Wildcard e certificados específicos

A aula também diferencia certificados **wildcard** e certificados específicos.

### Wildcard

Exemplo: `*.ufc.br`

Esse tipo de certificado permite cobrir vários subdomínios. É útil para simplificar a administração.

### Certificados específicos

Para sistemas mais críticos, o professor observa que o ideal é usar certificados específicos, e não depender apenas de wildcard.

A justificativa é que, em serviços mais sensíveis, convém ter mais granularidade e controle.

## 24. Subject Alternative Name (SAN)

O professor também destaca os **Subject Alternative Names (SANs)**, que permitem que um mesmo certificado seja válido para múltiplos nomes e domínios.

Esse ponto apareceu no exemplo do Google, em que o mesmo certificado atende vários domínios e serviços.

## 25. Tipos de validação de certificados

A aula diferencia tipos de validação:

### DV — Domain Validation

Valida essencialmente que o solicitante controla o domínio.

Foi comentado que o **Let’s Encrypt**, em geral, está muito associado a esse modelo.

### OV — Organization Validation

Além do domínio, há validação organizacional.

### EV — Extended Validation

Validação mais criteriosa, muito associada a instituições financeiras e ambientes em que se quer reforçar a confiança na identidade da organização.

O professor menciona que, em sua observação, viu EV em exemplos como Banco do Brasil e Itaú.

## 26. Let’s Encrypt

O Let’s Encrypt foi mencionado como uma alternativa bastante utilizada para emissão de certificados de domínio, geralmente de forma gratuita.

O professor observa que ele é fortemente associado à validação de domínio e que tem seu papel sobretudo em cenários em que se quer praticidade na emissão e renovação.

## 27. Certificados pessoais e assinatura com validade jurídica

A aula menciona certificados como:

- **e-CPF**;
- **e-CNPJ**.

Esses certificados permitem assinatura digital com validação formal, normalmente associados a tokens, smart cards ou outros mecanismos de proteção da chave privada.

Também foi comentado o uso do **gov.br** para assinatura digital. O professor chama atenção para um ponto importante: a validade está no **documento digital íntegro**. Imprimir esse documento faz perder a característica de validação digital daquele arquivo.

## 28. Active Directory como CA interna

Outro ponto muito interessante foi a observação de que uma organização pode manter uma **autoridade certificadora interna**, por exemplo com **Active Directory**.

Nesse caso:

- a CA interna não precisa ser confiada fora da rede;
- ela pode emitir certificados para autenticação e serviços internos;
- isso é útil para autenticação em ambientes corporativos.

O professor inclusive observa que, apesar das críticas frequentes ao ecossistema Windows, o AD continua sendo uma ferramenta muito forte para gestão centralizada e políticas de grupo.

## 29. Revogação: CRL e OCSP

A aula distingue dois mecanismos centrais de revogação:

### CRL — Certificate Revocation List

- lista estática;
- precisa ser baixada e consultada.

### OCSP — Online Certificate Status Protocol

- serviço online;
- permite checar em tempo real se um certificado foi revogado.

### Observação importante do professor

O uso de OCSP pode gerar discussão sobre **privacidade**, porque a autoridade certificadora consegue saber que determinado cliente consultou o status de um certificado específico, o que pode revelar indiretamente acessos a certos serviços.

## 30. Emissão de certificados e CSR

A emissão de certificados foi descrita com base no uso de **CSR (Certificate Signing Request)**.

Fluxo geral:

1. o usuário ou servidor gera um par de chaves;
2. cria uma CSR;
3. envia a requisição para a autoridade certificadora;
4. a AC/RA valida os dados;
5. a AC assina o certificado;
6. o certificado passa a poder ser usado no serviço.

O professor reforça que a **chave privada não deveria sair do controle do titular**.

## 31. Lifecycle management e operação contínua

Um certificado não deve ser visto apenas como um arquivo isolado. Há todo um ciclo de vida:

- request;
- emissão;
- provisionamento;
- discovery;
- inventário;
- monitoramento;
- renovação;
- revogação.

Esse ponto foi trabalhado com ênfase operacional.

### Observação importante

Se a renovação não for bem monitorada e agendada, o sistema pode ficar indisponível assim que o certificado expirar. Como a validade é por **data e hora**, e não apenas “por dia”, a falta de atenção pode causar interrupções reais.

O professor ressalta que, com prazos de validade cada vez menores, a tendência é que a renovação precise ser **automatizada**.

## 32. Formatos de certificado

A aula comenta formatos em texto e binário.

### Formatos em texto

- PEM;
- PKCS #7;
- Base64.

### Formatos binários

- DER;
- PKCS #12.

Também foi observado que, no Linux, a extensão do arquivo não é o mais importante; o sistema e as ferramentas podem reconhecer o conteúdo pelo próprio formato interno.

## 33. Cadeia de certificados e bundle

Quando há cadeia com múltiplos certificados, pode ser necessário montar um **bundle**, concatenando certificados em ordem apropriada para atender à aplicação que irá usá-los.

O professor observa que nem toda aplicação permite inserir vários arquivos separadamente, então o empacotamento da cadeia pode ser necessário.

## 34. Visualização com OpenSSL

Mesmo quando um certificado está em Base64 e parece ilegível, é possível usar o **OpenSSL** para visualizá-lo em formato compreensível, semelhante ao que navegadores exibem.

## 35. Componentes do certificado X.509

A aula apresenta o padrão **X.509**, atualmente usado sobretudo na **versão 3**.

Os principais componentes citados foram:

- versão;
- número de série;
- algoritmo de assinatura;
- nome do emitente;
- período de validade;
- nome do sujeito/titular;
- chave pública;
- indicador do algoritmo;
- extensões;
- assinatura digital.

### Observações adicionais

- na versão 3 aparecem extensões importantes, inclusive indicando se o certificado é de CA;
- o **Common Name (CN)** foi destacado como elemento obrigatório do sujeito;
- o CN indica o nome do site, serviço ou entidade à qual o certificado se refere.

## 36. Firefox e armazenamento de certificados raiz

O professor comenta que o **Firefox** possui seu próprio repositório de certificados raiz, diferentemente de cenários em que se usa diretamente o repositório do sistema operacional.

Isso é relevante para entender por que manter navegador e sistema atualizados impacta diretamente a cadeia de confiança.

## 37. Comprometimento de autoridade certificadora

Foi lembrado que, se uma autoridade certificadora for comprometida, navegadores e sistemas podem removê-la da lista de confiança por atualização.

A consequência prática é que certificados emitidos por ela passam a ser tratados como não confiáveis.

## 38. TLS: sucessor do SSL

A aula também conecta toda essa base com o **TLS**.

### Pontos principais

- SSL é considerado obsoleto;
- TLS é o sucessor;
- o uso atual se concentra em **TLS 1.2** e **TLS 1.3**;
- o TLS 1.3 reduz handshakes e remove suporte a algoritmos antigos e fracos.

## 39. Onde o TLS atua

O professor explica que o TLS atua entre transporte e aplicação, protegendo comunicações específicas, como:

- HTTPS;
- IMAPS;
- SMTPS;
- FTPS.

Também chama atenção para uma confusão comum:

- **FTPS** usa TLS;
- **SFTP** usa SSH.

## 40. Autenticação mútua em TLS

Em muitos casos, o cliente autentica apenas o servidor. Mas a aula também menciona o cenário de **autenticação mútua**, em que o servidor exige certificado digital do cliente.

Isso aparece em contextos como:

- sistemas governamentais;
- ambientes jurídicos;
- acesso com token ou smart card.

## 41. Função hash

Na parte final do conteúdo associado aos slides, aparecem as **funções hash**.

Elas foram apresentadas como uma espécie de **impressão digital dos dados**.

### Características centrais

- recebem entrada de tamanho arbitrário;
- produzem saída de tamanho fixo;
- são determinísticas;
- a mesma entrada gera a mesma saída.

## 42. Usos de função hash

Entre os usos citados:

- armazenamento de senhas;
- assinaturas digitais;
- autenticação;
- integridade de dados;
- estruturas de dados e indexação.

## 43. Propriedades importantes da função hash

A aula destaca propriedades fundamentais:

### Determinismo

Mesma entrada, mesma saída.

### Saída de tamanho fixo

Independentemente do tamanho da entrada, a saída tem tamanho fixo.

### Resistência à pré-imagem

Dado o hash, deve ser inviável descobrir a entrada original.

### Resistência à segunda pré-imagem

Dado um valor de entrada, deve ser inviável encontrar outro valor diferente que produza o mesmo hash.

### Resistência a colisões

Deve ser inviável encontrar duas entradas diferentes com o mesmo hash.

## 44. Colisão

A noção de **colisão** é importante porque mostra o limite de segurança de funções hash fracas.

A aula ilustra que dois conteúdos diferentes não deveriam produzir o mesmo hash em um contexto seguro. Quando isso se torna viável, a função deixa de ser adequada para uso criptográfico sério.

## 45. Algoritmos hash citados

Foram mencionados:

- **MD5**;
- **SHA-1**;
- **SHA-2**:
  - SHA-224
  - SHA-256
  - SHA-384
  - SHA-512
- **SHA-3**:
  - SHA3-224
  - SHA3-256
  - SHA3-384
  - SHA3-512

O conteúdo deixa claro o contexto de evolução: MD5 e SHA-1 aparecem historicamente, mas algoritmos mais modernos são os mais adequados para uso atual.

## 46. Hash e integridade de dados

A integridade por hash funciona da seguinte forma:

1. gera-se o hash do conteúdo antes do envio;
2. o destinatário recebe os dados;
3. recalcula o hash;
4. compara com o valor esperado.

Se os valores coincidirem, a integridade é preservada.

## 47. MAC — Message Authentication Code

A aula também apresenta o **MAC**, que combina o uso de algoritmo com uma **chave secreta compartilhada**.

O MAC permite verificar:

- integridade;
- autenticidade.

A diferença para o hash simples é justamente a presença da chave secreta.

## 48. Hash e assinatura digital

O elo entre hash e assinatura digital aparece com muita força na aula:

- primeiro gera-se o hash da mensagem;
- depois assina-se esse hash com a chave privada;
- o destinatário verifica com a chave pública.

Isso evita cifrar toda a mensagem assimetricamente e torna o processo mais eficiente.

## 49. Síntese geral da aula

A aula de 07/04 articula um bloco muito importante da disciplina:

1. **criptografia assimétrica** como solução para o problema de distribuição de chaves;
2. **assinatura digital** como mecanismo de autenticidade, integridade e não-repúdio;
3. **algoritmos assimétricos** mais importantes, com destaque para RSA, DSA/ECDSA, ECC e Diffie-Hellman;
4. **PKI / ICP** como estrutura de confiança para chaves públicas e certificados;
5. **certificados digitais** como vínculo entre chave pública e identidade;
6. **TLS** como aplicação prática dessa infraestrutura em protocolos seguros;
7. **função hash** como base para integridade, MAC e assinatura digital.
