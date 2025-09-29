Git Merge Branches



1]

The Nautilus application development team has been working on a project repository /opt/news.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:





Create a new branch xfusion in /usr/src/kodekloudrepos/news repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.



->



Steps to Complete the Task:

1]Log in to the Storage Server.

&nbsp;ssh natasha@172.16.238.15



2]Navigate to the repository.

&nbsp;sudo cd /usr/src/kodekloudrepos/news



3]Ensure repo is trusted (fix for dubious ownership, if needed).

git config --global --add safe.directory /usr/src/kodekloudrepos/news



4]Switch to master branch

&nbsp;sudo git checkout master

&nbsp;sudo git pull origin master



5]Create and switch to new branch xfusion

git checkout -b xfusion



6]Copy the file into the repo

&nbsp;sudo cp /tmp/index.html .



7]Verify:

ls -l index.html



8]Stage and commit the file

sudo git add index.html

sudo git commit -m "Added index.html file in xfusion branch"



9]Push the new branch to origin

git push origin xfusion



10]Switch back to master branch

git checkout master



11]Merge xfusion branch into master

git merge xfusion



12]Push updated master to origin

git push origin master





✅ Final Verification

Check branches:

git branch -a



\* master

&nbsp; xfusion

&nbsp; remotes/origin/master

&nbsp; remotes/origin/xfusion





