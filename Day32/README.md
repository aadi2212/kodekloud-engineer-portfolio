Git Rebase



1]

he Nautilus application development team has been working on a project repository /opt/official.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:





One of the developers is working on feature branch and their work is still in progress, however there are some changes which have been pushed into the master branch, the developer now wants to rebase the feature branch with the master branch without loosing any data from the feature branch, also they don't want to add any merge commit by simply merging the master branch into the feature branch. Accomplish this task as per requirements mentioned.



Also remember to push your changes once done.



->



📘 Documentation – Git Rebase (Feature Branch with Master)



Objective



The Nautilus application development team has been working on a repository /opt/official.git, cloned under /usr/src/kodekloudrepos. A developer is working on a feature branch, while other changes have already been pushed to the master branch.



The requirement was to rebase the feature branch with master without losing any data from the feature branch and without creating merge commits (so git merge was not allowed).





Steps Performed:

1]Navigate to the repository

cd /usr/src/kodekloudrepos/official



2]Check branches

git branch -a

Confirmed that both master and feature branches exist.



3]Update master branch

&nbsp;git checkout master

&nbsp;git pull origin master



Ensured the local master branch is up to date with remote.



4]Switch to feature branch

git checkout feature



5]Rebase feature onto master

git rebase master



Re-applied commits from feature branch on top of the updated master.



In case of conflicts:

1]Resolved conflicts manually.

2]Staged changes:

git add <file>



3]Continued rebase:

git rebase --continue



4]Verify commit history

git log --oneline --graph --all



\* 5f0b0f1 (HEAD -> feature) Add new feature

\* 481a103 (origin/master, master) Update info.txt

| \* e5b1e26 (origin/feature) Add new feature

|/  

\* c435c9a initial commit



Confirmed that feature commits are now on top of master.

No merge commits were introduced.



5]Push rebased feature branch to remote.

git push origin feature --force



Final State:

The feature branch is now fully rebased on top of the latest master.

Developer’s changes are intact, and history is clean (no merge commits).

Remote repository updated successfully after force push.



