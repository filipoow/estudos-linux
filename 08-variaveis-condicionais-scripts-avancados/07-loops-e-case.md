# Estruturando loops e usando case para condicionais múltiplas

## Recapitulando e aprofundando os loops

O arquivo sobre [estruturação de scripts](../06-fundamentos-so-unix-shell/06-estruturando-scripts-shell.md) já apresentou o `for` e mencionou o `while`. Vale reunir os três tipos de loop do Bash lado a lado, já que cada um se encaixa melhor numa situação diferente.

**`for`**: percorre uma lista de valores já conhecida, um de cada vez.

```bash
for servico in nginx postgresql redis; do
    systemctl status "$servico"
done
```

**`while`**: repete enquanto uma condição continuar verdadeira, útil quando não se sabe de antemão quantas repetições serão necessárias.

```bash
while ! ping -c 1 servidor.local &> /dev/null; do
    echo "Aguardando servidor responder"
    sleep 2
done
```

Esse exemplo reaproveita o `ping`, apresentado no arquivo sobre [diagnóstico de rede](../05-rede-usuarios-seguranca/06-ping-nslookup-diagnostico.md), repetindo a tentativa a cada dois segundos até o servidor responder.

**`until`**: é o espelho do `while`, repete enquanto a condição for falsa, parando assim que ela se tornar verdadeira. O mesmo exemplo acima poderia ser reescrito de forma um pouco mais direta com `until`:

```bash
until ping -c 1 servidor.local &> /dev/null; do
    echo "Aguardando servidor responder"
    sleep 2
done
```

A escolha entre `while` e `until` é, na maioria das vezes, uma questão de qual das duas formas deixa a condição mais fácil de ler, já que tecnicamente é sempre possível reescrever um no formato do outro invertendo a condição com `!`.

## `case`: várias comparações, sem empilhar `elif`

Quando uma mesma variável precisa ser comparada contra várias opções possíveis, um encadeamento longo de `elif`, apresentado no arquivo sobre [condicionais](03-condicionais-if-elif-else-ternario.md), fica repetitivo e difícil de ler. O `case` resolve exatamente esse cenário:

```bash
case $1 in
    iniciar)
        systemctl start nginx
        ;;
    parar)
        systemctl stop nginx
        ;;
    reiniciar)
        systemctl restart nginx
        ;;
    *)
        echo "Uso: $0 {iniciar|parar|reiniciar}"
        exit 1
        ;;
esac
```

Cada padrão é seguido de `)`, o bloco de comandos correspondente, e `;;` marcando o fim daquele caso. O padrão `*)` funciona como um "caso padrão", capturando qualquer valor que não bateu com nenhuma das opções anteriores, de forma parecida com o `else` final de um `if`/`elif`. O `case` também aceita múltiplos valores separados por `|` num mesmo padrão, e até um uso limitado de curingas, como `s*)` para casar qualquer valor começando com "s".

## Uma observação importante: case não é um loop

Vale reforçar algo que confunde quem está começando: apesar do nome parecido com estruturas de repetição de outras linguagens, o `case` do Bash não repete nada, ele avalia a condição uma única vez e executa o bloco correspondente, exatamente como um `if`/`elif`/`else` mais organizado, só que voltado para comparar um único valor contra várias opções possíveis, em vez de avaliar condições diferentes entre si.

## Fontes

- [How to Use Until Loops and Case Statements in Bash, Tecmint](https://pro.tecmint.com/bash-until-loop-and-case-statement/)
- [Bash case Statement: Syntax and Examples, phoenixNAP](https://phoenixnap.com/kb/bash-case-statement)
- [BASH: using loops, for, while, until, with examples, Setevoy](https://setevoy.substack.com/p/bash-using-loops-for-while-until-with-examples-f519eda7f41b)
