# Controlando a expansão de variáveis: aspas simples e duplas

## Três formas de escrever a mesma variável, três resultados diferentes

Depois de entender variáveis locais e de ambiente, apresentadas no [arquivo anterior](01-variaveis-ambiente-locais.md), falta um detalhe que costuma gerar bugs difíceis de rastrear: como o shell trata uma variável muda completamente dependendo de como ela é escrita, sem aspas, entre aspas duplas, ou entre aspas simples.

## Sem aspas: expansão total, incluindo divisão em palavras

Quando uma variável é usada sem nenhuma aspa, o shell expande o valor e depois divide esse resultado em várias palavras separadas, usando espaços como separador, um processo chamado word splitting.

```bash
arquivo="meu documento.txt"
ls $arquivo
```

Esse exemplo provavelmente falha, porque o `ls` recebe dois argumentos separados, `meu` e `documento.txt`, em vez de um único nome de arquivo com espaço no meio. É um dos erros mais comuns e mais traiçoeiros em scripts de shell, já que funciona perfeitamente enquanto o valor não tiver espaço, e quebra silenciosamente assim que tiver.

## Aspas duplas: expande, mas mantém tudo junto

Colocar a variável entre aspas duplas resolve exatamente esse problema: o valor ainda é expandido normalmente, incluindo variáveis e substituição de comandos com `$()`, mas o resultado inteiro é tratado como uma única palavra, sem divisão.

```bash
arquivo="meu documento.txt"
ls "$arquivo"
```

Agora o `ls` recebe um único argumento, `meu documento.txt`, exatamente como pretendido. Por esse motivo, a recomendação praticamente unânime entre quem escreve scripts de shell é: use aspas duplas ao redor de qualquer variável, por padrão, a menos que exista um motivo específico para não usar.

## Aspas simples: nada é expandido, nem mesmo variáveis

Aspas simples vão um passo além das duplas: elas tratam tudo dentro delas como texto absolutamente literal, sem expandir variáveis nem executar substituição de comandos.

```bash
nome="Filipe"
echo 'Ola, $nome'
```

Esse comando imprime literalmente `Ola, $nome`, sem substituir pelo valor da variável. Isso já apareceu, de forma prática, no arquivo sobre [Heredoc](../07-processamento-texto-e-automacao/05-heredoc-e-comandos-arquivos.md), onde colocar o delimitador entre aspas simples desativava a expansão de variáveis dentro do bloco inteiro, pela mesma razão explicada aqui.

## Resumindo as três formas

| Forma | Expande variáveis | Expande comandos `$()` | Protege contra word splitting |
|---|---|---|---|
| Sem aspas | Sim | Sim | Não |
| `"Aspas duplas"` | Sim | Sim | Sim |
| `'Aspas simples'` | Não | Não | Sim |

## A regra prática que evita a maioria dos bugs

Se o objetivo é o valor literal, sem nenhuma substituição, use aspas simples. Se o objetivo é usar o valor de uma variável (ou o resultado de um comando), quase sempre a escolha certa é aspas duplas. Deixar uma variável sem aspas deveria ser a exceção, reservada para os raros casos em que a divisão em palavras é, de fato, o comportamento desejado, e não um acidente.

## Fontes

- [Difference between Single and Double Quotes in Bash, Saturn Cloud](https://saturncloud.io/blog/difference-between-single-and-double-quotes-in-bash/)
- [What's the Difference Between Single and Double Quotes in the Bash Shell?, How-To Geek](https://www.howtogeek.com/29980/whats-the-difference-between-single-and-double-quotes-in-the-bash-shell/)
- [Bash Single vs Double Quotes: Differences, Escaping & Expansion, GoLinuxCloud](https://www.golinuxcloud.com/bash-single-vs-double-quotes/)
