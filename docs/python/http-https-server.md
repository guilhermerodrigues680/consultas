---
layout: default
title: HTTP e HTTPS Server
parent: Python
---

# HTTP e HTTPS Server com Python
{: .no_toc }

## Conteúdo
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## HTTP Server

```sh
# http server Python 2
python -m SimpleHTTPServer

# http sever Python 3
python3 -m http.server
```

## HTTPS Server Python 2.7

Crie um arquivo chamado `server-https-py27.py` com o código abaixo:

```py
# Python 2.7
# taken from https://www.piware.de/2011/01/creating-an-https-server-in-python/
# generate server.pem with the following command:
#    openssl req -new -x509 -keyout server.pem -out server.pem -days 365 -nodes
# run as follows:
#    python server-https-py27.py
# then in your browser, visit:
#    https://localhost:4443

import BaseHTTPServer, SimpleHTTPServer
import ssl

httpd = BaseHTTPServer.HTTPServer(('localhost', 4443), SimpleHTTPServer.SimpleHTTPRequestHandler)
httpd.socket = ssl.wrap_socket (httpd.socket, certfile='server.pem', server_side=True)
httpd.serve_forever()
```

```sh
# https server Python 2
python server-https-py27.py
```


## HTTPS Server Python 3

Crie um arquivo chamado `server-https-py3x.py` com o código abaixo:

```py
# Python 3
# taken from https://piware.de/2011/01/creating-an-https-server-in-python/
# generate server.pem with the following command:
#    openssl req -new -x509 -keyout server.pem -out server.pem -days 365 -nodes
# run as follows:
#    python3 server-https-py3x.py
# then in your browser, visit:
#    https://localhost:4443

from http.server import HTTPServer, SimpleHTTPRequestHandler
import ssl

httpd = HTTPServer(('localhost', 5443), SimpleHTTPRequestHandler)
httpd.socket = ssl.wrap_socket(httpd.socket, certfile='server.pem', server_side=True)
httpd.serve_forever()
```

```sh
# https sever Python 3
python3 server-https-py3x.py
```
