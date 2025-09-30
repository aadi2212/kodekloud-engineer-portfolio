Docker Python App



1]

A python app needed to be Dockerized, and then it needs to be deployed on App Server 1. We have already copied a requirements.txt file (having the app dependencies) under /python\_app/src/ directory on App Server 1. Further complete this task as per details mentioned below:

1\. Create a Dockerfile under /python\_app directory:

&nbsp;	• Use any python image as the base image.

&nbsp;	• Install the dependencies using requirements.txt file.

&nbsp;	• Expose the port 5003.

&nbsp;	• Run the server.py script using CMD.

2\. Build an image named nautilus/python-app using this Dockerfile.

3\. Once image is built, create a container named pythonapp\_nautilus:

&nbsp;	• Map port 5003 of the container to the host port 8097.

4\. Once deployed, you can test the app using curl command on App Server 1.



curl http://localhost:8097/



->



Task: Dockerize and Deploy Python App on App Server 1



Objective:



1]Dockerize a Python application.

2]Deploy it on App Server 1.

3]Expose the app on host port 8097 mapping to container port 5003.

4]Verify using curl.



Directory Structure:

/python\_app/

&nbsp;   Dockerfile

&nbsp;   /src/

&nbsp;       requirements.txt

&nbsp;       server.py





Step1:Ensure directories exist

sudo mkdir -p /python\_app/src

ls -l /python\_app





Expected Output:

drwxr-xr-x 2 root root 4096 Sep 24  src

requirements.txt and server.py must be inside /python\_app/src/.





Step2:Create server.py

sudo vi /python\_app/src/server.py



Content:

from flask import Flask

app = Flask(\_\_name\_\_)



@app.route("/")

def home():

&nbsp;   return "Welcome to xFusionCorp Industries!"



if \_\_name\_\_ == "\_\_main\_\_":

&nbsp;   app.run(host="0.0.0.0", port=5003)





Step3:Create Dockerfile

cd /python\_app

sudo vi Dockerfile  --if error occours i.e. command not found then its related to the editor which is not installed in the system so we need to install it using command i.e. sudo yum install -y vim



Content:

\# Use a Python base image

FROM python:3.11-slim



\# Set working directory

WORKDIR /app



\# Copy requirements.txt

COPY src/requirements.txt .



\# Install dependencies

RUN pip install --no-cache-dir -r requirements.txt



\# Copy application source code

COPY src/ .



\# Expose application port

EXPOSE 5003



\# Run the Python server

CMD \["python", "server.py"]





Verify:

ls -l /python\_app/Dockerfile



Expected Output:

-rw-r--r-- 1 root root ... Dockerfile





Step4:Build Docker Image

docker build -t nautilus/python-app .



Error occoured:

failed to resolve source metadata for docker.io/library/python:3.11-slim

dial tcp 10.0.0.6:443: i/o timeout



Reason:

1]The server could not reach Docker Hub (or the registry mirror timed out).

2]Docker cannot download the base image automatically.



Solution: Pull the image manually

docker pull python:3.11-slim



Expected output: Docker downloads all layers successfully.



Then rebuild the image:

docker build -t nautilus/python-app .





Step5:Run Docker Container

Remove old container if exists:



Run container:

docker run -d --name pythonapp\_nautilus -p 8097:5003 nautilus/python-app



Verify container:

docker ps





Step6:Test Application

curl http://localhost:8097/



Expected Output:

Welcome to xFusionCorp Industries!





Notes / Lessons Learned

&nbsp;	• Important: Dockerfile must be located at /python\_app/Dockerfile, not /home/username/python\_app/. Grader checks exact path.

&nbsp;	• Network issues: DeadlineExceeded errors happen if server cannot reach Docker Hub or mirror. Pull images manually if needed.

&nbsp;	• Container naming and port mapping must exactly match instructions for KodeKloud task grading.



