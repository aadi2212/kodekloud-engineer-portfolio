Manage Git Pull Requests



1]

Max want to push some new changes to one of the repositories but we don't want people to push directly to master branch, since that would be the final version of the code. It should always only have content that has been reviewed and approved. We cannot just allow everyone to directly push to the master branch. So, let's do it the right way as discussed below:

SSH into storage server using user max, password Max\_pass123 . There you can find an already cloned repo under Max user's home.



Max has written his story about The 🦊 Fox and Grapes 🍇



Max has already pushed his story to remote git repository hosted on Gitea branch story/fox-and-grapes



Check the contents of the cloned repository. Confirm that you can see Sarah's story and history of commits by running git log and validate author info, commit message etc.



Max has pushed his story, but his story is still not in the master branch. Let's create a Pull Request(PR) to merge Max's story/fox-and-grapes branch into the master branch



Click on the Gitea UI button on the top bar. You should be able to access the Gitea page.



UI login info:

\- Username: max

\- Password: Max\_pass123

PR title : Added fox-and-grapes story

PR pull from branch: story/fox-and-grapes (source)

PR merge into branch: master (destination)



Before we can add our story to the master branch, it has to be reviewed. So, let's ask tom to review our PR by assigning him as a reviewer



Add tom as reviewer through the Git Portal UI

&nbsp;	• Go to the newly created PR

&nbsp;	• Click on Reviewers on the right

&nbsp;	• Add tom as a reviewer to the PR

Now let's review and approve the PR as user Tom



Login to the portal with the user tom



Logout of Git Portal UI if logged in as max



UI login info:

\- Username: tom

\- Password: Tom\_pass123

PR title : Added fox-and-grapes story

Review and merge it.

Great stuff!! The story has been merged! 👏



Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



->



Task: Git Pull Request Workflow – Max’s Story



Objective:

Ensure that Max’s story is merged into master only after review, following a proper Pull Request (PR) workflow.





Environment:

• Server: Storage Server (Stratos DC)

• User 1 (Author): Max (Max\_pass123)

• User 2 (Reviewer): Tom (Tom\_pass123)

• Repository Path: ~/<repo-name> (already cloned)

• Remote Git host: Gitea

• Branch containing new story: story/fox-and-grapes

• Target branch: master





Step 1: SSH and Verify Repository

ssh max@<storage-server-ip>

\# Password: Max\_pass123





&nbsp;2: Navigate to cloned repository

cd ~/<repo-name>



3: Check current branch

git branch

Should show: \* story/fox-and-grapes



4:Verify commits in the branch

git log --oneline --decorate



Output example:

b61085f (HEAD -> story/fox-and-grapes, origin/story/fox-and-grapes) Added fox-and-grapes story

3d8d862 (origin/master, origin/HEAD, master) Merge branch 'story/frogs-and-ox'

40e4b4b Fix typo in story title

18ce805 Completed frogs-and-ox story

08d9cd1 Added the lion and mouse story

2ab2ecd Add incomplete frogs-and-ox story





Observation:

Max’s story is committed (b61085f) on story/fox-and-grapes.

Master branch has prior approved stories.





Step 2: Create Pull Request (PR) via Gitea

1]Open the repository in Gitea UI.



2]Login as Max

Username: max

Password: Max\_pass123



3]Click New Pull Request.



4]Configure PR:

Title: Added fox-and-grapes story

Source branch: story/fox-and-grapes

Destination branch: master



5]Click Create PR.





Step 3: Assign Reviewer

1]In the PR page, locate Reviewers on the right.

2]Add Tom as a reviewer.



Step 4: Review and Merge PR as Tom

1]Log out as Max.



2]Log in as Tom:

Username: tom

Password: Tom\_pass123



3]Open the PR Added fox-and-grapes story.



4]Review the changes and commit history.



5]Click Approve → Merge the PR into master.



Outcome:

Max’s story is now safely merged into master.

Master branch contains only reviewed and approved content.



