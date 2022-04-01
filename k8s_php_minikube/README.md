
#  K8s deployment with PHP,NGINX,MARIADB  using minikube cluster

## minikube installation guide

       https://minikube.sigs.k8s.io/docs/start/   


## Step 1 – Configure PHP:7.0-fpm
- First, create a new parent directory inside your k8s project with the following command:
       
       mkdir  -p  ~/k8s-project/phpfpm/code/


- Download PHP code  (its a e-commerce simple code )

       git clone https://github.com/kodekloudhub/learning-app-ecommerce   ~/k8s-project/phpfpm/code/
    
 here paste all php data that  along with index.php  


- First, you will need to modify the PHP image and install the PHP extension for mariadb in order to connect to the mariadb database.

- First, create a Dockerfile for PHP with the following command:

           nano  ~/k8s-project/phpfpm/Dockerfile

- Add the following lines:

            FROM php:7.0-fpm  
            RUN docker-php-ext-install mysqli pdo pdo_mysql
            RUN docker-php-ext-enable mysqli
            COPY ./code/ /var/www/html/


Save and close the file.

## Step 2 – Configure an Nginx environment

- First, create a directory for your project with the following command:

 
      mkdir  -p  ~/k8s-project/nginx/code/

- Download PHP code  (its a e-commerce simple code )

       git clone https://github.com/kodekloudhub/learning-app-ecommerce   ~/k8s-project/nginx/code/
    
 here paste all php data that  along with index.php 



- Next, create an Nginx default configuration file to run your PHP application:

       nano ~/k8s-project/nginx/default.conf
Add the following lines:
```
server {  

     listen 80 default_server;  
     root /var/www/html;  
     index index.html index.php;  

     charset utf-8;  

     location / {  
      try_files $uri $uri/ /index.php?$query_string;  
     }  

     location = /favicon.ico { access_log off; log_not_found off; }  
     location = /robots.txt { access_log off; log_not_found off; }  

     access_log off;  
     error_log /var/log/nginx/error.log error;  

     sendfile off;  

     client_max_body_size 100m;  

     location ~ .php$ {  
      fastcgi_split_path_info ^(.+.php)(/.+)$;  
      fastcgi_pass localhost:9000;  
      fastcgi_index index.php;  
      include fastcgi_params;  
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;  
      fastcgi_intercept_errors off;  
      fastcgi_buffer_size 16k;  
      fastcgi_buffers 4 16k;  
    }  

     location ~ /.ht {  
      deny all;  
     }  
    } 

```
```
here look at localhost:9000 , because we use multi-container pod means  nginx,php in one pod  so in that case namespace are same of both container they will share namespace and both container will communicate by 'localhost':'container-port' example :-> localhost:9000
```
Save and close the file.

- Next, create a Dockerfile inside the nginx directory. This will copy the Nginx default config file and php code in /var/www/html/ to the Nginx container. 


         nano ~/k8s-project/nginx/Dockerfile
- Add the following lines:

      FROM nginx

      COPY ./default.conf /etc/nginx/conf.d/default.conf

      COPY ./code/ /var/www/html/


## Step 3 - Create a Docker image of Both and push on DockerHub

         docker image build . -t deepcloudmosphere/nginx:php
         docker image build . -t deepcloudmosphere/php:mysqli


  Note:-> here '.' represent Dockerfile in current directory 

         docker push deepcloudmosphere/nginx:php
         docker push deepcloudmosphere/php:mysqli

## Step 4 - Create a Deployment manifest file with multi-container Pod .

### deploymentphp.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
    name: deploymentphp
    labels:
       tier: frontend

spec:
    replicas: 5
    selector:
       matchLabels:
           app: myphp 
           tier: frontend
   
    template:
         metadata:
             name: php-pod
             labels:
               app: myphp
               tier: frontend
 
         spec:
            containers:
               - name: nginx-ctr
                 image: deepcloudmosphere/nginx:php
                 ports:
                   - containerPort: 80

               - name: phpfpm
                 image: deepcloudmosphere/php:mysqli
                 ports:
                   - containerPort: 9000
```

## Step 5 - Create NodePort type service for deploymentphp.yaml
it will make forword request from Port on Node to port on Pod and user will acces this application by nodePort with node ip ex:->` 192.168.92.18:30005 `

### servicephp.yaml

```
apiVersion: v1
kind: Service
metadata: 
     name: servicephp

spec:  
  type: NodePort
  ports: 
    - targetPort: 80
      port: 80
      nodePort: 30005
  selector:
     app: myphp
     tier: frontend

```

## Step 6 – Configure  a Mariadb manifest file 

### deploymentdb.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
   name: deploymentdb
   labels:
      tier: backend

spec:
    replicas: 2
    selector:
      matchLabels:
          app: mysqldb
          tier: backend
    template:
       metadata:
           name: db-pod
           labels:
             app:  mysqldb
             tier: backend
       spec:
         containers:
           - name: mysqldb-ctr
             image: mysql 
             volumeMounts:
                - name: mysql-data
                  mountPath: var/lib/mysql
             ports:
               - containerPort: 3306
             env: 
              - name: MYSQL_ROOT_PASSWORD
                value: "mysql"
              - name : MYSQL_DATABASE
                value: ecomdb 
              - name : MYSQL_USER
                value: "deepak"
              - name : MYSQL_PASSWORD
                value: "deepak123"


         volumes:
             - name: mysql-data
```

#### Note:-> In `deploymentdb.yaml` file you can see `MYSQL_DATABASE` ,`MYSQL_USER` and `MYSQL_PASSWORD`  need to create because that user and db  are hard coded in index.php file  . 
#### you can see in following <snap> of index.php :-

     <snap>
      <?php
                        $link = mysqli_connect('db', 'deepak', 'deepak123', 'ecomdb');
                        if ($link) {
                        $res = mysqli_query($link, "select * from products;");
                        while ($row = mysqli_fetch_assoc($res)) { ?>

     <snap>

so, it can find database for by db service  name  



### Load data in mariadb  Database.

-  minikube ssh 


- Load Product Inventory Information to database
 ```

cat > db-load-script.sql <<-EOF
USE ecomdb;
CREATE TABLE products (id mediumint(8) unsigned NOT NULL auto_increment,Name varchar(255) default NULL,Price varchar(255) default NULL, ImageUrl varchar(255) default NULL,PRIMARY KEY (id)) AUTO_INCREMENT=1;

INSERT INTO products (Name,Price,ImageUrl) VALUES ("Laptop","100","c-1.png"),("Drone","200","c-2.png"),("VR","300","c-3.png"),("Tablet","50","c-5.png"),("Watch","90","c-6.png"),("Phone Covers","20","c-7.png"),("Phone","80","c-8.png"),("Laptop","150","c-4.png");

EOF

```


- Run sql script

       mysql -u root -pmariadb< db-load-script.sql


 #### Note:-> we mounted a volume in `/var/lib/mysql` , so our data is loaded and persistent.

You can check it by take CLI of mariadb cotainer 

- Take a CLI of db by following command:
  
        docker exec -it [db container id or name ] /bin/sh


- Enter in mariadb:

         mysql -u root -pmariadb  

- use ecomdb

- show tables; 

- select * from [table name]; 

## Step 7 - Create ClusterIP  type service for deploymentdb.yaml

it will map port on db-pod to  port on service and this will make accessible by service name or ClusterIP

#### servicedb.yaml

```
apiVersion: v1
kind: Service
metadata:
    name: db
    labels:
       tier: backend
 
spec:
   type: ClusterIP
   ports: 
    - targetPort: 3306
      port: 3306
   
   selector: 
     app: mysqldb
     tier: backend
  
```


## Step 8 :-> Post all yaml file to api server on master using kubectl CLI tool

  ``` kubectl create -f deploymentphp.yaml ```

  ``` kubectl create -f servicephp.yaml ```

  ``` kubectl create -f deploymentdb.yaml ```

  ``` kubectl create -f servicedb.yaml ```


check all are running or not by :->

 ``` kubectl get  deployment [deployment name ] ```

 ``` kubectl get  service [service name ] ```



## Step 9 ->  Finally test  on your browser..

- Get minikube IP

            minikube ip

Paste ip on browser with service port ex-> `192.168.92.18:30005`

          







