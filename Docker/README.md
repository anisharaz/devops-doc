# Docker
Docker is an application which is used to manage containers. Containers are lightweight and portable encapsulations of an environment in which to run applications. Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels.For more details check this out [Docker overview](https://docs.docker.com/guides/docker-overview/)

## Installation

**Ubuntu**
```bash
sudo apt-get update
sudo apt-get install docker.io
```
**Arch**
```bash
sudo pacman -S docker
sudo systemctl start docker
sudo systemctl enable docker
```
Note: For other OS, please refer to the official [Docker documentation](https://docs.docker.com/get-docker/)

## Configuration
Isolating Containers in a separate network is a best practice to avoid conflicts with other containers and only containers in same network can communicate to each other. To create a new network, run the following command:
```bash
docker network create --driver bridge --subnet 11.0.0.0/24 --attachable deploy
```
Note: you can change the subnet as you want.
## Usage
To run a container, use the following command:
```bash
docker run -d --network deploy --ip {ip_address} --name {container_name} {image_name}
```
> [!IMPORTANT]
> Replace `{ip_address}` with the desired IP address. from range 10.0.0.2 - 10.0.0.254

# Docker Compose 
Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services(containers). Then, with a single command, you create and start all the services from your configuration. For more details check this out [Docker Compose overview](https://docs.docker.com/compose/)

## Installation
Docker compose comes pre-installed with Docker.So to check if docker compose is working correctly run the following command:
```bash
docker compose --help
```
If you get the version of docker-compose then it is installed correctly.

> [!NOTE]
> The older version of `docker compose` command is `docker-compose`. So if you are using an older version of docker-compose then you can use `docker-compose --version` to check is compose is working correctly.

## Usage
To use docker compose, you need to create a `docker-compose.yml` file in the root directory of your project. Here is an example of a `docker-compose.yml` file with 2 services (web and db):
```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: example
```
To run the services defined in the `docker-compose.yml` file, use the following command:
```bash
docker compose up -d
```
To stop the services, use the following command:
```bash
docker compose down
```
By Defualt docker compose creates a network with the name of the directory where the `docker-compose.yml` file is present. If that is ok for your use case then you can use the default network. But if you want to attach the services(containers) to an existing network then you have to specify the network name in the `docker-compose.yml` file. Here is an example of how to specify the network name in the `docker-compose.yml` file:
```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    networks:
        internal-network:
            ipv4_address: 11.0.0.9
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: password
    networks:
        internal-network:
            ipv4_address: 11.0.0.8   

networks:
  internal-network:
    name: deploy    //name of the existing network
    external: true
```
In the above example, the services `web` and `db` are attached to the network `deploy`. The network `deploy` is an existing network. If the network `deploy` does not exist then docker compose will not run.

***The `ipv4_address` in the compose file is set according to the CIDR of the network created during docker configuration***

