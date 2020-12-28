---
layout: default
title: Comandos
parent: Shell script
---

# Comandos Shell script
{: .no_toc }

## Conteúdo
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Comprimir e extrair

- [www.tecmint.com/18-tar-command-examples-in-linux](https://www.tecmint.com/18-tar-command-examples-in-linux/)
- [www.rootusers.com/gzip-vs-bzip2-vs-xz-performance-comparison](https://www.rootusers.com/gzip-vs-bzip2-vs-xz-performance-comparison/)

```sh
# Comprimir GZIP
tar -czvf server.log.tar.gz server.log

# Extrair GZIP
tar -xzvf server.log.tar.gz

# Comprimir XZ
tar -cJf server.log.tar.xz server.log

# Extrair XZ
tar -xvf server.log.tar.xz

# Comprimir bz2
tar cvfj server.log.tar.bz2 server.log

# Extrair bz2
tar -xvf server.log.tar.bz2

# Listar conteudo de um tar
tar -tvf <file>

#XZ_OPT=-9 tar -Jcvf file.tar.xz /path/to/directory
```

## Listar e ordenar arquivos

```sh
##### Listar e ordenar por tamanho

df -h | grep "/dev/sda"
du -ah --max-depth=1 | sort -h

# Ordernar por tamanho
ls -S -lah

# Ordernar por data
ls -laht

# Alterar data modificacao arquivo
touch -a -m -t 202011281205.09 vim-depois.png

# Buscar arquivos criados a 1 dia (exec com todos)
find . -type f -mtime -1 -exec echo {} +

# Buscar arquivos criados a 1 dia (exec 1 a 1)
find . -type f -mtime -1 -exec echo {} \;

# Contar linhas arquivo
wc -l <file>
wc -l server.log | cut -f1 -d' '
wc -l < server.log | xargs
echo `wc -l < 5primeiras.txt`
```

## Script comprimir log

```sh
#!/bin/bash
# OBS: No MacOs o sed e diferente -> sed -i "" "1,${LINHAS_LOG}d" $FILE

FILE=server.log

if test -f "$FILE"; then
    echo "$FILE existe."
else
    echo "$FILE nao existe. Encerando script."
    exit 1
fi

LINHAS_LOG=$(wc -l < $FILE | xargs)

echo "O log possui $LINHAS_LOG linhas. E ocupa $(du -h $FILE | cut -f1) de espaco."

if [ $LINHAS_LOG -eq 0 ]; then
    echo "O arquivo de log esta vazio. Encerando script."
    exit 1
fi

echo "Iniciando backup das $LINHAS_LOG linhas do log."
BKP_FILENAME=${FILE}-bkp-$(date +"%Y-%m-%d-%H-%M-%S")
head -$LINHAS_LOG $FILE > $BKP_FILENAME

echo "Compactando backup das $LINHAS_LOG linhas do log."
gzip $BKP_FILENAME

echo "Deletando $LINHAS_LOG linhas do arquivo de log original"
sed -i "1,${LINHAS_LOG}d" $FILE

ls -lah $FILE ${BKP_FILENAME}.gz

echo "FIM do backup!"
echo ""
```
