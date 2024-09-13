# Minicurso-Docker

*Docker-compose-install

1-Baixar o docker-compose versão 2.29.2
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
2-Dar permissão de execução ao Docker Compose
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
3-Verificar se a instalação foi bem-sucedida
```bash
docker-compose --version
```

## Atividade 1

*Introdução ao Docker

### 1-Verifique a Instalação

#Rode o seguinte comando para verificar se o Docker está funcionando corretamente:
```bash
docker --version
```
*Trabalhando com Containers

### 2-Rodar um Container

#Use a imagem oficial do Docker para o Ubuntu e execute um container interativo. O comando a seguir irá baixar a imagem do Ubuntu, se ela não estiver presente, e iniciar um container:
```bash
docker run -it ubuntu
```
#Dentro do container, rode alguns comandos básicos como `ls`, `pwd`, e `cat` `/etc/os-release.`
Para sair do container, digite `exit`.

### 3-Listar Containers em Execução

#Verifique os containers ativos:
```bash
docker ps
```
#Verifique todos os containers (ativos e inativos):
```bash
docker ps -a
```

### 4-Remover um Container

#Para remover um container, primeiro anote o ID ou nome do container (usando docker ps -a) e remova-o:
```bash
docker rm <container_id>
```
#Para remover uma imagem que não está sendo usada:
```bash
docker rmi ubuntu
```

### 5-Criando sua Própria Imagem Docker

#Crie uma pasta chamada meu_primeiro_docker: 
```bash
mkdir meu_primeiro_docker
```
#crie um arquivo chamado Dockerfile
```bash
nano Dockerfile
```
#Dentro do Dockerfile coloque o seguinte:
```bash
# Usar imagem base do Ubuntu
FROM ubuntu

# Atualizar pacotes e instalar o curl
RUN apt-get update && apt-get install -y curl

# Definir o comando padrão para o container
CMD ["echo", "Olá, Docker!"]
```
#obs: use ctrl + x para salvar e enter para finalizar 

### 6-Construir a Imagem

#Navegue até a pasta onde está o Dockerfile e rode o comando para construir a imagem:
```bash
docker build -t meu_primeiro_docker .
```
#Rodar sua Imagem. Após construir a imagem, rode o seguinte comando para criar um container a partir dela:
```bash
docker run meu_primeiro_docker
```
## Conclusão

Ao completar essa atividade, você terá aprendido:

1-Instalar e configurar o Docker.

2-Baixar e rodar containers de imagens existentes.

3-Criar e rodar sua própria imagem Docker.

4-Gerenciar containers e volumes.
