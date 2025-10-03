Revert Deployment to Pervious Version in Kubernetes



1]Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.

There exists a deployment named nginx-deployment; initiate a rollback to the previous revision.

Note: The kubectl utility on jump\_host is configured to interact with the Kubernetes cluster.



->



Task: Rollback Kubernetes Deployment



Scenario:

The Nautilus DevOps team deployed a new release of an application using the nginx-deployment. After the release, a customer reported a bug. The requirement is to rollback the deployment to the previous version.



Steps Performed

1\. Check Deployment Rollout History

kubectl rollout history deployment nginx-deployment



Displays revision history of the deployment.

Confirms that a previous version exists to rollback to.





2\. Rollback to the Previous Revision

kubectl rollout undo deployment nginx-deployment

Reverts the deployment to the last working revision.



3\. Verify Rollout Status

kubectl rollout status deployment nginx-deployment



Ensures rollback completed successfully.

Waits until all pods are updated and running.



4\. Validate Deployment and Pods

kubectl get pods 

kubectl describe deployment nginx-deployment



• Confirms pods are running the previous version of the image.

• Ensures application is back to a stable state.



Outcome:

1]The nginx-deployment was successfully rolled back to the previous revision.

2]All pods are running with the old stable version.

3]The application is restored and the bug introduced in the latest release is avoided.







