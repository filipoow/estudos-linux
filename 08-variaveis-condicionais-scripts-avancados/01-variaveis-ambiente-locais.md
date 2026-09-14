# Variáveis de ambiente e variáveis locais

## Dois escopos diferentes para guardar valores

O arquivo sobre [scripts em shell](../04-pacotes-scripts-automacao/03-scripts-shell.md) já mostrou o básico de uma variável, atribuir um valor e ler esse valor com `$`. O que ainda não foi explicado é que existem dois tipos de variável no shell, com escopos bem diferentes entre si: variáveis locais, que só existem dentro da sessão de shell atual, e variáveis de ambiente, que são repassadas para qualquer programa que essa sessão venha a executar.

## Variável local: só vale aqui

Por padrão, toda variável criada num script ou terminal é local:

```bash
mensagem="ola"
echo $mensagem
```

Essa variável existe enquanto aquele shell estiver rodando, mas se esse script chamar outro programa, esse programa não vai enxergar `mensagem`, mesmo que os dois estejam rodando um dentro do outro. A variável simplesmente não é repassada adiante.

## Variável de ambiente: repassada para os filhos

Para que um valor seja repassado a qualquer programa executado a partir daquele shell, é preciso exportá-lo com o comando `export`:

```bash
export CAMINHO_APP="/opt/app"
```

A partir desse ponto, `CAMINHO_APP` está disponível não só no shell atual, mas em qualquer processo filho iniciado por ele, incluindo outros scripts chamados de dentro deste, ou programas externos que leem variáveis de ambiente para se configurar. Variáveis de ambiente já conhecidas do sistema, como `PATH` (mencionada de passagem no arquivo sobre o [shell como interpretador de comandos](../06-fundamentos-so-unix-shell/04-shell-interpretador-comandos.md)) ou `HOME`, seguem exatamente essa mesma lógica.

Para ver todas as variáveis de ambiente ativas numa sessão, existe o comando `printenv` (ou `env`), e para remover uma variável, local ou exportada, usa-se `unset`.

## Por que a convenção de maiúsculas existe

Existe uma convenção amplamente seguida, embora não seja uma regra obrigatória do shell: variáveis de ambiente costumam usar letras maiúsculas com palavras separadas por underline, como `CAMINHO_APP` ou `PATH`, enquanto variáveis locais de um script costumam usar minúsculas, como `mensagem` ou `contador`. Essa convenção não é decorativa, ela ajuda a distinguir rapidamente, só de olhar o nome, o que é uma configuração de escopo amplo (potencialmente vinda de fora do script, ou usada por outros programas) do que é um detalhe interno daquele script específico, reduzindo o risco de sobrescrever por acidente uma variável de ambiente importante do sistema.

## Onde isso importa na prática

Essa distinção fica evidente todas as vezes em que um script precisa se comunicar com um programa externo que ele mesmo iniciou, como no exemplo de container Docker apresentado no arquivo sobre [Docker e Golang](../07-processamento-texto-e-automacao/08-docker-golang-processamento-logs.md): configurações passadas via variáveis de ambiente exportadas são o jeito padrão de configurar aplicações dentro de containers, exatamente porque esse mecanismo de herança entre processos pai e filho funciona de forma previsível e independente da linguagem em que a aplicação foi escrita.

## Fontes

- [export Command in Linux: Set Bash Environment Variables, Linuxize](https://linuxize.com/post/export-command-in-linux/)
- [Guide to Naming Conventions for Shell Variables, Baeldung on Linux](https://www.baeldung.com/linux/shell-variable-naming-conventions)
- [Guide to Unix/Environment Variables, Wikibooks](https://en.wikibooks.org/wiki/Guide_to_Unix/Environment_Variables)
