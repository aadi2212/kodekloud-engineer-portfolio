1]Linux user set-up with non-interactive shell



To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell. Here's your task: Create a user named siva with a non-interactive shell on App Server 3.



->



🛠 Step-by-Step Instructions



1️⃣ SSH into App Server 3

&nbsp;ssh banner@172.16.238.12

&nbsp;pass-BigGr33n



2️⃣ Create the User siva with Non-Interactive Shell

Use the /sbin/nologin shell depending on your system



&nbsp;sudo usermod -s /sbin/nologin siva 



✅ This prevents siva from logging into the system interactively.

You can verify the shell path available on your server using:



&nbsp;getent  passwd siva



siva:x:1001:1001::/home/siva:/sbin/nologin



&nbsp;  cat /etc/shells



3️⃣ Verify User and Shell



&nbsp;grep siva /etc/passwd 



Expected output-



siva:x:1001:1001::/home/siva:/sbin/nologin





Task Info-



Task Id- Linus user and Shell Management

Date- 7/8/2025

Category- Linux, User Management, Shell Access Control

Environment- Stratos Datacenter

Server Involved-

Stapp03- \[User-banner]





🎯 Objective

To support the backup agent tool's specifications, the system admin team at xFusionCorp Industries has requested the creation of a non-login service account:



• Create a user named siva on App Server 3.

• The user should have no interactive login shell.





📌 Notes:

&nbsp;	• This is useful for automation or service users who don't require shell access.

&nbsp;	• If /sbin/nologin is not available, use /usr/sbin/nologin or /bin/false



