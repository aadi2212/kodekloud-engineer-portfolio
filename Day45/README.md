Resolve Docker-file Issues



1]

The Nautilus DevOps team is working to create new images per requirements shared by the development team. One of the team members is working to create a Dockerfile on App Server 1 in Stratos DC. While working on it she ran into issues in which the docker build is failing and displaying errors. Look into the issue and fix it to build an image as per details mentioned below:



a. The Dockerfile is placed on App Server 1 under /opt/docker directory.



b. Fix the issues with this file and make sure it is able to build the image.



c. Do not change base image, any other valid configuration within Dockerfile, or any of the data been used — for example, index.html.



Note: Please note that once you click on FINISH button all the existing containers will be destroyed and new image will be built from your Dockerfile.



->



Dockerfile Task: Building a Custom HTTPD Image with SSL



Server: App Server 1, Stratos DC

Task: Build a Docker image from a Dockerfile provided by the Nautilus DevOps team.





Step:1Task Requirements:

1]Dockerfile location: /opt/docker



2]Base image: httpd:2.4.43 (cannot be changed)



3]Configure Apache to:

Change default port 80 → 8080.

Enable SSL modules and include SSL configuration.



4]Copy SSL certificates (server.crt, server.key) and index.html into the container



5]Successfully build the image without modifying the base image or any data





Step:2Original Issues Faced:

Incorrect httpd.conf path



RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf.d/httpd.conf



1]Path /usr/local/apache2/conf.d/httpd.conf does not exist in official httpd:2.4.43 image.

2]All other sed commands referencing this path failed.





COPY path errors:

Build failed if certs/ or html/ directories were missing or not in the build context (/opt/docker)





Step:3Corrected Dockerfile:

FROM httpd:2.4.43



\# Update Apache to listen on port 8080

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf



\# Enable SSL modules and include SSL config

RUN sed -i '/LoadModule ssl\_module modules\\/mod\_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule socache\_shmcb\_module modules\\/mod\_socache\_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

RUN sed -i '/Include conf\\/extra\\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf



\# Copy SSL certificates

COPY certs/server.crt /usr/local/apache2/conf/server.crt

COPY certs/server.key /usr/local/apache2/conf/server.key



\# Copy website content

COPY html/index.html /usr/local/apache2/htdocs/





Notes:

1]All sed commands point to the correct httpd.conf path.

2]COPY commands assume certs/ and html/ exist in /opt/docker.





Step:4Build the Docker Image:

docker build -t my-httpd-ssl /opt/docker



-t my-httpd-ssl → tags the image

/opt/docker → build context (must contain Dockerfile + certs + html)





Step:5 Verify the image

docker images | grep my-httpd-ssl





Lessons Learned / Errors Encountered

1]Incorrect httpd.conf path caused all sed commands to fail → fixed by using /usr/local/apache2/conf/httpd.conf.

2]COPY path issues → build context must include certs/ and html/.

3]Docker build context → files must exist relative to the build directory.





Summary:

Successfully built a custom HTTPD Docker image with:

1]Port 8080 configured

2]SSL enabled

3]Website content properly mapped



