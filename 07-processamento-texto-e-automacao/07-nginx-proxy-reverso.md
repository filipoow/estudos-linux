# Nginx como proxy reverso: instalação, configuração e gerenciamento

## Um novo papel para uma ferramenta já conhecida

O arquivo sobre [servidor web básico](../05-rede-usuarios-seguranca/05-servidor-web-basico.md) mostrou o Nginx servindo páginas diretamente. Existe um segundo uso, tão comum quanto esse, que vale a pena aprofundar: usar o Nginx como proxy reverso, uma camada que fica entre o mundo externo e uma ou mais aplicações internas.

## O que é um proxy reverso

Num proxy reverso, o cliente (o navegador de alguém, por exemplo) se conecta ao Nginx, e é o Nginx quem decide para qual servidor interno encaminhar aquela requisição, recebe a resposta desse servidor, e a devolve ao cliente como se tivesse sido ele mesmo quem respondeu. A aplicação real, muitas vezes escrita em outra linguagem e rodando numa porta interna qualquer, nunca fica exposta diretamente à internet.

Isso é diferente de um proxy comum (direto), que atua no sentido contrário, intermediando o acesso de um cliente a servidores externos. O proxy reverso protege e organiza o acesso a servidores internos.

## Configuração básica com `proxy_pass`

A instalação do Nginx segue exatamente o que já foi apresentado no arquivo sobre [servidor web básico](../05-rede-usuarios-seguranca/05-servidor-web-basico.md), usando o `apt`, já detalhado no arquivo sobre [APT e dpkg](../04-pacotes-scripts-automacao/01-apt-dpkg-gerenciamento-pacotes.md). O que muda é a configuração, guardada em `/etc/nginx/sites-available`:

```
server {
    listen 80;
    server_name meusite.com;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

A diretiva `proxy_pass` é o coração dessa configuração: ela diz ao Nginx para onde encaminhar as requisições que chegam em `/`, nesse caso uma aplicação rodando localmente na porta 3000. As diretivas `proxy_set_header` repassam informações importantes para a aplicação interna, como o endereço IP real de quem fez a requisição, que de outra forma apareceria como sendo o próprio Nginx.

Um detalhe que costuma causar confusão: a presença ou ausência de uma barra no final da URL em `proxy_pass` muda o comportamento. Com barra no final, o Nginx substitui o caminho correspondente pela URI indicada; sem ela, o caminho original completo é repassado sem alteração.

## Aplicando a configuração com segurança

O fluxo de aplicar mudanças, já apresentado no arquivo sobre [servidor web básico](../05-rede-usuarios-seguranca/05-servidor-web-basico.md), continua valendo aqui: testar antes de aplicar, e recarregar sem derrubar conexões em andamento.

```
sudo nginx -t
sudo systemctl reload nginx
```

## Acompanhando o que o proxy está fazendo

Como qualquer serviço gerenciado pelo systemd, o estado do Nginx pode ser consultado com `systemctl status nginx`, e seus registros de log são acessíveis pelo `journalctl`, apresentado em detalhe no arquivo sobre [logs e journalctl](../05-rede-usuarios-seguranca/04-logs-journalctl.md):

```
journalctl -u nginx -f
```

Esse comando acompanha em tempo real os logs do serviço Nginx, essencial para diagnosticar por que uma requisição não está chegando corretamente à aplicação interna, um dos problemas mais comuns ao configurar um proxy reverso pela primeira vez.

## Fontes

- [NGINX Reverse Proxy, NGINX Documentation](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)
- [NGINX Reverse Proxy: Setup Guide with proxy_pass Examples, getpagespeed](https://www.getpagespeed.com/server-setup/nginx/nginx-reverse-proxy)
- [How to Configure Nginx as a Reverse Proxy for Microservices, oneuptime](https://oneuptime.com/blog/post/2026-02-20-nginx-reverse-proxy-guide/view)
