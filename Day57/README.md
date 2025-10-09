Print Environment Variables 



1]The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.

1\. Create a pod named print-envars-greeting.

2\. Configure spec as, the container name should be print-env-container and use bash image.

3\. Create three environment variables:

a. GREETING and its value should be Welcome to

b. COMPANY and its value should be Nautilus

c. GROUP and its value should be Group

4\. Use command \["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"'] (please use this exact command), also set its restartPolicy policy to Never to avoid crash loop back.

5\. You can check the output using kubectl logs -f print-envars-greeting command.

Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.





->



🧾 Task Name: Configure and Test Environment Variables Inside a Kubernetes Pod



Scenario:

The Nautilus DevOps team is preparing the environment for an upcoming application that sends greeting messages to users.



To validate the setup, a sample Pod needs to be created that uses environment variables and prints a custom greeting message when it runs.





🎯 Task Requirements:

1]Create a Pod named print-envars-greeting.

2]Container name: print-env-container.

3]Use the image: bash.

4]Configure 3 environment variables:



i]GREETING=Welcome to

ii]COMPANY=Nautilus

Iii]GROUP=Group





Use the command:

\["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']





Set restartPolicy: Never





Verify the output using:

kubectl logs -f print-envars-greeting





Implementation Steps:

Step 1: Create Pod YAML



Create a file named print-envars-greeting.yaml:



apiVersion: v1

kind: Pod

metadata:

&nbsp; name: print-envars-greeting

spec:

&nbsp; containers:

&nbsp;   - name: print-env-container

&nbsp;     image: bash

&nbsp;     command: \["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']

&nbsp;     env:

&nbsp;       - name: GREETING

&nbsp;         value: "Welcome to"

&nbsp;       - name: COMPANY

&nbsp;         value: "Nautilus"

&nbsp;       - name: GROUP

&nbsp;         value: "Group"

&nbsp; restartPolicy: Never





Step 2: Apply the YAML

kubectl apply -f print-envars-greeting.yaml





Step 3: Verify Pod Status

Check if the pod has completed successfully:

kubectl get pods



You should see the pod status as Completed.





Step 4: View the Output

Fetch the logs:

kubectl logs -f print-envars-greeting





✅ Expected Output:

Welcome to Nautilus Group





Concepts Learned:

1]Environment Variables in Pods: Used to pass configuration or dynamic data to containers.

2]Command Override: The command field in the Pod spec overrides the container image’s default entrypoint.

3]restartPolicy: Never: Ensures the pod won’t enter a CrashLoopBackOff when its command completes.

4]kubectl logs: Displays the standard output (stdout) from container execution.





🧾 Verification Checklist



Checkpoint	Command	Expected Result

Pod created	kubectl get pods	print-envars-greeting in Completed state

Logs checked	kubectl logs -f print-envars-greeting	Displays “Welcome to Nautilus Group”

Restart policy	Check YAML	restartPolicy: Never







