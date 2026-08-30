# Agendando tarefas automáticas com o CronTab

## Um script que roda sozinho, na hora certa

O arquivo anterior, sobre [scripts em shell](03-scripts-shell.md), resolve o problema de repetir uma sequência de comandos sem precisar digitar tudo de novo toda vez. Mas ainda falta uma peça: alguém precisa lembrar de rodar aquele script. O cron é o serviço do Linux responsável justamente por isso, ele roda em segundo plano, o tempo todo, verificando se chegou a hora de executar alguma tarefa agendada, e disparando o comando ou script correspondente sozinho, sem intervenção humana.

## A ferramenta usada para configurar isso: `crontab`

Cada usuário do sistema pode ter sua própria lista de tarefas agendadas, chamada de crontab. Para editar essa lista, usa-se o comando `crontab -e`, que abre um editor de texto (geralmente configurado para usar o `nano`, apresentado no arquivo sobre [editores de texto](../02-terminal-na-pratica/03-editores-texto-terminal.md)) com a lista atual de tarefas daquele usuário.

```
crontab -e
```

Para apenas visualizar a lista atual sem editar, existe o comando `crontab -l`.

## A sintaxe: cinco campos de tempo

Cada linha dentro do crontab segue um formato fixo de cinco campos de tempo, seguidos do comando a ser executado:

```
minuto hora dia-do-mes mes dia-da-semana comando
```

- **Minuto**: de 0 a 59.
- **Hora**: de 0 a 23, no formato de 24 horas.
- **Dia do mês**: de 1 a 31.
- **Mês**: de 1 a 12.
- **Dia da semana**: de 0 a 7, onde tanto 0 quanto 7 representam domingo, e os números do meio seguem a sequência normal da semana.

Um asterisco (`*`) em qualquer desses campos significa "qualquer valor", ou seja, aquele campo não impõe nenhuma restrição. Por exemplo, para rodar um script de backup todo dia às 3 da manhã:

```
0 3 * * * /home/filipe/scripts/backup.sh
```

E para rodar uma limpeza de diretório temporário toda sexta-feira às 22h:

```
0 22 * * 5 /home/filipe/scripts/limpeza.sh
```

## Recursos adicionais de sintaxe

Além do asterisco simples, o crontab aceita algumas combinações úteis:

- **Listas, separadas por vírgula**: `0 9,18 * * *` roda às 9h e também às 18h.
- **Intervalos, com hífen**: `0 9 * * 1-5` roda às 9h, mas só de segunda a sexta.
- **Passos, com barra**: `*/15 * * * *` roda a cada quinze minutos.

Um detalhe que costuma confundir: quando tanto o campo de dia do mês quanto o de dia da semana são especificados ao mesmo tempo (diferentes de `*` nos dois), o cron combina os dois com "ou", não com "e". Ou seja, uma tarefa configurada para "dia 13" e "sexta-feira" ao mesmo tempo roda tanto em qualquer dia 13 quanto em qualquer sexta-feira, não exclusivamente em sextas-feiras 13.

## Usos comuns: backups e limpeza automática

Dois dos usos mais frequentes do cron em administração de sistemas são justamente backups periódicos e limpeza de arquivos temporários, exatamente os exemplos usados acima. A lógica por trás é sempre a mesma: em vez de depender de alguém lembrar de rodar uma tarefa de manutenção manualmente, ela é configurada uma única vez, e o próprio sistema garante que ela aconteça no horário certo, dali em diante, sem falhar por esquecimento humano.

## Fontes

- [Crontab Format & Syntax Explained, adminschoice](https://adminschoice.com/crontab-quick-reference/)
- [Cron Expression Examples, Crontab.guru](https://crontab.guru/examples.html)
- [Crontab syntax explained, Stack Harbor Knowledge Base](https://stackharbor.com/en/knowledge-base/crontab-syntax-guide/)
