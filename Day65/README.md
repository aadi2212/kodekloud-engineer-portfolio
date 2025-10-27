Deploy Redis Deployment in Kubernetes



1]The Nautilus application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:

Create a redis deployment with following parameters:

1\. Create a config map called my-redis-config having maxmemory 2mb in redis-config.

2\. Name of the deployment should be redis-deployment, it should use

redis:alpine image and container name should be redis-container. Also make sure it has only 1 replica.

3\. The container should request for 1 CPU.

4\. Mount 2 volumes:

a. An Empty directory volume called data at path /redis-master-data.

b. A configmap volume called redis-config at path /redis-master.

c. The container should expose the port 6379.

5\. Finally, redis-deployment should be in an up and running state.

Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.





->



✅ Task Name: Deploy Redis on Kubernetes Cluster for Testing





🎯 Objective:

The Nautilus DevOps team needed to deploy a Redis caching service on the Kubernetes cluster to improve application performance. This deployment will be used for testing before moving to production.





Step-by-Step Implementation:



1\. Create a ConfigMap for Redis configuration

We’ll define maxmemory to limit Redis memory usage.



apiVersion: v1

kind: ConfigMap

metadata:

&nbsp; name: my-redis-config

data:

&nbsp; redis-config: |

&nbsp;   maxmemory 2mb







2\. Create the Redis Deployment

This deployment will use the redis:alpine image, mount two volumes (ConfigMap + EmptyDir), and expose port 6379.



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: redis-deployment

spec:

&nbsp; replicas: 1

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: redis

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: redis

&nbsp;   spec:

&nbsp;     containers:

&nbsp;     - name: redis-container

&nbsp;       image: redis:alpine

&nbsp;       ports:

&nbsp;       - containerPort: 6379

&nbsp;       resources:

&nbsp;         requests:

&nbsp;           cpu: "1"

&nbsp;       volumeMounts:

&nbsp;       - name: data

&nbsp;         mountPath: /redis-master-data

&nbsp;       - name: redis-config

&nbsp;         mountPath: /redis-master

&nbsp;     volumes:

&nbsp;     - name: data

&nbsp;       emptyDir: {}

&nbsp;     - name: redis-config

&nbsp;       configMap:

&nbsp;         name: my-redis-config

&nbsp;         items:

&nbsp;         - key: redis-config

&nbsp;           path: redis.conf





3\. Apply the YAML Files

kubectl apply -f redis-configmap.yml

kubectl apply -f redis-deployment.yml







4\. Verify the Deployment

Check if the pod is running:

kubectl get pods





You should see output similar to:

NAME                                READY   STATUS    RESTARTS   AGE

redis-deployment-7d4b8d7d5f-b5k6r   1/1     Running   0          1m





Inspect pod details and verify volumes and configuration:

kubectl describe pod <pod-name>







🧠 Key Notes:

• ConfigMap: Holds Redis memory configuration (maxmemory 2mb).

• EmptyDir Volume: Used for temporary Redis data storage during pod lifetime.

• ConfigMap Volume: Mounts Redis configuration file to /redis-master.

• Resource Requests: Ensures the pod gets at least 1 CPU.

• Port 6379: Default Redis communication port.



