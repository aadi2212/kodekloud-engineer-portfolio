&nbsp;Git hard reset



1]

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/demo present on Storage server in Stratos DC. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the HEAD and the branch itself to a commit with message add data.txt file. Find below more details:

&nbsp;	1. In /usr/src/kodekloudrepos/demo git repository, reset the git commit history so that there are only two commits in the commit history i.e initial commit and add data.txt file.

&nbsp;	2. Also make sure to push your changes.

->

Objective

The Nautilus application development team had been testing in the Git repository /usr/src/kodekloudrepos/demo. A few unwanted commits were pushed. The requirement was to reset the repository so that only the following two commits remain in the history:

1\. Initial commit

2\. add data.txt file

The cleaned history also needed to be pushed to the remote repository.

Steps Performed:

1]Navigate to the repository

cd /usr/src/kodekloudrepos/demo

2]Check commit history

&nbsp;git log --oneline

Identified the commit hash corresponding to the commit message add data.txt file.

In our case 4998c7d add data.txt file

3]Interactive rebase to keep only two commits

&nbsp;git rebase rebase  -i --root  --if got issue then use sudo

This opens an editor listing all commits starting from the very first one.

Keep only:

pick <hash\_init> initial commit

pick <hash\_data> add data.txt file

Delete (or comment out) all other commits.

Save and exit.

Git will rewrite history so only these two commits remain.



4]Verify commit history

git log --oneline

Expected:

<hash\_data> add data.txt file

<hash\_init> initial commit



5]Force push to remote (to clean the repo completely)

git push origin master --force



🔹 Final State

Only two commits remain:

1]initial commit

2]add data.txt file

Remote repository history is rewritten and cleaned.

Work tree points to the state from add data.txt file.



