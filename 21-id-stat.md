# Identidade e detalhes de arquivos: os comandos id e stat

## `id`: quem você é para o sistema

O comando `id` mostra as credenciais da conta que está sendo usada naquele momento no terminal: o identificador numérico do usuário (UID), o identificador numérico do grupo principal (GID), e a lista de todos os outros grupos aos quais essa conta também pertence.

```
id
```

Um resultado típico se parece com isto:

```
uid=1000(filipe) gid=1000(filipe) grupos=1000(filipe),27(sudo),1001(docker)
```

Esses números por trás dos nomes não são só um detalhe técnico, eles são, na prática, a verdadeira identidade da conta para o kernel. Nomes de usuário existem para facilitar a vida de quem está lendo a tela, mas internamente o sistema usa esses números para decidir, em cada arquivo, se aquela conta tem permissão de acesso, comparando o UID e os GIDs do processo com o dono e o grupo registrados no arquivo, exatamente o sistema de permissões apresentado no arquivo sobre [chmod e chown](16-permissoes-chmod-chown.md). Vale notar que a conta root sempre tem UID igual a 0, não importa a distribuição.

O `id` também aceita indicar outro usuário como argumento, mostrando as credenciais dele em vez das do usuário atual:

```
id maria
```

## `stat`: os detalhes completos de um arquivo

Enquanto o `ls -l` mostra um resumo das informações de um arquivo, o `stat` mostra um relatório bem mais completo, incluindo dados que não aparecem no `ls` comum.

```
stat arquivo.txt
```

A saída típica traz, entre outras informações:

- **Inode**: o número do inode que representa esse arquivo no sistema de arquivos, o mesmo conceito apresentado no arquivo sobre [hardlinks e softlinks](20-links-hardlink-softlink.md).
- **Links**: quantos nomes diferentes (hardlinks) apontam para esse mesmo inode.
- **Size**: o tamanho do arquivo, geralmente em bytes.
- **Access, Modify e Change**: três datas diferentes e fáceis de confundir entre si. Access é a última vez que o conteúdo foi lido. Modify é a última vez que o conteúdo foi alterado. Change é a última vez que os metadados do arquivo mudaram, como permissões ou dono, mesmo que o conteúdo em si não tenha sido tocado.
- **Uid e Gid**: o dono e o grupo dono do arquivo, os mesmos conceitos que aparecem na saída do comando `id`.

## Por que esses dois comandos se complementam

O `id` responde "quem sou eu para o sistema", e o `stat` responde "o que exatamente esse arquivo diz sobre si mesmo". Juntos, eles cobrem os dois lados de toda decisão de permissão que o Linux toma o tempo todo: de um lado, a identidade de quem está pedindo acesso, e do outro, as regras específicas registradas naquele arquivo. Entender os dois comandos ajuda bastante a diagnosticar aquele clássico erro de "permissão negada", permitindo comparar diretamente quem você é (via `id`) com o que o arquivo exige (via `stat` ou `ls -l`).

## Fontes

- [id Command in Linux: Display User and Group Information, Linuxize](https://linuxize.com/post/id-command-in-linux/)
- [Understanding the output of the stat command, Linux Audit](https://linux-audit.com/filesystems/understanding-the-output-of-the-stat-command/)
- [Linux stat Command with Examples, phoenixNAP](https://phoenixnap.com/kb/linux-stat)
