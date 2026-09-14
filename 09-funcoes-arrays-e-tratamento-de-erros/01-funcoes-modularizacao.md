# Modularizando código em Bash com funções

## De um script único a peças reaproveitáveis

O arquivo sobre [estruturação de scripts](../06-fundamentos-so-unix-shell/06-estruturando-scripts-shell.md) já mostrou um exemplo simples de função. Vale aprofundar esse conceito, porque é ele que separa um script curto e descartável de um script organizado, fácil de manter e de reaproveitar em outros contextos.

## Definindo e chamando uma função

Uma função em Bash agrupa um bloco de comandos sob um nome, para que esse bloco possa ser chamado várias vezes sem repetir código:

```bash
verificar_servico() {
    systemctl status "$1"
}

verificar_servico nginx
verificar_servico postgresql
```

Repare que a função é definida antes de ser usada, e chamada como se fosse qualquer outro comando do sistema, só pelo nome. Isso não é coincidência: dentro do shell, funções e programas externos são chamados exatamente da mesma forma, o que reforça a ideia, já apresentada no arquivo sobre a [filosofia Unix](../06-fundamentos-so-unix-shell/03-filosofia-unix.md), de que tudo se encaixa numa interface comum e previsível.

## Argumentos de uma função

Dentro de uma função, os argumentos recebidos na chamada ficam disponíveis exatamente como os argumentos posicionais de um script inteiro, apresentados no arquivo sobre [parâmetros e getopts](../08-variaveis-condicionais-scripts-avancados/05-parametros-argumentos-getopts.md): `$1`, `$2`, `$@`, `$#`, só que referindo-se aos argumentos passados para a função, não ao script como um todo.

```bash
saudacao() {
    echo "Ola, $1! Voce tem $2 anos."
}

saudacao "Filipe" 30
```

## Devolvendo um resultado

Diferente de funções em linguagens de programação tradicionais, uma função Bash não "devolve" um valor no sentido usual. O comando `return`, quando usado, só devolve um código de saída numérico, entre 0 e 255, o mesmo conceito de código de saída já apresentado no arquivo sobre [processos e exit codes](../07-processamento-texto-e-automacao/06-processos-pgrep-pkill-xargs-exit-codes.md), servindo para indicar sucesso ou tipo de falha, não para transportar um valor de verdade, como um texto ou um número calculado.

Para de fato obter um valor calculado dentro da função, o padrão é usar `echo` para imprimir o resultado, e capturar essa saída de fora, usando substituição de comando:

```bash
somar() {
    local resultado=$(( $1 + $2 ))
    echo "$resultado"
}

total=$(somar 5 3)
echo "O total e $total"
```

## Por que modularizar vale o esforço

Reunir lógica repetida dentro de funções traz benefícios que só ficam evidentes em scripts um pouco maiores: uma correção de bug precisa ser feita num único lugar, não em cada trecho repetido; o código fica mais fácil de ler, já que um nome de função bem escolhido, como `verificar_servico`, explica sozinho o que aquele bloco faz, sem precisar reler cada linha; e fica muito mais simples testar uma peça isolada do script, sem precisar rodar o programa inteiro de ponta a ponta. Essa organização se torna ainda mais importante quando combinada ao escopo restrito das variáveis dentro de funções, tema do [próximo arquivo](02-escopo-local-em-funcoes.md).

## Fontes

- [Bash Functions, Linuxize](https://linuxize.com/post/bash-functions/)
- [How to Return Value From a Bash Function, KodeKloud](https://kodekloud.com/blog/return-value-from-bash-function/)
- [Bash function: define, call, arguments, local, and return, GoLinuxCloud](https://www.golinuxcloud.com/bash-function/)
