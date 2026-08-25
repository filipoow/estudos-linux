# Manipulação de arquivos e diretórios pelo terminal

## Por que fazer isso pelo terminal, e não pelo gerenciador de arquivos

Já expliquei no arquivo sobre [linha de comando](04-linha-de-comando.md) por que o terminal é tão central no Linux. Aqui o foco é diferente: entrar de fato na prática de criar, mover, apagar e organizar arquivos e pastas usando só comandos, sem clicar em nada. É basicamente a habilidade mais usada no dia a dia de quem administra um sistema Linux, seja um servidor sem interface gráfica nenhuma, seja um notebook pessoal.

Todo comando que aparece aqui segue o mesmo padrão de uso: o nome do comando, seguido de opções (que geralmente começam com um traço, como `-l`), seguido do que o comando deve afetar, como um nome de arquivo ou pasta.

## Criando pastas: `mkdir`

O comando `mkdir` ("make directory") cria uma pasta nova.

```
mkdir projetos
```

Uma opção bem útil é `-p`, que cria pastas dentro de pastas de uma vez só, mesmo que as intermediárias ainda não existam:

```
mkdir -p projetos/estudos/linux
```

Sem o `-p`, esse comando falharia caso a pasta `projetos` ainda não existisse.

## Criando arquivos vazios: `touch`

O comando `touch` foi feito originalmente para atualizar a data de modificação de um arquivo, mas o uso mais comum no dia a dia é outro: se o arquivo indicado não existe, `touch` cria ele vazio.

```
touch notas.txt
```

É um jeito rápido de criar um arquivo em branco para editar depois, ou para testar se um script tem permissão de escrita numa pasta.

## Listando o conteúdo: `ls`

O `ls` mostra o que existe dentro de uma pasta. Sozinho, ele lista só os nomes. Combinado com opções, mostra muito mais informação:

- `ls -l` mostra uma lista detalhada, com permissões, dono do arquivo, tamanho e data.
- `ls -a` mostra também os arquivos ocultos, aqueles cujo nome começa com ponto.
- `ls -lh` junta o formato detalhado com tamanhos em formato legível (KB, MB, GB), em vez de só bytes.

## Andando entre pastas: `cd`

O `cd` ("change directory") muda a pasta atual em que o terminal está trabalhando.

- `cd nome_da_pasta` entra numa pasta dentro da atual.
- `cd ..` sobe um nível, para a pasta que contém a atual.
- `cd ~` ou só `cd` sem nada leva direto para a pasta pessoal do usuário.
- `cd -` volta para a última pasta em que você estava antes do último `cd`, útil para alternar rápido entre dois lugares.

## Apagando: `rm`

O `rm` ("remove") apaga arquivos. É o comando que mais merece cuidado nesta lista, porque não existe lixeira por padrão no terminal, uma vez apagado, o arquivo se foi.

```
rm notas.txt
```

Para apagar uma pasta inteira, com tudo dentro dela, é preciso combinar duas opções: `-r` (recursivo, para entrar nas subpastas) e `-f` (força, sem pedir confirmação a cada arquivo).

```
rm -rf projetos_antigos
```

Vale grifar: `rm -rf` é poderoso e perigoso na mesma medida. Um espaço digitado no lugar errado nesse comando já causou muita dor de cabeça a administradores de sistema experientes, então é sempre bom conferir o caminho antes de apertar Enter.

## Organizando de forma eficiente

Uma prática comum é usar `mkdir -p` para montar de uma vez toda uma estrutura de pastas de um projeto, e combinar isso com `touch` para já deixar os arquivos principais criados:

```
mkdir -p meu_projeto/{src,docs,testes}
touch meu_projeto/README.md
```

Essa sintaxe com chaves `{}` é um recurso do shell chamado expansão de chaves, ele gera várias palavras a partir de um padrão só, nesse exemplo criando as três subpastas `src`, `docs` e `testes` numa única chamada de comando, em vez de rodar `mkdir` três vezes separadas.

## Fontes

- [mkdir(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/mkdir.1.html)
- [Guide to Unix/Commands/File System Utilities, Wikibooks](https://en.wikibooks.org/wiki/Guide_to_Unix/Commands/File_System_Utilities)
- [Linux Basic Commands: ls, mkdir, rm, cd, pwd, cat, touch, Linux Book Center](https://www.linuxbookcenter.com/linux-basic-commands-ls-mkdir-rm-cd-pwd-cat-touch/)
