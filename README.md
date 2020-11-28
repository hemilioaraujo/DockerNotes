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

# Excluir container
sudo docker rm <id>;

# Iniciar container
sudo docker start <id>

# Sair do container e manter rodando
ctrl + p;
ctrl + q;

# Parar container
sudo docker stop <id>;

# Acessar o container
sudo docker attach <id>;

# Verificar alterações de arquivos do container
sudo docker diff <id>;

# Commitar alterações
sudo docker commit <id>; <nome_que_quiser>:<versão_ex: 1.0>;

# Para container
exit;

# Executar comandos no docker direto do host
sudo docker exec <id> <comando>;

# Todas informações do container
sudo docker inspect <id>;

# Estatisticas de uso
sudo docker stats;

# Criar --link entre containers
sudo docker run -it --name <nome_do_container> --link <container_id_ou_name>:<alias>;

```

# Localização de arquivos importantes
Images, containers, volumes, and networks

```
/var/lib/docker/
```
