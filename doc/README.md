# Docker

## Sobre o Docker

???

## Comandos Docker

```zsh
$ docker ps
```

> Listar containers ativos

<br/>

```zsh
$ docker ps -q
```

> Listar ids dos containers ativos

<br/>

```zsh
$ docker run <nome da imagem>
$docker run hello-world
```

> Rodar um container

<br/>

```zsh
$ docker run --rm <nome da imagem>
```

> Rodar um container que será removido automaticamente ao ser parado

<br/>

```zsh
$ docker run -p <porta na sua maquina>:<porta do container> <nome da imagem>

$ docker run -p 8080:80 hello-world
```

> Rodar um container definindo as portas de acesso

<br/>

```zsh
$ docker run -d <nome da imagem>
```

> Rodar um container e liberar o terminal

<br/>

```zsh
$ docker ps -a
```

> Listar containers ativos e parados

<br/>

```zsh
$ docker ps -a -q
```

> Listar ids dos containers ativos e parados

<br/>

```zsh
$ docker attach <hash | name>
```

> Lava para o container

<br/>

```zsh
$ docker start <hash | name>
```

> Inicia um container

<br/>

```zsh
$ docker stop <hash | name>
```

> Para um container

<br/>

```zsh
$ docker rm <hash | name>
```

> Remove um container parado

<br/>

```zsh
$ docker rm $(docker ps -a -q) -f
```

> Remove todos os containers, ativos ou inativos

<br/>

```zsh
$ docker rm -f <hash | name>
```

> Remove containers ativos

<br/>

```zsh
$ docker run --name <nome do container> <nome da imagem>
```

> Define um nome para o container

<br/>

```zsh
$ docker run -it <hash | name> bash
```

> Rodar um container e executar o bash

<br/>

```zsh
$ docker exec <hash | name>
```

> Executa um comando no container

<br/>

```zsh
$ docker exec -it <hash | name> bash
```

> Executa o bash no container

<br/>

```zsh
$ docker run -d -v "$(pwd)":<caminho da imagem> <nome na imagem>
```

> Monta um bind da sua pasta com a parta da imagem do container

<br/>

```zsh
$ docker run -d --mount type=bind,source="$(pwd)",target=<caminho na imagem> <nome da imagem>
```

> Monta um bind da sua pasta com a parta da imagem do container porem é mais indicado do que o -v

<br/>

```zsh
$ docker volume
```

> Comandos do volume

<br/>

```zsh
$ docker volume create <nome do volume>
```

> Cria um volume local

<br/>

```zsh
$ docker volume inspect <nome do volume>
```

> Exibe informaões do volume

<br/>

```zsh
$ docker volume prune
```

> Remove os ecessos dos volumes

<br/>

```zsh
$ docker run -d --mount type=volume,source=<nome do volume>,target=<caminho na imagem> <nome da imagem>
```

> Cria um container utilizando um volume local

<br/>

```zsh
$ docker run -d -v <nome do volume>:<caminho na imagem> <nome da imagem>
```

> Cria um container utilizando um volume local

<br/>

```zsh
$ docker images
```

> Lista de imagens baixadas

<br/>

```zsh
$ docker rmi <nome da imagem>
```

> Remove uma imagem

<br/>

```zsh
$ docker build -t <usuario do docker-hub>/<nome dado por você para a imagem>:latest <local do dockerfiler local>

$ docker build -t moniqueaz/<nome dado por você para a imagem>:latest .
```

> Criar uma imagem local

<br/>

```zsh
$ docker logs nginx

$ docker logs <nome do container>
```

> Exibe o log do container

<br/>
<br/>

## Dockerfile

### Sobre o dockerfile

???

Exemplo:

```Dockerfile
FROM nginx:latest

USER moniqueaz

WORKDIR  /app
RUN apt-get update && \
    apt-get install vim -y
COPY html/ /usr/share/nginx/html
```

FROM - Informa a imagem \
USER - Nome do usuario \
WORKDIR - Diretorio que será criado no container e para onde vc serã direcionado. \
RUN - Os comandos que serão executados \
COPY - Você pode copiar arquivos locais para o container \
CMD - Comando variado \
ENTRYPOINT - Comando fixo \
EXPOSE - Define a porta do container \

<br/>

### Update Docker-Hub

<br/>

```zsh
$ docker login
```

> Para iniciar o processo de login

<br/>

```zsh
$ docker push <nome da imagem>
```

> Para subir a sua imgem para o seu docker-hub

<br/>

```zsh
$ docker logout
```

> Para deslogar

<br/>
<br/>

## Network

### Sobre network no docker

???

<br/>

Tipos de Networks:

Bridge - Geralmente usado para comunicação ente containers. \
Host - Permite a comunica"ao entre o container e o host de forma direto, eles estaram na mesma rede. \
Overlay - Permite a comunicação entre containers em maquinas diferentes de farma que parece que estão na mesma rede. \
Macla - Você consegue definir um endereço mac no seu container de forma que parece ser uma maquina plugada na sua rede. \
None - Define que o container irá rodar de forma isolada. \

<br/>

```zsh
$ docker network

Usage:	docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.
```

> Exibe os comandos do atrelados ao network

<br/>

```zsh
$ docker network ls
```

> Lista as redes do docker

<br/>

```zsh
$ docker network prune
```

> Remove os networks que não estão sendo utilizados.

<br/>

```zsh
$ docker network inspect <tipo da network>
```

> Irá printa informações da network no terminal

<br/>

```zsh
$ docker network create --diver <tipo da network> <nome da network>
```

> Irá printa informações da network no terminal

<br/>

```zsh
$ docker run -itd --name <nome do container> --network <nome do network> <nome da imagem>
```

> Irá rodar um container em um network definido

<br/>

```zsh
$ docker network connect <nome da network> <nome do container>
```

> Irá conectar um container a um network definido

<br/>

Acesse de dentro do container bash

```zsh
root@123534:/# culr http://host.docker.internal:<porta>
```

> Você irá acessar um servidor no host a partir do seu container

<br/>

## Volume

Criando um container node com compartilhamento de volume.

```zsh
docker run --rm -it -v $(pwd)/:/usr/src/app -p 3000:3000 node:15 bash
```

<br/>

### Como disponibilizar esse volume?

Criando um Dockerfile

```Dockerfile
FROM node:15

WORKDIR /urs/src/app

COPY . .

EXPOSE 3000

CMD [ "node", "index.js" ]
```

<br/>

Apois a criação do Dockerfile, execute o comando de criação de uma imagem

```zsh
$ docker build -t <nome do seu usuario no docker-hub>/<nome da sua imagem> .
```

<br/>

Ele irá começar a rodar o build. \
Então ao executar o comando abaixo, ele irá executar corretamente a imagem criada.

```zsh
$ docker run -p 3000:3000 <nome da image>
```

<br/>
Um ponto importante é que quando se trabalha com volume, se trabalha com 2 Dockerfiles,
um para prod e outro para desenvolvimento.

A unica diferena entre os 2 arquivos é o .prod no final do Dockerfile de produção `Dockerfile.prod`,
e que no caso do Dockerfile de desenvolvimento você realmente executa os comandos ao invez de realizar o `COPY . .`.

Um ponto importante é que para buildar o `Dockerfile.prod` você executa o seguinte comando.

```zsh
$ docker build-t <nome do seu usuario no docker-hub>/<nome da sua imagem> . -f Dockerfile.prod
```

> Lembrando que para executar esse comando da forma exibida é preciso estar dentro da pasta onde se encontra o Dockerfile, caso contrario é preciso passar o caminho correto no lugar do . e do Dockerfile.prod, Ex.: /node -f /nodeDockerfile.prod.
> <br/>

<br/>
<br/>

---

<br/><br/>

## Docker Commands

```zsh
Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/home/monique/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/home/monique/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/home/monique/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/home/monique/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  engine      Manage the docker engine
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```

---

## Multi-stage builds

???

### Gerando build

```zsh
$ docker build -t moniqueaz/laravel:prod . -f Dockerfile.prod
$ docker build -t <usuario do docker-hub>/<nome da imagem>:<tag> <local> -f <arquivo a ser processado>
```

> Toda vez que voce mudar o nome padrao do docker file, vc tera que usar o -f

```zsh
$ docker images | grep laravel
```

> Busca todas as imagens com laravel no nome

---

## Passo a Passo

Apos criar um Dockerfile, voce preciar gerar o build desse arquivo, com o build criado, vamos criar o network para ajudar na comunicacao dos containers, apos a criacao do network rode a imagem criada apontando para o network criado
