Troubleshoot Deployment issues in Kubernetes



1]Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.



The deployment name is redis-deployment. The pods are not in running state right now, so please look into the issue and fix the same.



Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.



->



🧾 Task Name: Fix Redis Deployment on Kubernetes Cluster



Scenario:



The Nautilus DevOps team had deployed a Redis application (redis-deployment) on the Kubernetes cluster. The application was running fine, but recent changes caused the pods to fail. The task was to identify the issue and restore the Redis pods to a running state.





Initial Observation:

Check the pods:

kubectl get pods



NAME	STATUS

redis-deployment-54cdf4f76d-tb5zq	Pending

redis-deployment-5bcd4c7d64-8z44n	ContainerCreating / ImagePullBackOff





Errors Identified:

1.ConfigMap Not Found

MountVolume.SetUp failed for volume "config" : configmap "redis-conig" not found



1]The Deployment was trying to mount a ConfigMap named redis-conig (typo).

2]This caused pods to remain in Pending state.



2.Image Pull Error

ImagePullBackOff

One pod failed because the container image was incorrectly specified as redis:alpin.

Correct image should be redis:alpine.



Issue	Cause

Pod Pending / ContainerCreating	Deployment referenced a non-existent ConfigMap (redis-conig).

Pod ImagePullBackOff	Typo in container image name (redis:alpin instead of redis:alpine).





Fix Steps:

1]Inspect Deployment YAML

kubectl get deployment redis-deployment -o yaml



Check the volumes section:

volumes:

\- name: config

&nbsp; configMap:

&nbsp;   name: redis-conig   # ❌ typo





Check the container image:

containers:

\- name: redis-container

&nbsp; image: redis:alpin   # ❌ typo





2]Edit Deployment to fix issues

kubectl edit deployment redis-deployment



Fix ConfigMap name:

volumes:

\- name: config

&nbsp; configMap:

&nbsp;   name: redis-config   # ✅ corrected





Fix container image:

containers:

\- name: redis-container

&nbsp; image: redis:alpine   # ✅ corrected





3]Delete old pods to apply changes

kubectl delete pod -l app=redis



Deployment will automatically create new pods with corrected configuration.





4]Verify pods are running

kubectl get pods

kubectl rollout status deployment/redis-deployment



All pods should now be Running.





5]Check pod logs (optional)

kubectl logs <pod-name>





Redis should start normally:

\* Ready to accept connections



