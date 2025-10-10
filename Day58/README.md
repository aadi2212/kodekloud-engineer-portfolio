Deploy Grafana on Kubernetes Cluster



1]The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.



1.) Create a deployment named grafana-deployment-devops using any grafana image for Grafana app. Set other parameters as per your choice.

2.) Create NodePort type service with nodePort 32000 to expose the app.

You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.

Note: The kubectl on jump\_host has been configured to work with kubernetes cluster.



->



🧾 Task Name: Deploy Grafana Application on Kubernetes



Scenario:



The Nautilus DevOps team is setting up Grafana to visualize and analyze application metrics. The task involves deploying Grafana as a Kubernetes Deployment and exposing it externally using a NodePort Service on port 32000.



Requirements:

1]Deployment Name: grafana-deployment-devops.

2]Container Image: grafana/grafana:latest (any grafana image acceptable).

3]Service Type: NodePort

4]NodePort: 32000

5]Goal: Access the Grafana login page in a browser to verify successful deployment.

6]kubectl: Already configured to access the Kubernetes cluster.





Steps to Implement:

Step 1: Create the Grafana Deployment

Create a YAML file named grafana-deployment.yaml or run directly via CLI.



✅ Using CLI:

kubectl create deployment grafana-deployment-devops --image=grafana/grafana:latest



Alternatively, you can create it using YAML (recommended for documentation):



grafana-deployment.yml



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: grafana-deployment-devops

&nbsp; labels:

&nbsp;   app: grafana

spec:

&nbsp; replicas: 1

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: grafana

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: grafana

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: grafana-container

&nbsp;         image: grafana/grafana:latest

&nbsp;         ports:

&nbsp;           - containerPort: 3000





Apply the deployment:

kubectl apply -f grafana-deployment.yml



Verify deployment:

kubectl get deployments

kubectl get pods





Step 2: Create the NodePort Service

Expose Grafana externally using a NodePort service on port 32000.



✅ Using CLI:

kubectl expose deployment grafana-deployment-devops \\

&nbsp; --type=NodePort \\

&nbsp; --port=3000 \\

&nbsp; --target-port=3000 \\

&nbsp; --name=grafana-service \\

&nbsp; --node-port=32000





Or create via YAML:



grafana-service.yml



apiVersion: v1

kind: Service

metadata:

&nbsp; name: grafana-service

spec:

&nbsp; type: NodePort

&nbsp; selector:

&nbsp;   app: grafana

&nbsp; ports:

&nbsp;   - port: 3000

&nbsp;     targetPort: 3000

&nbsp;     nodePort: 32000





Apply it:

kubectl apply -f grafana-service.yaml





Step 3: Verify Deployment and Service

Check if pods and services are running:

kubectl get pods

kubectl get svc



You should see:

NAME                TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE

grafana-service     NodePort   10.96.0.15      <none>        3000:32000/TCP   2m





Step:4 Access Grafana

Find your Node IP:

kubectl get nodes -o wide



Open your browser and go to:

http://<Node-IP>:32000



You should see the Grafana login page 🎉





Step 5 (Optional): Verify Container Logs

To confirm Grafana started correctly:



kubectl logs -f <grafana-pod-name>



Look for:

HTTP Server Listen: http://0.0.0.0:3000





✅ Verification Checklist



Checkpoint	Status

Deployment created (grafana-deployment-devops)	✅

Grafana container running	✅

NodePort service exposed on 32000	✅

Accessed Grafana login page	✅





📘 Summary:

Deployed Grafana on Kubernetes using a Deployment and exposed it externally via a NodePort Service (32000). Verified successful setup by accessing the Grafana login page in a browser — confirming that the service is reachable and functional.









