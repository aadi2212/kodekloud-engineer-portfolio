Create Docker Image From Container



1] 

One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:



a. Create an image games:datacenter on Application Server 2 from a container ubuntu\_latest that is running on same server.



->



📒 OneNote Documentation



Task Title: Create Image from Running Container



Objective:

A Nautilus developer was testing changes inside a container. To preserve these changes, the DevOps team was asked to create a new image from the running container ubuntu\_latest. The new image should be tagged as games:datacenter.



Steps Taken:



1]Checked running containers:

docker ps

Confirmed ubuntu\_latest container was running.



2]Created a new image from the container:

docker commit ubuntu\_latest games:datacenter

This saved the container state as a new Docker image with name games and tag datacenter.



3]Verified the new image:

docker images

REPOSITORY   TAG          IMAGE ID       CREATED         SIZE

games        datacenter   ff6f25964b13   6 seconds ago   132MB

ubuntu       latest       802541663949   3 weeks ago     78.1MB



Confirmed games:datacenter was listed among available images.



Final Outcome:

The container ubuntu\_latest was successfully committed into a new image named games:datacenter.







