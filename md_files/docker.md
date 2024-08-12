# Docker na prática com Python

- URL: https://www.youtube.com/watch?v=VnOWvqKzJNk&list=PLAgbpJQADBGIDbMSopaqFnGm7GJnwru0-&index=1&pp=iAQB

## Aula 2
- Instalar pre-commit para realizar tarefas todas vez que dou um commit no git?
- posso criar um arquivo .sh com uma lista de comandos que devem rodar, em função da tarefa estou querendo.
Isso pode ser passado como exemplo para instalar um novo ambiente;

## Aula 3
- O dockerhub armazena imagens de aplicações
- `docker images` mostra as imagens que tenho em meu pc local
- Posso criar vários containers de uma mesma imagem.
- `docker run <imagem>` cria um container
- `docker ps -a` lista os containers ativos
- `docker pull <imagem>` baixa a imagem do dockerhub
- .dockerignore e Dockerfile
- `docker build --tag <image_name>` cria a build de uma imagem
- `docker start <ID>` roda um container em estado inativo
- `docker exec -it <ID_container> bash` entro dentro do meu container. A opçãp `-it` significa iterativo e permite passar uma opção a ser rodada dentro do container. Neste caso, a opção a ser rodada será o bash.
É possível interagir com o repositório normalmente.
- `exit` sai do container

## Aula 4
- `docker inspect <ID_container>` mostra as configurações do ambiente do container rodando
- É possível criar uma porta em meu container que permite acessá-lo através do IP da máquina host
- `docker run -p 2300:5000 -d <image_name>` a opção -p pareia a porta local 2300 com a porta do container 5000. A opção -d libera a libera a linha de comando do terminal (assim como o &)
- `docker stop <ID_container>` para o container
- Em bando de dados, existe o conceitos de volumes que guarda toda a configuração de um container. Se este container for parado, é possível rodar um novo e recuperar toda a informação anteiror.
Para mais informações, acesse: https://docs.docker.com/engine/storage/
- `docker volume ls` lista os volumes criados localmente

## Aula 5
- Pode-se criar uma conexão dedicada entre containers. Para isso, posso criar uma conexão através do comando `docker create <network_name>`. Esta conexão pode ser passada na hora de rodar o container. Com isso, pode esconder este cotainer dentro do docker e fazer o acesso por um outro container de interface.


## Aula 6
- Docker Composer permite comporto todo o ambiente de containers a serem rodados à partir de um arquivo.
Este arquivo é chamado docker-compose.yml. Este arquivo possui uma série de variáveis especificando como montar o docker environment.
Para mais informações, acesse este link: https://docs.docker.com/language/python/develop/#add-a-local-database-and-persist-data


# APRENDA DOCKER DO ZERO | TUTORIAL COMPLETO COM DEPLOY

- url: https://www.youtube.com/watch?v=DdoncfOdru8
- Posso criar uma conta no dockerhub a fazer o upload da minha imagem gerada.
Esta imagem será disponibilizada para qualquer um baixar
- Para tanto, devo criar um token na minha conta e registrá-lo em minha máquina local
- Em seguida, devo criar um novo repositório e dar um push da minha imagem para ele
- Feito isso, posso baixar esta imagem na minha máquina alvo.
- Na hora de criar a imagem, posso passar uma flag com o sistema operacional da máquina alvo


## Para o caso do windows
- Usar o docker no windows é um pouco mais difícil. Ele precisa rodar em cima do WSL. Além disso, para acessar uma interface gráfica do container, é preciso instalar uma VNC neste container e acessar via minha máquina local. 
- Logo, parece uma má escolha colocar isso no CCS.