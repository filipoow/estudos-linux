# Técnicas avançadas de grep: regex Perl e integração com find

## Além da busca simples de palavra

O `grep` já apareceu várias vezes neste repositório, desde o uso básico no arquivo sobre [visualização e filtragem de conteúdo](../02-terminal-na-pratica/02-visualizacao-filtragem-conteudo.md). Com expressões regulares mais avançadas, apresentadas no arquivo sobre [regex](01-expressoes-regulares.md), o `grep` se torna uma ferramenta bem mais poderosa do que buscar uma palavra fixa dentro de um arquivo.

## Os diferentes modos de regex do grep

O `grep` suporta mais de um "dialeto" de expressão regular, selecionado por opção:

- `-G` (padrão): Basic Regular Expression (BRE), o modo mais limitado e mais antigo.
- `-E`: Extended Regular Expression (ERE), adiciona recursos como `+`, `?` e `|` sem precisar escapá-los com barra invertida. Cobre a grande maioria das necessidades do dia a dia.
- `-F`: trata o padrão como texto fixo, sem interpretar nenhum caractere como regex, útil quando se quer buscar um símbolo que teria significado especial em regex, como um ponto literal.
- `-P`: ativa o modo Perl-Compatible Regular Expressions (PCRE), com recursos avançados que nem `BRE` nem `ERE` oferecem.

## O que o modo Perl (`-P`) desbloqueia

O modo `-P` é o mais poderoso, trazendo recursos como lookahead e lookbehind, que permitem exigir que um padrão apareça (ou não apareça) antes ou depois do trecho que de fato interessa, sem incluir esse contexto no resultado final.

```
grep -P '(?=.*erro)(?=.*banco de dados)' log.txt
```

Esse exemplo usa dois lookaheads para encontrar linhas que contenham tanto "erro" quanto "banco de dados", em qualquer ordem entre si, algo que seria bem mais difícil de expressar só com `BRE` ou `ERE`. O modo `-P` também suporta diretamente os quantificadores preguiçosos apresentados no arquivo sobre [regex](01-expressoes-regulares.md) (`*?`, `+?`), que não existem nos outros modos do `grep`.

## Combinando grep com find

O `grep` sozinho busca dentro de arquivos que já foram indicados a ele. O comando `find`, por sua vez, localiza arquivos e pastas dentro de uma árvore de diretórios, segundo critérios como nome, tamanho ou data de modificação (já usado, de passagem, no arquivo sobre [integração de scripts e cron](../04-pacotes-scripts-automacao/05-integracao-scripts-cron-manutencao.md), para apagar logs antigos). Juntos, os dois cobrem um caso de uso muito comum: buscar um padrão de texto espalhado por muitos arquivos diferentes.

```
find /var/log -name "*.log" -exec grep -l "erro critico" {} \;
```

A opção `-exec` do `find` roda um comando para cada arquivo encontrado, substituindo `{}` pelo caminho daquele arquivo específico, e `\;` marca o fim do comando executado. Nesse exemplo, o resultado é a lista de arquivos de log que contêm a frase "erro critico" em algum lugar.

Uma alternativa mais direta, quando o objetivo é simplesmente buscar recursivamente dentro de uma pasta, é usar a própria opção `-r` do `grep`, já mencionada no arquivo sobre [visualização e filtragem de conteúdo](../02-terminal-na-pratica/02-visualizacao-filtragem-conteudo.md), o que dispensa o `find` para os casos mais simples, deixando a combinação com `find` reservada para quando é preciso filtrar antes por nome de arquivo, tamanho ou data.

## Fontes

- [Grep Regex: Regular Expressions Syntax and Examples, Linuxize](https://linuxize.com/post/regular-expressions-in-grep/)
- [Grep examples: Perl-Compatible Regular Expressions](https://queirozf.com/entries/grep-examples-perl-compatible-regular-expressions)
- [find(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/find.1.html)
