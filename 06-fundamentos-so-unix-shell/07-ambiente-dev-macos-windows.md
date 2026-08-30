# Configurando um ambiente de desenvolvimento no macOS e no Windows

## Shell scripting fora do Linux

Tudo o que foi apresentado neste repositório até aqui, comandos, shell, scripts, pressupõe um ambiente Linux (ou um terminal Unix, no caso do macOS). Mas boa parte de quem estuda essas ferramentas usa, no dia a dia, um notebook com Windows ou macOS. A boa notícia é que os dois sistemas oferecem caminhos bem estabelecidos para se ter um ambiente de linha de comando compatível com tudo que já foi visto neste repositório.

## macOS: já é Unix por baixo

Como mencionado no arquivo sobre a [história do Unix](02-historia-influencia-unix.md), o macOS é, tecnicamente, um sistema certificado como Unix, construído sobre uma base derivada do BSD. Isso significa que o terminal do macOS já roda um shell de verdade nativamente, sem precisar de nenhuma camada extra de compatibilidade, o próprio aplicativo Terminal (ou alternativas populares, como o iTerm2) abre diretamente numa sessão de shell, historicamente `bash` e, desde 2019, `zsh` por padrão, já apresentado no arquivo sobre [tipos de shell](05-tipos-de-shell.md).

Para instalar ferramentas de desenvolvimento adicionais, como compiladores e utilitários de linha de comando usados por outros programas, o macOS oferece as Xcode Command Line Tools, instaláveis com um único comando:

```
xcode-select --install
```

A partir daí, o ambiente já está pronto para receber gerenciadores de pacotes como o Homebrew, apresentado no [próximo arquivo](08-docker-homebrew.md).

## Windows: o WSL como ponte para o Linux

O Windows, diferente do macOS, não é baseado em Unix, então rodar comandos e scripts pensados para Linux exigiria, tradicionalmente, máquinas virtuais completas ou soluções de compatibilidade limitadas. Isso mudou com o WSL (Windows Subsystem for Linux), atualmente na sua segunda versão, o WSL2.

O WSL2 roda um kernel Linux completo e de verdade, dentro de uma máquina virtual leve, integrada de forma bem próxima ao restante do Windows. Na prática, isso significa ter acesso a uma distribuição Linux real (o Ubuntu é a opção padrão), com o mesmo `bash`, os mesmos gerenciadores de pacotes como `apt`, já apresentado no arquivo sobre [APT e dpkg](../04-pacotes-scripts-automacao/01-apt-dpkg-gerenciamento-pacotes.md), e a mesma estrutura de comandos vista ao longo deste repositório, rodando lado a lado com os programas do Windows.

Em versões recentes do Windows 10 e 11, a instalação se resume a um único comando, rodado no Prompt de Comando ou PowerShell como administrador:

```
wsl --install
```

Esse comando ativa os recursos necessários do Windows, baixa o kernel Linux mais recente, e instala a distribuição padrão, tudo de forma automática. Depois de instalado, os arquivos do sistema Linux ficam acessíveis pelo Windows através de um caminho de rede especial (`\\wsl$`), permitindo transitar entre os dois mundos sem grande atrito.

## Por que isso importa

Ter um ambiente de linha de comando compatível com Linux, mesmo usando Windows ou macOS no dia a dia, é o que permite praticar e aplicar tudo o que foi apresentado neste repositório sem precisar de uma máquina Linux dedicada. É também, cada vez mais, um requisito comum no mercado de trabalho em tecnologia, já que boa parte da infraestrutura de servidores e ferramentas de desenvolvimento, incluindo o Docker, apresentado no próximo arquivo, pressupõe familiaridade com esse tipo de ambiente.

## Fontes

- [Set up a WSL development environment, Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/setup/environment)
- [WSL2 Tutorial: The Complete Guide for Windows 10 & 11, SitePoint](https://www.sitepoint.com/wsl2/)
- [How to easily set up a Linux development environment on Windows using WSL2, Medium](https://medium.com/@hannybal/how-to-easily-set-up-a-linux-development-environment-on-windows-using-wsl2-729a48c33eee)
