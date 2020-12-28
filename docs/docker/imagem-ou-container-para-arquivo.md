---
layout: default
title: Imagem ou container para arquivo
parent: Docker
---

# Imagem ou container para arquivo
{: .no_toc }

## Conteúdo
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Imagem do docker para arquivo

```sh
docker save heroku-in-docker:latest | gzip > heroku-in-docker_latest.tar.gz
docker save heroku-in-docker:latest > heroku-in-docker_latest.tar

docker load < heroku-in-docker_latest.tar.gz
docker load < heroku-in-docker_latest.tar
```

## Container para imagem

```sh
docker commit abcfe818adf0 debug/alpine:latest
```

## Carregar container

```sh
cat exampleimage.tgz | docker import - exampleimagelocal:new
docker import container.tar 
```
