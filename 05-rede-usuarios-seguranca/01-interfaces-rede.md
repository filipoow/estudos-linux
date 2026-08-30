# Configurando e gerenciando interfaces de rede no Linux

## Toda máquina Linux tem, no mínimo, uma interface de rede

Uma interface de rede é o ponto de conexão do sistema com uma rede, seja uma placa Ethernet física, uma antena Wi-Fi, ou uma interface virtual criada por software, como a interface de loopback (`lo`), que todo sistema Linux tem por padrão e representa a própria máquina falando consigo mesma. Gerenciar essas interfaces significa poder consultar, ativar, desativar e configurar endereços para cada uma delas.

## `ip`: a ferramenta moderna

Por muitos anos, o comando de referência para essa tarefa foi o `ifconfig`. Ele ainda aparece em tutoriais antigos, mas está oficialmente descontinuado desde o início dos anos 2000, e vem sendo substituído pelo comando `ip`, parte do pacote iproute2, ativamente mantido e com suporte a recursos de rede mais modernos que o `ifconfig` nunca chegou a cobrir.

Para listar as interfaces de rede disponíveis e seus endereços IP:

```
ip addr show
```

O resultado mostra cada interface com seu nome, seu estado (ativa ou não) e o endereço IP configurado, já incluindo a máscara de sub-rede no formato CIDR (por exemplo, `192.168.1.50/24`), o que facilita bastante entender o tamanho daquela rede sem cálculo adicional.

Para ver só o estado das interfaces, sem os endereços:

```
ip link show
```

E para ativar ou desativar uma interface específica:

```
sudo ip link set eth0 up
sudo ip link set eth0 down
```

## `nmcli`: quando o NetworkManager está no comando

Em boa parte das distribuições voltadas a desktop, a rede é gerenciada por um serviço chamado NetworkManager, e a ferramenta de linha de comando para controlá-lo é o `nmcli`. Ela cobre tarefas como listar redes Wi-Fi disponíveis e conectar a elas, algo que o `ip` sozinho não faz, já que ele lida com a configuração de baixo nível da interface, não com a negociação de uma rede sem fio.

```
nmcli device wifi list
nmcli device wifi connect "Nome-da-Rede" password "senha"
```

## Por que isso importa na prática

Boa parte dos problemas de conectividade em um servidor Linux começa exatamente aqui: uma interface que não subiu, um endereço IP que não foi atribuído corretamente, ou uma rota que aponta para o lugar errado. Saber consultar rapidamente o estado das interfaces com `ip addr show` costuma ser o primeiro passo de qualquer diagnóstico de rede, antes mesmo de partir para ferramentas mais específicas, como o `ping` e o `nslookup`, apresentados no arquivo sobre [diagnóstico de rede](06-ping-nslookup-diagnostico.md).

## Fontes

- [24 Useful "IP" Commands to Configure Network Interfaces, Tecmint](https://www.tecmint.com/ip-command-examples/)
- [How to Use the 'ip' Command for Modern Linux Network Configuration, Digitash](https://digitash.com/linux/terminal/use-ip-command-modern-linux-network-configuration/)
- [Persistent Network Configuration - ip, nmcli, NetworkManager, Penguin Gym Linux](https://penguin-gym-linux.com/en/articles/lpic/network-configuration)
