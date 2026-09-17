# Gerenciando processos longos: nohup, screen e tmux

## O problema: processos que morrem junto com o terminal

Por padrão, um processo iniciado dentro de uma sessão de terminal está atrelado a ela: se a sessão fecha, seja porque a pessoa saiu do terminal, seja porque uma conexão SSH caiu, o processo em execução também é encerrado junto. Isso é um problema real para tarefas demoradas, como uma migração de banco de dados ou o processamento de um arquivo grande, que não deveriam ser interrompidas só porque a conexão de rede oscilou por um segundo.

## `nohup`: a solução mais simples

O `nohup` ("no hang up") impede que um processo receba o sinal de encerramento que normalmente é enviado quando o terminal que o iniciou é fechado.

```
nohup ./processar_dados.sh &
```

O `&` no final, já mencionado de passagem em arquivos anteriores, coloca o comando para rodar em segundo plano, devolvendo o controle do terminal imediatamente. Combinado com `nohup`, o processo continua rodando mesmo depois que a sessão de terminal for encerrada. Por padrão, a saída que normalmente apareceria na tela é redirecionada para um arquivo chamado `nohup.out`, na pasta onde o comando foi executado, preservando esse conteúdo mesmo sem ninguém olhando em tempo real.

A limitação do `nohup` é justamente sua simplicidade: uma vez que o processo está rodando em segundo plano, não existe um jeito fácil de "voltar" para ele depois e interagir diretamente, só é possível conferir a saída já registrada no arquivo de log.

## `screen` e `tmux`: sessões completas que sobrevivem à desconexão

Quando o objetivo não é só deixar um comando rodando, mas manter uma sessão de terminal inteira, à qual seja possível voltar depois, exatamente como estava, entram os multiplexadores de terminal, `screen` e `tmux`. Os dois seguem a mesma lógica geral: criam uma sessão de terminal que roda de forma independente da conexão que a originou, permitindo desconectar (detach) e reconectar (attach) a essa mesma sessão depois, com tudo exatamente como foi deixado, incluindo processos ainda em execução.

```
tmux new -s manutencao
```

Esse comando cria uma nova sessão `tmux` chamada `manutencao`. Dentro dela, qualquer comando roda normalmente, como num terminal comum. Para se desconectar sem encerrar nada, o atalho padrão é `Ctrl+B` seguido de `D`. Para retomar essa mesma sessão depois, de qualquer lugar:

```
tmux attach -t manutencao
```

O `screen`, mais antigo, funciona de forma bastante parecida, com `screen -S nome` para criar e `screen -r nome` para retomar.

A diferença mais relevante entre os dois hoje em dia é que o `tmux` costuma ser considerado mais moderno e mais flexível, especialmente por permitir dividir a mesma sessão em múltiplos painéis e janelas de forma mais intuitiva, enquanto o `screen`, apesar de mais antigo, ainda vem pré-instalado em mais distribuições por padrão.

## Escolhendo a ferramenta certa

Para simplesmente deixar um comando rodando sem precisar interagir com ele depois, `nohup` é suficiente e mais simples. Para qualquer cenário onde a intenção é voltar depois e continuar trabalhando dentro daquela mesma sessão, como acompanhar o progresso de uma tarefa longa, ou manter várias janelas de trabalho organizadas dentro de uma única conexão remota, `tmux` (ou `screen`) é a ferramenta certa.

## Fontes

- [nohup vs. screen vs. tmux: Managing Long-Running Processes on Remote Systems, GitHub Gist](https://gist.github.com/MangaD/632e8f5a6649c9b2e30e2e5d3926447b)
- [Detach Processes With Disown and Nohup, ServerWatch](https://www.serverwatch.com/guides/detach-processes-with-disown-and-nohup/)
- [Keep commands alive after SSH: nohup, tmux, screen & zellij, Mojalab](https://mojalab.com/keep-processes-running-after-ssh-nohup-tmux-screen-zellij/)
