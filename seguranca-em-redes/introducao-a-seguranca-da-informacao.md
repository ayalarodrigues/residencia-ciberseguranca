# Segurança em Redes - Aula de 06/04

## Introdução à Segurança da Informação


## 1. Nivelamento inicial da turma

A aula começou com uma introdução aos conceitos fundamentais da segurança, como forma de nivelar a turma, inclusive considerando que parte dos alunos já atua na área. O professor destacou que esse alinhamento é importante para que todos tenham a mesma base antes das partes práticas da disciplina.

Os temas anunciados para esse início foram:

- tríade CID/CIA;
- vulnerabilidade, ameaça e risco;
- introdução ao **Cybersecurity Framework do NIST**;
- princípios gerais da segurança;
- criptografia;
- ataques e engenharia social.

## 2. Segurança da Informação x Cibersegurança

Um dos primeiros pontos enfatizados foi a diferença entre **Segurança da Informação** e **Cibersegurança**.

A ideia central foi a seguinte:

- **Segurança da Informação** é mais ampla;
- **Cibersegurança** faz parte da Segurança da Informação, mas não a esgota;
- além do meio digital, a Segurança da Informação também envolve informação física, controle de acesso a ambientes, documentos, processos e aspectos burocráticos e administrativos.

Ou seja, não basta pensar apenas em computadores, rede, servidor e internet. Também é necessário considerar:

- acesso físico a prédios e salas;
- proteção de documentos;
- definição de regras;
- processos de controle;
- práticas legais e documentais.

Mesmo sendo uma residência voltada à segurança cibernética, o professor deixou claro que não se pode ignorar a parte mais administrativa e organizacional da segurança.

## 3. O que é segurança na prática

A segurança foi apresentada como um **conjunto de práticas técnicas, legais, documentais e tecnológicas** voltadas para:

- prevenir;
- detectar;
- corrigir;
- controlar acessos;
- segregar ambientes;
- reduzir impactos quando incidentes acontecerem.

Entre os exemplos citados, apareceram elementos como:

- firewall;
- servidor de autenticação;
- IDS;
- IPS;
- switches;
- segmentação de rede;
- separação entre rede de usuários e DMZ.

Também foi comentado que **não existe sistema 100% seguro**. Por isso, a segurança não se resume a evitar incidentes, mas também a detectá-los e responder adequadamente.

## 4. Exemplo prático de rede e autenticação

Durante a explicação, o professor comentou a própria rede do ambiente da aula e observou que, ao conectar o equipamento, não houve autenticação prévia antes de receber IP. A partir disso, ele comparou cenários como:

- **captive portal**: o usuário já recebe IP antes da autenticação;
- **WPA2/WPA3 Enterprise com 802.1X**: o IP só é entregue após autenticação.

Isso foi usado para mostrar que a forma como a rede é estruturada interfere diretamente na segurança.

## 5. Tríade da Segurança da Informação: CID / CIA

O professor apresentou a famosa tríade da segurança:

- **Confidencialidade**;
- **Integridade**;
- **Disponibilidade**.

Esses três elementos foram apresentados como o **pilar** da Segurança da Informação.

### Confidencialidade

Garante que a informação só seja acessada por quem tem autorização.

Exemplo dado em aula: se uma mensagem é enviada a uma pessoa específica, espera-se que apenas ela tenha acesso ao conteúdo. Se alguém intercepta e lê essa mensagem, a confidencialidade foi quebrada.

A principal tecnologia associada a esse princípio foi a **criptografia**.

### Integridade

Garante que a informação não foi alterada indevidamente durante o armazenamento ou a transmissão.

Exemplo usado em sala: uma mensagem enviada com determinado valor não pode chegar ao destino com outro valor. Se isso ocorre, a integridade foi comprometida.

Para isso, foram mencionados mecanismos como:

- hash;
- assinaturas digitais.

### Disponibilidade

Garante que o sistema, a informação ou o serviço estejam acessíveis quando necessário.

O professor enfatizou que, na área de redes, a disponibilidade costuma assumir papel central. Quando há uma falha, o que geralmente mais mobiliza as pessoas não é perguntar se os dados continuam íntegros ou confidenciais, mas sim por que o sistema caiu, por que a internet não funciona ou por que o serviço está indisponível.

## 6. Outros princípios além da tríade

Embora a tríade tenha sido apresentada como base, o professor também mencionou outros princípios importantes:

- **Autenticidade**: garantir que quem enviou algo é realmente quem diz ser;
- **Não-repúdio / irretratabilidade**: impossibilidade de negar a autoria de uma ação;
- **Conformidade**: aderência a normas, padrões e legislações;
- **Autenticação e autorização**: ligadas ao controle de acesso;
- **Princípio do menor privilégio**: cada pessoa deve ter apenas o mínimo de acesso necessário para exercer sua função.

Nesse ponto, ele chamou atenção para o fato de que segurança frequentemente gera atrito dentro das organizações, porque limitar acessos costuma contrariar expectativas de quem ocupa cargos mais altos, mas não precisa necessariamente operar certos sistemas.

## 7. A centralidade da disponibilidade em redes

Um ponto muito marcante da aula foi a observação de que, para a área de redes, a disponibilidade costuma ser a prioridade mais visível.

Para ilustrar isso, o professor relatou um caso real de falha em firewall na UFC, envolvendo a porta WAN de interligação com a RNP. Nesse incidente:

- quem estava dentro da instituição ainda conseguia trabalhar em certa medida;
- quem estava fora não conseguia acessar os sistemas;
- o grande problema percebido por todos era a indisponibilidade.

Com isso, ele mostrou que, na prática, a cobrança mais imediata costuma girar em torno da manutenção dos serviços no ar.

## 8. Vulnerabilidade, ameaça e risco

A aula também diferenciou três conceitos fundamentais.

### Vulnerabilidade

É uma fraqueza, falha ou ponto explorável.

### Ameaça

É a possibilidade, evento ou circunstância que pode explorar uma vulnerabilidade e causar impacto negativo.

### Risco

É a probabilidade de uma ameaça explorar uma vulnerabilidade, considerando também o impacto dessa exploração.

A gestão de risco exige observar:

- quais são os ativos da organização;
- quais vulnerabilidades esses ativos possuem;
- quais ameaças incidem sobre eles;
- qual a probabilidade de exploração;
- qual o impacto para a instituição.

## 9. Ativos e análise de risco

O professor destacou que a análise de risco começa pelo **levantamento de ativos**.

Ativos não são apenas computadores. Também podem ser:

- pessoas;
- prédio físico;
- equipamentos diversos;
- dispositivos de rede;
- sistemas;
- infraestrutura.

A partir da identificação dos ativos, passa-se a analisar vulnerabilidades, ameaças, probabilidade de exploração e impacto.

A gestão de risco foi apresentada como um **ciclo contínuo**, e não como uma ação isolada.

## 10. Tratamento do risco

Foram apresentadas quatro estratégias clássicas de tratamento do risco:

- **aceitar**;
- **mitigar**;
- **transferir**;
- **evitar**.

### Aceitar

Quando o risco é baixo e o custo de tratá-lo não compensa.

### Mitigar

Quando se reduzem as chances ou os impactos, mesmo sem eliminar totalmente o problema.

Exemplo citado: a falha **Spectre**, mitigada por software e alterações no kernel, ainda com perda de performance.

### Transferir

Quando o risco é repassado a terceiros.

Exemplo dado: **seguros**, inclusive no contexto de LGPD e possíveis vazamentos.

### Evitar

Quando se elimina a fonte do problema ou se substitui o elemento vulnerável.

Foi comentado, por exemplo, que evitar integralmente um risco pode exigir trocar o componente vulnerável, em vez de apenas mitigá-lo.

## 11. Severidade x probabilidade

Ao discutir matriz de risco, o professor chamou atenção para a relação entre:

- **probabilidade**;
- **severidade / impacto**.

Mesmo quando a probabilidade é pequena, se o impacto for catastrófico, o risco pode continuar sendo alto. A recomendação apresentada foi ter uma postura conservadora diante de riscos de alto impacto, porque a exploração de um evento grave pode comprometer a própria continuidade da organização.

## 12. Introdução ao NIST Cybersecurity Framework 2.0

O **NIST Cybersecurity Framework 2.0** foi apresentado como um documento público com orientações para implementação de cibersegurança.

O professor ressaltou que o framework:

- não deve ser visto como algo linear;
- não é um processo engessado;
- organiza a segurança em funções, categorias e subcategorias.

As funções destacadas foram:

- **Governança**;
- **Identificação**;
- **Proteção**;
- **Detecção**;
- **Resposta**;
- **Recuperação**.

Também foi comentado que a **Governança**, que antes aparecia de forma menos explícita, passou a ganhar mais destaque na versão 2.0.

## 13. Governança

Na parte de governança, a aula destacou temas como:

- contexto organizacional;
- políticas públicas e internas;
- definição de papéis, responsabilidades e autoridades;
- supervisão;
- cadeia de suprimentos;
- gestão de risco.

Uma ideia muito forte aqui foi: **segurança não se faz sozinho**.

Mesmo com equipe técnica muito competente, a segurança depende do restante da organização. O professor reforçou várias vezes a importância do usuário e do fator humano.

## 14. Usuário como elo fraco

Foi destacada a noção de que o usuário costuma ser o elo mais fraco da segurança, não como crítica pessoal, mas porque:

- o usuário sempre pode agir de forma inesperada;
- ele pode clicar sem ler;
- pode ignorar sinais de alerta;
- pode comprometer a segurança por desconhecimento.

O professor citou o caso de uma máquina com vários malwares e relatou que o usuário havia clicado em um pop-up sem sequer ler seu conteúdo. A partir disso, reforçou a necessidade de:

- treinamento;
- conscientização;
- políticas claras e divulgadas.

Essas políticas, segundo ele, não podem ficar escondidas: precisam ser públicas para os funcionários.

## 15. LGPD dentro da governança e da resposta

A LGPD apareceu em diferentes momentos da aula.

Na parte de governança, ela foi associada principalmente à definição de:

- responsáveis;
- papéis institucionais;
- interlocução com a **ANPD**;
- responsáveis por reporte de incidentes.

Também foi citada a necessidade de definir quem será procurado em caso de incidente, inclusive em interações com entidades como o **CERT.br**.

Mais adiante, a LGPD também apareceu ligada à **proteção de dados** e à **resposta a incidentes**.

## 16. Identificação

Na função de identificação, foram mencionados:

- gerenciamento de ativos;
- avaliação de riscos;
- levantamento de melhorias;
- reconhecimento de problemas e oportunidades de fortalecimento da segurança.

A identificação não serve apenas para encontrar falhas já existentes, mas também para perceber o que pode ser melhorado antes que vire problema.

## 17. Proteção

Na função de proteção, a aula abordou:

- gerenciamento de identidade;
- autenticação;
- controle de acesso;
- conscientização e treinamento;
- segurança de dados;
- segurança de plataforma;
- resiliência da infraestrutura.

Aqui o professor voltou a enfatizar campanhas internas de phishing como forma de treinamento. Empresas podem enviar e-mails simulados propositalmente para observar quem clica e, com isso, reforçar ações educativas.

Foi mencionado também o risco do **BYOD (Bring Your Own Device)**, já que dispositivos trazidos de fora podem introduzir malware no ambiente corporativo.

## 18. Resiliência e redundância

Ainda dentro de proteção, o professor falou sobre a importância da resiliência da infraestrutura e exemplificou com redundância em firewall e data center.

Foram mencionados cenários como:

- redundância quente (**hot**);
- redundância morna (**warm**);
- redundância fria (**cold**).

A ideia central foi que a existência de redundância pode reduzir tempo de indisponibilidade, embora o tipo de redundância faça diferença no tempo de recuperação.

## 19. Detecção

Na função de detecção, apareceram temas como:

- monitoramento contínuo;
- análise de eventos adversos;
- observação de tráfego e comportamento anômalo.

Ferramentas citadas:

- SIEM;
- sistemas de agregação de logs;
- Zabbix;
- Cacti;
- Nagios.

Exemplos de sinais que podem indicar incidente:

- aumento anormal de broadcast;
- estouro de tabela MAC;
- tentativas de sniffing;
- comportamento fora do padrão.

## 20. Resposta a incidentes

Na função de resposta, o professor citou:

- comunicação de incidentes;
- análise do ocorrido;
- preservação de registros;
- levantamento de logs e evidências;
- mitigação.

Um ponto importante foi a orientação de **não sobrescrever imediatamente um ambiente comprometido**. Em vez de simplesmente restaurar backup na mesma máquina, o ideal é preservar o sistema afetado para entender:

- como ocorreu a infecção ou o acesso indevido;
- qual foi a via de entrada;
- quais registros podem servir à análise;
- quem foi o responsável ou de onde partiu o ataque.

## 21. Recuperação

Na função de recuperação, foram mencionados:

- execução do plano de recuperação;
- retomada dos serviços;
- comunicação da recuperação;
- identificação de perdas e impactos após o incidente.

## 22. Exemplo estrutural: energia, UPS e gerador

O professor relatou um caso de indisponibilidade na UFC relacionado a falha de energia e problemas no gerador do data center.

Esse exemplo foi usado para mostrar que:

- nem toda indisponibilidade nasce diretamente de falha lógica ou de software;
- falhas estruturais também impactam segurança e continuidade;
- UPS/no-break não sustenta todo tipo de carga;
- ar-condicionado exige outra estrutura de sustentação energética;
- a indisponibilidade pode surgir de infraestrutura física, não apenas digital.

## 23. Perfis de hacker: hats

Em outro momento da aula, foram apresentados os perfis chamados de **hats**:

- **White Hat**: profissional autorizado a testar e identificar falhas;
- **Black Hat**: agente malicioso, que invade para roubar, causar dano ou comprometer sistemas;
- **Grey Hat**: atua sem autorização, mas sem intenção declarada de causar dano, muitas vezes informando a falha ao dono do sistema depois.

O professor observou que, mesmo sem má intenção, o Grey Hat continua sujeito a responsabilização, porque não tinha autorização prévia para agir.

## 24. Princípios de segurança retomados com exemplos

Em nova retomada, os princípios foram explicados de forma aplicada:

- **Confidencialidade**: impedir que terceiros leiam mensagens ou dados;
- **Integridade**: impedir alteração indevida da informação;
- **Disponibilidade**: garantir acesso quando necessário;
- **Autenticidade**: garantir a identidade do emissor;
- **Não-repúdio**: impedir negação posterior da autoria.

Esses conceitos foram ligados diretamente ao uso de:

- criptografia;
- hash;
- assinatura digital.

## 25. Criptografia simétrica e assimétrica

A aula apresentou as duas abordagens principais da criptografia.

### Criptografia simétrica

Usa a **mesma chave** para cifrar e decifrar.

Foi comparada à chave física de uma porta: quem tem a chave abre e fecha.

O grande problema apontado foi a **distribuição segura da chave**. Se a chave for interceptada durante o compartilhamento, o esquema perde sua proteção.

### Criptografia assimétrica

Usa um **par de chaves**:

- uma pública;
- uma privada.

A explicação central foi:

- o que é cifrado com a pública só a privada correspondente decifra;
- o que é cifrado com a privada pode ser verificado com a pública.

Isso resolve o problema de distribuição, já que a chave pública pode ser divulgada livremente.

## 26. Assinatura digital e hash

Um ponto importante da aula foi mostrar que a criptografia assimétrica não serve apenas para confidencialidade, mas também para **autenticidade**.

O professor explicou que, quando algo é cifrado com a chave privada, a intenção não é esconder o conteúdo, mas provar autoria.

Como cifrar arquivos grandes com criptografia assimétrica é custoso, entra o uso do **hash**:

- gera-se um resumo matemático do documento;
- esse hash é cifrado com a chave privada;
- o receptor calcula o hash do documento recebido;
- depois decifra a assinatura com a chave pública;
- por fim, compara os hashes.

Se os valores coincidirem, o documento é considerado **íntegro e autêntico**.

## 27. Certificado digital

O certificado digital foi apresentado como o elemento que **vincula uma chave pública a uma identidade**.

Ele é emitido por uma **Autoridade Certificadora (AC)**, que funciona como um terceiro confiável.

Exemplo trabalhado em aula:

- ao acessar o site de um banco e ver o cadeado, o navegador verifica o certificado;
- se a assinatura vier de uma autoridade confiável, a conexão é aceita;
- se o certificado for assinado por algo desconhecido, o navegador exibe aviso de insegurança.

Também foram citados:

- **ICP-Brasil**;
- certificados **A1**;
- certificados **A3**;
- **e-CPF**;
- **e-CNPJ**.

## 28. Recomendações do NIST e certificados

No começo da aula, o professor também conectou o tema da criptografia às recomendações do NIST e comentou exemplos como:

- exigência de chaves RSA com tamanho mínimo aceito por implementações modernas;
- redução do tempo de validade de certificados;
- necessidade crescente de automatizar a gestão desses certificados.

A ideia transmitida foi que boas práticas de segurança vão pressionando as organizações a adotarem processos mais maduros e automatizados.

## 29. Ataques passivos e ativos

Ao tratar de ataques, a aula diferenciou:

### Ataques passivos

O atacante apenas observa.

Exemplo: **sniffing**, em que ele monitora o tráfego sem alterar o serviço.

A dificuldade maior nesses ataques é a detecção, pois não há necessariamente interrupção visível.

### Ataques ativos

O atacante interfere, altera, engana ou derruba.

Exemplos citados:

- **Man-in-the-Middle (MitM)**;
- **Spoofing**;
- **negação de serviço**.

## 30. Phishing e suas variações

O professor tratou o phishing como uma forma de “pescaria”, em que a vítima é induzida a clicar, fornecer senha ou seguir para um site falso.

Foram citadas diferentes modalidades:

- **Blind Phishing**: envio massivo, sem alvo específico;
- **Spear Phishing**: direcionado a uma pessoa específica;
- **Clone Phishing**;
- **Whale / Whaling**: voltado a alvos de maior hierarquia ou poder;
- **Vishing**: phishing por voz ou telefone;
- menção a uma forma ampla de ataque com objetivo de obter “colheita” em escala.

No caso do **Spear Phishing**, o professor destacou que o atacante estuda a vítima e molda a abordagem para aumentar a chance de sucesso.

## 31. Engenharia social

A engenharia social apareceu como um eixo central da aula.

Entre os pontos trabalhados:

- manipulação do comportamento humano;
- exploração do medo, pressa, confiança e desatenção;
- boatos;
- coleta de informações;
- campanhas de influência.

### Exemplo do caso Pix

Foi citado um caso envolvendo a infraestrutura do Pix, em que um funcionário teria sido abordado por engenharia social e cooptação fora do ambiente formal de trabalho.

## 32. Campanhas de influência e contexto social

Foi destacado que campanhas de influência e coleta de informações se tornam especialmente sensíveis em contextos de decisão, como períodos eleitorais, mostrando que segurança não se limita ao campo técnico, mas também envolve comportamento social e circulação de informação.

## 33. Introdução à criptografia como “escrita secreta”

Ao iniciar o bloco mais diretamente voltado à criptografia, o professor retomou a origem etimológica da palavra e apresentou a criptografia como a ciência voltada a ocultar e proteger dados contra leitura não autorizada.

O ponto central foi que a criptografia busca resolver o problema do **canal inseguro**. Um meio de comunicação, por natureza, pode ser interceptado; por isso, os dados precisam ser protegidos mesmo quando trafegam em ambientes potencialmente expostos.

## 34. Canal inseguro e meio compartilhado

A explicação foi amarrada a uma analogia simples: em um ambiente em que todos podem ouvir ou observar, o canal não é seguro por natureza.

Por isso:

- informação sensível não deve depender apenas do canal;
- a proteção precisa acompanhar o dado;
- a criptografia protege o significado da mensagem, ainda que o tráfego em si continue visível.

## 35. Criptografia x esteganografia

Foi feita uma diferenciação entre:

- **Criptografia**: torna a informação incompreensível para quem não possui a chave;
- **Esteganografia**: oculta a própria presença da informação dentro de outro meio.

Como exemplo, o professor comentou um caso de malware embutido em imagem por meio de esteganografia, associado a aplicativos de limpeza e ferramentas suspeitas em dispositivos Android mais antigos.

## 36. Software indesejado e percepção do usuário

Ao falar desses aplicativos, o professor também comentou a presença de softwares que, embora não sejam necessariamente malwares clássicos, são indesejados, invasivos ou problemáticos.

A discussão passou por:

- softwares pré-instalados;
- consumo de recursos;
- dificuldade de remoção;
- indução do usuário a escolhas ruins;
- falsa impressão de segurança ao instalar múltiplos antimalwares ao mesmo tempo.

## 37. Funcionamento básico da criptografia simétrica

A criptografia simétrica foi representada em termos de:

- texto simples;
- algoritmo de cifra;
- chave;
- texto cifrado.

Nesse modelo, a mesma chave serve para cifrar e para decifrar.

O professor destacou que, sozinha, a criptografia simétrica trabalha principalmente com **confidencialidade**. O grande problema é que, se a chave for descoberta por terceiro, o canal pode ser lido.

## 38. Limitações da criptografia simétrica

Foi dito explicitamente que a criptografia simétrica, por si só, não resolve tudo. Ela não garante automaticamente:

- autenticidade;
- não-repúdio;
- integridade em sentido amplo.

Seu foco principal é o sigilo do conteúdo, desde que a chave permaneça protegida.

## 39. Criptoanálise, frequência e o caso Enigma

Na parte final registrada, a aula entrou em noções introdutórias de criptoanálise.

O professor comentou a ideia de inferir padrões a partir de recorrência em textos e citou o caso da **máquina Enigma** em referência ao filme e à recorrência de certas expressões previsíveis na comunicação. A explicação foi usada para mostrar como repetições e padrões podem ajudar na quebra de cifras, dependendo do contexto.

## 40. Tipos de ataque à cifra

Foram mencionados alguns cenários clássicos de ataque:

- ataque baseado apenas no **texto cifrado**;
- ataque com **texto claro conhecido**;
- ataque com **texto claro escolhido**.

A explicação buscou mostrar que o algoritmo pode ser público e, ainda assim, a segurança continuar válida. O objetivo do atacante passa a ser descobrir a chave ou um método de recuperação do texto, e não simplesmente “esconder” o algoritmo.

## 41. Segurança por obscuridade

A aula encerra esse trecho criticando a ideia de que segurança depende de esconder o funcionamento interno do mecanismo.

No caso da criptografia, o professor destacou que o ideal é que:

- o algoritmo seja público;
- a segurança esteja concentrada na chave;
- o sistema continue seguro mesmo que se conheça a técnica utilizada.

---

## Fechamento da anotação

Nesta aula, o professor construiu uma base conceitual ampla para a disciplina, conectando:

- fundamentos da Segurança da Informação;
- diferenças entre segurança da informação e cibersegurança;
- tríade CID;
- risco, ativos e governança;
- NIST CSF 2.0;
- LGPD e responsabilidades institucionais;
- fator humano e engenharia social;
- princípios de criptografia, assinatura digital e certificado digital;
- ataques comuns em redes e em comunicação.

A gravação disponível termina ainda dentro do bloco de criptografia e criptoanálise, então esta anotação cobre apenas o conteúdo efetivamente registrado no material enviado.
