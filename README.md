# SpringBootWithDocker
Deploy Spring Boot app on your local machine using Docker
# Overview

## What is Docker?

  Docker is a virtualization management software for managing containers and images.

Now what are containers and images

- ## Container 
      
       A container is a runtime environment for images.

- ## Image 
     
      An image is a software that you run within the containers.
      
      The image is built using layers. Suppose you have an existing image, you added your app and 
      built a new image and other people can build an image on top of your image.
 
 ## So what docker does actually?
 
 - Build that image.
 - Deploy those images into containers.
 - And manage those containers like starting and stoping those containers.
 
 ## Why should we use Docker in the first place?
 
 - It’s easy to build docker images and put it into containers.
 - It’s lightweight.
 - It’s cloud agnostic. That means you don’t have to worry about a particular version of Linux is available or not, 
   several cloud platforms support docker and you can just move your docker container from one cloud platform to another.
 - And it’s scalable. You can startup multiple containers with your image in a matter of seconds.  
 
 ## Install Docker
 * [Download Docker](https://www.docker.com/products/docker-engine) for your platform and install it.
 
 you can check if it's properly installed by running a command.
 
  <code> docker ps </code>
  
  <code> CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES</code>
  
It shows the existing containers running with docker.

Ok great!

Now that you are setup for using Docker. We’ll now deploy a sample spring boot app using Docker.

## ADD MAVEN PLUGIN

We have to add * [Docker Maven Plugin](https://github.com/spotify/docker-maven-plugin) in you pom.xml

Inside our <plugins> </plugins> tag we need to add this plugin.

```xml  
  <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <configuration>
        <imageName>example-app</imageName>
        <baseImage>java:8</baseImage>
        <entryPoint>["java", "-jar", "/${project.build.finalName}.jar"]</entryPoint>
        <!-- copy the service's jar file from target into the root directory of the image --> 
        <resources>
           <resource>
             <targetPath>/</targetPath>
             <directory>${project.build.directory}</directory>
             <include>${project.build.finalName}.jar</include>
           </resource>
        </resources>
      </configuration>
    </plugin>  
```
Remember, we talked about our image is built on top on a base image here. We see java:8 is our base image here. 
Since spring boot needs Java 8 to run, we can’t use older versions than Java 8.

The <resources> tag tell what to copy into the root of the file system, in this case, its build jar file of our 
application and the <entryPoint> tag is what we need to run this jar file inside the container.

# Build Docker Image
Now open your Terminal and cd to your project directory. And Run this command to build the image.
  
`mvn clean package docker:build`

As I’m using a mac, although commands are similar in both Mac and Linux

`cd Documents/springspace/AccountingApp && ./mvnw clean package docker:build`

If you get an exception like this:

`java.lang.UnsupportedClassVersionError: org/apache/maven/wrapper/MavenWrapperMain : Unsupported major.minor version 51.0`

then add Java 8 to as your system variable. For Mac, you can use this command to temporarily add Java as a system variable

`export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home`

Building new Docker image will take some time. After a successful build, you’ll see this success message on your terminal.

and you’ve successfully built your software image on top of Java 8 system image.

Wait, what?? where is Java 8 image then?

Okay, let me show you something. Scroll up to your terminal and let’s see if you can find the docker is downloading Java 8 image from their repository. 

If you want to verify if your docker image is built, you can run this command:

`docker images`

# RUN DOCKER IMAGE

Now it’s time to finally run our application on our local machine. To run this docker image we have to execute this command:

`docker run -it -p 80:8080 example-app`

Here -it stands for an interactive terminal, -p is the port, since docker is running on a host machine that has port 80 and your container is listening port 8080, we have to map that to two ports so that on host machine it li to tens port 80 and maps that port on port 8080.

If everything is okay the your app is deployed successfully.

Open a browser, and enter your ip address on address bar. You’re app is running!!




 
