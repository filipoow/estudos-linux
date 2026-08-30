# Instalando e configurando um servidor web básico

## Juntando tudo num exemplo prático

Este arquivo é, propositalmente, um exercício de aplicação: pegar os comandos de [APT](../04-pacotes-scripts-automacao/01-apt-dpkg-gerenciamento-pacotes.md) e de gerenciamento de [processos e serviços](../03-arquivos-permissoes-processos/03-processos-ps-top-htop.md) já apresentados neste repositório, e usá-los para colocar um servidor web básico no ar. O Nginx foi escolhido aqui como exemplo, por ser leve e um dos mais usados no mundo hoje, mas o mesmo raciocínio geral se aplica ao Apache, a outra opção clássica.

## Instalando

O primeiro passo é sempre atualizar a lista de pacotes disponíveis, e só então instalar:

```
sudo apt update
sudo apt install nginx
```

## Gerenciando o serviço com systemctl

Diferente de um programa comum, um servidor web precisa ficar rodando continuamente em segundo plano, esperando por conexões. Esse tipo de programa é gerenciado pelo systemd, já apresentado no arquivo sobre o [processo de inicialização](../01-fundamentos/07-processo-inicializacao.md), através do comando `systemctl`:

```
sudo systemctl start nginx
sudo systemctl status nginx
sudo systemctl enable nginx
```

O `start` liga o serviço imediatamente. O `status` mostra se ele está rodando, e é o primeiro comando a rodar quando algo parece não estar funcionando. Já o `enable` garante que o Nginx suba automaticamente sempre que o sistema for reiniciado, sem precisar de intervenção manual a cada boot.

Depois de qualquer mudança de configuração, o fluxo recomendado é testar a configuração antes de aplicá-la de fato:

```
sudo nginx -t
sudo systemctl reload nginx
```

O `nginx -t` verifica se a sintaxe da configuração está correta, sem aplicar nada ainda, evitando que um erro de digitação derrube o serviço. O `reload` aplica as mudanças sem interromper as conexões já em andamento, diferente de um `restart`, que reinicia o serviço por completo.

## Onde tudo fica guardado

Seguindo a lógica do FHS, já detalhada no arquivo sobre [estrutura de diretórios](../03-arquivos-permissoes-processos/01-estrutura-diretorios-fhs.md), o Nginx organiza seus arquivos em locais previsíveis:

- **`/var/www/html`**: onde fica a página padrão exibida quando alguém acessa o servidor, e onde normalmente se coloca o conteúdo real do site.
- **`/etc/nginx/sites-available`**: onde ficam os arquivos de configuração de cada site que o servidor pode hospedar.

## Confirmando que está no ar

Depois de instalado e rodando, basta acessar o endereço IP da máquina (ou `localhost`, se for a mesma máquina) num navegador, ou testar direto pelo terminal com uma ferramenta como `curl`, para confirmar que o servidor está respondendo. É também nesse momento que os comandos de diagnóstico de rede, apresentados no [próximo arquivo](06-ping-nslookup-diagnostico.md), se tornam úteis, para confirmar que a máquina está mesmo acessível antes de suspeitar de um problema na configuração do próprio Nginx.

## Fontes

- [Install and configure Nginx, Ubuntu](https://ubuntu.com/tutorials/install-and-configure-nginx)
- [How to Install Nginx Web Server on Ubuntu, phoenixNAP](https://phoenixnap.com/kb/install-nginx-on-ubuntu)
- [How to Install and Configure Nginx as a Web Server on Ubuntu, oneuptime](https://oneuptime.com/blog/post/2026-01-15-install-configure-nginx-ubuntu/view)
