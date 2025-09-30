Copy File To Docker Container



1]

The Nautilus DevOps team possesses confidential data on App Server 3 in the Stratos Datacenter. A container named ubuntu\_latest is running on the same server.





Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu\_latest container located at /opt/. Ensure the file is not modified during this operation.



->



📒 OneNote Documentation



Task Title: Copy Encrypted File from Host to Container



Objective:



The Nautilus DevOps team needs to transfer a confidential encrypted file from App Server 3 (host) to the ubuntu\_latest container under /opt/, ensuring the file remains unchanged.





Steps Taken:



1]Checked running containers:

&nbsp;docker ps 

Confirmed that the container ubuntu\_latest was active.



2]Copied the encrypted file from host to container:

&nbsp;docker cp /tmp/nautilus.txt.gpg  ubuntu\_latest:/opt/

This command securely transfers the file without modification.



3]Verified inside the container:

docker exec -it ubuntu\_latest bash

ls -l /opt/nautilus.txt.gpg



Confirmed that the file exists in /opt/.



Final Outcome:

The file /tmp/nautilus.txt.gpg was successfully copied to /opt/ inside ubuntu\_latest container without any modification. File integrity was verified using checksums.







