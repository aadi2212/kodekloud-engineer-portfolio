Set Up Git Repository on Storage Server



1]

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:

&nbsp;	1. Utilize yum to install the git package on the Storage Server.

&nbsp;	2. Create a bare repository named /opt/beta.git (ensure exact name usage).

->



📘 Documentation: Set Up Git Repository on Storage Server



🎯 Objective



The Nautilus development team requested a Git repository setup for a new application project.

Requirements:



1]Install git on the Storage Server.

2]Create a bare repository at /opt/beta.git.



Steps to Complete:



Step 1: SSH into Storage Server

ssh natasha@ststor01



Step 2: Install Git

sudo yum install -y git





Verify installation:

git --version





Step 3: Create Bare Repository

sudo mkdir -p /opt/beta.git

sudo git init --bare /opt/beta.git





Output should show:

Initialized empty Git repository in /opt/beta.git/





Step 4: Verify

Check inside the repo:



ls /opt/beta.git

You should see folders like branches, hooks, info, objects, refs.





✅ Final Outcome

1]git installed on Storage Server.

2]Bare Git repository created at /opt/beta.git.

3]Ready for the development team to clone and push code.





⚡ Bare repositories are typically used for central repos (shared by teams), since they don’t contain a working directory — only Git history.





