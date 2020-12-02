# DockerNotes
My DockerNotes

# Instalação

## Remover instalações antigas

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

## Instalando

```bash
# Atualiza pacotes
sudo apt-get update;
# Instala docker
sudo apt-get install docker-ce docker-ce-cli containerd.io;
# Verificar instalação
sudo docker run hello-world;
```

## Iniciar serviço

```bash
/etc/init.d/docker start;
```

## Desinstalando

```bash
# Desinstalando e removendo configurações
sudo apt-get purge docker-ce docker-ce-cli containerd.io;

# Excluindo images, containers e volumes
sudo rm -rf /var/lib/docker;
```


## Criar container

```bash
# Estrutura básica
sudo docker run -i -t <distro>:<versão> <processo_a_executar_ao_iniciar>;

# Exemplo
sudo docker run -i -t ubuntu:14.10 /bin/bash;

# Caso a imagem não esteja localmente, ele faz o download
# Para configurar portas de acesso basta adicionar o parâmetro -p <porta_host>:<porta_container>
```

### Parâmetros para a criação de containers

| Parâmetro  | Descrição  |Exemplo |
|:-:|:-:|:-:|
| `-it`  |  `-t` aloca terminal que permite acesso `-i` interatividade com o terminal|`docker run -it debian`|
|`-v`| cria um diretório compartilhado do host com o container|`-v  /<diretório>`|
|`-w`|Define um diretório padrão ao se conectar via SSH|`-w /home/application`|
|`-p`|Define as configurações de porta em relação gost/container.|`-p <porta_host>:<porta_container>`|
|`--link`|Cria um link entre containers.|`docker run -it --name <nome_do_container> --link <container_id_ou_name>:<alias>;`|
|`--name`|Dá um nome ao container.|`docker run -it --name <nome_do_container> <dist>:<versão>;`|
|`-m`|Define o limite de memória que o container usa do host|`-m 512M`|
|`--cpu-shares`|Define o limite de uso de CPU. Usando proporção entre os containers existentes. Ex. Container A 1024, B 512, C 512 usarão respectivamente 50%, 25% e 25% da CPU host.|`--cpu-shares 512`|
|`-e`|Este parâmetro é utilizado quando se necessita passar variáveis de ambiente para o container|`-e POSTGRESQL_USER=docker`|
|`--volumes-from`|Utilizado para importar volume de outro container. |`--volume-from <nome_container>`|

# Comandos Importantes

```bash
# Listar containers em execução
sudo docker ps;

# Listar todos os containers
sudo docker ps -l;

# Listar imagens disponíveis
sudo docker images;

# Excluir imagem
sudo docker image rm <image_id>;
# ou
sudo docker rmi <image_id>;

# Fazer poll de uma imagem do DockerHub
sudo docker poll <imagem_do_hub>;

# Excluir container
sudo docker rm <id>;

# Iniciar container
sudo docker start <id>

# Sair do container e manter rodando
ctrl + p;
ctrl + q;

# Parar container
sudo docker stop <id>;

# Pausar container
sudo docker pause <id>;

# Despausar container
sudo docker unpause <id>;

# Acessar o container
sudo docker attach <id>;

# Verificar alterações de arquivos do container
sudo docker diff <id>;

# Commitar alterações na imagem
sudo docker commit <id> <nome_que_quiser>:<versão_ex: 1.0>;

# Para container
exit;

# Executar comandos no docker direto do host
sudo docker exec <id> <comando>;

# Todas informações do container
sudo docker inspect <id>;

# Estatisticas de uso
sudo docker stats;

# Ver log do container
sudo docker log <id>;

# Listar processos em execução no container
sudo docker top <id>;

# Criar --link entre containers
sudo docker run -it --name <nome_do_container> --link <container_id_ou_name>:<alias>;

# Remove todos os contêineres não usados
docker container prune;

# Remove todas as imagens não usadas
docker image prune;

# Remove todos os volumes não usados
docker volume prune;

# Remove todas as redes não utilizadas
docker network prune;
```

## Docker UPDATE

> O docker update server para modificar o setup de um container em execução

```docker
# Atualizando o limite de memória
docker update -m 256M <id>;
```

# Localização de arquivos importantes
Images, containers, volumes, and networks

```
/var/lib/docker/
```


# Dockerfile

> Criando Dockerfile para automatizar o build do container.

## Observações
* O nome do arquivo é Dockerfile(Com D maiúsculo).
* O comando de build tem que receber o diretório onde o Dockerfile está.

## Parâmetros

```Dockerfile
# Qual imagem será utilizada(primeiro parâmetro)
FROM <distro>:<versão>

# Quem criou o container
MAINTAINER <nome_ou_email>

# Executar comandos
# Para cada execução uma nova camada
RUN <comandos_desejados>

# Enviar arquivo ou diretório do host para o container(inclusive .tar)
ADD <arquivo> <diretório_do_container>

# Parâmetros para o Entrypoint
CMD

# Entripoint
ENTRYPOINT

# Adicionar metadatas (informações)
LABEL <nome>="<conteúdo>"

# Copiar arquivos ou diretório do host para o container(.tar exclusivo)
COPY <arquivo> <diretório_do_container>

# Passar variáveis de ambiente
ENV <nome>="<conteúdo>"

# Porta do container disponível
EXPOSE 80

# Usuário default
USER <nome_do_user>

# Diretório de trabalho padrão
WORKDIR /<nome_do_diretório>

# Criar o volume do container
VOLUME /<diretório>
```

## Exemplo mínimo com ubuntu 20.04 e apache2

```Dockerfile
From ubuntu:20.04
MAINTAINER hemilioaraujo@gmail.com
RUN apt-get update && apt-get install -y apache2 && apt-get clean
```

## Build

```bash
# Sem especificação do nome e tag da imagem
sudo docker build .

# Com especificação do nome e tag da imagem
sudo docker build -t <imagem>:<tag>;
```

# Compartilhando diretórios

## Compartilhamento com host
### Criar container com compartilhamento simples
>`sudo docker run -ti -v /<nome_diretório> <imagem>;`

Desta forma, o diretório pode ser encontrado via comando inspect: 
>`sudo docker inspect -f {{.Mounts}} <id_container>;`

### Criar container com compartilhamento definindo diretórios
>`docker run -ti -v /<diretório_host>:/<volume_container> <imagem>;`

### Criar container com compartilhamento de outro container
>`docker run --name <nome_container> --volumes-from <container_volume_compartilhado> <imagem>;`



<!-- 
# Criando container postgresql

docker run -d -p 5434:5432 --name pgsql2 --volumes-from dbdados -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql
 -->

<!-- 
 # CONTAINER APACHE2 DEBIAN

FROM debian
RUN apt-get update && apt-get install -y apache2 && apt-get clean
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

LABEL Description="Webserver"

VOLUME /var/www/html

EXPOSE 80

CMD ["apache2ctl","-D","FOREGROUND"] 
-->


# Referências

* [LINUXtips](https://www.youtube.com/playlist?list=PLf-O3X2-mxDk1MnJsejJwqcrDC5kDtXEb) - Playlist Descomplicando Docker

* [LINUXtips](https://www.youtube.com/playlist?list=PLf-O3X2-mxDkiUH0r_BadgtELJ_qyrFJ) - Curso Descomplicando Docker V1

* [devwikiblog](https://www.devwikiblog.com/posts/3/utilizando-docker-em-ambiente-de-desenvolvimento) - Utilizando Docker em ambiente de desenvolvimento
