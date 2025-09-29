Install Ansible



1]During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with Ansible for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use jump host as an Ansible controller to test different kind of tasks on rest of the servers.





Install ansible version 4.8.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.



->





Here’s how you can complete it step-by-step:



1️⃣ Install pip3 if not already installed



&nbsp; sudo yum install python3-pip -y   #For RHEL/CentOS



&nbsp;sudo apt install python3-pip -y   #For Ubuntu/Debian





2️⃣ Install Ansible 4.8.0 using pip3 (system-wide)



Since they want it globally available for all users, don’t install in --user mode — install with sudo:



&nbsp;sudo pip3 install ansible=4.0.8





3️⃣ Verify installation



Check the installed version:



&nbsp;ansible --version





You should see:

ansible \[core 2.11.x]

&nbsp; 



4️⃣ Ensure binary is globally accessible



Pip’s global bin path should already be in $PATH. Usually, for global installs:

&nbsp;	• On RHEL/CentOS: /usr/local/bin

&nbsp;	• On Ubuntu/Debian: /usr/local/bin





Check:



&nbsp;which ansible



