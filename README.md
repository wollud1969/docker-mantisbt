`MantisBT` is an open source issue tracker that provides
a delicate balance between simplicity and power.

## Example docker-compose.yml
The examples suppose you will have the data for your containers in `/srv/mantis`. Adapt for your server.

```
mantisbt:
  image: xlrl/mantisbt:latest
  ports:
    - "8989:80"
  links:
    - mysql
  volumes:
    - ./config:/var/www/html/config
	- ./custom:/var/www/html/custom
  restart: always

mysql:
  image: mariadb:latest
  environment:
    - MYSQL_ROOT_PASSWORD=root
    - MYSQL_DATABASE=bugtracker
    - MYSQL_USER=mantisbt
    - MYSQL_PASSWORD=mantisbt
  volumes:
	- ./mysql:/var/lib/mysql
  restart: always
```

> You can use `mysql`/`postgres` instead of `mariadb`.

## Install

```
$ firefox http://localhost:8989/admin/install.php
>>> username: administrator
>>> password: root
```

```
==================================================================================
Installation Options
==================================================================================
Type of Database                                        MySQL/MySQLi
Hostname (for Database Server)                          mysql
Username (for Database)                                 mantisbt
Password (for Database)                                 mantisbt
Database name (for Database)                            bugtracker
Admin Username (to create Database if required)         root
Admin Password (to create Database if required)         root
Print SQL Queries instead of Writing to the Database    [ ]
Attempt Installation                                    [Install/Upgrade Database]
==================================================================================
```

## Email

Append following to `/srv/mantis/config/config_inc.php`

```
$g_phpMailer_method = PHPMAILER_METHOD_SMTP;
$g_administrator_email = 'admin@example.org';
$g_webmaster_email = 'webmaster@example.org';
$g_return_path_email = 'mantisbt@example.org';
$g_from_email = 'mantisbt@example.org';
$g_smtp_host = 'smtp.example.org';
$g_smtp_port = 25;
$g_smtp_connection_mode = 'tls';
$g_smtp_username = 'mantisbt';
$g_smtp_password = '********';
```

## Disabling admin folder

MantisBT recommends to disable access to the admin folder once the initial
configuration has been done.

Do to so, set the environment variable `DISABLE_ADMIN_FOLDER` in your
container startscript of `docker-compose.yml` to `true`.

## LDAP

Append following to `/srv/mantis/config/config_inc.php` for LDAP
authentication against an Active Directory server:

```
$g_login_method = LDAP;
$g_ldap_server = 'ldap://dc.example.com';
$g_ldap_root_dn = 'dc=example,dc=com';
$g_ldap_bind_dn = 'cn=readuser, dc=example, dc=com';
$g_ldap_bind_passwd = 'geheim123';
$g_ldap_organization = '';
$g_use_ldap_email = ON;
$g_use_ldap_realname = ON;
$g_ldap_protocol_version = 3;
$g_ldap_follow_referrals = OFF;
$g_ldap_uid_field = 'sAMAccountName';
```

