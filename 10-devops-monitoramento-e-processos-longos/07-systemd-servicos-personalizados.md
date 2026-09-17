# Usando systemd para gerenciar serviços de forma persistente

## Indo além de gerenciar serviços que já existem

Os arquivos sobre [servidor web básico](../05-rede-usuarios-seguranca/05-servidor-web-basico.md) e [Nginx como proxy reverso](../07-processamento-texto-e-automacao/07-nginx-proxy-reverso.md) já mostraram como usar `systemctl` para controlar um serviço já existente, como o Nginx. O que ainda não foi apresentado é como transformar um script ou programa próprio, escrito por quem administra o sistema, num serviço gerenciado da mesma forma pelo systemd, apresentado em detalhe no arquivo sobre o [processo de inicialização](../01-fundamentos/07-processo-inicializacao.md).

## Por que isso é melhor do que rodar com nohup ou tmux

O arquivo anterior, sobre [nohup, screen e tmux](06-nohup-screen-tmux.md), apresentou formas de manter um processo rodando mesmo sem uma sessão de terminal ativa. Essas ferramentas resolvem bem tarefas pontuais, mas têm uma limitação importante para serviços que precisam ficar no ar de forma permanente: nenhuma delas reinicia o processo automaticamente se ele travar, nem garante que o serviço suba sozinho quando o servidor for reiniciado. É exatamente isso que um serviço gerenciado pelo systemd oferece nativamente.

## Criando um arquivo de unidade (unit file)

Um serviço personalizado é descrito por um arquivo de configuração, guardado em `/etc/systemd/system`, com a extensão `.service`:

```
# /etc/systemd/system/meu-monitor.service

[Unit]
Description=Script de monitoramento personalizado
After=network.target

[Service]
ExecStart=/usr/local/bin/monitor.sh
Restart=on-failure
User=filipe

[Install]
WantedBy=multi-user.target
```

O arquivo se divide em três seções. `[Unit]` guarda metadados e dependências, como a descrição do serviço e a instrução `After=network.target`, garantindo que ele só inicie depois que a rede já estiver disponível. `[Service]` define como o processo de fato roda: o caminho completo do script (o `ExecStart` sempre exige um caminho absoluto), a política de reinício (`Restart=on-failure` reinicia o serviço automaticamente se ele terminar com um código de erro, o mesmo conceito de código de saída já apresentado no arquivo sobre [processos e exit codes](../07-processamento-texto-e-automacao/06-processos-pgrep-pkill-xargs-exit-codes.md)), e sob qual usuário o processo deve rodar. Já `[Install]` define o comportamento quando o serviço é habilitado para iniciar automaticamente no boot.

## Ativando o serviço

Depois de criar o arquivo, é preciso avisar o systemd de que algo mudou, antes de conseguir usar o novo serviço:

```
sudo systemctl daemon-reload
sudo systemctl start meu-monitor
sudo systemctl enable meu-monitor
```

O `daemon-reload` recarrega a configuração interna do systemd, reconhecendo o novo arquivo de unidade. O `start` liga o serviço imediatamente, e o `enable` garante que ele suba automaticamente em todo boot futuro, exatamente como já apresentado para o Nginx no arquivo sobre [servidor web básico](../05-rede-usuarios-seguranca/05-servidor-web-basico.md). A partir daí, o script `monitor.sh` passa a ser gerenciado como qualquer outro serviço do sistema, com seus logs acessíveis via `journalctl -u meu-monitor`, já apresentado no arquivo sobre [logs e journalctl](../05-rede-usuarios-seguranca/04-logs-journalctl.md).

## Quando vale a pena esse esforço extra

Configurar um arquivo de unidade dá mais trabalho do que simplesmente rodar `nohup ./monitor.sh &`, mas esse esforço compensa exatamente nos cenários onde confiabilidade importa de verdade: um script de monitoramento contínuo, um agente do Node Exporter (apresentado no arquivo sobre [Node Exporter e scraping](04-node-exporter-scraping-prometheus.md)), ou qualquer processo que precisa estar sempre no ar, se recuperando sozinho de falhas, sem depender de alguém lembrar de religá-lo manualmente depois de um reinício do servidor.

## Fontes

- [How to Create a Custom systemd Service Unit File on RHEL, oneuptime](https://oneuptime.com/blog/post/2026-03-04-create-custom-systemd-service-unit-file-rhel-9/view)
- [Create systemd Service Unit File with Example, GoLinuxCloud](https://www.golinuxcloud.com/create-systemd-service-example/)
- [Use systemd to Start a Linux Service at Boot, Linode Docs](https://www.linode.com/docs/guides/start-service-at-boot/)
