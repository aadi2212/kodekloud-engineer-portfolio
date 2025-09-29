Secure Root SSH Access



1]

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.



Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.



->



Task: Disable Direct SSH Root Login on All App Servers

Date: 10-Aug-2025

Project: xFusionCorp Industries – Stratos Datacenter Security Hardening

Category: Linux Server Security / SSH Configuration



Background

Following security audits, the xFusionCorp Industries security team implemented stricter access control policies. One of these measures requires disabling direct SSH root login on all application servers to prevent unauthorized privileged access.





Objective

Disable direct SSH root login on:

&nbsp;	• App Server 1

&nbsp;	• App Server 2

&nbsp;	• App Server 3





Step-by-Step Implementation-



1]SSH into Each App Server

ssh tony@172.16.238.10



2]Switch to root

&nbsp; sudo su -



3]Edit SSH Configuration

&nbsp;vi /etc/ssh/sshd\_config



Find

&nbsp;PermitRootLogin Yes



Change to 

&nbsp;PermitRootLogin no



If the line is commented (# at the beginning), remove the # and set it to no.





4]Restart SSH Service-

&nbsp; systemctl restart sshd



5]Verify Configuration-

&nbsp;grep PermitRootLogin /etc/ssh/sshd\_config



Expected Output-

PermitRootLogin no





Validation

&nbsp;	• Confirm that SSH login with root user is now blocked.

&nbsp;	• Only non-root accounts with sudo privileges can access via SSH.





Security Impact

&nbsp;	• Reduces the risk of brute-force attacks on root user.

&nbsp;	• Enforces use of individual user accounts for accountability.



