MariaDB Troubleshooting

1]There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.

Look into the issue and fix the same.

->

📝 Task: Fix MariaDB Service Startup Failure

Steps to Complete the Task:

1]Check MariaDB service status
 sudo systemctl status mariadb

Service showed failed (inactive).


2]Check logs for errors

 sudo journalctl -xeu mariadb

Found error:

Make sure the /var/lib/mysql is empty before running mariadb-prepare-db-dir.

This indicated an issue with the data directory.


3]Verify MariaDB datadir configuration

grep -i datadir /etc/my.cnf /etc/my.cnf.d/*.cnf


Output:
 datadir=/var/lib/mysql


4]check where the actual data files are stored

 sudo ls -l /var/lib/
 sudo ls -l /var/lib/mysqld
Found system tables (ibdata1, ib_logfile0, mysql/, etc.) inside /var/lib/mysqld.


5]Stop MariaDB service before making changes

 sudo systemctl stop mariadb


6]Move data directory to the expected path

 sudo mv /var/lib/mysqld  /var/lib/mysql


7]Fix permissionson the new data directory

 sudo chown -R mysql:mysql  /var/lib/mysql
 sudo chmod -R 750 /var/lib/mysql 


8]Start MariaDB service again

 sudo systemctl start mariadb
sudo systemctl status mariadb

Service started successfully

9]Verify mariadb is working

 mysql -u root -p -e "SHOW DATABASES"

Confirmed output of default databases (mysql, performance_schema, etc.).


✅ Final Outcome

1]MariaDB service started and is now running normally.
2]Root cause: Data directory was at /var/lib/mysqld instead of /var/lib/mysql as configured.
3]Solution: Moved data directory to the correct location and fixed permissions.
<img width="925" height="1837" alt="image" src="https://github.com/user-attachments/assets/4d91b3e9-0b5e-4911-9829-145e8bf9e084" />

