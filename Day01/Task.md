✅ Day 1: Linux User Setup with Non-Interactive Shell
✅ 1. Task Overview

The system admin team at xFusionCorp Industries needs to create a service account for a backup agent.
This account must not have interactive shell access.

Your task:

👉 Create a user siva on App Server 3 (stapp03) with a non-interactive shell.

✅ 2. Objective

Create user siva on stapp03

Assign a non-login shell (/sbin/nologin or equivalent)

Ensure the user cannot log in interactively

✅ 3. Access the Server

SSH into App Server 3:

ssh banner@172.16.238.12


Password: BigGr33n

✅ 4. Configure the User Shell

Assign a non-interactive shell to the user:

sudo usermod -s /sbin/nologin siva


👉 This ensures siva cannot log in or access an interactive shell.

✅ 5. Verification
🔍 Check user entry:
getent passwd siva


Expected Output:

siva:x:1001:1001::/home/siva:/sbin/nologin

🔍 Check all available shells:
cat /etc/shells

🔍 Verify via /etc/passwd:
grep siva /etc/passwd


Expected:

siva:x:1001:1001::/home/siva:/sbin/nologin

✅ 6. Notes

This configuration is ideal for service accounts or automation users.

Alternative non-login shells:

/usr/sbin/nologin

/bin/false

📘 7. Task Information
Field	Details
Task ID	Linux User & Shell Management
Date	7/8/2025
Category	Linux, User Management, Shell Access Control
Environment	Stratos Datacenter
Server	stapp03 (User: banner)
🎉 Final Result

✔ User siva created
✔ Non-interactive shell applied
✔ Verification complete
✔ Task successfully completed



