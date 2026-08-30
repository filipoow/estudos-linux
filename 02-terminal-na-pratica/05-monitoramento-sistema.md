# Monitorando o sistema: df, free, vmstat, dmesg e htop

## Entender o que a máquina está fazendo

Depois de saber manipular arquivos e editar textos, o próximo passo natural é conseguir enxergar o que está acontecendo por dentro do sistema: quanto espaço em disco ainda sobra, quanta memória está sendo usada, se o processador está sob estresse, e o que o kernel andou registrando sobre o hardware. Esses comandos de monitoramento são essenciais tanto para diagnosticar problemas quanto para simplesmente entender melhor como um sistema Linux se comporta.

## `df`: espaço em disco

O `df` ("disk free") mostra quanto espaço está ocupado e quanto ainda está livre em cada sistema de arquivos montado na máquina.

```
df -h
```

A opção `-h` ("human readable") transforma os números, que por padrão vêm em blocos de disco pouco intuitivos, em unidades legíveis como KB, MB e GB. É o primeiro comando que qualquer administrador roda quando um sistema começa a se comportar de forma estranha, porque um disco cheio pode travar desde a instalação de programas até o simples ato de salvar um arquivo.

## `free`: memória RAM e swap

O `free` mostra como a memória do sistema está sendo usada, separando entre memória RAM física e a memória swap (o espaço no disco usado como extensão da RAM quando ela se esgota).

```
free -h
```

De novo com `-h` para formato legível. A saída mostra colunas como total, usado, livre e disponível, sendo essa última a mais importante na prática: ela indica quanta memória realmente pode ser usada por novos programas, já contando com memória que está temporariamente ocupada por cache, mas que pode ser liberada se necessário.

## `vmstat`: uma visão geral e contínua

O `vmstat` ("virtual memory statistics") vai além da memória, mostrando num único relatório informações sobre processos, memória, uso de disco (I/O) e uso de processador.

```
vmstat 2
```

Passar um número como argumento faz o `vmstat` repetir a leitura a cada intervalo de segundos indicado, nesse exemplo a cada dois segundos, criando uma espécie de linha do tempo do comportamento do sistema, útil para acompanhar como os números mudam enquanto algum processo pesado está rodando.

## `dmesg`: o que o kernel está registrando

O `dmesg` mostra o conteúdo do buffer de mensagens do kernel, um registro interno onde o próprio núcleo do sistema anota eventos de baixo nível: reconhecimento de hardware conectado, carregamento de drivers, erros de disco, e outros acontecimentos que ocorrem numa camada bem mais profunda do que a maioria dos programas comuns enxerga.

```
dmesg | tail -n 30
```

Esse exemplo, combinando `dmesg` com o `tail` já apresentado no arquivo sobre [visualização e filtragem de conteúdo](02-visualizacao-filtragem-conteudo.md), mostra só as últimas trinta mensagens registradas, que costumam ser as mais relevantes para diagnosticar um problema recente, como um pen drive que não foi reconhecido ou uma falha de disco.

## `htop`: uma central de monitoramento interativa

Diferente dos comandos anteriores, que mostram uma foto ou uma sequência de fotos do sistema, o `htop` é uma ferramenta interativa que fica rodando na tela, atualizando em tempo real. Ele é uma versão mais moderna e visual do comando `top`, tradicionalmente presente em qualquer sistema Unix.

```
htop
```

Na tela do `htop` aparecem barras coloridas mostrando o uso de cada núcleo do processador e da memória, além de uma lista de processos em execução, ordenável por consumo de CPU, memória ou outros critérios. É possível navegar pela lista com as setas do teclado e, dali mesmo, encerrar um processo travado, sem precisar sair do terminal ou abrir um gerenciador de tarefas gráfico. Diferente do `top`, que já vem instalado por padrão em quase toda distribuição, o `htop` geralmente precisa ser instalado à parte, mas costuma valer o esforço pela clareza visual.

## Por que aprender esses comandos importa

Servidores, a esmagadora maioria das máquinas Linux no mundo, normalmente não têm tela nem interface gráfica. Nesse contexto, esses cinco comandos formam praticamente todo o painel de instrumentos disponível para entender a saúde do sistema, o equivalente ao gerenciador de tarefas e ao monitor de recursos de um sistema com interface gráfica, só que acessível remotamente, de qualquer lugar, através de uma simples conexão de terminal.

## Fontes

- [Linux System Monitoring Commands and Tools, GeeksforGeeks](https://www.geeksforgeeks.org/linux-unix/linux-system-monitoring-commands-and-tools/)
- [Linux System Monitoring: top, htop, vmstat, and iostat Explained, The Practical Linux Handbook](https://practicallinuxbook.com/blog/linux-system-monitoring-guide/)
- [24 Best Command Line Performance Monitoring Tools for Linux, Tecmint](https://www.tecmint.com/command-line-tools-to-monitor-linux-performance/)
