# Segurança em Nuvem — Aula 1


## 1. Visão geral, objetivos e metodologia

 Antigamente, seu uso era mais associado aos profissionais de TI, mas atualmente praticamente qualquer pessoa utiliza serviços em nuvem, muitas vezes sem perceber. Esse uso se massificou e, à medida que aplicações e dados são colocados na nuvem, surgem diferentes tipos de ameaças e problemas de segurança. É justamente nesse contexto que a disciplina se insere.

O professor explicou que seriam discutidas algumas dessas ameaças, boas práticas e os modelos de responsabilidade. A turma não seria tratada apenas como usuária de serviços em nuvem, mas também como desenvolvedora e como responsável pela gestão da infraestrutura. Em outras palavras, os alunos assumiriam, ao longo do curso, tanto o papel de clientes da nuvem quanto o papel de quem administra esse ambiente, em uma perspectiva próxima à de DevOps. Ele destacou que diversos problemas podem surgir quando uma configuração é feita de maneira incorreta, deixando vulnerabilidades, portas ou outros recursos expostos. Também observou que muitos riscos conhecidos em redes físicas continuam existindo em redes virtuais, de modo que conhecimentos prévios de segurança de redes permanecem aplicáveis ao ambiente de nuvem.

O professor explicou que a proposta era compreender quais são esses riscos e vulnerabilidades e, ao mesmo tempo, aprender na prática como configurar e utilizar um ambiente de nuvem. Esse seria o foco dos quatro dias da disciplina. O objetivo era que, ao final, os alunos compreendessem os principais conceitos relacionados à segurança de sistemas e dados em nuvem. Também seriam apresentados conceitos básicos de computação em nuvem para garantir que todos partissem de um nível de conhecimento semelhante. Como alguns alunos já tinham tido contato com AWS, isso facilitaria parte das atividades, mas ainda seria necessário nivelar a turma.

O professor explicou que, depois dos conceitos introdutórios — o que é nuvem, por que ela surgiu e como funciona —, o curso passaria a analisar, no contexto da Amazon Web Services, que seria utilizada como estudo de caso, como proteger a infraestrutura e as aplicações para reduzir problemas, ameaças e possibilidades de ataque. A ementa começaria com uma introdução à computação em nuvem e à própria AWS. Primeiro, a computação em nuvem seria tratada de forma teórica; depois, o foco passaria para os serviços da Amazon e para a segurança desses serviços em níveis de plataforma e infraestrutura, incluindo a divisão de responsabilidades. Uma das perguntas centrais seria: quando uma organização contrata um serviço em nuvem, quem é responsável por cada parte da segurança?

O professor explicou que seria necessário entender o que é responsabilidade do desenvolvedor ou gestor da infraestrutura e o que é responsabilidade do provedor. A disciplina incluiria análise de riscos, conceitos mencionados no plano como AA e BAC, principais ameaças à nuvem, tipos de ataque e formas de prevenção. As aulas seriam majoritariamente expositivas, apoiadas em slides, mas o professor pretendia passar por essa parte de forma relativamente rápida para priorizar as aulas práticas. A turma teria acesso a um ambiente Amazon/AWS no qual poderia criar máquinas, criar serviços, configurar recursos e realizar acessos.

O professor explicou que a parte prática teria bastante peso. Haveria atividades dentro do próprio ambiente da Amazon, incluindo quizzes de perguntas e respostas e exercícios práticos. Entre os exemplos, ele citou configurar uma máquina de acordo com determinados requisitos e configurar um bucket de modo que certos grupos não pudessem acessar determinados arquivos. Essas atividades serviriam para colocar em prática os conceitos estudados ao longo da disciplina.

### 1.1. Bibliografia e materiais de apoio

O professor explicou que qualquer bibliografia escolhida para computação em nuvem tende a ficar desatualizada rapidamente. Um livro pode ser atual durante um ou dois anos, mas, como os serviços e as plataformas mudam com muita velocidade, perde atualidade com o tempo. Mesmo assim, livros de introdução à computação em nuvem continuam úteis para a parte conceitual. Ele comentou que procurou concentrar bastante informação nos slides para diminuir a dependência de livros e que, atualmente, ferramentas de IA generativa também podem ajudar a esclarecer rapidamente conceitos específicos que não tenham ficado claros.

O professor explicou ainda que trouxe livros voltados à área de segurança e materiais específicos para certificações. Alguns desses livros têm pouco conteúdo teórico e muitos exercícios, servindo principalmente para apresentar exemplos de perguntas e de questões semelhantes às cobradas em provas de certificação. Outros são mais teóricos. Também existem materiais específicos para certificações da Amazon.

O professor comentou que um dos livros apresentados era mais recente, de 2020, e incluía alguns desafios e temas de pesquisa. Por isso, poderia ser interessante para quem futuramente pretendesse seguir para mestrado ou doutorado. Como parte da turma ainda não tinha experiência prática com nuvem, ele reforçou que seria melhor começar pelos conceitos fundamentais para nivelar o conhecimento de todos. A partir daí, a discussão inicial seria justamente por que a computação em nuvem existe, por que se estuda segurança em nuvem e qual é a importância desse tema.

## 2. Motivação e evolução da computação em nuvem

O professor explicou que a história da computação pode ser vista como um movimento recorrente entre centralização e distribuição. Segundo ele, em um primeiro momento predominavam os mainframes, com o processamento concentrado. Depois, o computador pessoal passou a ser valorizado justamente por distribuir o poder computacional. Com o tempo, porém, os computadores pessoais deixaram de ser suficientes para determinadas demandas, o que levou novamente à busca por máquinas muito mais poderosas, como os supercomputadores.

O professor destacou que os supercomputadores são poderosos, mas caros e inacessíveis para muitas pessoas. Isso impulsionou novamente a distribuição do poder computacional, com notebooks mais robustos e dispositivos móveis. Posteriormente, o movimento voltou para a centralização com a computação em nuvem, na qual os recursos retornam aos data centers, embora não mais concentrados em uma única máquina, mas em grandes conjuntos de equipamentos e racks. Depois disso, voltou a surgir a discussão sobre computação de borda, justamente porque enviar todo o processamento para a nuvem também pode ser caro.

O professor observou que esse movimento continua atualmente. Com o uso intensivo de GPUs em data centers, muita carga voltou a ser processada de forma centralizada, especialmente em aplicações relacionadas a LLMs. Ao mesmo tempo, como esses modelos são caros e consomem muitos recursos — inclusive energia e água —, já existem esforços para criar LLMs mais otimizadas que possam executar na borda. Ele comentou que já começam a surgir equipamentos pequenos capazes de executar esse tipo de processamento localmente, inclusive em casa. Para o professor, essa alternância entre centralizar e distribuir é uma característica recorrente da evolução da computação.

O professor explicou que uma das motivações centrais da computação em nuvem foi a redução de custos. Manter infraestrutura em vários locais é caro, pois envolve profissionais para gerenciá-la, manutenção, energia, licenças de software e outros gastos. A nuvem surgiu como uma alternativa para reduzir parte desses custos e transferir várias responsabilidades operacionais para o provedor.

O professor explicou que a computação em nuvem também permitiu que empresas pequenas e grandes reduzissem o custo de capital, chamado por ele de CAPEX, e passassem a trabalhar mais com custo operacional, ou OPEX. Em vez de comprar antecipadamente uma grande infraestrutura, uma empresa com poucos clientes pode começar com uma máquina pequena e gastar pouco. Conforme o número de clientes aumenta, novos recursos podem ser contratados. Dessa forma, a empresa evita investir em infraestrutura de que ainda não precisa.

O professor ressaltou que essa lógica foi uma das motivações para a adoção da nuvem. Empresas e desenvolvedores perceberam que poderiam aumentar os recursos gradualmente, conforme a necessidade. Ele também comentou que, ao longo dos anos, a própria ideia de “nuvem” passou a aparecer fora do ambiente técnico e se tornou familiar até para pessoas leigas. Serviços como iCloud e produtos da Google com a palavra *cloud* contribuíram para essa massificação do conceito.

O professor apresentou como exemplo um episódio relacionado ao Enem de 2019, quando milhões de pessoas tentaram acessar resultados e o SISU praticamente ao mesmo tempo. Segundo ele, cerca de quatro milhões de pessoas fizeram o exame e muitas tentaram acessar o sistema simultaneamente. A infraestrutura não estava preparada para lidar com aquela demanda. Esse tipo de situação ilustra um dos desafios da computação em nuvem: administrar o ambiente de forma que ele consiga escalar quando ocorre um crescimento muito grande e repentino no número de acessos.

O professor comentou que notícias sobre sistemas que param ou não suportam picos de demanda continuam aparecendo. Ele citou como exemplo a loteria e a Mega da Virada, cujo sorteio não teria ocorrido no horário tradicional do dia 31 porque o sistema não conseguiu suportar a quantidade de pessoas tentando apostar. O professor relatou que também tentou acessar o serviço naquele dia, entrou em uma fila estimada em várias horas e enfrentou travamentos, erros e necessidade de recarregar a página, retornando novamente à fila. Muitas pessoas não conseguiram jogar e os sistemas da Caixa ficaram sobrecarregados, fazendo com que o sorteio fosse realizado somente no dia seguinte, por volta das 10 ou 11 horas.

A partir desse exemplo, o professor observou que, mesmo em 2025/2026, ainda existem problemas de capacidade que vêm sendo discutidos há muitos anos. Ele acrescentou, porém, que nem sempre a questão é simplesmente falta de preparo técnico: em alguns casos, pode não compensar financeiramente dimensionar uma infraestrutura para suportar um pico extremo que acontece apenas por algumas horas ao ano.

O professor relacionou essa discussão a outro exemplo, o de *Game of Thrones*. Durante a exibição de episódios muito aguardados, quem queria assistir ao vivo precisava, em alguns momentos, entrar em uma fila de acesso. Segundo ele, sem essa fila, o serviço poderia cair para todos, em uma situação semelhante ao efeito de um ataque de negação de serviço, com muitas pessoas tentando acessar ao mesmo tempo. Ele relatou que costumava entrar no serviço algumas horas antes, assistir a episódios anteriores enquanto aguardava e, ainda assim, às vezes enfrentava travamentos justamente quando o episódio principal começava.

O professor explicou que a empresa provavelmente tinha recursos financeiros para construir uma infraestrutura capaz de suportar aquele pico, mas a pergunta arquitetural relevante era se valeria a pena investir tanto para atender a aproximadamente duas horas de demanda extrema e, durante o restante da semana, manter grande parte dessa infraestrutura ociosa. Para um arquiteto, desenvolvedor ou gestor de solução em nuvem, essa é uma decisão importante: nem sempre é necessário disponibilizar todo o poder computacional possível. Em certos casos, mecanismos como filas e mensagens informando ao usuário para tentar novamente mais tarde podem ser uma solução mais adequada do ponto de vista econômico.

O professor explicou que esse é um dos debates importantes em computação em nuvem e apresentou alguns casos clássicos. Um deles foi o da empresa Animoto, mencionada na aula como uma empresa que teve bastante sucesso por volta de 2008 e 2009. Ela foi uma das primeiras plataformas a permitir que usuários enviassem vídeos e imagens e recebessem como resultado uma apresentação ou vídeo mais elaborado, com música e transições. Hoje esse tipo de funcionalidade é comum em diversas plataformas, mas, naquele período, era algo relativamente novo.

O professor explicou que a Animoto começou com aproximadamente 5.000 usuários e, depois de uma campanha no Facebook, chegou rapidamente a cerca de 150.000 novos usuários em três dias. Para uma startup, esse crescimento era excelente, mas poderia ter sido fatal caso a solução não tivesse sido desenvolvida, desde o início, com princípios de computação em nuvem e capacidade de escalar. Se a infraestrutura tivesse travado no primeiro dia, ninguém conseguiria usar o serviço e a própria startup poderia ter fracassado. Ele relacionou esse cenário ao impacto que uma publicação feita por pessoas com milhões de seguidores, como Neymar ou Vinícius Júnior, poderia ter sobre um sistema: uma simples postagem pode gerar milhões de acessos em poucos minutos.

O professor ressaltou que, se a infraestrutura não estiver preparada para crescer, esse pico se torna um problema. No modelo atual, não é aceitável dizer que será necessário comprar uma máquina e esperar meses até que ela seja entregue, instalada e configurada para então aumentar a capacidade. Esse modelo fazia sentido para provedores de décadas atrás, mas as aplicações atuais precisam conseguir crescer rapidamente.

Outro caso apresentado pelo professor foi o do *New York Times*. O jornal tinha a intenção de digitalizar todo o seu arquivo histórico, originalmente em papel. As páginas tinham sido escaneadas e o objetivo era transformar aquelas imagens em texto e em páginas pesquisáveis, utilizando OCR para converter o conteúdo das imagens e organizar as matérias, imagens e edições por data.

O professor explicou que os engenheiros do *New York Times* inicialmente pensaram em comprar uma máquina poderosa para executar o processamento. Essa máquina poderia levar três ou quatro semanas para chegar e, depois, seriam necessários dois ou três meses de processamento. Ao avaliar a situação, surgiu outra questão: depois que a digitalização fosse concluída, o que seria feito com aquela máquina? Talvez aquele nível de poder computacional nunca mais fosse necessário.

Diante disso, segundo o professor, a equipe decidiu testar a nuvem. Em vez de comprar o equipamento, utilizou recursos da Amazon e conseguiu instanciar várias máquinas em paralelo, alcançando um poder computacional muito superior ao da máquina que seria comprada. Em aproximadamente dois ou três dias, foi possível processar aquilo que poderia levar semanas ou meses. Além de concluir o trabalho muito mais rapidamente, o custo foi menor do que o da compra do equipamento. O professor destacou esse caso como um exemplo clássico das vantagens da computação em nuvem.

O professor explicou que essas situações ilustram as principais motivações para o uso de nuvem. Muitas empresas precisam disponibilizar serviços pela Internet, mas ainda não sabem exatamente qual capacidade computacional será necessária. A nuvem permite começar com uma determinada capacidade e escalar conforme a demanda. Mais adiante na disciplina seriam apresentadas as características que permitem essa escalabilidade automática.

O professor acrescentou que o superprovisionamento é caro. Quando uma organização compra equipamentos próprios, precisa considerar energia, ar-condicionado, manutenção, equipe de TI, armazenamento e backup. Como a compra e a entrega de novos equipamentos levam tempo, normalmente não se compra um servidor que atenda apenas à demanda da semana seguinte; é preciso superdimensionar para que ele suporte vários meses de crescimento. Como consequência, no início o equipamento pode operar com apenas 5% ou 10% de utilização de CPU e permanecer subutilizado até chegar a patamares como 70% ou 80%, quando a organização começa a providenciar outro servidor.

O professor ressaltou que manter equipamentos *on-premises* é caro e que as empresas aprenderam isso na prática. Implantação, armazenamento e backup custam tempo, equipe, recursos computacionais, energia, água e outros recursos necessários à operação. Ele também relembrou períodos de crise em que as organizações buscavam reduzir gastos com TI e destacou que a nuvem surgiu como uma alternativa para essa redução.

Nesse contexto, o professor voltou à distinção entre CAPEX e OPEX. Em vez de gastar, por exemplo, R$ 300.000 para montar uma infraestrutura, uma organização pode preferir pagar mensalmente R$ 3.000, R$ 4.000 ou R$ 5.000 pelo uso da nuvem. Esse custo passa a fazer parte da operação cotidiana. Além disso, o custo pode ser associado aos próprios clientes: conforme novos clientes aparecem, a empresa aumenta os recursos; quando a demanda diminui, reduz a quantidade de recursos alocados. O professor destacou que essa dinamicidade — aumentar e diminuir recursos de acordo com a demanda — é uma das grandes vantagens da nuvem.

### 2.1. Computação utilitária

O professor explicou que um conceito importante para compreender a nuvem é o de **computação utilitária**. Segundo ele, em 1961, John Macart foi convidado para uma palestra no MIT e questionado sobre como imaginava a computação do futuro. A resposta apresentada naquela ocasião praticamente definiu o conceito de *utility computing*: um dia, a computação seria semelhante a água, energia e telecomunicações.

O professor explicou que, nessa visão, o usuário pagaria com base no uso, tratando a computação como um bem ou serviço de utilidade que qualquer pessoa poderia contratar quando precisasse. Esse conceito ficou conhecido como computação utilitária. Para o professor, o cenário atual se aproxima muito dessa previsão, pois hoje qualquer pessoa consegue utilizar serviços em nuvem e, muitas vezes, já os utiliza no cotidiano sem pensar neles dessa forma.

Como exemplos, o professor citou serviços de entretenimento e armazenamento. Quando alguém quer assistir a um filme, pode contratar Netflix, Prime, YouTube Premium ou outro serviço. Para ouvir música, pode usar Spotify, outro serviço de streaming ou o próprio YouTube Premium. Se o armazenamento do celular não for suficiente para fotos, pode contratar iCloud, Google Photos ou outro serviço de armazenamento de dados. A ideia central é que algo que antes seria tratado como um bem comprado passa a ser consumido como serviço.

O professor destacou que esse modelo também permite cancelar e trocar de fornecedor. Se uma pessoa se cansar da Netflix ou não encontrar mais conteúdo de interesse, pode cancelar e contratar outro serviço, como HBO, Paramount ou Disney. Ele comentou, de forma bem-humorada, que o problema atual é justamente a quantidade de serviços diferentes: uma pessoa da casa pode querer Disney, outra Netflix, enquanto Amazon Prime acaba sendo mantido também por estar associado às compras na Amazon. Assim, os usuários acabam acumulando diversos serviços.

O professor observou que esse cenário já havia sido imaginado décadas atrás e que é interessante perceber como a computação chegou ao modelo previsto por John Macart. Ele retomou a evolução dos mainframes, dos computadores pessoais e, depois, dos dispositivos móveis.

Para ilustrar a transformação provocada pelos dispositivos móveis, o professor mencionou imagens de dois momentos de eleição de um novo papa, em 2005 e 2013. Segundo ele, a diferença entre as imagens mostra a passagem de um cenário em que as pessoas observavam diretamente o acontecimento para outro em que grande parte do público utilizava celulares para registrar tudo. Ele comentou que a mesma situação pode ser vista atualmente em carnaval, shows e outros eventos, nos quais muitas pessoas permanecem com o celular em mãos gravando o que acontece.

O professor ressaltou que os dispositivos móveis atuais colocam grande poder computacional nas mãos das pessoas. Um celular que é carregado para todos os lugares pode ser mais potente do que computadores que eram considerados avançados até pouco tempo atrás. Para ele, o cenário atual combina a era dos dispositivos móveis com a era dos serviços e dos *web services*, formando um novo paradigma.

O professor explicou que, desde aproximadamente 2008, algumas tendências se tornaram muito importantes, entre elas a escalabilidade da Web. Atualmente existem serviços web em praticamente todos os lugares, e grande parte do que é desenvolvido disponibiliza uma API ou alguma interface que permita a comunicação entre sistemas. A computação paralela e distribuída também ganhou importância, permitindo processar informações em diferentes locais.

O professor relacionou esse cenário ao momento atual dos agentes de IA. Esses agentes podem executar parte de suas tarefas na nuvem, na máquina local ou até mesmo no celular. Ele comentou que os slides provavelmente precisariam ser atualizados para incluir essa “era dos agentes”, já que o assunto passou a aparecer em praticamente todas as discussões. Esses agentes dependem de interatividade e da capacidade de consumir APIs e outros serviços; sem essa integração, sua atuação fica limitada.

O professor também destacou a mobilidade. Atualmente, um usuário pode sair de uma rede Wi-Fi, entrar em outra rede em outro prédio e continuar conectado; pode alternar entre Wi-Fi, Bluetooth, 4G e 5G enquanto se desloca. Os serviços precisam acompanhar esse comportamento e atender usuários em movimento. Como exemplos de aplicações ligadas a essa mobilidade, ele citou patinetes elétricos que operam apenas em determinadas regiões e serviços como Uber.

Segundo o professor, o ambiente atual é resultado da combinação de diversos elementos: a nuvem fornecendo uma base de serviços; os dispositivos móveis oferecendo mobilidade e poder computacional próximo ao usuário; as redes oferecendo conectividade por Wi-Fi, Bluetooth, 4G e 5G; e os *web services* funcionando como uma das principais tecnologias de comunicação entre processos. Para ele, grande parte da inovação atual surge justamente dessa integração.

O professor explicou que esse cenário também se relaciona à expansão dos grandes data centers. Ele observou que o Ceará vive um momento particularmente favorável à instalação dessas estruturas porque Fortaleza possui uma localização estratégica em relação aos cabos de fibra óptica. Há cabos conectando a cidade à África, ao restante do Brasil, à América do Sul e aos Estados Unidos, o que torna a região atraente para novos data centers.

O professor citou, entre os exemplos, o data center do TikTok e uma visita realizada à Angola Cables, na Praia do Futuro. Segundo ele, havia no local áreas inteiras reservadas para grandes empresas, incluindo espaços associados a Amazon, Microsoft, Facebook/Meta e outros clientes. Mesmo alguns espaços aparentemente vazios já estavam alugados, porque as empresas reservavam capacidade para uso futuro. Em alguns casos, instalavam apenas um ou dois racks e mantinham o restante da área reservado. Essa demanda estaria levando a Angola Cables a expandir sua estrutura, e o professor destacou que cada vez mais data centers vêm sendo instalados na região.

O professor explicou ainda que algumas tecnologias essenciais para esse ecossistema atingiram alto grau de maturidade. Arquiteturas orientadas a serviços e tecnologias web estão consolidadas, e muitos profissionais já tiveram contato com aplicações web, inclusive pequenas aplicações em Python. As redes também estão muito mais disseminadas: 4G e 5G funcionam bem principalmente nas grandes cidades, enquanto cidades menores ainda podem contar com 4G ou 3G.

O professor observou que as rodovias continuam sendo um ponto de dificuldade para conectividade em algumas regiões, mas tecnologias como Starlink e outras soluções de comunicação via satélite estão mudando esse cenário. Mesmo em locais bastante isolados, um modem de comunicação por satélite pode garantir acesso à Internet. Isso tem permitido novas aplicações e ampliado as possibilidades de inovação.

Como exemplo, o professor mencionou o setor agropecuário, que antes precisava realizar grandes esforços para levar fibra óptica a determinadas áreas. Com soluções via satélite, torna-se possível cobrir boa parte de uma fazenda utilizando alguns pontos de conectividade e, a partir deles, integrar redes de sensores sem fio para atender às necessidades locais.

Por fim, o professor explicou que as tecnologias de virtualização foram grandes habilitadoras da computação em nuvem. Ele mencionou o Docker como exemplo e observou que atualmente é difícil desenvolver determinados tipos de projeto sem recorrer a virtualização ou contêineres. Antigamente, instalar diferentes pacotes na mesma máquina podia gerar conflitos de dependências, especialmente em Python: um pacote exigia uma versão de determinada dependência e outro pacote exigia uma versão diferente, fazendo o ambiente “quebrar”.

O professor explicou que, com o tempo, surgiram os ambientes virtuais do Python e, posteriormente, os contêineres passaram a oferecer uma alternativa ainda mais prática. Ele relatou que, em seus próprios projetos, costuma utilizar Docker. Em vez de manter uma grande quantidade de ambientes virtuais separados, pode definir em um Dockerfile a versão do Python e todas as dependências necessárias. Esse arquivo pode ser armazenado junto ao projeto ou colocado no GitHub e, em qualquer lugar, permite reconstruir praticamente o mesmo ambiente. Segundo o professor, essa facilidade também contribuiu para a adoção da computação em nuvem.

## 3. Definições e fundamentos de computação em nuvem

O professor explicou que, depois de discutir as motivações para o uso da nuvem, era necessário estabelecer uma definição formal de computação em nuvem. Para isso, apresentou a definição do NIST e destacou alguns elementos centrais. Segundo essa definição, computação em nuvem é um modelo computacional que permite acesso sob demanda, por meio de uma rede, a um conjunto de recursos computacionais.

O professor explicou que essa rede pode ser a Internet ou outra rede, inclusive uma rede privada. O acesso sob demanda pode envolver diferentes tipos de recursos computacionais: redes, servidores virtuais, ambientes de computação com ou sem GPU, ambientes físicos, recursos locais ou remotos, armazenamento, aplicações e serviços. Em outras palavras, o recurso disponibilizado pela nuvem pode assumir diferentes formas.

Outro ponto enfatizado pelo professor foi o provisionamento rápido. Ele explicou que, em um ambiente de nuvem, a solicitação de um servidor não deve depender de esperar dias para que alguém o disponibilize. O esperado é que o recurso seja criado em poucos minutos e fique pronto para utilização. Assim, três aspectos importantes da definição apresentada pelo NIST são: o acesso precisa ser **sob demanda**, precisa ocorrer **por meio da rede** e os recursos precisam poder ser **rapidamente provisionados**.

O professor apresentou também outra definição, baseada em um artigo de 2008 mencionado por ele como o artigo de Vaquero. Naquele período, existiam muitas definições diferentes de computação em nuvem. O objetivo do trabalho foi justamente reunir o que diferentes pesquisadores vinham dizendo sobre o tema, analisar os termos mais utilizados e, ao final, propor uma definição construída a partir das várias definições existentes.

O professor explicou que esse artigo se tornou muito citado e pode ser encontrado facilmente no Google Acadêmico, com milhares de citações. Segundo ele, o trabalho inteiro se dedica a discutir o que é computação em nuvem, quais conceitos apareciam nas definições existentes e como esses conceitos poderiam ser reunidos em uma definição mais abrangente.

O professor apresentou a ideia de que a nuvem funciona como um grande repositório de recursos virtualizados, facilmente utilizáveis e acessíveis. Esses recursos podem incluir hardware, plataformas e serviços. Um ponto importante é que eles podem ser dinamicamente reconfigurados para se ajustar à carga do sistema. Assim, se a utilização aumenta, os recursos podem ser ampliados; se a utilização diminui, eles podem ser reduzidos, evitando que o usuário pague desnecessariamente por capacidade ociosa.

O professor explicou que essa capacidade de ajuste também está relacionada ao uso ótimo de recursos. Ele comentou que muitos trabalhos acadêmicos em computação em nuvem se dedicaram justamente a problemas de otimização: diante de determinada demanda, seria melhor utilizar duas, três ou quatro máquinas? Parte significativa das pesquisas buscava descobrir a quantidade ideal de recursos para atender uma aplicação com determinada carga.

O professor explicou ainda que esse conjunto de recursos costuma ser explorado por meio de um modelo de pagamento baseado no uso. Ao mesmo tempo, as garantias fornecidas pelo provedor de infraestrutura são formalizadas por meio de um SLA, ou acordo de nível de serviço. Para ele, essa parte amplia a definição do NIST porque introduz explicitamente a ideia de responsabilidades e garantias associadas ao provedor.

O professor destacou que provedores de nuvem têm responsabilidades contratuais e precisam manter os serviços disponíveis dentro dos níveis acordados. Mesmo que os usuários normalmente não leiam os contratos, existe um acordo de nível de serviço por trás da contratação. Ele citou como exemplo a Amazon e mencionou uma disponibilidade de 99,97% do tempo. Isso implica que, caso o serviço fique indisponível, deve retornar dentro de um intervalo compatível com a garantia oferecida.

O professor explicou que esse tipo de SLA não existe apenas em nuvem. Praticamente todo serviço de TI ou telecomunicações possui algum acordo desse tipo. Um plano de Internet móvel, por exemplo, pode estabelecer valores mínimos de velocidade de upload e download, quantidade de dados disponíveis e informações sobre cobertura. Em geral, essas condições aparecem no contrato, mesmo que o cliente não as leia em detalhes.

O professor também utilizou serviços de entretenimento como exemplo. Segundo ele, se um serviço contratado ficar indisponível por um período suficientemente grande e violar o SLA, o cliente pode ter direito a compensação, normalmente na forma de desconto. O mesmo vale para uma conexão residencial de Internet. Ficar um dia inteiro sem serviço representa aproximadamente entre 3% e 4% de um mês, o que já pode ser suficiente para violar um acordo de disponibilidade de 99%.

O professor observou que, embora os clientes possam ter direito a desconto, muitas pessoas não solicitam a compensação porque precisariam entrar em contato com a empresa, informar quando a falha começou e terminou e gastar tempo para receber um valor relativamente pequeno, como R$ 3, R$ 4 ou R$ 5. Na opinião dele, esse tipo de compensação deveria ser automático, já que a própria empresa sabe quando o serviço ficou indisponível. Ainda assim, as empresas acabam se beneficiando do fato de que a maioria dos usuários não reclama.

Por fim, o professor explicou que a computação em nuvem já foi um tema mais frequente em notícias e discussões como uma novidade tecnológica, mas atualmente se tornou tão comum que deixou de chamar a mesma atenção. Justamente por isso, considerou importante que a turma compreendesse de forma clara o que é a nuvem e como ela funciona antes de avançar para as questões específicas de segurança.

## 4. Segurança em nuvem: responsabilidade, riscos e ameaças

O professor explicou que, embora aquela fosse uma aula introdutória, alguns temas de segurança já poderiam ser antecipados. Um deles é o **modelo de responsabilidade compartilhada**. Como já havia sido discutido, todo serviço de TI envolve algum SLA e um conjunto de métricas, garantias e regras que devem ser respeitadas pelo provedor. Nas aulas seguintes, a turma analisaria com mais profundidade a divisão de responsabilidades entre o cliente e a Amazon. O professor ressaltou que essa divisão muda de acordo com o serviço utilizado.

Como exemplo, o professor apresentou o **Amazon RDS (Relational Database Service)**. Ao contratar esse serviço de banco de dados, parte da segurança passa a ser responsabilidade da Amazon. O provedor fica responsável, por exemplo, por determinadas atualizações e correções de vulnerabilidades. Se surgir um bug crítico ou uma vulnerabilidade na versão do banco oferecida pelo serviço, cabe à Amazon executar a atualização correspondente.

O professor explicou que a disponibilidade do serviço também pode ficar sob responsabilidade da Amazon. Se o banco de dados cair, o provedor precisa restabelecê-lo dentro das condições do serviço contratado. Da mesma forma, dependendo das configurações escolhidas pelo cliente — como a habilitação de mecanismos de escalabilidade —, a Amazon pode assumir a responsabilidade de aumentar os recursos quando o banco deixar de atender à quantidade de requisições recebidas.

Ao mesmo tempo, o professor destacou que outras responsabilidades continuam sendo do cliente. Uma senha administrativa fraca, por exemplo, é uma escolha do usuário, embora a própria Amazon possa impor requisitos mínimos de complexidade. Outro exemplo seria deixar a porta do banco de dados exposta para toda a Internet. Ele mencionou a porta 3306, associada ao MySQL, e explicou que, em vez de permitir que apenas as máquinas internas necessárias acessem o banco, um administrador pode, por erro de configuração, liberar a porta para o mundo inteiro. Nesse caso, a responsabilidade é do cliente. A disciplina aprofundaria justamente esses mecanismos e a divisão de responsabilidades.

O professor explicou que também seriam retomados os pilares clássicos da segurança — **confidencialidade, integridade e disponibilidade** —, agora aplicados especificamente ao contexto da nuvem. A partir desses pilares, seriam discutidas as principais ameaças encontradas em ambientes de nuvem e as formas de evitá-las.

O professor destacou que o principal tipo de problema é a **configuração incorreta**. Um serviço pode ser criado e deixado exposto, uma porta pode ficar aberta para a Internet, uma senha pode ser fraca ou uma regra de roteamento pode ser configurada de forma errada. Segundo ele, esse é um risco particularmente importante porque administradores de ambientes em nuvem costumam ter grande poder de configuração; portanto, um erro cometido com privilégios elevados pode causar consequências significativas.

Outro problema apresentado pelo professor foi o **gerenciamento inadequado de identidades**. Isso pode ocorrer quando senhas fracas são permitidas, quando não se utiliza autenticação de dois fatores em situações adequadas ou quando usuários recebem privilégios superiores aos necessários. Ele retomou o princípio de segurança segundo o qual cada pessoa deve receber apenas os menores privilégios necessários para desempenhar sua função.

O professor explicou que não se deve conceder perfil de administrador apenas para facilitar uma tarefa. É necessário verificar se aquele usuário realmente precisa ser administrador, se um perfil de moderador seria suficiente ou se seria melhor criar um novo perfil com permissões específicas. Ele observou que, muitas vezes, para evitar o trabalho de configurar corretamente as permissões, alguém acaba liberando acessos além do necessário. Os provedores de nuvem, incluindo a Amazon, oferecem mecanismos específicos para gerenciar esse tipo de controle.

O professor identificou **APIs inseguras** como outro risco relevante. A nuvem é amplamente controlada por APIs. Embora muitas atividades da disciplina fossem realizadas por meio do console gráfico — clicando e configurando opções na interface —, praticamente tudo também pode ser realizado programaticamente. Como exemplo, ele citou a biblioteca **Boto 3** para Python, com a qual é possível criar e encerrar máquinas, modificar configurações de firewall, configurar e remover chaves, criar e desligar bancos de dados, migrar bancos e executar diversas outras ações sobre os recursos da AWS.

O professor explicou que, para profissionais acostumados a trabalhar por linha de comando, essa capacidade é bastante útil. Também citou a **AWS CLI**, que permite executar operações por comandos e que, internamente, também se apoia nas APIs da plataforma. Por esse motivo, a proteção das interfaces programáticas, das credenciais e dos tokens é essencial.

O professor apresentou, então, um exemplo de erro relacionado a credenciais. Esquecer uma máquina ligada pode gerar uma conta inesperada, mas ele relatou um caso mais grave ocorrido com um aluno que, na época, estava no mestrado. Esse aluno teria colocado no próprio código uma chave e tokens de acesso e enviado o conteúdo para o GitHub. Como existem bots que varrem repositórios constantemente em busca de credenciais válidas, a chave foi descoberta e utilizada por terceiros.

Segundo o professor, a partir daquela credencial foram criadas muitas máquinas, utilizadas para atividades como ataques, operação de bots e mineração de Bitcoin. Na época, ataques desse tipo eram bastante comuns. O incidente gerou uma cobrança da ordem de milhares de dólares, e o professor mencionou algo em torno de 3.000. Ele usou o caso para reforçar que **gestão de tokens e gestão de credenciais** são aspectos críticos da segurança em nuvem.

O professor também destacou as **ameaças internas**. Assim como em qualquer organização, uma empresa está sujeita a espionagem industrial, espionagem comercial e outros riscos associados a pessoas que já possuem algum nível de acesso. Por isso, reforçou novamente que privilégios devem ser concedidos apenas a quem realmente precisa deles e que acessos, chaves e tokens devem ser cuidadosamente controlados.

O professor explicou que, durante as atividades práticas, esses problemas seriam retomados no contexto dos serviços concretos. Ao criar um determinado serviço, a turma deveria observar onde uma senha é configurada, onde uma porta é alterada, como limitar o acesso somente às máquinas de um ambiente específico e quais configurações podem gerar exposição desnecessária.

Como exemplo, o professor comentou que um banco de dados normalmente não deveria ficar diretamente acessível fora da rede da aplicação. Em uma arquitetura típica, quem acessa o banco é o *back-end*. O *back-end* pode precisar disponibilizar uma porta para comunicação externa, mas isso não significa que a porta do próprio banco de dados deva ser exposta. A ideia seria aprender a subir os serviços de maneira restritiva, liberando somente as portas e os acessos realmente necessários.

### 4.1. Soberania de dados, LGPD e proteção de informações

O professor explicou que outro tema importante para a disciplina é a **soberania de dados**. Existem legislações no Brasil, na Europa e nos Estados Unidos que impõem requisitos específicos sobre onde determinados dados podem ser armazenados e como devem ser protegidos. Esses requisitos podem ser especialmente relevantes para empresas de contabilidade, organizações da área jurídica e órgãos públicos, como Receita Federal, fazenda pública, SUS e outros.

O professor destacou que determinadas legislações nacionais podem impedir que certos dados sejam armazenados fora do Brasil. Assim, quando uma empresa presta serviço para um órgão sujeito a esse tipo de requisito e utiliza computação em nuvem, precisa garantir que os dados permaneçam fisicamente ou logicamente em território brasileiro, conforme a exigência aplicável. Em alguns casos, além da localização, também pode haver requisitos sobre armazenamento criptografado e nível de criptografia.

O professor explicou que existem diversos padrões e certificações internacionais relacionados a esse tipo de proteção e que talvez não houvesse tempo para aprofundar todos durante a disciplina. Ainda assim, recomendou que os alunos anotassem os nomes apresentados nos slides para posterior pesquisa. Como exemplo, mencionou aplicações que envolvem uso de cartão de crédito, para as quais existem padrões específicos que precisam ser seguidos.

Ao relacionar o tema à LGPD, o professor destacou perguntas fundamentais: **o dado realmente é necessário?** e **esse dado precisa passar por todos os serviços da arquitetura?** Segundo ele, essas perguntas devem fazer parte do processo de desenvolvimento desde o início.

O professor apresentou como exemplo um projeto em desenvolvimento com uma empresa que possui uma **chopeira inteligente** com quatro bicos. O equipamento contém um Raspberry interno e câmeras associadas às posições de atendimento, permitindo que várias pessoas façam autoatendimento ao mesmo tempo. Essas câmeras são controladas pelo Raspberry, e o sistema também se conecta a serviços em nuvem.

O professor explicou que, quando a face de uma pessoa é capturada por uma dessas câmeras, a LGPD exige que seja possível informar exatamente onde os dados biométricos estão sendo tratados. A arquitetura precisa deixar claro se a imagem da face passa pelo Raspberry, se é encaminhada para a nuvem e se permanece ou não armazenada localmente. Se a face não for armazenada no Raspberry, isso reduz determinadas obrigações relacionadas a esse dispositivo; se for armazenada, essa retenção precisa ser declarada e tratada adequadamente.

O professor destacou que a primeira pergunta deve ser por que a face é necessária. Se o objetivo for apenas identificar a pessoa, depois que a identificação for concluída talvez não haja motivo para manter a imagem. Em um cenário, por exemplo, no qual o sistema precisa apenas verificar se o usuário é maior de idade em um ambiente com consumo incluído, pode ser suficiente consultar a informação necessária, liberar o uso e apagar o registro correspondente.

Por outro lado, o professor explicou que a necessidade de retenção muda conforme o modelo de negócio. Se o consumo for cobrado conforme o uso e o cliente posteriormente contestar a cobrança — afirmando, por exemplo, que bebeu cinco chopes em vez de dez —, a empresa precisará de alguma evidência para demonstrar quem efetivamente realizou aqueles consumos. Nesse cenário, guardar determinados registros pode ter uma finalidade legítima de auditoria.

O professor explicou que, em uma arquitetura desse tipo, a face não deveria ficar armazenada indiscriminadamente em serviços comuns nem disponível livremente em um banco ou *storage*. Ela poderia ser encaminhada para um serviço específico de auditoria e armazenada de forma criptografada. O dado seria consultado somente se alguém questionasse uma compra. Nessa situação, o sistema poderia apresentar não apenas a face, mas também o contexto do ambiente e um *timestamp* indicando o momento do consumo, de modo a demonstrar que a pessoa realmente estava naquele local.

A partir desse exemplo, o professor reforçou que o desenvolvimento de soluções em nuvem exige atenção às obrigações da LGPD. Além disso, alguns serviços de nuvem podem, por padrão, replicar dados em diferentes locais. O desenvolvedor precisa conhecer e controlar esse comportamento quando existem requisitos de privacidade, residência ou soberania dos dados.

O professor apresentou então o **Amazon S3** como um serviço que seria estudado posteriormente. Ele o comparou, de forma simplificada, a uma espécie de Google Drive voltado ao armazenamento de objetos. O serviço permite criar um espaço — um *bucket* — no qual dados podem ser colocados e sobre o qual diferentes permissões podem ser configuradas.

O professor explicou que é possível definir permissões de leitura, escrita, leitura e escrita, somente leitura ou somente escrita, além de decidir se o conteúdo terá acesso público ou privado. Também é possível criptografar os dados. Um *bucket*, por exemplo, pode ser configurado para utilizar determinada chave de criptografia, fazendo com que os arquivos sejam armazenados de forma criptografada.

O professor acrescentou que, caso o usuário utilize uma chave própria, surge também a responsabilidade de gerenciá-la. Se essa chave for perdida, o acesso aos dados pode ser comprometido de forma irreversível. Por esse motivo, é comum utilizar outro serviço da própria Amazon para gerenciar as chaves de criptografia. Esse serviço de gerenciamento de chaves também seria apresentado durante a disciplina.

Durante a discussão sobre soberania, surgiu uma comparação com o contexto dos Estados Unidos, onde existem mecanismos de avaliação de segurança para provedores que atendem órgãos federais. O professor aproveitou a questão para explicar as diferenças e dificuldades do cenário brasileiro.

### 4.2. Soberania de nuvem e cenário brasileiro

O professor explicou que, no Brasil, não existe um único órgão central responsável por cuidar de toda a avaliação de segurança e soberania de nuvem para o governo. Segundo ele, a situação não é trivial porque há legislações que determinam que apenas servidores de determinados órgãos podem ter acesso aos dados daquele próprio órgão. Isso dificulta a atuação de outra instituição como avaliadora centralizada e torna o ambiente regulatório e operacional complexo.

Como exemplo dessa complexidade, o professor mencionou um caso associado ao Banco Master. Segundo ele, até mesmo quando a Justiça precisava acessar determinados dados, o acesso não era simples. Uma equipe precisava se deslocar ao Banco Central, e os dados eram disponibilizados em um ambiente controlado. A razão seria a existência de regras que impediam que essas informações fossem simplesmente copiadas para um pen drive ou levadas para outro local.

O professor explicou que, paralelamente, existe atualmente um movimento importante em direção à **soberania de data centers**. Empresas brasileiras, especialmente empresas públicas ou de capital misto, como Serpro e Dataprev, já possuíam data centers e vêm reforçando essas estruturas para atender melhor às aplicações governamentais. Assim, quando um sistema governamental precisa cumprir uma legislação que impede que os dados saiam do país, uma alternativa é executar a solução em uma nuvem nacional operada por essas instituições.

O professor citou como motivação adicional a variação de custos de serviços de nuvem. Ele lembrou um caso em que o custo de armazenamento de dados teria dobrado de um ano para outro para determinada instituição, mostrando que dependência de provedores externos também pode representar um problema econômico.

O professor explicou que o Brasil ainda não possui um ecossistema de nuvem capaz de competir diretamente com os grandes provedores globais. Mesmo quando existem ambientes nacionais capazes de criar máquinas virtuais e oferecer infraestrutura básica, eles não conseguem acompanhar a quantidade de serviços que empresas como Amazon, Azure e Google lançam continuamente.

Como exemplo da velocidade de mudança, o professor relatou que deixou de preparar tutoriais baseados em capturas de tela para a disciplina de computação em nuvem. Antigamente, ele produzia passo a passo com *screenshots*, mas percebeu que, em cerca de seis meses, as interfaces já podiam mudar completamente. Como a disciplina de desenvolvimento para nuvem é ministrada há bastante tempo, os slides precisam ser atualizados praticamente todos os anos.

O professor destacou que a velocidade de evolução dos grandes provedores é muito alta. A Amazon lança serviços novos com frequência, modifica outros e eventualmente descontinua alguns. Antigamente, era possível visualizar boa parte dos serviços da Amazon em uma única tela; atualmente, os menus são divididos em categorias, submenus e novas especializações. Dentro de computação, por exemplo, existem opções com GPU, sem GPU, contêineres, máquinas virtuais e diversas outras possibilidades.

O professor explicou que, por esse motivo, equipes menores dificilmente conseguem acompanhar a indústria. Uma organização com algumas centenas de profissionais não consegue competir em ritmo de inovação com milhares de pessoas desenvolvendo continuamente novos serviços para Amazon, Azure ou Google.

Apesar dessa dificuldade, o professor destacou que já existe um movimento inicial em direção à soberania, com iniciativas de IA nacional, data centers nacionais e empresas brasileiras participando desse processo. Ainda assim, ele considerou o movimento lento e observou que não surgiu no Brasil um grande *player* privado capaz de competir de forma significativa com os gigantes internacionais.

O professor comparou o cenário com o dos Estados Unidos, onde grandes empresas privadas exercem esse papel. Na visão apresentada em aula, um cenário mais competitivo no Brasil exigiria algumas grandes empresas privadas capazes de disputar esse mercado, mas isso atualmente é muito difícil.

Como exemplo, o professor mencionou a Globo, uma empresa de grande porte que em determinado momento tentou manter um ambiente próprio de nuvem. Segundo ele, a dificuldade não era apenas de desempenho, mas também de mão de obra e escala operacional. A empresa chegou a apresentar artigos e trabalhos em eventos de redes e de Web/mídia sobre sua infraestrutura e liberava diversas ferramentas próprias como *open source*, o que indicava a existência de uma equipe forte de data center.

Mesmo assim, o professor explicou que, com o crescimento de serviços como Globoplay, manter toda a infraestrutura de forma própria se tornou muito difícil, levando à utilização de grandes provedores, como Amazon ou Google. Ele não afirmou qual provedor a Globo utilizava naquele momento, mas destacou que a empresa dependia de uma dessas grandes nuvens.

O professor reforçou que competir com os grandes provedores é difícil também por causa do preço. Em determinados casos, uma máquina virtual pode ser executada por poucos centavos de dólar, e alcançar esse nível de custo e escala com infraestrutura própria é extremamente difícil. Ele lembrou também que o UOL, em determinado período, tentou oferecer um serviço de nuvem mais voltado à infraestrutura, permitindo a criação de máquinas virtuais, mas destacou a dificuldade de sustentar a competição.

O professor explicou que não considerava muito provável, no curto prazo, que provedores brasileiros acompanhassem a mesma velocidade dos grandes provedores internacionais. A Europa, segundo ele, adotou outra estratégia: estabeleceu leis determinando onde determinados dados deveriam ficar e quais regras deveriam ser cumpridas. Como consequência, as grandes empresas tiveram de instalar data centers na própria região para continuar atendendo ao mercado.

O professor observou que movimento semelhante começa a acontecer no Brasil. As maiores empresas de nuvem já possuem data centers em São Paulo e, futuramente, podem ampliar a presença para outras regiões, talvez inclusive o Ceará. Para que uma estrutura nacional seja competitiva, ele avaliou que algum nível de apoio governamental pode ser necessário, especialmente por meio de exigências relacionadas à chamada **nuvem soberana**.

O professor mencionou ainda um projeto relacionado a um **sistema de mensageria soberano para o Brasil**, desenvolvido em parceria com a ABIN. Segundo ele, o sistema já estava em execução, mas restrito ao SISBIN, o Sistema Brasileiro de Inteligência, envolvendo instituições como Polícia Federal, Polícia Rodoviária Federal, a própria ABIN e polícias civis dos estados. O sistema teria sido desenvolvido pela equipe do professor, mas deveria ser executado exclusivamente em uma nuvem soberana, com o data center correspondente mantendo a infraestrutura de *back-end*.

O professor ressaltou que esse tipo de solução continua sendo um desafio econômico. Na estimativa apresentada por ele, uma infraestrutura equivalente hospedada em um grande provedor como a Amazon poderia custar aproximadamente metade do valor de uma infraestrutura nacional equivalente. A diferença decorre, entre outros fatores, do alto nível de automação dos grandes provedores. Enquanto nesses ambientes boa parte da operação é automatizada, um data center menor pode precisar de uma equipe muito maior para executar tarefas semelhantes.

A partir daí, o professor apresentou uma crítica adicional aos data centers: eles geram muitos empregos durante a construção, mas relativamente poucos empregos permanentes na operação. Como exemplo, retomou a visita à Angola Cables e comentou que tinha interesse em encaminhar alunos para estágio, mas descobriu que uma operação de data center muito grande poderia funcionar com uma equipe de aproximadamente 12 pessoas, muitas delas trabalhando remotamente.

O professor explicou que alguns profissionais ficam de plantão e precisam conseguir chegar rapidamente ao local quando há uma ocorrência física. Parte do acompanhamento pode ser realizado por NOCs localizados em diferentes cidades ou até em outros países, enquanto a equipe local permanece pequena. Além da equipe de TI, há empregos associados à segurança física do data center, mas o número de profissionais de tecnologia necessários para operar uma estrutura muito grande pode ser surpreendentemente reduzido.

O professor relacionou esse cenário à formação profissional em redes. Ele citou o exemplo de um curso tecnólogo em redes em Quixadá que forma dezenas de pessoas por ano e questionou onde todos esses profissionais encontrariam vagas específicas para administrar infraestrutura física tradicional. Muitas organizações deixaram de manter grandes data centers próprios e passaram a utilizar a nuvem.

Como consequência, o professor explicou que crescem as oportunidades em áreas como desenvolvimento e DevOps, nas quais o profissional administra uma **infraestrutura virtual** em vez de um data center físico. Para ele, esse é um dos temas que o Brasil precisará discutir ao pensar no futuro da formação profissional, da infraestrutura e da soberania tecnológica.

## 5. Histórico e infraestrutura da computação em nuvem

O professor explicou que, depois de abordar as motivações, a definição de computação em nuvem e a ideia de computação utilitária — isto é, computação consumida como uma *commodity*, de forma semelhante a água, energia ou telefonia —, era importante compreender um pouco do histórico que levou ao cenário atual.

O professor explicou que um dos marcos iniciais desse processo foi a ARPANET, surgida na década de 1960, em 1969. Posteriormente, tecnologias de virtualização começaram a ser adotadas por diversas empresas e evoluíram ao longo do tempo até se tornarem uma das bases da computação em nuvem.

O professor destacou que a Amazon lançou seu primeiro serviço de nuvem em 2006 e que, desde então, o setor cresceu continuamente. Ele comentou que houve até um ano de queda em determinado gráfico apresentado, o que chamou sua atenção, mas a tendência geral ao longo do tempo foi de forte crescimento.

O professor apresentou também uma linha do tempo com soluções privadas de nuvem. Entre os exemplos citados estavam Eucalyptus, OpenNebula, OpenStack e outras plataformas que se propunham a oferecer, pelo menos no nível de infraestrutura, serviços semelhantes aos de provedores públicos para execução *on-premises*.

O professor relatou que começou seu mestrado em uma época em que essas soluções eram bastante utilizadas. Em seus trabalhos, utilizou primeiro OpenNebula e depois OpenStack. Ele ressaltou que tanto OpenStack quanto OpenNebula continuam existindo e podem ser utilizados por organizações que desejem manter um ambiente próprio de nuvem.

Como exemplo atual, o professor retomou a visita à Angola Cables. A empresa mostrou à turma seu ambiente de nuvem baseado em OpenStack, utilizado para oferecer infraestrutura aos clientes. Nesse ambiente, os clientes podem executar máquinas virtuais, contêineres, bancos de dados gerenciados e outros recursos. O professor comentou, porém, que até mesmo na disciplina regular de computação em nuvem do departamento, com 64 horas, a nuvem privada deixou de receber tanta atenção porque a adoção de nuvens públicas se tornou predominante.

O professor apontou o **Office 365** como outro marco importante. A Microsoft, tradicionalmente associada à venda de produtos como Windows e Office, percebeu que também precisava oferecer seus softwares no modelo de serviço em nuvem. Por volta de 2011, o Office começou a se consolidar nesse formato.

O professor explicou que o modelo de comercialização também mudou. Antigamente, era possível comprar uma versão do Office com uso praticamente permanente e continuar utilizando aquela mesma licença por cinco ou seis anos. Atualmente, o modelo predominante é de assinatura: o usuário paga periodicamente pela versão instalada ou utiliza uma versão online, como o Office 365. Para o professor, isso exemplifica como tanto o mercado quanto a própria forma de consumir software foram transformados pela nuvem.

O professor retomou então as motivações para adotar computação em nuvem e destacou novamente a **redução de custos**. Além dela, chamou atenção para o conceito de **time to market**. Uma empresa que precisa desenvolver e disponibilizar rapidamente um serviço tende a se beneficiar da nuvem porque não pode perder muito tempo construindo toda a infraestrutura do zero.

O professor explicou que esse fator é particularmente importante para startups. Muitas vezes, uma startup precisa chegar ao mercado antes dos concorrentes, e a nuvem permite disponibilizar rapidamente um produto sem construir uma infraestrutura própria. A Amazon, inclusive, possui programas voltados especificamente para startups. Uma empresa pode preencher formulários, apresentar sua proposta e, em determinados casos, receber créditos ou *grants* em dólares para utilizar os serviços da plataforma.

### 5.1. Regiões, locais de borda e zonas de disponibilidade

O professor apresentou estatísticas de 2025 e alertou que elas provavelmente já estariam desatualizadas, devido à velocidade de expansão da AWS. Naquele material, a Amazon possuía 38 regiões. Ele explicou que uma **região** corresponde a um local geográfico no qual o cliente pode escolher executar determinados serviços.

Durante a aula, o professor abriu o site de infraestrutura global da AWS para mostrar a distribuição dos recursos. Ele destacou que, nos Estados Unidos, existem duas regiões de nuvem dedicadas especificamente ao governo norte-americano, uma no leste e outra no oeste. Usuários comuns não podem executar serviços nessas regiões governamentais.

Ao observar a América do Sul, o professor explicou que São Paulo aparece como uma região da AWS. Ele também chamou atenção para outros pontos exibidos no mapa, como locais no Chile, esclarecendo que nem todos representam regiões completas. Alguns deles são **locais de borda**.

O professor explicou que um **local de borda** também é uma infraestrutura de data center, mas não disponibiliza todos os serviços de uma região completa. Esses locais normalmente executam serviços mais associados a CDN (*Content Delivery Network*). A função de uma CDN é manter conteúdo mais próximo dos usuários para reduzir latência e melhorar o desempenho.

Como exemplo, o professor explicou que, quando um usuário em Fortaleza assiste a um vídeo da Netflix, o serviço pode identificar qual local de borda está mais próximo e entregar o conteúdo a partir dele, em vez de buscar todos os dados em um data center muito distante. Assim, alguns serviços da Amazon podem operar nesses locais de borda, embora recursos completos de computação, como a criação de determinadas máquinas virtuais, dependam de uma região.

O professor mencionou que, no material apresentado, a AWS possuía 38 regiões e cerca de 120 **zonas de disponibilidade**. Ele explicou que uma zona de disponibilidade pode ser entendida, de forma simplificada, como um data center dentro de uma região. Em uma região como São Paulo, a Amazon pode manter três ou mais data centers separados, cada um correspondendo a uma zona de disponibilidade.

O professor ressaltou que a Amazon procura garantir independência entre essas zonas. Cada uma pode possuir conexões de Internet diferentes, alimentação elétrica própria e segurança física sob responsabilidade do provedor. Se ocorrer uma falha em uma zona — por exemplo, queda de conectividade, problema de energia ou outro incidente —, as demais podem continuar funcionando.

Ao mesmo tempo, o professor explicou que essas zonas são interligadas por conexões de alta velocidade. Por isso, ao criar recursos na AWS, o usuário frequentemente pode escolher em qual zona de disponibilidade deseja executá-los.

O professor destacou que essa escolha é importante para disponibilidade. O preço pode ser o mesmo entre diferentes zonas de uma mesma região, mas, se uma aplicação tiver três réplicas e todas forem colocadas na mesma zona, uma falha nessa zona pode derrubar toda a aplicação. Para reduzir esse risco, é recomendável distribuir as máquinas ou réplicas entre zonas diferentes.

O professor explicou que, embora essas máquinas estejam fisicamente mais distantes umas das outras, a Amazon fornece comunicação de alta velocidade entre as zonas. O ponto principal é garantir que elas não estejam concentradas na mesma infraestrutura física.

O professor comentou que a quantidade de zonas de disponibilidade varia entre regiões e mencionou que a região do leste dos Estados Unidos, na Virgínia, possuiria até oito zonas. Ele a descreveu como uma das maiores regiões e também uma das mais baratas da AWS.

O professor explicou que o custo de executar uma máquina pode variar conforme a região. Executar uma instância no leste dos Estados Unidos pode ser mais barato do que executá-la em São Paulo. Entretanto, a decisão não pode ser tomada apenas com base em preço. É necessário considerar novamente **compliance** e **soberania de dados**: uma aplicação que armazena ou processa determinados dados pode ou não ter permissão para ser executada fora do Brasil.

O professor apresentou ainda algumas estatísticas do material, incluindo valores relacionados à quantidade de infraestrutura e de clientes. Segundo os dados de 2025 mostrados nos slides, a AWS possuía aproximadamente 4,19 milhões de clientes ou contas criadas. Ele observou que uma única conta pode representar uma organização inteira. Também citou uma receita de aproximadamente US$ 107 bilhões em 2024 e destacou que esses números vêm crescendo.

O professor mostrou ainda exemplos de empresas que declararam publicamente utilizar a Amazon para algum tipo de serviço. Ele ressaltou que aparecer como cliente da AWS não significa necessariamente executar toda a infraestrutura na Amazon. Uma empresa pode manter data centers próprios e utilizar a AWS apenas para determinadas cargas.

Como exemplo, o professor mencionou a Meta/Facebook, que possui data centers próprios, mas também pode utilizar serviços da Amazon para determinadas finalidades. Citou também o LinkedIn, que teria sido cliente da Amazon antes de ser adquirido pela Microsoft. Segundo ele, atualmente o LinkedIn estaria executando seus serviços na nuvem da Microsoft, e a migração de todos aqueles dados provavelmente teria sido um trabalho considerável.

O professor observou ainda que alguns exemplos presentes em imagens antigas dos slides já não existiam ou haviam mudado de operação, como Peixe Urbano e outras empresas. Esse fato reforçava a necessidade de atualizar o material constantemente.

O professor explicou que o site da AWS possui diversos *cases* nos quais empresas relatam publicamente por que utilizam a plataforma. Ele lembrou, por exemplo, de um vídeo da Guanabara em que um profissional da empresa explicava os motivos para usar a Amazon. Para o professor, esses relatos são interessantes porque mostram aplicações reais e ajudam a entender o que organizações concretas fazem na nuvem e por que escolhem determinados serviços.

Por fim, o professor retomou que as definições fundamentais de computação em nuvem já haviam sido apresentadas e indicou que o próximo passo seria estudar as características essenciais da nuvem. Segundo ele, o próprio NIST define a computação em nuvem a partir de cinco características essenciais.

## 6. Características essenciais da computação em nuvem

O professor explicou que a primeira característica essencial da computação em nuvem é o **autosserviço sob demanda** (*on-demand self-service*). Para que um ambiente seja considerado nuvem, o usuário precisa conseguir solicitar e consumir recursos quando precisar, normalmente por meio de APIs, preferencialmente sem depender de uma negociação ou intervenção humana para cada solicitação.

O professor observou, porém, que esse princípio possui limites práticos. Em teoria, um usuário poderia entrar na Amazon e solicitar vinte máquinas imediatamente. Em uma pesquisa realizada pelo laboratório, entretanto, a equipe precisou executar cerca de mil máquinas e encontrou um limite imposto pela plataforma. Foi necessário solicitar aumento de cota por meio de formulário, mas a solicitação não foi aprovada.

Segundo o professor, a equipe pretendia utilizar essas aproximadamente mil máquinas por um período curto, em torno de duas horas, para uma pesquisa que simulava chamadas de vídeo. A Amazon avaliou que haveria um esforço relevante para provisionar aquela quantidade de recursos por tão pouco tempo e não liberou o pedido. O professor comentou que, se as máquinas fossem utilizadas por um mês, talvez a decisão fosse diferente. Assim, o autosserviço sob demanda funciona dentro de limites considerados razoáveis pelo provedor; solicitações muito grandes podem exigir negociação adicional.

O professor explicou que outra característica fundamental é a **elasticidade**. Elasticidade significa conseguir aumentar e diminuir os recursos ao longo do tempo de acordo com a carga. Para ilustrar, ele utilizou um gráfico em que uma linha representava a demanda dos usuários e outra representava a capacidade disponibilizada pela infraestrutura.

O professor explicou que, em um modelo reativo clássico, uma aplicação começa com determinada quantidade de recursos. Quando a utilização chega a um limite, uma nova máquina é criada. Se a demanda continuar crescendo e atingir outro limite, outra máquina é adicionada, e assim sucessivamente. Quando a demanda diminui, o ideal seria também remover recursos que deixaram de ser necessários.

O professor chamou atenção para dois problemas que aparecem nesse tipo de escalabilidade. Em alguns momentos, a infraestrutura pode ficar **superdimensionada**, com capacidade muito maior do que a demanda real, o que significa pagar por recursos que não estão sendo utilizados. Em outros momentos, a demanda pode crescer mais rapidamente do que a infraestrutura consegue reagir; a nova máquina pode demorar para ser criada ou o mecanismo pode reagir tarde demais. Nessa situação, faltará capacidade e parte dos clientes poderá deixar de ser atendida.

O professor explicou que existem diversas técnicas de elasticidade, embora a disciplina não fosse aprofundá-las. Entre os conceitos citados estavam elasticidade horizontal, vertical, preditiva e reativa. O resultado ideal seria fazer a capacidade computacional acompanhar a curva de demanda da forma mais próxima possível, reduzindo tanto desperdício de recursos quanto falta de capacidade.

O professor destacou que alcançar esse comportamento pode envolver técnicas de predição, capazes de antecipar para onde o consumo está caminhando. Também pode envolver elasticidade horizontal com máquinas menores. Se as unidades de capacidade forem muito grandes, cada aumento acrescenta um salto excessivo de recursos; utilizando máquinas menores, é possível aumentar ou reduzir a capacidade de forma mais gradual.

O professor observou que existem muitos trabalhos acadêmicos dedicados a esse problema, incluindo otimização de escalabilidade, otimização de elasticidade e algoritmos preditivos para elasticidade. Na disciplina, porém, o objetivo era principalmente compreender o conceito como uma característica essencial da computação em nuvem.

Outra característica apresentada pelo professor foi o **pooling de recursos**. Ele explicou que uma das razões pelas quais a Amazon consegue praticar preços tão competitivos é o grande número de clientes utilizando a mesma infraestrutura. Muitos usuários solicitam recursos simultaneamente, permitindo que o provedor compartilhe de forma eficiente sua capacidade física.

O professor explicou que esse compartilhamento também levou a muitas pesquisas sobre a interferência entre cargas de trabalho. Em um data center existem racks; dentro deles, servidores físicos; e, dentro de um mesmo servidor, podem existir máquinas virtuais pertencentes a clientes diferentes. Assim, uma máquina virtual de um usuário pode compartilhar hardware com a máquina virtual de outro usuário.

Como exemplo, o professor explicou que uma dessas máquinas pode estar executando computação de alto desempenho, processamento de imagens ou outra carga muito pesada, utilizando 100% de CPU e transferindo grandes quantidades de dados. Pesquisas mostraram que uma carga desse tipo pode impactar o desempenho de outro serviço executado em uma máquina virtual vizinha no mesmo hardware físico.

O professor destacou que as tecnologias de virtualização evoluíram para reduzir esse tipo de interferência, por meio de mecanismos de isolamento de CPU, isolamento de contêineres e outras técnicas. Embora o problema atualmente seja menos grave do que já foi, ainda pode existir algum nível de impacto entre cargas que compartilham a mesma infraestrutura.

O professor explicou que essa é uma consequência do modelo **multi-inquilino** (*multi-tenant*) da nuvem: diversos clientes fazem *deploy* e enviam requisições para recursos pertencentes ao mesmo grande ambiente físico. Em determinadas situações, uma carga pode afetar outra. Por outro lado, é justamente esse compartilhamento em larga escala que permite aos grandes provedores manter preços competitivos.

O professor relacionou essa vantagem à **economia de escala**. Se empresas como Globo, UOL ou Angola Cables tivessem o mesmo volume de usuários de um grande provedor global, talvez também conseguissem reduzir mais os seus custos. A lógica é semelhante a “ganhar no atacado”: em vez de obter uma margem grande sobre poucas máquinas, o provedor pode obter uma margem menor por recurso, mas atender uma quantidade enorme de clientes.

O professor explicou que isso também aumenta a utilização dos servidores físicos. Manter um servidor funcionando consome energia, água, infraestrutura e mão de obra independentemente de ele estar usando 20% ou 100% da CPU. Se apenas 20% estiverem ocupados, existem 80% de capacidade potencialmente disponível para outros clientes. O compartilhamento de recursos permite aproveitar melhor essa capacidade.

Por fim, o professor retomou outras características essenciais já discutidas: **pagamento baseado no uso**, **garantias estabelecidas por SLA** e **amplo acesso por meio da rede**. A nuvem deve poder ser acessada de diferentes lugares por meio da rede e de suas APIs. Com isso, a turma havia percorrido as principais características essenciais da computação em nuvem apresentadas naquela parte da aula.

## 7. Modelos de serviço

O professor explicou que os **modelos de serviço** estão relacionados ao tipo de recurso que é oferecido, ao perfil de quem o utiliza e à divisão de responsabilidades sobre o ambiente. Ele apresentou os três níveis clássicos: Software como Serviço, Plataforma como Serviço e Infraestrutura como Serviço.

### 7.1. Software como Serviço (SaaS)

O professor explicou que o primeiro modelo é o **Software como Serviço (SaaS)**, no qual o consumidor normalmente atua como usuário final. Todos utilizam SaaS no cotidiano, muitas vezes sem pensar nisso como computação em nuvem. Entre os exemplos citados estavam Google Docs, Google Drive, Dropbox, Office 365, Netflix, ChatGPT e Gemini.

Nesse modelo, o usuário consome diretamente uma aplicação pronta. O professor destacou que existe um conjunto gigantesco de usuários de Software como Serviço, incluindo pessoas que não trabalham com TI. Também observou que alunos que seguirem para a área de desenvolvimento poderão futuramente produzir softwares destinados a serem fornecidos a terceiros exatamente nesse formato.

### 7.2. Plataforma como Serviço (PaaS)

O professor explicou que o segundo modelo é a **Plataforma como Serviço (PaaS)** e que seus usuários típicos são desenvolvedores de aplicações. Nesse caso, o desenvolvedor possui o próprio código, mas não precisa administrar toda a infraestrutura utilizada para executá-lo.

Como exemplo, o professor descreveu uma situação em que o desenvolvedor informa ao Google ou à Amazon que deseja executar uma aplicação Python e deixa a plataforma cuidar do restante. Também citou o Heroku e outras plataformas nas quais o usuário envia parte do código, executa poucos comandos e rapidamente coloca a aplicação em funcionamento.

O professor destacou que a característica central da plataforma como serviço é que o ambiente é gerenciado por terceiros, enquanto o código continua pertencendo ao desenvolvedor. Assim, há um conjunto de usuários especificamente voltado ao desenvolvimento, que consome esse tipo de plataforma para executar suas aplicações.

### 7.3. Infraestrutura como Serviço (IaaS)

O professor explicou que o terceiro modelo é a **Infraestrutura como Serviço (IaaS)**. Em comparação com SaaS, esse modelo possui um grupo de usuários mais especializado, normalmente formado por profissionais de DevOps, arquitetos de soluções e pessoas responsáveis pela infraestrutura.

Esses profissionais criam máquinas virtuais, configuram redes virtuais, criam buckets e bancos de dados e definem a infraestrutura sobre a qual as aplicações serão executadas. O professor ressaltou que, nesse nível, o cliente possui muito mais controle, mas também assume mais responsabilidades.

### 7.4. Everything as a Service (XaaS)

O professor explicou que atualmente também é comum falar em **Everything as a Service (XaaS)**, ou seja, praticamente qualquer recurso tecnológico pode ser oferecido como serviço.

Como exemplo, apresentou **Game as a Service**. Nesse modelo, o usuário não precisa instalar ou executar localmente todo o jogo. O professor mencionou serviços ligados à Nvidia e ao Xbox e lembrou uma propaganda antiga em que uma pessoa jogava, em um tablet, um jogo 3D muito pesado enquanto estava sentada em uma praça no Japão. O tablet não possuía poder computacional para executar localmente aquele jogo, que normalmente exigiria uma GPU dedicada, mas o processamento era realizado remotamente e entregue ao dispositivo como serviço.

O professor citou também **conteúdo e dados como serviço**, relacionando a ideia a plataformas como Dropbox, Google Drive, Mega e outras ferramentas de armazenamento e distribuição de conteúdo.

Na área de redes, o professor explicou que praticamente toda a infraestrutura também pode ser virtualizada. É possível trabalhar com roteadores virtuais, switches virtuais, VLANs, túneis virtualizados, conexões criptografadas com ou sem VPN e diferentes tipos de pontos de acesso. Ele comentou que alguns equipamentos de rede mais novos utilizados no laboratório permitem criar várias redes lógicas independentes sobre o mesmo hardware, como uma rede principal, uma rede de visitantes e outras redes separadas. Esse é um exemplo de virtualização de rádio e de rede.

O professor apresentou também o conceito de **Desktop as a Service**. Em alguns projetos do LSBD, o profissional pode usar praticamente qualquer computador local e acessar um ambiente remoto que ocupa a tela inteira. Embora pareça estar trabalhando no próprio notebook, na realidade está utilizando um desktop virtual executado em outro lugar.

O professor explicou que esse tipo de ambiente depende de conectividade com a Internet. Se o profissional estiver em uma região sem conexão, não conseguirá trabalhar, pois os recursos estão no ambiente remoto. Por outro lado, a máquina virtual disponibilizada pode ser muito mais potente do que o computador local, com recursos como 64 GB de RAM, GPU e outras capacidades.

O professor ressaltou ainda que o Desktop as a Service pode aumentar o controle de segurança. O código permanece dentro do ambiente corporativo e determinadas ações podem ser restringidas. Como exemplo, citou o uso de `pip install`: dependendo da política, o usuário não pode instalar qualquer pacote livremente. A organização controla o que pode ou não ser instalado e mantém o ambiente mais padronizado e seguro. Segundo ele, esse modelo vem se tornando cada vez mais comum.

O professor apresentou também **Security as a Service**. Já existem empresas que fornecem serviços de VPN, gateway, firewall, DMZ e outros componentes de segurança como serviço. Algumas operadoras e empresas de conectividade podem criar redes totalmente isoladas para um cliente, inclusive em arquiteturas do tipo *hub-and-spoke*, estabelecendo VPNs e integrando várias filiais a uma matriz por meio de uma rede privada.

O professor explicou que esses exemplos ajudam a compreender os principais papéis envolvidos na nuvem. Um desenvolvedor pode consumir serviços de infraestrutura, consumir serviços de plataforma e também criar ou consumir softwares oferecidos como serviço. O usuário final normalmente consome Software como Serviço. Já o provedor é responsável por disponibilizar os diferentes níveis de serviço e garantir sua operação.

O professor destacou que uma mesma pessoa pode assumir papéis diferentes em momentos diferentes. Alguém que trabalhe para Amazon, Google ou outro provedor pode estar no papel de fornecedor, garantindo que a infraestrutura permaneça funcionando, executando manutenção e aplicando correções de segurança. Em outro contexto, essa mesma pessoa pode atuar como desenvolvedora, como profissional DevOps ou simplesmente como usuária final de serviços em nuvem.

O professor explicou que, no nível de **Infraestrutura como Serviço**, há diversas responsabilidades do cliente. Uma delas é escolher o sistema operacional. O usuário decide, por exemplo, se a aplicação será executada em Ubuntu, Windows ou outro sistema e qual versão será utilizada.

O professor ressaltou que, nesse nível, quem administra o sistema operacional é o próprio cliente. Se uma máquina Linux permanecer dois anos sem atualizações, a responsabilidade é de quem gerencia a infraestrutura. Ninguém fará automaticamente um `apt upgrade` por aquele administrador, a menos que ele tenha configurado mecanismos específicos para isso.

O professor chamou atenção para a dificuldade que aparece quando existem centenas ou milhares de máquinas virtuais. Ainda que gerenciar todas elas seja trabalhoso, a responsabilidade continua pertencendo ao cliente quando ele escolhe atuar nesse nível de serviço.

O professor explicou que o mesmo vale para armazenamento. Se uma máquina possui 100 GB de disco e todo o espaço é consumido, cabe ao cliente administrar a situação. Também é responsabilidade do cliente definir regras de firewall e decidir em quais zonas de disponibilidade os recursos serão executados.

Como exemplo, o professor explicou que, se alguém criar dez máquinas e colocar todas na mesma zona de disponibilidade, uma falha naquela zona pode derrubar todas as instâncias. Nesse caso, parte da responsabilidade é do próprio arquiteto, que poderia ter distribuído os serviços entre zonas diferentes.

O professor ressaltou que atuar em infraestrutura oferece mais controle, algo que pode ser interessante para profissionais de segurança, porque eles sabem exatamente como os recursos estão configurados. Ao mesmo tempo, esse controle traz maior responsabilidade. Quanto mais diretamente o cliente gerencia o ambiente, mais aspectos de segurança e operação ficam sob sua responsabilidade.

O professor comentou que grandes provedores oferecem muitos serviços nesse nível, citando Amazon, Google e Azure. Como exemplo, lembrou que, no EC2, o usuário pode criar uma máquina virtual, escolher o tamanho da instância, quantidade de memória e armazenamento, pagando normalmente um valor por hora de utilização.

Ao passar para **Plataforma como Serviço**, o professor explicou que a divisão de responsabilidades muda. O provedor passa a fornecer e manter o sistema operacional e também define quais linguagens de programação e versões são suportadas.

Como exemplo, o professor explicou que, se um desenvolvedor chegar com uma aplicação escrita em uma linguagem que a plataforma não suporta, como Rust no exemplo dado em aula, o provedor pode exigir que o próprio usuário crie uma máquina e cuide de toda a infraestrutura. Por outro lado, linguagens amplamente suportadas, como Java, Python, PHP e JavaScript/Node, normalmente possuem ambientes prontos.

O professor destacou que, nesse modelo, o provedor cuida do sistema operacional e das atualizações. Muitas vezes o desenvolvedor nem sabe exatamente qual sistema operacional está sendo utilizado por baixo da plataforma, e isso deixa de ser relevante para sua tarefa. Pode ser, por exemplo, alguma versão do Amazon Linux, mas quem administra essa camada é a própria Amazon.

O desenvolvedor se concentra principalmente no código. O professor explicou que a plataforma pode permitir apontar para um repositório ou enviar um arquivo compactado com a aplicação. A partir daí, o provedor cuida de preparar o ambiente, compilar quando necessário e colocar o código em execução.

Como exemplos de PaaS, o professor mencionou o **Google App Engine**, que oferecia uma cota gratuita para pequenos projetos. Segundo ele, era possível manter vários sistemas pequenos sem custo, desde que o uso de tráfego, upload e download permanecesse dentro das cotas. Um sistema destinado a distribuir vídeos poderia ultrapassar rapidamente o limite, mas um site pessoal, portfólio ou pequeno projeto para currículo poderia funcionar gratuitamente por bastante tempo, enquanto o Google mantivesse aquela política.

O professor citou também o **Azure App Service**, no qual o usuário pode escolher um ambiente para executar uma aplicação em determinada linguagem e versão, e uma solução equivalente da Amazon, mencionada como **Elastic Beanstalk**. Segundo ele, esse serviço suportava linguagens como Ruby on Rails, PHP, JavaScript, Python e Java, além de diferentes versões de cada ambiente, embora essa lista pudesse ter sido ampliada com o tempo.

Por fim, o professor retomou o **Software como Serviço**. Nesse nível, o usuário praticamente não administra nada da infraestrutura. Ao utilizar Google Docs, por exemplo, não escolhe a região na qual o serviço será executado, a linguagem em que o software foi desenvolvido nem o sistema operacional utilizado. Todas essas decisões pertencem ao provedor; o usuário simplesmente acessa e utiliza a aplicação.

O professor citou como exemplos Salesforce, Google Drive, Google Docs, Office 365, Google Classroom e Google Meet. Em todos esses casos, o usuário consome a aplicação pronta e não precisa conhecer as camadas inferiores.

O professor explicou ainda que existem empresas que oferecem até mesmo infraestrutura física dedicada como serviço. Nesse tipo de contratação, uma máquina física pode ser reservada exclusivamente para um cliente. Se esse cliente utilizar apenas 1% da CPU, os recursos restantes continuarão reservados para ele. Trata-se de um nível com menos compartilhamento e mais controle, mas também com custo diferente.

Durante essa explicação, o professor fez uma observação informal de que, após conversar sobre determinados serviços, frequentemente começava a receber propagandas semelhantes em plataformas como Instagram, usando isso como gancho para comentar que anúncios de contratação de máquinas dedicadas e serviços de nuvem aparecem com frequência.

O professor resumiu a relação entre os modelos dizendo que, conforme o nível de abstração aumenta, o usuário perde parte do controle e também reduz suas responsabilidades. Na infraestrutura, há grande controle e grande responsabilidade; na plataforma, parte do ambiente é abstraída pelo provedor; no Software como Serviço, o usuário praticamente apenas utiliza a aplicação. Inversamente, quanto mais se desce em direção à infraestrutura, maior é o controle técnico disponível.

Por fim, o professor explicou que a disciplina trabalharia principalmente nos níveis de **infraestrutura** e **plataforma**, porque esses níveis têm maior relação com segurança em nuvem. O foco não seria desenvolver uma aplicação segura para a nuvem, o que poderia constituir outra disciplina específica sobre desenvolvimento seguro, mas compreender como construir e proteger ambientes de nuvem e suas respectivas infraestruturas.

## 8. Modelos de implantação

O professor explicou que existem quatro formas clássicas de implantação de nuvem: pública, privada, comunitária e híbrida.

### 8.1. Nuvem pública

O professor explicou que a **nuvem pública** é o modelo utilizado quando se contratam provedores como Amazon, Azure, Google, Alibaba Cloud, Angola Cables Cloud ou outros serviços disponíveis comercialmente para o público em geral.

Segundo ele, o termo “pública” significa que qualquer pessoa ou organização que cumpra as condições do provedor — por exemplo, possuindo uma forma de pagamento válida — pode contratar o serviço. Isso não significa que todos os recursos sejam gratuitos. O professor comparou a ideia ao conceito de “praia pública” existente em alguns países: qualquer pessoa pode entrar, embora isso não signifique que tudo o que existe naquele ambiente seja gratuito. Ele observou que a analogia é menos intuitiva no Brasil porque, por definição, as praias já são públicas.

O professor explicou que alguns provedores oferecem níveis gratuitos ou determinados serviços sem cobrança dentro de certas condições. Amazon e Google, por exemplo, possuem recursos que podem ser utilizados gratuitamente dentro de limites específicos. Entretanto, a maior parte dos serviços de nuvem pública é paga.

### 8.2. Nuvem privada

O professor explicou que o conceito de **nuvem privada** precisa ser compreendido com cuidado. Uma nuvem privada não precisa necessariamente estar fisicamente dentro da organização. Um ambiente de nuvem instalado no próprio LSBD e utilizado apenas por pessoas autorizadas seria privado, mas também é possível ter uma nuvem privada hospedada fisicamente no data center de um provedor externo.

Como exemplo, o professor retomou as regiões da Amazon destinadas ao governo dos Estados Unidos. Esses ambientes são executados em infraestrutura da AWS, mas não estão disponíveis para qualquer cliente com cartão de crédito; o uso é restrito ao governo norte-americano. Por isso, podem ser entendidos como ambientes privados destinados a uma organização ou conjunto específico de organizações.

O professor explicou que uma nuvem privada pode estar *on-premises*, como em uma sala de data center da própria instituição, ou dentro de uma infraestrutura de terceiros. Durante a discussão, foi levantado o exemplo de espaços físicos reservados dentro de um data center como o da Angola Cables. O professor esclareceu que o proprietário do data center pode fornecer apenas energia, conectividade e segurança física, sem ter acesso à rede lógica nem aos dados do cliente.

O professor explicou que, em alguns casos, existe também a figura de um operador contratado. Uma empresa como a Microsoft, por exemplo, pode não querer manter um funcionário permanentemente naquele local e contratar o próprio data center para executar determinadas atividades de manutenção. Nessa situação, o profissional terceirizado atua de acordo com o contrato e, operacionalmente, como extensão da empresa cliente, mas o ambiente continua sendo privado.

O professor relacionou essa ideia ao conceito de **nuvem soberana**. Uma organização governamental pode contratar um provedor para instalar equipamentos dentro de um data center nacional e restringir completamente o acesso externo. Ele citou novamente iniciativas brasileiras em que equipamentos de grandes provedores são fisicamente instalados em data centers nacionais e ficam sob gestão local.

O professor explicou que, no caso citado em aula, a nuvem soberana brasileira possui tanto componentes baseados em tecnologias *open source* quanto equipamentos de provedores comerciais. Em um dos arranjos mencionados, a Amazon fornece servidores que são instalados fisicamente no data center nacional. O provedor entrega a infraestrutura, mas o ambiente fica isolado e sob gestão do órgão responsável.

Segundo o professor, uma vantagem desse modelo é poder utilizar parte dos serviços e das tecnologias do provedor comercial sem transferir os dados para fora do ambiente soberano. Alguns serviços podem ser oferecidos localmente, enquanto outros não estarão disponíveis caso dependam de hardware que não exista naquele data center, como determinadas GPUs.

O professor explicou que esse arranjo também pode simplificar a renovação da infraestrutura. Quando um equipamento envelhece ou fica desatualizado, o contrato pode prever que o provedor substitua o hardware, reduzindo a necessidade de a instituição se preocupar diretamente com todo o ciclo de atualização física.

O professor mencionou que, no projeto de mensageria anteriormente citado, a equipe executava o sistema sobre uma plataforma chamada **Estaleiro**. Essa plataforma utilizava Linux e Kubernetes, com uma camada de Kubernetes customizada pelo provedor nacional. O projeto MSGB era executado sobre esse ambiente.

O professor explicou que, dependendo da decisão do patrocinador do projeto, também seria possível utilizar uma infraestrutura Amazon ou Google instalada fisicamente dentro do data center soberano. Segundo ele, esses ambientes já estavam disponíveis e havia um processo para receber também uma solução da Microsoft.

O professor ressaltou que a instalação desse tipo de ambiente envolve treinamento. Não basta simplesmente colocar os equipamentos do provedor no data center. A empresa precisa ensinar à equipe local como o ambiente funciona, porque depois a gestão operacional ficará com essa equipe. O provedor não necessariamente abre o código de seus produtos, mas os responsáveis locais precisam saber acompanhar utilização, falhas, disponibilidade e manutenção.

O professor explicou que essa gestão local é particularmente importante quando a legislação exige que os dados permaneçam **fisicamente no Brasil** e sob gestão de uma entidade nacional. Se o requisito fosse apenas manter os dados dentro do território brasileiro, uma região pública da AWS em São Paulo poderia ser suficiente em determinados casos. Entretanto, algumas exigências de soberania envolvem também quem possui controle sobre o ambiente.

O professor comentou que empresas norte-americanas de tecnologia, como Amazon e Apple, podem ser convocadas pelo Congresso dos Estados Unidos e questionadas sobre fornecimento de dados. Em sua explicação, destacou que o governo norte-americano possui grande poder sobre empresas sediadas naquele país. Por isso, para determinados projetos soberanos, não basta apenas que o servidor esteja fisicamente no Brasil; é desejável que a gestão e o controle também estejam fora do alcance operacional direto do provedor estrangeiro.

O professor explicou que, em uma nuvem soberana corretamente isolada, a infraestrutura de rede deve impedir que cópias dos dados sejam enviadas para outros locais. Em ambientes públicos comuns, por outro lado, sistemas distribuídos podem manter réplicas em diferentes regiões por razões de disponibilidade e recuperação de desastres.

Para ilustrar a importância da distribuição geográfica, o professor lembrou os atentados às Torres Gêmeas. Segundo ele, naquela época algumas empresas mantinham um escritório principal em uma torre e um escritório de backup na outra. Como as duas torres foram destruídas, algumas organizações perderam simultaneamente o ambiente principal e o ambiente de contingência. O exemplo foi utilizado para mostrar que redundância não deve significar apenas ter duas cópias próximas; em cenários críticos, é importante distribuí-las geograficamente.

O professor explicou que, na nuvem pública, uma organização pode manter uma cópia de dados no leste dos Estados Unidos e outra no oeste para reduzir o risco de uma catástrofe regional afetar tudo ao mesmo tempo. Dentro de uma mesma região, as zonas de disponibilidade também são projetadas para reduzir riscos: energia, conectividade e mecanismos de combate a incêndio são separados entre os data centers.

Retomando o exemplo de São Paulo, o professor explicou que a região possui múltiplas zonas de disponibilidade. Em teoria, mesmo que um data center sofra um incêndio ou falha grave, as demais zonas devem continuar funcionando. Essa independência faz parte da garantia arquitetural apresentada pelos provedores, ainda que eventos extremos sejam, naturalmente, difíceis de testar no mundo real.

O professor resumiu que uma **nuvem privada** é destinada ao uso exclusivo de uma única organização, independentemente de estar fisicamente dentro dessa organização ou hospedada em infraestrutura de terceiros.

### 8.3. Nuvem comunitária

O professor explicou que a **nuvem comunitária** pode ser entendida como um ambiente compartilhado por duas ou mais organizações que possuem interesses comuns. Como exemplo, se dois laboratórios decidissem reunir e compartilhar seus recursos para construir uma nuvem de uso conjunto, essa infraestrutura poderia ser classificada como uma nuvem comunitária.

### 8.4. Nuvem híbrida e multicloud

O professor explicou que a **nuvem híbrida** resulta da combinação de diferentes tipos de nuvem, como uma nuvem privada com uma nuvem pública ou outros arranjos entre ambientes distintos. Muitas empresas também trabalham atualmente com o conceito de **multicloud**, utilizando mais de um provedor público.

O professor explicou que uma das motivações para multicloud é reduzir dependência de um único provedor. Existem falhas em que um serviço pode permanecer indisponível por várias horas porque toda a aplicação depende de uma única nuvem. Para mitigar esse risco, uma organização pode executar parte dos serviços na Amazon e outra parte no Azure, por exemplo, permitindo que um ambiente sustente determinadas funções caso o outro enfrente problemas.

O professor observou que diferentes combinações podem formar ambientes híbridos. Uma nuvem comunitária pode ser combinada com uma pública, uma privada pode ser combinada com uma pública e outras composições são possíveis. Duas privadas compartilhadas entre organizações, por outro lado, se aproximam do conceito de nuvem comunitária.

O professor retomou algumas vantagens gerais da nuvem: provisionamento mais rápido, implantação simplificada e administração mais fácil. Se a organização optar por trabalhar apenas em nível de plataforma, pode delegar ainda mais responsabilidades ao provedor.

O professor explicou que licenças de software normalmente aparecem embutidas no custo do serviço. Ao comparar uma máquina Linux com uma máquina Windows, por exemplo, a versão Windows tende a custar um pouco mais porque há uma licença associada ao sistema operacional.

O professor ressaltou que a nuvem reduz custos de implantação, manutenção e infraestrutura e oferece muitas vantagens, especialmente para startups e organizações em fase de crescimento. Entretanto, destacou que também existem desvantagens e que, depois que uma aplicação cresce e entra realmente em produção, a conta pode se tornar significativa.

Como exemplo, o professor citou um projeto que rapidamente chegou a cerca de R$ 5.000 por mês. Uma aplicação de produção com banco de dados, três réplicas e mecanismos de backup pode gerar milhares de reais mensais mesmo sem utilizar grande quantidade de armazenamento. Ele estimou que apenas um banco de dados relativamente simples, como PostgreSQL com três réplicas e backups duas vezes por dia, poderia chegar facilmente a algo em torno de R$ 1.000 ou R$ 2.000 mensais, dependendo da configuração.

O professor explicou que existe uma diferença importante entre ambiente de desenvolvimento e produção. No desenvolvimento, pode ser suficiente usar uma única instância pequena. Em produção, são necessários recursos maiores, redundância, backup e outros mecanismos, elevando consideravelmente o custo.

O professor destacou ainda que os provedores cobram por muitos itens diferentes: quantidade de dados enviados, quantidade de dados recebidos, IP fixo, domínio e diversos outros recursos. Por isso, uma fatura de nuvem pode conter dezenas de itens e ser difícil de interpretar. Esse nível de detalhamento e de cobrança pode se tornar um obstáculo para algumas empresas.

Como exemplo de movimento contrário à nuvem, o professor citou um projeto com uma empresa da área de **video analytics**, que utiliza câmeras distribuídas pela cidade. A empresa decidiu retirar parte da infraestrutura da nuvem porque a conta, na faixa de R$ 5.000 a R$ 6.000 por mês, deixou de ser economicamente interessante. Na avaliação da empresa, em aproximadamente dois anos esse valor poderia pagar a compra de um servidor próprio.

O professor explicou que, diante disso, a empresa optou por comprar o servidor e instalá-lo em seu próprio data center. Ao mesmo tempo, adotou uma estratégia híbrida do ponto de vista comercial: quando surgisse um contrato com cliente, a infraestrutura específica daquele cliente poderia ser colocada na nuvem, pois o custo mensal seria incorporado ao próprio contrato. Assim, se a infraestrutura de um cliente gerasse R$ 700 mensais de custo, esse valor seria previsto na contratação.

Por outro lado, para ambientes internos de desenvolvimento e homologação, a empresa considerava a nuvem cara demais. O professor utilizou esse exemplo para mostrar que as organizações podem fazer movimentos de ida e volta entre infraestrutura própria e nuvem conforme custos, requisitos e modelo de negócio.

## 9. Casos de uso de computação em nuvem

O professor explicou que alguns cenários de uso da nuvem já se tornaram clássicos, e um dos mais representativos é o **streaming de vídeo**. Ele comentou que provedores de Internet que atendem determinada quantidade mínima de usuários podem estabelecer parcerias com a Netflix para receber servidores de cache instalados dentro de sua própria infraestrutura.

Segundo o professor, a Netflix pode enviar ao provedor um equipamento fechado, que é instalado no data center local e configurado para armazenar temporariamente conteúdos muito consumidos pelos clientes daquele provedor. À medida que determinados filmes e séries são assistidos com frequência, esses conteúdos ficam disponíveis no cache local.

O professor explicou que essa estratégia traz duas vantagens imediatas para o usuário: reduz a latência e melhora a percepção de velocidade e qualidade do serviço. Além disso, reduz o volume de tráfego que precisa atravessar redes externas do provedor.

Para explicar essa economia, o professor descreveu a Internet como uma estrutura hierárquica de provedores. Existem provedores pequenos e regionais, provedores com atuação nacional e provedores capazes de transportar tráfego internacional. Ele comentou que a nomenclatura dos níveis pode variar e que não tinha certeza, naquele momento, sobre a numeração exata dos níveis, mas o princípio hierárquico era o ponto relevante.

Como exemplo, o professor mencionou um pequeno provedor chamado Pop Telecom, utilizado no prédio onde mora. Segundo ele, trata-se de uma empresa com cobertura limitada a determinados bairros e possivelmente algumas cidades, responsável principalmente por fornecer a “última milha” de fibra óptica ao cliente final.

O professor explicou que um provedor desse porte não possui, sozinho, toda a conectividade nacional e internacional necessária para chegar a qualquer destino na Internet. Por isso, contrata conectividade de um provedor maior, como a Brisanet no exemplo dado em aula. Esse provedor maior, por sua vez, também precisa contratar conectividade de operadores com acesso às rotas internacionais.

O professor exemplificou o caminho de uma comunicação entre um cliente local e um servidor no Japão. O tráfego pode sair do pequeno provedor, passar por uma operadora de alcance nacional, seguir para um provedor internacional e atravessar as redes necessárias até chegar ao Japão. No destino, pode ocorrer o processo inverso, passando por provedores de diferentes níveis até atingir o servidor final. Essa hierarquia é parte do funcionamento da Internet.

O professor explicou que os contratos entre provedores envolvem capacidade de tráfego. Assim, se os clientes de um pequeno ISP estiverem assistindo intensamente à Netflix, grande quantidade de dados precisará percorrer essas redes e consumir a capacidade contratada. Se o conteúdo puder ser atendido por um cache localizado dentro do próprio provedor, esse tráfego externo é reduzido.

O professor destacou que uma parcela muito grande do tráfego atual da Internet já é composta por streaming, citando uma estatística aproximada de 60%. Esse volume inclui YouTube, Netflix, Prime e outros serviços semelhantes. A tendência, segundo ele, é crescer ainda mais porque muitas pessoas estão substituindo a televisão tradicional por satélite por serviços transmitidos via IP. Assim, tráfego que antes passava por uma infraestrutura de TV passa a utilizar a rede de dados.

O professor explicou que o cache local da Netflix não consegue armazenar todo o catálogo. Conteúdos muito pouco assistidos ainda precisarão ser buscados remotamente. Entretanto, itens populares — como os conteúdos presentes no “Top 10” da Netflix Brasil — provavelmente estarão disponíveis localmente. Isso reduz o tráfego do provedor e melhora a experiência do cliente.

O professor ressaltou que a própria Netflix também se beneficia. Em vez de milhões de clientes acessarem diretamente os servidores centrais e gerarem enorme volume de tráfego pago pela própria empresa, parte das solicitações é atendida pelos caches distribuídos. Dessa forma, tanto a Netflix quanto o provedor economizam recursos, enquanto o usuário obtém melhor desempenho. O professor caracterizou essa estratégia como uma situação de ganho para todas as partes.

O professor explicou que **jogos online** utilizam princípio semelhante. Muitos jogos mantêm servidores em diferentes localidades e permitem que o jogador escolha aquele com menor latência. Normalmente, verifica-se o *ping* e escolhe-se o servidor mais próximo ou com menor tempo de resposta. A nuvem facilita esse modelo porque permite criar e distribuir servidores em diversos locais geográficos.

Como observação pessoal, o professor comentou que a escolha do servidor nem sempre depende apenas da latência. Ele relatou que jogava *World of Warcraft* aproximadamente vinte anos antes e que, em determinados momentos, jogadores evitavam servidores com muitos brasileiros por questões relacionadas ao comportamento da comunidade, lotação ou coincidência de horários de pico. Assim, às vezes era escolhido um servidor um pouco mais distante, desde que a latência continuasse aceitável.

O professor citou a **NASDAQ** como outro caso de uso. Segundo ele, a bolsa norte-americana armazena dados históricos na Amazon. Quando alguém solicita, por exemplo, o histórico de variação de uma empresa em determinado período, esses dados podem estar hospedados na infraestrutura da AWS.

O professor retomou também a Netflix e explicou que ela utiliza amplamente o **Amazon CloudFront**, serviço de CDN da AWS. Ele descreveu o CloudFront como um grande sistema de cache distribuído. Quando um usuário solicita uma página, um arquivo ou um vídeo, o serviço identifica uma cópia disponível mais próxima daquele usuário e o redireciona para ela.

O professor explicou que o CloudFront opera justamente em locais de borda, conceito apresentado anteriormente. A função é aproximar o conteúdo do consumidor e evitar que todas as requisições precisem chegar à região central onde os dados foram originalmente armazenados.

O professor mencionou ainda o **Spotify**, que durante bastante tempo utilizou serviços da Amazon e posteriormente migrou parte de sua infraestrutura para o Google. Ele observou que existe competição constante entre os grandes provedores e que empresas de grande porte podem trocar de plataforma quando outro fornecedor oferece condições técnicas ou comerciais mais interessantes.

Por fim, o professor citou a **Guanabara** como outro caso de sucesso, afirmando que o sistema de venda de passagens da empresa utiliza infraestrutura da Amazon. Ele comentou que existem diversos outros *cases* disponíveis para quem quiser analisar de forma mais concreta como diferentes organizações utilizam computação em nuvem.

## 10. Falhas e indisponibilidade em serviços de nuvem

O professor explicou que, apesar de todas as vantagens apresentadas — provisionamento sob demanda, elasticidade, redução de custos e facilidade de expansão —, a nuvem também falha. Existem diversos casos documentados de indisponibilidade em grandes provedores e serviços.

O professor mencionou um incidente de 2017 em que uma falha significativa da Amazon permaneceu por bastante tempo e impactou diversos serviços e empresas, entre eles Reddit, Adobe, Spotify, Netflix e Airbnb. Também citou um incidente envolvendo a Netflix em 2011, sobre o qual a própria empresa publicou posteriormente aprendizados e mudanças de arquitetura.

O professor ressaltou que praticamente todos os anos surgem novos problemas em serviços de nuvem. A confiabilidade melhorou ao longo do tempo, mas falhas continuam fazendo parte da operação. Para ele, o caso da Netflix foi particularmente interessante porque a empresa analisou o incidente, identificou o que precisava mudar em sua infraestrutura e compartilhou publicamente os aprendizados. Ele recomendou a leitura desse material caso o link presente nos slides ainda estivesse ativo.

O professor citou também um incidente do Dropbox em 2014. Para mostrar que o problema não se limita a casos antigos, apresentou um site dedicado a registrar incidentes e destacou uma lista com as dez principais falhas de 2025.

Segundo o professor, a lista mostrava muitos incidentes relevantes. Ele mencionou problemas no Azure em janeiro e outubro, um incidente da Amazon em outubro, um problema da OpenAI em novembro e episódios envolvendo serviços como ChatGPT e Cloudflare.

O professor explicou que, nos Estados Unidos, empresas são obrigadas a divulgar determinados incidentes. Por isso, ao abrir os registros, normalmente é possível encontrar um resumo no site que acompanha as falhas e, em algum ponto, um link para o relatório ou comunicado oficial da própria empresa. Nesses documentos, o provedor costuma detalhar o que ocorreu — por exemplo, uma falha em DNS — e quais serviços foram impactados.

O professor observou que não havia feito ainda uma seleção equivalente para 2026, inclusive porque ainda existiriam muitos eventos ao longo do ano capazes de gerar grandes picos de demanda, como a Black Friday. Segundo ele, ainda haveria bastante oportunidade para novos incidentes acontecerem.

O professor explicou que falhas menores acontecem com muita frequência. No painel de status da Amazon, por exemplo, é comum encontrar algum serviço em manutenção ou apresentando algum tipo de indisponibilidade. A lista de “top 10” mostrada em aula, entretanto, estava focada em eventos suficientemente grandes para impactar clientes de maneira relevante e virar notícia.

O professor ressaltou que esses não eram incidentes triviais de poucos minutos sem consequências. Em muitos casos, empresas deixaram de faturar grandes quantias porque um serviço essencial ficou indisponível por determinado período. Por isso, compreender que a nuvem também falha é fundamental para projetar arquiteturas resilientes.

## 11. Introdução à AWS e ao mercado de nuvem

O professor explicou que faria uma introdução rápida aos principais serviços da Amazon para nivelar a turma, já que a AWS seria o estudo de caso utilizado na disciplina. Ele ressaltou, porém, que os conceitos apresentados não ficam restritos à Amazon. Mesmo que futuramente um profissional trabalhe com Azure, Google Cloud, Oracle Cloud ou outro provedor, grande parte dos conceitos será a mesma e, em muitos casos, até os nomes dos serviços ou das categorias são semelhantes. O que muda principalmente é a interface, a localização das opções e alguns detalhes de implementação.

Como exemplo, o professor citou o **IAM**, relacionado ao gerenciamento de identidades e autorizações. Segundo ele, todos os grandes provedores possuem algum serviço equivalente de identidade e acesso, e o nome acabou se tornando amplamente utilizado porque a Amazon exerceu grande influência sobre o mercado.

O professor explicou a diferença entre um padrão formal e um **padrão de fato**. Um padrão formal é definido em documentação ou especificação oficial; um padrão de fato surge porque uma implementação se torna tão dominante que os demais participantes do mercado passam a imitá-la para manter compatibilidade.

Segundo o professor, muitas APIs da Amazon se tornaram padrões de fato. Ele citou como exemplo APIs para operações de armazenamento de arquivos. Se um novo provedor quiser conquistar clientes da AWS, uma estratégia é implementar uma interface compatível com a API utilizada pela Amazon. Dessa forma, o cliente pode migrar sem reescrever grande parte do código: basta trocar o *endpoint* para apontar para o novo serviço. Para o professor, esse é um exemplo claro de tecnologia que se transforma em padrão pela força da adoção do mercado.

O professor apresentou então dados de **market share**. No material mostrado em aula, a Amazon aparecia com aproximadamente 28% do mercado, embora já tivesse possuído participação muito maior, em torno de 60% em períodos anteriores. Ele explicou que a Microsoft cresceu bastante depois que Satya Nadella direcionou fortemente a empresa para serviços em nuvem.

O professor comentou que o Google também construiu uma posição relevante, especialmente oferecendo como serviço tecnologias nas quais já possuía grande experiência. Segundo ele, o Google demorou um pouco mais para entrar de forma agressiva no mercado de nuvem e, ao planejar sua estratégia, teria partido da pergunta: **em quais áreas a empresa já é muito boa?**

Entre essas áreas, o professor citou streaming de vídeo, por causa do YouTube; mapas; busca; armazenamento e outros serviços já dominados pela empresa. A partir daí, o Google passou a disponibilizar essas capacidades para terceiros. Assim, quem deseja construir um serviço com características semelhantes às de produtos do Google pode contratar componentes da própria infraestrutura da empresa.

O professor utilizou o **Google Translate** como exemplo de um serviço que já foi muito utilizado diretamente e incorporado a sites por meio de plugins. Antes da popularização dos modelos generativos, praticamente todo mundo recorria ao Google Translate para traduzir conteúdo. Muitos sites exibiam um componente no topo da página permitindo selecionar o idioma. Posteriormente, essa função passou a ser incorporada diretamente aos navegadores, como o Chrome, que pode traduzir automaticamente páginas inteiras.

O professor explicou que também existem grandes nuvens chinesas, mencionando a Alibaba Cloud como exemplo. O mercado chinês é mais fechado, e muitos serviços estrangeiros enfrentam restrições de acesso. Como consequência, empresas locais possuem um mercado interno enorme e relativamente protegido.

Segundo o professor, isso torna algumas estatísticas do mercado chinês menos visíveis para quem está fora do país. Ele observou que a escala interna provavelmente é gigantesca e relacionou esse cenário ao avanço chinês em IA generativa. Na visão apresentada, a China é um dos poucos ambientes capazes de competir diretamente com empresas norte-americanas nessa área justamente por possuir um mercado de bilhões de pessoas e grande capacidade de investimento.

O professor explicou que, quando serviços estrangeiros são bloqueados, alguns usuários podem tentar contornar as restrições com VPNs, assumindo os riscos envolvidos, mas grande parte da população simplesmente adota as soluções locais. Isso cria um mercado cativo que facilita o crescimento das empresas nacionais. Informações financeiras também nem sempre ficam facilmente disponíveis até que uma empresa precise abrir seus dados para entrar em uma bolsa de valores ou captar recursos em determinados mercados.

O professor mencionou o TikTok como exemplo de empresa chinesa submetida a pressões regulatórias nos Estados Unidos. Ele comentou que, até pouco tempo antes da aula, existiam discussões sobre a necessidade de vender parte da operação norte-americana para evitar um bloqueio, com várias empresas interessadas na aquisição. Ele ressaltou que não havia acompanhado o desfecho mais recente daquela discussão, utilizando o caso apenas para exemplificar a tensão entre mercado, regulação e soberania tecnológica.

O professor voltou então ao mercado de nuvem e destacou que, apesar da concorrência, a Amazon ainda aparecia em primeiro lugar nos dados mostrados. Também chamou atenção para o crescimento contínuo do setor. Mesmo com pandemia e outros problemas econômicos, o mercado de nuvem não apresentou, no gráfico exibido, anos de retração total.

O professor explicou que a taxa percentual de crescimento pode estar diminuindo em relação aos primeiros anos, mas o mercado continua aumentando em valores absolutos. Segundo ele, a IA tende a ampliar ainda mais essa expansão.

Para o professor, uma diferença importante é que, antigamente, muitos serviços em nuvem eram utilizados principalmente por pessoas ligadas à tecnologia. Hoje, com IA generativa, qualquer profissional pode consumir recursos de nuvem indiretamente. Publicitários, advogados, juízes, médicos e pessoas de praticamente qualquer área passaram a utilizar ferramentas baseadas em IA.

O professor ressaltou ainda que aplicações de IA consomem muitos recursos computacionais. Assim, o mercado cresce tanto pela expansão do número de usuários quanto pelo volume de infraestrutura necessário para executar esses modelos. Ao mesmo tempo, as empresas que disputam esse mercado também gastam enormes quantias em data centers, GPUs e treinamento de modelos.

O professor comentou ter visto análises segundo as quais o maior risco financeiro não estaria necessariamente nas empresas que vendem infraestrutura, como fabricantes de GPUs, mas nas empresas que estão investindo pesadamente para competir diretamente em IA. Ele mencionou uma disputa entre OpenAI, Anthropic, Google, Microsoft e Meta.

Na explicação do professor, algumas empresas podem acabar desistindo de desenvolver modelos próprios em escala máxima e optar por parcerias ou acordos com quem se consolidar no mercado. Ele comentou que a Microsoft investiu fortemente na OpenAI e integrou a tecnologia aos próprios produtos. Para ele, é possível que, no futuro, apenas três ou quatro grandes plataformas dominem o núcleo desse mercado, enquanto outras empresas adotem as tecnologias produzidas por elas.

O professor ressaltou que, independentemente de qual empresa vença essa disputa, por trás dos serviços de IA existem infraestruturas de nuvem. Quando um usuário envia uma mensagem ao ChatGPT, anexa um PDF ou utiliza um agente de IA integrado ao VS Code, há tráfego de rede, APIs e processamento remoto envolvidos.

Como exemplo, o professor descreveu um agente de IA que recebe a tarefa de limpar uma pasta de downloads e remover arquivos duplicados. O agente pode coletar informações na máquina local, enviar uma requisição para um serviço remoto, receber um script gerado, executá-lo localmente e depois realizar novas chamadas. Assim, essas aplicações utilizam serviços de rede e de nuvem continuamente.

### 11.1. Origem da AWS

O professor explicou que a história da AWS está ligada à evolução da própria Amazon. A empresa começou vendendo livros e realizando entregas. Com o tempo, passou a vender produtos de terceiros e se tornou uma das principais plataformas de comércio eletrônico dos Estados Unidos.

O professor destacou que a Amazon cresceu de uma operação iniciada em uma garagem para grandes galpões, centros de distribuição e uma infraestrutura tecnológica capaz de sustentar o comércio eletrônico em larga escala. Essa infraestrutura precisava ser dimensionada para os períodos de maior demanda.

Segundo o professor, a empresa percebeu que possuía grande quantidade de máquinas justamente porque precisava garantir capacidade para momentos como Black Friday, Dia dos Pais, Dia das Mães, Valentine’s Day, Natal e outras datas de pico. Durante boa parte do restante do ano, parte significativa dessa infraestrutura ficava ociosa.

O professor explicou que foi a partir dessa percepção que surgiu a ideia de disponibilizar os recursos excedentes para terceiros. Em vez de deixar máquinas paradas durante os períodos de menor movimento, a Amazon poderia alugá-las. Ele situou esse “estalo” no período de surgimento dos primeiros serviços da AWS e explicou que essa lógica levou à criação de serviços de computação sob demanda.

O professor explicou que, à medida que a Amazon começou a construir data centers e oferecer capacidade computacional a terceiros, o negócio de nuvem cresceu muito e passou a gerar receita comparável ou, em determinados momentos, superior à própria operação de venda de produtos. A empresa então se consolidou também como fornecedora de serviços de nuvem de diferentes tipos.

O professor retomou os modelos de serviço para mostrar que os grandes provedores conseguem oferecer várias camadas. É possível fornecer infraestrutura instalada *on-premises*, Infraestrutura como Serviço, Plataforma como Serviço e Software como Serviço. Em IaaS, o cliente gerencia aplicações, sistema operacional e parte da configuração, enquanto rede física, virtualização e infraestrutura de data center ficam com o provedor. Em PaaS, o cliente se concentra principalmente em aplicações e dados. Em SaaS, o usuário apenas consome o serviço.

### 11.2. Ambiente local versus AWS

O professor explicou que gosta de comparar um ambiente local com a AWS porque essa equivalência ajuda as organizações a compreender como uma migração pode funcionar. Muitas empresas se sentem mais confortáveis com a nuvem quando percebem que componentes conhecidos da infraestrutura tradicional possuem equivalentes no ambiente virtual.

Como exemplo, se uma organização possui dez máquinas executando serviços em um data center local, ela procurará recursos semelhantes na nuvem. Se possui regras de firewall, buscará um mecanismo equivalente. Se possui uma rede física, construirá uma rede virtual. Se possui usuários e mecanismos de autenticação, utilizará um serviço de identidade correspondente.

O professor explicou que, na Amazon, a infraestrutura de rede virtual é criada com a **VPC (Virtual Private Cloud)**. Dentro dela, o usuário pode definir roteadores, switches virtuais, sub-redes e outras características, modelando a rede de acordo com suas necessidades.

O professor explicou que máquinas anteriormente executadas localmente com tecnologias de virtualização como KVM, Xen ou VirtualBox podem ser migradas para serviços de máquinas virtuais da Amazon. Contêineres possuem serviços específicos. Bancos de dados e armazenamento também possuem equivalentes gerenciados.

Como exemplos, o professor citou o **RDS** para bancos de dados relacionais e o **S3** para armazenamento de arquivos e objetos. Também mencionou **Elastic File System** e **Elastic Block Store**. Ao criar uma máquina, é possível associar armazenamento em bloco e, se o espaço de 100 GB deixar de ser suficiente, aumentar esse volume para 200 GB sem precisar substituir fisicamente um disco.

O professor explicou que os provedores foram construindo diferentes camadas equivalentes às estruturas encontradas em data centers tradicionais. Relembrou também que a AWS organiza sua infraestrutura em regiões e zonas de disponibilidade, interligadas por rede de alta velocidade e separadas em aspectos como energia, conectividade e proteção contra incidentes físicos.

### 11.3. Principais serviços AWS

O professor apresentou alguns dos principais serviços da Amazon. Para criação de máquinas virtuais, mencionou o serviço de computação correspondente ao EC2, referido durante a fala como “S2”. Para contêineres, a Amazon também oferece serviços específicos.

Para armazenamento, o professor destacou o **S3**, cujo nome foi apresentado como *Simple Storage Service*. O serviço permite armazenar arquivos e objetos. Para bancos de dados relacionais, citou o **RDS**; para bancos não relacionais ou NoSQL, mencionou o **DynamoDB**.

O professor destacou novamente o **IAM** como um serviço especialmente importante para a disciplina de segurança, pois é utilizado para gerenciamento de usuários, identidades, funções e permissões de acesso.

Também citou o **CloudWatch**, voltado ao monitoramento personalizável de recursos em nuvem, e uma solução de implantação de aplicações em nível de plataforma, na qual o desenvolvedor entrega uma aplicação Java ou de outra linguagem suportada e solicita que o provedor a execute e gerencie a infraestrutura subjacente.

O professor retomou o **CloudFront** para distribuição de conteúdo e o **VPC** para criação da infraestrutura de rede virtual, incluindo firewall, redes virtuais, switches e roteadores.

O professor observou que a imagem exibida nos slides era antiga e já estava muito diferente da interface atual da Amazon. O número de serviços cresceu tanto que atualmente não seria possível mostrar tudo em uma única captura de tela; seria necessário separar por categorias, como computação, armazenamento, análise de dados e IA.

Ele comentou que, no material antigo, recursos de IA apareciam apenas de forma limitada, muitas vezes dentro de categorias como *analytics* ou *machine learning*. Atualmente, a própria área de IA já ocupa uma quantidade significativa de serviços e opções.

Por fim, o professor explicou que qualquer pessoa pode criar uma conta particular na AWS, cadastrar um cartão de crédito e utilizar o console da Amazon. A plataforma oferece uma camada gratuita por determinado período e alguns serviços continuam possuindo cotas gratuitas mesmo depois disso. Ele alertou, entretanto, que não confiaria cegamente na ideia de “gratuito”, pois é necessário acompanhar cuidadosamente as cotas e o consumo para evitar cobranças inesperadas.

## 12. AWS Academy e ambiente de laboratório

O professor explicou que um dos motivos para a disciplina utilizar a Amazon como estudo de caso é a disponibilidade do **AWS Academy**. A UFC participa desse programa e possui uma conta que permite criar turmas e fornecer créditos aos alunos para atividades práticas.

Segundo o professor, cada aluno receberia aproximadamente US$ 50 de crédito por turma. Como haviam sido criadas duas turmas para a disciplina, os alunos teriam créditos separados em cada uma delas.

O professor explicou que seriam utilizados dois ambientes principais. Um deles é o **Learner Lab**, no qual o aluno possui maior liberdade para criar recursos e experimentar a plataforma. O outro é o **Cloud Security**, que apresenta atividades mais específicas relacionadas a segurança, como gerenciamento de permissões e configurações definidas previamente.

O professor explicou que o Learner Lab é mais aberto para experimentação: o aluno pode criar máquinas e manipular diferentes recursos. Entretanto, tanto o Learner Lab quanto o AWS Academy possuem limitações importantes, principalmente em relação ao **IAM**.

Segundo o professor, essas restrições existem por questões de segurança da própria Amazon. O ambiente utilizado pelos alunos funciona como um **sandbox**, isto é, as ações realizadas ficam confinadas dentro de um espaço controlado e não podem se propagar livremente para outros ambientes. Como a Amazon oferece contas educacionais com créditos gratuitos, precisa utilizar fortemente mecanismos de IAM para impedir abusos.

O professor explicou que, por esse motivo, determinadas operações que seriam permitidas em uma conta pessoal não estão disponíveis no Academy. No Learner Lab, por exemplo, o aluno não consegue criar livremente novos usuários de IAM. Existe um usuário padrão fornecido pelo ambiente. Em uma conta pessoal, por outro lado, o proprietário poderia criar dezenas ou centenas de usuários, de acordo com sua necessidade.

O professor explicou que a limitação do Learner Lab não impediria a prática dos conceitos de identidade e acesso. Em algumas atividades, a própria Amazon disponibilizaria previamente várias contas ou identidades e apresentaria tarefas como permitir que determinado conjunto de usuários execute certas ações enquanto outro conjunto não pode executá-las. Assim, a turma conseguiria praticar controle de acesso mesmo sem criar usuários do zero.

### 12.1. Primeiro acesso ao laboratório

O professor explicou que, embora não houvesse tempo naquela aula para explorar a maioria dos serviços, queria garantir que todos conseguissem realizar o primeiro acesso ao ambiente.

Ao entrar no site fornecido pelo AWS Academy e abrir a área de cursos, os alunos deveriam encontrar o **Learner Lab** e o **Cloud Security**. O professor mostrou a interface a partir da visualização de aluno para que a turma enxergasse praticamente a mesma tela que ele estava utilizando.

O professor explicou que o menu lateral permite acessar os módulos e outras partes do curso. Na interface também aparece o limite de orçamento disponível, em torno de US$ 50. Ao abrir os módulos, o aluno encontra diferentes materiais de apoio.

O professor recomendou assistir aos vídeos introdutórios disponíveis no ambiente. Segundo ele, um dos vídeos mostra como entrar no ambiente virtual. O conteúdo está em inglês, mas seria possível utilizar legendas para facilitar o acompanhamento.

O professor explicou que, para iniciar efetivamente a prática, o ponto principal era acessar o link que abre o laboratório da AWS. Ao clicar nesse link, um novo ambiente é carregado. Ele mencionou o nome desse ambiente durante a demonstração e destacou que a página apresenta várias instruções importantes.

O professor recomendou que os alunos lessem essas instruções porque ali são informados todos os serviços que podem ou não ser utilizados, além das restrições específicas da conta educacional. A turma havia sido criada para permanecer disponível durante vários meses.

O professor explicou que o guia deve ser consultado sempre que uma tentativa de criação de recurso resultar em erro. Antes de concluir que existe algum problema técnico, o aluno deve verificar se aquele serviço, região ou tipo de recurso está autorizado no AWS Academy.

Como exemplo, o professor explicou que havia restrições sobre as regiões nas quais instâncias EC2 podiam ser criadas. Ao procurar pelas regras de EC2, era possível encontrar a lista de regiões permitidas. Ele observou que uma região no oeste dos Estados Unidos havia sido liberada mais recentemente e não estava disponível quando ministrou a disciplina no ano anterior. Assim, naquele momento, as máquinas podiam ser criadas apenas em determinados locais no leste ou no oeste dos Estados Unidos.

O professor explicou que, para iniciar o ambiente, o aluno deve clicar em **Start Lab**. Enquanto o laboratório está sendo preparado, o indicador aparece em uma cor de espera e, quando fica verde, o ambiente está pronto para uso. Nesse momento, a AWS está criando o sandbox necessário para a sessão.

O professor chamou atenção para dois elementos importantes: o botão do laboratório e o contador de tempo. Cada sessão permanece online por aproximadamente **quatro horas**. Esse limite existe para evitar que um aluno esqueça recursos ligados e consuma todo o crédito disponível.

O professor explicou que, se o aluno terminar uma atividade, sair para almoçar e esquecer o ambiente ligado, ao final das quatro horas o laboratório desligará as máquinas automaticamente. Entretanto, os recursos não são necessariamente apagados; eles são apenas desligados.

Quando o aluno retornar em outro dia e clicar novamente em **Start Lab**, os recursos previamente configurados podem ser religados. Assim, um banco de dados ou uma máquina configurada anteriormente continua existindo. Se o aluno possuir muitos recursos e não quiser continuar consumindo crédito durante a sessão, pode desligá-los manualmente.

O professor explicou também a função de **End Lab**. Esse comando permite encerrar a sessão antes do término automático das quatro horas. Por outro lado, se o aluno estiver no meio de uma atividade e o contador estiver próximo de terminar, pode clicar novamente em **Start Lab** para reiniciar o período de quatro horas.

### 12.2. AWS Details, credenciais e acesso por SSH

O professor explicou que, depois de o laboratório estar conectado, a área **AWS Details** apresenta informações importantes. Entre elas, existe um console de linha de comando que funciona de maneira semelhante a ter o cliente da AWS instalado localmente.

Por meio desse terminal, é possível executar comandos, listar máquinas, verificar status e realizar diversas operações sem utilizar exclusivamente a interface gráfica. O professor comentou que pessoalmente não havia decorado todos os comandos e costumava realizar muitas tarefas clicando no console, mas destacou que a linha de comando é uma alternativa especialmente útil para quem prefere trabalhar no terminal.

O professor explicou que a área AWS Details também apresenta um **token de acesso**. No ambiente do Academy, esse token é renovado aproximadamente a cada quatro horas. Caso o aluno queira utilizar ferramentas da AWS diretamente em sua própria máquina, precisa fornecer essas credenciais ao cliente local.

Segundo o professor, em Linux ou Windows é possível criar, dentro do diretório pessoal, a pasta oculta utilizada pela AWS e o arquivo `credentials`. As informações fornecidas pelo Academy podem ser copiadas para esse arquivo. As ferramentas da AWS procurarão automaticamente as credenciais nesse local para autenticar as requisições.

O professor observou que, no Academy, esse procedimento pode ser um pouco inconveniente porque o token muda a cada quatro horas. Em um ambiente de desenvolvimento real, porém, compreender como as credenciais são configuradas é importante para executar automações e acessar os serviços programaticamente.

O professor explicou que o laboratório também fornece, por padrão, uma credencial para acesso por **SSH** utilizando criptografia de chave pública. O arquivo disponibilizado em formato `.pem` corresponde à chave privada utilizada para autenticação.

Para usuários de Windows que utilizam ferramentas como PuTTY, o professor explicou que o ambiente também fornece uma versão da chave em formato **PPK**, compatível com esse software. Em versões mais recentes do Windows e em sistemas como macOS, é possível utilizar diretamente o comando `ssh`, caso ele esteja disponível, e trabalhar com a chave apropriada.

O professor ressaltou que a chave disponibilizada pelo Academy é apenas uma chave padrão. Posteriormente, seria possível criar outras chaves, e ele mostraria na aula seguinte como fazer isso.

### 12.3. Console AWS e serviços disponíveis

O professor explicou que o console principal da AWS pode ser aberto clicando no nome ou símbolo da AWS indicado em verde no ambiente do Academy. Isso abre outra janela e coloca o aluno dentro do console propriamente dito.

No canto superior da interface, o usuário consegue visualizar informações relacionadas à conta. O professor destacou que, como os alunos estão dentro de um **sandbox**, muitas áreas e operações aparecem como acesso negado por motivos de segurança. Em uma conta pessoal, o nome e as permissões exibidos seriam diferentes.

O professor mostrou também a área de seleção de **região**. Apenas determinadas regiões estavam liberadas para o laboratório. Ele comentou que normalmente deixava selecionada a região do norte da Virgínia por ser uma opção prática para as atividades da disciplina e por estar entre as regiões permitidas.

O professor explicou que o console apresenta acesso a uma grande quantidade de serviços. Algumas áreas exibem ainda informações de saúde ou status do ambiente. Ao abrir o menu de serviços, é possível navegar por categorias como análise de dados, armazenamento, bancos de dados, computação e outras.

Na parte de análise de dados, o professor citou serviços como **Athena**, **CloudSearch**, **Data Exchange** e **EMR**, este último associado a processamento de grandes volumes de dados. Na área de armazenamento, voltou a mencionar o **S3**, utilizado para armazenar arquivos.

Na categoria de bancos de dados, o professor citou **Aurora** e **RDS**. Ele descreveu o Aurora como uma solução relacionada ao MySQL e explicou que, por meio do RDS, é possível executar diferentes bancos relacionais, incluindo PostgreSQL, Oracle e SQL Server, de acordo com as opções suportadas.

O professor mencionou também bancos não relacionais. Para armazenamento de documentos como XML ou JSON, citou um serviço compatível com MongoDB. Também apresentou o **DynamoDB** como banco de dados não relacional orientado a chave e valor e comparou esse modelo ao Redis, destacando que é útil para armazenar dados acessados a partir de identificadores únicos.

O professor explicou que a turma passaria por esses serviços ao longo das aulas, mas que o primeiro foco de computação seria o serviço utilizado para criação de máquinas virtuais. Ele chamou atenção para a grande quantidade de serviços existentes no console.

O professor mostrou ainda o **IAM**, responsável pela gestão de usuários e permissões. Embora os alunos pudessem abrir parte da interface, o sandbox não permitiria executar todas as operações. O objetivo naquele momento era apenas observar como o serviço é organizado.

### 12.4. IAM: funções, usuários, políticas e grupos

O professor explicou que o ambiente já possuía várias **funções** (*roles*) criadas por padrão. Ele descreveu uma *role* como uma espécie de “chapéu” de autorização que define qual conjunto de permissões está ativo em determinado contexto.

Como exemplo, o professor explicou que um profissional pode assumir uma função de monitoramento de rede que permita apenas visualizar determinadas informações. Se uma automação tiver como objetivo somente monitorar recursos, não há motivo para ela possuir permissão para remover, criar ou alterar componentes. Uma *role* somente de leitura reduz o risco de uma falha no script causar danos maiores.

O professor destacou que essa é uma aplicação prática do princípio de menor privilégio. Ao limitar o script ou usuário às permissões estritamente necessárias, evita-se que um erro ou comprometimento resulte em ações que aquela identidade nunca deveria realizar.

O professor mostrou que, no Academy, existia apenas o usuário padrão e que a criação de usuários adicionais estava bloqueada. As **políticas** seriam explicadas com mais profundidade nas aulas seguintes. Em uma conta completa, o IAM é o local utilizado para criar usuários, funções, políticas e grupos.

O professor explicou que regras relacionadas a troca de senha também são administradas nessa área. Se uma organização quiser exigir troca de senha a cada dois meses, por exemplo, esse tipo de política é configurado no sistema de identidade. O mesmo vale para requisitos de autenticação de dois fatores.

O professor explicou que o IAM permite ainda criar **grupos** com diferentes perfis. Um grupo de administradores pode receber permissões para criar, excluir e modificar praticamente qualquer recurso. Já um grupo de desenvolvimento pode ser configurado para não criar máquinas virtuais, mas ainda visualizar as instâncias existentes, consultar seus endereços IP e conectar-se a elas por SSH.

Assim, em vez de configurar cada pessoa individualmente, os usuários podem ser adicionados aos grupos correspondentes à sua função. O professor informou que esses conceitos seriam explorados mais profundamente na aula seguinte.

### 12.5. VPC, sub-redes e tabela de rotas

O professor apresentou também a **VPC (Virtual Private Cloud)**, embora tenha observado que o assunto é mais complexo e provavelmente não haveria tempo para aprofundá-lo naquela primeira aula.

O professor explicou que uma VPC representa a rede virtual na AWS. Como analogia, lembrou que a rede Wi-Fi de um laboratório possui elementos como endereço IP, roteador, máscara de rede e DNS. Na nuvem, a VPC é o primeiro local em que uma estrutura equivalente começa a ser configurada.

O professor mostrou que o Academy já havia criado uma VPC padrão na região do norte da Virgínia. Também havia diversas **sub-redes** criadas automaticamente. Segundo ele, essa distribuição estava relacionada às zonas de disponibilidade existentes naquela região.

Para explicar o conceito de sub-rede, o professor propôs imaginar uma rede maior, como `10.0.0.0/16`, sendo dividida em partes menores e distribuída entre diferentes locais. Essas partes passam a funcionar como sub-redes interligadas dentro da VPC.

O professor mostrou um exemplo de rede cujo bloco aparecia na interface como algo semelhante a `172.31.0.0/16`. A partir desse bloco principal, a AWS já havia criado várias sub-redes e também uma **tabela de rotas**. Cada um desses componentes pode ser aberto e analisado individualmente.

O professor mencionou ainda outros elementos exibidos na VPC, como o bloco CIDR, recursos de fluxo e configurações relacionadas à rede, mas informou que tudo isso seria explorado com mais calma na aula seguinte.

O professor recomendou que os alunos utilizassem os vídeos e módulos disponíveis no AWS Academy para revisar a interface e os conceitos. Esses materiais explicam os botões, o funcionamento do laboratório e diferentes recursos do ambiente.

Naquele momento, o objetivo principal era garantir que todos já tivessem acesso ao ambiente e soubessem onde encontrar os materiais, iniciar e encerrar o laboratório, visualizar credenciais e navegar pelo console. Ele recomendou que os alunos assistissem a alguns dos vídeos disponíveis antes do próximo encontro.
