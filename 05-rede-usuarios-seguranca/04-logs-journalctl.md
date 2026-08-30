# A importância dos logs: monitorando eventos em tempo real com journalctl

## Por que registrar tudo que acontece

Um sistema Linux gera, o tempo todo, registros detalhados de praticamente tudo que faz: serviços que iniciam ou falham, tentativas de login, mensagens do próprio kernel, erros de aplicações. Esses registros são os logs, e são, junto com os comandos de monitoramento já apresentados no arquivo sobre [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md), a principal fonte de informação para entender o que aconteceu num sistema depois do fato, algo que um comando como `htop`, que só mostra o presente, não consegue oferecer.

Sem logs, diagnosticar por que um serviço caiu de madrugada, ou identificar uma tentativa de acesso suspeita, seria praticamente impossível. É por isso que logs são considerados, tanto para diagnóstico quanto para segurança, um dos pilares mais importantes da administração de sistemas.

## O journal do systemd

Nas distribuições modernas que usam o systemd como sistema de inicialização, já apresentado no arquivo sobre o [processo de inicialização](../01-fundamentos/07-processo-inicializacao.md), os logs são centralizados por um serviço chamado journald, que guarda tudo num formato binário estruturado, dentro de `/var/log/journal`. Diferente dos arquivos de texto simples tradicionais, esse formato binário permite buscas e filtros muito mais rápidos e precisos.

A ferramenta usada para ler esses registros é o `journalctl`.

## Usando o journalctl

Rodado sozinho, o `journalctl` mostra todo o histórico de logs disponível, começando pelo mais antigo:

```
journalctl
```

Na prática, quase sempre é mais útil filtrar. Alguns dos filtros mais usados:

- **Acompanhar em tempo real**, como um `tail -f`: `journalctl -f`, mostra novas mensagens à medida que acontecem, sem precisar ficar rodando o comando de novo.
- **Filtrar por um serviço específico**: `journalctl -u nginx`, mostra só os registros ligados àquele serviço (unit) do systemd.
- **Filtrar por período**: `journalctl --since "2026-08-01" --until "2026-08-02"`, mostra só os registros dentro de um intervalo de datas.
- **Filtrar por gravidade**: `journalctl -p err`, mostra só mensagens classificadas como erro ou mais graves, ignorando avisos e informações rotineiras.
- **Ver só as mensagens mais recentes**: `journalctl -n 50`, funciona de forma parecida com o `tail`, apresentado no arquivo sobre [visualização e filtragem de conteúdo](../02-terminal-na-pratica/02-visualizacao-filtragem-conteudo.md), mostrando as últimas cinquenta entradas.
- **Ver só mensagens do kernel**: `journalctl -k`, equivalente ao `dmesg` apresentado no arquivo sobre [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md).

Vale notar que, por padrão, só o root e usuários dos grupos `adm` ou `systemd-journal` podem ler o journal completo, mais uma aplicação prática do princípio de menor privilégio apresentado no arquivo sobre [segurança básica](03-seguranca-basica.md).

## O hábito que faz diferença

A maior parte dos administradores de sistema só aprende a valorizar o `journalctl -f` depois de precisar dele numa emergência, acompanhando ao vivo um serviço que está falhando repetidamente, tentando entender o motivo exato da falha no momento em que ela acontece. Desenvolver o hábito de checar logs regularmente, e não só quando algo já quebrou, é o que separa uma reação tardia de uma detecção precoce de problema.

## Fontes

- [journalctl Command in Linux: Query and Filter System Logs, Linuxize](https://linuxize.com/post/journalctl-command-in-linux/)
- [journalctl(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/journalctl.1.html)
- [systemd/Journal, ArchWiki](https://wiki.archlinux.org/title/Systemd/Journal)
