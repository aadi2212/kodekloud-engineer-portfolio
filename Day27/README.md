Git Revert Some Changes



1]

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/demo present on Storage server in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

&nbsp;	1. In /usr/src/kodekloudrepos/demo git repository, revert the latest commit ( HEAD ) to the previous commit (JFYI the previous commit hash should be with initial commit message ).

&nbsp;	2. Use revert demo message (please use all small letters for commit message) for the new revert commit.

->



&nbsp;   Steps to Complete the Task:



1]SSH into Storage Server

ssh natasha@ststor01



2]Navigate to the repo

cd /usr/src/kodekloudrepos/demo



3]Navigate to the repo

cd /usr/src/kodekloudrepos/demo



4]Check git log to see commits

&nbsp;sudo git log --oneline



5]Example output:

1393c4e (HEAD -> master, origin/master) add data.txt file

13be74c initial commit





6]Revert the latest commit (HEAD)

git revert does not use -m "message" for normal commits. Either let the editor open or use the two-step non-interactive method below.



Option A (editor opens):

git revert HEAD



When the editor opens, replace the default text with:

revert demo message



Option B (non-interactive, one shot):

git revert -n HEAD

git commit -m "revert demo message"



If permissions require it (repo owned by root), prefix with sudo.





7]Verify new commit on top.

git log --oneline -n 3



Expected:

66a9c61 (HEAD -> master) revert demo message

1393c4e (origin/master) add data.txt file

13be74c initial commit



✅ The new top commit should be revert demo message.





End Result:

Latest commit reverted.

A new commit revert demo message created to undo the changes.

Repo now points back to the state of the initial commit (but with proper history maintained).



