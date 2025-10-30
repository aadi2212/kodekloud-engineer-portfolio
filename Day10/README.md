Linux Bash Script

1]The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 3 in Stratos Datacenter, and they need to create a bash script named news_backup.sh which should accomplish the following tasks. (Also remember to place the script under /scripts directory on App Server 3).


a. Create a zip archive named xfusioncorp_news.zip of /var/www/html/news directory.

b. Save the archive in /backup/ on App Server 3. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.

c. Copy the created archive to Nautilus Backup Server server in /backup/ location.

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.

->


Task: Automate website backup from App Server 3 to Backup Server

Environment
Source (App Server 3): user banner (your login), path /var/www/html/news
Destination (Backup Server): clint@stbkp01.stratos.xfusioncorp.com, path /backup/
Script path: /scripts/news_backup.sh
Archive name: xfusioncorp_news.zip


Step-by-Step Completion

Log in into App server3
1]ssh banner@172.16.238.10
   password-BigGr33n


2]Prepare Directories
  sudo mkdir -p  /scripts /backup
  sudo chown banner:banner /scripts


3]Create the backup script
  vi /scripts/news_backup.sh

Add the following content:
#!/bin/bash

# Variables
SRC_DIR="/var/www/html/news"
TMP_BACKUP="/backup/xfusioncorp_news.zip"
REMOTE_USER="clint"
REMOTE_HOST="stbkp01.stratos.xfusioncorp.com"
REMOTE_BACKUP="/backup/"

# Step 1: Create zip archive
zip -r $TMP_BACKUP $SRC_DIR

# Step 2: Copy archive to backup server
scp $TMP_BACKUP ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_BACKUP}
 

4]Make script executable:
 chmod +x /scripts/news_backup.sh


5] Enable passwordless SSH from App Server 3 → Backup Server

Generate a key (if you don’t already have one):
 ssh-keygen -t rsa

6]Install the public key on the backup server:
ssh-copy-id clint@stbkp01.stratos.xfusioncorp.com
  
      OR

If any error occoured then create .ssh directory and in under this create one file authorized_keys
i.e. sudo mkdir -p .ssh
 cd .ssh  and then create touch authorized_keys
And then paste the public key that u generated in the authorized_keys files
And then echo " your public key banner@stapp03.stratos.xfusioncorp.com=clint@stbkp01.stratos.xfusioncorp.com"  >> ~/.ssh/authorized_keys  and then your issue gets resolved


7]Test login (should NOT ask for a password):

ssh clint@stbkp01.stratos.xfusioncorp.com
exit

Optional steps for verification:

1]Run the backup 
/scripts/news_backup.sh

2]Verify

On App Server 3:

ls -l /backup/xfusioncorp_news.zip

On Backup Server:

ssh clint@stbkp01.stratos.xfusioncorp.com "ls -l /backup/xfusioncorp_news.zip"


<img width="921" height="2487" alt="image" src="https://github.com/user-attachments/assets/041140a1-66f9-4a32-873d-a6516ca6cffd" />
