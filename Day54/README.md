1]We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.

1\. Create a pod named volume-share-datacenter.

2\. For the first container, use image fedora with latest tag only and remember to mention the tag i.e fedora:latest, container should be named as volume-container-datacenter-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/beta.

3\. For the second container, use image fedora with the latest tag only and remember to mention the tag i.e fedora:latest, container should be named as volume-container-datacenter-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/games.

4\. Volume name should be volume-share of type emptyDir.

5\. After creating the pod, exec into the first container i.e volume-container-datacenter-1, and just for testing create a file beta.txt with any content under the mounted path of first container i.e /tmp/beta.

6\. The file beta.txt should be present under the mounted path /tmp/games on the second container volume-container-datacenter-2 as well, since they are using a shared volume.



Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.



->





Kubernetes Task: Shared Volume Between Containers in a Pod



Task Name:



Volume Sharing in Multi-Container Pod





Objective:

Create a Kubernetes pod with two containers sharing an emptyDir volume, and verify that files created in one container are accessible in the other.





Pod Details:



Parameter	Value

Pod Name	volume-share-datacenter

Container 1	volume-container-datacenter-1

Container 2	volume-container-datacenter-2

Image	fedora:latest

Volume Name	volume-share

Volume Type	emptyDir

Mount Paths	/tmp/beta (Container 1), /tmp/games (Container 2)

Command	sleep infinity (keeps containers running)





Initial YAML Configuration (with Error):



apiVersion: v1

kind: Pod

metadata:

&nbsp; name: volume-share-datacenter

spec:

&nbsp; containers:

&nbsp;   - name: volume-container-datacenter-1

&nbsp;     image: fedora:latest

&nbsp;     command: \["sleep","infinity"]

&nbsp;     VolumeMounts:   # ❌ Incorrect capitalization

&nbsp;       - name: volume-share

&nbsp;         mountPath: /tmp/beta



&nbsp;   - name: volume-container-datacenter-2

&nbsp;     image: fedora:latest

&nbsp;     command: \["sleep","infinity"]

&nbsp;     VolumeMounts:   # ❌ Incorrect capitalization

&nbsp;       - name: volume-share

&nbsp;         mountPath: /tmp/games



&nbsp; volumes:

&nbsp;   - name:volume-share  # ❌ Missing space after colon

&nbsp;     emptyDir: {}





Error Received:

error parsing volume-share-datacenter.yml: 

error converting YAML to JSON: yaml: line 23: mapping values are not allowed in this context





Cause of Error:

1]VolumeMounts key is incorrectly capitalized → should be volumeMounts.

2]YAML is sensitive to indentation and spaces after colons.

3]Minor spacing issue in - name:volume-share → should be - name: volume-share.





Corrected YAML Configuration:



apiVersion: v1

kind: Pod

metadata:

&nbsp; name: volume-share-datacenter

spec:

&nbsp; containers:

&nbsp;   - name: volume-container-datacenter-1

&nbsp;     image: fedora:latest

&nbsp;     command: \["sleep","infinity"]

&nbsp;     volumeMounts:

&nbsp;       - name: volume-share

&nbsp;         mountPath: /tmp/beta



&nbsp;   - name: volume-container-datacenter-2

&nbsp;     image: fedora:latest

&nbsp;     command: \["sleep","infinity"]

&nbsp;     volumeMounts:

&nbsp;       - name: volume-share

&nbsp;         mountPath: /tmp/games



&nbsp; volumes:

&nbsp;   - name: volume-share

&nbsp;     emptyDir: {}





Steps to Execute:



1]Apply the corrected pod manifest:

kubectl apply -f volume-share-datacenter.yml





2]Verify pod is running:

kubectl get pods



3]Exec into first container and create a test file:

kubectl exec -it volume-share-datacenter -c volume-container-datacenter-1 -- bash

echo "This is a test file for shared volume" > /tmp/beta/beta.txt

ls -l /tmp/beta





4]Exec into second container and check file presence:

kubectl exec -it volume-share-datacenter -c volume-container-datacenter-2 -- bash

ls -l /tmp/games

cat /tmp/games/beta.txt





Expected Result:

1]File beta.txt created in /tmp/beta of container 1 appears in /tmp/games of container 2.

2]Confirms that emptyDir volume is shared between containers in the same pod.



Notes / Lessons Learned:

1]YAML is case-sensitive. Always use volumeMounts (not VolumeMounts).

2]Ensure proper indentation and spaces after colons.

3]emptyDir volume is ephemeral, it exists only while the pod is running.

4]This setup is useful for temporary data sharing between containers in the same pod.





