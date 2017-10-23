
# How to Install Apache 2.4.x, MariaDB 10.x, and PHP 7.x on Ubuntu 16.04


!(LAMP)[http://resource.thaicreate.com/upload/tutorial/mariadb-apache-php-replace-mysql-01.jpg]

---

When deploying a web site or a web app, the most common web service solution for that is to setup a LAMP stack which consists of Linux, Apache, MySQL, and PHP.

In this article, we will learn how to setup an up-to-date LAMP stack by installing the latest stable releases of **Apache 2.4.x, MariaDB 10.x, and PHP 7.x** on Ubuntu 16.04.


Let's GO :exclamation::exclamation:

## Prerequisites

* An up-to-date Ubuntu 16.04 x64 server instance.
* A sudo user. See instructions for Debian in this Vultr article.
* Do all updates if #apt-get update

## The instrucstions are separedes in the 4 Steps

1. Apache Install
2. MariaDB Install
3. PHP Install
4. Firewall Config

#### Step 1: Install Apache 2.4.x

Install the latest stable release of **Apache 2.4.x** using the following command:

_sudo apt-get install apache2 -y_
Use the below command to confirm the installation:

_apache2 -v_
The output should resemble:

Server version: Apache/2.4.18 (Ubuntu)
Server built:   2016-07-14T12:32:26
In a production environment, you will want to remove the default Ubuntu Apache welcome page:

_sudo mv /var/www/html/index.html /var/www/html/index.html.bak_
For security purposes, you should prevent Apache from exposing files and directories within the web root directory /var/www/html to visitors:

_sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.bak_
_sudo sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf_
Note: In accordance with your specific requirements, you can customize more settings in that file later.

Start the Apache service and make it start on system boot:

_sudo systemctl start apache2.service_
_sudo systemctl enable apache2.service_

#### Step 2: Install MariaDB 10.x

At the time of writing this article, the current stable release of MariaDB is 10.1. You can use the following commands to install MariaDB 10.1 on your Ubuntu 16.04 x64 system.

Setup the system apt repo:

_sudo apt-get install software-properties-common_
_sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8_
_sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://mirror.jmu.edu/pub/mariadb/repo/10.1/ubuntu xenial main'
Install MariaDB:_

_sudo apt update -y_
_sudo apt install -y mariadb-server_

During the installation process, the MariaDB package configuration wizard will automatically pop up and ask you to setup a new password for the MariaDB root user. For now, just press Enter every time the wizard pops up to skip this step because we will setup a password for the MariaDB root user in the following securing MariaDB procedure.

Having MariaDB installed, you can confirm the installation with:

_mysql -V_
The output should be similar to:

mysql  Ver 15.1 Distrib 10.1.22-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
Start the MariaDB service:

_sudo systemctl start mariadb.service_
_sudo systemctl enable mariadb.service_


Secure the installation of MariaDB:

_sudo /usr/bin/mysql_secure_installation_
During the interactive process, answer questions one by one as follows:

Enter current password for root (enter for none): <Enter>
Set root password? [Y/n]: Y
New password: <your-MariaDB-root-password>
Re-enter new password: <your-MariaDB-root-password>
Remove anonymous users? [Y/n]: Y
Disallow root login remotely? [Y/n]: Y
Remove test database and access to it? [Y/n]: Y
Reload privilege tables now? [Y/n]: Y
Note: Be sure to replace <your-MariaDB-root-password> with your own MariaDB root password.

In this fashion, MariaDB 10.1 has been securely installed onto your system. In the future, you can setup designated users and databases for your web apps as follows:

Log into the MySQL shell as root:

mysql -u root -p
Type the MariaDB root password you set earlier when prompted.

Create a MariaDB database webapp, a database user webappuser, and the database user's password yourpassword:

CREATE DATABASE webapp;
CREATE USER 'webappuser'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON webapp.* TO 'webappuser'@'localhost' IDENTIFIED BY 'yourpassword' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
If necessary, you can customize MariaDB by reviewing and editing the main MariaDB config file which is /etc/mysql/my.cnf:

_sudo cp /etc/mysql/my.cnf /etc/mysql/my.cnf.bak_
_sudo vi /etc/mysql/my.cnf_
Remember to restart the MariaDB service if you make any modifications to that file:

_sudo systemctl restart mariadb.service_


### Step 3: Install PHP 7.0 or 7.1

When dealing with PHP 7.x, please refer to another Vultr article which describes the process in detail.


### Step 4: Setup the UFW firewall

By default, the UFW firewall on Ubuntu 16.04 is inactive. You should enable the UFW firewall in order to enhance security:

_sudo ufw app list_
_sudo ufw allow OpenSSH_
_sudo ufw allow in "Apache Full"_
_sudo ufw enable_
That's all. After going through the above procedures, the LAMP stack would have been up and running on your Ubuntu 16.04 system. You can then deploy your own web app on the basis of the LAMP stack.

Enjoy it! :metal: :metal:
[More About Apache](http://www.apache.org)
[More About MariaDB](http://www.mariadb.org)
[More About PHP](http://www.php.net)
