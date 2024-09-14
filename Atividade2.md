## Atividade 2

### 1-Introdução

#Criar um contêiner Docker com um servidor Web Python.

Pré-requisitos:

-Docker instalado;

-Python v3.x.

### 2-Construção do servidor Web

Criar uma pasta para colocar todos os arquivos.
```bash
mkdir "nome da pasta"
```
#Criar um arquivo index.html:
```bash
nano index.html
```
#Dentro do arquivo coleque:
```bash
<html>
   <body>
      <h1>Hello Docker</h1>
   </body>
</html>
```

### 3-Criar o servidor Web Python

#Na pasta utilizada, criar o arquivo webserver.py:
```bash
nano webserver.py
```
#Dentro do arquivo "webserver.py", coloque o seguinte:
```bash
#!/usr/bin/python3
from http.server import BaseHTTPRequestHandler, HTTPServer
import time
import json
from socketserver import ThreadingMixIn 
import threading


hostName = "0.0.0.0"
serverPort = 80


class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        # curl http://<ServerIP>/index.html
        if self.path == "/":
            # Respond with the file contents.
            self.send_response(200)
            self.send_header("Content-type", "text/html")
            self.end_headers()
            content = open('index.html', 'rb').read()
            self.wfile.write(content)
        else:
            self.send_response(404)
            return


class ThreadedHTTPServer(ThreadingMixIn, HTTPServer):
    pass


if __name__ == "__main__":
    webServer = ThreadedHTTPServer((hostName, serverPort), Handler)
    print("Server started http://%s:%s" % (hostName, serverPort))


    try:
        webServer.serve_forever()
    except KeyboardInterrupt:
        pass


    webServer.server_close()
    print("Server stopped.")
```

### 4-Executar em um Docker container

#Criar um Dockerfile:
```bash
FROM python:3
ADD index.html index.html
ADD server.py server.py
EXPOSE 8888
ENTRYPOINT ["python3", "server.py"]
```
#Executar os comandos:
```bash
docker build -f Dockerfile . -t web-server-test
```
```bash
docker run — rm -p 8000:80 — name web-server-test web-server-test
```
#Agora acesse http://localhost:8000 no navegador

## Conclusão 
#Esta atividade proporcionou uma introdução prática ao uso do Docker para criar e executar aplicações web. A experiência adquirida inclui a configuração de um ambiente básico de servidor web, a criação de um Dockerfile para empacotar a aplicação, e o uso de comandos Docker para construir e rodar containers. Esse conhecimento é fundamental para quem deseja explorar a construção e o gerenciamento de aplicações containerizadas.
