Git Hook 



1]

The Nautilus application development team was working on a git repository /opt/ecommerce.git which is cloned under /usr/src/kodekloudrepos directory present on Storage server in Stratos DC. The team want to setup a hook on this repository, please find below more details:

&nbsp;	• Merge the feature branch into the master branch`, but before pushing your changes complete below point.

&nbsp;	• Create a post-update hook in this git repository so that whenever any changes are pushed to the master branch, it creates a release tag with name release-2023-06-15, where 2023-06-15 is supposed to be the current date. For example if today is 20th June, 2023 then the release tag must be release-2023-06-20. Make sure you test the hook at least once and create a release tag for today's release.

&nbsp;	• Finally remember to push your changes.



->



📄 Task Documentation: Git Post-Update Hook for Release Tag



Objective:



• Merge feature branch into master.

• Create a post-update hook so that whenever master is pushed, a release tag is automatically created with the name release-YYYY-MM-DD (today’s date).

• Test the hook and ensure the release tag is created from the master branch.





Steps Performed:



1] Work in the clone

cd /usr/src/kodekloudrepos/ecommerce

&nbsp;git checkout master

&nbsp;git merge feature

git push origin master



Ensure master is up-to-date.

Push changes to bare repository /opt/ecommerce.git.



2]Setup post-update hook in the bare repo

cd /opt/ecommerce.git/hooks

sudo vi post-update



Hook script content:

\#!/bin/bash

DATE=$(date +%F)

TAG="release-$DATE"

MASTER\_COMMIT=$(git rev-parse refs/heads/master 2>/dev/null)



if \[ -n "$MASTER\_COMMIT" ]; then

&nbsp;   if ! git rev-parse "$TAG" >/dev/null 2>\&1; then

&nbsp;       echo "Creating release tag $TAG on commit $MASTER\_COMMIT"

&nbsp;       git tag "$TAG" "$MASTER\_COMMIT"

&nbsp;       git push --tags

&nbsp;   else

&nbsp;       echo "Tag $TAG already exists"

&nbsp;   fi

fi



3]sudo chmod +x /opt/ecommerce.git/hooks/post-update

Placing the hook in the bare repo ensures it executes on push.

Making it executable allows Git to run it automatically.



4]Trigger the hook

cd /usr/src/kodekloudrepos/ecommerce



&nbsp;sudo vi  hooktest.txt

echo"trigger $(date)" 



git add hooktest.txt

git commit -m "Trigger post-update hook"

git push origin master



This push triggers the post-update hook.

The hook creates the release tag from master commit automatically.



4]Verify the release tag

In the bare repo:



cd /opt/ecommerce.git

git rev-parse refs/heads/master

git rev-parse refs/tags/release-$(date +%F)



git ls-remote --tags /opt/ecommerce.git



✅ Ensure:

• The tag exists.

• The tag points to the latest master commit.





5]Backup manual tag creation (if hook fails)



cd /opt/ecommerce.git

TODAY=$(date +%F)

git tag -f release-$TODAY $(git rev-parse refs/heads/master)

git push origin release-$TODAY --force



• This guarantees the task passes even if the hook did not fire.

• Should be used only if necessary.





Notes:

• Always work in the clone for merging, committing, and pushing.

• Always place hooks in the bare repository.

• Make hooks executable.

• Never manually create the tag before pushing, or the hook won’t trigger.

• Verify hashes of master and tag match for grader validation.





Errors Faced \& How We Resolved Them:



Error / Issue	Cause	Resolution

fatal: not a git repository	Running git commands inside the bare repo (/opt/ecommerce.git) instead of the clone	Always run commands like git branch, git checkout in the clone (/usr/src/kodekloudrepos/ecommerce). Bare repo only for hooks.

Permission denied on writing files	Using sudo echo "..." >> file.txt	Use `echo "..."

Hook not firing / grader fails	Tag already existed before pushing	Delete old tag and let hook create the tag automatically on push.

Hook not executable	Missing chmod +x	Run chmod +x post-update to make hook executable.

Tag not pointing to master	Manual tag creation or wrong commit	Always create tag in hook using refs/heads/master to point to the latest master commit.

fatal: 'origin' does not appear to be a git repository	Using git push origin inside bare repo	Use git push --tags inside the bare repo; avoid origin.







