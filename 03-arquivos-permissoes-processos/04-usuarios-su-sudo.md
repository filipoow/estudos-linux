# Controle de sessão: usuários comuns, superusuário, su e sudo

## Por que o Linux separa usuários comuns de administradores

Desde seus primórdios como sistema derivado do Unix, o Linux foi desenhado para ser usado por várias pessoas ao mesmo tempo, cada uma com sua própria conta, seus próprios arquivos e seu próprio nível de permissão. Essa separação, mesmo num computador pessoal usado só por uma pessoa, continua sendo importante hoje por um motivo de segurança: se o usuário do dia a dia tivesse poder irrestrito sobre o sistema o tempo todo, qualquer erro de digitação ou programa malicioso poderia destruir o sistema inteiro sem esforço nenhum.

## O superusuário, ou root

Existe, em todo sistema Linux, uma conta especial chamada root, também conhecida como superusuário. Diferente de qualquer conta comum, o root não está sujeito às regras normais de permissão de arquivos, ele pode ler, escrever, apagar e executar absolutamente qualquer coisa no sistema, além de instalar programas, criar outros usuários e alterar configurações centrais. É justamente por esse poder total que o uso direto e constante da conta root é considerado uma má prática, o risco de um erro grave aumenta muito quando não existe nenhuma barreira de proteção entre um comando digitado errado e o sistema inteiro.

## `su`: virando outro usuário

O comando `su` ("substitute user" ou "switch user") troca a sessão atual do terminal para outro usuário, pedindo a senha dessa conta de destino.

```
su
```

Sem nenhum nome depois, o `su` tenta virar o root por padrão, pedindo a senha do root. A partir daí, todos os comandos digitados rodam como se fossem executados pelo próprio root, até a pessoa digitar `exit` para voltar à sessão original. Também é possível indicar outro usuário específico:

```
su maria
```

## `sudo`: pedindo permissão emprestada, comando por comando

O `sudo` ("superuser do") segue uma lógica diferente: em vez de trocar de identidade por completo, ele executa um único comando com privilégios de administrador, pedindo a senha da própria conta que está usando o `sudo` (não a senha do root).

```
sudo apt update
```

Depois de rodar, a sessão volta imediatamente ao usuário comum, sem deixar uma sessão de root aberta esperando por um próximo comando. Esse comportamento reduz bastante o risco de um comando perigoso ser digitado por engano numa sessão de root esquecida aberta.

## A diferença que realmente importa

A distinção central entre os dois não é técnica, é de postura de segurança. Com `su`, a pessoa efetivamente se torna o root, com acesso irrestrito enquanto durar aquela sessão, e depende do conhecimento da senha real da conta root. Com `sudo`, a pessoa continua sendo ela mesma, apenas emprestando privilégio para um comando específico, autenticando com sua própria senha, e o sistema pode inclusive registrar exatamente quais usuários rodaram quais comandos com privilégio elevado, algo que ajuda muito em auditorias de segurança.

Por esse motivo, distribuições voltadas a usuários comuns, como o Ubuntu, chegam a desabilitar login direto na conta root por padrão, incentivando o uso do `sudo` no lugar. Isso não elimina o superusuário do sistema, ele continua existindo e continua sendo essencial para certas tarefas, apenas muda o jeito como as pessoas normalmente chegam até ele.

## Fontes

- [Sudo Vs Su: The Difference Between sudo and su Explained, phoenixNAP](https://phoenixnap.com/kb/sudo-vs-su-differences)
- [Exploring the differences between sudo and su commands in Linux, Red Hat](https://www.redhat.com/en/blog/difference-between-sudo-su)
- [Difference between the root user and super (sudo) user, Computer Networking Notes](https://www.computernetworkingnotes.com/linux-tutorials/difference-between-the-root-user-and-super-sudo-user.html)
