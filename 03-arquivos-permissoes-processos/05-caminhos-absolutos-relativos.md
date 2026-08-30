# Navegando pelo sistema de arquivos: caminhos absolutos e relativos

## Dois jeitos de dizer onde um arquivo está

O comando `cd`, já apresentado no arquivo sobre [manipulação de arquivos e diretórios](../02-terminal-na-pratica/01-manipulacao-arquivos-diretorios.md), aceita caminhos escritos de duas formas bem diferentes, e entender essa diferença é essencial para não se perder (ou pior, apagar algo no lugar errado) usando o terminal.

## Caminho absoluto: o endereço completo

Um caminho absoluto começa sempre pela raiz do sistema, representada pela barra `/`, e descreve o trajeto completo até o arquivo ou pasta, não importa de onde o comando está sendo executado.

```
cd /home/filipe/documentos
```

Não importa em qual pasta o terminal estava antes desse comando, o resultado é sempre o mesmo: o terminal vai parar exatamente em `/home/filipe/documentos`. É o equivalente a dar um endereço completo, com cidade, rua e número, algo que funciona não importa de onde a pessoa está partindo.

## Caminho relativo: a partir de onde você já está

Um caminho relativo, ao contrário, não começa com barra, e descreve o trajeto a partir da pasta atual em que o terminal já se encontra.

```
cd documentos
```

Esse comando só funciona se, no momento em que ele é digitado, o terminal já estiver dentro de uma pasta que contenha uma subpasta chamada `documentos`. Rodado de outro lugar, o mesmo comando levaria a um resultado completamente diferente, ou simplesmente falharia por não encontrar a pasta.

Caminhos relativos usam também alguns símbolos especiais:

- `.` representa a pasta atual.
- `..` representa a pasta imediatamente acima da atual.
- `../..` sobe dois níveis de uma vez, e assim por diante.

Por exemplo, estando em `/home/filipe/documentos`, o comando `cd ../downloads` leva até `/home/filipe/downloads`, subindo um nível e depois entrando na pasta vizinha.

## Quando usar cada um

Não existe uma regra fixa, mas existe uma lógica prática por trás da escolha. Caminhos absolutos são mais seguros dentro de scripts e automações, já que funcionam sempre do mesmo jeito, não importa de onde o script foi chamado, o que evita erros silenciosos. Caminhos relativos, por outro lado, são mais rápidos de digitar no uso interativo do dia a dia, especialmente quando se está navegando entre pastas próximas umas das outras, sem precisar escrever o endereço inteiro toda vez.

Vale lembrar também do `pwd`, apresentado no arquivo sobre [ajuda e documentação](../02-terminal-na-pratica/04-ajuda-documentacao-terminal.md): rodar `pwd` antes de usar um caminho relativo é um hábito simples que evita boa parte da confusão, já que ele revela exatamente qual é o ponto de partida que o caminho relativo vai considerar.

## Fontes

- [Linux Filesystem Navigation Basics, LinuxConfig](https://linuxconfig.org/filesystem-basics)
- [cd(1p), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/cd.1p.html)
- [Linux Filesystem Hierarchy, GoLinuxCloud](https://www.golinuxcloud.com/linux-filesystem-hierarchy/)
