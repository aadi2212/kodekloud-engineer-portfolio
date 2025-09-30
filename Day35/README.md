Install Docker Packages and Start Docker Service



1]

The Nautilus DevOps team aims to containerize various applications following a recent meeting with the application development team. They intend to conduct testing with the following steps:

&nbsp;	1. Install docker-ce and docker compose packages on App Server 3.

&nbsp;	2. Initiate the docker service.

->



📄 Task Documentation: Docker \& Docker Compose Installation on App Server 3



Objective:



1]Install Docker CE and Docker Compose on App Server 3.

2]Start and enable the Docker service.

3]Test that Docker and Docker Compose are functioning correctly.



Steps Performed:



1]Install Docker CE



sudo yum install -y yum-utils

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install -y docker-ce docker-ce-cli containerd.io



2]Install Docker Compose



Option 1: Using package manager

sudo yum install -y docker-compose   





But its not worked I used another option





Option 2: Using Docker Compose official binary

sudo curl -L "https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose



sudo chmod +x /usr/local/bin/docker-compose



docker-compose --version



3]Start and Enable Docker Service

sudo systemctl start docker

sudo systemctl enable docker

sudo systemctl status docker



start → start Docker immediately

enable → ensure Docker starts on system boot

status → verify Docker is active and running



4]Test Docker installation

docker version                             # Check Docker engine version

docker info                                   # Check Docker daemon info 

docker run hello-world              # Test running a container



Successful hello-world container run confirms Docker is working properly.

Docker Compose version check confirms Compose installation.



✅ Notes / Observations

Always ensure Docker service is running before using Docker Compose.

Use the official binary if the package manager version of Docker Compose is outdated.

This setup prepares App Server 3 for containerizing applications as per DevOps requirements.





