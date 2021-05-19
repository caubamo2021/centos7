#!/bin/sh
yum install -y wget vim sudo
yum install -y epel-release
rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm rpm -Uvh remi-release-7*.rpm
yum update -y
timedatectl set-timezone Asia/Ho_Chi_Minh
timedatectl
yum install -y ntp 
systemctl start ntpd 
systemctl enable ntpd
yum install yum-utils -y
sudo yum-config-manager --enable remi-php56
yum update -y
yum --enablerepo=remi,remi-php56 install php php-cli php-common php-devel php-gd php-mbstring -y  php-mysql php-pdo php-xml php-xmlrpc php-soap
yum --enablerepo=remi,remi-php56 install php-mcrypt php-opcache php-pear php-mcrypt php-opcache php-pear -y
yum install libgearman-devel gcc make -y
pecl install gearman
echo 'extension=gearman.so' >> /etc/php.d/gearman.ini

yum -y install php-fpm

echo "<FilesMatch \.php$>" >> /etc/httpd/conf.d/php.conf
echo "SetHandler "proxy:fcgi://127.0.0.1:9000"" >> /etc/httpd/conf.d/php.conf
echo "</FilesMatch> >> /etc/httpd/conf.d/php.conf" >> /etc/httpd/conf.d/php.conf

systemctl enable php-fpm.service
systemctl start php-fpm.service

sudo yum -y install gcc php-devel php-pear
sudo yum -y install ImageMagick ImageMagick-devel 

sudo pecl install imagick

sudo echo "extension=imagick.so" > /etc/php.d/imagick.ini

wget   http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xfz ioncube_loaders_lin_x86-64.tar.gz
cp   ioncube/ioncube_loader_lin_5.6.so    /usr/lib64/php/modules  

echo "zend_extension = /usr/lib64/php/modules/ioncube_loader_lin_5.6.so" >> /etc/php.ini

yum  install cronie -y

yum  install -y git

yum install nginx -y

touch  /etc/yum.repos.d/MariaDB.repo

echo "[mariadb]" >> /etc/yum.repos.d/MariaDB.repo
echo "name = MariaDB" >> /etc/yum.repos.d/MariaDB.repo
echo "baseurl = http://yum.mariadb.org/10.1/centos7-amd64" >> /etc/yum.repos.d/MariaDB.repo
echo "gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB" >> /etc/yum.repos.d/MariaDB.repo
echo "gpgcheck=1" >> /etc/yum.repos.d/MariaDB.repo

yum install -y MariaDB-client

