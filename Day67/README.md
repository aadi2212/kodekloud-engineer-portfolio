Deploy Guest Book app On Kubernetes



1]The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.

BACK-END TIER

1\. Create a deployment named redis-master for Redis master.

a.) Replicas count should be 1.

b.) Container name should be master-redis-devops and it should use image redis.

c.) Request resources as CPU should be 100m and Memory should be 100Mi.

d.) Container port should be redis default port i.e 6379.

2\. Create a service named redis-master for Redis master. Port and targetPort should be Redis default port i.e 6379.

3\. Create another deployment named redis-slave for Redis slave.

a.) Replicas count should be 2.

b.) Container name should be slave-redis-devops and it should use gcr.io/google\_samples/gb-redisslave:v3 image.

c.) Requests resources as CPU should be 100m and Memory should be 100Mi.

d.) Define an environment variable named GET\_HOSTS\_FROM and its value should be dns.

e.) Container port should be Redis default port i.e 6379.

4\. Create another service named redis-slave. It should use Redis default port i.e 6379.

FRONT END TIER

5\. Create a deployment named frontend.

a.) Replicas count should be 3.

b.) Container name should be php-redis-devops and it should use gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff image.

c.) Request resources as CPU should be 100m and Memory should be 100Mi.

d.) Define an environment variable named as GET\_HOSTS\_FROM and its value should be dns.

e.) Container port should be 80.

6\. Create a service named frontend. Its type should be NodePort, port should be 80 and its nodePort should be 30009.

Finally, you can check the guestbook app by clicking on App button.

You can use any labels as per your choice.

Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.



->



Guestbook Application Deployment on Kubernetes



Task Overview:



Deploy the Guestbook application with a 3-tier architecture:

• Back-end Tier → Redis Master and Redis Slave

• Front-end Tier → PHP frontend





Requirements:

1]Proper replicas, container names, resource requests, ports, and environment variables.

2]NodePort service for frontend access.





1️⃣ Redis Master:



Deployment:



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: redis-master

spec:

&nbsp; replicas: 1

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: redis-master

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: redis-master

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: master-redis-devops

&nbsp;         image: redis

&nbsp;         resources:

&nbsp;           requests:

&nbsp;             cpu: "100m"

&nbsp;             memory: "100Mi"

&nbsp;         ports:

&nbsp;           - containerPort: 6379





Service:



apiVersion: v1

kind: Service

metadata:

&nbsp; name: redis-master

spec:

&nbsp; selector:

&nbsp;   app: redis-master

&nbsp; ports:

&nbsp;   - port: 6379

&nbsp;     targetPort: 6379





2️⃣ Redis Slave

Deployment:



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: redis-slave

spec:

&nbsp; replicas: 2

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: redis-slave

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: redis-slave

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: slave-redis-devops

&nbsp;         image: gcr.io/google-samples/gb-redisslave:v3

&nbsp;         resources:

&nbsp;           requests:

&nbsp;             cpu: "100m"

&nbsp;             memory: "100Mi"

&nbsp;         env:

&nbsp;           - name: GET\_HOSTS\_FROM

&nbsp;             value: dns

&nbsp;         ports:

&nbsp;           - containerPort: 6379





Service:



apiVersion: v1

kind: Service

metadata:

&nbsp; name: redis-slave

spec:

&nbsp; selector:

&nbsp;   app: redis-slave

&nbsp; ports:

&nbsp;   - port: 6379

&nbsp;     targetPort: 6379





3️⃣ Frontend

Deployment:



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: frontend

spec:

&nbsp; replicas: 3

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: frontend

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: frontend

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: php-redis-devops

&nbsp;         image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff

&nbsp;         ports:

&nbsp;           - containerPort: 80

&nbsp;         resources:

&nbsp;           requests:

&nbsp;             cpu: "100m"

&nbsp;             memory: "100Mi"

&nbsp;         env:

&nbsp;           - name: GET\_HOSTS\_FROM

&nbsp;             value: dns





Service (NodePort):



apiVersion: v1

kind: Service

metadata:

&nbsp; name: frontend

spec:

&nbsp; type: NodePort

&nbsp; selector:

&nbsp;   app: frontend

&nbsp; ports:

&nbsp;   - port: 80

&nbsp;     targetPort: 80

&nbsp;     nodePort: 30009





4️⃣ Deployment Steps

1\. Apply all resources:



kubectl apply -f redis-master-deployment.yaml

kubectl apply -f redis-master-service.yaml

kubectl apply -f redis-slave-deployment.yaml

kubectl apply -f redis-slave-service.yaml

kubectl apply -f frontend-deployment.yaml

kubectl apply -f frontend-service.yaml





2\. Verify pods and services:

kubectl get pods

kubectl get svc





3\. Access frontend:

http://<node-ip>:30009





5️⃣ Issues \& Resolution

• Issue: Frontend deployment selector typo (fronend instead of frontend)

• Impact: Pods were not selected by deployment → service couldn’t route traffic → app inaccessible

• Resolution: Fixed selector to match pod labels exactly

• All pods configured with resource requests (CPU 100m, Memory 100Mi) for cluster efficiency.

• Environment variable GET\_HOSTS\_FROM=dns ensures proper backend discovery.





6️⃣ Outcome



• Guestbook application deployed successfully.

• Redis master and slaves running and discoverable.

• Frontend exposed via NodePort (30009) and accessible.

• Task completed successfully without errors.





