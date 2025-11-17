Day 1 — Linux User Setup with Non-Interactive Shell
🏢 Task Overview

The system admin team at xFusionCorp Industries needs to create a service account for a backup agent.
This account must not have interactive shell access.

Your task:

👉 Create a user siva on App Server 3 with a non-interactive shell.

🎯 Objective

Create a user siva on stapp03.

Assign a non-login shell (/sbin/nologin or equivalent).

Ensure the user cannot log in interactively.

🛠 Step-by-Step Instructions
1️⃣ SSH into App Server 3
ssh banner@172.16.238.12
# password: BigGr33n

2️⃣ Assign a Non-Interactive Shell to User siva

Use /sbin/nologin or another non-login shell depending on OS:

sudo usermod -s /sbin/nologin siva


👉 This ensures the user cannot access the shell interactively.

3️⃣ Verify the User's Shell

Check user entry:

getent passwd siva


Expected:

siva:x:1001:1001::/home/siva:/sbin/nologin


Check available shells:

cat /etc/shells


Double-check via /etc/passwd:

grep siva /etc/passwd


Expected:

siva:x:1001:1001::/home/siva:/sbin/nologin

📌 Notes

Useful for automation/service accounts that should not access the terminal.

Alternative non-login shells:

/usr/sbin/nologin

/bin/false

📘 Task Information
Field	Details
Task ID	Linux User & Shell Management
Date	7/8/2025
Category	Linux, User Management, Shell Access Control
Environment	Stratos Datacenter
Server	stapp03 (User: banner)
✅ Final Result

✔ User siva created
✔ Non-interactive shell applied
✔ Verification complete
✔ Task successfully completed




