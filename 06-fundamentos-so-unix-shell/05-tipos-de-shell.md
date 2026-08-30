# Comparando diferentes tipos de shell e suas aplicações

## Existe mais de um shell, e a escolha importa

O arquivo anterior explicou o que um [shell faz como interpretador de comandos](04-shell-interpretador-comandos.md), mas vale deixar claro: "o shell" não é um programa único. Existem vários, com históricos, sintaxes e propósitos diferentes entre si, e saber a diferença entre eles evita confusão na hora de escrever um script ou configurar um ambiente novo.

## `sh`: o ancestral comum

O Bourne Shell, cujo executável se chama `sh`, foi criado por Stephen Bourne nos primeiros anos do Unix, e é o ancestral direto ou indireto de praticamente todos os shells usados hoje. Ele é minimalista, sem muitos dos recursos considerados básicos atualmente, mas é justamente essa simplicidade que o torna, ainda hoje, o padrão mínimo de compatibilidade: scripts escritos visando `sh` puro tendem a rodar em praticamente qualquer sistema Unix ou Linux, mesmo os mais antigos ou enxutos.

## `bash`: o padrão da maioria das distribuições Linux

O Bash (Bourne Again Shell) é uma extensão do `sh` original, criada como parte do Projeto GNU, já apresentado no arquivo sobre [software livre e GPL](../01-fundamentos/02-software-livre-gpl.md). É o shell padrão na esmagadora maioria das distribuições Linux, e foi o shell usado, implicitamente, em todos os exemplos de comando ao longo deste repositório. Ele mantém boa compatibilidade com o `sh` original, mas adiciona recursos bem mais modernos, como histórico de comandos navegável, autocompletar por Tab, e uma linguagem de script mais rica, incluindo as estruturas apresentadas no arquivo sobre [estruturação de scripts](06-estruturando-scripts-shell.md).

## `zsh`: personalização em primeiro lugar

O Z Shell, criado por Paul Falstad no início dos anos 90, incorpora boas ideias de vários shells anteriores, e se destacou justamente pela capacidade de personalização, temas visuais, sistemas de plugins e autocompletar mais inteligente. Ganhou um público grande entre desenvolvedores, e passou a ser o shell padrão no macOS a partir de 2019.

## `fish`: simplicidade amigável, fora do padrão

O Friendly Interactive Shell adota uma filosofia diferente: em vez de exigir configuração para ficar confortável de usar, como acontece com `bash` e `zsh`, ele já vem com recursos como sugestões automáticas e destaque de sintaxe habilitados por padrão, sem esforço nenhum do usuário. A contrapartida é que sua sintaxe de script se afasta mais do padrão `sh`/`bash`, o que o torna menos indicado quando o objetivo é escrever scripts pensados para rodar em qualquer sistema.

## `csh` e derivados: uma ramificação à parte

O C Shell, criado como parte do BSD, outra grande família de sistemas derivada do Unix, adotou uma sintaxe deliberadamente parecida com a da linguagem C, o que agradou programadores acostumados a essa linguagem, mas também criou uma ramificação de sintaxe incompatível com a linha `sh`/`bash`. Hoje é bem menos comum do que era no passado, embora ainda apareça em alguns sistemas mais antigos ou específicos.

## Como escolher

Para uso interativo do dia a dia, escolha o shell que for mais confortável, `zsh` e `fish` costumam agradar mais por conta da personalização e da experiência amigável. Já para escrever scripts que precisam rodar de forma confiável em servidores, sistemas diferentes, ou ambientes automatizados, `bash` (ou, em casos extremos de portabilidade, `sh` puro) continua sendo a escolha mais segura, justamente por ser o denominador comum mais amplamente disponível.

## Fontes

- [Evolution of Unix Shells: A Comprehensive Guide to sh, bash, fish, zsh, csh, and ksh, machaddr](https://machaddr.substack.com/p/evolution-of-unix-shells-a-comprehensive)
- [Linux Shells for Beginners, freeCodeCamp](https://www.freecodecamp.org/news/linux-shells-explained/)
- [Guide to Unix/Explanations/Choice of Shell, Wikibooks](https://en.wikibooks.org/wiki/Guide_to_Unix/Explanations/Choice_of_Shell)
