# A importância de variáveis locais em funções

## Por padrão, tudo é global

Depois de aprender a modularizar código com funções, no [arquivo anterior](01-funcoes-modularizacao.md), existe uma armadilha que precisa ficar clara: diferente de muitas linguagens de programação, onde variáveis criadas dentro de uma função automaticamente desaparecem ao final dela, no Bash o comportamento padrão é o oposto. Uma variável criada dentro de uma função, sem nenhuma palavra-chave especial, é global por padrão, visível e alterável por qualquer outra parte do script, inclusive depois que a função já terminou de rodar.

```bash
contador=10

atualizar() {
    contador=999
}

atualizar
echo "$contador"
```

Esse script imprime `999`, não `10`, porque a função `atualizar` alterou diretamente a variável global `contador`, mesmo sem nenhuma intenção explícita de fazer isso. Em scripts pequenos isso raramente causa problema, mas conforme um script cresce e passa a ter várias funções, esse comportamento se torna uma fonte silenciosa de bugs: uma função pode sobrescrever, sem querer, uma variável usada por outra parte completamente diferente do código, só porque os nomes coincidiram.

## `local`: restringindo o escopo de propósito

A palavra-chave `local`, já mencionada de passagem no exemplo de soma do [arquivo anterior](01-funcoes-modularizacao.md), resolve exatamente esse problema. Uma variável declarada com `local` só existe dentro daquela função (e dentro de funções chamadas por ela), desaparecendo assim que a função termina, sem afetar nada fora dali.

```bash
contador=10

atualizar() {
    local contador=999
    echo "Dentro da funcao: $contador"
}

atualizar
echo "Fora da funcao: $contador"
```

Agora o resultado muda: dentro da função, `contador` vale `999`, mas fora dela, a variável global original continua intacta, valendo `10`. A função criou sua própria cópia local, isolada, sem nenhum efeito colateral sobre o resto do script.

## Uma boa prática, não uma obrigação técnica

O Bash não exige o uso de `local`, o script funciona perfeitamente sem ele, em termos de sintaxe. A recomendação de sempre declarar como locais as variáveis internas de uma função é uma convenção de boas práticas, no mesmo espírito das recomendações já vistas no arquivo sobre [shebang e shellcheck](../08-variaveis-condicionais-scripts-avancados/04-boas-praticas-shebang-shellcheck.md): o script continua rodando sem seguir a convenção, mas seguir reduz drasticamente a chance de bugs difíceis de rastrear, especialmente à medida que mais funções vão sendo adicionadas ao mesmo script por diferentes pessoas, ou pela mesma pessoa em momentos diferentes, já sem lembrar todos os nomes de variável usados antes.

## A regra prática

Sempre que uma variável existe só para servir a lógica interna de uma função, ela deveria ser declarada com `local`. Só deveria ficar de fora dessa regra uma variável que, de propósito, precisa comunicar um resultado para fora da função, e mesmo nesse caso, como já visto no arquivo sobre [funções e modularização](01-funcoes-modularizacao.md), a forma mais segura de "exportar" um resultado costuma ser via `echo` e captura por substituição de comando, não por meio de uma variável global deixada de propósito sem `local`.

## Fontes

- [Bash function: define, call, arguments, local, and return, GoLinuxCloud](https://www.golinuxcloud.com/bash-function/)
- [Functions, Bash Scripting Tutorial, ryanstutorials.net](https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php)
- [Bash Functions Explained (Variables, Arguments, Return), phoenixNAP](https://phoenixnap.com/kb/bash-function)
