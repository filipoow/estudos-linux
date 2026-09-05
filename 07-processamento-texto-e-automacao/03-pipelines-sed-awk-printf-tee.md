# A arquitetura de pipelines: encadeando grep, sed, awk, printf e tee

## Um pipeline é uma linha de montagem de dados

O arquivo sobre [redirecionamento e pipes](../04-pacotes-scripts-automacao/02-redirecionamento-pipes.md) já explicou o mecanismo básico do pipe (`|`): conectar a saída de um comando à entrada do próximo. Uma pipeline de processamento de texto é justamente isso aplicado várias vezes em sequência, cada comando fazendo uma transformação específica sobre os dados, até chegar no resultado final desejado. É a aplicação mais direta da filosofia Unix, apresentada no [arquivo correspondente](../06-fundamentos-so-unix-shell/03-filosofia-unix.md): ferramentas pequenas, cada uma fazendo uma coisa, combinadas para resolver um problema maior.

## `sed`: editando texto conforme ele passa

O `sed` (stream editor) lê um texto linha por linha e aplica uma transformação a cada uma, sem exigir um editor interativo como o `vim`, apresentado no arquivo sobre [editores de texto](../02-terminal-na-pratica/03-editores-texto-terminal.md). O uso mais comum é a substituição de texto:

```
sed 's/erro/ERRO/g' log.txt
```

Esse comando substitui, em cada linha, a palavra "erro" por "ERRO". A letra `s` indica substituição, e o `g` no final indica que a troca deve acontecer em todas as ocorrências da linha, não só na primeira.

## `printf`: controlando a formatação da saída

Enquanto o `echo`, já usado em exemplos anteriores, imprime texto de forma simples, o `printf` permite controlar precisamente o formato da saída, de um jeito parecido com a função homônima da linguagem C, incluindo casas decimais, largura de campo e tipo de dado.

```
printf "Usuario: %s, Tentativas: %d\n" "maria" 3
```

Esse controle fino de formatação é especialmente útil ao gerar relatórios ou logs próprios dentro de um script, onde a apresentação consistente do resultado importa.

## `tee`: dividindo o caminho sem interromper a pipeline

O `tee` resolve um problema comum: às vezes é necessário tanto salvar o resultado de uma pipeline num arquivo quanto continuar vendo (ou processando) esse resultado na tela ou na próxima etapa da pipeline. Sem o `tee`, seria preciso escolher um ou outro, redirecionando para um arquivo com `>` (perdendo a visualização) ou deixando na tela (sem salvar). O `tee` duplica o fluxo, mandando o mesmo conteúdo para um arquivo e para a saída padrão ao mesmo tempo, permitindo que a pipeline continue adiante normalmente.

```
comando | tee saida.txt | grep "erro"
```

Esse exemplo salva a saída completa do comando em `saida.txt`, e ao mesmo tempo continua filtrando essa mesma saída com `grep` em busca da palavra "erro", sem que uma coisa atrapalhe a outra.

## Um pipeline completo, juntando tudo

Reunindo ferramentas já apresentadas neste repositório, um exemplo realista de análise de log poderia ser:

```
cat acesso.log | grep "POST" | awk '{print $1}' | sort | uniq -c | sort -rn | tee resumo.txt
```

Lendo da esquerda para a direita: mostra o log inteiro, filtra só as linhas com requisições POST, extrai o primeiro campo (tipicamente o endereço IP), ordena essa lista, conta quantas vezes cada IP aparece, ordena o resultado do maior para o menor, e por fim salva esse resumo num arquivo enquanto ainda mostra o resultado na tela. Nenhum desses comandos, sozinho, resolve o problema todo, mas encadeados, formam uma análise completa em uma única linha.

## Fontes

- [grep, awk and sed, three VERY useful command-line utilities, University of York](https://www-users.york.ac.uk/~mijp1/teaching/2nd_year_Comp_Lab/guides/grep_awk_sed.pdf)
- [Tee (command), Wikipédia](https://en.wikipedia.org/wiki/Tee_%28command%29)
- [Supercharge Text Processing with awk and sed, Medium](https://medium.com/@sre999/supercharge-text-processing-with-awk-and-sed-practical-guide-speed-hacks-8b592e15c082)
