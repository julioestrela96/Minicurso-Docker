## Atividade 3

*Construindo e Conectando um Ambiente de Aplicação Web Completo.

*Nessa atividade você irá construir um ambiente completo de aplicação web utilizando Docker. O ambiente incluirá um servidor web Nginx, um banco de dados PostgreSQL e uma aplicação web simples. Você configurará e conectará todos os serviços para criar uma aplicação funcional.

### 1-Preparar o Ambiente

#Crie um diretório de projeto chamado docker-web-app.

#Dentro deste diretório, crie os seguintes subdiretórios: postgres, app, nginx.

### 2-Configuração do PostgreSQL

#Dentro do diretório postgres, crie um arquivo init.sql para inicializar o banco de dados com uma tabela simples.

#init.sql:
```bash
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

INSERT INTO users (name) VALUES ('Alice'), ('Bob'), ('Charlie');
```
#Crie um arquivo Dockerfile para configurar a inicialização do banco de dados.
```bash
FROM postgres:latest
ENV POSTGRES_DB=mydatabase
ENV POSTGRES_USER=myuser
ENV POSTGRES_PASSWORD=mypassword
COPY init.sql /docker-entrypoint-initdb.d/
```
#Construa a imagem personalizada do PostgreSQL (comando `docker build`)

### 3-Configuração da Aplicação Web
#No diretório app, crie uma aplicação Flask simples que se conecta ao PostgreSQL

#app.py:
```bash
from flask import Flask, jsonify
from flask_sqlalchemy import SQLAlchemy
import os

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://myuser:mypassword@postgres:5432/mydatabase'
db = SQLAlchemy(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(80), nullable=False)

@app.route('/')
def index():
    return "Bem-vindo à minha aplicação web!"

@app.route('/users')
def get_users():
    users = User.query.all()
    return jsonify([u.name for u in users])

if __name__ == '__main__':
    app.run(host='0.0.0.0')
```
#Crie um arquivo requirements.txt para as dependências do Python.

#requirements.txt:
```bash
Flask
Flask-SQLAlchemy
psycopg2-binary
```
#Crie um arquivo Dockerfile para configurar a aplicação Flask.

#Dockerfile:
```bash
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```
#Construa a imagem da aplicação (comando `docker build`)

### 4-Configuração do Nginx

#Dentro do diretório nginx, crie um arquivo nginx.conf para personalizar a configuração do Nginx.

#nginx.conf:
```bash
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://app:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

#Crie um arquivo Dockerfile para configurar o Nginx.

#Dockerfile:
```bash
FROM nginx:latest
COPY nginx.conf /etc/nginx/conf.d/default.conf
```
#Construa a imagem docker do seu nginx (comando `docker build`)

*OBS: Outra forma de copiar o nginx.conf da sua máquina local para o container do docker, seria usar a opção -v no momento da criação do container.
Exemplo:  `docker run --name meu-nginx -v /home/seuusuario/nginx/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx`

### 5-Configuração do Docker-compose

#No diretório raiz docker-web-app, crie um arquivo docker-compose.yml para orquestrar todos os serviços.

#docker-compose.yml:
```bash
version: '3'
services:
  nginx:
    build: ./nginx
    ports:
      - "8080:80"
    networks:
      - webnet

  app:
    build: ./app
    depends_on:
      - postgres
    networks:
      - webnet

  postgres:
    build: ./postgres
    environment:
      POSTGRES_PASSWORD: mypassword
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - webnet

volumes:
  pgdata:

networks:
  webnet:
```

### 6-Executando a aplicação

#Após configurar tudo, execute `docker-compose up -d` para iniciar os containers.

#Verifique se a aplicação web está acessível em [http://localhost:8080](http://localhost:8080/).

## Conclusão
#Essa atividade proporcionou uma visão abrangente sobre como construir e conectar múltiplos serviços usando Docker. Desde a criação de imagens Docker personalizadas até a configuração de um ambiente de aplicação web completo com Docker Compose, a atividade destacou a importância de uma abordagem modular e a capacidade de orquestrar diversos serviços. Essa prática é fundamental para o desenvolvimento e a gestão de aplicações complexas em ambientes de produção.
