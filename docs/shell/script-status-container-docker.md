---
layout: default
title: Script status container docker
parent: Shell script
---

# Script status container docker

Script para monitorar um container do docker

_OBS: Ajustar script antes de executar e usar o código do GitHub devido Liquid tentar renderizar o script_
{: .text-red-300}

---

```sh
#!/bin/bash
## Source container config file

## ASCII codes
red='\e[01;31m'
green='\e[01;32m'
yellow='\e[01;33m'
no_color='\e[0m'
beep='\007'

## Update frequency in seconds
frequency=1

printf "\n  %b\n\n" "Status sendo atualizado a cada ${frequency} segundo(s)."

## The code uses the strategy of writing and returning the carriage,
## so the status of the container is updated without printing new lines.
while [ true ]
do
    #status=`/usr/bin/docker container ps -a --format '{{.Image}},{{.Status}}'|grep ${name}|/usr/bin/cut -d',' -f2|/usr/bin/cut -d' ' -f1`
    status="ok"
    
    # Checks that the string is not empty and the status obtained is 'Up'
    if [[ -n "$status" && "$status" =  "Up" ]]; then
        printf "%-5s%b%70s \r" "" "Status - ${green}$status${no_color}" ""
    
    # else if, check if the string is empty
    elif [ -z "$status" ]; then
        printf "%-5s%b%70s \r" "" "Status - ${yellow}Sem registros de execução ou parada${no_color}" ""

    # else, print the status obtained
    else
        printf "%-5s%b%70s%b \r" "" "Status - ${red}$status${no_color}" "" $beep
    fi
    
    printf "\u001b[35m [\]\u001b[0m\r"
    sleep 0.1
    printf "\u001b[36m [|]\u001b[0m\r"
    sleep 0.1
    printf "\u001b[35m [/]\u001b[0m\r"
    sleep 0.1
    printf "\u001b[36m [-]\u001b[0m\r"
    #sleep 1

    sleep $frequency
done

```