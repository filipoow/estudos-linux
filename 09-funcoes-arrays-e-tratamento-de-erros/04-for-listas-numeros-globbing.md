# Iterando com for: listas, números e globbing

## O for além de uma lista de nomes fixos

O arquivo sobre [loops e case](../08-variaveis-condicionais-scripts-avancados/07-loops-e-case.md) mostrou o `for` percorrendo uma lista de valores escritos diretamente no script. Existem duas outras formas muito comuns de alimentar um `for`, que valem a pena conhecer separadamente: percorrer uma sequência numérica, e percorrer arquivos que casam com um padrão.

## Percorrendo números com seq ou expansão de chaves

Para repetir um bloco um número específico de vezes, ou percorrer um intervalo numérico, existem duas abordagens comuns. A primeira usa o comando `seq`:

```bash
for numero in $(seq 1 5); do
    echo "Tentativa $numero"
done
```

A segunda usa expansão de chaves, um recurso do próprio shell, sem depender de um comando externo:

```bash
for numero in {1..5}; do
    echo "Tentativa $numero"
done
```

Vale um detalhe técnico importante aqui: a expansão de chaves acontece antes da expansão de variáveis, dentro da ordem de processamento do shell. Isso significa que `{1..$n}`, com uma variável dentro das chaves, não funciona como intervalo dinâmico, porque no momento em que as chaves são expandidas, `$n` ainda não foi substituído por um número. Para intervalos com um limite definido por variável, a forma com `seq "$n"` (ou um loop `while` com contador, como já visto no arquivo sobre [loops e case](../08-variaveis-condicionais-scripts-avancados/07-loops-e-case.md)) é a escolha mais segura.

## Globbing: percorrendo arquivos que casam com um padrão

Globbing é o mecanismo do shell que expande um padrão com curingas para a lista real de arquivos que combinam com ele, antes mesmo do comando ser executado. Os curingas mais comuns são `*` (qualquer sequência de caracteres, inclusive nenhuma), `?` (exatamente um caractere qualquer) e `[]` (um conjunto específico de caracteres aceitáveis numa posição, como `[0-9]`).

```bash
for arquivo in *.log; do
    echo "Processando $arquivo"
done
```

Esse exemplo, já usado de forma resumida no arquivo sobre [manipulação de arquivos e diretórios](../02-terminal-na-pratica/01-manipulacao-arquivos-diretorios.md), percorre todo arquivo da pasta atual terminado em `.log`. Diferente de uma expressão regular, apresentada no arquivo sobre [regex](../07-processamento-texto-e-automacao/01-expressoes-regulares.md), o globbing é mais simples e é resolvido pelo próprio shell antes do comando começar a rodar, não por uma ferramenta como `grep` interpretando o padrão linha por linha depois.

Um cuidado importante: se nenhum arquivo casar com o padrão, o comportamento padrão do Bash é deixar o padrão sem expandir, então o loop rodaria uma única vez com o texto literal `*.log`, em vez de não rodar nenhuma vez. Para evitar essa armadilha, existe a opção `shopt -s nullglob`, que faz o padrão desaparecer completamente quando não há nenhum arquivo correspondente, resultando num loop que simplesmente não executa nada nesse caso, o comportamento que a maioria dos scripts realmente espera.

## Por que separar essas três formas

Uma lista fixa serve quando os valores já são conhecidos de antemão e não mudam. Uma sequência numérica serve para repetição controlada, contagens ou tentativas limitadas. E globbing serve especificamente para agir sobre arquivos existentes no sistema, sem precisar listá-los manualmente, deixando o próprio sistema de arquivos "informar" ao script quais itens existem no momento da execução.

## Fontes

- [Glob Expansion in Bash, LinuxSimply](https://linuxsimply.com/bash-scripting-tutorial/expansion/glob-expansion/)
- [Bash Globbing and Wildcards: *, ?, [], globstar and extglob, GoLinuxCloud](https://www.golinuxcloud.com/bash-globbing/)
- [Bash Expansion Order Explained: Brace, Variables, Splitting & Globbing, GoLinuxCloud](https://www.golinuxcloud.com/bash-shell-expansions/)
