Resolve Git Merge Conflicts



1]

Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details:

SSH into storage server using user max and password Max\_pass123. Under /home/max you will find the story-blog repository. Try to push the changes to the origin repo and fix the issues. The story-index.txt must have titles for all 4 stories. Additionally, there is a typo in The Lion and the Mooose line where Mooose should be Mouse.



Click on the Gitea UI button on the top bar. You should be able to access the Gitea page. You can login to Gitea server from UI using username sarah and password Sarah\_pass123 or username max and password Max\_pass123.



Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



->



Git Troubleshooting Task – Story Blog Repo



Repository



Location: /home/max/story-blog

Remote: http://git.stratos.xfusioncorp.com/sarah/story-blog.git



Objective:

The Nautilus application development team wanted the following changes in the story-blog repository:

1]Fix push issues faced by user max while pushing changes.

2]Ensure story-index.txt contains titles of all 4 stories.

3]Fix typo: "The Lion and the Moose" -> "The Lion and the Mouse"





Steps Performed:

1]Logged in as Max

ssh max@<storage\_server>

cd /home/max/story-blog



2]Check repo status

git status



See what files Max changed.

Run:



git log --oneline --decorate

to confirm local vs remote state.

Often the issue is that local branch is behind origin, so push is rejected.



3]Tried to push changes

git push origin master



Error received:

! \[rejected] master -> master (non-fast-forward)

error: failed to push some refs

hint: Updates were rejected because a pushed branch tip is behind its remote



4]Investigated the issue:

Cause: Local branch was behind remote master (Sarah had pushed new commits).

Solution: Rebase local changes on top of updated remote.



5]Pulled with rebase

git pull origin master --rebase



Conflict in story-index.txt:



<<<<<<< HEAD

1\. The Lion and the Mooose

...

=======

1\. The Lion and the Mouse

...

>>>>>>>



Resolved conflict manually:



1\. The Lion and the Mouse

2\. The Frogs and the Ox

3\. The Fox and the Grapes

4\. The Donkey and the Dog





5]Staged and continued rebase

git add story-index.txt

git rebase --continue



6]Committed typo fix

git commit -m "Fixed story-index titles and corrected typo in The Lion and the Mouse"



7]Pushed successfully

git push origin master

\# Username: max

\# Password: Max\_pass123



8]Verified on Gitea UI

1]Open Gitea UI (top bar button).

2]Login as sarah/Sarah\_pass123 or max/Max\_pass123.

3]Confirm the latest commit appears with your message.

4]Check file contents in UI:



story-index.txt → must list all 4 stories.

Story file → must show Mouse instead of Mooose.





Issue Faced:

1]Push failed due to non-fast-forward error because local branch was behind remote.

2]Merge conflict occurred in story-index.txt during rebase.



Resolution:

1]Used git pull origin master --rebase to reapply Max’s changes on top of Sarah’s commits.

2]Manually fixed the conflict in story-index.txt.

3]Staged the fix, continued the rebase, and pushed changes successfully.



✅ Final Outcome:

1]story-index.txt now contains all 4 story titles.

2]Typo “Mooose” corrected to “Mouse”.

3]Changes pushed successfully as user Max.

4]Verified in Gitea UI.



