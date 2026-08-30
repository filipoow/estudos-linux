# Processos e tarefas: monitorando com ps, top e htop

## O que é um processo

Todo programa em execução no Linux, do maior servidor web ao menor script, vira aquilo que o sistema chama de processo assim que começa a rodar. Cada processo recebe um número de identificação único, o PID (Process ID), e o kernel usa esse número para controlar tudo relacionado àquele programa: quanto tempo de processador ele recebe, quanta memória ele pode usar, e quando ele deve ser encerrado. O primeiro processo que o sistema cria ao ligar, sempre com PID 1, é o próprio init (hoje, na maioria das distribuições, o systemd), como já foi explicado no arquivo sobre o [processo de inicialização](../01-fundamentos/07-processo-inicializacao.md). Todos os outros processos do sistema descendem dele, formando uma árvore de processos pai e processos filho.

## `ps`: uma fotografia do momento

O `ps` ("process status") mostra os processos em execução no exato instante em que o comando é rodado, uma espécie de fotografia parada. Sozinho, ele mostra só os processos ligados à sessão atual do terminal, mas combinado com opções mostra muito mais:

```
ps aux
```

Essa combinação clássica de opções mostra todos os processos do sistema (`a`), incluindo os que não estão ligados a um terminal (`x`), com informações detalhadas sobre o usuário dono de cada processo (`u`), como consumo de memória e de processador.

Por ser uma fotografia e não algo que fica atualizando sozinho, o `ps` é a ferramenta certa quando o objetivo é usar o resultado dentro de um script, ou filtrar a saída com o `grep`, já apresentado no arquivo sobre [visualização e filtragem de conteúdo](../02-terminal-na-pratica/02-visualizacao-filtragem-conteudo.md):

```
ps aux | grep firefox
```

## `top`: acompanhamento em tempo real

Diferente do `ps`, o `top` fica rodando na tela, atualizando a lista de processos periodicamente, por padrão a cada alguns segundos. Ele mostra, no topo da tela, um resumo geral do sistema (uso de processador, memória, quantidade de processos), e abaixo uma lista ordenável dos processos que mais consomem recursos naquele momento.

```
top
```

O `top` é útil justamente quando o problema não está claro ainda, e é preciso observar o comportamento do sistema ao vivo, por exemplo para flagrar um processo que aparece e desaparece rápido, ou que só consome muito processador em picos específicos.

## `htop`: a versão mais amigável

O `htop`, já apresentado com mais detalhes no arquivo sobre [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md), cumpre basicamente o mesmo papel do `top`, mas com uma interface bem mais fácil de ler, com cores, barras de uso por núcleo do processador, e navegação pelo teclado para explorar e até encerrar processos diretamente da tela, sem precisar decorar comandos extras.

## Comparando os três

Cada um desses comandos tem seu momento certo. O `ps` é ideal para consultas rápidas, específicas, ou para uso dentro de scripts de automação, já que sua saída é simples de processar por outros programas. O `top` é a escolha natural para acompanhar o sistema ao vivo em qualquer máquina Linux, já que vem instalado por padrão em praticamente todas as distribuições. E o `htop`, quando disponível, costuma ser preferido por quem passa bastante tempo explorando manualmente o estado do sistema, pela clareza visual e pela facilidade de interação.

## Fontes

- [How to Use top, htop, and ps to Monitor System Processes on RHEL, oneuptime](https://oneuptime.com/blog/post/2026-03-04-top-htop-ps-monitor-processes-rhel-9/view)
- [A Practical Guide to Linux Process List Commands: Ps, Top, Htop, ITU Online](https://www.ituonline.com/blogs/a-practical-guide-to-linux-process-list-commands-ps-top-htop/)
- [ps(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/ps.1.html)
