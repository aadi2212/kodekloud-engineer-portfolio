Git Create Branches



1]Nautilus developers are actively working on one of the project repositories, /usr/src/kodekloudrepos/news. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team: 



On Storage server in Stratos DC create a new branch xfusioncorp\_news from master branch in /usr/src/kodekloudrepos/news git repo. 



Please do not try to make any changes in the code.



->



Steps to Create the New Branch.



1]Log in to the Storage Server.

&nbsp;ssh natasha@172.16.238.15



2]Switch to the natasha user (if required by your environment)

sudo su - natasha



3]Navigate to the repository.

cd /usr/src/kodekloudrepos/news



4]Check the current branch (should be master)

git status

git branch



But After git status I got error:

fatal: detected dubious ownership in repository at '/usr/src/kodekloudrepos/news' To add an exception for this directory, call: git config --global --add safe.directory /usr/src/kodekloudrepos/news



After git branch I got error:

fatal: detected dubious ownership in repository at '/usr/src/kodekloudrepos/news' To add an exception for this directory, call: git config --global --add safe.directory /usr/src/kodekloudrepos/news



✅ Fix for “detected dubious ownership” issue:

Run the following command as the natasha user:

git config --global --add safe.directory /usr/src/kodekloudrepos/news



This tells Git that /usr/src/kodekloudrepos/news is a trusted repository, even if ownership doesn’t match.



Then again check the status of git and git branch again.





5]Fetch latest updates (safe practice).

git fetch --all



6]Create and switch to the new branch.

git checkout -b xfusioncorp\_news master



7]Verify the new branch

git branch



You should now see:

\* xfusioncorp\_news

&nbsp; master





✅ Key Notes:

Do not edit any files.

The task only requires creating a branch, not pushing it unless explicitly stated.







