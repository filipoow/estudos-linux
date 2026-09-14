# Boas práticas ao escrever scripts: shebang e shellcheck

## Um shebang mais cuidadoso do que parece

O arquivo sobre [scripts em shell](../04-pacotes-scripts-automacao/03-scripts-shell.md) já apresentou o shebang básico, `#!/bin/bash`. Vale um refinamento importante: em sistemas diferentes, o Bash nem sempre está instalado exatamente em `/bin/bash`. Por isso, uma prática cada vez mais recomendada é usar:

```bash
#!/usr/bin/env bash
```

Em vez de apontar para um caminho fixo, essa forma pede ao programa `env` para localizar o `bash` usando a variável `PATH` do sistema, mencionada no arquivo sobre [variáveis de ambiente](01-variaveis-ambiente-locais.md), o que torna o script mais portável entre sistemas diferentes, sem depender de que o Bash esteja sempre no mesmo lugar. A escolha entre as duas formas depende do contexto: para scripts pensados para rodar amplamente, `#!/usr/bin/env bash` tende a ser mais seguro; para scripts de sistema muito específicos, onde o caminho exato já é conhecido e controlado, `#!/bin/bash` direto também é uma escolha válida.

Vale reforçar também: um script sem nenhum shebang é ambíguo, o sistema pode interpretá-lo com `sh`, `bash` ou outro shell padrão, dependendo de como for chamado, e esses shells nem sempre se comportam da mesma forma diante do mesmo código, como já visto no arquivo sobre [tipos de shell](../06-fundamentos-so-unix-shell/05-tipos-de-shell.md).

## `shellcheck`: um revisor automático para scripts

Mesmo seguindo boas práticas conscientemente, é fácil cometer erros sutis num script de shell, como esquecer de colocar uma variável entre aspas, um problema detalhado no arquivo sobre [aspas e expansão de variáveis](02-aspas-expansao-variaveis.md). O `shellcheck` é uma ferramenta de análise estática, feita especificamente para scripts de shell, que lê o código sem executá-lo e aponta problemas comuns antes que eles causem um erro real em produção.

```
shellcheck meu_script.sh
```

Entre os problemas que o `shellcheck` costuma identificar estão variáveis usadas sem aspas, uso de sintaxe obsoleta (como crases para substituição de comando, em vez de `$()`), comandos `rm` potencialmente perigosos, e blocos de tratamento de erro ausentes, o mesmo tipo de cuidado já discutido no arquivo sobre [estruturação de scripts](../06-fundamentos-so-unix-shell/06-estruturando-scripts-shell.md).

## Onde o shellcheck se encaixa no fluxo de trabalho

O `shellcheck` pode ser usado de várias formas: diretamente no terminal antes de rodar um script novo, integrado a editores de código através de extensões (disponíveis para VS Code, Sublime Text, Vim, entre outros), ou incorporado a um pipeline de integração contínua, rodando automaticamente a cada mudança de código antes que ela seja aceita. Essa última opção é especialmente valiosa em scripts que fazem parte de uma rotina de manutenção automatizada, como os exemplos vistos no arquivo sobre [integração de scripts e cron](../04-pacotes-scripts-automacao/05-integracao-scripts-cron-manutencao.md), onde um erro não detectado poderia rodar silenciosamente de madrugada, sem ninguém por perto para perceber.

## Fontes

- [Linting and Improving Your Bash Scripts with ShellCheck, Nick Janetakis](https://nickjanetakis.com/blog/linting-and-improving-your-bash-scripts-with-shellcheck)
- [ShellCheck, shell script analysis tool](https://www.shellcheck.net/)
- [ShellCheck and Linting, DevOpsBeast](https://devopsbeast.com/courses/bash-scripting/debugging-shell/shellcheck-linting)
