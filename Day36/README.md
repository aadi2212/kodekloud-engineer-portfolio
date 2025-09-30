Deploy Nginx Container on Application Server



1]

The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on Application Server 2. Complete the task with the following instructions:



i]On Application Server 2 create a container named nginx\_2 using the nginx image with the alpine tag. Ensure container is in a running state.



->



📘 Task Documentation – Nginx Container Deployment on Application Server 2



🔹 Task Description



The Nautilus DevOps team required an nginx container deployment on Application Server 2 for application testing. The container should:



1]Be created using the nginx:alpine image.

2]Run in a container named nginx\_2.

3]Remain in a running state.





🔹 Steps Performed



1] Logged in to Application Server 2

ssh user@appserver2



2] Verified Docker installation and service

&nbsp;docker --version

systemctl status docker



If Docker was not running:

sudo systemctl start docker

sudo systemctl enable docker



3]Pulled the nginx:alpine image

&nbsp;sudo docker pull nginx:alpine



4]Created and Started the container

&nbsp;sudo docker run -d --name  nginx\_2 nginx:alpine



• -d → Detached mode (runs in background).

• --name nginx\_2 → Sets container name.

• nginx:alpine → Lightweight Nginx image based on Alpine Linux.





5]Verified container status

&nbsp;sudo docker ps



Expected Output:

CONTAINER ID   IMAGE          COMMAND                  STATUS         NAMES

abcd1234efgh   nginx:alpine   "/docker-entrypoint.…"   Up X minutes   nginx\_2





6]Validation

Container nginx\_2 is up and running.

Logs can be checked with:



sudo docker logs nginx\_2



✅ Final Outcome



• Container Name: nginx\_2

• Image Used: nginx:alpine

• State: Running successfully on Application Server 2.





