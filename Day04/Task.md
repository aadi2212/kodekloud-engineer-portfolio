Script Execution Permissions



1]In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named xfusioncorp.sh. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 1 within the Stratos Datacenter.



Your task is to grant executable permissions to the /tmp/xfusioncorp.sh script on App Server 1. Additionally, ensure that all users have the capability to execute it.



->



Task: Grant Executable Permissions to /tmp/xfusioncorp.sh on App Server 1



Scenario:

The sysadmin team has distributed a new backup automation script, xfusioncorp.sh, across servers. On App Server 1, the script exists but does not have executable permissions. All users must be able to execute it.





Steps Performed-

1\. SSH into App Server 1

ssh tony@172.16.238.10



2]Verify script permissions before change

&nbsp;ls -l /tmp/xfusioncorp.sh



&nbsp; sample output-

---------- 1 root root 40 Aug 11 06:57 /tmp/xfusioncorp.sh



Here, the absence of x means it’s not executable.



3]Grant execute permission to all users

chmod a+x /tmp/xfusioncorp.sh



a+x-> Adds execute permission for all (owner, group, others)

&nbsp;

4]Verify permissions after change

&nbsp;ls -l /tmp/xfusioncorp.sh



---x--x--x 1 root root 40 Aug 11 06:57 /tmp/xfusioncorp.sh



5]Correct fix:



&nbsp;chmod a+xr /tmp/xfusioncorp.sh



r-read permission(needed for scripts)

x-execute permission



6]Verify

&nbsp;ls -l /tmp/xfusioncorp.sh



&nbsp;output-

-r-xr-xr-x 1 root root 40 Aug 11 06:57 /tmp/xfusioncorp.sh



