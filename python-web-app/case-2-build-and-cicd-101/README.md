Objective:     
To complete objectives for this case, you'll need to access and clone the ../app-source-code folder and try to execute different commands

# 1. Understanding artifacts in an SDLC: 
Artifacts can be documents, architecture diagrams or application binaries, docker images etc. Basically anything that is deliverable to different or same business line.

<<<<<<< HEAD
Thought process:
- What type of artifacts are required in order to run the application
- # How we can define the build and release cycles using these artifacts ?
- What gets updates when there is a change in the application ( can be source code change due to new release, can be network change, can be firewall change etc)
- How artifacts differ when it comes to monolith OR a micro-service ?
=======
>>>>>>> 7fd98aeb12cd3f6bcb0b893f86c549b837ece216




### Thought process:
 
#### - What type of artifacts are required in order to run the application
 
 ``` To run this application required only application binaries artifacts```

#### - How we can define the build and release cycles using these artifacts ?

#### - What gets updates when there is a change in the application ( can be source code change due to new release, can be network change, can be firewall change etc)

``` if there is any change in the application then will need to change source code or may be network and firewall. ```

#### - How artifacts differ when it comes to monolith OR a micro-service ?

- In terms to monolithic

``` all of the code for a system is in a single codebase that is compiled together and produces a single artifact. ```

- In terms to micro-service 

``` a micro-service(non-monolithic) module design may have code split into multiple modules or libraries that can be compiled separately, stored in repositories and referenced when required. ```

![image](https://user-images.githubusercontent.com/98619865/169771941-7664c536-5256-4181-8e7d-420ea99722d6.png)










       


















 # 2. Establish a pipeline/system design to build artifacts in CI (continuous integration) system so that they are available for CD (continuous deployment) system

### Thought process:
#### - Thinking about storing builds or deployable in repositories?

 - here i am going to  use github to store all builds or source code  so , i can easily pull and push code whenever any change will happened for that following command will be use.

          git pull  git@github.com:DeepCloudmosphere/cloud_POCs.git

   and make somechnage in my local repo and push to remote repo by following command.

         git push  git@github.com:DeepCloudmosphere/cloud_POCs.git
         
#### - Prepare the repository ( What are public and private repositories)?


   - for doing the above stuff(push and pull) need to create a repositories in my github account (here i use repo name `cloud_POCs`)`
     
       ![Screenshot 2022-06-17 at 4 44 40 PM](https://user-images.githubusercontent.com/98619865/174293340-2564af77-579b-483c-8865-0aca1303c014.png)



   Note:- 

      -   Public repositories are accessible to everyone on the internet. 
 
      - Private repositories are only accessible to you, people you explicitly share access with, and, for organization repositories, certain organization members.
      
#### - How will you select the repository for the enterprise ? (think about technologies used and one unified way of creating repo for multiple application)


- i will select Github enterprise because of cool features.

      GitHub Enterprise Support
      Additional security, compliance, and deployment controls
      50,000 GitHub Actions minutes
      50GB GitHub Packages storage

- And can also use `lerna` technologies for managing repo of multiple application
    
    Lerna is a library that manages multi-repository structures inside a single repository by splitting subsets of the repository into own “sub” repositories. A repository organized in this way is called a mono-repo.

    Lerna is primarily used in bigger projects that can become challenging to maintain over time. It modularizes the code into smaller manageable repositories and abstracts shareable code that can be used across these sub repos.
    
#### - Build a CI system? ( build Jenkins or bamboo or something similar), For our exercise bamboo or Jenkins anything can be used or both to learn different ways

   - For that i use Jenkins for CI system and create jenkins file for actually implementtion of CI system in whole pipeline.
   
``` 
   
   pipeline{
    agent any 
    
    environment {
        IMAGE_TAG= "${env.BUILD_TAG}"
    }
    // pull git repo and save into workspace
    stages{
        stage('Cloning Git') {
            steps {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'ssh_login', url: 'git@github.com:DeepCloudmosphere/cloud_POCs.git']]])
            }
        }
        // Building Docker images
        stage('Building image') {
            steps {
                script {
                    sh  "docker build -t deepcloudmosphere/python-web:${IMAGE_TAG}   python-web-app/"
                }
            }
        }
        stage('Pushing to DockerHub') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
              sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
              sh 'docker push  deepcloudmosphere/python-web:${IMAGE_TAG}'

                 }
            
           }
        }
        
 ```
   
   
#### - For workshop purpose these systems need to run locally (think about running a Jenkins locally, make sure its data persists with restarts, you can run in docker or a VM)

   To run locally i prefer VM and install jenkins with dependency(java) and run as a system service that are expose port 8080 so, on `localhost:8080` it will be accessing...  
   
   Data is presistent at `/var/lib/jenkins/` directory on my Virtual Machine.
   
   ![Screenshot 2022-07-02 at 3 36 18 PM](https://user-images.githubusercontent.com/98619865/176996006-d095cb42-059f-4dd7-b3a1-c206b28d940e.png)

  
   
#### - To expose your local system accessible over internet you can use a tool like this - https://ngrok.com/ ( think about why would you expose this system over internet? does any outside system requires its presence over a URL that is accessible)

   I would like to expose my local virtual machine over internet because if any changes in souce code so i want Jenkins to monitor . it will also help our devloper to check if any build fail .. 
   FOR THAT I WILL USE NGROK TOOL 
   
   ``` Ngrok is a reverse proxy that allows traffic to be redirected from the generated url to wherever your local server is running.```
   
   To expose VM firt install ngrok tool and configure to expose on port 8080 so that will generate url to  access jenkins..
   
   ![Screenshot 2022-07-02 at 5 22 25 PM](https://user-images.githubusercontent.com/98619865/176999599-ebd1bc41-7112-4c60-ad8b-9b0653ed65ce.png)

   *Finally local system is exposed over internet and can be accessed by `https://d088-136-232-96-190.in.ngrok.io` url*
   
  
  
  
  
  
    
#### - Build your artifacts (docker images, binaries or similar things that are repeatable and keep changing with application change)





  ![Screenshot 2022-07-02 at 3 40 28 PM](https://user-images.githubusercontent.com/98619865/176999726-b760cec1-ede6-475a-891e-e816b116ebfe.png)
  ![Screenshot 2022-07-02 at 3 38 27 PM](https://user-images.githubusercontent.com/98619865/176999727-8c7c15f1-a61e-497c-b92f-6fc0faf4a8db.png)
  ![Screenshot 2022-07-02 at 3 39 15 PM](https://user-images.githubusercontent.com/98619865/176999732-4affa475-7709-48bc-8f56-3b9d8baf280d.png)

     
#### - Try to change a version of your application and build a versioned artifacts in the repository? (Feeling confident?)

   Yes,  after building CI system i have tried to change version of application on github repository and CI system  will automatically build my artifact(Dockerfile) and also pushed on DockerHub..
        
   Yes i am feeling confident .

# 3. Don't think about CD yet and read about need of a CI system and prepare notes around why do you need a CI system (maximum five bullet points)

-  Continuous Integration is the practice of continuously integrating the changes made to the project and testing them accordingly at least on a daily basis or more frequently.
-  Automating your build, test, and deploy processes can alleviate much of the headaches and problems commonly happening on projects.
-  The key goals of continuous integration are to find and address bugs quicker, improve software quality, and reduce the time it takes to validate and release new software updates.
-  At a high level, the value of continuous integration is to: Reduce risks. Reduce repetitive manual processes. Generate deployable software at any time and at any place.
- The Continuous Integration process collaborates with the Continuous Delivery workflow that makes code sharing easy and regularized. This makes the process more transparent and collaborative among team members.

# 4. Prepare a diagram of above system and keep it in the lucidchart (make sure you involve all tech components involved like Source code repo, CI system, Image or binary repo etc)

   
