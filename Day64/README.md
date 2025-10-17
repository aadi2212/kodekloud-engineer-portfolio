Fix Python App Deployed on Kubernetes Cluster 



1]One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

1\. The deployment name is python-deployment-xfusion, its using poroko/flask-demo-appimage. The deployment and service of this app is already deployed.

2\. nodePort should be 32345 and targetPort should be python flask app's default port.



Note: The kubectl on jump\_host has been configured to work with the kubernetes cluster.



->



Task: Fix Python App Deployed on Kubernetes Cluster



Scenario:

One of the DevOps engineers tried deploying a Python Flask application on the Kubernetes cluster. The app was not coming up due to misconfiguration. The goal was to fix the deployment so that the Flask app is running and accessible via the specified NodePort.



Objective:

1]Fix the existing Deployment for the Flask app.

2]Ensure the pod starts successfully using a valid container image.

3]Expose the app via a NodePort on 32345.

4]Confirm end-to-end functionality: Deployment → Pod → Service → NodePort → Browser.





1️⃣ Corrected Deployment YAML



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: python-deployment-xfusion

spec:

&nbsp; replicas: 1

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: python-app

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: python-app

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: python-container

&nbsp;         image: app:latest

&nbsp;         ports:

&nbsp;           - containerPort: 5000





Key Points:

1]replicas: 1 → single pod for testing.

2]selector.matchLabels matches pod template labels exactly.

3]containerPort: 5000 matches Flask’s listening port.

4]Image corrected to app:latest to fix ImagePullBackOff.





2️⃣ Corrected Service YAML (NodePort)



apiVersion: v1

kind: Service

metadata:

&nbsp; name: python-service-xfusion

spec:

&nbsp; type: NodePort

&nbsp; selector:

&nbsp;   app: python-app

&nbsp; ports:

&nbsp;   - port: 5000

&nbsp;     targetPort: 5000

&nbsp;     nodePort: 32345





Key Points:

1]NodePort exposes the service externally.

2]nodePort: 32345 makes the app accessible outside the cluster.

3]targetPort: 5000 maps to the container’s Flask port.







3️⃣ Steps Taken to Fix:

1]Deleted the broken pod/deployment its needed:

kubectl delete deployment python-deployment-xfusion





2]Applied corrected Deployment:

kubectl apply -f python-deployment.yml





3]Deleted the broken pod/service its needed:

Kubectl delete service python-service-xfusion





4]Applied corrected Service:

kubectl apply -f python-service.yml





5]Verified pod is running:

kubectl get pods

kubectl describe pod python-deployment-xfusion-xxxxx

kubectl logs python-deployment-xfusion-xxxxx





6]Verified service and NodePort:

kubectl get svc python-service-xfusion





7]Accessed the Flask app in browser:

http://<NodeIP>:32345





Output:

Hello World Pyvo 1!





4️⃣ Troubleshooting / Lessons Learned



Issue	Cause	Solution

Pod ImagePullBackOff	Incorrect image name	Changed image to app:latest

502 Bad Gateway	Pod not running or Flask listening on localhost	Ensure Flask binds to 0.0.0.0:5000

Selector mismatch	Service could not reach pod	Verified Deployment labels match Service selector



5️⃣ Verification Checklist



Parameter	Expected Value	Status

Deployment Name	python-deployment-xfusion	✔️

Pod Status	Running	✔️

Container Image	app:latest	✔️

Container Port	5000	✔️

Service Type	NodePort	✔️

NodePort	32345	✔️

App Output	Hello World Pyvo 1!	✔️





6️⃣ Key Takeaways

1]Always verify container image availability before deployment.

2]Ensure Flask binds to 0.0.0.0 for external accessibility.

3]Kubernetes NodePort allows accessing services from outside the cluster.

4]Matching Deployment labels and Service selectors is critical for traffic routing.



