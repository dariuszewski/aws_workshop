# LAB2 Configuration of WordPress in AWS

## 0. Configure VM and login
#### Autorization
```
$ eval `ssh-agent`
$ ssh-add ~/id_student
$ s**1
> Identity added ...
```
#### EC2 VM
```
ssh ec2-user@IP_ADDR
```

<hr>

## 1. WP install step-by-step


### 0. read requirements from wordpress site:
- php > 7.4
- MariaDB > 10.1
- Apache HTTP server

-> create a directory for your blog: `sudo mkdir -p /var/www/blog`

## 1. install Apache http server
<hr>

`sudo yum search apache` -> check repo for apache phrase
<br>
```
$ sudo yum install httpd
```
#### -> configure apache server
- http servers on port 8080 in default - > `netstat -lntp`
- port 8080 is not listed, service is not on
- check status of http -> `sudo systemctl status httpd`
- status is loaded but inactive
- start service -> `sudo systemctl start httpd` (start/restart/stop)
- now `netstat -lntp` shows service listening on 8080

Now we can visit IP_ADDR website and see apache Test Page

check if you can serve new content: (check `apache.org`)
```
$ sudo su
$ echo "hello world" > /var/www/blog/index.html
$ cd /etc/httpd/conf.d/
$ nano blog.conf
```
```
#/etc/httpd/conf.d/blog.conf

<VirtualHost *:80>
    DocumentRoot "/var/www/blog"
    DirectoryIndex wordpress/index.php index.html
</VirtualHost>
```
```
$ cd ~
$ sudo systemctl restart httpd
```
content should be visible on machine's IP address

<br>

## 2. install WordPress
<hr>

1. go to wordpress website and copy link to installation, then type:

```
$ wget https://pl.wordpress.org/latest-pl_PL.tar.gz
$ tar -xf latest-pl_PL.tar.gz
```
2. Whole directory have to be moved to /var/www/blog/
```
$ mv wordpress /var/www/blog
$ sudo systemctl restart httpd
```
from now on vm is serving `wordpress/index.php` but it doesn't know PHP language, next step is to install it.

<br>

## 3. Install PHP
<hr>

`https://rpms.remirepo.net/` -> useful php repo


### 1. install EPEL repository:

```
$ yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

### 2. Enable remirepo

```
$ yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

### 3. install PHP $ mySQL packages

```
$ yum install php80 php80-php
$ yum install php-mysqlnd
$ yum install php80-php-pecl-mysql
```
<br>

## 4. Install MariaDB
<hr>

### 1. Create  `/etc/yum.repos.d/MariaDB.repo`

```
[mariadb]
name = MariaDB
baseurl = https://ftp.bme.hu/pub/mirrors/mariadb/yum/10.6/centos7-amd64
gpgkey=https://ftp.bme.hu/pub/mirrors/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

### 2. Install MariaDB server

```
sudo yum install MariaDB-server MariaDB-client
```

### 3. Create DB, user & grant permissions
<br>

1. start DB server
```
$ systemctl start mariadb
$ systemctl status mariadb
$ mysql
```
<br>

2. configure db

snippet: 
```
create database databasename;

create user 'user' identified by 'password';
grant all privileges on databasename.* to 'user'@localhost identified by 'password';
grant all privileges on databasename.* to 'user'@host;
grant all privileges on databasename.* to 'user'@%;

FLUSH PRIVILEGES;
```
snippet corrected

```
create database blog;

create user 'blog' identified by 'blog';

grant all privileges on blog.* to 'blog'@localhost identified by 'blog';

grant all privileges on blog.* to 'blog'@;

FLUSH PRIVILEGES;
```

3. login

```
mysql -u blog -p blog
```


## 5. Conffigure WP with MariaDB
<hr>

4. Put DB config in wordpress config

```
$ cd /var/www/blog/wordpress
$ cp wp-config-sample.php wp-config.php
$ nano wp-config.php
```
- add `blog` anywhere for `user_name` etc