# DevOps_atividades
Atividades

# 1 - 

## Define o limite de uso de CPU/Lista processos ordenados por uso de CPU/ imprime cabeçalho / imprime processos acima do limite/ espera 5 segundos antes de repetir
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


# 2 - Desenvolva um script que utiliza comandos como ps e sort para exibir os processos que estão consumindo mais memória.
# 3 - Crie um script que verifica se um processo específico está em execução e exibe seu status.
# 4 - Elabore um script para analisar os logs do sistema em busca de mensagens de erro relacionadas a processos.
# 5 - Crie um script para monitorar as mensagens de erro no log do sistema em intervalos regulares usando cron jobs. O script deve registrar em um arquivo as últimas 5 linhas de mensagens de erro, possibilitando uma visão periódica da atividade do sistema.


# DEPENDÊNCIA
## Sistema Operacional Linux
