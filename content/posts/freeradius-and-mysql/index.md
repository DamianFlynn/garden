---
title: FreeRadius and MySQL
date: '2020-03-14T00:00:00Z'
draft: true
slug: freeradius-and-mysql
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/freeradius-and-mysql/index.md
neocortex_source_sha: b8ddd08a5be9a57216903d304b48cd562ff703bce3305d59bb745b001a767859
description: Guide to installing an alternative Radius service, for those who may
  have implemented Windows NPS Server - License Free and Open Source, Enterprise Ready
tags:
- Linux
- Networking
- Azure
categories:
- IT Pro/DevOps
- OpenSource
- Identity
featuredImage: banner.png
---

# MySQL

We have 3 stages
1. Install the MySQL Environment
2. Create the Tables for FreeRadius
3. Configure the MySQL Extension on FreeRadius

## MariaDB

Install MySQL service

```bash
sudo apt install mariadb-server
sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

## Establish FreeRadius Tables

Establish the raidus database, and a user object to access this

```bash
sudo mysql -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 54
Server version: 10.1.47-MariaDB-0ubuntu0.18.04.1 Ubuntu 18.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database radius;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> grant all on radius.* to radius@localhost identified by "R@d1u5P@55w0rd!2021";
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> quit
Bye
```

## Extend FreeRadius with MySQL

```bash
sudo apt -y install freeradius-mysql 
```

Import the freeradius MySQL database scheme:

```bash
mysql -u root -p radius < /etc/freeradius/3.0/mods-config/sql/main/mysql/schema.sql
```

Check tables created:

```bash
$ sudo  mysql -u root -p -e "use radius;show tables;"
Enter password: 
+------------------+
| Tables_in_radius |
+------------------+
| nas              |
| radacct          |
| radcheck         |
| radgroupcheck    |
| radgroupreply    |
| radpostauth      |
| radreply         |
| radusergroup     |
+------------------+
```

Create a soft link for sql module under /etc/freeradius/3.0/mods-enabled/

```bash
sudo ln -s /etc/freeradius/3.0/mods-available/sql /etc/freeradius/3.0/mods-enabled/
```

Configure SQL module and change the database connection parameters to suit your environment.

```bash
sudo vim /etc/freeradius/3.0/mods-enabled/sql
```

Your sql section should look similar to below.

```conf
sql {
  driver = "rlm_sql_mysql"
  dialect = "mysql"

  # Connection info:
  server = "localhost"
  port = 3306
  login = "radius"
  password = "R@d1u5P@55w0rd!2021"

  # Database table configuration for everything except Oracle
  radius_db = "radius"
}

# Set to ‘yes’ to read radius clients from the database (‘nas’ table)
# Clients will ONLY be read on server startup.
read_clients = yes

# Table to keep radius client info
client_table = "nas"
```

Then change group right of `/etc/freeradius/3.0/mods-enabled/sql`

```bash
sudo chgrp -h freerad /etc/freeradius/3.0/mods-available/sql
sudo chown -R freerad:freerad /etc/freeradius/3.0/mods-enabled/sql
```
Restart FreeRadius service:

```bash
sudo systemctl restart freeradius.service
```

---
<!-- enrich:containment -->
*Part of: damianflynn*
