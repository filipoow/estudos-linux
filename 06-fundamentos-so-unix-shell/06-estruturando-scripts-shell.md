# Estruturando scripts de shell para automação

## Indo além de uma sequência linear de comandos

O arquivo sobre [scripts em shell](../04-pacotes-scripts-automacao/03-scripts-shell.md) mostrou o básico: shebang, variáveis, permissão de execução. Um script real de automação, porém, quase sempre precisa de mais do que uma lista de comandos rodando um atrás do outro sem nenhuma lógica entre eles. Precisa tomar decisões, repetir tarefas, e reagir de forma organizada quando algo dá errado. É disso que trata a estruturação de um script.

## Condicionais: tomando decisões

A estrutura `if` permite que um script tome caminhos diferentes dependendo de uma condição:

```bash
if [ -f "arquivo.txt" ]; then
    echo "O arquivo existe"
else
    echo "O arquivo nao existe"
fi
```

Os colchetes `[ ]` são, na prática, um comando de teste, que verifica a condição indicada, nesse caso `-f`, que checa se um caminho aponta para um arquivo comum existente. Outros testes comuns incluem `-d` (verifica se é uma pasta) e comparações de texto ou número, como `-eq` (igual) ou `-lt` (menor que).

## Loops: repetindo tarefas

Quando a mesma ação precisa ser aplicada várias vezes, os loops evitam repetir código manualmente. O `for` percorre uma lista de valores, um de cada vez:

```bash
for arquivo in *.log; do
    echo "Processando $arquivo"
done
```

Esse exemplo roda o bloco de código uma vez para cada arquivo terminado em `.log` na pasta atual, substituindo automaticamente a variável `arquivo` a cada repetição. Já o `while` repete um bloco enquanto uma condição continuar verdadeira, útil quando não se sabe de antemão quantas repetições serão necessárias.

## Funções: reaproveitando lógica

Assim como em qualquer linguagem de programação, um script de shell pode agrupar um conjunto de comandos numa função, dando um nome a esse bloco e permitindo chamá-lo várias vezes sem repetir o código:

```bash
verificar_espaco() {
    df -h / | tail -n 1
}

verificar_espaco
```

Esse exemplo reaproveita o `df`, já apresentado no arquivo sobre [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md), dentro de uma função reutilizável, que pode ser chamada quantas vezes forem necessárias ao longo do script.

## Códigos de saída: um script também "responde"

Todo comando ou script, ao terminar, devolve um número chamado código de saída (exit code), guardado automaticamente na variável especial `$?`. Por convenção, `0` significa que tudo correu bem, e qualquer valor diferente de zero indica algum tipo de erro. Isso é o que permite que um script reaja ao sucesso ou à falha de um comando anterior:

```bash
apt-get update
if [ $? -ne 0 ]; then
    echo "Falha ao atualizar pacotes"
    exit 1
fi
```

Esse padrão, verificar o código de saída de cada etapa crítica antes de seguir para a próxima, é o que separa um script realmente confiável de automação de um simples amontoado de comandos que pode falhar silenciosamente no meio do caminho, sem que ninguém perceba.

## Estrutura como disciplina, não como enfeite

Vale reforçar: essas estruturas não existem para deixar um script "mais sofisticado", elas existem porque automação real, do tipo que roda sozinha, sem supervisão humana direta, como os exemplos vistos no arquivo sobre [integração com CronTab](../04-pacotes-scripts-automacao/05-integracao-scripts-cron-manutencao.md), precisa lidar com imprevistos de forma previsível. Um script bem estruturado sabe o que fazer quando algo sai do esperado, em vez de simplesmente continuar rodando às cegas.

## Fontes

- [Bash Scripting Tutorial, freeCodeCamp](https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/)
- [Bash Script Tutorial: Write and Run Shell Scripts, DataCamp](https://www.datacamp.com/tutorial/how-to-write-bash-script-tutorial)
- [Shell Scripting: Bash Automation for System Administrators, Networkers Home](https://www.networkershome.com/fundamentals/linux/shell-scripting-bash-automation/)
