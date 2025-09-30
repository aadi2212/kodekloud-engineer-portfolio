Linux SSH Authentication



The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure the thor user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e tony for app server 1). Based on the requirements, perform the following:





Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.





->



Task: Set up Password-less SSH for thor User on Jump Host

Objective:

Allow the thor user on the jump host to log in to all app servers via their respective sudo users (e.g., tony on App Server 1) without entering a password, so scripts can run automatically.





Steps Performed-



Step 1: Log in to Jump Host as thor



&nbsp;ssh thor@jump\_host.stratos.xfusioncorp.com

&nbsp;password- mjolnir123





Step 2: Check for existing SSH keys



ls -l ~/.ssh/id\_rsa.pub





Step 3: Generate SSH key pair



&nbsp;ssh-keygen -t rsa





Step 4: Prepare the sudo user account on the app server



1]Log in to the app server as the sudo user (example: tony on App Server 1):



&nbsp;ssh  tony@172.16.238.10

&nbsp;password-Ir0nM@n





2]Check if the .ssh directory exists:

ls -ld ~/.ssh





3]If it does not exist, create it and set permissions:

mkdir -p ~/.ssh

chmod 700 ~/.ssh





Step 5: Create the authorized\_keys file



&nbsp;touch ~/.ssh/authorized\_keys

&nbsp;chmod 600  ~/.ssh/authorized\_keys





Step 6: Add thor’s public key



1]Append the public key from thor to authorized\_keys:



&nbsp;echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDvUR7FRlIPsuaPbSgo14bqJEv1TbmVUi12hL17TEgO97gsPd7GFDQdl5P/1K3p5pwf8J2F7iD0LGFTiO/NsSWqal6APF+nnEFttWLM6+LVWM4DqYo7Z+SLpuNMjqSNzckQ1WJCIGuDnMG9FIrhYjDLgZfiH+dFuA6UszwcHBVjfvjf4NwXJNpFBseEAZPrhK/68GchL+BdN2FB4qVyaJKpMiU7+AK9uo1TI62DMRAYTnWHUNN8lUWh0s1kizpsp3WgQJpX90JZ12h/67vySBSq+t5wMBlksoub/gK7GqdwU1Wjrs+I4oEqK6d5PE3Gm4QJ4yrJVBiXv4pGARddlbYdqMq3PoXc4Hz54SiveRlt0sDH2+EdWC384+WZHVtAGI6yjs6/c2TIRxpye5Eb9fmRDJ3bSDa7OtV1b1f4TXr+EoIDG3uORsBMEZqhqoSRM4488PevUPOjzdmYyf/RQZM321FFTtYSJgu5i5PwwMgObFhnJySnG98ssCgWdKW+W30= thor@jumphost.stratos.xfusioncorp.com"  >> ~/.ssh/authorized\_keys 





Step 8: Test Password-less SSH on app server1,2 and 3



&nbsp;ssh tony@172.16.238.10

&nbsp;ssh steve@172.16.238.11

&nbsp;ssh banner@172.16.238.12





✅ Result:

The thor user on the jump host can now SSH into all app servers via their sudo users without a password, enabling scripts to run automatically.





