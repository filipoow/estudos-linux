# Gerenciamento de usuários e grupos

## Onde o sistema guarda quem é quem

O arquivo sobre [su e sudo](../03-arquivos-permissoes-processos/04-usuarios-su-sudo.md) já apresentou a diferença entre usuário comum e superusuário. Aqui o foco é anterior a isso: como esses usuários e os grupos aos quais pertencem são de fato criados e administrados. Todo usuário e grupo do sistema fica registrado em três arquivos de texto simples, guardados em `/etc`, seguindo a lógica de organização já explicada no arquivo sobre o [FHS](../03-arquivos-permissoes-processos/01-estrutura-diretorios-fhs.md):

- **`/etc/passwd`**: uma linha por usuário, com sete campos separados por dois-pontos, incluindo nome de usuário, UID, GID do grupo principal, pasta pessoal e o shell padrão daquele usuário. Esse arquivo pode ser lido por qualquer pessoa no sistema.
- **`/etc/shadow`**: guarda as senhas criptografadas e as regras de validade de cada senha, e só pode ser lido pelo root. Essa separação existe justamente por segurança, para que a senha criptografada não fique exposta num arquivo de leitura livre como o `/etc/passwd`.
- **`/etc/group`**: lista cada grupo do sistema, seu GID, e quais usuários fazem parte dele.

Apesar de serem arquivos de texto comuns, a recomendação forte é nunca editá-los diretamente. Sempre usar os comandos apropriados, que cuidam de manter tudo consistente entre os três arquivos ao mesmo tempo.

## Criando e ajustando usuários

O comando `useradd` cria um novo usuário:

```
sudo useradd -m -s /bin/bash maria
```

A opção `-m` cria automaticamente a pasta pessoal do novo usuário dentro de `/home`, e `-s` define qual shell ele vai usar por padrão. Sem `-m`, o usuário é criado sem pasta pessoal, o que raramente é o que se quer.

Depois de criado, o usuário ainda não tem senha definida, o que é feito separadamente:

```
sudo passwd maria
```

Para alterar um usuário já existente, como adicionar ele a um grupo extra, usa-se o `usermod`:

```
sudo usermod -aG docker maria
```

Aqui, `-a` significa "append" (adicionar, sem remover as associações de grupo que já existiam) e `-G` indica o grupo suplementar a adicionar, nesse caso o grupo `docker`. Esquecer o `-a` é um erro comum e perigoso: sem ele, o `usermod -G` substitui completamente a lista de grupos suplementares do usuário, em vez de só adicionar um novo.

## Criando e gerenciando grupos

O comando equivalente para grupos é o `groupadd`:

```
sudo groupadd equipe-dev
```

Grupos existem justamente para simplificar a gestão de permissões apresentada no arquivo sobre [chmod e chown](../03-arquivos-permissoes-processos/02-permissoes-chmod-chown.md): em vez de ajustar permissão arquivo por arquivo para cada pessoa, cria-se um grupo, adicionam-se os usuários relevantes a ele, e as permissões passam a ser controladas no nível do grupo como um todo.

## Por que essa separação importa

Voltando ao princípio já mencionado no arquivo sobre [su e sudo](../03-arquivos-permissoes-processos/04-usuarios-su-sudo.md), sistemas multiusuário existem justamente para isolar o que cada pessoa (ou serviço) pode fazer. Um bom gerenciamento de usuários e grupos é a base prática desse isolamento: cada conta com só as permissões que realmente precisa, cada grupo reunindo pessoas com a mesma necessidade de acesso, nada mais que isso. Essa disciplina, simples na teoria, é uma das defesas mais eficazes contra erros e invasões, tema aprofundado no próximo arquivo, sobre [segurança básica](03-seguranca-basica.md).

## Fontes

- [Linux User and Group Management, GoLinuxCloud](https://www.golinuxcloud.com/linux-user-group-management/)
- [Chapter 4. Managing Users and Groups, Red Hat Documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-managing_users_and_groups)
- [Linux: Guide to useradd, usermod, and groupadd, LinuxBlog.io](https://linuxblog.io/linux-useradd-usermod-groupadd/)
