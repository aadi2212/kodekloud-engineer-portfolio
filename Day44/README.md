Write a Docker Compose File



1]

The Nautilus application development team shared static website content that needs to be hosted on the httpd web server using a containerised platform. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines. Below are the details:



a. On App Server 2 in Stratos DC create a container named httpd using a docker compose file /opt/docker/docker-compose.yml (please use the exact name for file).



b. Use httpd (preferably latest tag) image for container and make sure container is named as httpd; you can use any name for service.



c. Map 80 number port of container with port 3000 of docker host.



d. Map container's /usr/local/apache2/htdocs volume with /opt/itadmin volume of docker host which is already there. (please do not modify any data within these locations).



->



Hosting Static Website on HTTPD Container using Docker Compose



Server: App Server 2, Stratos DC

Task: Host static website content on httpd using a containerized platform



Task Requirements

1]Create a container named httpd using a Docker Compose file located at /opt/docker/docker-compose.yml.

2]Use httpd:latest image.

3]Map container port 80 to host port 3000.

4]Map container volume /usr/local/apache2/htdocs to host volume /opt/itadmin (pre-existing).

5]Use Docker Compose for container orchestration.



&nbsp;Docker Compose File



Correct YAML (/opt/docker/docker-compose.yml):



version: "3"

services:

&nbsp; web:

&nbsp;   image: httpd:latest

&nbsp;   container\_name: httpd

&nbsp;   ports:

&nbsp;     - "3000:80"

&nbsp;   volumes:

&nbsp;     - /opt/itadmin:/usr/local/apache2/htdocs

&nbsp;   restart: always





Explanation of fields:

1]services.web: Service name; can be any name (web used here).

2]image: Pulls the latest HTTPD server image.

3]container\_name: Ensures the container is named httpd (mandatory for the task).

4]ports: Maps host port 3000 → container port 80.

5]volumes: Maps host directory /opt/itadmin → container web root /usr/local/apache2/htdocs.

6]restart: always: Ensures the container restarts automatically after host reboot.





Commands used:



Step:1]Navigate to the docker directory

&nbsp;          cd /opt/docker



Step:2]Bring down any existing containers (if present)

&nbsp;          docker compose down



Step:3]Start container using Docker Compose

&nbsp;          docker compose -f /opt/docker/docker-compose.yml up -d



Explanation:

1]Initially, docker-compose up -d was attempted.

2]Error encountered: -bash: docker-compose: command not found



Reason: Recent Docker versions include Docker Compose V2, which uses docker compose (space) instead of docker-compose.



3]The command docker compose -f /opt/docker/docker-compose.yml up -d explicitly points to the Compose file and starts the container in detached mode.



Step:4]Verify container is running

&nbsp;           docker ps



Step:5]Verify volume mount

&nbsp;          docker exec -it httpd ls /usr/local/apache2/htdocs



Step:6]Test website access

&nbsp;         curl http://localhost:3000





Errors Faced \& Lessons Learned:

Incorrect Compose property:

1]Mistake: Used docker\_container: instead of container\_name:.

2]Effect: Docker Compose reported additional properties 'docker\_container' not allowed.

3]Lesson: Use container\_name for naming containers in Compose.



Docker Compose command not found:

1]Mistake: Used docker-compose up -d

2]Effect: command not found because Docker Compose V2 uses docker compose (space).

3]Lesson: Always check Docker Compose version with docker compose version before running commands.



Volume mismatch (in previous lab attempt):

1]Mistake: Used wrong host volume path (e.g., /opt/devops instead of /opt/itadmin).

2]Effect: Task failed because KodeKloud grader checks exact host paths.

3]Lesson: Always cross-verify host paths in task instructions.



