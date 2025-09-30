Docker port mapping



1]The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on Application Server 2 in Stratos Datacenter. Please perform the task as per details mentioned below:



a. Pull nginx:alpine docker image on Application Server 2.

b. Create a container named games using the image you pulled.

c. Map host port 6300 to container port 80. Please keep the container in running state.



->



Task: Setup Nginx Container on Application Server 2



Objective:

Deploy an nginx-based container for hosting an application as per Nautilus DevOps team requirements.





Steps Performed:

1]SSH into Application Server 2

&nbsp; ssh steve@172.16.238.11



2]Pull the nginx:alpine image

&nbsp; docker pull nginx:alpine

&nbsp; Ensures we use a lightweight version of nginx.



3]Run a container named games

&nbsp;docker run -d --name games -p 6300:80 nginx:alpine



-d → Detached mode, runs in background

--name games → Assigns container name

-p 6300:80 → Maps host port 6300 to container port 80





4]Verify running container

&nbsp; docker ps



Output:

CONTAINER ID   IMAGE          COMMAND                  PORTS                  NAMES

<id>           nginx:alpine   "nginx -g 'daemon of…"   0.0.0.0:6300->80/tcp   games





5]Test nginx page (optional)

curl http://localhost:6300

Returned nginx default welcome HTML.





Learning \& Notes:

1]Using nginx:alpine keeps the container lightweight and efficient.

2]Port mapping (-p host:container) is crucial to make services accessible from outside.

3]Naming containers (--name) helps in easier management (logs, exec, stop, restart).



