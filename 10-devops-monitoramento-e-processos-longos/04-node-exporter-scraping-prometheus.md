# Node Exporter e as regras de scraping no Prometheus

## De onde o Prometheus tira os números

O arquivo anterior, sobre [Prometheus, Grafana e Alertmanager](03-prometheus-grafana-alertmanager.md), explicou que o Prometheus coleta métricas continuamente, mas deixou uma pergunta em aberto: coleta de onde, exatamente? Por padrão, o Prometheus não sabe nada sobre CPU, memória ou disco de uma máquina Linux, ele só sabe consultar, em intervalos regulares, um endereço HTTP que devolve números num formato específico. É aí que entra o Node Exporter.

## Node Exporter: traduzindo o sistema operacional em métricas

O Node Exporter é um programa pequeno, que roda como um serviço em segundo plano em cada servidor que se quer monitorar (usando os mesmos conceitos de serviço persistente apresentados no arquivo sobre [systemd](07-systemd-servicos-personalizados.md)), e cuja única função é expor, numa porta HTTP (por padrão a 9100), um conjunto enorme de métricas sobre aquela máquina: uso de CPU, memória disponível, espaço em disco, tráfego de rede, e muitas outras, essencialmente uma versão automatizada e contínua do que comandos como `df` e `free`, já apresentados no arquivo sobre [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md), mostram sob demanda.

Depois de instalado e rodando, é possível conferir que o Node Exporter está funcionando simplesmente acessando seu endereço:

```
curl http://localhost:9100/metrics
```

Esse comando devolve uma lista longa de métricas em texto simples, cada uma com um nome, um valor numérico, e opcionalmente algumas etiquetas descrevendo o contexto daquela métrica.

## Configurando o Prometheus para coletar (scrape) essas métricas

O ato de o Prometheus consultar periodicamente um endereço como esse é chamado de scraping, e é configurado no arquivo `prometheus.yml`, dentro da seção `scrape_configs`:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
```

O `scrape_interval` define de quanto em quanto tempo o Prometheus vai buscar novos dados, nesse exemplo a cada quinze segundos. Dentro de `scrape_configs`, cada `job_name` agrupa um conjunto de alvos (`targets`) que devem ser consultados da mesma forma, nesse caso um único servidor rodando o Node Exporter na porta padrão. É perfeitamente possível (e comum) adicionar vários endereços na mesma lista de `targets`, permitindo que um único Prometheus monitore dezenas ou centenas de servidores diferentes, todos expondo métricas através de seus próprios Node Exporters.

## Validando a configuração antes de aplicar

Assim como já foi recomendado para outras ferramentas neste repositório, como o Nginx no arquivo sobre [proxy reverso](../07-processamento-texto-e-automacao/07-nginx-proxy-reverso.md), o Prometheus também oferece um jeito de validar a configuração antes de efetivamente recarregá-la:

```
promtool check config /etc/prometheus/prometheus.yml
```

Esse comando evita que um erro de sintaxe no `prometheus.yml` derrube a coleta de métricas de toda a infraestrutura de uma vez, um cuidado especialmente importante justamente porque, se o monitoramento parar de funcionar silenciosamente, pode levar um bom tempo até alguém perceber que os dashboards do Grafana pararam de atualizar.

## Fontes

- [Monitoring Linux host metrics with the Node Exporter, Prometheus.io](https://prometheus.io/docs/guides/node-exporter/)
- [How to Configure Node Exporter for Prometheus, oneuptime](https://oneuptime.com/blog/post/2026-01-25-prometheus-node-exporter/view)
- [Getting started, Prometheus.io](https://prometheus.io/docs/prometheus/latest/getting_started/)
