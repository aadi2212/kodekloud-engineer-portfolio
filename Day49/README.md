Deploy Applications with Kubernetes Deployments



1]The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:

Create a deployment named httpd to deploy the application httpd using the image httpd:latest (ensure to specify the tag)

Note: The kubectl utility on jump\_host is set up to interact with the Kubernetes cluster.



->



📘 KodeKloud Task Documentation – Deploy Application with Kubernetes Deployment



Task Objective:



Create a Deployment named httpd to deploy the httpd application using the httpd:latest image.



Requirements:

1]Deployment name: httpd

2]Container name: httpd-container

3]Image: httpd:latest

4]Label: app=httpd\_app





Steps Followed:



1]Initial Deployment YAML (with errors):



appVersion: /apps/v1

kind: Deployment

metadata:

  name: httpd

spec:

  replicas: 1

  selector:

    matchlabels:

      app: httpd\_app

  template:

    metadata:

      labels:

        app: httpd\_app

    spec:

      containers:

        - name: httpd-container

          image: httpd:latest





2]Applied the manifest:

kubectl apply -f httpd-deployment.yaml





❌ Error Encountered



error: error validating "httpd-deployment.yaml": error validating data: apiVersion not set



Root Causes:

1]matchlabels → should be matchLabels (camel case)





✅ Resolution

Corrected Deployment YAML:



apiVersion: apps/v1

kind: Deployment

metadata:

  name: httpd

spec:

  replicas: 1

  selector:

    matchLabels:

      app: httpd\_app

  template:

    metadata:

      labels:

        app: httpd\_app

    spec:

      containers:

        - name: httpd-container

          image: httpd:latest





3]Apply corrected manifest:

 kubectl apply -f httpd-deployment.yaml





4]Verification

Check deployment status:

kubectl get deployment httpd





5]Check pods created by deployment:

kubectl get pods -l app=httpd\_app



6]Describe deployment (for detailed info):

kubectl describe deployment httpd





Final Outcome:

1]Deployment httpd successfully created.

2]Pod(s) running httpd:latest image.

3]YAML errors fixed.

4]Verified using kubectl get and kubectl describe.

