# Prevendo e tratando erros: trap e códigos de saída

## Nem todo erro é previsível com um if

O arquivo sobre [debug com set -x e permissões](../08-variaveis-condicionais-scripts-avancados/08-debug-set-x-permissoes.md) já mostrou o `set -e`, que interrompe um script assim que um comando falha. Mas parar não é sempre suficiente: muitas vezes é preciso reagir a esse encerramento, seja limpando arquivos temporários, seja avisando que algo deu errado, mesmo quando o script é interrompido de forma inesperada, como um Ctrl+C do próprio usuário. É esse cenário que o `trap` resolve.

## O que é um trap

O `trap` registra uma função ou comando para ser executado automaticamente quando o script recebe um determinado sinal, ou atinge um evento especial, como o próprio encerramento do script.

```bash
limpar() {
    echo "Removendo arquivos temporarios"
    rm -rf /tmp/processamento_$$
}

trap limpar EXIT
```

O evento `EXIT` é especial: ele não é um sinal do sistema operacional propriamente dito, ele dispara sempre que o script termina, seja terminando normalmente, seja terminando por causa de um `exit` explícito, seja interrompido por conta do `set -e`, já apresentado no arquivo sobre [debug e permissões](../08-variaveis-condicionais-scripts-avancados/08-debug-set-x-permissoes.md). Isso garante que a função de limpeza rode em praticamente qualquer cenário de encerramento, não só no caminho feliz onde tudo funciona.

## Reagindo a sinais específicos

Além do `EXIT`, o `trap` também pode reagir a sinais reais enviados por fora do script, como o `SIGINT`, enviado quando alguém aperta Ctrl+C:

```bash
trap 'echo "Interrompido pelo usuario, encerrando com seguranca"; exit 1' SIGINT
```

Isso permite que um script reaja de forma controlada a uma interrupção manual, em vez de simplesmente morrer no meio de uma operação sensível, como uma escrita em disco pela metade.

## O evento ERR: reagindo especificamente a falhas

Existe ainda o evento `ERR`, que dispara quando qualquer comando do script retorna um código de saída diferente de zero, o mesmo conceito já apresentado no arquivo sobre [processos e exit codes](../07-processamento-texto-e-automacao/06-processos-pgrep-pkill-xargs-exit-codes.md). Diferente do `EXIT`, que sempre dispara ao final, o `ERR` dispara no momento exato da falha, permitindo registrar exatamente qual comando causou o problema:

```bash
trap 'echo "Falha na linha $LINENO"' ERR
```

A variável especial `$LINENO` guarda o número da linha onde o erro ocorreu, uma informação valiosa para diagnosticar rapidamente onde um script parou de funcionar como esperado, especialmente em scripts mais longos.

## Um padrão comum: registrar o trap logo após criar o recurso

Uma boa prática recomendada é registrar o `trap` de limpeza logo depois de criar o recurso que ele deveria limpar, e não só no início do script, de forma isolada. Se um arquivo temporário é criado numa linha, o `trap` responsável por removê-lo deveria vir logo em seguida, garantindo que qualquer falha entre a criação do recurso e o registro do `trap` não deixe esse recurso órfão, sem nada responsável por limpá-lo depois.

```bash
pasta_temp=$(mktemp -d)
trap 'rm -rf "$pasta_temp"' EXIT

# resto do script usando $pasta_temp
```

## Por que isso fecha bem este bloco de aulas

O `trap`, combinado com tudo que já foi visto neste bloco (funções, escopo local, arrays, e o tratamento de erro por código de saída), representa o nível de cuidado esperado num script pensado para rodar sem supervisão direta, exatamente o tipo de automação apresentado lá no arquivo sobre [integração de scripts e cron](../04-pacotes-scripts-automacao/05-integracao-scripts-cron-manutencao.md). Um script que prevê como vai falhar, e o que fazer quando isso acontecer, é a diferença entre uma automação confiável e uma que, mais cedo ou mais tarde, vai deixar bagunça para trás.

## Fontes

- [Bash trap Command Explained, phoenixNAP](https://phoenixnap.com/kb/bash-trap-command)
- [Bash trap: EXIT Cleanup, Signals, ERR and Ctrl+C, GoLinuxCloud](https://www.golinuxcloud.com/bash-trap/)
- [Using Trap to Exit Bash Scripts Cleanly, Putorius](https://www.putorius.net/using-trap-to-exit-bash-scripts-cleanly.html)
