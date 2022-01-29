# LAB3 Automatic installation of WP and it's dependencies on EC2 machine using Ansible


## 1. List of servers

<hr>

>To start we need to create a file with list of machines we want to automatically configure. To do this we will fill up hosts.ini with server IPs and default usernames.

(still we have to load ssh key before)

```
nano files/hosts.ini
```
#### hosts.ini
```
[blog_nodes]
18.158.60.31   ansible_user=ec2-user
```

check connection (use from hosts.ini dir):

```
$ ansible -m ping -i hosts.ini blog_nodes
```
 and it should return 
 
 ```
18.158.60.31 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
 ```

<br>

## 2. Creating .yaml config file

<hr>

```
nano files/setup_wp.yaml
```

`setup_wp.yaml` will store all commands and will be used to call them in order to automatically set WP in our machine as in `LAB02`.
<br>

`.yaml` is very similar to Python notation. it is focusing on removing unnecessary braces and replace them with indentation (using double space).
<br>

`docs.ansible.com` is a documentation for all popular modules available within service.

to activate `setup_wp.yaml` as a script type:

```
$ ansible-playbook -i hosts.ini setup_wp.yaml
```

## 3. Setting up `httpd-vhost.conf` file
<hr>

File below will be needed ne EC2 to run WP. It's same as in LAB02.

```
<VirtualHost *:80>
    DocumentRoot "/var/www/wordpress"
    DirectoryIndex index.php index.html
</VirtualHost>
```

## 4. DBMS `mariadb.repo` file
<hr>

Similarly to LAB02, we will also need cofiguration file for DBMS as follows:

```
[mariadb]
name = MariaDB
baseurl = https://ftp.bme.hu/pub/mirrors/mariadb/yum/10.6/centos7-amd64
gpgkey=https://ftp.bme.hu/pub/mirrors/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

## 5. Edit `wp_config`
<hr>

```
<?php
define('DB_NAME', '{{ DB_NAME }}');
define('DB_USER', '{{ DB_USER }}');
define('DB_PASSWORD', '{{ DB_PW }}');
define('DB_HOST', 'localhost');
define('DB_CHARSET', 'utf8mb4');
define('DB_COLLATE', '');
define('AUTH_KEY', 'DjqDFfs5yaqII6Rtu*X*GRITUBJ/qs^[4/erzySZ0TyCKY,.t>0ih3KZt&r9E$A%');
define('SECURE_AUTH_KEY','N8 r%1UJ5R#>^&)faeBE1]c*eVaWt !fT3H6V2YU(brKT)IUKmgr1@+foewOI6<?');
define('LOGGED_IN_KEY','AG8c,H0w%]..(6}gGZ3Vnb7ezz2_oU;ny1S1@y:}[esplT2Y>Dr64Ebh/l`s~3+:');
define('NONCE_KEY','U~z5NGSnH~wZr?;QW8ZXBk.ECFjbb])RvM).~;h$+G!QIGN;yEA2yU:3p1+{(]lF');
define('AUTH_SALT','kFVL:5&k^tv.1H=4?7/yaTw]8_SNI59i/l^6y8[`-Hggn52Ara/?Wrb?$-J4t1Rx');
define('SECURE_AUTH_SALT', 'B=u)zF>(yPL*Hx{Kl,Qep:},Q|jAQ,i`M_>tfsz]RGSz_JfZc8Y[CKj+ LA/=+XL');
define('LOGGED_IN_SALT', '%@t)jOu$X{?Qi;$Z<3?p&74LbJEiwhVHRiiEncmWOSi*IMQ9P/oQ~T2e;kg1NF6,');
define('NONCE_SALT', '$}~|o$2km=`OyJZOW$BXB`2T,B~3@<)4JIWTg8fv-XPXlRhyf]eg4)pSAa33ms7z');
define('WP_DEBUG', false);
$table_prefix = 'wp_';
if (! defined('ABSPATH')) {
define('ABSPATH', __DIR__ . '/');
}
require_once ABSPATH . 'wp-settings.php';
?>
```