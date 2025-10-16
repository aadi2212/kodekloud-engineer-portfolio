Deploy IRON Gallery App on Kubernetes



1]There is an iron gallery app that the Nautilus DevOps team was developing. They have recently customized the app and are going to deploy the same on the Kubernetes cluster. Below you can find more details:

1\. Create a namespace iron-namespace-xfusion

2\. Create a deployment iron-gallery-deployment-xfusion for iron gallery under the same namespace you created.

:- Labels run should be iron-gallery.

:- Replicas count should be 1.

:- Selector's matchLabels run should be iron-gallery.

:- Template labels run should be iron-gallery under metadata.

:- The container should be named as iron-gallery-container-xfusion, use kodekloud/irongallery:2.0 image ( use exact image name / tag ).

:- Resources limits for memory should be 100Mi and for CPU should be 50m.

:- First volumeMount name should be config, its mountPath should be /usr/share/nginx/html/data.

:- Second volumeMount name should be images, its mountPath should be /usr/share/nginx/html/uploads.

:- First volume name should be config and give it emptyDir and second volume name should be images, also give it emptyDir.

3\. Create a deployment iron-db-deployment-xfusion for iron db under the same namespace.

:- Labels db should be mariadb.

:- Replicas count should be 1.

:- Selector's matchLabels db should be mariadb.

:- Template labels db should be mariadb under metadata.

:- The container name should be iron-db-container-xfusion, use kodekloud/irondb:2.0 image ( use exact image name / tag ).

:- Define environment, set MYSQL\_DATABASE its value should be database\_host, set MYSQL\_ROOT\_PASSWORD and MYSQL\_PASSWORD value should be with some complex passwords for DB connections, and MYSQL\_USER value should be any custom user ( except root ).

:- Volume mount name should be db and its mountPath should be /var/lib/mysql. Volume name should be db and give it an emptyDir.

4\. Create a service for iron db which should be named iron-db-service-xfusion under the same namespace. Configure spec as selector's db should be mariadb. Protocol should be TCP, port and targetPort should be 3306 and its type should be ClusterIP.

5\. Create a service for iron gallery which should be named iron-gallery-service-xfusion under the same namespace. Configure spec as selector's run should be iron-gallery. Protocol should be TCP, port and targetPort should be 80, nodePort should be 32678 and its type should be NodePort.

Note:

&nbsp;	1. We don't need to make connection b/w database and front-end now, if the installation page is coming up it should be enough for now.

&nbsp;	2. The kubectl on jump\_host has been configured to work with the kubernetes cluster.

->

🧾 Task Documentation: Deploy Iron Gallery App and Iron DB on Kubernetes Cluster





🎯 Objective:

The Nautilus DevOps team developed and customized a web application called Iron Gallery.

The goal is to deploy both the frontend (Iron Gallery app) and backend (Iron DB) components on a Kubernetes cluster, each with their own configurations, volumes, and services.





🧱 Task Breakdown:

We will complete the following in sequence:

1]Create a Namespace

2]Deploy Iron Gallery Frontend (with volumes \& resource limits)

3]Deploy Iron DB Backend (with environment variables \& persistent storage)

4]Create Services for both applications

5]Verify all deployments and services





Step 1: Create Namespace

File Name: namespace.yml



apiVersion: v1

kind: Namespace

metadata:

&nbsp; name: iron-namespace-xfusion



Command:

kubectl apply -f namespace.yml





Verification:

kubectl get namespaces



✅ Expected Output:

iron-namespace-xfusion namespace appears in the list.







🌐 Step 2: Create Iron Gallery Deployment (Frontend)

File Name: iron-gallery-deployment.yml



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: iron-gallery-deployment-xfusion

&nbsp; namespace: iron-namespace-xfusion

&nbsp; labels:

&nbsp;   run: iron-gallery

spec:

&nbsp; replicas: 1

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     run: iron-gallery

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       run: iron-gallery

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: iron-gallery-container-xfusion

&nbsp;         image: kodekloud/irongallery:2.0

&nbsp;         resources:

&nbsp;           limits:

&nbsp;             memory: "100Mi"

&nbsp;             cpu: "50m"

&nbsp;         volumeMounts:

&nbsp;           - name: config

&nbsp;             mountPath: /usr/share/nginx/html/data

&nbsp;           - name: images

&nbsp;             mountPath: /usr/share/nginx/html/uploads

&nbsp;     volumes:

&nbsp;       - name: config

&nbsp;         emptyDir: {}

&nbsp;       - name: images

&nbsp;         emptyDir: {}





Command:

kubectl apply -f iron-gallery-deployment.yml





Verification:

kubectl get pods -n iron-namespace-xfusion

kubectl describe deployment iron-gallery-deployment-xfusion -n iron-namespace-xfusion





✅ Expected Output:

Pod iron-gallery-deployment-xfusion-xxxxx running successfully.

Resource limits and volume mounts applied correctly.





🧮 Step 3: Create Iron DB Deployment (Backend)

File Name: iron-db-deployment.yml



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: iron-db-deployment-xfusion

&nbsp; namespace: iron-namespace-xfusion

&nbsp; labels:

&nbsp;   db: mariadb

spec:

&nbsp; replicas: 1

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     db: mariadb

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       db: mariadb

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: iron-db-container-xfusion

&nbsp;         image: kodekloud/irondb:2.0

&nbsp;         env:

&nbsp;           - name: MYSQL\_DATABASE

&nbsp;             value: database\_host

&nbsp;           - name: MYSQL\_ROOT\_PASSWORD

&nbsp;             value: "rootpass123"

&nbsp;           - name: MYSQL\_PASSWORD

&nbsp;             value: "userpass456"

&nbsp;           - name: MYSQL\_USER

&nbsp;             value: "customuser"

&nbsp;         volumeMounts:

&nbsp;           - name: db

&nbsp;             mountPath: /var/lib/mysql

&nbsp;     volumes:

&nbsp;       - name: db

&nbsp;         emptyDir: {}





Command:

kubectl apply -f iron-db-deployment.yml





Verification:

kubectl get pods -n iron-namespace-xfusion

kubectl logs <iron-db-pod-name> -n iron-namespace-xfusion





✅ Expected Output:

Database container starts successfully and environment variables are correctly set.







Step 4: Create Iron DB Service

File Name: iron-db-service.yml



apiVersion: v1

kind: Service

metadata:

&nbsp; name: iron-db-service-xfusion

&nbsp; namespace: iron-namespace-xfusion

spec:

&nbsp; selector:

&nbsp;   db: mariadb

&nbsp; ports:

&nbsp;   - protocol: TCP

&nbsp;     port: 3306

&nbsp;     targetPort: 3306

&nbsp; type: ClusterIP





Command:

kubectl apply -f iron-db-service.yml





Verification:

kubectl get svc -n iron-namespace-xfusion





✅ Expected Output:

A ClusterIP service named iron-db-service-xfusion with port 3306.







🌍 Step 5: Create Iron Gallery Service

File Name: iron-gallery-service.yml



apiVersion: v1

kind: Service

metadata:

&nbsp; name: iron-gallery-service-xfusion

&nbsp; namespace: iron-namespace-xfusion

spec:

&nbsp; selector:

&nbsp;   run: iron-gallery

&nbsp; ports:

&nbsp;   - protocol: TCP

&nbsp;     port: 80

&nbsp;     targetPort: 80

&nbsp;     nodePort: 32678

&nbsp; type: NodePort





Command:

kubectl apply -f iron-gallery-service.yml





Verification:

kubectl get svc -n iron-namespace-xfusion





✅ Expected Output:

iron-gallery-service-xfusion service created with NodePort 32678.





You can now access the Iron Gallery application by visiting:

http://<Node-IP>:32678





🔍 Final Verification

kubectl get all -n iron-namespace-xfusion





✅ Expected Output:

• Two Deployments (iron-gallery, iron-db)

• Two Pods (one per deployment)

• Two Services (ClusterIP for DB, NodePort for frontend)

• Namespace listed as active





⚙️ Troubleshooting Notes



Issue	Cause	Fix

namespace already exists	Namespace was previously created	Continue using existing namespace

did not find expected key	YAML indentation error	Validate spacing using yamllint or editor

CrashLoopBackOff on DB pod	Incorrect env variables or passwords	Recheck DB env configuration

Service not accessible	Wrong NodePort or firewall	Verify NodePort and open port in cluster





🧠 Concepts Reinforced

&nbsp;	• Namespace isolation

&nbsp;	• Deployments with environment variables

&nbsp;	• Resource limits for CPU and memory

&nbsp;	• Volume management with emptyDir

&nbsp;	• Exposing Pods using ClusterIP and NodePort services







✅ Summary:

1]The Iron Gallery app and Iron DB were successfully deployed within the namespace iron-namespace-xfusion.

2]Both applications run independently, with persistent storage and networking configured for scalability and testing.

3]This setup lays the foundation for future integration between frontend and backend services.



