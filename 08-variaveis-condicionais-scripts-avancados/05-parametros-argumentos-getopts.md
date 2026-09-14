# Recebendo parâmetros de entrada: argumentos posicionais e getopts

## Scripts que reagem ao que é digitado, não só ao que foi escrito neles

Até aqui, a maioria dos exemplos deste repositório trouxe valores fixos, escritos diretamente no código do script. Scripts realmente reutilizáveis, porém, costumam receber informação de fora, no momento em que são chamados, através de argumentos passados na linha de comando.

## Argumentos posicionais: a forma mais simples

Quando um script é chamado como `./backup.sh producao 7`, cada palavra depois do nome do script fica disponível através de variáveis especiais:

- `$0` é o nome do próprio script.
- `$1`, `$2`, `$3`... são o primeiro, segundo, terceiro argumento, e assim por diante (a partir de dez argumentos, é preciso usar chaves, como `${10}`).
- `$#` é a quantidade total de argumentos recebidos.
- `$@` representa todos os argumentos, cada um preservado como uma palavra separada, especialmente quando usado como `"$@"` entre aspas duplas, seguindo a mesma lógica já explicada no arquivo sobre [aspas e expansão de variáveis](02-aspas-expansao-variaveis.md).

```bash
#!/usr/bin/env bash
ambiente=$1
dias_retencao=$2

echo "Fazendo backup do ambiente $ambiente, mantendo $dias_retencao dias"
```

Quando é preciso processar os argumentos um a um, sem saber de antemão quantos existem, o comando `shift` descarta o primeiro argumento e desloca todos os outros uma posição para trás, então `$2` vira `$1`, e assim sucessivamente, permitindo percorrer a lista inteira com um loop.

## `getopts`: quando a ordem dos argumentos não deveria importar

Argumentos posicionais funcionam bem quando a ordem é fixa e previsível, mas ficam frágeis quando um script aceita várias opções, algumas obrigatórias, outras não, e o usuário deveria poder passá-las em qualquer ordem, como se vê em praticamente qualquer comando padrão do Linux, por exemplo `tar -x -v -f arquivo.tar`. O `getopts` é a forma nativa e portável de implementar esse estilo de opções num script de shell.

```bash
#!/usr/bin/env bash

while getopts "a:d:v" opcao; do
    case $opcao in
        a) ambiente="$OPTARG" ;;
        d) dias="$OPTARG" ;;
        v) modo_verboso=true ;;
        *) echo "Opcao invalida"; exit 1 ;;
    esac
done

echo "Ambiente: $ambiente, dias: $dias"
```

A string `"a:d:v"` define as opções aceitas: `a` e `d` esperam um valor associado (indicado pelos dois-pontos depois da letra), enquanto `v` funciona como uma flag simples, sem valor. Dentro do loop, `$OPTARG` guarda o valor recebido pela opção atual, e o `case`, apresentado com mais detalhe no arquivo sobre [loops e case](07-loops-e-case.md), direciona cada opção para o tratamento correspondente. Rodando esse script como `./deploy.sh -a producao -d 30 -v`, o `getopts` reconhece cada opção independentemente da ordem em que foram passadas.

## Escolhendo entre as duas abordagens

Para scripts simples, com poucos argumentos numa ordem fixa e conhecida, argumentos posicionais são suficientes e mais diretos de escrever. Para scripts com várias opções configuráveis, especialmente aqueles pensados para serem usados por outras pessoas (ou lembrados meses depois pelo próprio autor), `getopts` produz uma interface mais clara e mais tolerante a erro de uso, ao custo de um pouco mais de código para configurar.

## Fontes

- [Bash Positional Arguments: How to Use $1, $2, $@, and shift, Linuxize](https://linuxize.com/post/bash-positional-parameters/)
- [How to Use Bash Getopts With Examples, KodeKloud](https://kodekloud.com/blog/bash-getopts/)
- [Handling positional parameters, The Bash Hackers Wiki](https://flokoe.github.io/bash-hackers-wiki/scripting/posparams/)
