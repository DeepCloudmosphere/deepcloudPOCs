{\rtf1\ansi\ansicpg1252\cocoartf2638
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fnil\fcharset0 Menlo-Regular;\f1\fnil\fcharset0 Menlo-Italic;}
{\colortbl;\red255\green255\blue255;\red70\green137\blue204;\red23\green23\blue23;\red202\green202\blue202;
\red194\green126\blue101;\red99\green159\blue215;}
{\*\expandedcolortbl;;\cssrgb\c33725\c61176\c83922;\cssrgb\c11765\c11765\c11765;\cssrgb\c83137\c83137\c83137;
\cssrgb\c80784\c56863\c47059;\cssrgb\c45490\c69020\c87451;}
\paperw11900\paperh16840\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\deftab720
\pard\pardeftab720\partightenfactor0

\f0\fs24 \cf2 \cb3 \expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 # Step 1 \'96 Create an Nginx Container\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf4 \cb3 Before starting, you will need to create and launch an Nginx container to host the PHP application.\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 First, create a directory for your project with the following command:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3  \cb1 \
\cb3       mkdir ~/docker-project\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Next, change the directory to your project and create a docker-compose.yml file to launch the Nginx container.\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3              cd ~/docker-project\cb1 \
\cb3              nano docker-compose.yml\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Add the following lines:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf6 \cb3 \strokec6 nginx:   \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       image: nginx:latest  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       container_name: nginx-container  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       ports:   \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6        - 80:80 \cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5  ```\cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3  it will make sure nginx container is running on port 80.\cb1 \
\cb3  Save and close the file when you are finished.\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2  - \cf4 \strokec4 Next, launch the Nginx container with the following command:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3        docker-compose up -d\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2  - \cf4 \strokec4 You can check the running container with the following command:\cb1 \
\pard\pardeftab720\partightenfactor0
\cf4 \cb3    \cb1 \
\cb3         docker ps\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2  - \cf4 \strokec4 You should see the following output:\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf6 \cb3 \strokec6 CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                               NAMES\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6 c6641e4d5bbf   nginx:latest   "/docker-entrypoint.\'85"   5 seconds ago   Up 3 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp   nginx-container\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Now, open your web browser and access your Nginx container using the URL http://your-server-ip. You should see the Nginx test page on the following screen:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3 image..............\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 # Step 2 \'96 Create a PHP Container\cf4 \cb1 \strokec4 \
\cf2 \cb3 \strokec2 - \cf4 \strokec4 First, create a new directory inside your project with the following command:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3       mkdir  ~/docker-project/php_code\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Download PHP code  (its a e-commerce simple code )\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3        git clone https://github.com/kodekloudhub/learning-app-ecommerce ~/docker-project/php_code/\cb1 \
\
\cb3  here paste all php data that  along with index.php  \cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 First, you will need to modify the PHP image and install the PHP extension for mariadb in order to connect to the mariadb database.\cb1 \
\
\cf2 \cb3 \strokec2 - \cf4 \strokec4 First, create a Dockerfile for PHP with the following command:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3            nano  ~/docker-project/php_code/Dockerfile\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Add the following lines:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3             FROM php:7.0-fpm  \cb1 \
\cb3             RUN docker-php-ext-install mysqli pdo pdo_mysql\cb1 \
\cb3             RUN docker-php-ext-enable mysqli\cb1 \
\
\cb3 Save and close the file.\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 create a directory for Nginx inside your project directory:\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3        mkdir ~/docker-project/nginx\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Next, create an Nginx default configuration file to run your PHP application:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3        nano ~/docker-project/nginx/default.conf\cb1 \
\cb3 Add the following lines:\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf6 \cb3 \strokec6 server \{  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      listen 80 default_server;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      root /var/www/html;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      index index.html index.php;  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      charset utf-8;  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      location / \{  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       try_files $uri $uri/ /index.php?$query_string;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      \}  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      location = /favicon.ico \{ access_log off; log_not_found off; \}  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      location = /robots.txt \{ access_log off; log_not_found off; \}  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      access_log off;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      error_log /var/log/nginx/error.log error;  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      sendfile off;  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      client_max_body_size 100m;  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      location ~ .php$ \{  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       fastcgi_split_path_info ^(.+.php)(/.+)$;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       fastcgi_pass php:9000;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       fastcgi_index index.php;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       include fastcgi_params;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       fastcgi_intercept_errors off;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       fastcgi_buffer_size 16k;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       fastcgi_buffers 4 16k;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6     \}  \cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6      location ~ /.ht \{  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       deny all;  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      \}  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6     \} \cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf4 \cb3 Save and close the file.\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Next, create a Dockerfile inside the nginx directory. This will copy the Nginx default config file to the Nginx container. \cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3          nano ~/docker-project/nginx/Dockerfile\cb1 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Add the following lines:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3          FROM nginx\cb1 \
\
\cb3          COPY ./default.conf /etc/nginx/conf.d/default.conf\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Next, edit the docker-compose.yml file:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3        nano ~/docker-project/docker-compose.yml\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Remove the old contents and add the following contents:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf6 \cb3 \strokec6 version: "3.9"\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6 services:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6    nginx:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      build: ./nginx/\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      ports:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6        - 80:80\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6   \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      volumes:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6          - ./php_code/:/var/www/html/\cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6    php:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      build: ./php_code/\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      expose:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6        - 9000\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      volumes:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6         - ./php_code/:/var/www/html/\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Now, launch the container with the following command:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3             cd ~/docker-project\cb1 \
\cb3             docker-compose up -d\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 You can verify the running containers with the following command:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3         docker ps\cb1 \
\
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Now, open your web browser and access the URL http://your-server-ip or localhot . You should see your php web content \cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf6 \cb3 \strokec6 `but here  is something wrong when you invoke url your php is running properly but output is not that we want  we want fetch data from database and display it so lets create database.`\cf4 \cb1 \strokec4 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 # Step 3 \'96 Create a Mariadb Container\cf4 \cb1 \strokec4 \
\
\cf2 \cb3 \strokec2 - \cf4 \strokec4 In this section, we will create a mariadb database container and linked it to all other containers. \cb1 \
\
\cf2 \cb3 \strokec2 - \cf4 \strokec4  Then, edit the docker-compose.yml file to create entry for  a \cf6 \strokec6 `mariadb`\cf4 \strokec4  container\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3                 nano ~/docker-project/docker-compose.yml\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Make the following changes:\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf6 \cb3 \strokec6 version: "3.9"\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6 services:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6    nginx:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      build: ./nginx/\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      ports:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6        - 80:80\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6   \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      volumes:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6          - ./php_code/:/var/www/html/\cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6    php:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      build: ./php_code/\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      expose:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6        - 9000\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6      volumes:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6         - ./php_code/:/var/www/html/\cf4 \cb1 \strokec4 \
\
\
\cf6 \cb3 \strokec6    db:    \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       image: mariadb  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       volumes: \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6         -    mysql-data:/var/lib/mysql\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6       environment:  \cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6        MYSQL_ROOT_PASSWORD: mariadb\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6        MYSQL_DATABASE: ecomdb \cf4 \cb1 \strokec4 \
\
\
\cf6 \cb3 \strokec6 volumes:\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6     mysql-data:\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5   ```\cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ## Configure Database\cf4 \cb1 \strokec4 \
\
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Take a CLI of db by following command:\cb1 \
\pard\pardeftab720\partightenfactor0
\cf4 \cb3   \cb1 \
\cb3                  docker exec -it \cf5 \strokec5 [db container id or name ]\cf4 \strokec4  /bin/sh\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Enter in mariadb:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3          mysql -u root -pmariadb  \cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Create user in db:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3            CREATE USER 'deepak'@'%'  IDENTIFIED BY "deepak123";\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4  Provide ALL PRIVILEGES to user :\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3            GRANT ALL PRIVILEGES on 
\f1\i *.*
\f0\i0  to 'deepak'@'%';\cb1 \
\cb3            FLUSH PRIVILEGES;\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 back to db bash :\cb1 \
\pard\pardeftab720\partightenfactor0
\cf4 \cb3  \cb1 \
\
\cb3           exit \cb1 \
\
\
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Load Product Inventory Information to database\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5  ```\cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf6 \cb3 \strokec6 cat > db-load-script.sql <<-EOF\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6 USE ecomdb;\cf4 \cb1 \strokec4 \
\cf6 \cb3 \strokec6 CREATE TABLE products (id mediumint(8) unsigned NOT NULL auto_increment,Name varchar(255) default NULL,Price varchar(255) default NULL, ImageUrl varchar(255) default NULL,PRIMARY KEY (id)) AUTO_INCREMENT=1;\cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6 INSERT INTO products (Name,Price,ImageUrl) VALUES ("Laptop","100","c-1.png"),("Drone","200","c-2.png"),("VR","300","c-3.png"),("Tablet","50","c-5.png"),("Watch","90","c-6.png"),("Phone Covers","20","c-7.png"),("Phone","80","c-8.png"),("Laptop","150","c-4.png");\cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6 EOF\cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Run sql script\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3              mysql -u root -pmariadb< db-load-script.sql\cb1 \
\
\
\cb3  we mounted a volume in db \cf6 \strokec6 `/var/lib/mysql`\cf4 \strokec4  so our data is loaded and persistent.\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf6 \cb3 \strokec6 NOTE:->\cf4 \cb1 \strokec4 \
\
\
\cf6 \cb3 \strokec6 'deepak'@'%' :-> 'user'@'any host'\cf4 \cb1 \strokec4 \
\
\cf6 \cb3 \strokec6 *.*.         :-> all databases . all tables \cf4 \cb1 \strokec4 \
\
\
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf4 \cb1 \strokec4 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Exit bash of db:\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3              exit\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ## Update index.php\cf4 \cb1 \strokec4 \
\
\cf2 \cb3 \strokec2 - \cf4 \strokec4 Update index.php file to connect to the right database server.\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf4 \cb3         sudo sed -i 's/172.20.1.101/db/g' /var/www/html/index.php\cb1 \
\
\cb3        \cf2 \strokec2 <snap>\cf4 \cb1 \strokec4 \
\cb3       <?php\cb1 \
\cb3                         $link = mysqli_connect('db', 'deepak', 'deepak123', 'ecomdb');\cb1 \
\cb3                         if ($link) \{\cb1 \
\cb3                         $res = mysqli_query($link, "select * from products;");\cb1 \
\cb3                         while ($row = mysqli_fetch_assoc($res)) \{ ?>\cb1 \
\
\cb3        \cf2 \strokec2 <snap>\cf4 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 - \cf4 \strokec4 so, it can find database for by db name . because all container are communicate by name of container in same natwork.\cb1 \
\
\
\
\cf2 \cb3 \strokec2 ### chek URL and refresh it........\cf4 \cb1 \strokec4 \
}