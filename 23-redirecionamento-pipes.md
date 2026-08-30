# Redirecionando entradas e saídas: pipes e redirecionadores no shell

## Todo programa tem três canais padrão

Para entender redirecionamento, é preciso conhecer um conceito que fica escondido na maior parte do uso comum do terminal: todo programa executado no Linux nasce com três canais de comunicação padrão já abertos, chamados de streams. A entrada padrão (stdin) é de onde o programa lê dados, normalmente o teclado. A saída padrão (stdout) é para onde o programa envia seus resultados normais, normalmente a própria tela do terminal. E o erro padrão (stderr) é um canal separado, também normalmente exibido na tela, reservado especificamente para mensagens de erro.

O motivo de existirem canais separados para stdout e stderr, mesmo os dois aparecendo juntos na tela por padrão, é justamente permitir que cada um seja redirecionado de forma independente, o que é a base de tudo que vem a seguir.

## `>`: redirecionando a saída para um arquivo

O operador `>` pega a saída padrão de um comando e, em vez de mostrar na tela, escreve dentro de um arquivo, substituindo qualquer conteúdo que já existisse nele.

```
ls -l > lista.txt
```

Esse comando não mostra nada na tela, o resultado do `ls -l` vai inteiro para dentro do arquivo `lista.txt`. Um cuidado importante: se o arquivo já existir, seu conteúdo anterior é apagado sem aviso.

## `>>`: acrescentando, sem apagar o que já existe

O operador `>>` funciona de forma parecida com o `>`, mas em vez de sobrescrever o arquivo, ele acrescenta o novo conteúdo ao final do que já existia.

```
echo "novo registro" >> log.txt
```

Esse comando é muito usado justamente para ir acumulando informação num mesmo arquivo ao longo do tempo, como um histórico ou um log, sem perder o que já foi escrito antes.

## `<`: redirecionando a entrada a partir de um arquivo

O operador `<` faz o caminho inverso dos anteriores: em vez de mandar a saída de um comando para um arquivo, ele pega o conteúdo de um arquivo e entrega como se fosse a entrada padrão de um comando, como se o conteúdo do arquivo tivesse sido digitado no teclado.

```
comando < arquivo_de_entrada.txt
```

É um uso menos comum no dia a dia do que `>` e `>>`, mas aparece com frequência em scripts que processam dados de um arquivo, ou em programas que esperam receber informação por esse canal específico.

## `2>` e `2>&1`: separando os erros

Como o stderr é um canal independente do stdout, ele precisa de um operador próprio para ser redirecionado, identificado pelo número do canal (2, o identificador padrão do stderr).

```
comando 2> erros.txt
```

Isso manda só as mensagens de erro para o arquivo, enquanto a saída normal continua aparecendo na tela. Para juntar os dois canais num único destino, usa-se `2>&1`, que direciona o stderr para o mesmo lugar já definido para o stdout:

```
comando > tudo.txt 2>&1
```

A ordem aqui importa: o shell processa os redirecionamentos da esquerda para a direita, então primeiro o stdout é direcionado para `tudo.txt`, e só depois o stderr é apontado para seguir o mesmo caminho que o stdout já estava seguindo.

## `|`: conectando a saída de um comando à entrada de outro

O pipe, já mencionado em arquivos anteriores como o de [visualização e filtragem de conteúdo](10-visualizacao-filtragem-conteudo.md), conecta diretamente a saída padrão de um comando à entrada padrão do próximo, sem passar por um arquivo intermediário no meio do caminho.

```
ps aux | grep firefox
```

Vale um detalhe técnico interessante: por padrão, só o stdout passa pelo pipe, o stderr continua sendo mostrado direto na tela. Essa escolha de design é proposital, garante que mensagens de erro continuem visíveis imediatamente, mesmo quando a saída normal do comando está sendo encaminhada para outro programa em vez de aparecer na tela.

## Fontes

- [Linux Redirection and Pipes, GoLinuxCloud](https://www.golinuxcloud.com/linux-redirection-pipes/)
- [Five ways to use redirect operators in Bash, Red Hat](https://www.redhat.com/en/blog/redirect-operators-bash)
- [Unix/Linux Shell I/O Redirection, teaching.idallen.com](https://teaching.idallen.com/cst8207/19w/notes/200_redirection.html)
