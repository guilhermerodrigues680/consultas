---
layout: default
title: Aplicação Spring Boot no Repl.it
parent: Repl.it
---

# Aplicação Spring Boot no Repl.it - API Rest + Static Server

Template no Repl.it: [repl.it/@GuilhermeRodri8/spring-boot-application](https://repl.it/@GuilhermeRodri8/spring-boot-application)

---

## Configurando projeto

### Criando o projeto utilizando o Spring Initializr

- Crie um novo Repl e escolha a linguagem `Java`
- Apos criado o Repl.it vá para o console
- No console do repl use o comando abaixo para gerar o projeto do Spring Boot usando a command-line do Spring Initializr (https://docs.spring.io/initializr/docs/current/reference/html/#command-line)

  ```sh
  # Gere o projeto do Spring Boot usando a command-line do Spring Initializr
  curl https://start.spring.io/starter.zip \
    -d type=maven-project \
    -d bootVersion=2.3.2 \
    -d javaVersion=8 \
    -d dependencies=web \
    -o my-project.zip
  ```

- Ou se preferir, gere o projeto usando o [start.spring.io/](https://start.spring.io/) e import no repl o zip gerado.
- No console do repl, use o comando abaixo para descompactar o projeto:
  ```sh
  # Descompacte o arquivo .zip
  unzip my-project.zip
  ```
- Após descompactado o projeto, pode-se deletar o arquivo `.zip`

---

### Configurando o Repl.it para iniciar a nossa Aplicação Spring Boot

Nessa etapa vamos configurar a ação do botão `RUN ▶` do repl, vamos configura-lo para quando clicado executar o Maven Wrapper que vem junto com projeto gerado pelo Spring Initializr.

Crie um arquivo na raiz do repl chamado `.replit` e insire nele:

```toml
language = "java"
run = "mvnw spring-boot:run"
```

Em seguida use do console do repl para remover o arquivo `Main.java` criado pelo repl

```sh
# Remova o arquivo Main.java
rm -f Main.java
```

Clique no botão `RUN ▶` para executar nossa Aplicação Spring Boot, automaticamente o Repl.it já abrirá um **web view** acessando nossa aplicação. O formato da url para acessar a aplicação é: `https://<repl-name>.<user-name>.repl.co` ou `https://<repl-name>--<user-name>.repl.co/`

Aparecerá a mensagem:

```txt
Whitelabel Error Page

This application has no explicit mapping for /error, so you are seeing this as a fallback.
```

Isso acontece porque nossa aplicação não possui nenhuma rota implementada e nem arquivos estáticos.

Use o botão `STOP ■` para parar a aplicação.

---

### Criando um web controller para a aplicação

- Seguindo o modelo do guia oficial [Building an Application with Spring Boot](https://spring.io/guides/gs/spring-boot/), vamos criar uma classe `HelloController` para a raiz `/ `da aplicação.
- Crie o arquivo `src/main/java/com/example/demo/HelloController.java` e insira nele:

```java
// src/main/java/com/example/demo/HelloController.java

package com.example.demo;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {

	@RequestMapping("/")
	public String index() {
		return "Saudações do Spring Boot!";
	}

}
```

Clique no botão `RUN ▶` para executar nossa aplicação e em seguida de um **Refresh** no **web view**, observe que ela respondeu com a String que configuramos `"Saudações do Spring Boot!"`;

---

## Dicas

### Arquivos estáticos

- Arquivos estáticos devem ser colocados na pasta `src/main/resources/static/`
- Eles serão servido a partir da raiz do APP.
- Ex: `src/main/resources/static/about.html` será `https://<nome-do-repl>.<usuario>.repl.co/about.html`
- Ex: `src/main/resources/static/js/app.js` será `https://<nome-do-repl>.<usuario>.repl.co/js/app.js`

### Atalhos

- Pressione Ctrl + C no terminal para parar a aplicação.
- Use o comando `python3 -m http.server` para iniciar um servidor com os arquivos do diretório atual usando Python e HTTP

### Gerando o arquivo jar da aplicação

- Use o comando `./mvnw clean package` ou `./mvnw package` para compilar o projeto
- Faça o download do zip do projeto pelo menu Files do repl.
- Em seu computador, extraia o zip e acesse a pasta target. O arquivo `demo-0.0.1-SNAPSHOT.jar` é o projeto compilado.
- Ou se preferir, no console do repl.it use o comando `python3 -m http.server` para baixar somente o projeto compilado.
- Use o comando `java -jar target/demo-0.0.1-SNAPSHOT.jar` para executar o jar da aplicação.

---

## Implementando uma página estática e uma API na aplicação

### Implementando API

- Crie a pasta `src/main/java/com/example/demo/controllers/`
- Se existir o arquivo `src/main/java/com/example/demo/HelloController.java` remova ele.
- Crie o arquivo `src/main/java/com/example/demo/controllers/HelloController.java` e insira nele:

```java
// src/main/java/com/example/demo/controllers/HelloController.java

package com.example.demo.controllers;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {

	@RequestMapping("/api/hello")
	public String hello() {
		return "Saudações do Spring Boot!";
	}

}
```

### Criando página estática

- Crie um arquivo `index.html` dentro de `src/main/resources/static/`
- Crie uma pasta chamada `js` dentro de `src/main/resources/static/`
- Crie um arquivo `app.js` dentro de `src/main/resources/static/js/`

Preencha o `index.html` com:

```html
<!-- src/main/resources/static/index.html -->

<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      text-align: center;
      font-family: Arial, Helvetica, sans-serif;
      font-size: smaller;
    }
  </style>
  <title>Aplicação Spring Boot no Repl.it</title>
</head>
<body>
  <h1>Aplicação Spring Boot no Repl.it - API Rest + Static Server</h1>
  <p><a href="/api/hello">/api/hello</a></p>

  <script src="js/app.js"></script>
</body>
</html>
```

Preencha o `app.js` com:

```js
// src/main/resources/static/js/app.js

const sucessMsg = document.createElement('h1');
sucessMsg.innerHTML = "app.js carregado!";
document.body.appendChild(sucessMsg);

const sucessMsgApi = document.createElement('h1');
sucessMsgApi.innerHTML = "api res: ";

fetch('/api/hello')
.then(res => res.text())
.then(res => sucessMsgApi.innerHTML += res)
.catch(err => sucessMsgApi.innerHTML += err)

document.body.appendChild(sucessMsgApi);
```
