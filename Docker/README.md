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
