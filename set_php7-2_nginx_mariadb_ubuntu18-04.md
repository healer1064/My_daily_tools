/***** set server for yii2 in ubuntu 18.04 (nginx, php7.2, mariadb) ******/

1. ��ѧ�ҡ���Դ��� ubuntu ����
    /*** ������ ***/
    apt install net-tools
    apt update
    apt upgrate

2. �Դ��� nginx
    /*** ������ ***/
    apt install nginx
    
    /*** ���ͺ nginx ***/
    �Դ browser -> localhost ���� 127.0.0.1

    /*** part default ����Ѻ�ʴ��� ***/
    /var/www/html

    

3. �Դ��� mariadb
    /*** ������ ***/
    apt install mariadb-server


    /*** ��駤�� password ***/
    mysql_secure_installation

    Enter current password for root(enter for none): Enter    
    
    Change the root password? [Y/n]: Y
    
    New password: password

    Re-enter new password: password
    
    Remove anonymous user? [Y/n]: Y
    
    Disallow root login remotely? [Y/n]: Y
    
    Remove test database and access now? [Y/n]: Y
 
    Reload privilege tables now? [Y/n]: Y


    /***login to database***/
    mysql -u root -p
    password: password

    /*** ���ҧ database ***/
    create database dbname;

    /*** �ʴ���¡�� database ��� table ***/
    show databases;
    show tables;

    /*** ��ҵ�ͧ�������¹ password mariadb ***/    
    use mysql;
    UPDATE user SET plugin = �� WHERE user=�root�;
    FLUSH PRIVILEGES;
    exit;
    service mysqld restart

4. �Դ��� extensions ���ͧ������Ѻ php7.2
    /*** ������ ***/    
    apt install php7.2-fpm
    apt install php7.2-mbstring
    apt install php7.2-xml

    /*** 㹡óշ�� ubuntu �ѧ�����Դ��� zip ��� unzip #��ͧ�Դ������Шз������������ö composer install �� ***/
    apt install zip unzip

5. �Դ��� project
    /*** �����ѧ part ���������Ѻ�ʴ������ ***/
    cd /var/www
    
    /*** ������ ***/
    git clone <url>
    
    cd projectname
    
    sudo php init

    /*** ��駤���������͡Ѻ database ***/
    nano /var/www/pov-website/common/config/main-local.php
    /***file main-local.php***/
   	 'class' => 'yii\db\Connection',
       		 'dsn' => 'mysql:host=127.0.0.1;dbname=pov',
       		 'username' => 'root',
       		 'password' => 'password',
       		 'charset' => 'utf8',
    /***end file***/

6. ��駤�� part ���˹觷���ʴ������
    cd /etc/nginx/sites-enable/
    nano default
    
    /****file default****/
   	 
   	 server{
   		 listen 80 default_server;
   	     	listen [::]:80 default_server;

   		 root /var/www/pov-website;

   			 charset utf-8;
   			 client_max_body_size 128M;

   		 index index.php index.html index.htm index.nginx-debian.html;

   			 server_name localhost;

   		 location / {
           	 
           			}

   		 location ~ \.php$ {
           			 fastcgi_pass unix:/run/php/php7.2-fpm.sock;
           			 fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           			 include fastcgi_params;
   		   		 }
   		 location ~ /\.ht {
           			 deny all;
   			 }

   	 }
    
    /*****end file*****/

    nginx -s reload
    nginx -s reopen

    /*** ���ͺ��÷ӧҹ�����Ѻ�ͧ php �Ѻ nginx ***/
    
    cd /var/www/pov-website/

    nano info.php

    /*****file info.php*****/
   	 <?php
   		 phpinfo();
   		 
    /*****end file*****/


  		 /***run info.php***/
    �Դ browser -> localhost/info.php  ���� ip �ͧ server/info.php #����Դ info.php ���ʴ��������ö��ҹ php �����Ѻ nginx ������

7. �Դ��� extensions �����������Ѻ��ҹ�Ѻ yii2
    �� requirements �ͧ yii2 #�Դ��駵�� extension ����ѧ�����Դ���
    �Դ browser -> localhost/requirements.php
    ��    apt install php7.2-intl
   	 apt install php7.2-sqlite
   	 apt install php7.2-mysql
   	 apt install php7.2-gd
    �繵�

8. �Դ��� composer � project
    /*** ���价�� part �ͧ project ***/
    cd /var/www/pov-web-site
    
    /*** ���ҧ������� vendor �����䢡��͹حҵ�������ö ��¹ ��ҹ ź �� �������� vendor ��� composer.json ��� composer.lock ***/
   	 sudo mkdir vendor
   	 
   	 sudo chmod -R 777 vendor
   	 sudo chmod -R 777 composer.json
   	 sudo chmod -R 777 composer.lock    
   		 
    
    composer install

9. migrate database
    /*** ���价�� part �ͧ project *///
    cd /var/www/pov-web-site

    /*** migrate database ***/
    ./yii migrate

    /*** ���ͺ run project ***/
    �Դ browser -> localhost/frontend/web



/********* �óշ���ͧ����Ѿ���������ҡ���� 2M *********/
     /*** ������ php.ini �ͧ������ fpm ��� cli �������� ***/     
     nano /etc/php7.2/fpm/php.ini
     /*** �������繴ѧ��� ***/
     upload_max_filesize = 20M
     post_max_size = 20M  
     
     nano /etc/php7.2/cli/php.ini
     /*** �������繴ѧ��� ***/
     upload_max_filesize = 20M
     post_max_size = 20M  
     
     /*** ������ nginx.conf �������� ***/
     nano /etc/nginx/nginx.conf
     /*** ����������������ǹ�ͧ http {} �ѧ��� ***/
     Http {
	client max_body_size 20m;
     }

     /*** Restart php-fpm ��� nginx ���¤���� ***/
     Service php7.2-fpm restart && service nginx restart


