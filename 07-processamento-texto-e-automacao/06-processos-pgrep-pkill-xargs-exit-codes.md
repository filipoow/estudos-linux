# Manipulando processos: pgrep, pkill, xargs e códigos de saída

## Encontrando processos pelo nome, sem rodeios

O arquivo sobre [processos: ps, top e htop](../03-arquivos-permissoes-processos/03-processos-ps-top-htop.md) mostrou como listar processos em execução, muitas vezes combinando `ps aux` com um `grep` para filtrar pelo nome de um programa específico. O `pgrep` resolve essa combinação de forma mais direta, sem precisar encadear dois comandos:

```
pgrep firefox
```

Esse comando devolve diretamente os números de PID de todos os processos cujo nome bate com "firefox", sem o ruído extra que `ps aux | grep firefox` costuma trazer, como a linha do próprio comando `grep` aparecendo na lista de resultados por engano.

## Encerrando processos com pkill

Enquanto o `pgrep` só lista os processos encontrados, o `pkill` faz a mesma busca, mas em vez de listar, envia um sinal (por padrão, o sinal de encerramento SIGTERM) para cada processo encontrado:

```
pkill firefox
```

Esse comando é equivalente, na prática, a rodar `kill` em cada PID que o `pgrep firefox` retornaria, só que numa única chamada, mais direta e menos sujeita a erro de digitação de PID.

## `xargs`: transformando uma lista em argumentos de outro comando

Nem todo comando aceita receber uma lista de itens vinda de um pipe diretamente como argumento, muitos esperam esses valores digitados explicitamente na linha de comando. O `xargs` resolve essa lacuna, pegando a saída de um comando e transformando cada linha dela num argumento para o próximo comando.

```
pgrep -f processo_antigo | xargs kill -9
```

Aqui, o `pgrep -f` localiza os processos pelo nome completo do comando, e o `xargs` pega cada PID retornado e o passa como argumento para o `kill -9`, encerrando cada um deles à força. Esse padrão, "encontrar com um comando, agir com outro através do `xargs`", aparece com frequência em scripts de manutenção de sistemas, muito na mesma linha dos exemplos já vistos no arquivo sobre [grep avançado e find](04-grep-avancado-find.md).

## Códigos de saída: como um processo "avisa" o resultado

Todo processo, ao terminar, devolve um número chamado código de saída, e esse conceito já foi introduzido no arquivo sobre [estruturação de scripts](../06-fundamentos-so-unix-shell/06-estruturando-scripts-shell.md). Vale aprofundar aqui, especialmente no contexto de gerenciamento de processos: o código fica disponível na variável `$?` logo após a execução, e por convenção, `0` significa sucesso, qualquer valor diferente de zero indica algum tipo de falha, sendo que números diferentes costumam representar tipos diferentes de erro, dependendo do programa.

```bash
pkill servico_antigo
if [ $? -eq 0 ]; then
    echo "Processo encerrado com sucesso"
else
    echo "Nenhum processo encontrado ou falha ao encerrar"
fi
```

Esse tipo de verificação é o que permite que um script de automação reaja de forma inteligente ao resultado de uma tentativa de encerrar um processo, em vez de simplesmente assumir que tudo funcionou como esperado e seguir em frente cegamente.

## Fontes

- [pgrep and pkill, Linux scripting process management friends, opensourcehacker](https://opensourcehacker.com/2012/11/26/pgrep-and-pkill-your-linux-scripting-process-management-friends/)
- [pgrep(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/pgrep.1.html)
- [Understanding exit codes in bash scripting, Medium](https://medium.com/@gudisagebi1/understanding-exit-codes-in-bash-scripting-699ce918a9c8)
