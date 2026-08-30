# O conceito de software livre e a licença GPL

## A frustração que virou um movimento

A história do software livre normalmente começa com Richard Stallman, programador do Laboratório de Inteligência Artificial do MIT no fim dos anos 70 e início dos 80. Um episódio costuma ser citado como o estopim: o laboratório tinha uma impressora Xerox que travava com frequência, e Stallman queria simplesmente corrigir o código do driver para avisar os usuários quando isso acontecesse. O problema é que o fabricante não fornecia o código-fonte, então não havia como consertar nada. Isso pode parecer um detalhe pequeno, mas para Stallman representava algo maior: o software estava deixando de ser algo que se compartilha e se aperfeiçoa em conjunto, para virar propriedade fechada que o usuário não tem controle nenhum sobre ela.

Em 1983, Stallman anunciou o Projeto GNU, com um objetivo bem definido: construir um sistema operacional completo, compatível com Unix, formado inteiramente por software livre. O nome GNU é um acrônimo recursivo, "GNU's Not Unix". Em 1985 ele fundou a Free Software Foundation (FSF) para dar suporte institucional e jurídico a esse projeto.

## O que "livre" significa aqui

Um ponto que confunde muita gente é achar que "software livre" quer dizer "software grátis". Não é bem assim, Stallman sempre foi enfático nessa distinção: o "livre" se refere a liberdade, não a preço. A Free Software Foundation resume isso em quatro liberdades básicas que um software precisa garantir para ser considerado livre:

- Liberdade de executar o programa como você quiser, para qualquer propósito.
- Liberdade de estudar como o programa funciona e adaptá-lo às suas necessidades, o que exige acesso ao código-fonte.
- Liberdade de redistribuir cópias, para ajudar outras pessoas.
- Liberdade de distribuir cópias das suas versões modificadas, para que toda a comunidade se beneficie das melhorias.

Um software pode ser gratuito e mesmo assim não ser livre, se essas liberdades não existirem. E pode, em teoria, ser cobrado e mesmo assim ser livre, desde que as quatro condições sejam respeitadas.

## Copyleft: usar o direito autoral para garantir liberdade

Para colocar essas ideias em prática de um jeito que se sustentasse legalmente, Stallman criou o conceito de copyleft, um trocadilho com "copyright". A lógica é curiosa: em vez de usar o direito autoral para restringir o uso de uma obra, como normalmente se faz, o copyleft usa esse mesmo mecanismo legal para garantir que a obra (e tudo que for derivado dela) permaneça livre para sempre. Na prática, isso significa que qualquer pessoa pode modificar um software licenciado assim, mas se ela distribuir essa versão modificada, é obrigada a manter a mesma licença e liberar o código-fonte também.

Esse princípio virou a GNU General Public License, a GPL, lançada em sua primeira versão em janeiro de 1989. É hoje uma das licenças de software livre mais usadas no mundo, e foi justamente sob a GPL que Linus Torvalds decidiu licenciar o Linux, decisão que teve um peso enorme no crescimento do projeto, porque garantiu a qualquer empresa ou desenvolvedor que suas contribuições nunca poderiam ser "fechadas" por outra parte.

## As versões da GPL

A GPL passou por revisões importantes ao longo do tempo:

- **GPLv1 (1989)**: estabeleceu a estrutura básica, as quatro liberdades e a obrigação de distribuir o código-fonte junto com o programa.
- **GPLv2 (1991)**: é a versão mais usada historicamente, e é a licença sob a qual o kernel Linux é distribuído até hoje.
- **GPLv3 (2007)**: trouxe atualizações importantes, como proteções contra patentes de software e contra a chamada "tivoização", que é quando um fabricante distribui hardware que só roda versões assinadas digitalmente do software, mesmo sendo GPL, impedindo na prática que o usuário use uma versão modificada. A GPLv3 também melhorou a linguagem jurídica para funcionar melhor fora dos Estados Unidos.

Vale registrar que o kernel Linux em si permanece sob GPLv2, não GPLv3. Isso já gerou debates dentro da comunidade, mas a decisão de manter a versão 2 segue valendo até hoje.

## Software livre e código aberto não são exatamente sinônimos

É comum ver "software livre" e "open source" usados como se fossem a mesma coisa, e na prática, para a maioria dos softwares, os dois termos acabam descrevendo o mesmo conjunto de licenças. Mas a origem dos dois é diferente: o movimento de software livre, liderado pela FSF, enfatiza uma questão ética, a liberdade do usuário. O movimento open source, que surgiu depois, nos anos 90, prefere enfatizar vantagens práticas, como qualidade de código e eficiência de desenvolvimento colaborativo. São filosofias com ênfases diferentes, ainda que na prática compartilhem boa parte das mesmas regras.

## Fontes

- [GNU General Public License, Wikipédia](https://en.wikipedia.org/wiki/GNU_General_Public_License)
- [The Free Software Definition, GNU.org](https://www.gnu.org/philosophy/free-sw.html)
- [What is Copyleft?, GNU.org](https://www.gnu.org/licenses/copyleft.html)
- [The History of the GNU General Public License](https://www.free-soft.org/gpl_history/)
- [Open Source Software Licenses 101: GPL v3, FOSSA](https://fossa.com/blog/open-source-software-licenses-101-gpl-v3/)
