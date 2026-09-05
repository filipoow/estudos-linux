# Expressões regulares: encontrando padrões de texto

## Mais que buscar uma palavra fixa

O arquivo sobre [visualização e filtragem de conteúdo](../02-terminal-na-pratica/02-visualizacao-filtragem-conteudo.md) já mostrou o `grep` buscando uma palavra específica dentro de um arquivo. Expressões regulares (ou regex) vão além disso: são uma linguagem para descrever padrões de texto, não só palavras exatas. Em vez de procurar literalmente "erro", uma regex pode procurar "qualquer linha que comece com uma data, seguida de um número de quatro dígitos, seguida da palavra erro", tudo numa única expressão.

## Os blocos básicos de uma regex

Alguns símbolos formam a base de praticamente qualquer regex:

- `.` (ponto) representa qualquer caractere único.
- `*` significa "zero ou mais repetições do que veio antes".
- `+` significa "uma ou mais repetições do que veio antes".
- `?` significa "zero ou uma ocorrência do que veio antes", tornando esse trecho opcional.
- `[]` define um conjunto de caracteres aceitáveis numa posição, como `[0-9]` para qualquer dígito.
- `^` e `$` marcam, respectivamente, o início e o fim de uma linha.

Juntando esses blocos, um padrão como `^[0-9]{4}-[0-9]{2}-[0-9]{2}` descreve uma data no formato ano-mês-dia no início de uma linha, por exemplo `2026-08-24`.

## Gulosa por padrão: o comportamento que confunde iniciantes

Por padrão, os quantificadores (`*`, `+`, `{n,m}`) em regex são gulosos (greedy): eles tentam casar o máximo de texto possível, e só recuam se isso for necessário para o restante do padrão funcionar. Isso gera um comportamento que costuma surpreender quem está começando. Imagine o texto `<b>negrito</b> e <i>itálico</i>` e o padrão `<.+>`. Por ser guloso, esse padrão não para no primeiro `>` que encontra, ele avança até o último `>` da linha inteira, capturando `<b>negrito</b> e <i>itálico</i>` inteiro, em vez de capturar só `<b>`.

## Preguiçosa: parando no primeiro encontro possível

Para forçar o comportamento oposto, existe o quantificador preguiçoso (lazy), obtido adicionando um `?` logo depois do quantificador guloso: `*?`, `+?`, `{n,m}?`. Usando `<.+?>` no mesmo exemplo anterior, o padrão para de casar assim que encontra o primeiro `>` possível, resultando em quatro correspondências separadas: `<b>`, `</b>`, `<i>` e `</i>`.

Entender essa diferença é essencial sempre que o objetivo é capturar blocos delimitados dentro de um texto maior, como tags, aspas ou parênteses, situação em que o comportamente guloso por padrão quase sempre produz um resultado maior (e errado) do que o esperado.

## Por que isso importa tanto no dia a dia

Expressões regulares aparecem por trás de praticamente toda ferramenta de processamento de texto que será apresentada no restante deste bloco de aulas: o `grep`, apresentado com mais profundidade no arquivo sobre [grep avançado](04-grep-avancado-find.md), o `sed`, apresentado no arquivo sobre [pipelines](03-pipelines-sed-awk-printf-tee.md), e até o `awk`. Entender a lógica de gulosa versus preguiçosa evita boa parte dos erros mais comuns e mais frustrantes de quem está aprendendo a usar essas ferramentas.

## Fontes

- [Greedy and lazy quantifiers, javascript.info](https://javascript.info/regexp-greedy-and-lazy)
- [Performance of Greedy vs. Lazy Regex Quantifiers, Steven Levithan](https://blog.stevenlevithan.com/archives/greedy-lazy-performance)
- [Grep Regex: Regular Expressions Syntax and Examples, Linuxize](https://linuxize.com/post/regular-expressions-in-grep/)
