1]Temporary User Setup with Expiry



As part of the temporary assignment to the Nautilus project, a developer named kirsty requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:





Create a user named kirsty on App Server 2 in Stratos Datacenter. Set the expiry date to 2024-01-28, ensuring the user is created in lowercase as per standard protocol.



->



Step-by-step solution



1]SSH into App Server 2



&nbsp;ssh steve@172.16.238.11

&nbsp;pass- Am3ric@





2]Create the user with an expiry date



&nbsp;sudo useradd -e  2024-01-28 kirsty



Note-

-e= sets the account expiration date (2024-01-28)

&nbsp;kirsty= username in lowercase





3]Set a password for the user



&nbsp;sudo passwd kirsty



New passwd- kirsty@123



4]Verify the account details



sudo chage -l kirsty



Then you should see the output-



Account expires    : Jan 28, 2024



