🚀 Day 3 – Secure Root SSH Access

Task: Disable Direct SSH Root Login on All App Servers
Date: 10 Aug 2025
Category: Linux Server Security / SSH Hardening
Project: xFusionCorp Industries – Stratos Datacenter

🔒 Background

During a recent security audit, the xFusionCorp security team introduced new security measures to strengthen SSH access.
One of the most important changes is to disable direct SSH login for the root user to reduce security risks.

🎯 Objective

Disable direct SSH root login on:

App Server 1

App Server 2

App Server 3

🛠️ Step-by-Step Implementation
1️⃣ SSH into Each App Server
ssh tony@172.16.238.10

2️⃣ Switch to Root
sudo su -

3️⃣ Edit SSH Configuration
vi /etc/ssh/sshd_config


Find:

#PermitRootLogin yes


Change to:

PermitRootLogin no

4️⃣ Restart SSH Service
systemctl restart sshd

5️⃣ Verify the Configuration
grep PermitRootLogin /etc/ssh/sshd_config


Expected output:

PermitRootLogin no

✅ Validation

Root login over SSH is blocked.

Only non-root users with sudo privileges can log in.

Improves server security and minimizes risk.

🔐 Security Impact

Prevents brute-force attacks on the root account.

Encourages secure user-based access.

Strengthens overall SSH security posture.
