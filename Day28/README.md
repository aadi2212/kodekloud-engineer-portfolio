Git Cherry Pick



1]

The Nautilus application development team has been working on a project repository /opt/games.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with the DevOps team:





There are two branches in this repository, master and feature. One of the developers is working on the feature branch and their work is still in progress, however they want to merge one of the commits from the feature branch to the master branch, the message for the commit that needs to be merged into master is Update info.txt. Accomplish this task for them, also remember to push your changes eventually.



->



Steps to complete the task:

1]SSH into Storage Server

ssh natasha@ststor01



2]Go to repo directory

cd /usr/src/kodekloudrepos/games



3]Check branches

git branch  --if got error the use sudo

Ensure you see master and feature.



4]Find the commit hash on feature branch

git log feature --oneline



Look for the commit with message:

<commit\_hash> Update info.txt



827195a (HEAD -> feature, origin/feature) Update welcome.txt

1bb2733 Update info.txt

9e5fe67 (origin/master, master) Add welcome.txt

f641130 initial commit





5]Switch to master branch

&nbsp;sudo git checkout master



6]Cherry-pick that commit into master

git cherry-pick <commit\_hash>

git cherry-pick  1bb2733 



⚠️ If there are conflicts, resolve them, then run:

git add <file>

git cherry-pick --continue





7]Push master branch to remote

&nbsp; git push origin master





✅ End Result

Only the commit Update info.txt from feature branch is merged into master.

Work-in-progress commits from feature remain untouched.

Changes are pushed to the remote (/opt/games.git).



