# Docker e Homebrew: gerenciando pacotes e containers

## Duas ferramentas, dois problemas diferentes

Fechando este bloco sobre ambiente de desenvolvimento, vale apresentar duas ferramentas que aparecem o tempo todo no dia a dia de quem trabalha com shell scripting e automação, mas que resolvem problemas bem diferentes entre si: o Homebrew instala programas, o Docker isola ambientes inteiros.

## Homebrew: um gerenciador de pacotes para quem não tem um nativo tão completo

O arquivo sobre [APT e dpkg](../04-pacotes-scripts-automacao/01-apt-dpkg-gerenciamento-pacotes.md) já mostrou como distribuições Linux baseadas em Debian gerenciam a instalação de programas. O macOS, apesar de ser tecnicamente Unix, como visto no arquivo sobre a [história do Unix](02-historia-influencia-unix.md), não vem com um gerenciador de pacotes de linha de comando tão robusto quanto o APT. O Homebrew nasceu justamente para preencher essa lacuna, e hoje é praticamente padrão entre desenvolvedores que usam Mac (também funciona em Linux, embora seja bem menos comum ali, onde o gerenciador nativo já cobre bem essa necessidade).

A instalação de um programa com Homebrew segue uma lógica bem parecida com a do `apt`, apresentada anteriormente:

```
brew install git
```

Vale um detalhe importante: por padrão, o Homebrew instala ferramentas de linha de comando. Para programas com interface gráfica completa, ele usa uma extensão chamada Homebrew Cask:

```
brew install --cask docker
```

## Docker: isolando ambientes inteiros, não só programas

Enquanto o Homebrew (ou o APT) instala um programa direto no sistema, compartilhando as mesmas bibliotecas e configurações do resto da máquina, o Docker resolve um problema diferente: rodar uma aplicação dentro de um ambiente isolado, chamado container, com suas próprias bibliotecas e dependências, completamente separado do sistema hospedeiro por fora dele.

Isso resolve um problema clássico de desenvolvimento: "na minha máquina funciona", quando um programa se comporta de forma diferente em ambientes distintos por causa de pequenas diferenças de versão de bibliotecas instaladas. Com Docker, a aplicação roda sempre dentro do mesmo ambiente controlado, não importa em qual máquina o container seja executado, seja o notebook do desenvolvedor, seja um servidor de produção.

```
docker run hello-world
```

Esse comando clássico baixa e roda um container mínimo de teste, só para confirmar que o Docker está instalado e funcionando corretamente.

## Uma observação sobre como o Docker roda no macOS e no Windows

Vale um detalhe técnico interessante para fechar: containers Docker dependem, no fundo, de recursos específicos do kernel Linux para funcionar. Isso significa que, tanto no macOS quanto no Windows, o Docker não roda "nativamente" no sentido estrito, ele sobe, por baixo dos panos, uma máquina virtual leve rodando Linux (usando o WSL2 no caso do Windows, já apresentado no [arquivo anterior](07-ambiente-dev-macos-windows.md), ou o framework de virtualização do próprio macOS), e é dentro dessa máquina virtual que os containers de fato rodam. Para quem usa a ferramenta, esse detalhe fica praticamente invisível, mas explica por que o Docker Desktop, a versão com interface gráfica mais usada nesses dois sistemas, tem um custo de recursos (memória, principalmente) um pouco maior do que se poderia esperar de "só rodar um container".

## Fontes

- [Explaining Package Managers, Medium](https://pavolkutaj.medium.com/explaining-package-managers-ad79b7b4403a)
- [How to Install Docker Using Homebrew, Delft Stack](https://www.delftstack.com/howto/docker/brew-docker/)
- [Docker on Mac with Homebrew: A Step-by-Step Tutorial](https://www.souysoeng.com/2024/05/docker-on-mac-with-homebrew-step-by.html)
