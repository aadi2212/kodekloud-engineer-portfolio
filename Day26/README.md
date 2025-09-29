Git Manage Remotes



1]The xFusionCorp development team added updates to the project that is maintained under /opt/ecommerce.git repo and cloned under /usr/src/kodekloudrepos/ecommerce. Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on /usr/src/kodekloudrepos/ecommerce repository as per details mentioned below: 





a. In /usr/src/kodekloudrepos/ecommerce repo add a new remote dev\_ecommerce and point it to /opt/xfusioncorp\_ecommerce.git repository.





b. There is a file /tmp/index.html on same server; copy this file to the repo and add/commit to master branch. 





c. Finally push master branch to this new remote origin.



->



Task: Git Remote Update in Ecommerce Repo



Task Details:

1]Git repo to update (local clone): /usr/src/kodekloudrepos/ecommerce

2]New remote to add: dev\_ecommerce → /opt/xfusioncorp\_ecommerce.git

3]Additional requirement: Copy /tmp/index.html into repo, commit it, and push to new remote (master branch).

4]Server: Storage Server (Stratos DC)





Steps Performed:

1]SSH into Storage Server

ssh natasha@ststor01



2]Navigate to ecommerce repo

cd /usr/src/kodekloudrepos/ecommerce



3]Add new remote

Initially, a typo (dev\_ecommerece) caused errors.

Fixed by removing and re-adding remote:



git remote remove dev\_ecommerece

git remote add dev\_ecommerce /opt/xfusioncorp\_ecommerce.git

git remote -v



Verified correct output:

dev\_ecommerce  /opt/xfusioncorp\_ecommerce.git (fetch)

dev\_ecommerce  /opt/xfusioncorp\_ecommerce.git (push)

origin         /opt/ecommerce.git (fetch)

origin         /opt/ecommerce.git (push)





4]Copy the file and commit

cp /tmp/index.html .

git add index.html

git commit -m "Add index.html file"



5]Safe.directory issue

Faced error:



fatal: detected dubious ownership in repository at '/usr/src/kodekloudrepos/ecommerce'



Tried:

git config --global --add safe.directory /usr/src/kodekloudrepos/ecommerce



Didn’t resolve (repo owned by root).

Finally used sudo git push ... which worked.



6]Push changes to new remote

sudo git push dev\_ecommerce master



Errors Faced \& Fixes:

Error 1: Wrong remote name

Cause: Typo (dev\_ecommerece instead of dev\_ecommerce).

Fix: Removed and re-added correctly.



Error 2: Dubious ownership

Cause: Repo owned by root, user natasha flagged by Git security.

Fix: Attempted safe.directory, but final resolution was using sudo git push.





Final Outcome:

✅index.html added and committed to master branch.

✅ Successfully pushed to new remote dev\_ecommerce (/opt/xfusioncorp\_ecommerce.git).

✅ Task completed successfully.



