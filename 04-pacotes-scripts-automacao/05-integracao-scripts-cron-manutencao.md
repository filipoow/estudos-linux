# Juntando as peças: scripts e agendamento na manutenção de sistemas

## Um sistema que cuida de si mesmo

Cada arquivo anterior deste bloco de aulas apresentou uma peça separada: o gerenciamento de pacotes com [APT e dpkg](01-apt-dpkg-gerenciamento-pacotes.md), o encadeamento de comandos com [redirecionadores e pipes](02-redirecionamento-pipes.md), a criação de [scripts em shell](03-scripts-shell.md), e o agendamento automático com o [CronTab](04-crontab-agendamento.md). Nenhuma dessas peças, sozinha, resolve um problema real de administração de sistemas. A força aparece quando elas são combinadas.

Um administrador de sistemas experiente raramente executa tarefas de manutenção manualmente, dia após dia. Em vez disso, ele escreve um script que já faz tudo que seria feito manualmente, testa esse script até ter certeza que funciona como esperado, e então entrega esse script para o cron rodar sozinho, no horário certo, sem depender de ninguém lembrar de nada.

## Um exemplo completo, do início ao fim

Imagine a tarefa de manter um servidor atualizado e limpo, removendo logs antigos que não são mais necessários. O primeiro passo é escrever o script, reunindo comandos já apresentados ao longo deste repositório:

```bash
#!/bin/bash
echo "Atualizando lista de pacotes"
apt-get update -y

echo "Aplicando atualizacoes disponiveis"
apt-get upgrade -y

echo "Removendo logs com mais de 30 dias"
find /var/log -name "*.log" -mtime +30 -delete

echo "Manutencao concluida em $(date)" >> /var/log/manutencao.log
```

Repare que esse script usa vários conceitos já explicados separadamente: os comandos do [APT](01-apt-dpkg-gerenciamento-pacotes.md) para atualizar o sistema, e o redirecionador `>>`, apresentado no arquivo sobre [pipes e redirecionadores](02-redirecionamento-pipes.md), para registrar um histórico de quando a manutenção rodou, sem apagar os registros anteriores.

O segundo passo é dar permissão de execução a esse arquivo, como já mostrado no arquivo sobre [scripts em shell](03-scripts-shell.md):

```
chmod +x manutencao.sh
```

O terceiro e último passo é agendar esse script para rodar sozinho, usando o `crontab -e`, apresentado no [arquivo anterior](04-crontab-agendamento.md), por exemplo toda madrugada de domingo às 4h:

```
0 4 * * 0 /home/filipe/scripts/manutencao.sh
```

## Por que isso é o coração da administração de sistemas

Esse tipo de integração, script mais agendamento, é basicamente o que sustenta a operação de servidores no mundo real, muito além de qualquer exemplo didático. Backups automáticos, renovação de certificados de segurança, limpeza de espaço em disco, monitoramento com alertas, praticamente toda rotina crítica de manutenção de um servidor Linux segue exatamente essa mesma lógica: um script confiável, testado, e um agendamento automático que garante que ele rode sem falhas, mesmo que ninguém esteja olhando naquele momento específico.

É também por isso que vale tanto a pena entender cada peça isoladamente antes de juntar tudo, como foi feito ao longo destes arquivos: um script mal escrito, rodando automaticamente todo dia às 4 da manhã sem ninguém observando, pode causar bem mais estrago do que o mesmo erro cometido manualmente, uma vez, sob supervisão. Automação multiplica tanto os acertos quanto os erros.

## Fontes

- [find(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/find.1.html)
- [Bash Scripting Tutorial, freeCodeCamp](https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/)
- [Crontab Format & Syntax Explained, adminschoice](https://adminschoice.com/crontab-quick-reference/)
