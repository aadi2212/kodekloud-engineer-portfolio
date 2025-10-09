Deploy Nginx Webserver on Kubernetes Cluster



1]Some of the Nautilus team developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create a deployment for it with multiple replicas. Below you can find more details about it:

1\. Create a deployment using nginx image with latest tag only and remember to mention the tag i.e nginx:latest. Name it as nginx-deployment. The container should be named as nginx-container, also make sure replica counts are 3.

2\. Create a NodePort type service named nginx-service. The nodePort should be 30011.

Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.



->



🧾 Task Name: Deploy Highly Available Nginx Web Application on Kubernetes



Scenario:

The Nautilus DevOps team is setting up a highly available and scalable static website on a Kubernetes cluster. To achieve this, the team will deploy the website using an Nginx Deployment with multiple replicas, ensuring availability and load balancing through a NodePort Service.





Task Requirements:

1]Create a Deployment named nginx-deployment.

2]Use the image nginx:latest.

3]Container name: nginx-container

4]Number of replicas: 3

5]Create a NodePort Service named nginx-service.

6]The service should expose the Deployment on NodePort 30011.





Implementation Steps:

Step 1: Create Deployment YAML

nginx-deployment.yaml



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: nginx-deployment

&nbsp; labels:

&nbsp;   app: nginx

spec:

&nbsp; replicas: 3

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: nginx

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: nginx

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: nginx-container

&nbsp;         image: nginx:latest

&nbsp;         ports:

&nbsp;           - containerPort: 80



Apply it:

kubectl apply -f nginx-deployment.yaml



Verify:

kubectl get deployments

kubectl get pods





Step 2: Create NodePort Service YAML

nginx-service.yaml



apiVersion: v1

kind: Service

metadata:

&nbsp; name: nginx-service

spec:

&nbsp; type: NodePort

&nbsp; selector:

&nbsp;   app: nginx

&nbsp; ports:

&nbsp;   - port: 80

&nbsp;     targetPort: 80

&nbsp;     nodePort: 30011





Apply it:

kubectl apply -f nginx-service.yaml



Check:

kubectl get svc nginx-service





Step 3: Verification

Checkpoint	Command	Expected Output

Deployment created	kubectl get deployments	nginx-deployment (3/3) READY

Pods running	kubectl get pods	3 Pods in Running state

Service created	kubectl get svc	nginx-service (NodePort 30011)

App accessible	Open browser	Nginx default welcome page



Finally access the application through browser 





Concepts Learned:

1]Deployment: Ensures declarative management and scaling of Pods.

2]ReplicaSets: Maintain desired Pod counts for high availability.

3]NodePort Service: Exposes the application externally.

4]Labels \& Selectors: Connect Deployments and Services efficiently.





⚡ Common Issues \& Fixes

❌ Error



Error from server (BadRequest): error when creating "nginx-service.yml":

Service in version "v1" cannot be handled as a Service:

strict decoding error: unknown field "spec.ports\[0].NodePort"





✅ Cause

YAML field name is case-sensitive — i used NodePort instead of nodePort.





🧩 Fix

Use the correct lowercase key:

nodePort: 30011





Then reapply:

kubectl apply -f nginx-service.yml





Verification:

kubectl get svc nginx-service





Expected output:

NAME             TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE

nginx-service    NodePort   10.96.x.x      <none>        80:30011/TCP   5s





