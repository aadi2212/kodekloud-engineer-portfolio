Git stash



1]

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/beta present on Storage server in Stratos DC. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:





Look for the stashed changes under /usr/src/kodekloudrepos/beta git repository, and restore the stash with stash@{1} identifier. Further, commit and push your changes to the origin.



->



📘 Documentation – Restore Git Stash (stash@{1})



Objective



The Nautilus application development team had stashed some in-progress changes in the repository /usr/src/kodekloudrepos/beta. The requirement was to restore the stash identified as stash@{1}, commit the restored changes, and push them to the remote repository.





Steps Performed:



1]Navigate to the repository

cd /usr/src/kodekloudrepos/beta



2]List available stashes

git stash list

Output displayed multiple stashes, including the required stash@{1}.



stash@{0}: WIP on master: 98c5590 initial commit

stash@{1}: WIP on master: 98c5590 initial commit





3]Apply the stash

git stash apply stash@{1}

Restored the changes into the working directory.

The stash was not deleted from the stash list (since we used apply instead of pop).



4]Verify changes

git status

git diff

Confirmed that files from stash@{1} appeared as modified/added.



5]Stage and commit changes

git add .

git commit -m "Restored changes from stash@{1}"



6]Push changes to remote

git push origin master



Final State:

1]The stash stash@{1} was successfully restored.

2]Changes were committed with the message:



Restored changes from stash@{1}



Remote repository is fully updated with these changes.



