Deploy MYSQL on Kubernetes



1]A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:



1.) Create a PersistentVolume mysql-pv, its capacity should be 250Mi, set other parameters as per your preference.



2.) Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as mysql-pv-claim and request a 250Mi of storage. Set other parameters as per your preference.



3.) Create a deployment named mysql-deployment, use any mysql image as per your preference. Mount the PersistentVolume at mount path /var/lib/mysql.



4.) Create a NodePort type service named mysql and set nodePort to 30007.



5.) Create a secret named mysql-root-pass having a key pair value, where key is password and its value is YUIidhb667, create another secret named mysql-user-pass having some key pair values, where frist key is username and its value is kodekloud\_aim, second key is password and value is GyQkFRVNr3, create one more secret named mysql-db-url, key name is database and value is kodekloud\_db10



6.) Define some Environment variables within the container:



a) name: MYSQL\_ROOT\_PASSWORD, should pick value from secretKeyRef name: mysql-root-pass and key: password



b) name: MYSQL\_DATABASE, should pick value from secretKeyRef name: mysql-db-url and key: database



c) name: MYSQL\_USER, should pick value from secretKeyRef name: mysql-user-pass key key: username



d) name: MYSQL\_PASSWORD, should pick value from secretKeyRef name: mysql-user-pass and key: password



Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.



->





Task: Deploy MySQL on Kubernetes Cluster



Objective:

Deploy a MySQL server on Kubernetes with persistent storage, secrets, environment variables, and a NodePort service for external access.





Requirements:

1]PersistentVolume: mysql-pv with 250Mi storage.

2]PersistentVolumeClaim: mysql-pv-claim requesting 250Mi.

3]Deployment: mysql-deployment using mysql:8.0 image, mount PVC at /var/lib/mysql.

4]Secrets:



1]mysql-root-pass → key: password → value: YUIidhb667

2]mysql-user-pass → keys: username: kodekloud\_aim, password: GyQkFRVNr3

3]mysql-db-url → key: database → value: kodekloud\_db10



5]Environment Variables in Deployment:



1]MYSQL\_ROOT\_PASSWORD → from secret mysql-root-pass

2]MYSQL\_DATABASE → from secret mysql-db-url

3]MYSQL\_USER → from secret mysql-user-pass

4]MYSQL\_PASSWORD → from secret mysql-user-pass



6]NodePort Service: mysql exposing port 3306 on nodePort 30007.







Step 1: Create Secrets



apiVersion: v1

kind: Secret

metadata:

&nbsp; name: mysql-root-pass

type: Opaque

stringData:

&nbsp; password: YUIidhb667

---

apiVersion: v1

kind: Secret

metadata:

&nbsp; name: mysql-user-pass

type: Opaque

stringData:

&nbsp; username: kodekloud\_aim

&nbsp; password: GyQkFRVNr3

---

apiVersion: v1

kind: Secret

metadata:

&nbsp; name: mysql-db-url

type: Opaque

stringData:

&nbsp; database: kodekloud\_db10





Apply:

kubectl apply -f mysql-secrets.yaml





Error Faced:

Initial mysql-db-url.yml was:



stringData:

&nbsp; key: database

&nbsp; value: kodekloud\_db10





Issue: Kubernetes Secret does not accept key/value. Deployment failed with:

Resolution: Corrected the YAML to map database directly to its value.





Step 2: Create PersistentVolume



apiVersion: v1

kind: PersistentVolume

metadata:

&nbsp; name: mysql-pv

spec:

&nbsp; capacity:

&nbsp;   storage: 250Mi

&nbsp; accessModes:

&nbsp;   - ReadWriteOnce

&nbsp; persistentVolumeReclaimPolicy: Retain

&nbsp; hostPath:

&nbsp;   path: /mnt/data/mysql



kubectl apply -f mysql-pv.yaml





Step 3: Create PersistentVolumeClaim



apiVersion: v1

kind: PersistentVolumeClaim

metadata:

&nbsp; name: mysql-pv-claim

spec:

&nbsp; accessModes:

&nbsp;   - ReadWriteOnce

&nbsp; resources:

&nbsp;   requests:

&nbsp;     storage: 250Mi



kubectl apply -f mysql-pvc.yaml







Step 4: Create Deployment



apiVersion: apps/v1

kind: Deployment

metadata:

&nbsp; name: mysql-deployment

spec:

&nbsp; replicas: 1

&nbsp; selector:

&nbsp;   matchLabels:

&nbsp;     app: mysql

&nbsp; template:

&nbsp;   metadata:

&nbsp;     labels:

&nbsp;       app: mysql

&nbsp;   spec:

&nbsp;     containers:

&nbsp;       - name: mysql-container

&nbsp;         image: mysql:8.0

&nbsp;         ports:

&nbsp;           - containerPort: 3306

&nbsp;         env:

&nbsp;           - name: MYSQL\_ROOT\_PASSWORD

&nbsp;             valueFrom:

&nbsp;               secretKeyRef:

&nbsp;                 name: mysql-root-pass

&nbsp;                 key: password

&nbsp;           - name: MYSQL\_DATABASE

&nbsp;             valueFrom:

&nbsp;               secretKeyRef:

&nbsp;                 name: mysql-db-url

&nbsp;                 key: database

&nbsp;           - name: MYSQL\_USER

&nbsp;             valueFrom:

&nbsp;               secretKeyRef:

&nbsp;                 name: mysql-user-pass

&nbsp;                 key: username

&nbsp;           - name: MYSQL\_PASSWORD

&nbsp;             valueFrom:

&nbsp;               secretKeyRef:

&nbsp;                 name: mysql-user-pass

&nbsp;                 key: password

&nbsp;         volumeMounts:

&nbsp;           - name: mysql-storage

&nbsp;             mountPath: /var/lib/mysql

&nbsp;     volumes:

&nbsp;       - name: mysql-storage

&nbsp;         persistentVolumeClaim:

&nbsp;           claimName: mysql-pv-claim





kubectl apply -f mysql-deployment.yml







Errors Faced:

&nbsp;	1. CreateContainerConfigError → caused by typos: valueForm instead of valueFrom, wrong secret keys, wrong PVC reference claimname.

&nbsp;	2. Fixed all typos, corrected secret names and PVC claimName.







Step 5: Create NodePort Service



apiVersion: v1

kind: Service

metadata:

&nbsp; name: mysql

spec:

&nbsp; type: NodePort

&nbsp; selector:

&nbsp;   app: mysql

&nbsp; ports:

&nbsp;   - port: 3306

&nbsp;     targetPort: 3306

&nbsp;     nodePort: 30007



kubectl apply -f mysql-service.yml







Step 6: Verify Deployment



kubectl get pods

kubectl get pvc

kubectl get pv

kubectl get svc mysql

kubectl describe pod mysql-deployment-<pod-name>





✅ Expected Results:



• Pod: Running

• PVC: Bound

• NodePort: 30007

• MySQL container environment variables correctly populated from secrets.







Lessons Learned / Key Points

&nbsp;	1. Kubernetes Secrets are key-sensitive; stringData must directly map keys to values.

&nbsp;	2. valueFrom must be spelled correctly; valueForm is invalid.

&nbsp;	3. PVC claimName is case-sensitive and must be properly nested under volumes.

&nbsp;	4. YAML indentation errors are the most common cause of CreateContainerConfigError.

&nbsp;	5. Always verify secrets with:

&nbsp;          kubectl describe secret <secret-name>



&nbsp;	6. Use kubectl describe pod and kubectl logs to troubleshoot pod startup errors.



