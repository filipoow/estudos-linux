# Monitorando conexões de rede: netstat e seu substituto moderno

## Completando o conjunto de ferramentas de monitoramento

Os comandos `ps`, `top` e `htop`, já apresentados em profundidade nos arquivos sobre [processos: ps, top e htop](../03-arquivos-permissoes-processos/03-processos-ps-top-htop.md) e [monitoramento do sistema](../02-terminal-na-pratica/05-monitoramento-sistema.md), respondem à pergunta "o que está rodando e consumindo recursos nesta máquina". O `netstat` responde a uma pergunta diferente e complementar: quais conexões de rede estão abertas, e quais programas estão escutando em quais portas.

```
netstat -tulnp
```

Essa combinação de opções costuma ser a mais usada: `-t` mostra conexões TCP, `-u` mostra UDP, `-l` mostra só portas em modo de escuta (aguardando conexões), `-n` mostra endereços numéricos em vez de tentar resolver nomes (o que deixa o comando mais rápido), e `-p` mostra qual processo é o dono de cada conexão, uma informação valiosa para descobrir, por exemplo, qual programa está usando uma porta específica.

## Um detalhe importante: o netstat está sendo substituído

Vale um aviso direto: o `netstat` é considerado obsoleto desde o início dos anos 2000, e já foi removido da instalação padrão de diversas distribuições Linux modernas. Seu substituto recomendado é o `ss` ("socket statistics"), que consulta as informações diretamente do kernel através de uma interface mais moderna, sendo tanto mais rápido quanto mais rico em informação do que o `netstat`.

```
ss -tulnp
```

Repare que a sintaxe é quase idêntica à do `netstat`, o que torna a transição bem tranquila para quem já está acostumado com o comando antigo. Outros comandos de rede também foram redistribuídos entre ferramentas mais modernas: `netstat -r`, que mostrava a tabela de rotas, foi substituído por `ip route`, já apresentado no arquivo sobre [interfaces de rede](../05-rede-usuarios-seguranca/01-interfaces-rede.md).

## Por que essa informação importa no dia a dia

Saber quais portas estão abertas, e quais processos as controlam, é essencial tanto para diagnóstico quanto para segurança. Do lado do diagnóstico, combina bem com os comandos de conectividade já apresentados no arquivo sobre [ping e nslookup](../05-rede-usuarios-seguranca/06-ping-nslookup-diagnostico.md): se um serviço deveria estar respondendo numa porta específica e não está, `ss -tlnp` mostra rapidamente se o programa sequer chegou a abrir aquela porta. Do lado da segurança, tema já aprofundado no arquivo sobre [segurança básica](../05-rede-usuarios-seguranca/03-seguranca-basica.md), revisar periodicamente quais portas estão abertas ajuda a identificar serviços expostos sem necessidade, ou até processos suspeitos escutando em portas que não deveriam estar em uso.

## Fontes

- [netstat vs ss usage guide on Linux, Computing for Geeks](https://computingforgeeks.com/netstat-vs-ss-usage-guide-linux/)
- [An Introduction to the ss Command, Linux Foundation](https://training.linuxfoundation.org/resources/tutorials/an-introduction-to-the-ss-command/)
- [Linux tools: How to use the ss command, Red Hat](https://www.redhat.com/en/blog/ss-command)
