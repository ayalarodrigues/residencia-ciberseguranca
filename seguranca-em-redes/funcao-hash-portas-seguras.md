# Aula 08/04 — Função Hash e Redes Seguras

## Visão geral da aula

A aula articulou dois blocos principais de conteúdo:

1. **Função Hash**, com foco no conceito, propriedades, aplicações e relação com integridade e proteção de senhas.
2. **Redes Seguras**, com foco em infraestrutura, encaminhamento de tráfego, segmentação, dispositivos de borda, ataques e defesa em profundidade.

A explicação foi bastante prática, sempre relacionando os conceitos com exemplos reais de rede, uso de senhas, verificação de arquivos e infraestrutura da UFC.

---

# 1. Função Hash

## 1.1. O que é uma função hash

Uma função hash executa operações sobre um dado de entrada e retorna uma **saída de tamanho fixo**, independentemente do tamanho do conteúdo original.

A ideia central é que ela funcione como uma espécie de **impressão digital** dos dados.

### Ponto central destacado em aula

- a entrada pode ter tamanho arbitrário;
- a saída tem tamanho fixo;
- a mesma entrada deve sempre produzir a mesma saída.

Ou seja, trata-se de uma função **determinística**.

## 1.2. Determinismo

O professor reforçou que uma função hash precisa ser determinística porque, se a mesma entrada gerasse saídas diferentes a cada execução, ela deixaria de ser útil para verificação de integridade ou para autenticação.

Exemplo discutido em aula:

- se o sistema armazenou o hash de uma senha;
- no momento do login, o sistema calcula novamente o hash da senha digitada;
- isso só funciona se **a mesma senha gerar exatamente o mesmo hash**.

## 1.3. Saída fixa e entrada variável

Uma característica importante das funções hash é que a saída possui tamanho fixo, mesmo quando a entrada é muito maior.

O professor comentou o caso do **MD5**, cuja saída é de **128 bits**, e usou como exemplo uma frase longa para mostrar que, ainda que a entrada seja extensa, a saída continua tendo sempre o mesmo tamanho.

Isso também foi relacionado ao exemplo de uma **imagem ISO do Kali Linux**, que pode ter vários gigabytes e, ainda assim, gerar um hash compacto.

## 1.4. Impressão digital dos dados

A função hash foi apresentada como uma forma de representar um conjunto de dados de tamanho arbitrário por meio de um valor compacto.

Essa ideia de “impressão digital” é útil porque permite:

- verificar se um arquivo foi alterado;
- comparar conteúdos sem precisar analisar todo o dado original;
- apoiar mecanismos de autenticação e assinatura.

---

## 1.5. Aplicações das funções hash

Os slides e a fala do professor associaram as funções hash aos seguintes usos:

- armazenamento de senhas;
- autenticação;
- integridade de dados;
- assinaturas digitais;
- estruturas de dados e indexação de informações.

### Exemplo prático citado

Foi mencionado o uso de **checksum** para verificar a integridade de imagens ISO. Ao baixar uma distribuição como o Kali Linux, o site costuma disponibilizar um valor de hash para que o usuário compare com o arquivo baixado.

Se o hash calculado localmente coincidir com o valor publicado, isso indica que o arquivo chegou íntegro.

---

## 1.6. Propriedades importantes de uma função hash

### a) Determinística

A mesma entrada precisa gerar a mesma saída.

### b) Entrada de tamanho variado

A função deve aceitar entradas de tamanhos diferentes. Caso contrário, sua utilidade ficaria muito limitada.

### c) Saída de tamanho fixo

Independentemente do tamanho da entrada, o hash terá tamanho fixo.

### d) Resistência à pré-imagem

Significa que, conhecendo o algoritmo e o valor do hash, não deve ser viável reconstruir a entrada original.

Em outras palavras, o hash não foi feito para ser “desfeito”.

O professor relacionou isso ao vazamento de senhas: quando alguém obtém hashes de senhas, ainda assim precisa recorrer a técnicas como:

- ataque de dicionário;
- força bruta;
- ferramentas como **John the Ripper**.

Mesmo assim, o sucesso do ataque depende muito do tamanho e da força da senha.

### e) Resistência à segunda pré-imagem

Esse conceito apareceu como algo mais teórico, ligado à construção das funções hash, e o professor comentou que não é um tema do uso cotidiano.

A ideia geral é que, dado um valor de entrada conhecido, deve ser impraticável encontrar **outra entrada diferente** que gere o mesmo hash.

### f) Resistência a colisões

Há colisão quando **duas entradas diferentes produzem a mesma saída**.

O professor explicou que, como a saída tem tamanho fixo e o espaço de entrada pode ser enorme, a possibilidade matemática de colisão existe. O problema não é a existência teórica da colisão, mas sim a facilidade com que ela poderia ser encontrada.

Por isso, uma boa função hash precisa tornar essa descoberta **computacionalmente impraticável**.

### g) Efeito de avalanche

Foi enfatizado que pequenas mudanças na entrada, idealmente até mesmo em **um único bit**, devem produzir um hash completamente diferente.

Isso diferencia uma função hash criptográfica de uma operação simples de soma ou subtração.

---

## 1.7. Salt no armazenamento de senhas

O professor também comentou o papel do **salt**.

O salt é um valor adicional que o sistema associa à senha antes de gerar o hash. Esse valor:

- não precisa ser conhecido pelo usuário;
- não deve ser o mesmo para todas as senhas;
- ajuda a gerar hashes diferentes mesmo quando dois usuários escolhem a mesma senha.

Isso dificulta ataques baseados em tabelas prontas e reduz o impacto de reutilização de senhas iguais.

---

## 1.8. Tamanho dos blocos e algoritmos citados

A aula mencionou alguns algoritmos clássicos e suas características gerais:

### MD5

- blocos de entrada de **512 bits**;
- saída de **128 bits**.

### SHA-1

- blocos de entrada de **512 bits**;
- saída de **160 bits**.

### SHA-2

Família que inclui:

- SHA-224;
- SHA-256;
- SHA-384;
- SHA-512.

### SHA-3

Família que inclui:

- SHA3-224;
- SHA3-256;
- SHA3-384;
- SHA3-512.

### Observação importante feita em aula

O professor destacou que, para **armazenamento de senhas**, a recomendação apresentada foi utilizar **pelo menos SHA-2 256**.

Já o **MD5**, embora inadequado para cenários modernos de proteção de senhas, ainda foi citado como algo que pode aparecer em verificações de integridade de arquivos, dependendo do contexto.

---

## 1.9. Padding

Foi mencionado que alguns algoritmos trabalham com blocos de tamanho fixo. Quando a entrada não se encaixa exatamente nesse tamanho, ela precisa ser **preenchida**.

Esse preenchimento é chamado de **padding**.

---

## 1.10. Integridade de dados

A integridade apareceu como uma aplicação central das funções hash.

A lógica é simples:

- calcula-se o hash de um conteúdo original;
- depois, calcula-se novamente o hash do conteúdo recebido;
- se os valores coincidirem, assume-se que o conteúdo não foi alterado.

Isso foi relacionado tanto ao download de arquivos quanto à proteção de mensagens e dados trafegados.

---

## 1.11. MAC e assinatura digital

Os slides também apontaram dois usos importantes das funções hash:

### Message Authentication Code (MAC)

O MAC combina a mensagem com uma **chave secreta**, permitindo verificar:

- integridade;
- autenticidade.

Ou seja, não basta que a mensagem permaneça inalterada: é importante também confirmar que ela veio de quem deveria ter enviado.

### Assinatura digital

A assinatura digital usa o hash da mensagem em conjunto com criptografia assimétrica para permitir:

- validação de autoria;
- validação de integridade.

Assim, qualquer alteração na mensagem tende a invalidar a verificação da assinatura.

---

# 2. Redes Seguras

## 2.1. O que são redes seguras

Nos slides, redes seguras foram associadas a três pilares principais:

- **confiabilidade**;
- **integridade**;
- **disponibilidade**.

A aula reforçou que segurança de redes não se resume a bloquear ataques. Ela também envolve manter a rede funcionando corretamente, com estrutura adequada, redundância quando possível e proteção contra falhas.

## 2.2. Problemas comuns em redes

Entre os problemas destacados nos slides e retomados na explicação, aparecem:

- pontos únicos de falha;
- insegurança por padrão;
- ausência de documentação;
- excesso de concentração na defesa de perímetro.

A ideia é que uma rede insegura não é apenas aquela que sofre ataque, mas também aquela mal planejada, mal documentada ou excessivamente dependente de um único elemento crítico.

---

## 2.3. Principais dispositivos de rede

Os slides listam como dispositivos centrais:

- firewalls;
- switches;
- pontos de acesso sem fio;
- roteadores;
- servidores DNS;
- balanceadores de carga.

Esses elementos aparecem como parte da infraestrutura que sustenta a segurança, o encaminhamento do tráfego e a disponibilidade dos serviços.

---

## 2.4. Encaminhamento de tráfego

### Roteadores

Os roteadores operam na **camada 3**, realizando o roteamento entre redes distintas.

Trabalham com:

- IPv4;
- IPv6.

### Switches

Os switches operam na **camada 2**, realizando a comutação de quadros dentro da mesma LAN.

Trabalham com:

- endereços MAC.

O professor retomou essa distinção para reforçar a ideia de que:

- **IP** está relacionado à comunicação lógica;
- **TCP e UDP** estão ligados ao transporte entre processos.

---

## 2.5. Camada de rede e fragmentação

A aula destacou o papel do **Internet Protocol (IP)** como responsável por encapsular os dados e permitir a comunicação lógica entre dispositivos.

Também houve uma discussão importante sobre **fragmentação de pacotes**.

### IPv4 e a fragmentação no núcleo da rede

O professor explicou que o IPv4 permitia fragmentação em roteadores no meio do caminho, o que gerava problemas.

Exemplo dado:

- um pacote grande pode sair de uma rede que suporta quadros maiores;
- ao encontrar um enlace menor, o roteador precisa fragmentar esse pacote;
- isso pode abrir espaço para abuso, especialmente se forem enviados muitos fragmentos sem que o equipamento saiba claramente quando a remontagem terminará.

Nesse cenário, seria possível consumir memória do roteador até prejudicar o encaminhamento normal do tráfego, causando **negação de serviço**.

### IPv6

Na explicação, o IPv6 foi apresentado como uma abordagem em que a fragmentação não fica no núcleo da rede da mesma forma. O pacote é ajustado na origem/interface antes de seguir, e a remontagem acontece no hospedeiro de destino.

---

## 2.6. Resolução de IP em MAC

Os slides apresentam dois mecanismos principais:

### ARP

- usado em redes IPv4;
- opera na camada 2;
- utiliza **ARP Request** e **ARP Reply**;
- trabalha com **broadcast**.

### NDP

- usado em redes IPv6;
- vai além de apenas descobrir endereços MAC;
- envolve mensagens como **NS, NA, RS e RA**;
- trabalha com **multicast**.

---

## 2.7. Segmentação e segregação de rede

A segmentação de rede foi mostrada como a prática de dividir a rede em partes menores.

### Objetivos da segmentação

- isolar segmentos na camada 2;
- reduzir domínio de broadcast;
- segregar tráfego;
- melhorar gerenciamento;
- favorecer desempenho.

Mais adiante, esse tema reapareceu na explicação sobre **defesa em profundidade**, em que a segmentação com **VLANs** foi apresentada como barreira contra movimentação lateral do atacante.

---

## 2.8. Internet, intranet, extranet e DMZ

Os slides trouxeram a separação entre diferentes contextos de acesso:

### Intranet

Ambiente interno da organização, voltado ao uso autorizado dentro da instituição.

### Extranet

Extensão controlada de acesso para parceiros, clientes ou agentes externos autorizados.

### Internet

Rede pública, ampla e não confiável por padrão.

### DMZ (Zona Desmilitarizada)

A DMZ aparece como uma zona intermediária entre a rede interna e a internet, normalmente usada para hospedar serviços públicos, como:

- servidores web;
- servidores de e-mail.

A função da DMZ é reduzir o risco de exposição direta da rede privada.

---

## 2.9. Exemplo prático da infraestrutura da UFC

O professor utilizou a topologia da UFC como exemplo real para discutir redundância e ponto único de falha.

### Observações destacadas

- enlaces em vermelho representavam **1 Gigabit**;
- enlaces em amarelo representavam **10 Gigabits**;
- enlaces em azul representavam **100 Gigabits**.

O **Pici** foi apontado como o único ponto com enlace de **100 Gbps**, justamente por concentrar o **POP-CE** e o ponto de presença da **RNP**.

### Consequência prática

Todo o tráfego da UFC destinado à internet converge para o Pici. Assim:

- se o Pici falhar, a UFC perde a saída para a internet;
- o tráfego interno entre unidades ainda pode continuar funcionando, desde que os caminhos locais permaneçam ativos.

Esse exemplo foi usado para ilustrar claramente o conceito de **ponto crítico** e **ponto único de falha**.

---

## 2.10. Ataques de negação de serviço

A aula apresentou os ataques de **negação de serviço (DoS)** como ataques cujo objetivo principal não é roubar dados, mas sim tornar o serviço indisponível.

### Flooding

Uma forma clássica de DoS é o **flooding**, em que o atacante envia um volume muito grande de pacotes ou conexões para esgotar a capacidade do alvo.

Exemplo dado em aula:

- se um servidor suporta 1000 conexões simultâneas;
- e o atacante cria 2000 conexões falsas;
- os usuários legítimos podem ficar sem acesso.

### DDoS

O **Distributed Denial of Service (DDoS)** amplia esse problema:

- o ataque não parte de uma só máquina;
- ele parte de várias máquinas distribuídas;
- essas máquinas podem formar botnets ou conjuntos de hosts comprometidos.

Nesse caso, a mitigação se torna mais difícil porque o tráfego malicioso vem de muitos IPs e lugares diferentes.

---

## 2.11. Firewalls e dispositivos de borda

O firewall foi apresentado como uma barreira entre:

- a rede interna, tratada como confiável;
- a internet, tratada como não confiável.

### Função principal

Filtrar o tráfego com base em regras.

Exemplo mencionado:

- permitir apenas portas **80** e **443** para um servidor web;
- bloquear portas como **22 (SSH)** e portas de banco de dados para conexões externas.

Esse controle reduz a **superfície de ataque**.

### NGFW

A aula também mencionou os **Next-Generation Firewalls (NGFW)**, que analisam mais do que IP e porta.

Eles podem inspecionar o conteúdo do tráfego para identificar:

- ataques conhecidos;
- aplicações indevidas trafegando em portas aparentemente normais.

---

## 2.12. IDS e IPS

A distinção entre IDS e IPS foi explicada de forma direta.

### IDS — Intrusion Detection System

- atua de forma passiva;
- monitora o tráfego;
- gera alertas quando detecta algo suspeito;
- não interrompe diretamente o ataque.

### IPS — Intrusion Prevention System

- atua de forma ativa;
- fica inline no tráfego;
- detecta assinaturas ou comportamentos anômalos;
- pode derrubar a conexão e bloquear a ação maliciosa.

### Comparação usada pelo professor

- o **IDS** é como um vigia que observa e avisa;
- o **IPS** é como um vigia que observa e já intervém.

Foi comentado ainda que, na prática, muitas empresas usam esses mecanismos em conjunto ou integrados ao firewall.

---

## 2.13. Segurança física

A aula destacou que segurança de rede não é só tráfego, protocolo e pacote.

Também envolve o ambiente físico onde a infraestrutura está instalada.

### Pontos citados

- controle de acesso ao datacenter;
- monitoramento por câmera;
- refrigeração;
- nobreaks;
- geradores.

A ideia central é que, se o serviço fica indisponível por falha física, falta de energia ou acesso indevido aos equipamentos, a segurança também foi comprometida.

---

## 2.14. Man-in-the-Middle (MitM)

Outro ataque abordado foi o **Man-in-the-Middle**.

Nesse tipo de ataque, o invasor se coloca entre duas partes que acreditam estar se comunicando diretamente.

### O que o atacante pode fazer

- interceptar o tráfego;
- ler dados transmitidos;
- alterar a mensagem antes de repassá-la.

### Formas de mitigação citadas

- uso de **HTTPS**;
- uso de **certificados digitais**;
- validação da identidade do servidor.

O professor chamou atenção para o risco de **Wi-Fi público**, em que um atacante pode criar um ponto de acesso falso e induzir a vítima a se conectar.

---

## 2.15. Defesa em profundidade

A aula encerrou com o conceito de **Defesa em Profundidade**.

A comparação usada foi a de camadas de uma cebola: mesmo que uma barreira falhe, outras ainda devem existir.

### Camadas destacadas na explicação

1. **segmentação de rede com VLANs**;
2. **controle de acesso com autenticação forte**, como MFA;
3. **criptografia de dados em repouso**.

A ideia principal é que não existe solução única e absoluta. Segurança depende de múltiplas camadas atuando em conjunto.

---

# 3. Síntese final da aula

A aula articulou segurança de redes e funções hash como partes complementares da proteção da informação.

De um lado, as **funções hash** apareceram como base para:

- integridade;
- autenticação;
- proteção de senhas;
- assinatura digital.

Do outro, **redes seguras** foram tratadas como resultado de:

- boa topologia;
- redução de pontos únicos de falha;
- segmentação;
- filtragem de tráfego;
- monitoramento;
- proteção física;
- uso de múltiplas camadas de defesa.

A mensagem final da aula foi que segurança não é algo estático. Trata-se de um processo contínuo de:

- monitoramento;
- ajuste;
- revisão da infraestrutura;
- combinação de mecanismos técnicos e organizacionais.
