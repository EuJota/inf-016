# inf-016

## Tutorial - Docker Swarm local

### Inicializando docker swarm 

```
docker swarm init --advertise-addr {ip}
```

Este comando inicializa o ambiente swarm e transforma o dispositivo atual em um nó gerenciador.

### Ver informações sobre o nó no swarm 

```
docker node ls
```

### Criar um serviço

```
docker service create --replicas {numero de réplicas} --name {nome do serviço} {nome da imagem}
```

Exemplo: 

```
docker service create --replicas 1 --name helloworld nginx
```

Nós podemos utilizar um arquivo de docker-compose para realizar a mesma coisa: 

Sample Yaml:
```
version: "3"
services:
  dataservice:
    image: nginx:latest
    deploy:
      replicas: 3
    ports:
      - "3000:80"
```

O arquivo yaml acima define um serviço: dataservice com 3 réplicas e usa a imagem nginx mais recente enquanto expõe o contêiner da porta 80 à porta 3000 do host.

Em geral, o formato para criar um serviço no swarm a partir de um arquivo yaml docker-compose é:

```
docker stack deploy -c {docker-compose file name} {docker compose app name}
```

Docker stack é um comando usado para implantar um arquivo docker-compose em um ambiente swarm.

### Verificar os serviços funcionando

```
docker service ls 
```

### Escalando um serviço

```
docker service scale {nome do serviço}={numero de réplicas}
```

### Visualizar o estado dos workers

```
docker service ps {nome do serviço}
```

### Adicionar uma instância como worker do nó principal
```
docker swarm join --token SWMTKN-1-1m3v8cpftvqo8slbnwqhiff5idka3r7a74mnxekmvpo2wjhcvn-b6ykxy7mc4pouhumlgyvudqma 192.168.0.18:2377
```
### Adicionar uma instância como manager
```
docker node ls
docker node promote {node name}
```

### Remover a permissão de manager de um node
```
docker node demote {node name}
```

### Sair do Swarm Mode 

```
docker swarm leave
```

Se não funcionar, utilize:

```
docker swarm leave --force
```

### Remover um node do swarm

```
docker node rm {node name}
```

### Inspencionar um serviço
```
docker service inspect SERVICE_NAME
```

### Inspencionar um node
```
docker node inspect self --pretty
```

### Remover um serviço

```
docker service rm {nome do serviço}
```

## Docker Swarm Commands Cheatsheet 

```
#Initilizes swarm environment 
docker swarm init 
#Check the current state of the swarm with container information
docker info 
#View information of nodes in the swarm 
docker node ls 
#Create a service with the CLI
docker service create --replicas {number of replicas} --name {service name} {image name} 
#Create a service with docker-compose file
docker stack deploy -c {docker-compose file name} {docker compose app name} 
#Scaling a service
docker service scale {service name}={number of replicas}
#Check for running services 
docker service ls
#View state of worker nodes
docker service ps {service name}
#Remove a service
docker service rm {service name}
#Leave Swarm 
docker swarm leave 
#Leave swarm forcefully 
docker swarm leave --force
```
