# DevOps_atividades
Atividades

## 1 - 

## Define o limite de uso de CPU/Lista processos ordenados por uso de CPU/ imprime cabeçalho / imprime processos acima do limite/ espera 5 segundos antes de repetir/ mostra ao usuário a opção de parar o script(Ctr+C).
```
#!/bin/bash

CPU_LIMITE=20

echo "Monitorando processos com uso de CPU acima de $CPU_LIMITE%..."
echo "Pressione Ctrl+C para parar."

while true; do
    echo "----------------------------"
    echo "Horário: $(date)"
    echo "Processos com uso de CPU > $CPU_LIMITE%:"
    
   
    ps -eo pid,comm,%cpu --sort=-%cpu | awk -v limite="$CPU_LIMITE" '
    NR==1 { print $0; next }  
    $3 > limite { print }     
    '

    sleep 5 
done


```
## Bash
## Salve o script como: monitor_cpu.sh
## Dê permissão de execução: chmod +x monitor_cpu.sh
##


## 2 - Desenvolva um script que utiliza comandos como ps e sort para exibir os processos que estão consumindo mais memória.
## exibindo 10 processos.
```
#!/bin/bash


NUM_PROC=10

echo "Exibindo os $NUM_PROC processos que mais consomem memória RAM:"
echo "---------------------------------------------------------------"
echo "PID     %MEM    COMANDO"

# Lista os processos com uso de memória, ordena e exibe os principais
ps -eo pid,%mem,comm --sort=-%mem | head -n $((NUM_PROC + 1))

```
## Salve como top_mem.sh
## Torne executável: chmod +x top_mem.sh
## Execute: ./top_mem.sh
##

## 3 - Crie um script que verifica se um processo específico está em execução e exibe seu status.
# Nome do processo a verificar (pode ser o nome do comando, como "firefox" ou "nginx")
```
PROCESSO="$1"

if [ -z "$PROCESSO" ]; then
    echo "Uso: $0 <nome_do_processo>"
    exit 1
fi

# Verifica se o processo está em execução
PID=$(pgrep -x "$PROCESSO")

if [ -n "$PID" ]; then
    echo "O processo '$PROCESSO' está em execução."
    echo "PID(s): $PID"
    echo "Status detalhado:"
    ps -fp "$PID"
else
    echo "O processo '$PROCESSO' NÃO está em execução."
fi
```
## Salve como verifica_processo.sh
## execute passando o nome do processo como argumento 
## ./verifica processo.sh firefox
##

# 4 - Elabore um script para analisar os logs do sistema em busca de mensagens de erro relacionadas a processos.
```
#!/bin/bash

# Verifica se o usuário informou o nome do processo
PROCESSO="$1"

if [ -z "$PROCESSO" ]; then
    echo "Uso: $0 <nome_do_processo>"
    exit 1
fi


LOGFILE="/var/seulog/syslog"
[ -f "$LOGFILE" ] || LOGFILE="/var/seulog/messages"

if [ ! -f "$LOGFILE" ]; then
    echo "Arquivo de log não encontrado."
    exit 2
fi
echo “Analisando erros relacionados ao processo '$PROCESSO' em $LOGFILE..."

# Filtra mensagens de erro relacionadas ao processo
grep -i "$PROCESSO" "$LOGFILE" | grep -iE "error|fail|fatal|segfault" > /tmp/erros_$PROCESSO.log

if [ -s /tmp/erros_$PROCESSO.log ]; then
    echo " Erros encontrados:"
    cat /tmp/erros_$PROCESSO.log
else
    echo " Nenhum erro encontrado relacionado ao processo '$PROCESSO'."
fi



```
## Salve o script como analisa_erros.sh
## Execute com o nome do processo: ./analisa_erros.sh apache2

# 5 - Crie um script para monitorar as mensagens de erro no log do sistema em intervalos regulares usando cron jobs. O script deve registrar em um arquivo as últimas 5 linhas de mensagens de erro, possibilitando uma visão periódica da atividade do sistema.
```
#!/bin/bash

LOGFILE="/var/log/syslog"
[ -f "$LOGFILE" ] || LOGFILE="/var/seulog/messages"

# Arquivo onde os erros serão registrados
DESTINO="/var/seulog/erros_monitorados.log"

# Timestamp atual
echo "===== $(date '+%Y-%m-%d %H:%M:%S') =====" >> "$DESTINO"

# Captura as últimas 5 mensagens de erro
grep -iE "error|fail|fatal|segfault" "$LOGFILE" | tail -n 5 >> "$DESTINO"
echo "" >> "$DESTINO"

Salve como monitor_erros.sh

chmod +x monitor_erros.sh

```



# DEPENDÊNCIA
## Sistema Operacional Linux
