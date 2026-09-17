# Gerando e estruturando logs: informação versus erro

## `echo` sozinho não é uma estratégia de log

Boa parte dos scripts apresentados neste repositório usou `echo` para mostrar mensagens na tela. Para um script pequeno, isso é suficiente. Para um script de automação real, rodando sem supervisão direta, como os exemplos vistos no arquivo sobre [integração de scripts e cron](../04-pacotes-scripts-automacao/05-integracao-scripts-cron-manutencao.md), essa abordagem simples não escala: todas as mensagens se misturam, sem nenhuma forma de distinguir uma informação de rotina de um erro grave, nem de saber quando exatamente cada mensagem aconteceu.

## Separando canais: stdout para informação, stderr para erro

O arquivo sobre [redirecionamento e pipes](../04-pacotes-scripts-automacao/02-redirecionamento-pipes.md) já apresentou os canais padrão do shell, stdout e stderr. Essa separação é exatamente a base de uma boa estratégia de log: mensagens informativas de rotina deveriam ir para stdout, enquanto avisos e erros deveriam ir especificamente para stderr, usando o redirecionador `>&2`.

```bash
log_info() {
    echo "[INFO] $(date -u +%Y-%m-%dT%H:%M:%SZ) $1"
}

log_erro() {
    echo "[ERRO] $(date -u +%Y-%m-%dT%H:%M:%SZ) $1" >&2
}

log_info "Iniciando backup"
if ! tar -czf backup.tar.gz /dados; then
    log_erro "Falha ao criar o arquivo de backup"
    exit 1
fi
log_info "Backup concluido com sucesso"
```

Essas duas funções reaproveitam o conceito de funções apresentado no arquivo sobre [funções e modularização](../09-funcoes-arrays-e-tratamento-de-erros/01-funcoes-modularizacao.md), e já embutem duas boas práticas de uma vez: um rótulo indicando o nível da mensagem, e um timestamp em formato padronizado (UTC, no formato ISO 8601), o que facilita tanto a leitura humana quanto o processamento automático desses registros depois.

## Por que separar os canais importa na prática

Essa separação não é só estética. Ela permite, por exemplo, redirecionar cada tipo de mensagem para um destino diferente, guardando só os erros num arquivo de alerta, enquanto o restante segue para um log geral:

```
./script_de_backup.sh > log_geral.txt 2> log_erros.txt
```

Também é o que permite que ferramentas de monitoramento e alerta, como o Alertmanager, apresentado no [próximo arquivo](03-prometheus-grafana-alertmanager.md), consigam filtrar e reagir especificamente a mensagens de erro, sem precisar interpretar o conteúdo de cada linha para adivinhar sua gravidade.

## Níveis de log, além de informação e erro

Embora "informacional" e "erro" sejam os dois níveis mais básicos e mais citados, é comum encontrar uma escala um pouco mais rica em scripts mais elaborados: `DEBUG` (detalhes finos, úteis só durante investigação ativa de um problema, na mesma linha do `set -x` já apresentado no arquivo sobre [debug de scripts](../08-variaveis-condicionais-scripts-avancados/08-debug-set-x-permissoes.md)), `INFO` (o funcionamento normal do script), `WARNING` (algo que merece atenção, mas não impede o script de continuar) e `ERROR` (uma falha que compromete o resultado). Adotar essa escala, mesmo que de forma simplificada, ajuda quem lê os registros meses depois a filtrar rapidamente o nível de detalhe que interessa para cada situação, sem precisar reler tudo linha por linha.

## Fontes

- [Bash Script Logging Best Practices, megainterview](https://megainterview.com/bash-script-logging-best-practices/)
- [Structured Logging in Shell Scripting, Picus Security Engineering](https://medium.com/picus-security-engineering/structured-logging-in-shell-scripting-dd657970cd5d)
- [Logging in bash scripts, Graham Watts](https://grahamwatts.co.uk/bash-logging/)
