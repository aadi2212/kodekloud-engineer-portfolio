Execute Rolling Updates in Kubernetes



1]An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.17 with the latest updates.

Execute a rolling update for this application, integrating the nginx:1.17 image. The deployment is named nginx-deployment.

Ensure all pods are operational post-update.

Note: The kubectl utility on jump\_host is set up to operate with the Kubernetes cluster



->



Task Name: Rolling Update for Nginx Deployment



Objective:

Update the nginx-deployment to use the new image nginx:1.17 while ensuring zero downtime and all pods remain operational.





Steps Performed:

1]Verified Existing Deployment

kubectl get deployments

kubectl describe deployment nginx-deployment

Confirmed the deployment nginx-deployment exists and the current image in use.



2]Performed Rolling Update

kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17

Updated the container image to nginx:1.17 in a rolling update fashion.



3]Monitored Update Progress

kubectl rollout status deployment/nginx-deployment

Ensured all pods were updated successfully and became Ready.



4]Verified Updated Pods and Image

Kubectl get pods

Kubectl describe pod nginx-deployment

Confirmed all pods are running the new image nginx:1.17.



Outcome:

1]The nginx-deployment was successfully updated to nginx:1.17.

2]Rolling update ensured zero downtime.

3]All pods are operational post-update.



Benefits:

1]Deployment updates without disrupting the application.

2]Ensures new features and bug fixes from the updated image are live.



