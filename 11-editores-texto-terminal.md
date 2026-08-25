# Navegando e editando arquivos com nano e vim

## Por que aprender a editar texto sem sair do terminal

Em algum momento, olhar um arquivo não basta, é preciso alterar o conteúdo dele. Isso acontece o tempo todo em administração de sistemas: editar um arquivo de configuração, corrigir um script, ajustar uma variável de ambiente. Em servidores sem interface gráfica, que é a realidade da maioria das máquinas Linux em produção, não existe outra opção além de editar tudo por dentro do terminal. Por isso, praticamente toda distribuição Linux já vem com pelo menos um editor de texto de terminal instalado.

Os dois editores mais comuns são o `nano` e o `vim`, e eles representam duas filosofias bem diferentes de como editar texto.

## `nano`: simples e direto

O `nano` foi pensado para ser fácil de usar desde o primeiro contato, sem exigir que a pessoa aprenda nada antes de começar a digitar. Para abrir (ou criar) um arquivo:

```
nano arquivo.txt
```

Assim que abre, o cursor já fica pronto para escrever, como num bloco de notas comum. Não existem "modos" diferentes para alternar. Na parte de baixo da tela, o nano mostra os atalhos disponíveis, marcados com o símbolo `^` representando a tecla Ctrl. Os mais usados são:

- `Ctrl + O` para salvar (o "O" vem de "Write Out").
- `Ctrl + X` para sair.
- `Ctrl + K` para recortar uma linha inteira.
- `Ctrl + W` para buscar um trecho de texto.

Por essa simplicidade, o nano costuma ser o editor recomendado para quem está começando, ou para edições rápidas e pontuais, onde não vale a pena o esforço de trocar de ferramenta.

## `vim`: modal e poderoso

O `vim` (evolução do editor `vi`, um dos programas mais antigos ainda em uso no mundo Unix) segue uma lógica bem diferente: ele é um editor modal, o que significa que o teclado se comporta de um jeito diferente dependendo do modo em que o editor está.

Os três modos principais são:

- **Modo normal**: é o modo em que o vim abre por padrão. Nele, as teclas não digitam texto, elas executam comandos, como mover o cursor, apagar uma linha ou copiar um trecho.
- **Modo de inserção**: ativado ao apertar `i`, é o modo em que, finalmente, digitar realmente escreve texto no arquivo, como em qualquer editor comum. Para voltar ao modo normal, aperta-se `Esc`.
- **Modo de comando**: acessado digitando `:` a partir do modo normal, é onde se digitam instruções mais amplas, como salvar ou sair.

Os comandos de saída mais usados no modo de comando são:

- `:w` para salvar (write).
- `:q` para sair, o que só funciona se não houver alterações não salvas.
- `:wq` para salvar e sair de uma vez.
- `:q!` para sair sem salvar, descartando qualquer alteração feita.

Essa curva de aprendizado inicial, que costuma frustrar quem experimenta o vim pela primeira vez (inclusive travando sem saber como sair do programa), é também o motivo da sua força: uma vez memorizados os comandos, editar texto pelo vim é muito mais rápido do que em qualquer editor convencional, porque quase tudo pode ser feito sem tirar as mãos do teclado nem depender do mouse.

## Qual escolher

Não existe resposta certa. O nano é suficiente, e às vezes preferível, para a imensa maioria das edições do dia a dia. Já o vim compensa o investimento de tempo para quem edita arquivos de texto com muita frequência, especialmente programadores e administradores de sistema, que acabam ganhando velocidade real depois de internalizar os comandos. Vale notar que muitas distribuições vêm com o `vi` (a versão original, mais limitada) instalado por padrão, mesmo quando o `vim` completo precisa ser instalado à parte.

## Fontes

- [Vim Basics Tutorial, HowtoForge](https://www.howtoforge.com/vim-basics)
- [Modify Files in Linux using vi, nano or emacs, Baeldung on Linux](https://www.baeldung.com/linux/files-vi-nano-emacs)
- [An Introduction to Text Editors, Get to Know nano and vim, Linux.com](https://www.linux.com/topic/desktop/introduction-text-editors-get-know-nano-and-vim/)
