# Gerenciamento e permissionamento de arquivos: chmod e chown

## Todo arquivo tem um dono e uma regra de acesso

No Linux, absolutamente todo arquivo e toda pasta carrega duas informações fundamentais: quem é o dono (e a qual grupo esse dono pertence) e o que cada tipo de usuário pode fazer com aquele arquivo. Esse sistema de permissões é um dos pilares de segurança do sistema, e é também um dos pontos que mais confunde quem está começando, por misturar letras, números e siglas que parecem arbitrárias à primeira vista.

Rodando `ls -l` numa pasta, aparece algo parecido com isto no início de cada linha:

```
-rwxr-xr--
```

Esse texto de dez caracteres é a chave para entender tudo neste arquivo.

## Lendo a permissão

O primeiro caractere indica o tipo do item (`-` para arquivo comum, `d` para diretório, entre outros). Os nove caracteres seguintes se dividem em três grupos de três, representando, nessa ordem, o dono do arquivo, o grupo dono do arquivo, e todos os outros usuários do sistema. Cada grupo de três segue sempre a mesma ordem: leitura (`r`), escrita (`w`) e execução (`x`). Um traço no lugar de uma letra significa que aquela permissão específica está negada.

No exemplo `rwxr-xr--`, o dono pode ler, escrever e executar o arquivo, o grupo pode ler e executar, mas não escrever, e todos os outros usuários só podem ler.

## `chmod`: mudando as permissões

O `chmod` ("change mode") altera essas permissões, e aceita duas formas diferentes de escrever a mudança.

**Notação simbólica**: usa letras para indicar quem é afetado (`u` para dono, `g` para grupo, `o` para outros, `a` para todos) e um operador (`+` para adicionar, `-` para remover, `=` para definir exatamente).

```
chmod u+x script.sh
```

Esse comando adiciona permissão de execução para o dono do arquivo, sem alterar as outras permissões já existentes.

**Notação numérica (octal)**: cada permissão vale um número, leitura vale 4, escrita vale 2 e execução vale 1, e a soma desses valores forma um único dígito por grupo (dono, grupo, outros). Por exemplo, `rwx` soma 4+2+1=7, e `r-x` soma 4+1=5.

```
chmod 755 script.sh
```

Esse comando define de uma vez as permissões completas do arquivo: `7` (rwx) para o dono, `5` (r-x) para o grupo, e `5` (r-x) para os outros, um padrão bem comum para scripts e programas executáveis. Já `644` (rw-r--r--) é o padrão típico para arquivos de texto comuns, que não precisam ser executados por ninguém.

A diferença prática entre as duas notações é que a numérica define o estado final completo de uma vez, enquanto a simbólica altera só o que for pedido, deixando o resto como estava.

## `chown`: mudando o dono

Enquanto o `chmod` decide o que pode ser feito com um arquivo, o `chown` ("change owner") decide de quem é esse arquivo.

```
chown maria arquivo.txt
```

Esse comando transfere o arquivo para o usuário `maria`. Também é possível trocar o dono e o grupo ao mesmo tempo, separando os dois com dois pontos:

```
chown maria:equipe arquivo.txt
```

Diferente do `chmod`, que qualquer dono de arquivo pode usar dentro do que já é permitido a ele, o `chown` normalmente exige privilégios de administrador (`sudo`), já que trocar o dono de um arquivo é uma operação sensível o suficiente para não ficar disponível para qualquer usuário comum.

## Por que isso importa tanto

Esse sistema de permissões é o que impede, por exemplo, que um usuário comum apague ou modifique arquivos de configuração do sistema inteiro, ou que um script malicioso baixado da internet rode automaticamente sem antes receber permissão explícita de execução. Entender `chmod` e `chown` de verdade, e não só decorar comandos prontos, é o que separa quem só usa Linux de quem consegue de fato administrar um sistema com segurança.

## Fontes

- [Setting Permissions with chown and chmod, Baeldung on Linux](https://www.baeldung.com/linux/chown-chmod-permissions)
- [Chmod Numeric Permissions Notation Linux / Unix, nixCraft](https://www.cyberciti.biz/faq/unix-linux-bsd-chmod-numeric-permissions-notation-command/)
- [CHMOD Command: Change File Permissions in Linux, ss64.com](https://ss64.com/bash/chmod.html)
