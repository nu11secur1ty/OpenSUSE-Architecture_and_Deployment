# Reset a MySQL Ver 15.1 Distrib 10.0.35-MariaDB root password
-------------------------------------------
# Stop the MySQL service

- (Ubuntu and Debian) Run the following command:

```bash
sudo /etc/init.d/mysql stop
```
- (CentOS, Fedora, and Red Hat Enterprise Linux) Run the following command:

```bash
sudo /etc/init.d/mysqld stop
```
# Start MySQL without a password
- Run the following command. The ampersand (&) at the end of the command is required.

```bash
sudo mysqld_safe --skip-grant-tables &
```

# Connect to MySQL
- Run the following command:

```bash
mysql -uroot
```

# Set a new MySQL root password
- Run the following command:

```bash
use mysql;

update user set authentication_string=PASSWORD("mynewpassword") where User='root';

flush privileges;

quit
```

# Stop and start the MySQL service
- (Ubuntu and Debian) Run the following commands:

```bash
sudo /etc/init.d/mysql stop
sudo /etc/init.d/mysql start
```

- (CentOS, Fedora, and Red Hat Enterprise Linux) Run the following commands:

```bash
sudo /etc/init.d/mysqld stop
sudo /etc/init.d/mysqld start
```

# Log in to the database
- Test the new password by logging in to the database.

```bash
mysql -u root -p
```

- Enter your new password when prompted.

------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------

# Reset Root Password in MySQL 8.0

- Create restore file
  It is important that you make sure that file is `readable` by the mysql user. Within that file paste the following:

```bash
 vim /home/user/init-file.txt
```
- add in to this file
  In the above change `“new_password”` with the password that you wish to use.
```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```
- Now make sure that the MySQL service is stopped. You can do the following:
```bash
systemctl stop mysqld.service     # for distros using systemd 
/etc/init.d/mysqld stop           # for distros using init
rcmysql stop                      # Suse
``` 
- Then run the following:
```bash
mysqld --user=mysql --init-file=/home/user/init-file.txt --console
```
-This will start the MySQL service and during the process it will execute the init-file that you have created and thus the password for the root user will be updated. Make sure to delete the file once the password has been reset.
Make sure to stop the server and start it normally after that.

```bash
systemctl stop mysqld.service        # for distros using systemd 
systemctl restart mysqld.service     # for distros using systemd 
rcmysql stop                         # Suse

/etc/init.d/mysqld stop              # for distros using init
/etc/init.d/mysqld restart           # for distros using init
rcmysql restart                      # Suse
rmysql start                         # Suse 
```
- You should now be able to connect to the MySQL server as root using the new password.

```bash
mysql -u root -p
```
# Reset MySQL Root Password Using –skip-grant-tables

- First make sure that the MySQL service is stopped.

```bash
 systemctl stop mysqld.service     # for distros using systemd 
 /etc/init.d/mysqld stop           # for distros using init
 rcmysql stop                      # Suse
```
- Then start the service with the following option.
```bash
mysqld --skip-grant-tables --user=mysql &
```
- Then, you can connect to the mysql server by simply running.
```bash
mysql
```
- Since account-management is disabled when the service is started with `--skip-grant-tables` option, we will have to reload the grants. That way we will be able to change the password later:

```bash
 FLUSH PRIVILEGES;
```
- Now you can run the following query to update the password. Make sure to change “new_password” with the actual password you wish to use.

```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_passowrd';
```
- Now stop the MySQL server and start it normally.
```bash
 systemctl stop mysqld.service        # for distros using systemd 
 systemctl restart mysqld.service     # for distros using systemd 
 rcmysql stop                         # Suse 
 rcmysql restart                      # Suse

 /etc/init.d/mysqld stop              # for distros using init
 /etc/init.d/mysqld restart           # for distros using init
```
- You should be able to connect with your new password.
```bash
 mysql -u root -p
```


# Enjoy




