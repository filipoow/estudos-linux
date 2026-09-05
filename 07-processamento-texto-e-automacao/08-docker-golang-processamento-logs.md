# Docker e Golang na prática: processando logs de uma aplicação em container

## Fechando o bloco com um exemplo real

Este arquivo funciona como uma síntese: pegar conceitos já apresentados neste repositório, o Docker, introduzido no arquivo sobre [Docker e Homebrew](../06-fundamentos-so-unix-shell/08-docker-homebrew.md), e as ferramentas de processamento de texto vistas ao longo deste bloco, e aplicá-los a um cenário comum na rotina de quem trabalha com automação: uma aplicação escrita em Go, rodando dentro de um container, gerando logs que precisam ser analisados.

## Por que Go costuma aparecer ao lado de Docker

Go (ou Golang) é uma linguagem popular para esse tipo de cenário porque compila para um único binário, sem depender de um interpretador ou de bibliotecas externas instaladas separadamente no sistema. Isso combina bem com a filosofia de container do Docker: a imagem final pode ser extremamente enxuta, contendo só o binário compilado e o mínimo necessário para rodá-lo, sem carregar um ambiente de desenvolvimento inteiro dentro da imagem de produção.

Um Dockerfile típico para uma aplicação Go usa duas etapas: uma para compilar o programa, e outra, bem mais enxuta, só para executá-lo.

```dockerfile
FROM golang:1.22 AS build
WORKDIR /app
COPY . .
RUN go build -o servidor .

FROM debian:bookworm-slim
COPY --from=build /app/servidor /usr/local/bin/servidor
CMD ["servidor"]
```

## Por que a aplicação deve escrever logs na saída padrão

Uma boa prática amplamente recomendada para aplicações em container é escrever mensagens de log diretamente na saída padrão (stdout), o mesmo conceito de canais padrão apresentado no arquivo sobre [redirecionamento e pipes](../04-pacotes-scripts-automacao/02-redirecionamento-pipes.md), em vez de escrever num arquivo de log dentro do próprio container. Isso permite que o Docker capture esses registros automaticamente, tornando-os acessíveis através do comando `docker logs`, sem exigir configuração extra de onde o arquivo de log está guardado dentro de cada container.

```
docker logs meu-servidor
```

## Filtrando e contando com grep e wc -l

Uma vez que os logs estão acessíveis, as mesmas ferramentas já apresentadas neste bloco de aulas se aplicam normalmente, mesmo o log vindo de dentro de um container:

```
docker logs meu-servidor | grep "erro"
```

Esse comando reaproveita o `grep`, apresentado em detalhe no arquivo sobre [grep avançado e find](04-grep-avancado-find.md), para filtrar só as linhas de log que mencionam "erro". Para simplesmente contar quantas vezes algo aconteceu, sem precisar ler cada linha, combina-se com o `wc -l` ("word count", com a opção `-l` contando linhas em vez de palavras):

```
docker logs meu-servidor | grep "erro" | wc -l
```

Esse comando devolve um único número: a quantidade de linhas de log que continham a palavra "erro". É um padrão extremamente comum em monitoramento e diagnóstico rápido, e pode ser facilmente combinado com filtros de tempo, como `docker logs --since "1h" meu-servidor`, para restringir a contagem à última hora, por exemplo.

## O fio que conecta todo este bloco de aulas

Vale reparar como esse exemplo final não introduz nenhum conceito realmente novo, ele só combina, numa situação prática e realista, praticamente tudo que foi apresentado neste bloco: expressões regulares implícitas no `grep`, pipelines conectando comandos simples, e o ambiente de container apresentado lá no início. Essa é, no fundo, a habilidade central que todo esse conjunto de aulas pretende construir: não decorar comandos isolados, mas saber combiná-los diante de um problema real.

## Fontes

- [Go language-specific guide, Docker Docs](https://docs.docker.com/guides/golang/)
- [Dockerizing Go Applications: A Step-by-Step Guide, Better Stack](https://betterstack.com/community/guides/scaling-go/dockerize-golang/)
- [How to Grep Docker Logs: Commands & Examples, SigNoz](https://signoz.io/guides/docker-logs-grep/)
