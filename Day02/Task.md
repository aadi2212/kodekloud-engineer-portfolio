✅ Day 2: Temporary User Setup with Expiry
✅ 1. Task Overview

A developer named kirsty needs temporary access to the Nautilus project.
To maintain proper access control, her account must automatically expire on a specific date.

Your task:

👉 Create user kirsty on App Server 2 (stapp02) with an expiry date of 2024-01-28.
👉 Ensure the username remains lowercase per Linux standards.

✅ 2. Objective

Create user kirsty on stapp02

Set account expiry: 28 January 2024

Ensure the account becomes inaccessible after expiry

Verify expiry settings

✅ 3. Access the Server

SSH into App Server 2:

ssh steve@172.16.238.11


Password: Am3ric@

✅ 4. Create User with Expiry Date

Create the user and assign an expiration date:

sudo useradd -e 2024-01-28 kirsty


📌 Notes:

-e → sets the account expiry date

Username must be lowercase → kirsty

✅ 5. Set Password for the User

Assign a password:

sudo passwd kirsty


Example password used:
kirsty@123

✅ 6. Verify Expiry Details

Check account expiry:

sudo chage -l kirsty


Expected Output Snippet:

Account expires : Jan 28, 2024

📌 7. Additional Notes

Account expiry is ideal for contractors, temporary users, developers, or support engineers

After expiry, the user cannot log in

Expiry can be removed or extended using:

sudo chage -E -1 kirsty        # Remove expiry
sudo chage -E YYYY-MM-DD kirsty  # Extend expiry

📘 8. Task Information
Field	Details
Task ID	Temporary User Expiry Setup
Date	8/8/2025
Category	Linux, User Management, Access Control
Environment	Stratos Datacenter
Server	stapp02 (User: steve)
🎉 Final Result

✔ User kirsty created
✔ Expiry date set to 2024-01-28
✔ Password assigned
✔ Verification completed
✔ Task successfully completed
