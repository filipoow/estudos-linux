# As diferentes distribuições de Linux e suas características

## O que é uma distribuição, afinal

Como já ficou claro no arquivo sobre a [origem do Linux](01-origem-evolucao-linux.md), o Linux propriamente dito é só o kernel, a peça central que fala com o hardware. Para virar um sistema operacional completo e utilizável, o kernel precisa ser combinado com um monte de outras coisas: ferramentas de linha de comando, bibliotecas do sistema, um instalador, um gerenciador de pacotes, e muitas vezes uma interface gráfica inteira. Esse pacote completo, pronto para instalar num computador, é o que se chama de distribuição, ou "distro".

É por isso que existem tantas opções diferentes de "Linux" no mercado, na verdade todas usam o mesmo kernel (ou uma variação próxima dele), mas cada distribuição faz escolhas próprias sobre quais ferramentas incluir, como organizar tudo, e para qual tipo de usuário aquilo é pensado.

## As grandes famílias

Apesar de existirem centenas de distribuições catalogadas, a maioria delas descende de um pequeno número de projetos originais, o que ajuda bastante a entender o cenário:

**Família Debian**: o Debian é um dos projetos de distribuição mais antigos e é conhecido pela estabilidade e pelo compromisso forte com software livre. O Ubuntu, uma das distros mais populares para iniciantes, é construído em cima do Debian, e por sua vez deu origem a outras distros, como o Linux Mint.

**Família Red Hat**: o Red Hat Enterprise Linux (RHEL) é focado em ambientes corporativos, com suporte pago. O Fedora é o laboratório mais experimental dessa mesma família, onde novidades são testadas antes de eventualmente chegarem ao RHEL. O CentOS Stream ocupa um espaço intermediário entre os dois.

**Arch Linux**: segue uma filosofia bem diferente, focada em simplicidade de design (no sentido de menos camadas escondidas, não necessariamente mais fácil para iniciantes) e em deixar o usuário montar o sistema praticamente peça por peça, em vez de entregar tudo pronto.

**Slackware e Gentoo**: são projetos mais antigos ou mais radicais em suas escolhas técnicas, voltados para usuários que quer bastante controle manual sobre o sistema.

## Gerenciadores de pacotes

Uma das diferenças mais práticas entre distribuições, e que se sente todo dia no uso real, é o gerenciador de pacotes, o programa responsável por instalar, atualizar e remover softwares.

- Debian e Ubuntu usam o formato `.deb`, gerenciado por ferramentas como `apt`.
- Fedora e RHEL usam o formato `.rpm`, gerenciado principalmente pelo `dnf`.
- Arch Linux usa o `pacman`, além de contar com o AUR (Arch User Repository), um repositório extra mantido pela própria comunidade com uma quantidade enorme de pacotes que não fazem parte dos repositórios oficiais.
- Gentoo usa o Portage, acessado pelo comando `emerge`, que compila os programas a partir do código-fonte no próprio computador do usuário, em vez de baixar binários prontos.

Vale dizer que pacotes de um formato normalmente não funcionam em distros de outro formato, por isso um programa "para Ubuntu" nem sempre roda direto num Fedora, embora hoje existam formatos universais, como Flatpak e Snap, criados justamente para amenizar esse problema.

## Modelos de lançamento

Outra diferença relevante é como cada distribuição decide atualizar seus pacotes:

- **Lançamento fixo**: a distro define versões numeradas, lançadas em intervalos regulares (o Ubuntu, por exemplo, lança uma nova versão a cada seis meses, com versões de suporte estendido a cada dois anos). Depois de lançada, essa versão recebe principalmente correções de segurança, não pacotes totalmente novos.
- **Rolling release** (lançamento contínuo): não existem versões numeradas separadas, o sistema recebe atualizações constantes e vai sempre incorporando as versões mais recentes dos programas. O Arch Linux é o exemplo mais conhecido desse modelo.

Cada modelo tem uma troca envolvida: lançamentos fixos tendem a ser mais previsíveis e estáveis, rolling release entrega novidades mais rápido, mas exige mais atenção do usuário para lidar com eventuais quebras de compatibilidade.

## Escolhendo uma distro

Não existe "a melhor distribuição", existe a mais adequada para cada objetivo. Quem está começando costuma se dar bem com Ubuntu, Linux Mint ou Fedora, que priorizam facilidade de uso. Quem quer aprender a fundo como o sistema funciona, ou quer controle total sobre cada peça instalada, tende a se interessar por Arch ou Gentoo. Em servidores e ambientes corporativos, Debian, RHEL e Ubuntu Server dominam boa parte do mercado, justamente pela estabilidade e pelo suporte de longo prazo.

## Fontes

- [Arch compared to other distributions, ArchWiki](https://wiki.archlinux.org/title/Arch_compared_to_other_distributions)
- [Linux vs Unix vs Distro: Ubuntu, Debian, RHEL, Fedora and Arch Explained, GoLinuxCloud](https://www.golinuxcloud.com/linux-unix-distro-ubuntu-debian-rhel-fedora-arch/)
- [Comparison of Linux Distributions](https://eylenburg.github.io/linux_comparison.htm)
- [Debian releases, Debian Wiki](https://wiki.debian.org/DebianReleases)
