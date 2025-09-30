Deploy an App on Docker Containers



1]

The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie. Below are the details of the task:

1\. On App Server 1 in Stratos Datacenter create a docker compose file /opt/finance/docker-compose.yml (should be named exactly).

2\. The compose should deploy two services (web and DB), and each service should deploy a container as per details below:



For web service:

a. Container name must be php\_web.

b. Use image php with any apache tag. Check here for more details.

c. Map php\_web container's port 80 with host port 6000

d. Map php\_web container's /var/www/html volume with host volume /var/www/html.



For DB service:

a. Container name must be mysql\_web.

b. Use image mariadb with any tag (preferably latest). Check here for more details.

c. Map mysql\_web container's port 3306 with host port 3306

d. Map mysql\_web container's /var/lib/mysql volume with host volume /var/lib/mysql.

e. Set MYSQL\_DATABASE=database\_web and use any custom user ( except root ) with some complex password for DB connections.



3\. After running docker-compose up you can access the app with curl command curl <server-ip or hostname>:6000/



For more details check here.



Note: Once you click on FINISH button, all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.



->



Task: Deploy Web + DB Stack using Docker Compose



Objective:

Set up a containerized stack on App Server 1 using Docker Compose with a PHP + Apache web container and a MariaDB DB container.



Steps Performed:

1]Created Directory and File

mkdir -p /opt/finance    -- if not created

cd /opt/finance



File created:

/opt/finance/docker-compose.yml





2]Initial Docker Compose File (with mistake)

version: '3.8'



services:

&nbsp; web:

&nbsp;   container\_name: php\_web

&nbsp;   image: php:apache

&nbsp;   ports:

&nbsp;     - "6000:80"

&nbsp;   volumes:

&nbsp;     - /var/www/html:/var/www/html



&nbsp; db:

&nbsp;   container: mysql\_web   # ❌ Wrong key used here

&nbsp;   image: mariadb:latest

&nbsp;   ports:

&nbsp;     - "3306:3306"

&nbsp;   volumes:

&nbsp;     - /var/lib/mysql:/var/lib/mysql

&nbsp;   environment:

&nbsp;     MYSQL\_DATABASE: database\_web

&nbsp;     MYSQL\_USER: app\_user

&nbsp;     MYSQL\_PASSWORD: C0mpl3x@Pass

&nbsp;     MYSQL\_ROOT\_PASSWORD: R00t@Backup





3]Error Encountered

When running:

docker compose -f /opt/finance/docker-compose.yml up -d





Error message:

validating /opt/finance/docker-compose.yml: services.db Additional property container is not allowed



Root Cause:

Used container instead of the correct key container\_name.



4]Corrected Docker Compose File

version: '3.8'



services:

&nbsp; web:

&nbsp;   container\_name: php\_web

&nbsp;   image: php:apache

&nbsp;   ports:

&nbsp;     - "6000:80"

&nbsp;   volumes:

&nbsp;     - /var/www/html:/var/www/html



&nbsp; db:

&nbsp;   container\_name: mysql\_web

&nbsp;   image: mariadb:latest

&nbsp;   ports:

&nbsp;     - "3306:3306"

&nbsp;   volumes:

&nbsp;     - /var/lib/mysql:/var/lib/mysql

&nbsp;   environment:

&nbsp;     MYSQL\_DATABASE: database\_web

&nbsp;     MYSQL\_USER: app\_user

&nbsp;     MYSQL\_PASSWORD: C0mpl3x@Pass

&nbsp;     MYSQL\_ROOT\_PASSWORD: R00t@Backup





5]Deployment

docker compose -f /opt/finance/docker-compose.yml up -d

✔ Both containers (php\_web and mysql\_web) started successfully.





6]Verification

docker ps



Expected output:

CONTAINER ID	NAME	IMAGE	PORTS	STATUS

abc123xyz456	php\_web	php:apache	0.0.0.0:6000->80/tcp	Up

def789uvw012	mysql\_web	mariadb:latest	0.0.0.0:3306->3306/tcp	Up





7]Test web service:

curl http://<server-ip>:6000/





Outcome:

1]Initial deployment failed due to YAML key error (container vs container\_name).

2]Issue corrected, stack now runs successfully with:

&nbsp;  php\_web on port 6000

&nbsp;  mysql\_web on port 3306







