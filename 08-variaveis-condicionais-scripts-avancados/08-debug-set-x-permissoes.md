# Depurando scripts com set -x e revisando permissões de execução

## Quando o shellcheck não é suficiente

O arquivo sobre [shebang e shellcheck](04-boas-praticas-shebang-shellcheck.md) mostrou como identificar problemas num script antes mesmo de rodá-lo. Mas alguns bugs só aparecem em tempo de execução, quando uma variável recebe um valor inesperado, ou uma condição se comporta de forma diferente do previsto. Para esses casos, o Bash tem um modo de depuração embutido.

## `set -x`: mostrando cada comando antes de executá-lo

Adicionando `set -x` no início de um script (ou de um trecho específico dele), o Bash passa a imprimir cada comando, já com as variáveis substituídas pelos seus valores reais, antes de executá-lo, prefixado por um sinal de `+`.

```bash
#!/usr/bin/env bash
set -x

ambiente="producao"
if [ "$ambiente" = "producao" ]; then
    echo "Rodando em producao"
fi
```

Ao rodar esse script, a saída mostra não só "Rodando em producao", mas também cada linha de código sendo executada, incluindo o valor já expandido da condição testada, o que torna muito mais fácil enxergar exatamente onde o comportamento começa a divergir do esperado.

Para desligar esse modo de rastreamento no meio do script, sem precisar remover a linha, usa-se `set +x`. Isso permite isolar o rastreamento só ao trecho específico que está sendo investigado, em vez de poluir a saída inteira do script com informação de depuração desnecessária:

```bash
set -x
# trecho suspeito aqui
set +x
```

## `set -e`: parando no primeiro erro, em vez de seguir cegamente

Vale mencionar um companheiro comum do `set -x`, embora sirva a um propósito diferente: por padrão, um script de shell continua rodando mesmo depois que um comando falha, um comportamento que já apareceu de forma indireta no arquivo sobre [estruturação de scripts](../06-fundamentos-so-unix-shell/06-estruturando-scripts-shell.md), quando cada etapa crítica precisava ser verificada manualmente com `$?`. O `set -e` muda esse padrão, fazendo o script inteiro parar imediatamente assim que qualquer comando retornar um código de saída diferente de zero, evitando que o script continue executando etapas seguintes sobre uma base que já falhou.

## Revisando permissões de execução

Fechando este bloco de aulas, vale um lembrete direto sobre um problema bem mais básico, mas extremamente comum: um script escrito corretamente simplesmente não roda se não tiver permissão de execução, conforme já detalhado no arquivo sobre [permissões: chmod e chown](../03-arquivos-permissoes-processos/02-permissoes-chmod-chown.md). O sintoma costuma ser uma mensagem de "Permission denied" ao tentar rodar `./script.sh`, mesmo com o conteúdo do script perfeito.

```
chmod +x script.sh
```

Vale lembrar também que existe uma diferença entre rodar um script com `./script.sh`, que exige a permissão de execução, e rodá-lo como `bash script.sh`, que não exige, já que nesse segundo caso é o próprio Bash quem lê e interpreta o arquivo como um script comum, em vez de o sistema tentar executá-lo diretamente como um programa.

## Fontes

- [Bash set Command: set -e, set -x, and set -u Explained, Linuxize](https://linuxize.com/post/bash-set-command/)
- [Using set -x and set -e in Shell Scripting, HackerOne](https://www.hackerone.com/blog/using-set-x-and-set-e-shell-scripting-guide-enhanced-debugging-and-error-handling)
- [How to Handle Script Debugging with set -x, oneuptime](https://oneuptime.com/blog/post/2026-01-24-bash-debugging-set-x/view)
