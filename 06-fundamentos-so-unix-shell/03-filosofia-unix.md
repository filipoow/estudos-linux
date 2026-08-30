# A filosofia Unix e o conceito de tratar tudo como um arquivo

## Um conjunto de princípios, não uma regra técnica

Diferente de uma especificação técnica formal, a filosofia Unix é um conjunto de princípios de design, registrados por escrito pela primeira vez por Doug McIlroy, um dos pesquisadores dos Bell Labs, num artigo de 1978. Esses princípios não foram impostos de cima para baixo, eles emergiram do jeito como Thompson, Ritchie e os demais criadores do Unix, apresentado no [arquivo anterior](02-historia-influencia-unix.md), preferiam construir software. E se mostraram tão eficazes que continuam guiando o design de sistemas até hoje, Linux incluído.

## Os três pilares centrais

A formulação mais conhecida da filosofia Unix se resume a três ideias:

**Escreva programas que fazem uma coisa só, e façam bem feito.** Em vez de um programa gigante tentando resolver vários problemas ao mesmo tempo, a preferência é por ferramentas pequenas e focadas. É exatamente o padrão visto em comandos já apresentados neste repositório: `grep` só procura texto, `sort` só ordena, `head` só mostra o início de um arquivo. Cada um faz uma única coisa, e faz bem.

**Escreva programas que trabalhem juntos.** Ferramentas pequenas e isoladas só se tornam poderosas quando podem ser combinadas. É esse princípio que justifica a existência dos pipes, apresentados em detalhe no arquivo sobre [redirecionamento e pipes](../04-pacotes-scripts-automacao/02-redirecionamento-pipes.md): encadear a saída de um comando simples na entrada do próximo permite construir soluções complexas a partir de peças simples, em vez de depender de um único programa monolítico que tenta prever todos os usos possíveis.

**Escreva programas que lidam com fluxos de texto, porque texto é uma interface universal.** Ao adotar texto simples como formato de comunicação padrão entre programas, o Unix garantiu que praticamente qualquer ferramenta pudesse ler a saída de qualquer outra, sem precisar de conversores especiais ou formatos proprietários incompatíveis entre si.

## "Tudo é um arquivo"

Existe ainda uma ideia frequentemente associada à filosofia Unix, embora tecnicamente seja um princípio de design um pouco separado dos três anteriores: a noção de que, no Unix (e por herança, no Linux), praticamente tudo é representado através da mesma abstração usada para arquivos comuns. Não só documentos de texto, mas também dispositivos de hardware, como um disco ou uma porta serial, e até canais de comunicação entre processos, aparecem no sistema como se fossem arquivos, podendo ser lidos e escritos usando os mesmos comandos básicos já apresentados no arquivo sobre [manipulação de arquivos](../02-terminal-na-pratica/01-manipulacao-arquivos-diretorios.md).

Essa unificação simplifica enormemente a forma como o sistema é usado e programado: em vez de aprender uma interface diferente para cada tipo de recurso do sistema, aprende-se um único modelo, "ler e escrever num arquivo", e esse mesmo modelo se aplica de forma consistente à imensa maioria das coisas com que se interage no Linux.

## Por que isso ainda importa

Entender essa filosofia explica por que o Linux, décadas depois de suas ideias fundadoras, ainda privilegia ferramentas de linha de comando pequenas e combináveis em vez de aplicativos únicos e monolíticos para cada tarefa. Não é conservadorismo técnico, é a aplicação consistente de um princípio de design que se provou, na prática, extremamente durável e flexível.

## Fontes

- [Basics of the Unix Philosophy, homepage.cs.uri.edu](https://homepage.cs.uri.edu/~thenry/resources/unix_art/ch01s06.html)
- [Unix Philosophy: A Quick Look at the Ideas that Made Unix, Klara Systems](https://klarasystems.com/articles/unix-philosophy-a-quick-look-at-the-ideas-that-made-unix/)
- [Doing One Thing, Well: The UNIX Philosophy, Hackaday](https://hackaday.com/2018/09/10/doing-one-thing-well-the-unix-philosophy/)
