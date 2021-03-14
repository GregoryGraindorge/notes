MySQL Backup and Restore Commands

# MySQL Backup and Restore Commands for Database Administration

by [Ravi Saive](https://www.tecmint.com/author/admin/) | Published: November 2, 2012 | Last Updated: January 3, 2015

**Linux Certifications** - [RHCSA / RHCE Certification](https://www.tecmint.com/red-hat-rhcsa-rhce-exam-certification-book/) | [Ansible Automation Certification](https://www.tecmint.com/redhat-ansible-automation-exam-certification-book/) | [LFCS / LFCE Certification](https://www.tecmint.com/linux-foundation-lfcs-lfce-certification-exam-book/)

This article shows you several practical examples on how to perform various backup operations of **MySQL** databases using **mysqldump** command and also we will see how to restore them with the help of **mysql** and **mysqlimport** command in **Linux**.

**mysqldump** is a command-line client program, it is used to dump local or remote **MySQL** database or collection of databases for backup into a single flat file.

[![MySQL-Backup-and-Restore-300x227.jpg](../_resources/6225b4932e350e44111ce331e199baef.jpg)](https://www.tecmint.com/wp-content/uploads/2012/11/MySQL-Backup-and-Restore.jpg)

How to Backup and Restore MySQL Database

We assume that you already have **MySQL** installed on **Linux** system with administrative privileges and we assume that you already have a small amount of knowledge on **MySQL**. If you don’t have MySQL installed or don’t have any exposure to **MySQL** then read our articles below.

1. [Install MySQL Server on RHEL/CentOS 6-5, Fedora 17-12](https://www.tecmint.com/install-mysql-on-rhel-centos-6-5-fedora-17-12/)

2. [20 MySQL Commands for Database Administration](https://www.tecmint.com/mysqladmin-commands-for-database-administration-in-linux/)

### How to Backup MySQL Database?

To take a backup of **MySQL** database or databases, the database must exist in the database server and you must have access to it. The format of the command would be.

# mysqldump -u [username] –p[password] [database_name] > [dump_file.sql]

The parameters of the said command as follows.
1. **[username]** : A valid MySQL username.
2. **[password]** : A valid MySQL password for the user.
3. **[database_name]** : A valid Database name you want to take backup.
4. **[dump_file.sql]** : The name of backup dump file you want to generate.

#### How to Backup a Single MySQL Database?

To take a backup of single database, use the command as follows. The command will dump database [**rsyslog**] structure with data on to a single dump file called **rsyslog.sql**.

# mysqldump -u root -ptecmint rsyslog > rsyslog.sql

#### How to Backup Multiple MySQL Databases?

If you want to take backup of multiple databases, run the following command. The following example command takes a backup of databases [**rsyslog**, **syslog**] structure and data in to a single file called **rsyslog_syslog.sql**.

# mysqldump -u root -ptecmint --databases rsyslog syslog > rsyslog_syslog.sql

#### How to Backup All MySQL Databases?

If you want to take backup of all databases, then use the following command with option **–all-database**. The following command takes the backup of all databases with their structure and data into a file called **all-databases.sql**.

# mysqldump -u root -ptecmint --all-databases > all-databases.sql

#### How to Backup MySQL Database Structure Only?

If you only want the backup of database structure without data, then use the option **–no-data** in the command. The below command exports database [**rsyslog**] **Structure** into a file **rsyslog_structure.sql**.

# mysqldump -u root -ptecmint -–no-data rsyslog > rsyslog_structure.sql

#### How to Backup MySQL Database Data Only?

To backup database **Data** only without structure, then use the option **–no-create-info** with the command. This command takes the database [**rsyslog**] **Data**  into a file **rsyslog_data.sql**.

# mysqldump -u root -ptecmint --no-create-db --no-create-info rsyslog > rsyslog_data.sql

#### How to Backup Single Table of Database?

With the below command you can take backup of single table or certain tables of your database. For example, the following command only take backup of **wp_posts** table from the database **wordpress**.

# mysqldump -u root -ptecmint wordpress wp_posts > wordpress_posts.sql

#### How to Backup Multiple Tables of Database?

If you want to take backup of multiple or certain tables from the database, then separate each table with space.

# mysqldump -u root -ptecmint wordpress wp_posts wp_comments > wordpress_posts_comments.sql

#### How to Backup Remote MySQL Database

The below command takes the backup of remote server [**172.16.25.126**] database [**gallery**] into a local server.

# mysqldump -h 172.16.25.126 -u root -ptecmint gallery > gallery.sql

### How to Restore MySQL Database?

In the above tutorial we have seen the how to take the backup of databases, tables, structures and data only, now we will see how to restore them using following format.

# # mysql -u [username] –p[password] [database_name] < [dump_file.sql]

#### How to Restore Single MySQL Database

To restore a database, you must create an empty database on the target machine and restore the database using **msyql** command. For example the following command will restore the **rsyslog.sql** file to the **rsyslog** database.

# mysql -u root -ptecmint rsyslog < rsyslog.sql

If you want to restore a database that already exist on targeted machine, then you will need to use the **mysqlimport** command.

# mysqlimport -u root -ptecmint rsyslog < rsyslog.sql

In the same way you can also restore database tables, structures and data. If you liked this article, then do share it with your friends.

Sharing is Caring...[Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.tecmint.com%2Fmysql-backup-and-restore-commands-for-database-administration%2F)[Share on Twitter](https://twitter.com/intent/tweet/?text=MySQL+Backup+and+Restore+Commands+for+Database+Administration&url=https%3A%2F%2Fwww.tecmint.com%2Fmysql-backup-and-restore-commands-for-database-administration%2F&via=tecmint)[Share on Linkedin](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Fwww.tecmint.com%2Fmysql-backup-and-restore-commands-for-database-administration%2F&title=MySQL+Backup+and+Restore+Commands+for+Database+Administration)[Share on Reddit](https://reddit.com/submit?url=https%3A%2F%2Fwww.tecmint.com%2Fmysql-backup-and-restore-commands-for-database-administration%2F&title=MySQL+Backup+and+Restore+Commands+for+Database+Administration)