Manage Secrets in Kubernetes



1]The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

1\. We already have a secret key file media.txt under /opt location on jump host. Create a generic secret named media, it should contain the password/license-number present in media.txt file.

2\. Also create a pod named secret-nautilus.

3\. Configure pod's spec as container name should be secret-container-nautilus, image should be fedora with latest tag (remember to mention the tag with image). Use sleep command for container so that it remains in running state. Consume the created secret and mount it under /opt/apps within the container.

4\. To verify you can exec into the container secret-container-nautilus, to check the secret key under the mounted path /opt/apps. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.



Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.



->



🧠 Task: Store License Information Using Kubernetes Secrets



Scenario:

The Nautilus DevOps team needs to securely store license-based tool credentials inside the Kubernetes cluster. Kubernetes Secrets provide a secure way to store sensitive data such as passwords, API keys, or license numbers.





🎯 Objective:

1]Create a Secret named media from /opt/media.txt.

2]Create a Pod named secret-nautilus that mounts this secret at /opt/apps in the container.





🧩 Specifications:



Component	Details

Secret Name	media

Secret Source File	/opt/media.txt

Pod Name	secret-nautilus

Container Name	secret-container-nautilus

Image	fedora:latest

Mount Path	/opt/apps

Container Command	sleep 4800 (to keep running)





Implementation Steps:



Step 1: Create the Secret

kubectl create secret generic media --from-file=/opt/media.txt



Verify the secret:

kubectl get secrets

kubectl describe secret media





Step 2: Create the Pod YAML file

Create secret-nautilus.yml:



apiVersion: v1

kind: Pod

metadata:

  name: secret-nautilus

spec:

  containers:

    - name: secret-container-nautilus

      image: fedora:latest

      command: \["sleep", "4800"]

      volumeMounts:

        - name: secret-volume

          mountPath: /opt/apps

  volumes:

    - name: secret-volume

      secret:

        secretName: media





Step 3: Apply the Pod configuration



kubectl apply -f secret-nautilus.yml

kubectl get pods



✅ Wait until the pod is in Running state.





Step4: Verify the Secret in the Pod

kubectl exec -it secret-nautilus -- /bin/bash

cd /opt/apps

ls

cat media.txt



You should see the contents of the original media.txt file (password or license number).





✅ Outcome:

1]Secret media is securely stored in Kubernetes.

2]Pod secret-nautilus successfully mounts the secret at /opt/apps.

3]License/password is accessible inside the container only, ensuring security for sensitive tools.

