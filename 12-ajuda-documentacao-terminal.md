# Comandos de ajuda e documentação: man e pwd

## Ninguém decora tudo, e não precisa

Um dos maiores enganos de quem está começando com Linux é achar que vai precisar memorizar centenas de comandos e opções de cor. Na prática, mesmo administradores de sistema com anos de experiência consultam documentação o tempo todo. A diferença é que eles sabem onde procurar sem sair do terminal, e é exatamente isso que os comandos deste arquivo resolvem.

## `man`: o manual embutido no sistema

O `man` (de "manual") abre a documentação oficial de um comando, instalada localmente no próprio sistema, sem precisar de internet.

```
man ls
```

Esse comando abre uma página de manual detalhada sobre o `ls`, explicando o que ele faz e listando cada uma das suas opções. As páginas de manual seguem um formato bem padronizado, o que ajuda muito depois que a pessoa se acostuma com a estrutura:

- **NAME**: o nome do comando e uma frase curta descrevendo o que ele faz.
- **SYNOPSIS**: como o comando deve ser escrito, incluindo quais partes são opcionais.
- **DESCRIPTION**: a explicação completa do comportamento do comando e de cada opção disponível.
- **EXAMPLES**: em muitas páginas, exemplos práticos de uso.

Para navegar dentro do manual, usam-se as setas ou a barra de espaço para avançar, e a tecla `q` para sair e voltar ao terminal.

Vale saber que o `man` cobre não só comandos, mas também arquivos de configuração e funções usadas por programadores. Por isso as páginas de manual são organizadas em seções numeradas (comandos de usuário, chamadas de sistema, arquivos de configuração, entre outras), e ocasionalmente é preciso indicar a seção certa quando existe mais de um resultado com o mesmo nome.

Uma alternativa mais rápida e resumida ao `man`, presente na maioria dos comandos, é rodar o próprio comando seguido de `--help`, que mostra um resumo das opções direto no terminal, sem abrir o formato completo de manual.

## `pwd`: saber onde você está

Já apresentado no arquivo sobre [linha de comando](04-linha-de-comando.md), o `pwd` ("print working directory") merece ser reforçado aqui justamente por ser, junto com o `man`, uma das ferramentas de orientação mais básicas do terminal. Enquanto o `man` ajuda a entender o que um comando faz, o `pwd` ajuda a entender onde, dentro da árvore de pastas do sistema, aquele comando está prestes a ser executado.

```
pwd
```

Isso pode parecer um comando trivial demais para merecer atenção, mas é justamente esse tipo de comando simples que evita erros sérios. Muitos acidentes no terminal, como apagar a pasta errada, acontecem porque a pessoa perdeu a noção de em qual diretório estava trabalhando. Rodar `pwd` antes de qualquer comando mais arriscado é um hábito barato que evita consequências caras.

## Documentação como parte do trabalho, não uma muleta

Vale reforçar essa ideia: consultar `man`, `--help` ou até a documentação oficial de um projeto na internet não é sinal de falta de conhecimento, é parte normal do trabalho técnico. O sistema de páginas de manual existe desde os primeiros dias do Unix justamente porque os próprios criadores do sistema já sabiam que ninguém consegue (nem precisa) guardar tudo de memória.

## Fontes

- [Linux manual pages, man7.org](https://man7.org/linux/man-pages/index.html)
- [Linux commands overview, D3S, Charles University](https://d3s.mff.cuni.cz/teaching/nswi177/202223/manpage/)
- [Guides - Linux - Basic Commands and Concepts, Iowa State Research IT](https://research.it.iastate.edu/guides-linux-basic-commands-and-concepts)
