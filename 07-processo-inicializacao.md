# O processo de inicialização do Linux: BIOS, GRUB e init

## Uma cadeia de entregas

Ligar um computador com Linux e ver a tela de login aparecer parece instantâneo, mas por trás disso existe uma sequência bem definida de etapas, cada uma entregando o controle do sistema para a próxima. Entender essa cadeia ajuda muito na hora de diagnosticar problemas de boot, que costumam ser um dos momentos mais assustadores para quem está começando a mexer com Linux.

De forma resumida, a ordem é: firmware (BIOS ou UEFI), depois o gerenciador de boot (normalmente o GRUB), depois o kernel Linux propriamente dito, e por fim o sistema de inicialização (o init, hoje quase sempre o systemd), que sobe todos os serviços do sistema até chegar na tela de login.

## Etapa 1: BIOS ou UEFI

Assim que o computador é ligado, quem assume primeiro não é o Linux, é o firmware da placa-mãe, armazenado num chip próprio, separado do disco rígido. Esse firmware pode ser o BIOS tradicional, mais antigo, ou o UEFI, seu sucessor moderno, hoje presente na grande maioria dos computadores novos. A função dessa etapa é preparar o hardware básico para funcionar: ela roda um teste inicial de autodiagnóstico (o POST, "power-on self-test"), reconhece os dispositivos de armazenamento disponíveis e decide, com base numa lista de prioridades, de qual dispositivo tentar iniciar o sistema.

A diferença prática entre BIOS e UEFI está em como cada um localiza o próximo programa a executar. O BIOS carrega o gerenciador de boot a partir do MBR, os primeiros 512 bytes do disco. Já o UEFI carrega um arquivo com extensão `.efi` guardado numa partição especial do disco, chamada partição EFI. O UEFI também trouxe recursos como o Secure Boot, que verifica assinaturas digitais para dificultar a execução de software malicioso nessa fase tão inicial do sistema, além de suportar discos maiores e inicializar mais rápido que o BIOS tradicional.

## Etapa 2: o GRUB

Com o hardware básico pronto, o firmware entrega o controle para um gerenciador de boot. Na imensa maioria das distribuições Linux modernas, esse papel é do GRUB (GRand Unified Bootloader), justamente por ser flexível e rico em recursos. É o GRUB que mostra aquele menu de seleção que aparece às vezes ao ligar o computador, permitindo escolher entre diferentes sistemas operacionais instalados, ou entre versões diferentes do kernel Linux, algo útil quando uma atualização recente causa algum problema e é preciso voltar para uma versão anterior que funcionava.

A tarefa do GRUB é carregar o kernel escolhido para dentro da memória RAM, junto com um arquivo chamado initramfs (ou initrd), um sistema de arquivos temporário e enxuto que contém os drivers mínimos necessários para o kernel conseguir, na sequência, enxergar e montar o disco de verdade onde o sistema completo está instalado.

## Etapa 3: o kernel assume

Depois que o GRUB entrega o controle, o kernel Linux começa a rodar de fato. Ele inicializa a memória, reconhece o processador, carrega os drivers essenciais presentes no initramfs, e então monta o sistema de arquivos raiz definitivo, aquele onde o sistema operacional completo realmente está instalado. Uma vez que esse sistema de arquivos está acessível, o trabalho do kernel nessa fase inicial está feito, e ele passa o bastão para o primeiro processo do sistema.

## Etapa 4: init e systemd

O primeiro processo que o kernel executa recebe sempre o identificador PID 1, e é conhecido genericamente como "init". É esse processo o responsável por, a partir daí, subir todos os outros serviços do sistema: rede, som, interface gráfica, agendador de tarefas, e assim por diante, até o sistema estar pronto para uso.

Durante muitos anos, o SysVinit foi o padrão usado por praticamente todas as distribuições, mas hoje a maioria migrou para o systemd, um substituto mais moderno que consegue iniciar serviços em paralelo (em vez de um de cada vez, em sequência), o que acelera bastante o tempo de boot, além de oferecer um jeito padronizado de gerenciar, monitorar e reiniciar serviços através de "unidades" (units) e "alvos" (targets). O comando `systemctl`, usado no dia a dia para ligar, desligar ou verificar o status de um serviço, faz parte justamente do systemd.

## Por que vale entender isso

Saber essa sequência ajuda demais na hora de resolver problemas reais: um erro logo depois de ligar o computador, antes de qualquer logo aparecer, provavelmente é um problema de firmware ou de disco não reconhecido. Um erro relacionado a "não encontrei o sistema operacional" costuma apontar para uma falha do GRUB ou na configuração da partição de boot. E um sistema que liga, mostra o menu do GRUB, começa a carregar, mas trava antes da tela de login, geralmente indica um problema em algum serviço que o systemd está tentando iniciar. Ter esse mapa mental transforma um boot quebrado de "algo misterioso deu errado" para "sei exatamente em qual etapa investigar".

## Fontes

- [Guide to the Boot Process of a Linux System, Baeldung on Linux](https://www.baeldung.com/linux/boot-process)
- [Linux Boot Process: UEFI, GRUB, initramfs & systemd, GoLinuxCloud](https://www.golinuxcloud.com/linux-boot-process-explained-step-detail/)
- [System Boot and systemd, Penguin Gym Linux](https://penguin-gym-linux.com/en/articles/lpic/boot-and-systemd)
- [Boot Process with Systemd in Linux: A Detailed Guide, Dracula Servers](https://draculaservers.com/tutorials/boot-process-with-systemd-in-linux-a-detailed-guide/)
