# Dockerize Python Application.

## What is Docker?

**Docker** is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.


## Difference between Docker and a VM.

In a way, Docker is a bit like a virtual machine. But unlike a virtual machine, rather than creating a whole virtual operating system, Docker allows applications to use the same Linux kernel as the system that they're running on and only requires applications be shipped with things not already running on the host computer. This gives a significant performance boost and reduces the size of the application.

## Docker is used by ?
Docker is a tool that is designed to benefit both developers and system administrators, making it a part of many **DevOps** (developers + operations) toolchains. For developers, it means that they can focus on writing code without worrying about the system that it will ultimately be running on. It also allows them to get a head start by using one of thousands of programs already designed to run in a Docker container as a part of their application. For operations staff, Docker gives flexibility and potentially reduces the number of systems needed because of its small footprint and lower overhead.

## Some basic terminologies

### Docker image
-   It is a file, comprised of multiple layers, used to execute code in a Docker container.
-   They are a set of instructions used to create docker containers.

### Docker Container

-   It is a runtime instance of an image.
-   Allows developers to package applications with all parts needed such as libraries and other dependencies.

### Dockerfile

-   It is a text document that contains necessary commands which on execution helps assemble a Docker Image.
-   Docker image is created using a Docker file.
### Docker Engine
-   The software that hosts the containers is named Docker Engine.
-   Docker Engine is a client-server based application
-   The docker engine has **3 main** components:
    -   **Server**: It is responsible for creating and managing Docker images, containers, networks, and volumes on the Docker. It is referred to as a daemon process.
    -   **REST API**: It specifies how the applications can interact with the Server and instructs it what to do.
    -   **Client**: The Client is a docker command-line interface (CLI), that allows us to interact with Docker using the docker commands.


### Dockerhub


-   Docker Hub is the official online repository where you can find other Docker Images that are available for use.
-   It makes it easy to find, manage, and share container images with others.

## Setup Docker for different operating systems.
#### Linux
``` $ sudo apt install docker-ce```
``` $ sudo systemctl status docker```
#### Macos
```$ brew cask install docker```
#### Windows
[Watch this short video to install docker in windows](https://www.youtube.com/watch?v=_9AWYlt86B8)

### Python code for this tutorial.
```
import requests
covid=requests.get("https://api.covid19api.com/summary")
data=json.loads(covid.text)
print("worldwide")
latest=data['Global']
print(f"New Confirmed Cases:{latest['NewConfirmed']}")
print("New Deaths:"+str(latest['NewDeaths']))
print("New Recovered Cases:"+str(latest['NewRecovered']))
print("Total Confirmed Cases:"+str(latest['TotalConfirmed']))
print("Total Deaths:"+str(latest['TotalDeaths']))
print("Total Recovered Cases:"+str(latest['TotalRecovered']))
location=''
while location!='Q':
    print("Enter location:(press q to quit)")
    location=input()
    location=location[0:1].upper()+location[1:]
    l=data['Countries']
    country=[z for z in l if z['Country']==location]
    if len(country)==0:
        if location!='Q':
            print("invalid country!")
        continue
    else:
        country=country[0]
        print("New Confirmed Cases:"+str(country['NewConfirmed']))
        print("New Deaths:"+str(country['NewDeaths']))
        print("New Recovered Cases:"+str(country['NewRecovered']))
        print("Total Confirmed Cases:"+str(country['TotalConfirmed']))
        print("Total Deaths:"+str(country['TotalDeaths']))
        print("Total Recovered Cases:"+str(country['TotalRecovered'])
```
### Dockerfile for this tutorial.
```
FROM python:3.8 //base image

ADD covid.py . //adding file into the container or current dir.

RUN pip3 install requests //run in container to install packages

CMD ["python3", "covid.py"] // to initialize the code in container
```
####Building a container.
```docker build -t covidapp .```
Output:
```
┌──(ssoin01📛test)-[~/test]
└─$ sudo docker build -t covidapp .
[sudo] password for ssoin01: 
Sending build context to Docker daemon  4.096kB
Step 1/5 : FROM python:3
 ---> 5b3b4504ff1f
Step 2/5 : WORKDIR /home/ssoin01/test/
 ---> Using cache
 ---> a2644e5b51ea
Step 3/5 : ADD covid.py .
 ---> Using cache
 ---> 1b9b63e11b0b
Step 4/5 : RUN pip3 install requests
 ---> Using cache
 ---> 49a143792446
Step 5/5 : CMD [ "python3","covid.py" ]
 ---> Using cache
 ---> 460380159a9d
Successfully built 460380159a9d
Successfully tagged covidapp:latest
```
This command builds the container in layers or sequence of commands passed in the Dockerfile. "-t" flag is for giving a tag or name to the container.
####Running that container.
```docker run -i covidapp```
Output:
```
┌──(ssoin01📛test)-[~/test]
└─$ sudo docker run -i covidapp
worldwide
New Confirmed Cases:206113
New Deaths:4859
New Recovered Cases:287210
Total Confirmed Cases:173053278
Total Deaths:3726347
Total Recovered Cases:111118329
Enter location:(press q to quit)
Australia
New Confirmed Cases:18
New Deaths:0
New Recovered Cases:4
Total Confirmed Cases:30191
Total Deaths:910
Total Recovered Cases:23607
Enter location:(press q to quit)
q
```
This command runs the container."-i" flag stands for the interactive mode with the commandline for io.
#### Some other Docker commands.
######List all the running containers.
```docker ps```
```
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS               NAMES
8bf9021e9b9c        covidapp            "python3 covid.py"   25 seconds ago      Up 21 seconds                           elastic_lewin
```
######List all the containers on the machine.
```docker images```
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
covidapp1           latest              460380159a9d        7 hours ago         896MB
covidapp            latest              460380159a9d        7 hours ago         896MB
python              3                   5b3b4504ff1f        13 days ago         886MB
python-covid        latest              818829069116        6 weeks ago         892MB
python              3.8                 02583ab5c95e        8 weeks ago         883MB
ubuntu              latest              26b77e58432b        2 months ago        72.9MB

```
######Remove a container.
```docker rm ID_or_Name ID_or_Name```
##Conclusion

This guide covers some of the common commands used to remove images, containers, and volumes with Docker as well as a method to containerize your python application.
