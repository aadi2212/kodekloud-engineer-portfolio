Write a Dockerfile



1]

As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file /opt/docker/Dockerfile (please keep D capital of Dockerfile) on App server 2 in Stratos DC and configure to build an image with the following requirements:



a. Use ubuntu:24.04 as the base image.



b. Install apache2 and configure it to work on 6200 port. (do not update any other Apache configuration settings like document root etc).



->



Task Documentation – Dockerfile for Apache on Custom Port



Task Name:



Create a custom Dockerfile to build an Apache server running on port 6200



Environment:

App Server 2 in Stratos DC

Path for Dockerfile: /opt/docker/Dockerfile



Requirements:

1]Base image: ubuntu:24.04

2]Install apache2 package.

3]Configure Apache to work on port 6200 (keep default document root).

4]Expose the port.

5]Ensure Apache runs in the foreground.





Steps Performed:

1]Created directory and Dockerfile

&nbsp;mkdir -p /opt/docker

&nbsp;cd /opt/docker

&nbsp;vi Dockerfile



2]Dockerfile content:

FROM ubuntu:24.04



ENV DEBIAN\_FRONTEND=noninteractive



RUN apt-get update \&\& \\

&nbsp;   apt-get install -y apache2 \&\& \\

&nbsp;   apt-get clean \&\& \\

&nbsp;   rm -rf /var/lib/apt/lists/\*



RUN sed -i 's/80/6200/g' /etc/apache2/ports.conf \&\& \\

&nbsp;   sed -i 's/\*:80/\*:6200/g' /etc/apache2/sites-available/000-default.conf



EXPOSE 6200



CMD \["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]





3]Built the image:

&nbsp; docker build -t custom-apache:latest .



4]Ran a container from the image:

&nbsp;docker run -d -p 6200:6200 custom-apache:latest 



5]Verified Apache service on port 6200:

&nbsp;curl http://localhost:6200



Output: Default Apache welcome page HTML





Final Outcome:

A custom Docker image was successfully created using ubuntu:24.04, running Apache2 on port 6200, and tested with a running container.







