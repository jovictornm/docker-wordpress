# docker-wordpress
Comandos para implementação de servidor wordpress utilizando docker

<p float="left">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" height= "70" width="100" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/linux/linux-original.svg" height= "70" width="100" /> 
</p>

```
sudo yum install -y yum-utils 
```

Adicionar repositorio do docker

```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo 
```

Instalar Docker
```
sudo yum install docker-ce docker-ce-cli containerd.io
```

Iniciar serviço Docker

```
sudo systemctl start docker 
```

Criar arquivo docker.socket

```
vi /usr/lib/systemd/system/docker.socket 
```

Arquivo tem que estar assim:

```
[Unit]
Description=Docker Socket for the API
PartOf=docker.service

[Socket]
ListenStream=/var/run/docker.sock
SocketMode=0660
SocketUser=root
SocketGroup=docker

[Install]
WantedBy=sockets.target 
```

Iniciar Serviço docker.socket

```
sudo systemctl daemon-reload

sudo systemctl start docker.socket

sudo systemctl start docker  

sudo systemctl enable docker.service 
```

Instalar Docker Compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

Instalar dependências do Docker Compose

```
sudo yum install pip

sudo pip install docker-compose
```

Criando diretórios e arquivos wordpress

```
mkdir /root/wordpress

cd /root/wordpress

touch docker-compose.yaml

sudo vi docker-compose.yaml

```

colocar dentro do arquivo "docker-compose.yaml"

```
version: '2.0'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 80:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```
Iniciar Docker Compose (executar esse comando em modo usuario normal, não root)

```
docker-compose up 
```
          
