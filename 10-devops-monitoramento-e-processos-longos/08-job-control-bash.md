# Operadores e comandos para gerenciar processos no Bash: job control

## Uma camada de controle dentro do próprio terminal

Os arquivos sobre [pgrep, pkill e exit codes](../07-processamento-texto-e-automacao/06-processos-pgrep-pkill-xargs-exit-codes.md) e [nohup, screen e tmux](06-nohup-screen-tmux.md) já mostraram formas de gerenciar processos de fora, ou de mantê-los rodando além da sessão atual. Existe ainda uma camada mais imediata, embutida no próprio shell interativo, chamada job control (controle de jobs), que permite pausar, retomar e alternar processos entre primeiro e segundo plano, sem sair do terminal nem precisar de ferramentas externas.

## O `&`: iniciando algo em segundo plano

Um comando terminado em `&` roda em segundo plano, devolvendo o controle do terminal imediatamente:

```
./processar_dados.sh &
```

O shell atribui a esse processo um número de job, geralmente exibido entre colchetes, como `[1]`.

## `jobs`: vendo o que está rodando na sessão atual

O comando `jobs` lista todos os processos em segundo plano (ou pausados) associados à sessão de terminal atual:

```
jobs
```

Diferente do `ps`, apresentado no arquivo sobre [processos: ps, top e htop](../03-arquivos-permissoes-processos/03-processos-ps-top-htop.md), que mostra processos do sistema inteiro, o `jobs` mostra só os processos que essa sessão específica de shell iniciou.

## Pausando, retomando e alternando entre planos

Um processo rodando em primeiro plano pode ser pausado (não encerrado) com o atalho `Ctrl+Z`, devolvendo o controle ao terminal imediatamente. A partir daí, dois comandos decidem o que fazer com esse processo pausado:

- `bg` retoma o processo, mas mantendo ele em segundo plano.
- `fg` traz um processo de volta para o primeiro plano, como se nunca tivesse ido para segundo plano.

Ambos aceitam um identificador de job como argumento, escrito com `%` seguido do número, por exemplo `fg %1`, útil quando existe mais de um processo gerenciado ao mesmo tempo na mesma sessão.

## `wait`: pausando o script até um job terminar

Dentro de um script, o comando `wait` interrompe a execução até que um processo em segundo plano termine, o que é útil quando várias tarefas são disparadas em paralelo, mas o script só deve continuar depois que todas tiverem concluído:

```bash
processar_arquivo_a.sh &
processar_arquivo_b.sh &
wait
echo "Os dois processamentos terminaram"
```

## `disown`: soltando um processo da sessão atual

Por padrão, processos em segundo plano ainda estão vinculados à sessão que os iniciou, e recebem um sinal de encerramento se essa sessão for fechada, o mesmo problema já apresentado no arquivo sobre [nohup, screen e tmux](06-nohup-screen-tmux.md). O `disown` remove esse vínculo, sem precisar ter usado `nohup` desde o início:

```
./tarefa_longa.sh &
disown
```

## `kill` com job specs: encerrando pelo número do job

O `kill`, além de aceitar um PID como já visto no arquivo sobre [pgrep e pkill](../07-processamento-texto-e-automacao/06-processos-pgrep-pkill-xargs-exit-codes.md), também aceita diretamente um identificador de job:

```
kill %1
```

Esse comando envia o sinal padrão de encerramento (SIGTERM) para o job número 1, sem precisar descobrir o PID correspondente antes. Sinais diferentes podem ser especificados, como `kill -9 %1` para um encerramento forçado (SIGKILL), o mesmo conceito de sinais já apresentado antes.

## Um conjunto de ferramentas para o dia a dia interativo

Job control é especialmente útil no uso interativo do terminal, quando se está trabalhando diretamente numa sessão e precisa, por exemplo, pausar rapidamente uma tarefa para rodar outro comando urgente, sem perder o progresso da primeira. Para automação de verdade, sem intervenção humana, as ferramentas já apresentadas nos arquivos sobre [systemd](07-systemd-servicos-personalizados.md) e [trap e tratamento de erros](../09-funcoes-arrays-e-tratamento-de-erros/06-trap-tratamento-erros.md) tendem a ser mais adequadas, mas entender job control continua sendo parte essencial de trabalhar com confiança e agilidade dentro de qualquer terminal Linux.

## Fontes

- [Bash Job Control, Medium](https://copyconstruct.medium.com/bash-job-control-4a36da3e4aa7)
- [10 Linux/Unix Bash and KSH Shell Job Control Examples, nixCraft](https://www.cyberciti.biz/howto/unix-linux-job-control-command-examples-for-bash-ksh-shell/)
- [How to Handle Background Processes in Bash, oneuptime](https://oneuptime.com/blog/post/2026-01-24-bash-background-processes/view)
