Set Resource Limits in Kubernetes Pods



1]

The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:

Create a pod named httpd-pod with a container named httpd-container. Use the httpd image with the latest tag (specify as httpd:latest). Set the following resource limits:

Requests: Memory: 15Mi, CPU: 100m

Limits: Memory: 20Mi, CPU: 100m

Note: The kubectl utility on jump\_host is configured to operate with the Kubernetes cluster.



->



Task Name: Configure Resource Limits for Kubernetes Pod

Objective:

The Nautilus DevOps team noticed performance issues in Kubernetes applications due to unbounded resource utilization. The goal was to create a Pod with explicit resource requests and limits to optimize cluster performance.



Steps Performed:

&nbsp;	1. Created Pod Manifest File

&nbsp;		○ File name: httpd-pod.yml

&nbsp;		○ Initial configuration attempt:

apiVersion: v1

Kind: Pod    # ❌ Incorrect (uppercase "K")

metadata:

&nbsp; name: httpd-pod

spec:

&nbsp; containers:

&nbsp;   - name: httpd-container

&nbsp;     image: httpd:latest

&nbsp;     resources:

&nbsp;       requests:

&nbsp;         memory: "15Mi"

&nbsp;         cpu: "100m"

&nbsp;       limits:

&nbsp;         memory: "20Mi"

&nbsp;         cpu: "100m"

&nbsp;		

&nbsp;	2. Error Encountered

When applying the above YAML:

kubectl apply -f httpd-pod.yml



Error message:

error: error validating "httpd-pod.yml": error validating data: kind not set;

if you choose to ignore these errors, turn validation off with --validate=false

&nbsp;		○ Root Cause: Kubernetes is case-sensitive. The field must be kind (lowercase), not Kind.

&nbsp;		

&nbsp;	3. Fixed the Manifest

Corrected YAML:

apiVersion: v1

kind: Pod

metadata:

&nbsp; name: httpd-pod

spec:

&nbsp; containers:

&nbsp;   - name: httpd-container

&nbsp;     image: httpd:latest

&nbsp;     resources:

&nbsp;       requests:

&nbsp;         memory: "15Mi"

&nbsp;         cpu: "100m"

&nbsp;       limits:

&nbsp;         memory: "20Mi"

&nbsp;         cpu: "100m"

&nbsp;	

&nbsp;	4. Applied the Corrected Configuration

kubectl apply -f httpd-pod.yml

&nbsp;	

&nbsp;	5. Verification

&nbsp;		○ Checked pod creation:

kubectl get pods

&nbsp;		○ Validated resource limits:

kubectl describe pod httpd-pod | grep -A5 "Limits"



Outcome:

&nbsp;	• Pod httpd-pod was successfully created.

&nbsp;	• Resource requests and limits applied correctly.

&nbsp;	• Error was resolved by correcting the YAML kind field.

&nbsp;	

Benefits:

&nbsp;	• Ensures the httpd container runs within controlled CPU and memory boundaries.

&nbsp;	• Improves stability of other workloads in the cluster by preventing resource hogging.

&nbsp;	



