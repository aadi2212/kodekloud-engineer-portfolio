Day 2 — Temporary User Setup with Expiry
🏢 Task Overview

A developer named kirsty needs temporary access to the Nautilus project.
To ensure proper access management, her account must expire automatically on a specific date.

Your task:

👉 Create a user kirsty on App Server 2 with an expiry date of 2024-01-28.
👉 Username must be lowercase as per standard Linux protocol.

🎯 Objective

Create user kirsty on stapp02

Set expiry date: 28 January 2024

Ensure the user cannot log in after expiry

Verify the expiry details

🛠 Step-by-Step Instructions
1️⃣ SSH into App Server 2
ssh steve@172.16.238.11
# password: Am3ric@

2️⃣ Create the User with an Expiry Date

Use the -e option to set the account expiration:

sudo useradd -e 2024-01-28 kirsty


📌 Notes:

-e → sets account expiry

kirsty must be lowercase

3️⃣ Set Password for the User
sudo passwd kirsty


Example password used:

kirsty@123

4️⃣ Verify User Expiry Details
sudo chage -l kirsty


Expected output snippet:

Account expires : Jan 28, 2024

📌 Additional Notes

Using account expiry prevents unauthorized access after the required period

Very useful for contractors, temporary developers, support engineers

After expiration, user cannot log in unless expiry is removed or extended

📘 Task Information
Field	Details
Task ID	Temporary User Expiry Setup
Date	8/8/2025
Category	Linux, User Management, Access Control
Environment	Stratos Datacenter
Server	stapp02 (User: steve)
✅ Final Result

✔ User kirsty created
✔ Expiry date set to 2024-01-28
✔ Password configured
✔ Verification complete
✔ Task successfully completed
