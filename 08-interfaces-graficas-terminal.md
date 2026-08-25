# As interfaces gráficas comuns no Linux e o uso do terminal

## Duas camadas que se confundem, mas são diferentes

Quando alguém fala em "interface gráfica do Linux", geralmente está se referindo a coisas que, tecnicamente, são camadas separadas: o servidor gráfico, responsável por desenhar pixels na tela e receber entrada do teclado e mouse, e o ambiente de desktop, que é o conjunto visual de janelas, ícones, menus e barras de tarefas que o usuário efetivamente vê e usa no dia a dia. Um ambiente de desktop roda em cima de um servidor gráfico, ele não substitui essa camada, depende dela.

## Servidor gráfico: X11 e Wayland

Por décadas, o padrão do mundo Linux (e Unix em geral) foi o sistema X Window, mais conhecido como X11, na implementação chamada Xorg. Ele é maduro, estável e compatível com uma quantidade enorme de software antigo, mas carrega também limitações de arquitetura, já com bastante idade, que afetam desempenho e segurança em usos modernos, como telas de alta resolução ou múltiplos monitores com taxas de atualização diferentes.

O sucessor pensado para resolver esses problemas é o Wayland, iniciado em 2008. A diferença de arquitetura mais importante é que o Wayland junta, num único programa chamado compositor, o papel que antes era dividido entre o servidor gráfico e o gerenciador de janelas, permitindo uma comunicação mais direta entre o sistema e os aplicativos, com menos camadas intermediárias. Hoje o Wayland já é a opção padrão em distribuições grandes como Ubuntu, Fedora e Debian (quando usando o ambiente GNOME), e o ambiente KDE Plasma também vem migrando fortemente para ele. O ambiente XFCE ainda depende do X11 como padrão, embora já tenha suporte inicial ao Wayland em versões recentes.

## Ambientes de desktop

É nessa camada que o usuário realmente reconhece "a cara" de uma distribuição Linux, e é também onde existe a maior variedade de escolha dentro do ecossistema:

- **GNOME**: interface moderna e minimalista, é o ambiente padrão do Ubuntu e do Fedora, entre outros.
- **KDE Plasma**: bastante personalizável, com um visual que costuma lembrar mais o Windows tradicional, o que ajuda quem está migrando de outro sistema.
- **XFCE**: leve e econômico em recursos de hardware, é uma escolha comum para computadores mais antigos ou mais fracos.
- **Cinnamon**: usado por padrão no Linux Mint, também com uma proposta visual próxima da experiência tradicional de desktop.
- **MATE**: uma continuação da versão mais antiga do GNOME, mantendo uma interface mais tradicional.

Vale notar que, na maioria das distribuições, é perfeitamente possível instalar mais de um ambiente de desktop e escolher entre eles na tela de login, já que eles não são exclusivos entre si dentro do mesmo sistema.

## O papel do terminal nessa camada gráfica

Mesmo dentro de um ambiente gráfico completo, o terminal continua sendo uma ferramenta central, e não algo que só aparece quando algo dá errado. O terminal roda como um programa comum, um emulador de terminal, dentro da interface gráfica, geralmente aberto por um atalho de teclado ou pelo menu de aplicativos. A diferença é que, de dentro dele, o usuário tem acesso a controles muito mais precisos do sistema do que os que normalmente são expostos em botões e menus.

Isso explica por que usuários mais experientes recorrem ao terminal com tanta frequência: instalar um pacote específico com uma única linha de comando costuma ser mais rápido e mais previsível do que navegar por várias telas gráficas, além de permitir automatizar tarefas repetitivas através de scripts, algo que uma interface só de cliques dificilmente oferece com a mesma flexibilidade. Ao mesmo tempo, a interface gráfica continua sendo essencial para tornar o Linux acessível a quem não quer, ou não precisa, memorizar comandos para as tarefas do dia a dia, como navegar na internet, editar documentos ou assistir a vídeos.

## Um sistema com opções, não uma opção única

Diferente de sistemas como o Windows ou o macOS, onde a interface gráfica é uma peça fixa do sistema, no Linux ela é uma escolha, o que reflete bem a filosofia geral do projeto: dar ao usuário liberdade para montar o sistema do jeito que fizer mais sentido para o próprio uso, seja um desktop completo e bonito, seja um ambiente mínimo rodando quase inteiramente pelo terminal.

## Fontes

- [Xorg, X11, Wayland? Linux Display Servers And Protocols Explained, Linuxiac](https://linuxiac.com/xorg-x11-wayland-linux-display-servers-and-protocols-explained/)
- [Wayland, Debian Wiki](https://wiki.debian.org/Wayland)
- [Desktop Environments & Window Managers, Linux Switch](https://linuxswitch.us/desktop-environments/)
- [Linux Desktop Environment Guide: Choosing the Best One](https://www.pchardwarepro.com/en/Linux-desktop-environments-complete-guide-to-get-it-right/)
