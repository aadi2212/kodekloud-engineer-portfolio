Init Containers in Kubernetes



1]There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.

1\. Create a Deployment named as ic-deploy-devops.

2\. Configure spec as replicas should be 1, labels app should be ic-devops, template's metadata lables app should be the same ic-devops.

3\. The initContainers should be named as ic-msg-devops, use image ubuntu with latest tag and use command '/bin/bash', '-c' and 'echo Init Done - Welcome to xFusionCorp Industries > /ic/official'. The volume mount should be named as ic-volume-devops and mount path should be /ic.

4\. Main container should be named as ic-main-devops, use image ubuntu with latest tag and use command '/bin/bash', '-c' and 'while true; do cat /ic/official; sleep 5; done'. The volume mount should be named as ic-volume-devops and mount path should be /ic.

5\. Volume to be named as ic-volume-devops and it should be an emptyDir type.



Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.



->



Kubernetes Task: Using Init Containers in a Deployment



Objective:

Deploy an application on Kubernetes using init containers to perform pre-configuration tasks before starting the main application container. The goal is to ensure configuration files or prerequisites are set up in shared volumes before the main container starts.



Task Details:

• Deployment Name: ic-deploy-devops

• Replicas: 1

• Labels: app=ic-devops

• Init Container Name: ic-msg-devops

• Main Container Name: ic-main-devops

• Volume Name: ic-volume-devops (emptyDir)

• Volume Mount Path: /ic

• Init Container Function: Writes a message to /ic/official

• Main Container Function: Reads and prints the message from /ic/official every 5 seconds





Step 1: Deployment YAML



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: ic-deploy-devops

spec:

&nbsp; replicas: 1

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: ic-devops

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: ic-devops

&nbsp;   spec:

&nbsp;     initContainers:

&nbsp;       - name: ic-msg-devops

&nbsp;         image: ubuntu:latest

&nbsp;         command: \["/bin/bash", "-c", "echo Init Done - Welcome to xFusionCorp Industries > /ic/official"]

&nbsp;         volumeMounts:

&nbsp;           - name: ic-volume-devops

&nbsp;             mountPath: /ic

&nbsp;     containers:

&nbsp;       - name: ic-main-devops

&nbsp;         image: ubuntu:latest

&nbsp;         command: \["/bin/bash", "-c", "while true; do cat /ic/official; sleep 5; done"]

&nbsp;         volumeMounts:

&nbsp;           - name: ic-volume-devops

&nbsp;             mountPath: /ic

&nbsp;     volumes:

&nbsp;       - name: ic-volume-devops

&nbsp;         emptyDir: {}







Step 2: Apply the Deployment

kubectl apply -f ic-deploy-devops.yml





✅ Expected Output:

deployment.apps/ic-deploy-devops created





Step 3: Verify Pod Status

kubectl get pods

You should see a pod running with a name like ic-deploy-devops-<hash>

Status should be 1/1 Running





Step 4: Verify Init Container Execution

kubectl logs <pod-name>





Expected output:

Init Done - Welcome to xFusionCorp Industries



The message should repeat every 5 seconds as per the main container command.





Step 5: Verify Shared Volume

kubectl exec -it <pod-name> -- cat /ic/official



✅ Output:

Init Done - Welcome to xFusionCorp Industries

This confirms that the init container successfully created the file, and the main container is reading it.





Key Concepts Learned:

1]Init Containers: Run before the main container to perform setup tasks.

2]emptyDir Volumes: Share temporary storage between init containers and main containers.

3]VolumeMounts: Correctly mount volumes inside containers.

4]Automation Compliance: Naming and spec must match exact task requirements for automated graders.





Common Issues \& Fixes:



Issue	Cause	Fix

unknown field initcontainers	Wrong case	Use initContainers (capital C)

unknown field volumeMount	Wrong key	Use volumeMounts (plural)

Pod logs empty / file not found	Volume not shared or mounted incorrectly	Ensure volumeMounts point to the same emptyDir volume in both containers

Task failed in automated checker	Names mismatch	Use exact names: ic-msg-devops and ic-main-devops



Conclusion:

This task demonstrates how init containers can be used for pre-deployment configuration in Kubernetes. By using emptyDir volumes, the setup ensures that data created by the init container is available to the main container.





✅ Outcome: Task completed successfully.



