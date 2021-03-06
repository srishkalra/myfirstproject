Overview:
This Web Application has been built using Python based FLask Web framework.

Purpose: This Application is a basic Webserver which contains a Home page  and info page about an application.
         Further enhancements can be done to this application by creating URL's as per the requirement and to 
         achieve that app.py needs to be updated accordingly.

Version: Version of the APP has been set as v1.0 as initial and can be changed either in Docker file or while creating a Container and passing it as Env variable in          runtime.

DEBUG : Debug level is set as false by default and can be made true by passing as ENV variable while creating Docker Container

Logging: Logs of the applicaion can be found in "myapp.log" file which will be in the same path as your application,and can be found as you login to your APP container.This logs can be used for your troubleshooting and logging events for your application.Initial Log level has been set to INFO level and same information can be found on the webpage : http://localhost:5000/info

OS: This application uses UBUNTU based base image and thus will support ubuntu commands which will update it to latest as a part of Docker image build.

PORT Number:5000  (set as default but changeable either in DOCKER file or mentioning at runtime during the container creation.)

WEB URL: http://localhost:5000/
         http://localhost:5000/info

Where it will work with Host Ip as well and can be replaced with localhost in above link.

ex : http://<hostip>:5000/

Python FIle name : app.py and can be found in GIT URL.

DOCKER FILE:Can be found in git hub mentioned below.

Git URL :https://github.com/srishkalra/myfirstproject.git

Risk/Decisions: Git url need to be updated in Docker file  to user git  URL if path of "app.py"(Flask APP file).

1) Download the Docker file:
From Linux Terminal:
Create a new directory"newproject" as below"
#mkdir newproject
#cd newproject
Use the below command to clone from GIT repository which contains Dockerfile, related files:

git clone https://github.com/srishkalra/myfirstproject.git

After this you should be able to see the Dockerfile as below,in your working directory:
[root#myfirstproject]# ls
app.py  Dockerfile  README.md

For Windows(Docker Desktop): 
You can download the Docker file from browser using the below link:

https://github.com/srishkalra/myfirstproject.git
Copy the Docker file in your 

2) Building a Docker Image using Dockerfile: 

Downloaded "Dockerfile" from the Step1 can be used to build Docker image:

For Linux based Terminal, use below commands to Build a docker image:
Command:
docker build -t <tagname> .

Example:
docker build -t myappv1 . 

Note: myapp1 will be your image name in this example.

Below are the sample logs which the above command will end up with for a succesfully built image.

Step 12/12 : CMD ["python", "app.py"]
 ---> Running in 517b5c0b8d62
Removing intermediate container 517b5c0b8d62
 ---> 671d28fe949b
Successfully built 671d28fe949b
Successfully tagged myappv1:latest

For Windows, 



3) Viewing the Built Docker Images:

Command:docker images

Example:

docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
myappv1             latest              671d28fe949b        5 minutes ago       228MB

Note: Now to use this image to create a container, use image name as "myappv1:latest" as explained in next step.

4) Containerizing your Application from newly created Docker image:

Command: docker run -d --name=<name of your container> -p <host_port>:<App(container)_port> <Image_name>

-d option will run container in de-attach mode , and -p is used to mention port number.

Note: choose a port which is  available in your host machine. In below example, 5000 has been given default in application as ENV variable in DOcker file and can be changed, explained in further instructions.As this application is built on Flask framework which uses 5000 by default.

Example:
docker run -d --name=appv -p 5000:5000 myappv1:latest
docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
1144b339aeec        myappv1:latest      "python app.py"     7 seconds ago       Up 5 seconds        0.0.0.0:5000->5000/tcp   appv

Note: If you want to change the Port number other than 5000(Application port number) which is set as default, can be done using below command, by passing ENV Variable using "-e" option as show below which creating a container same applies for version as well,
#docker run -d --name=<container name> -e PORT=<any available port number> -e VERSION=<app version> <docker image name>

5) Check if Application is running successfully:
  Command: docker logs <appname>
  Example: docker logs appv

  O/p : It will show below logs.


5) Login to your Application now.
    
 Command: docker exec -it <appname> /bin/bash
Example:
#docker exec -it appv /bin/bash

O/p : You should be inside your container which runs your application.

6) Application Launching:

Run the below commands inside the container :

Test1:
Curl command: curl <ip:portnumber>/

#curl localhost:5000
This should give the Home page of this WEb Application which says " Hello WOrld" as below:

root@9bb260d373ab:/myfirstproject# curl localhost:5000
Hello, World!:root@9bb260d373ab:/myfirstproject#

Same you should be able to see on your browser.

Test2:

Curl command:
curl <ip:portnumber>/info

Example:
#curl localhost:5000/info
This should show the information about the application which is in JSON format in this case as below:

root@9bb260d373ab:/myfirstproject# curl localhost:5000/info
[
  {
    "service": [
      {
        "git sha": "d7a9795c8e2346eb791540f797d6272916a3bce9",
        "service": "app",
        "version": "v1.0"
      },
      {
        "loglevel": "INFO",
        "port": "5000"
      }
    ]
  },
  200
]


Same you should be able to see on your browser.
Note: if you are using checking the above command,on Host, Kindly use Host port Number in http://localhost:<host port>/info

root@9bb260d373ab:/myfirstproject#
Note: Same above command you should be able to run on Host with host port number.

7) Checking the Application logging:

Application logs can be found inside the container , the momemt you login , the same path where you get the prompt.
As it is ubuntu based image, linux based command will work.

Command : ls

Example:
root@9bb260d373ab:/myfirstproject# ls
Dockerfile  README.md  app.py  myapp.log

To see contents of myapp.log, use below command:

root@9bb260d373ab:/myfirstproject# cat myapp.log
2020-08-17 10:37:10,475 INFO werkzeug MainThread :  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
2020-08-17 10:37:10,476 INFO werkzeug MainThread :  * Restarting with stat
2020-08-17 10:37:10,710 WARNING werkzeug MainThread :  * Debugger is active!
2020-08-17 10:37:10,711 INFO werkzeug MainThread :  * Debugger PIN: 196-235-887
2020-08-17 10:37:22,046 INFO app Thread-2 : Displaying the info logs
2020-08-17 10:37:22,047 INFO werkzeug Thread-2 : 172.17.0.1 - - [17/Aug/2020 10:37:22] "GET /info HTTP/1.1" 200 -
2020-08-17 10:38:00,674 INFO app Thread-3 : Displaying the info logs
2020-08-17 10:38:00,675 INFO werkzeug Thread-3 : 127.0.0.1 - - [17/Aug/2020 10:38:00] "GET /info HTTP/1.1" 200 -
