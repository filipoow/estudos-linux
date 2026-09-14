# Manipulando arquivos em lote com find, exec e xargs

## De localizar arquivos a agir sobre eles em massa

O arquivo sobre [grep avançado e find](../07-processamento-texto-e-automacao/04-grep-avancado-find.md) já apresentou o `find` localizando arquivos, e combinando isso com `grep` para buscar texto dentro deles. Aqui o foco muda: usar essa mesma capacidade de localizar arquivos para agir sobre eles em lote, seja apagando, copiando, renomeando ou ajustando permissões de muitos arquivos de uma vez, sem precisar de um loop manual escrito à mão.

## `find -exec`: rodando um comando para cada resultado

A opção `-exec`, já mencionada rapidamente antes, roda um comando para cada arquivo encontrado, substituindo `{}` pelo caminho daquele arquivo:

```
find /var/backups -name "*.tmp" -exec rm {} \;
```

Esse comando apaga todo arquivo terminado em `.tmp` dentro de `/var/backups`. O `\;` marca o fim do comando executado para cada resultado. Existe uma variação, terminando com `+` em vez de `\;`, que agrupa vários arquivos numa única chamada do comando, em vez de rodar o comando uma vez para cada arquivo individualmente, o que costuma ser mais rápido quando há muitos resultados.

Operações um pouco mais elaboradas, como renomear arquivos trocando a extensão, normalmente exigem envolver o comando numa subchamada de shell:

```
find . -name "*.bak" -exec sh -c 'mv "$1" "${1%.bak}.backup"' sh {} \;
```

Aqui, `${1%.bak}` usa um recurso de manipulação de string do shell para remover o sufixo `.bak` do nome, antes de acrescentar `.backup` no lugar.

## `xargs`: quando o comando não aceita entrada por pipe diretamente

Já apresentado no arquivo sobre [processos: pgrep, pkill e xargs](../07-processamento-texto-e-automacao/06-processos-pgrep-pkill-xargs-exit-codes.md) para lidar com processos, o `xargs` também é amplamente usado para arquivos, e costuma ser mais rápido que `-exec` sozinho, porque agrupa vários itens numa única chamada de comando por padrão, em vez de abrir um processo novo para cada arquivo.

```
find /var/www/html -type f -name "*.php" -print0 | xargs -0 chmod 644
```

Esse comando ajusta a permissão de todos os arquivos `.php` encontrados de uma vez, reaproveitando o `chmod`, já apresentado no arquivo sobre [permissões](../03-arquivos-permissoes-processos/02-permissoes-chmod-chown.md). As opções `-print0` (do `find`) e `-0` (do `xargs`) merecem destaque: elas fazem os nomes de arquivo serem separados por um caractere nulo em vez de espaço ou quebra de linha, evitando que nomes de arquivo com espaço no meio sejam interpretados como dois arquivos separados, o mesmo tipo de problema de divisão de palavras já discutido no arquivo sobre [aspas e expansão de variáveis](../08-variaveis-condicionais-scripts-avancados/02-aspas-expansao-variaveis.md).

## Um cuidado que vale sempre seguir

Operações em lote, por definição, afetam muitos arquivos de uma vez, o que significa que um padrão de busca errado pode causar um estrago bem maior do que um comando único digitado por engano. A prática recomendada, principalmente ao lidar com `rm` em massa, é testar o `find` sozinho primeiro, sem o `-exec` ou o `xargs`, conferindo visualmente a lista de arquivos que seriam afetados, e só depois acrescentar a ação destrutiva, quando a lista já estiver confirmada como correta.

## Fontes

- [xargs Command in Linux with Examples, LinuxBlog.io](https://linuxblog.io/xargs-command-linux-examples/)
- [Using find with exec: Automating Actions on Found Files, DoHost](https://dohost.us/index.php/2025/09/07/using-find-with-exec-automating-actions-on-found-files/)
- [find(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/find.1.html)
