# Diagnosticando rede com ping e nslookup

## Duas perguntas diferentes sobre um mesmo problema

Quando algo não funciona numa rede, seja o acesso a um site, seja a comunicação entre dois servidores, existem basicamente duas perguntas a responder: "esse endereço existe e responde?" e "esse nome de domínio aponta para onde deveria?". O `ping` responde a primeira. O `nslookup` responde a segunda. Juntos, cobrem boa parte do diagnóstico inicial de qualquer problema de conectividade.

## `ping`: existe alguém do outro lado?

O `ping` envia pequenos pacotes de rede, chamados de ICMP Echo Request, para um endereço de destino, e espera pela resposta correspondente. Se a resposta chega, o destino está acessível pela rede, e o `ping` mostra também o tempo de ida e volta de cada pacote, uma medida direta da latência da conexão.

```
ping google.com
```

O comando roda continuamente até ser interrompido (geralmente com `Ctrl + C`), enviando um pacote novo a cada segundo, o que permite observar não só se a conexão funciona, mas também sua estabilidade ao longo do tempo, útil para identificar perda de pacotes intermitente.

Um uso importante do `ping` é justamente separar dois tipos de problema diferentes: se `ping google.com` funciona, mas `ping 8.8.8.8` (o endereço IP direto de um servidor DNS conhecido do Google) também funciona, a rede em si está OK. Se o IP responde mas o nome não, o problema provavelmente está na resolução de nomes, não na conectividade de rede propriamente dita, o que leva direto ao próximo comando.

## `nslookup`: para onde esse nome aponta?

O `nslookup` consulta o DNS (Domain Name System), o sistema que traduz nomes de domínio legíveis por humanos, como `google.com`, em endereços IP que as máquinas realmente usam para se comunicar entre si.

```
nslookup google.com
```

O resultado mostra qual servidor DNS respondeu à consulta e qual endereço IP está associado àquele domínio. É a ferramenta certa quando um site não carrega, mas não fica claro se o problema é de rede, de configuração do próprio site, ou da resolução do nome de domínio em si.

## Um fluxo de diagnóstico simples

Juntando os dois comandos, um roteiro básico e eficaz para investigar "não consigo acessar esse site" seria:

1. `ping` no nome de domínio, para checar se há resposta.
2. Se não houver resposta, `nslookup` no mesmo domínio, para verificar se ele está resolvendo para um endereço IP válido.
3. Se o `nslookup` falhar, o problema está na resolução de nomes (DNS). Se o `nslookup` funcionar mas o `ping` continuar falhando, o problema está mais provavelmente na rede ou no próprio servidor de destino, não estando disponível ou bloqueando esse tipo de pacote.

Esse tipo de raciocínio passo a passo, isolando uma camada do problema de cada vez, é uma habilidade que vale muito mais do que decorar os comandos isoladamente, e é a mesma lógica usada por administradores de rede experientes ao investigar praticamente qualquer falha de conectividade.

## Fontes

- [Linux troubleshooting commands: 4 tools for DNS name resolution problems, Red Hat](https://www.redhat.com/en/blog/DNS-name-resolution-troubleshooting-tools)
- [How to Test Network Connectivity in Linux (ping, traceroute, dig, nslookup), CrownCloud](https://blog.crowncloud.net/post/how-to-test-network-connectivity-in-linux-ping-traceroute-dig-nslookup/)
- [Network Configuration and Troubleshooting Commands in Linux, GeeksforGeeks](https://www.geeksforgeeks.org/linux-unix/network-configuration-trouble-shooting-commands-linux/)
