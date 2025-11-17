Day 3 – Secure Root SSH Access

Task: Disable direct SSH root login on all application servers
Date: 10 Aug 2025
Category: Linux Server Security / SSH Hardening

Objective

Disable direct SSH login for the root user on the following servers in the Stratos Datacenter:

App Server 1

App Server 2

App Server 3

Steps
1. SSH into Each App Server
ssh tony@172.16.238.10

2. Switch to Root
sudo su -

3. Edit SSH Configuration
vi /etc/ssh/sshd_config


Update the following line:

From:

#PermitRootLogin yes


To:

PermitRootLogin no

4. Restart SSH Service
systemctl restart sshd

5. Verify Configuration
grep PermitRootLogin /etc/ssh/sshd_config


Expected:

PermitRootLogin no

Validation

Root SSH access is disabled.

Only non-root users with sudo privileges can log in.

Security Benefit

Disabling root SSH login reduces security risks and prevents unauthorized privileged access.
