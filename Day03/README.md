🚀 Day 3 – Secure Root SSH Access

Task: Disable Direct SSH Root Login on All App Servers
Date: 10 Aug 2025
Category: Linux Server Security / SSH Hardening
Project: xFusionCorp Industries – Stratos Datacenter

🔒 Background

During a recent security audit, the xFusionCorp security team announced new protocols to strengthen server access controls. One of the key policies is to disable direct SSH login for the root user.
Root login over SSH is a common attack vector, and disabling it significantly reduces the risk of brute-force attacks.

🎯 Objective

Disable direct SSH root login on the following servers:

App Server 1

App Server 2

App Server 3

Only non-root users should be allowed to SSH, and administrative privileges should be gained through sudo when required.

🛠️ Step-by-Step Implementation
1️⃣ SSH into Each App Server
ssh tony@172.16.238.10    # Example: App Server 1

2️⃣ Switch to Root User
sudo su -

3️⃣ Edit the SSH Configuration File
vi /etc/ssh/sshd_config


Find this line:

#PermitRootLogin yes


Modify it to:

PermitRootLogin no


🔸 If it is commented out, remove the #.
🔸 Change yes → no.

4️⃣ Restart SSH Service
systemctl restart sshd

5️⃣ Verify the Configuration
grep PermitRootLogin /etc/ssh/sshd_config


Expected output:

PermitRootLogin no

✅ Validation

SSH login using the root user should now be blocked.

Only regular users with sudo privileges can log in.

Ensures accountability and minimizes unauthorized privileged access.

🔐 Security Impact

Prevents brute-force attacks targeting the root account.

Encourages the use of individual user accounts.

Strengthens overall SSH security posture in the datacenter.
