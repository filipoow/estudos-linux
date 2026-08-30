# Práticas de segurança no Linux para proteção de dados

## Segurança é uma soma de camadas, não uma única ação

Não existe um comando único que "deixa um Linux seguro". Segurança de sistema funciona melhor como uma pilha de camadas, cada uma cobrindo o que a anterior deixa passar. As práticas a seguir são consideradas a base mínima recomendada para qualquer máquina Linux exposta a uma rede, especialmente servidores.

## Princípio do menor privilégio

Esse princípio já apareceu de forma indireta nos arquivos sobre [su e sudo](../03-arquivos-permissoes-processos/04-usuarios-su-sudo.md) e sobre [usuários e grupos](02-usuarios-grupos.md): cada conta do sistema deve ter só o acesso mínimo necessário para cumprir sua função, nada além disso. Na prática, isso significa evitar logins diretos como root, preferir `sudo` para tarefas administrativas pontuais, e revisar periodicamente quais usuários pertencem a grupos privilegiados, como o grupo `sudo` ou `docker`.

## Autenticação por chave SSH em vez de senha

O acesso remoto a servidores Linux normalmente acontece via SSH. Autenticar por senha, mesmo uma senha forte, é vulnerável a ataques de força bruta, tentativas automatizadas e repetidas de adivinhar a senha correta. A alternativa mais segura é a autenticação por par de chaves: uma chave privada, que fica só na máquina de quem acessa, e uma chave pública, copiada para o servidor.

```
ssh-keygen
```

Esse comando gera o par de chaves. A chave pública gerada é então copiada para o arquivo `~/.ssh/authorized_keys` no servidor. Depois de configurado, é uma boa prática desabilitar completamente a autenticação por senha no SSH, deixando só a autenticação por chave disponível, o que elimina de vez o risco de força bruta contra senha.

## Firewall: expor só o necessário

Um firewall controla quais portas de rede aceitam conexão, e a recomendação padrão é a política de negação por padrão: bloquear tudo, e liberar explicitamente só as portas realmente necessárias, como a porta 22 (SSH) e a 443 (HTTPS), quando aplicável. No Linux, isso é feito por ferramentas como o `iptables` (mais antiga e de baixo nível) ou o `firewalld` (mais recente e amigável).

## Manter o sistema atualizado

Já vimos no arquivo sobre [APT e dpkg](../04-pacotes-scripts-automacao/01-apt-dpkg-gerenciamento-pacotes.md) como atualizar pacotes no sistema. Do ponto de vista de segurança, manter essas atualizações em dia não é opcional: falhas de segurança conhecidas são corrigidas constantemente pelos mantenedores de cada distribuição, e um sistema desatualizado acumula vulnerabilidades já publicamente documentadas, o que o torna um alvo bem mais fácil. Automatizar essas atualizações, usando os conceitos apresentados no arquivo sobre [CronTab](../04-pacotes-scripts-automacao/04-crontab-agendamento.md), reduz bastante essa janela de exposição.

## Acompanhar o que acontece no sistema

Nenhuma das práticas acima é útil se ninguém perceber quando algo sai do esperado. É por isso que os registros de log, tema do próximo arquivo sobre [logs e journalctl](04-logs-journalctl.md), são parte da segurança tanto quanto qualquer ferramenta de bloqueio: eles são o que permite identificar uma tentativa de invasão, um acesso fora do padrão, ou uma falha de configuração antes que ela vire um problema maior.

## Fontes

- [8 Essential Linux Security Best Practices, Wiz](https://www.wiz.io/academy/cloud-security/linux-security-best-practices)
- [Securing Remote Access to Linux Servers: Best Practices, linuxsecurity.com](https://linuxsecurity.com/news/server-security/secure-remote-access-linux-servers)
- [Linux Security Basics, Cycle.io](https://cycle.io/learn/linux-security-basics)
