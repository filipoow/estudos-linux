# Manipulando dados com cut, awk e tr, e fazendo aritmética no Bash

## Três ferramentas, três formas de recortar e transformar texto

Depois de aprender a encontrar padrões com expressões regulares, o passo seguinte é extrair e transformar partes específicas desse texto. `cut`, `awk` e `tr` cobrem essa necessidade de formas complementares, cada um com seu ponto forte.

## `cut`: recortando por posição ou delimitador

O `cut` extrai pedaços de cada linha, seja por posição de caractere, seja por um delimitador que separa "colunas" de texto.

```
cut -d',' -f1,3 arquivo.csv
```

Aqui, `-d','` define a vírgula como delimitador entre campos, e `-f1,3` pega o primeiro e o terceiro campo de cada linha. Também é possível pegar um intervalo, como `-f1-3` para os três primeiros campos. O `cut` é rápido e direto, mas só funciona bem quando o delimitador é sempre o mesmo caractere, de forma consistente em todas as linhas.

## `awk`: quando as colunas variam ou é preciso calcular algo

Quando os campos não são separados por um único caractere fixo, ou quando é preciso fazer algo além de simplesmente recortar, como somar valores, o `awk` entra em cena. Dentro do `awk`, cada linha é dividida automaticamente em campos, acessíveis como `$1`, `$2`, e assim por diante, com `$0` representando a linha inteira.

```
df -h | awk '{print $3}'
```

Esse exemplo reaproveita o `df`, apresentado no arquivo sobre [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md), e usa o `awk` para extrair só a terceira coluna da saída, o espaço usado em disco. O `awk` também separa colunas por espaços em branco por padrão, de forma mais flexível que o `cut`, sem exigir um delimitador único e fixo.

O `awk` também sabe fazer contas, o que o torna útil para somar uma coluna inteira de números:

```
awk '{soma += $1} END {print soma}' valores.txt
```

## `tr`: traduzindo e removendo caracteres

O `tr` ("translate") trabalha caractere por caractere, substituindo, removendo ou compactando um conjunto de caracteres por outro. Ele não recebe nome de arquivo como argumento, só funciona lendo da entrada padrão, então normalmente aparece depois de um pipe.

```
echo "TEXTO EM MAIUSCULA" | tr '[:upper:]' '[:lower:]'
```

Esse comando converte todo o texto para minúsculas. O `tr` também é usado com frequência para remover caracteres indesejados, como quebras de linha extras, ou para trocar um separador por outro, como vírgulas por quebras de linha.

## Fazendo contas no Bash

Diferente de linguagens de programação comuns, o Bash não trata números e texto da mesma forma por padrão, então operações aritméticas precisam de uma sintaxe própria, usando parênteses duplos:

```bash
resultado=$((5 + 3))
echo $resultado
```

Essa sintaxe `$(( ))` avalia a expressão aritmética dentro dela e devolve o resultado, suportando as operações básicas (`+`, `-`, `*`, `/`, `%` para resto da divisão). É essa construção que permite, por exemplo, contar quantas vezes um script já rodou, ou calcular um percentual dentro de uma condicional, como as apresentadas no arquivo sobre [estruturação de scripts](../06-fundamentos-so-unix-shell/06-estruturando-scripts-shell.md).

## Fontes

- [How to Use cut to Extract Columns, Linux Bash](https://linuxbash.sh/post/how-to-use-cut-to-extract-columns)
- [Efficient Text Processing in Linux: Awk, Cut, Paste, Linux Journal](https://www.linuxjournal.com/content/efficient-text-processing-linux-awk-cut-paste)
- [Linux Command: cut and tr, Medium](https://medium.com/geekculture/linux-command-cut-and-tr-94dcca16b49d)
