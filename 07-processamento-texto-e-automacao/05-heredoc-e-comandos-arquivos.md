# Here Document (Heredoc) e um retorno aos comandos de arquivos

## Um lembrete rápido: os comandos de sistema de arquivos

Comandos como `cd`, `ls`, `chmod`, `chown` e `mkdir` já foram apresentados em profundidade em arquivos anteriores deste repositório: navegação e criação de pastas no arquivo sobre [manipulação de arquivos e diretórios](../02-terminal-na-pratica/01-manipulacao-arquivos-diretorios.md), caminhos absolutos e relativos usados pelo `cd` no arquivo sobre [caminhos](../03-arquivos-permissoes-processos/05-caminhos-absolutos-relativos.md), e permissões com `chmod` e `chown` no arquivo sobre [permissões](../03-arquivos-permissoes-processos/02-permissoes-chmod-chown.md). Vale a pena revisar esses arquivos caso algum desses comandos ainda não esteja familiar, já que eles são a base sobre a qual o restante deste bloco de aulas se apoia. Aqui o foco é um recurso novo: o Heredoc.

## O que é um Heredoc

Um Here Document, ou Heredoc, é um jeito de escrever um bloco de texto com várias linhas diretamente dentro de um script ou comando, sem precisar de um arquivo separado, nem de várias chamadas de `echo` empilhadas uma atrás da outra. A sintaxe usa `<<` seguido de uma palavra delimitadora, geralmente `EOF` (End Of File) por convenção, embora qualquer palavra funcione.

```bash
cat <<EOF
Este é um texto
com varias linhas,
escrito direto no script.
EOF
```

Tudo que estiver entre a primeira linha (`cat <<EOF`) e a linha contendo só o delimitador (`EOF`) é tratado como um único bloco de texto, entregue ao comando indicado, nesse caso o `cat`.

## Salvando um Heredoc num arquivo

Combinando Heredoc com o redirecionador `>`, apresentado no arquivo sobre [redirecionamento e pipes](../04-pacotes-scripts-automacao/02-redirecionamento-pipes.md), é possível gerar arquivos de configuração inteiros de dentro de um script, de forma legível:

```bash
cat <<EOF > /etc/app/config.conf
usuario=admin
porta=8080
ambiente=producao
EOF
```

Esse padrão é extremamente comum em scripts de automação e provisionamento de servidores, exatamente o tipo de tarefa apresentada no arquivo sobre [integração de scripts e cron](../04-pacotes-scripts-automacao/05-integracao-scripts-cron-manutencao.md), quando é preciso gerar um arquivo de configuração completo como parte de uma rotina automatizada.

## Variáveis dentro do Heredoc

Por padrão, variáveis do shell são expandidas dentro de um Heredoc, ou seja, substituídas pelo valor que representam, exatamente como aconteceria em qualquer outro trecho de um script:

```bash
nome="Filipe"
cat <<EOF
Ola, $nome!
EOF
```

Esse comando imprime "Ola, Filipe!", com o valor da variável já substituído. Para desativar essa expansão, e tratar o conteúdo como texto totalmente literal, basta colocar o delimitador entre aspas simples na primeira linha: `<<'EOF'`. Isso é útil quando o bloco de texto contém símbolos como `$` que não devem ser interpretados como variáveis, por exemplo ao gerar um script para outra linguagem que também usa esse símbolo com outro significado.

## Um cuidado comum

O erro mais frequente ao usar Heredoc é um espaço acidental depois do delimitador de fechamento. O shell exige que a linha de fechamento contenha só o delimitador, sem nada mais, nem mesmo um espaço em branco à direita. Quando isso passa despercebido, o shell não reconhece o fim do bloco, e continua lendo o resto do script como se ainda fizesse parte do texto do Heredoc, gerando um erro confuso de "fim de arquivo inesperado".

## Fontes

- [Bash HereDoc Tutorial: Syntax, Examples, phoenixNAP](https://phoenixnap.com/kb/bash-heredoc)
- [Bash Heredoc: Complete Guide with Examples, Linuxize](https://linuxize.com/post/bash-heredoc/)
- [Bash Heredoc Tutorial For Beginners, OSTechNix](https://ostechnix.com/bash-heredoc-tutorial/)
