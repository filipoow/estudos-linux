# A estrutura de diretórios padrão do Linux: o FHS

## Um mapa comum entre distribuições diferentes

Já foi dito em outros arquivos deste repositório que o Linux organiza tudo numa única árvore de pastas, a partir da raiz `/`, sem letras de unidade como no Windows. O que ainda não foi explorado é que essa organização não é aleatória nem específica de cada distribuição: ela segue um padrão documentado chamado Filesystem Hierarchy Standard (FHS), criado em 1993 justamente para que qualquer administrador ou programa soubesse, de antemão, onde encontrar cada tipo de arquivo, não importa se o sistema é um Ubuntu, um Fedora ou um Debian.

O FHS separa os arquivos em categorias segundo dois critérios principais: se são compartilháveis (podem ser usados por outras máquinas na rede) ou não, e se são estáticos (só mudam quando o administrador instala ou atualiza algo) ou variáveis (mudam sozinhos, durante o uso normal do sistema). Essa lógica de classificação é o motivo pelo qual as pastas principais existem separadas do jeito que estão.

## As pastas mais importantes

**`/etc`**: guarda os arquivos de configuração do sistema e da maioria dos programas instalados. São arquivos estáticos, no sentido de que não mudam sozinhos, e não são compartilháveis entre máquinas diferentes, já que a configuração de um servidor específico normalmente não faz sentido em outro. É a pasta que um administrador mais visita ao ajustar como o sistema deve se comportar.

**`/var`**: guarda dados que mudam durante o funcionamento normal do sistema, como registros de log, filas de impressão, e caches temporários de programas. Essa pasta foi criada justamente para separar o que muda com frequência do que fica parado, permitindo, por exemplo, que a pasta `/usr` seja montada como somente leitura em alguns cenários, sem interferir no funcionamento do sistema.

**`/usr`**: apesar do nome sugerir "user", essa pasta não guarda arquivos pessoais dos usuários, e sim a maior parte dos programas, bibliotecas e documentações instaladas no sistema, o grosso do software que não faz parte do núcleo mínimo necessário para o sistema ligar. É comum essa pasta ser compartilhada entre várias máquinas de uma mesma rede.

**`/bin`**: guarda os comandos essenciais, usados tanto por administradores quanto por usuários comuns, como `cat`, `ls`, `cp` e outros comandos básicos apresentados nos arquivos anteriores deste repositório. Historicamente existia uma separação entre `/bin` (comandos essenciais para o sistema funcionar mesmo em modo de recuperação) e `/usr/bin` (demais comandos), embora em distribuições modernas essas duas pastas costumem estar unificadas, com `/bin` sendo apenas um atalho para dentro de `/usr/bin`.

**`/home`**: guarda os arquivos pessoais de cada usuário do sistema, cada um numa subpasta com seu próprio nome de usuário, por exemplo `/home/filipe`. É o equivalente direto da pasta "Usuários" do Windows ou da pasta pessoal do macOS, o espaço onde cada pessoa organiza seus próprios documentos, downloads e configurações individuais.

## Por que vale a pena internalizar isso

Saber de cabeça essas cinco pastas evita muita confusão na hora de resolver problemas reais. Se um programa não está funcionando por causa de uma configuração errada, o primeiro lugar a olhar é `/etc`. Se um disco está enchendo sem motivo aparente, `/var` costuma ser a primeira suspeita, por causa de logs que crescem sem controle. E entender que `/home` é isolado do resto do sistema explica, por exemplo, por que é possível reinstalar um Linux do zero mantendo os arquivos pessoais intactos, bastando manter essa pasta numa partição separada durante a instalação.

## Fontes

- [Filesystem Hierarchy Standard 3.0, Linux Foundation](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
- [The Filesystem Hierarchy Standard, Microsoft WhatTheHack](https://microsoft.github.io/WhatTheHack/020-LinuxFundamentals/Student/resources/fhs.html)
- [3.2. Overview of File System Hierarchy Standard (FHS), Red Hat Documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/4/html/reference_guide/s1-filesystem-fhs)
