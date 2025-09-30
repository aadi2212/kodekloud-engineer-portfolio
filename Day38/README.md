Pull Docker Image



1]

Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:



a. Pull busybox:musl image on App Server 2 in Stratos DC and re-tag (create new tag) this image as busybox:news.



->



📒 OneNote Documentation



Task Title: Pull and Retag BusyBox Image



Objective:

The Nautilus project developers want to test containerized environment features. As part of this, we need to pull the busybox:musl image on App Server 2 and re-tag it as busybox:news.





Steps Taken:



1]Pulled the required image:

docker pull busybox:musl

Downloaded the BusyBox image with the musl tag from Docker Hub.



2]Re-tagged the image:

docker tag busybox:musl busybox:news

Created a new tag news for the same image locally.



3]Verified the images:

docker images



REPOSITORY   TAG       IMAGE ID       CREATED         SIZE

busybox      musl      44f1048931f5   11 months ago   1.46MB

busybox      news      44f1048931f5   11 months ago   1.46MB



Confirmed both busybox:musl and busybox:news exist with the same image ID.



Final Outcome:

The image busybox:musl was successfully pulled and re-tagged as busybox:news. Both images are available on App Server 2 for application testing.



