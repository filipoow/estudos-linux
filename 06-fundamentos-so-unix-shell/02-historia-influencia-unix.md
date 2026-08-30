# A história e a influência do Unix no desenvolvimento de sistemas modernos

## Antes do Linux, havia o Unix

O arquivo sobre a [origem do Linux](../01-fundamentos/01-origem-evolucao-linux.md) já mencionou o Unix de passagem, ao explicar o contexto em que o Minix (um sistema criado para ensino, inspirado em Unix) motivou Linus Torvalds a começar seu próprio projeto. Vale agora voltar um pouco mais no tempo e conhecer o próprio Unix, o sistema que, direta ou indiretamente, moldou praticamente toda a computação moderna.

## Origem nos laboratórios Bell

O Unix nasceu em 1969, nos Bell Labs, o centro de pesquisa da empresa de telecomunicações AT&T, criado principalmente por Ken Thompson e Dennis Ritchie. O contexto por trás dessa criação é irônico: o Unix surgiu como reação à frustração de um projeto anterior, o Multics, um sistema operacional ambicioso demais para a época, que a Bell Labs acabou abandonando por ser complexo em excesso. Thompson, aproveitando ideias que considerava boas do Multics, mas simplificando bastante o design, criou uma versão inicial do Unix, rodando num computador então já considerado obsoleto pela empresa.

Uma decisão técnica se mostraria decisiva mais tarde: Dennis Ritchie criou, junto com o desenvolvimento do Unix, a linguagem de programação C, e o sistema foi reescrito nessa linguagem. Isso pode parecer um detalhe técnico menor, mas teve um efeito enorme: por ser escrito numa linguagem portável, o Unix podia ser adaptado para rodar em computadores de fabricantes diferentes, algo raro para a época, quando cada fabricante costumava ter seu próprio sistema operacional, incompatível com o dos concorrentes.

## Um sistema modular, quase por acidente

O design do Unix privilegiava programas pequenos, cada um fazendo uma coisa bem definida, capazes de se combinar entre si, uma filosofia detalhada com mais profundidade no arquivo sobre a [filosofia Unix](03-filosofia-unix.md). Essa abordagem modular, junto com um sistema de arquivos hierárquico (a mesma lógica de pastas dentro de pastas que o Linux herdou, e que já foi apresentada no arquivo sobre o [FHS](../03-arquivos-permissoes-processos/01-estrutura-diretorios-fhs.md)), se mostrou tão bem pensada que sobreviveu praticamente intacta até os sistemas atuais.

## A influência que chega até hoje

O impacto do Unix vai muito além de qualquer sistema que ainda carregue esse nome. Ele se tornou a referência de design para praticamente toda uma geração de sistemas operacionais que vieram depois, incluindo o próprio Linux, o macOS (que tecnicamente é certificado como um sistema Unix, construído sobre uma base BSD, outra família de sistemas derivada diretamente do Unix original), e diversos sistemas usados hoje em servidores ao redor do mundo. Boa parte da internet moderna, incluindo os servidores por trás de empresas como Google e Amazon, roda hoje sobre sistemas que seguem, direta ou indiretamente, os princípios de design estabelecidos pelo Unix há mais de cinco décadas.

## Fontes

- [History of Unix, Wikipédia](https://en.wikipedia.org/wiki/History_of_Unix)
- [The UNIX Evolution: An Innovative History, The Open Group Blog](https://blog.opengroup.org/2016/02/23/the-unix-evolution-an-innovative-history/)
- [Kenneth Thompson & Dennis Ritchie Develop UNIX, History of Information](https://www.historyofinformation.com/detail.php?id=872)
