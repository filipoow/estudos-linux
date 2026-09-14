# Estruturas condicionais: if, elif, else e expressões ternárias

## Retomando o básico, e indo além dele

O arquivo sobre [estruturação de scripts](../06-fundamentos-so-unix-shell/06-estruturando-scripts-shell.md) já apresentou o `if` mais simples, com um `else` opcional. Scripts reais, porém, quase sempre precisam avaliar mais de duas possibilidades, e é aí que entra o `elif`.

## `elif`: encadeando várias condições

O `elif` (contração de "else if") permite testar uma nova condição quando a anterior não foi verdadeira, sem precisar aninhar vários blocos `if` um dentro do outro.

```bash
uso=$(df / | tail -n 1 | awk '{print $5}' | tr -d '%')

if [ "$uso" -lt 50 ]; then
    echo "Disco tranquilo"
elif [ "$uso" -lt 80 ]; then
    echo "Disco em atencao"
elif [ "$uso" -lt 95 ]; then
    echo "Disco quase cheio"
else
    echo "Disco critico"
fi
```

Esse exemplo reaproveita o `df` e o `awk`, já apresentados nos arquivos sobre [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md) e [cut, awk e tr](../07-processamento-texto-e-automacao/02-cut-awk-tr-manipulacao-dados.md), para classificar o uso de disco em quatro faixas diferentes. O shell avalia as condições na ordem em que aparecem, e executa só o primeiro bloco cuja condição for verdadeira, ignorando o restante.

## Expressões ternárias: um `if` compacto, só para aritmética

Diferente de linguagens como JavaScript ou C, o Bash não tem um operador ternário de uso geral. O que existe é uma forma ternária restrita ao contexto de avaliação aritmética, dentro de `$(( ))`, já apresentado no arquivo sobre [cut, awk e tr](../07-processamento-texto-e-automacao/02-cut-awk-tr-manipulacao-dados.md):

```bash
maior=$(( a > b ? a : b ))
```

Essa linha guarda em `maior` o valor de `a` se `a` for maior que `b`, ou o valor de `b` caso contrário. É útil para decisões numéricas simples e diretas, mas importante notar sua limitação: funciona só dentro da avaliação aritmética, não substitui um `if` normal quando a decisão envolve texto, comandos, ou lógica mais complexa que uma simples comparação numérica.

## Escolhendo entre um e outro

Para decisões simples envolvendo números, a expressão ternária dentro de `$(( ))` é mais compacta e legível numa única linha. Para qualquer coisa além disso, comparação de texto, verificação de existência de arquivo (como já visto no arquivo sobre [estruturação de scripts](../06-fundamentos-so-unix-shell/06-estruturando-scripts-shell.md)), ou múltiplas condições encadeadas, o conjunto `if`/`elif`/`else` continua sendo a ferramenta certa, por ser mais explícito e mais flexível.

## Fontes

- [How to Use the Ternary Conditional Operator in Bash, Baeldung on Linux](https://www.baeldung.com/linux/bash-ternary-conditional-operator)
- [How To Script Error Free Bash If Statement?, Shell Tips](https://www.shell-tips.com/bash/if-statement/)
- [Usage of Ternary Operator in Bash, LinuxSimply](https://linuxsimply.com/bash-scripting-tutorial/operator/ternary-operator/)
