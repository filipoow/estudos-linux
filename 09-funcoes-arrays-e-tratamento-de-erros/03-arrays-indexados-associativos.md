# Arrays indexados e associativos no Bash

## Guardando mais de um valor numa única variável

Todas as variáveis apresentadas até aqui neste repositório guardam um único valor por vez. Um array permite guardar vários valores relacionados sob um único nome, acessíveis individualmente por posição ou por uma chave. O Bash oferece dois tipos bem diferentes: arrays indexados e arrays associativos.

## Arrays indexados: uma lista ordenada

Um array indexado guarda valores numa sequência numerada, começando em zero, parecido com listas na maioria das linguagens de programação.

```bash
servicos=("nginx" "postgresql" "redis")
```

Para acessar um elemento específico, usa-se a posição entre colchetes:

```bash
echo "${servicos[0]}"
```

Esse comando imprime `nginx`, o primeiro elemento. Para acessar todos os elementos de uma vez, existe uma sintaxe especial:

```bash
echo "${servicos[@]}"
```

E para percorrer cada elemento individualmente, o array se combina naturalmente com o `for`, já apresentado no arquivo sobre [loops e case](../08-variaveis-condicionais-scripts-avancados/07-loops-e-case.md):

```bash
for servico in "${servicos[@]}"; do
    systemctl status "$servico"
done
```

Repare o uso de aspas duplas ao redor de `${servicos[@]}`, seguindo a mesma lógica já explicada no arquivo sobre [aspas e expansão de variáveis](../08-variaveis-condicionais-scripts-avancados/02-aspas-expansao-variaveis.md): sem aspas, um elemento com espaço no nome seria dividido em várias palavras separadas, quebrando a iteração.

## Arrays associativos: pares de chave e valor

Um array associativo, diferente do indexado, usa texto como chave em vez de números sequenciais, funcionando de forma parecida com um dicionário. Diferente do indexado, ele precisa ser declarado explicitamente antes de ser usado, com `declare -A`:

```bash
declare -A servidores
servidores[web]="192.168.1.10"
servidores[banco]="192.168.1.20"
```

Também é possível declarar e popular tudo de uma vez:

```bash
declare -A servidores=( [web]="192.168.1.10" [banco]="192.168.1.20" )
```

Acessar um valor funciona pela chave, não pela posição:

```bash
echo "${servidores[web]}"
```

Para percorrer um array associativo, é preciso pegar as chaves separadamente, usando `!` antes do nome do array, e então buscar o valor correspondente a cada uma:

```bash
for chave in "${!servidores[@]}"; do
    echo "$chave: ${servidores[$chave]}"
done
```

## Quando usar cada tipo

Arrays indexados são a escolha natural quando a ordem importa, ou quando os elementos são simplesmente uma lista, sem nenhum nome específico associado a cada um, como a lista de serviços do primeiro exemplo. Arrays associativos fazem mais sentido quando cada valor tem um nome ou identificador próprio, e o acesso por esse nome é mais natural do que por posição, como associar cada serviço ao seu respectivo endereço IP. Essa distinção se torna especialmente útil em scripts de automação mais elaborados, como os exemplos de manutenção já vistos no arquivo sobre [integração de scripts e cron](../04-pacotes-scripts-automacao/05-integracao-scripts-cron-manutencao.md), onde configurações de vários ambientes ou servidores diferentes precisam ser organizadas de forma clara dentro do próprio script.

## Fontes

- [Bash Associative Array: How to Declare and Access It, phoenixNAP](https://phoenixnap.com/kb/bash-associative-array)
- [How to Handle Arrays in Bash Scripts, oneuptime](https://oneuptime.com/blog/post/2026-01-24-bash-arrays/view)
- [Bash Associative Array Explained With Examples In Linux, OSTechNix](https://ostechnix.com/bash-associative-array/)
