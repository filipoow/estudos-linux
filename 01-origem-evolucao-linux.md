# A origem e evolução do Linux e seu modelo de desenvolvimento colaborativo

## O ponto de partida: um estudante, um sistema didático e uma frustração

Para entender o Linux é preciso voltar antes dele, até o Unix, criado nos anos 60 e 70 nos laboratórios Bell. O Unix era robusto, mas caro e fechado. Nos anos 80, o professor Andrew Tanenbaum criou o Minix, um sistema operacional pequeno e simples, pensado exclusivamente para ensinar como um sistema operacional funciona por dentro em cursos de universidade. O Minix vinha junto de um livro, "Operating Systems: Design and Implementation", que se tornou leitura obrigatória para quem queria entender sistemas operacionais na prática.

Um desses leitores era Linus Torvalds, um estudante de 21 anos da Universidade de Helsinque, na Finlândia. Ele usava o Minix, mas esbarrava nas limitações que o sistema tinha de propósito, já que fora desenhado para ensino e não para uso real. Torvalds queria algo mais parecido com um Unix completo, que ele pudesse usar no seu próprio computador, um 386.

Entre abril e agosto de 1991, Torvalds foi escrevendo, sozinho, as primeiras peças de um kernel novo: um trocador de tarefas em assembly para o processador 386 e um driver de terminal. Em 25 de agosto de 1991, ele publicou uma mensagem que hoje é histórica no grupo de discussão comp.os.minix, dizendo algo como: "Estou fazendo um sistema operacional (gratuito) para clones 386(486) AT, só um hobby, não vai ser grande nem profissional como o GNU". A ironia é que se tornou justamente isso, grande e profissional, embora tenha começado como ele descreveu.

Em setembro de 1991 saiu a versão 0.01, com pouco mais de dez mil linhas de código. Era um núcleo funcional, mas muito limitado. O ponto de virada foi a licença: Torvalds decidiu liberar o código sob a GNU General Public License (GPL), o que significava que qualquer pessoa podia ler, modificar e redistribuir aquele código, desde que mantivesse a mesma liberdade para quem viesse depois.

## Por que "Linux" e não só um kernel qualquer

Uma confusão comum é achar que o Linux é um sistema operacional completo criado do zero. Na prática, o Linux é o kernel, a parte central que fala diretamente com o hardware, gerencia processos, memória e dispositivos. Um kernel sozinho não forma um sistema operacional usável. O que tornou o Linux utilizável desde o início foi o encontro com as ferramentas do Projeto GNU (compilador, shell, editores, bibliotecas), que já existiam havia anos mas ainda não tinham um kernel livre para rodar por cima. Esse detalhe importa tanto que existe até hoje um debate sobre chamar o sistema de "Linux" ou "GNU/Linux", e vale a pena entender essa história com mais calma no arquivo sobre [software livre e a GPL](02-software-livre-gpl.md).

## Um modelo de desenvolvimento diferente de tudo que existia

O que fez o Linux crescer tão rápido não foi só a técnica, foi o método. Antes da internet ser algo comum, desenvolver software em conjunto, com gente espalhada pelo mundo, era quase inviável. Torvalds aproveitou o acesso a redes acadêmicas para publicar o código e convidar qualquer pessoa interessada a testar, encontrar bugs e mandar melhorias.

O modelo funcionava (e ainda funciona) mais ou menos assim: alguém escreve uma modificação, chamada de patch, e envia para uma lista de discussão pública. Outros desenvolvedores revisam esse patch, apontam problemas, sugerem ajustes. Se o patch for aceito, ele sobe até um mantenedor daquele subsistema (por exemplo, o subsistema de rede ou o de sistemas de arquivos), e desse mantenedor o código eventualmente chega até Torvalds, que ainda hoje aceita as mudanças finais para cada nova versão do kernel.

Esse fluxo de trabalho baseado em patches por e-mail, sem depender de uma empresa dona da infraestrutura, é tão central para a cultura do projeto que continua sendo usado até hoje, mesmo com todas as ferramentas modernas disponíveis. Aliás, uma dessas ferramentas nasceu direto dessa necessidade: em 2005, o projeto ficou sem poder usar o sistema de controle de versão comercial que vinha utilizando (o BitKeeper), depois de um desentendimento sobre licenciamento com a empresa por trás dele. Torvalds, então, escreveu em poucos dias uma ferramenta nova, pensada para lidar com desenvolvimento distribuído em grande escala. Essa ferramenta é o Git, hoje o sistema de controle de versão mais usado no mundo, muito além do próprio Linux.

## De hobby a infraestrutura global

O Linux saiu de dez mil linhas de código escritas por uma pessoa para um kernel com dezenas de milhões de linhas, mantido por milhares de desenvolvedores de centenas de empresas diferentes, incluindo gigantes como Intel, Google, Red Hat e Samsung. Esse crescimento sustentado por décadas só foi possível porque o modelo colaborativo aberto permite que qualquer organização com interesse técnico contribua diretamente, sem precisar da permissão de um dono único do software. Isso está detalhado no arquivo sobre [comunidades de desenvolvedores](03-comunidades-desenvolvedores.md).

## Fontes

- [The Beginning of the Linux Open-Source Operating System](https://www.historyofinformation.com/detail.php?id=1668)
- [Linux kernel mailing list, Wikipédia](https://en.wikipedia.org/wiki/Linux_kernel_mailing_list)
- [Git turns 20: a Q&A with Linus Torvalds, GitHub Blog](https://github.blog/open-source/git/git-turns-20-a-qa-with-linus-torvalds/)
- [A Git Origin Story, Linux Journal](https://www.linuxjournal.com/content/git-origin-story)
- [List of maintainers and how to submit kernel changes, kernel.org](https://docs.kernel.org/process/maintainers.html)
