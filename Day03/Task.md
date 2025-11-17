✅ Day 3: Secure Root SSH Access
✅ 1. Task Overview

Disable direct SSH login for the root user on all application servers (App Server 1, App Server 2, App Server 3) to improve server security. Only non-root users with sudo privileges will be able to log in.

✅ 2. Prerequisites

Access to application servers via a non-root user (e.g., tony) with sudo privileges.

Basic knowledge of SSH and editing Linux configuration files.

✅ 3. Step-by-Step Instructions

SSH into each application server:

ssh tony@<server-ip>


Switch to the root user:

sudo su -


Edit the SSH configuration file:

vi /etc/ssh/sshd_config


Locate the line:

#PermitRootLogin yes


Change it to:

PermitRootLogin no


Save the file and exit the editor.

Restart the SSH service to apply changes:

systemctl restart sshd

✅ 4. Verification

Check that root login is disabled:

grep PermitRootLogin /etc/ssh/sshd_config


Expected Output:

PermitRootLogin no


Attempting to SSH directly as root should fail, while a non-root user with sudo should succeed.

✅ 5. Conclusion

Direct SSH access for the root user has been disabled on all application servers. This enhances security by requiring users to log in via non-root accounts and use sudo for administrative tasks, reducing the risk of unauthorized privileged access.
