# A utilização da linha de comando no Linux

## Por que ainda usar terminal em pleno século com interfaces gráficas

Quem começa a usar Linux logo esbarra na linha de comando, mesmo usando uma distribuição com interface gráfica bonita e moderna. Isso acontece porque, no mundo Linux, a linha de comando não é um recurso secundário para quem "não sabe usar o mouse", ela é a forma mais direta, previsível e poderosa de controlar o sistema. Praticamente tudo que existe como botão numa interface gráfica também pode ser feito digitando um comando, e o contrário nem sempre é verdade: existem tarefas de administração de sistema, automação e diagnóstico que simplesmente não têm um botão equivalente.

## Shell, terminal e prompt: entendendo os termos

Vale separar três palavras que costumam ser usadas meio misturadas. O terminal (ou emulador de terminal) é o programa com uma janela onde você digita e vê o texto, é a "casca visual". O shell é o programa que de fato interpreta os comandos que você digita e conversa com o sistema operacional, o mais comum em distribuições Linux é o Bash, mas existem outros, como o Zsh ou o Fish. O prompt é aquele texto que aparece esperando você digitar algo, geralmente terminando com `$` para usuário comum ou `#` para o usuário root, o administrador do sistema.

## A árvore de diretórios

Diferente do Windows, que separa o sistema em letras de unidade (C:, D:), o Linux organiza tudo numa única árvore de diretórios, começando na raiz, representada por uma barra `/`. A partir dela, existem pastas com propósitos padronizados, seguindo uma convenção chamada Filesystem Hierarchy Standard:

- `/home` guarda os arquivos pessoais de cada usuário, parecido com a pasta "Usuários" do Windows.
- `/etc` guarda arquivos de configuração do sistema e dos programas instalados.
- `/usr` guarda a maior parte dos programas e bibliotecas instalados.
- `/var` guarda dados que mudam com frequência, como registros de log.
- `/bin` e `/sbin` guardam programas essenciais, usados pelo próprio sistema para funcionar.

Entender essa estrutura ajuda bastante a não se perder quando um comando pede um caminho de arquivo.

## Comandos que valem a pena aprender primeiro

Alguns comandos formam a base de praticamente qualquer tarefa no terminal:

- `pwd`: mostra em qual diretório você está agora (o nome vem de "print working directory").
- `ls`: lista os arquivos e pastas do diretório atual. Com `ls -a`, ele também mostra arquivos ocultos, aqueles cujo nome começa com ponto.
- `cd`: muda de diretório. `cd ..` sobe um nível, `cd -` volta para o diretório anterior em que você estava.
- `cp`: copia um arquivo ou pasta.
- `mv`: move um arquivo, e também é o comando usado para renomear arquivos.
- `rm`: apaga arquivos. É bom usar com cuidado, porque no terminal não existe lixeira por padrão, o arquivo some.
- `mkdir`: cria uma pasta nova.
- `cat`: mostra o conteúdo de um arquivo de texto direto na tela.
- `grep`: procura um trecho de texto dentro de arquivos, muito usado para filtrar informação.
- `chmod`: altera as permissões de um arquivo, quem pode ler, escrever ou executar.
- `sudo`: executa um comando com privilégios de administrador, algo parecido com "Executar como administrador" no Windows.

## O poder de combinar comandos

Uma das razões pelas quais a linha de comando é tão produtiva é a possibilidade de encadear comandos simples para resolver problemas mais complexos. O símbolo `|`, chamado de pipe, pega a saída de um comando e usa como entrada do próximo. Por exemplo, `ls -l | grep ".txt"` lista os arquivos da pasta atual e filtra só os que têm ".txt" no nome. Já os símbolos `>` e `>>` redirecionam a saída de um comando para dentro de um arquivo, o primeiro sobrescrevendo o conteúdo, o segundo adicionando ao final. Essa lógica de pequenos programas que fazem uma coisa só, combinados entre si, é uma herança direta da filosofia Unix, e é um dos motivos pelos quais o terminal continua sendo tão relevante décadas depois de ter sido criado.

## Fontes

- [The Linux command line for beginners, Ubuntu](https://ubuntu.com/tutorials/command-line-for-beginners)
- [Linux Filesystem Hierarchy, GoLinuxCloud](https://www.golinuxcloud.com/linux-filesystem-hierarchy/)
- [Filesystem Hierarchy Standard, man7.org](https://man7.org/linux/man-pages/man7/hier.7.html)
- [The Unix Shell: Summary of Basic Commands, Software Carpentry](https://swcarpentry.github.io/shell-novice/reference.html)
