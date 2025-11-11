Set Resource Limits in Kubernetes Pods

1]The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:
Create a pod named httpd-pod with a container named httpd-container. Use the httpd image with the latest tag (specify as httpd:latest). Set the following resource limits:
Requests: Memory: 15Mi, CPU: 100m
Limits: Memory: 20Mi, CPU: 100m
Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

->

Task Name: Configure Resource Limits for Kubernetes Pod
Objective:
The Nautilus DevOps team noticed performance issues in Kubernetes applications due to unbounded resource utilization. The goal was to create a Pod with explicit resource requests and limits to optimize cluster performance.

Steps Performed:
	1. Created Pod Manifest File
		○ File name: httpd-pod.yml
		○ Initial configuration attempt:
apiVersion: v1
Kind: Pod    # ❌ Incorrect (uppercase "K")
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"
		
	2. Error Encountered
When applying the above YAML:
kubectl apply -f httpd-pod.yml

Error message:
error: error validating "httpd-pod.yml": error validating data: kind not set;
if you choose to ignore these errors, turn validation off with --validate=false
		○ Root Cause: Kubernetes is case-sensitive. The field must be kind (lowercase), not Kind.
		
	3. Fixed the Manifest
Corrected YAML:
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"
	
	4. Applied the Corrected Configuration
kubectl apply -f httpd-pod.yml
	
	5. Verification
		○ Checked pod creation:
kubectl get pods
		○ Validated resource limits:
kubectl describe pod httpd-pod | grep -A5 "Limits"

Outcome:
	• Pod httpd-pod was successfully created.
	• Resource requests and limits applied correctly.
	• Error was resolved by correcting the YAML kind field.
	
Benefits:
	• Ensures the httpd container runs within controlled CPU and memory boundaries.
	• Improves stability of other workloads in the cluster by preventing resource hogging.
	
Acknowledgement:
Thanks to KodeKloud for providing practical DevOps challenges that simulate real-world issues and error troubleshooting.
<img width="912" height="1963" alt="image" src="https://github.com/user-attachments/assets/f44eba4f-6d74-47d2-9d50-33f0547e193e" />



