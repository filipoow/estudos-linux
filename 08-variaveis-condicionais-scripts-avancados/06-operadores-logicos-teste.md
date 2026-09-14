# Operadores lógicos e de teste para strings e números

## A base por trás de todo `if`

Todo `if`, `elif` e `while` apresentado até aqui neste repositório depende de uma avaliação de teste por trás dos panos. Vale entender, com mais precisão, como o Bash compara valores, porque os operadores para texto e para número não são os mesmos, e misturá-los é uma fonte comum de bugs silenciosos.

## `[ ]` e `[[ ]]`: dois jeitos de testar uma condição

O colchete simples `[ ]` é, na prática, um comando de teste tradicional, compatível com `sh` puro, já apresentado no arquivo sobre [tipos de shell](../06-fundamentos-so-unix-shell/05-tipos-de-shell.md). O colchete duplo `[[ ]]` é uma extensão específica do Bash, mais moderna e mais segura, entre outras coisas porque lida melhor com variáveis vazias ou não citadas entre aspas, exigindo menos cuidado manual com o tema apresentado no arquivo sobre [aspas e expansão de variáveis](02-aspas-expansao-variaveis.md). Quando o script já assume Bash (via shebang `#!/usr/bin/env bash`), `[[ ]]` costuma ser a escolha preferida.

## Comparando números

Números são comparados com operadores de duas letras, e não com os símbolos matemáticos comuns:

| Operador | Significado |
|---|---|
| `-eq` | igual |
| `-ne` | diferente |
| `-lt` | menor que |
| `-le` | menor ou igual |
| `-gt` | maior que |
| `-ge` | maior ou igual |

```bash
if [ "$idade" -ge 18 ]; then
    echo "Maior de idade"
fi
```

Alternativamente, dentro de `[[ ]]` ou da avaliação aritmética `$(( ))`, já usada no arquivo sobre [expressões ternárias](03-condicionais-if-elif-else-ternario.md), os símbolos matemáticos tradicionais (`>`, `<`, `==`) também funcionam.

## Comparando texto

Para comparar strings, os operadores são outros:

| Operador | Significado |
|---|---|
| `=` ou `==` | igual (dentro de `[[ ]]`, prefira `==`) |
| `!=` | diferente |
| `-z` | string vazia |
| `-n` | string não vazia |

```bash
if [ -z "$nome" ]; then
    echo "Nome nao foi informado"
fi
```

Um erro comum é usar `-eq` para comparar texto, ou `==` para comparar número dentro do `[ ]` tradicional: `[[ "02" == "2" ]]` é falso, porque são strings diferentes caractere a caractere, enquanto `[[ 02 -eq 2 ]]` é verdadeiro, porque representam o mesmo valor numérico. Misturar essas duas lógicas é uma fonte silenciosa de bugs, já que o Bash não impede a combinação errada, ele só devolve um resultado inesperado.

## Operadores lógicos: combinando condições

Para combinar mais de uma condição, existem os operadores lógicos `&&` (E) e `||` (OU), além do `!` para negar uma condição:

```bash
if [ "$ambiente" = "producao" ] && [ "$confirmado" = "sim" ]; then
    echo "Prosseguindo com o deploy"
fi
```

Esse mesmo `&&` também é usado fora de uma estrutura `if`, para encadear comandos de forma que o segundo só rode se o primeiro tiver sucesso (código de saída igual a zero, conceito já apresentado no arquivo sobre [processos e exit codes](../07-processamento-texto-e-automacao/06-processos-pgrep-pkill-xargs-exit-codes.md)), enquanto `||` faz o oposto, rodando o segundo comando só se o primeiro falhar.

## Fontes

- [The test Command and When to Use -eq, =, and ==, Baeldung on Linux](https://www.baeldung.com/linux/test-command)
- [Bash Comparison Operators, Linuxize](https://linuxize.com/post/bash-comparison-operators/)
- [Test Operators in Bash, Linux Handbook](https://linuxhandbook.com/bash-test-operators/)
