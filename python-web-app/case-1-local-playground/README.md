# Objective:

To complete objectives for this case, you'll need to access and clone the ../app-source-code folder and try to execute different commands

## 1. To understand and list down application requirements
  ### Which language/languages are used in the source code**

   ```This application source code  is using python language```
   
  ### Are there multiple services that are packaged in the app?**
    
   ```Yes, there is a Mysql backend service that is packaged within the python source code  for read and write data from database.```
   
  ### Is it a monolith OR a microservice ? How can one differentiate them ?**

   ```Yes it is a monolithic application.```
   
  **Differentiate** 
    
  ``` In monolithic all the functionalities of a project exist in a single codebase that is tightly coupled.```
   
   ![image](https://user-images.githubusercontent.com/98619865/167298363-43fbf490-c2fc-4b07-afd4-9bfaea542691.png)
   
   ```But in microservice application is built as independent components that run each application process or functionality as a service and each service communicate each other via http or other lightweight protocol.```
    
   ![image](https://user-images.githubusercontent.com/98619865/167298374-21cb8864-9028-4818-8f4d-0ceabe3ef87e.png)
    


 ### What are the networking requirements for the app ?**

    - Is it a batch job that runs in the background linux process?

        ```No, it is not a batch job because ,a batch job is a scheduled program that is assigned to run on a computer without further user interaction.```

        ```This is synchronous task and it is fine when a user needs the result of calculation immediately.``` 

        ```Another use case is when the result is not relevant right now and the user just wants to schedule an execution of the task asynchronously that are run in background in linux process.```

    - Does it listen or expose any information on a port?

      ```this application is not exposing or documenting any port it just listen  on  port 5000 for flask  and 3306 for mysql database  by default.```
    - What kind of port binding is required for it to run?
      ```this application is not required any kind of port binding to run. because to run application simply execute file by python3 on locally that is listen port 5000 by default. ```

- Imp: If application is stateful or stateless? What if the difference?
- Dependency tree for application ( on what other application or distributed systems does it depend on? There can be other services/microservices or DBs probably with whome it interacts?)



## 2. Build application locally to understand the build process and dependencies.

  ### Application is dockerized or not?**

  ```No application is not dockerize its just in form of source code.```

  ### If the application is not dockerized can we dockerize it or not?**
    ``` 
    Yes, we can dockerize application with two simple steps

          1. Create docker file 
          2. build image with the help of docker file. 
    ```

   ### How can we build and test application locally ? Docker compose or similar solution ?**

- ``` To run application locally first install dependencies that are present in requirements.txt ```

  ![Screenshot 2022-05-15 at 5 41 00 PM](https://user-images.githubusercontent.com/98619865/168472308-93401e52-baca-478f-9114-0628c87c560c.png)


-  Run ```pip3 install -r requirements.txt``` to install dependencies.

- Run ``` python3 app.py ``` 




  ``` Before building application we must have to create a Dockerfile of that application source code.```

  ``` In this case we also take care of security and size of the image by using distroless image  provided by google.```

  - ```security is important because there is always a risk of open vulnerabilities in the base image thats why  using distroless image is better choice. ```

  - ```“Distroless” images contain only your application and its runtime dependencies. They do not contain package managers, shells or any other programs you would expect to find in a standard Linux distribution.```

  ```Now, We can use a `multi-stage`  build docker file to run our production containers with a distroless image.```

    - ```Multi-stage builds in our Dockerfile allow for as many stages of the build process. We can daisy-chain the outputs of one stage as inputs to the next stage. The only expectation is that the last stage will be our production stage with the distroless image.```



![Screenshot 2022-05-11 at 11 16 01 PM](https://user-images.githubusercontent.com/98619865/167915545-27084b3f-d85b-4c6a-910c-976b5ac30c76.png)




  ```By default, many containers are configured to execute as root, which is needed to install packages and make configuration settings. But after all that is done, we can change to a non-root user. distroless image is already included 'nonroot' user with user id (65532:65532).```



  ----------Build image by `docker image build -t <tag>` or `docker build -t <tag>` command----------
  
  ![Screenshot 2022-05-11 at 11 39 57 PM](https://user-images.githubusercontent.com/98619865/167918061-8e664c68-7e6f-4a76-b84c-81cf699e4a01.png)


------------check image by `docker images` command --------------

    - You can see image name is `deepcloudmosphere/python-web` with tag name `distroless`

 ![Screenshot 2022-05-11 at 11 45 28 PM](https://user-images.githubusercontent.com/98619865/167918736-a4b1b5fe-ebac-488f-864e-4551050ad748.png)

----------------------------------------------**Finally test application**---------------------------------------------------

  `But, first we need to run database container before python-web container.`
  
  -------run database container by `docker container run -d --name db_host1 -e MYSQL_ROOT_PASSWORD=mysql -e MYSQL_USER=db_user  -e MYSQL_PASSWORD=db_pass  -e MYSQL_DATABASE=db_name --expose 3306 mysql` command  and run `docker ps` to ensure that database is running ---------------------
  
  
  ![Screenshot 2022-05-15 at 12 54 15 PM](https://user-images.githubusercontent.com/98619865/168461984-bdb3a447-f630-4711-8659-5928be76e9ef.png)  
  




  ----------test web application by  ` docker container run -p 2424:5000 --name python-web1 -e MYSQL_ROOT_PASSWORD='mysql' -e DB_HOST='db_host1' -e DB_USER='db_user' -e DB_PASS='db_pass' -e DB_NAME='db_name' --link db_host1:db_host1 deepcloudmosphere/python-web:distroless
` command ----------

NOTE:-> ` --link db_host1:db_host1 ` section link the `db_host1` container  to python-web1 container that are listining `db_host1`


![Screenshot 2022-05-15 at 12 58 06 PM](https://user-images.githubusercontent.com/98619865/168462089-df4cb482-959d-498e-9bd2-82eb21e71195.png)




  **Docker compose**

- As you know Docker is a great tool to Encapsulate microservices and working together make a form of useful  application....
- Instead of gluing each  microservices together with script and long Docker command  previously we did that can be hard this is where Docker Compose comes.
- Docker Compose let's you describe entire app(microservices) in a single declarative YAML file and deploy it with a single command. Once app is deployed, you can manage it entire life cycle  with a single set of command.

  **step-1**  ``` create yaml file```

 ![Screenshot 2022-05-15 at 5 26 16 PM](https://user-images.githubusercontent.com/98619865/168471557-de4f3859-62ca-4f86-84fc-cd43913c2620.png)

 
  **step-2** Run command ```docker-compose -f docker-compose.dev.yml up```

![Screenshot 2022-05-15 at 5 34 50 PM](https://user-images.githubusercontent.com/98619865/168471913-51f1e33b-6ed0-4e7e-b16a-2dbf0ba7b386.png)


  ### If application runs as binary can we write a service file**
  
  ```firstly go to root user```
  
  #### first we need to make a binary by pyinstaller([How to install pyinstaller?])(https://medium.com/analytics-vidhya/how-to-create-executable-of-your-python-application-from-linux-windows-mac-bcbcdd4603d4) 
  
  ![Screenshot 2022-05-18 at 11 38 11 PM](https://user-images.githubusercontent.com/98619865/169143641-5884e859-a7b5-42c4-a17b-d741b713753b.png)    
  
  ``` Now test it locally ```
  
  ``` but first configure the database by changing root password by following command ```
  
  ![Screenshot 2022-05-19 at 1 14 04 AM](https://user-images.githubusercontent.com/98619865/169143994-b6d8ec25-8227-42cd-9713-babff47c685a.png)  
   
  ``` Start and stop mysql.service by systemctl command as root user ```
  
   Apply environment variable :-> ```  export DB_USER='root' DB_PASS='Dountless@123' DB_HOST='localhost' DB_NAME='deepak';```
   
  
  ``` Now test binary file dirctly run in bash or ```./app``` like this o..
  
  ![Screenshot 2022-05-19 at 2 09 57 AM](https://user-images.githubusercontent.com/98619865/169151891-3a03285a-abf2-4d87-a9a3-9ce0ac62ca47.png)  

  ``` Now we have a binary file so,Yes we can write a service file ``` 
  ![Screenshot 2022-05-19 at 10 15 28 AM](https://user-images.githubusercontent.com/98619865/169208508-71bdd2df-db07-489f-9920-35cf8d171e84.png)  
  - You can see the Output by
  
    ``` sudo systemctl deamon-reload ```
    
    ``` sudo systemctl restart app.service ```
    
    ``` sudo systemctl status app.service ```
  
![Screenshot 2022-05-19 at 10 14 07 AM](https://user-images.githubusercontent.com/98619865/169208684-9d9d237c-8320-430b-b39f-cac5ad9dad76.png)
  
  
