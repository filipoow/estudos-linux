# Visualizando e filtrando conteúdo de arquivos no terminal

## O problema que esses comandos resolvem

Depois de aprender a criar e organizar arquivos, o próximo passo natural é conseguir olhar o que tem dentro deles sem precisar abrir um editor toda vez, especialmente quando o arquivo é um log gigante ou uma configuração que só interessa em parte. Existe um pequeno conjunto de comandos clássicos do Unix, cada um pensado para um cenário específico, que juntos cobrem quase toda situação de leitura de arquivo texto.

## `cat`: mostrar tudo de uma vez

O `cat` (de "concatenate") joga o conteúdo inteiro de um arquivo direto na tela.

```
cat notas.txt
```

Ele também serve para juntar vários arquivos, exibindo o conteúdo de todos em sequência, e é bastante usado em conjunto com redirecionamento para criar um arquivo novo a partir da junção de outros. O problema do `cat` aparece em arquivos grandes: se o arquivo tem milhares de linhas, tudo passa voando pela tela.

## `more` e `less`: mostrar aos poucos

Para arquivos longos, existe o `more`, que mostra o conteúdo uma tela por vez, avançando com a barra de espaço. É um comando antigo e simples, mas com pouca flexibilidade, ele só anda para frente.

O `less` resolve essa limitação. Apesar do nome (uma brincadeira com "menos é mais"), ele faz tudo que o `more` faz e mais, permite rolar para frente e para trás livremente, e ainda oferece busca de texto dentro do arquivo apertando `/` seguido da palavra procurada. Na prática, `less` é o comando que a maioria dos usuários experientes acaba preferindo para explorar arquivos grandes.

## `head` e `tail`: só o começo ou só o final

Às vezes não interessa o arquivo inteiro, só uma ponta dele. O `head` mostra as primeiras linhas de um arquivo, por padrão as dez primeiras:

```
head arquivo.log
```

O `tail` faz o oposto, mostra as últimas linhas:

```
tail arquivo.log
```

Os dois aceitam a opção `-n` para escolher quantas linhas mostrar, por exemplo `tail -n 50` mostra as últimas cinquenta. O `tail` tem ainda um uso muito comum em administração de sistemas: a opção `-f` ("follow"), que mantém o comando rodando e mostra novas linhas em tempo real, à medida que são escritas no arquivo. É assim que se acompanha um log de servidor sendo gerado ao vivo:

```
tail -f /var/log/syslog
```

## `grep`: filtrar por conteúdo

Enquanto os comandos anteriores mostram um arquivo inteiro ou um pedaço dele, o `grep` faz outra coisa: busca por um padrão de texto e mostra só as linhas onde esse padrão aparece.

```
grep "erro" arquivo.log
```

Esse comando mostraria só as linhas do arquivo que contêm a palavra "erro". O `grep` aceita várias opções úteis, como `-i` para ignorar diferença entre maiúsculas e minúsculas, `-r` para procurar recursivamente dentro de todas as subpastas, e `-v` para fazer o inverso, mostrar só as linhas que não contêm o padrão buscado. Ele também entende expressões regulares, um jeito de descrever padrões de texto bem mais flexíveis do que uma palavra fixa, o que o torna uma das ferramentas mais poderosas (e mais usadas) de todo o ecossistema Unix.

## Combinando tudo com pipes

A força real desses comandos aparece quando são encadeados uns nos outros através do pipe (`|`), que já foi apresentado no arquivo sobre [linha de comando](04-linha-de-comando.md). Por exemplo, para ver só as últimas vinte linhas de um log que contenham a palavra "falha":

```
tail -n 100 arquivo.log | grep "falha"
```

Esse tipo de combinação, pegar ferramentas simples e encadear a saída de uma na entrada da outra, é a essência da filosofia Unix, e é o motivo pelo qual esses comandos, criados há décadas, continuam extremamente relevantes até hoje.

## Fontes

- [Viewing Files in Linux Using cat, more, and less, Baeldung on Linux](https://www.baeldung.com/linux/files-cat-more-less)
- [Cat, Less, Tail and Head, Command Line Text Processing](https://learnbyexample.gitbooks.io/command-line-text-processing/content/tail_less_cat_head.html)
- [grep(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/grep.1.html)
