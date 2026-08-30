# Criando e automatizando scripts em shell

## De comandos soltos a um programa organizado

Até aqui, cada comando apresentado neste repositório foi digitado sozinho, um de cada vez, diretamente no terminal. Um script de shell é o passo natural seguinte: um arquivo de texto guardando uma sequência inteira de comandos, prontos para serem executados juntos, na ordem em que foram escritos, sempre que for necessário. É basicamente a forma mais simples e mais direta de automatizar uma tarefa no Linux.

## A primeira linha: o shebang

Todo script de shell começa, por convenção, com uma linha especial chamada shebang, formada pelos caracteres `#!` seguidos do caminho para o programa que deve interpretar aquele script.

```bash
#!/bin/bash
```

Essa linha diz ao sistema, de forma explícita, qual interpretador usar para rodar o restante do arquivo, o Bash sendo o mais comum, ainda que outros existam, como o `/bin/sh`. Sem essa linha, o sistema pode acabar tentando executar o script com o shell padrão errado, ou o comportamento pode variar dependendo de como o script foi chamado.

## Variáveis: guardando e reaproveitando valores

Assim como em qualquer linguagem de programação, scripts de shell permitem guardar valores em variáveis, para reaproveitar ou reutilizar depois:

```bash
#!/bin/bash
pasta="/home/filipe/backups"
echo "Salvando backup em $pasta"
```

A atribuição de valor não pode ter espaços ao redor do sinal de igual, e para ler o valor de uma variável depois, usa-se o cifrão (`$`) antes do nome dela. É uma boa prática colocar o nome da variável entre chaves quando ela aparece dentro de um texto maior, como em `${pasta}`, para evitar ambiguidade sobre onde o nome da variável termina.

## Um exemplo prático simples

Juntando comandos já vistos em arquivos anteriores, um script simples de limpeza poderia ser assim:

```bash
#!/bin/bash
echo "Iniciando limpeza de arquivos temporarios"
rm -rf /tmp/cache_antigo
mkdir -p /tmp/cache_antigo
echo "Limpeza concluida"
```

Cada linha é um comando comum, dos mesmos apresentados no arquivo sobre [manipulação de arquivos e diretórios](09-manipulacao-arquivos-diretorios.md), só que agora reunidos num único arquivo, executados em sequência automaticamente.

## Dando permissão de execução

Criar o arquivo de texto com os comandos não é suficiente, o sistema também precisa saber que aquele arquivo pode ser executado como programa, e não só lido como um texto qualquer. Isso é feito com o `chmod`, já apresentado no arquivo sobre [permissões](16-permissoes-chmod-chown.md):

```
chmod +x limpeza.sh
```

Depois disso, o script pode ser rodado apontando explicitamente para o caminho dele, geralmente com `./` na frente quando ele está na pasta atual:

```
./limpeza.sh
```

O `./` é necessário porque, por padrão, o shell não procura programas para executar na pasta atual, só nas pastas listadas numa variável de sistema chamada PATH, então é preciso indicar explicitamente onde o script está.

## Por que automatizar assim vale a pena

Um script transforma uma sequência de comandos que exigiria digitação manual, repetida e sujeita a erro humano, em algo confiável e repetível com um único comando. Isso se torna ainda mais poderoso quando combinado com agendamento automático, tema do próximo arquivo, sobre [CronTab](25-crontab-agendamento.md), permitindo que tarefas rotineiras de manutenção rodem sozinhas, sem depender de ninguém lembrar de executá-las manualmente.

## Fontes

- [Bash Scripting Tutorial, freeCodeCamp](https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/)
- [First Bash Script - Shebang, Executable Permissions, Running Scripts, 8gwifi.org](https://8gwifi.org/tutorials/bash/first-script.jsp)
- [Shell Scripting Basics, Seneca Polytechnic](https://pressbooks.senecapolytechnic.ca/uli101/chapter/shell-scripting-basics/)
