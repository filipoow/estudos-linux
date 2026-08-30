# Gerenciando pacotes no Debian e derivados: APT e dpkg

## Duas camadas, um mesmo objetivo

No arquivo sobre [distribuições Linux](../01-fundamentos/05-distribuicoes-linux.md) já ficou claro que cada família de distribuição tem seu próprio gerenciador de pacotes. Aqui o foco é entender, com mais profundidade, como isso funciona na família Debian (que inclui Ubuntu, Linux Mint e várias outras), através de duas camadas de ferramentas que trabalham juntas, mas em níveis diferentes.

## `dpkg`: a camada mais baixa

O `dpkg` (Debian Package) é a ferramenta fundamental por trás de tudo. Ele sabe instalar, remover e consultar informações sobre um pacote `.deb` específico, o formato de arquivo usado para distribuir software nessa família de distribuições. O detalhe importante é que o `dpkg` trabalha sozinho, sem entender nada sobre de onde aquele pacote veio ou do que ele depende para funcionar.

```
sudo dpkg -i pacote.deb
```

Esse comando instala um arquivo `.deb` já baixado no computador. O problema é que, se esse pacote depender de outros programas que ainda não estão instalados, o `dpkg` simplesmente falha, sem saber buscar e instalar essas dependências sozinho. É justamente esse buraco que o APT foi criado para preencher.

## APT: a camada que resolve dependências e busca pacotes

O APT (Advanced Package Tool) é construído por cima do `dpkg`, adicionando duas capacidades essenciais: buscar pacotes em repositórios remotos, configurados no sistema (tradicionalmente no arquivo `/etc/apt/sources.list`), e resolver automaticamente a árvore de dependências de cada pacote, baixando e instalando tudo que for necessário para aquele programa funcionar, sem exigir que o usuário descubra isso manualmente.

Dentro do universo do APT existem, na prática, algumas ferramentas diferentes, cada uma com um histórico próprio:

**`apt-get`**: é a ferramenta mais antiga e tradicional para instalar, atualizar e remover pacotes.

```
sudo apt-get update
sudo apt-get install nome-do-pacote
```

O `apt-get update` atualiza a lista local de pacotes disponíveis nos repositórios configurados, sem instalar nada ainda, é sempre o primeiro comando a rodar antes de instalar algo novo, para garantir que o sistema saiba quais são as versões mais recentes disponíveis.

**`apt-cache`**: serve para consultar informações sobre pacotes sem instalar nada, como buscar pelo nome ou ver detalhes e dependências de um pacote específico.

```
apt-cache search "editor de texto"
apt-cache show nome-do-pacote
```

**`apt`**: é a ferramenta mais recente, pensada para unir num só comando o que antes exigia alternar entre `apt-get` e `apt-cache`, com uma saída mais organizada e amigável para uso direto no terminal por uma pessoa.

```
sudo apt update
sudo apt install nome-do-pacote
sudo apt search "editor de texto"
```

Na prática, para uso interativo do dia a dia, o comando `apt` é hoje o recomendado. Já o `apt-get` e o `apt-cache` continuam sendo preferidos dentro de scripts de automação, porque sua saída e seu comportamento são mais estáveis e previsíveis entre versões diferentes do sistema, algo que o `apt` não garante da mesma forma.

## Onde tudo isso fica guardado

Voltando à ideia de hierarquia de arquivos, apresentada no arquivo sobre o [FHS](../03-arquivos-permissoes-processos/01-estrutura-diretorios-fhs.md), o APT também segue essa organização: os repositórios configurados ficam em `/etc/apt`, os pacotes `.deb` já baixados ficam guardados temporariamente em `/var/cache/apt`, e os programas instalados de fato se espalham pelas pastas padrão do sistema, principalmente dentro de `/usr`.

## Fontes

- [Difference Between APT and DPKG in Ubuntu, GeeksforGeeks](https://www.geeksforgeeks.org/linux-unix/difference-between-apt-and-dpkg-in-ubuntu/)
- [Chapter 8. The Debian package management tools, debian.org](https://www.debian.org/doc/manuals/debian-faq/pkgtools.en.html)
- [dpkg vs apt vs apt-get: which to use and key differences, Mundo Bytes](https://mundobytes.com/en/dpkg-vs-apt-vs-apt-get:-real-differences-and-when-to-use-each-one/)
