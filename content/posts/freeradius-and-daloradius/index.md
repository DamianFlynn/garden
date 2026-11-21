---
title: FreeRadius and daloRADIUS?
date: '2020-03-15T00:00:00Z'
draft: true
slug: freeradius-and-daloradius
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/freeradius-and-daloradius/index.md
neocortex_source_sha: 319cde06d76d5173daaf29b52f069dbb9bcb432a711b1df9158c5037e3548297
description: Guide to installing an alternative Radius service, for those who may
  have implemented Windows NPS Server - License Free and Open Source, Enterprise Ready
tags:
- RADIUS
- Linux
- FreeRADIUS
- daloRADIUS
- MySQL
categories:
- Networking
- Identity and Security
series:
- RADIUS in a Cloud World
featuredImage: banner.png
---

**daloRADIUS** is an advanced RADIUS web management platform written in PHP and JavaScript. It is mainly aimed at managing Hotspots and general-purpose ISP deployments powered by FreeRADIUS server. Some good features of daloRADIUS include:

* Has a database abstraction layer with support for many database systems – MySQL, SQLite, PostgreSQL, MsSQL and Oracle
* Advanced user management
* Powerful graphical reporting and accounting
* Integrates with GoogleMaps for geo-location
* Has a billing engine

## Installation

### Apache Web Server

Install Apache web server by running:

```bash
sudo apt -y install apache2 php libapache2-mod-php php-{gd,common,mail,mail-mime,mysql,pear,db,mbstring,xml,curl}
```

Check the version of php installed:

```bash
php -v

PHP 7.2.24-0ubuntu0.18.04.7 (cli) (built: Oct  7 2020 15:24:25) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.24-0ubuntu0.18.04.7, Copyright (c) 1999-2018, by Zend Technologies
```

### daloRADIUS

We need to install Daloradius to get FreeRADIUS web administration interface.

```bash
wget https://github.com/lirantal/daloradius/archive/master.tar.gz
cd daloradius*
```

Now import Daloradius mysql tables

```bash
sudo mysql -u root -p radius < contrib/db/fr2-mysql-daloradius-and-freeradius.sql 
sudo mysql -u root -p radius < contrib/db/mysql-daloradius.sql
```

Configure daloRADIUS database connection details:

```bash
cd ..
sudo mv daloradius /var/www/html/
sudo chown -R www-data:www-data /var/www/html/daloradius/
sudo chmod 664 /var/www/html/daloradius/library/daloradius.conf.php
```

You should now modify the file `daloradius.conf.php` to adjust the MySQL database information. Open the `daloradius.conf.php` and add the database *username*, *password* and *db name*.

Edit the file `/var/www/html/daloradius/library/daloradius.conf.php`, focusing on the relevant variables to configure are:

```conf
$configValues['CONFIG_DB_HOST'] = 'localhost';
$configValues['CONFIG_DB_PORT'] = '3306';
$configValues['CONFIG_DB_USER'] = 'radius';
$configValues['CONFIG_DB_PASS'] = 'Str0ngR@diusPass';
$configValues['CONFIG_DB_NAME'] = 'radius';
```

To be sure everything works, restart freeradius and apache2

```bash
sudo systemctl restart freeradius.service apache2
```

Open Admin link using your system IP address or domain name `http://ip-address/daloradius/login.php` 

Default login details are:
* *Username*: **administrator** 
* *Password*: **radius**

Change the password after first login.

---
<!-- enrich:containment -->
*Part of: damianflynn*
