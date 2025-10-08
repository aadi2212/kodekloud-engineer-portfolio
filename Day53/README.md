🧾 Task Name: Investigate and Fix Nginx–PHP-FPM Issue in Kubernetes



Scenario:

The Nautilus DevOps team encountered an issue with an Nginx and PHP-FPM multi-container Pod running in Kubernetes. The application stopped serving the PHP page correctly. The task was to identify and fix the root cause, and then verify that the website loads successfully.





Pod \& ConfigMap Details

&nbsp;	• Pod name: nginx-phpfpm

&nbsp;	• ConfigMap name: nginx-config

&nbsp;	• Containers:

&nbsp;		○ nginx-container → Nginx web server

&nbsp;		○ php-fpm-container → PHP processor





Root Cause Analysis:

The issue occurred because the two containers were using different document root paths mounted from the shared volume:



Container	Mount Path	Volume Name

php-fpm-container	/var/www/html	shared-files

nginx-container	/usr/share/nginx/html	shared-files



Due to this mismatch:

1]Nginx was serving files from /usr/share/nginx/html (empty folder).

2]PHP-FPM was writing files to /var/www/html.

3]Hence, Nginx couldn’t find the PHP file, resulting in 404 or 403 errors.



This mismatch in mount paths was the main reason the task failed multiple times.



Solution Steps:



1]Export the Pod definition

kubectl get pod nginx-phpfpm -o yaml > /home/thor/definition.yml



2]Edit the Pod YAML

vi /home/thor/definition.yml



Locate the nginx-container section and change:

\- mountPath: /usr/share/nginx/html

&nbsp; name: shared-files



Change to:

\- mountPath: /var/www/html

&nbsp; name: shared-files





Ensure both containers now have:

volumeMounts:

&nbsp; - mountPath: /var/www/html

&nbsp;   name: shared-files





3]Recreate the Pod

Since volumeMounts are immutable, delete and recreate the pod:



kubectl delete pod nginx-phpfpm

kubectl apply -f /home/thor/definition.yml





4]Verify Pod status

kubectl get pods

Ensure both containers are in Running state.





5]Copy PHP file into Nginx document root

kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container



Verify:

kubectl exec -it nginx-phpfpm -c nginx-container -- ls /var/www/html





6] Test Website

Click the Website button in the lab interface.

✅ The PHP page should load successfully.





Key Learnings:

1]Kubernetes Pods cannot be edited directly for immutable fields like volumeMounts or volumes.

→ Must delete and recreate the Pod with corrected YAML.



2]Always verify shared volume mount paths between containers when debugging multi-container setups.



3]Consistency between Nginx’s root directive and PHP-FPM’s document root is crucial.





✅ Final Outcome:

The website was restored successfully after correcting the shared volume mount path in the Nginx container.

Task marked as Completed ✅.





