Configure Jenkins Job For Package Installation



1]Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:



1\. Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.



2\. Create a new Jenkins job named install-packages and configure it with the following specifications:

&nbsp;	• Add a string parameter named PACKAGE.

&nbsp;	• Configure the job to install a package specified in the $PACKAGE parameter on the storage server within the Stratos Datacenter.



Note:

1\. Ensure to install any required plugins and restart the Jenkins service if necessary. Opt for Restart Jenkins when installation is complete and no jobs are running on the plugin installation/update page. Refresh the UI page if needed after restarting the service.



2\. Verify that the Jenkins job runs successfully on repeated executions to ensure reliability.



3\. Capture screenshots of your configuration for documentation and review purposes. Alternatively, use screen recording software like loom.com for comprehensive documentation and sharing.



->





🧩 Task: Automate Package Installation via Jenkins



Project: Nautilus Infrastructure – Stratos Datacenter



Environment: Jenkins CI/CD Server





Objective:

Automate the installation of packages on the Storage Server (ststor01) using a Jenkins job that accepts a package name as a parameter.





⚙️ Task Requirements:



Access Jenkins UI

1]Username: admin

2]Password: Adm!n321





Create a new Jenkins job

1]Job Name: install-packages

2]Type: Freestyle Project





Add a Parameter

1]Type: String Parameter

2]Name: PACKAGE

3]Description: “Enter the package name to install on the storage server.”





Build Step Configuration

1]Add Execute Shell build step

2]Configure Jenkins to connect to ststor01 and install the package specified in $PACKAGE.





💡 Initial Command Used:

sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 'sudo yum install -y $PACKAGE'





❌ Error Encountered

Console Output:

sudo: a terminal is required to read the password; either use the -S option to read from standard input or configure an askpass helper

sudo: a password is required

Build step 'Execute shell' marked build as failure







Root Cause:

Jenkins runs non-interactively, and sudo expects a password prompt on a terminal (TTY). Therefore, the command failed to elevate privileges remotely.







🔧 First Fix Attempt

Tried adding password via echo and sudo -S:

sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 echo 'Bl@kW' | sudo -S yum install -y $PACKAGE





New Error:

yum install: error: the following arguments are required: PACKAGE





Root Cause:

The $PACKAGE variable was not being passed correctly to the remote host; it was being expanded locally on Jenkins.







✅ Final Working Solution

Rewrote the script using a here-document to properly pass $PACKAGE as an argument to the remote host.





Final “Execute Shell” Script:



\#!/bin/bash

set -euo pipefail



if \[ -z "${PACKAGE:-}" ]; then

&nbsp; echo "ERROR: PACKAGE parameter is empty."

&nbsp; exit 1

fi



SSH\_USER="natasha"

SSH\_HOST="ststor01"

SSH\_PASS="Bl@kW"



sshpass -p "$SSH\_PASS" ssh -o StrictHostKeyChecking=no ${SSH\_USER}@${SSH\_HOST} bash -s -- "$PACKAGE" <<'REMOTE'

pkg="$1"

echo "Installing package on remote host: $pkg"

echo 'Bl@kW' | sudo -S yum install -y "$pkg"

REMOTE







🧪 Verification:

1]Triggered the job → Build with Parameters

2]Entered different package names (git, wget, vim)

3]Console Output showed successful installation:



Complete!

Finished: SUCCESS







✅ Task Outcome:

1]Jenkins job install-packages successfully automates remote package installation.

2]Parameter $PACKAGE allows dynamic input for any package name.

3]Job verified on multiple runs — reliable and repeatable.





Key Learnings:

1]Always use sudo -S when automating privileged remote commands.

2]Escape variables (\\$PACKAGE) to ensure remote expansion.

3]sshpass is useful for lab-based automation but not production-grade.

4]Prefer passwordless sudo or Jenkins credentials store for secure automation.







🏁 Final Status: ✅ Task Completed Successfully.



