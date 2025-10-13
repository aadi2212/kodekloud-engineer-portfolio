Persistent Volumes in Kubernetes



1]The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:

1\. Create a PersistentVolume named as pv-xfusion. Configure the spec as storage class should be manual, set capacity to 5Gi, set access mode to ReadWriteOnce, volume type should be hostPath and set path to /mnt/security (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

2\. Create a PersistentVolumeClaim named as pvc-xfusion. Configure the spec as storage class should be manual, request 1Gi of the storage, set access mode to ReadWriteOnce.

3\. Create a pod named as pod-xfusion, mount the persistent volume you created with claim name pvc-xfusion at document root of the web server, the container within the pod should be named as container-xfusion using image httpd with latest tag only (remember to mention the tag i.e httpd:latest).

4\. Create a node port type service named web-xfusion using node port 30008 to expose the web server running within the pod.

Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.



->



Kubernetes Task: Deploy Web Application with Persistent Volume



Objective:



Deploy an HTTP web server on a Kubernetes cluster using a PersistentVolume (PV) and PersistentVolumeClaim (PVC) to store application files. Expose the web server externally using a NodePort Service.



Task Requirements:

1]PersistentVolume (PV)

• Name: pv-xfusion

• StorageClass: manual

• Capacity: 5Gi

• Access Mode: ReadWriteOnce

• Volume Type: hostPath

• Path: /mnt/security



2]PersistentVolumeClaim (PVC)

• Name: pvc-xfusion

• StorageClass: manual

• Requested Storage: 1Gi

• Access Mode: ReadWriteOnce





3]Pod

• Name: pod-xfusion

• Container Name: container-xfusion

• Image: httpd:latest

• Mount PVC to document root (/usr/local/apache2/htdocs)

• Label: app: xfusion





4]Service

• Name: web-xfusion

• Type: NodePort

• NodePort: 30008

• Selector: app: xfusion





Step-by-Step YAML Configurations:



1]PersistentVolume (pv-xfusion.yml)



apiVersion: v1

kind: PersistentVolume

metadata:

&nbsp; name: pv-xfusion

spec:

&nbsp; storageClassName: manual

&nbsp; capacity:

&nbsp;   storage: 5Gi

&nbsp; accessModes:

&nbsp;   - ReadWriteOnce

&nbsp; hostPath:

&nbsp;   path: /mnt/security





2]PersistentVolumeClaim (pvc-xfusion.yml)



apiVersion: v1

kind: PersistentVolumeClaim

metadata:

&nbsp; name: pvc-xfusion

spec:

&nbsp; storageClassName: manual

&nbsp; accessModes:

&nbsp;   - ReadWriteOnce

&nbsp; resources:

&nbsp;   requests:

&nbsp;     storage: 1Gi





3]Pod (pod-xfusion.yml)



apiVersion: v1

kind: Pod

metadata:

&nbsp; name: pod-xfusion

&nbsp; labels:

&nbsp;   app: xfusion

spec:

&nbsp; containers:

&nbsp;   - name: container-xfusion

&nbsp;     image: httpd:latest

&nbsp;     volumeMounts:

&nbsp;       - name: xfusion-volume

&nbsp;         mountPath: /usr/local/apache2/htdocs

&nbsp; volumes:

&nbsp;   - name: xfusion-volume

&nbsp;     persistentVolumeClaim:

&nbsp;       claimName: pvc-xfusion





4]NodePort Service (web-xfusion.yml)



apiVersion: v1

kind: Service

metadata:

&nbsp; name: web-xfusion

spec:

&nbsp; type: NodePort

&nbsp; selector:

&nbsp;   app: xfusion

&nbsp; ports:

&nbsp;   - protocol: TCP

&nbsp;     port: 80

&nbsp;     targetPort: 80

&nbsp;     nodePort: 30008





Deployment Steps:

1]Apply PV and PVC:

kubectl apply -f pv-xfusion.yml

kubectl apply -f pvc-xfusion.yml





2]Deploy the Pod:

kubectl apply -f pod-xfusion.yml





3]Deploy the NodePort Service:

kubectl apply -f web-xfusion.yml





4]Verify PV and PVC binding:

kubectl get pv,pvc





5]Verify Pod is running and ready:

kubectl get pod pod-xfusion -o wide





6]Add test HTML file inside the Pod:

kubectl exec -it pod-xfusion -- /bin/sh

echo '<h1>Hello Xfusion HTTPD!</h1>' > /usr/local/apache2/htdocs/index.html

exit





7]Access the web server via NodePort:

curl http://<NODE\_IP>:30008





Key Notes / Best Practices

&nbsp;	• PVC mount path must match the container’s document root. For httpd:latest, this is /usr/local/apache2/htdocs.

&nbsp;	• Always label Pods correctly to match Service selectors.

&nbsp;	• NodePort services must be accessed via the Node IP, not Pod IP.

&nbsp;	• PV mounted over the document root overrides default container content, so a test file must be added.

&nbsp;	• Ensure file permissions are readable by the web server (644 for files).







✅ Outcome:

• PV and PVC successfully bound.

• Pod pod-xfusion is running with httpd:latest.

• NodePort service web-xfusion exposes the web server on port 30008.

• Test HTML page served correctly from mounted PV.



