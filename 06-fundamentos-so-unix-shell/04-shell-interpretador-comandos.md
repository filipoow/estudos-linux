# O funcionamento básico do shell como interpretador de comandos

## O tradutor entre você e o sistema

Todos os comandos apresentados ao longo deste repositório, do simples `ls` ao mais elaborado script combinando pipes e redirecionadores, passam por um mesmo programa antes de virar ação real no sistema: o shell. Entender o que ele de fato faz, por dentro, ajuda a formar uma imagem mais clara de tudo que já foi visto até aqui.

## Um interpretador, no sentido literal da palavra

O shell é, tecnicamente, um interpretador de linha de comando: um programa que fica esperando por texto digitado, interpreta esse texto segundo regras bem definidas, e traduz isso em ações concretas, geralmente executando outros programas. Esse ciclo de "ler, interpretar, executar, esperar de novo" se repete indefinidamente, e é conhecido como um loop de leitura e execução.

Quando você digita `ls -l /home` e aperta Enter, o shell faz, em sequência:

1. **Lê** a linha de texto digitada.
2. **Interpreta** essa linha, identificando que `ls` é o nome de um programa a ser executado, `-l` é uma opção passada para ele, e `/home` é um argumento, o caminho sobre o qual o comando deve atuar.
3. **Localiza** o programa `ls` dentro das pastas do sistema listadas numa variável chamada PATH, algo já mencionado de passagem no arquivo sobre [scripts em shell](../04-pacotes-scripts-automacao/03-scripts-shell.md).
4. **Executa** esse programa, entregando a ele as opções e argumentos identificados.
5. **Aguarda** o programa terminar, mostra o resultado na tela, e volta a esperar por um novo comando.

## Muito além de rodar um comando isolado

Se o shell só soubesse localizar e rodar programas, ele já seria útil, mas boa parte do seu poder vem de recursos adicionais que ele mesmo interpreta antes mesmo de qualquer programa externo entrar em ação. Já vistos em arquivos anteriores deste repositório:

- **Variáveis**, apresentadas no arquivo sobre [scripts em shell](../04-pacotes-scripts-automacao/03-scripts-shell.md), guardadas e substituídas pelo próprio shell antes de um comando rodar.
- **Redirecionadores e pipes**, apresentados no arquivo sobre [redirecionamento](../04-pacotes-scripts-automacao/02-redirecionamento-pipes.md), que são interpretados e montados pelo shell, não pelos programas em si.
- **Expansão de caminhos e chaves**, como o exemplo `mkdir -p pasta/{a,b,c}` visto no arquivo sobre [manipulação de arquivos](../02-terminal-na-pratica/01-manipulacao-arquivos-diretorios.md), onde o próprio shell transforma esse padrão em três comandos separados antes de rodar qualquer coisa.

Esse é um ponto sutil, mas importante: quando um comando "não funciona como esperado", muitas vezes o problema não está no programa que se está tentando rodar, mas em como o shell interpretou (ou falhou em interpretar) o que foi digitado antes de sequer chamar esse programa.

## O shell como programa comum, ele mesmo

Vale reforçar um ponto já explicado no arquivo sobre [shell, kernel e hardware](../02-terminal-na-pratica/06-shell-kernel-hardware.md): o próprio shell é só mais um programa, rodando no espaço de usuário, sem nenhum poder especial sobre o hardware que outros programas comuns não tenham. Toda a "mágica" que ele parece fazer, controlar processos, redirecionar dados, interpretar comandos complexos, acontece inteiramente em software, através de chamadas de sistema como qualquer outro programa, e não por acesso privilegiado direto ao computador.

## Fontes

- [Understanding Terminal, Console, Shell and Kernel, GeeksforGeeks](https://www.geeksforgeeks.org/operating-systems/what-is-terminal-console-shell-and-kernel/)
- [Shell vs Kernel, GeeksforGeeks](https://www.geeksforgeeks.org/operating-systems/difference-between-shell-and-kernel/)
- [Evolution of Unix Shells, machaddr](https://machaddr.substack.com/p/evolution-of-unix-shells-a-comprehensive)
