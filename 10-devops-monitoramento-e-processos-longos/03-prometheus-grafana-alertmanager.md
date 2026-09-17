# Monitoramento contínuo com Prometheus, Grafana e Alertmanager

## Além dos comandos pontuais

Os arquivos sobre [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md) e [processos: ps, top e htop](../03-arquivos-permissoes-processos/03-processos-ps-top-htop.md) mostraram como consultar o estado de uma máquina no momento exato em que o comando é digitado. Isso funciona bem para diagnóstico pontual, mas não serve para acompanhar um sistema continuamente ao longo do tempo, nem para avisar alguém automaticamente quando algo sai do esperado, mesmo de madrugada, sem ninguém olhando o terminal naquele instante. É esse problema que o conjunto Prometheus, Grafana e Alertmanager resolve.

## Três ferramentas, três responsabilidades separadas

**Prometheus** é o coração do conjunto: um banco de dados de séries temporais, especializado em coletar e armazenar métricas numéricas ao longo do tempo, como uso de CPU, memória disponível, ou número de requisições por segundo. Diferente dos comandos pontuais já vistos, o Prometheus coleta esses números em intervalos regulares, automaticamente, guardando um histórico que pode ser consultado depois.

**Grafana** não coleta nada por conta própria, ele se conecta ao Prometheus (e a outras fontes de dados) para consultar essas métricas armazenadas e transformá-las em painéis visuais, gráficos e dashboards, muito mais fáceis de interpretar de relance do que uma tabela de números crus.

**Alertmanager** cuida da última etapa: quando o Prometheus detecta que uma métrica ultrapassou um limite configurado (memória acima de 90%, por exemplo), ele envia essa informação para o Alertmanager, que decide como notificar alguém, agrupando alertas parecidos, evitando notificações repetidas, e encaminhando o aviso para o destino certo, como e-mail, Slack ou outro canal configurado.

## Como as três peças se encaixam

Essa divisão de responsabilidades é deliberada: o Prometheus não precisa saber como avisar alguém, só precisa saber quando algo merece atenção. O Alertmanager não precisa saber como coletar dados, só precisa saber o que fazer com um alerta que já chegou até ele. E o Grafana não precisa se preocupar em guardar nada, só em consultar e exibir. Esse desenho, onde cada peça faz uma coisa bem definida e se comunica com as outras através de uma interface clara, lembra bastante a filosofia Unix, já apresentada no arquivo correspondente, aplicada agora numa escala de infraestrutura inteira, em vez de comandos individuais de terminal.

## O fluxo completo de um alerta

Juntando as três peças: o Prometheus coleta métricas continuamente de servidores e aplicações. Regras de alerta, configuradas dentro do próprio Prometheus, definem condições que merecem atenção. Quando uma dessas condições é satisfeita, o Prometheus dispara um alerta para o Alertmanager. O Alertmanager decide como (e se) notificar alguém, considerando agrupamento e repetição. E, em paralelo a tudo isso, o Grafana permanece disponível para qualquer pessoa consultar visualmente o histórico dessas mesmas métricas, seja para investigar um incidente já resolvido, seja só para acompanhar a saúde geral do sistema.

## Fontes

- [Monitoring & Alerting: Prometheus, Grafana & Alertmanager, Medium](https://medium.com/@junnexclusive/monitoring-alerting-prometheus-grafana-alertmanager-c38a903ce405)
- [What is Prometheus Grafana stack?, Whizlabs](https://www.whizlabs.com/blog/prometheus-grafana-stack/)
- [Kubernetes Monitoring with Prometheus: AlertManager, Grafana, PushGateway, Sysdig](https://www.sysdig.com/blog/kubernetes-monitoring-with-prometheus-alertmanager-grafana-pushgateway-part-2)
