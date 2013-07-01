1.	Ссылка для скачивания http://www.ubuntu.com/download/server/thank-you?distro=server&bits=32&release=lts (32-х битная версия).
2.	Включена УЗ root с паролем 12345.
3.	Установка базовых пакетов:

apt-get install mc

apt-get install samba – в конец файла /etc/samba/smb.conf добавляем

[global]
	security = share

[webhome]
    comment = webhome directory
    path = /webhome
    read only = no
    writable = yes
    create mask = 0777
    directory mask = 0777
    guest ok = yes

[root]
    comment = root
    path = /
    read only = no
    writable = yes
    create mask = 0777
    directory mask = 0777
    guest ok = yes

4.	Установка apache и php
apt-get install apache2 php5 php5-gd php5-memcache php5-mysql php5-pgsql php5-xdebug php-pear – в файле /etc/apache2/ports.conf

NameVirtualHost *:80 меняем на NameVirtualHost *

В файле /etc/apache2/httpd.conf прописываем

SetEnv PROJECT_MODE_DEVELOP 1
SetEnv adminallow 1

Alias /js/a "/usr/local/lib/js"
AliasMatch ^/js/v/(.*)-version-(\d+)\.(jpg|jpeg|gif|png|js|css|txt|htm)$ /usr/local/lib/js/$1.$3
AliasMatch ^/js/v/(.*)$ /usr/local/lib/js/$1

<Directory "/webhome">
    Options ExecCGI Includes SymLinksIfOwnerMatch
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>

Подключаем модуль rewrite путем создания ссылки на /etc/apache2/mods-available/rewrite.load в /etc/apache2/mods-enabled.
В файле /etc/php5/apache2/php.ini меняем настройки:

serialize_precision = 100
disable_functions =
include_path = ".:/usr/share/php:/usr/local/lib/php"
session.gc_probability = 1

5.	Установка прав 777 на папки /usr/share, /usr/local/lib. Перенос библиотек из папок /usr/share/php, /usr/local/lib/js, /usr/local/lib/php
6.	Обновляем парсер
apt-get install re2c
apt-get install libpcre3-dev
7.	Установка модулей postgresql
apt-get install postgresql-common postgresql-client-common
8.	Установка балансировщика postgresql
apt-get install pgbouncer
9.	Установка ssh сервера
apt-get install openssh-server
10.	Установка svn
apt-get install subversion
Перенос файла настроек из папки /.subversion
11.	Установка дополнительных библиотек
apt-get install build-essential
12.	Установка gearman
apt-get install gearman gearman-job-server gearman-server gearman-tools
apt-get install python-software-properties
add-apt-repository ppa:gearman-developers/ppa
apt-get install libgearman-dev
pecl install gearman-1.0.3
echo "extension=gearman.so" > /etc/php5/conf.d/gearman.ini
13.	Установка библиотеки oauth
apt-get install liboauth-php
pecl install oauth
echo "extension=oauth.so" > /etc/php5/conf.d/oauth.ini
14.	Перенос папок debls, hoteladmin, ServiceCheck, sqlrelay-monitor, tourindex.ru в папку /webhome.
15.	Перенос сетевых настроек /etc/network/interfaces.
В описание интерфейса сетевой карты добавить
dns-nameservers 213.85.204.190, 213.85.204.220
16.	Перенос настроек локальных доменов /etc/apache2/sites-enabled.