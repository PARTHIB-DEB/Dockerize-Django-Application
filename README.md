# [![My Skills](https://simpleskill.icons.workers.dev/svg?i=docker)](https://www.docker.com/) Dockerize Postgres Infused Django App [![My Skills](https://skillicons.dev/icons?i=django)](https://www.djangoproject.com/)

Containerization is just like cooking 🧑‍🍳 - You gather all ingredients and put in the container . Before cooking , there were in distinct shape , size , color but after cooking its a whole meal 🥗. Just like that , You create different services for your application , precisely **Full Stack Application** , and then you put those services in a docker container - it may be databases , backend servers , frontend server , cache servers , queueing systems etc. Each of them has their own host & port to show them to you but within docker , they are containerized , more definitely - **multi-containerized** (because unlike cooking , if you put all stuffs in a single container , the size of it will be so big that your puny SSDs may not handle it and forget those servers to even run quickly 😄)

But how those containers communicate with each other - if we consider cooking , ingredients got assembled in a large pot over , but here we have **Networks** 🚠.
By defeault when you are locally building the project with docker , the containers communicate and run within **default** network of type **bridge**. But that **may not work** , if you pull your docker project from docker hub and directly build and run that shit !

This tutorial cheatsheet is all about dockerizing a working django backend who throws REST APIS like spiderman's web to you and your frontend 🕸 . **It doesn't tell you anything about django but mostly about the hosting process in docker .**

For those who know nothing but nuts in docker :suspect: , follow this tutorial - [link](https://docker-curriculum.com/)

Install Docker - [link](https://docs.docker.com/engine/install/)

Docker Hub - [link](https://hub.docker.com/)

My django + postgres project - [link](https://github.com/PARTHIB-DEB/FARMERS-API)

## Dockerfile vs docker-compose.yml (or YAML) 👦 😎

### Dockerfile and Docker Image
Dockerfile is the primary execution-point where you write docker commands. Among which some are the command to run the servers and install the dependencies and others are commands to store the project in a certain location in the docker conatiner hosted in Linux OS (want to know more about basics , go the link above). 

But how docker runs a project ? Just by creating Images encrypted by SHA algorithms . Images ? yeah , just like git does (if you have used it) , docker captures the snapshot or precisely **current version** of the project (where each files and folder has their own status) with a unique alpha-numeric code generated by SHA encryption. You pull that image , build it and run it . Whenever you run an image , a container is assigned to that image and the *unique image-code* tells which project has to be containerized .
That's how an image can be managed by multiple containers and that's why you have to manage containers , to keep your remaining space in SSD , SAFE !!

So for any static site (site that don't need databases or other services to interact with) , if you want to deploy the project into **Docker Hub** (A place like Github , where you push and pull your docker images) ,  you just need the dockerfile and you are good to go .

But what happens when the server has a database connection ? then you need to create more than one services , for that you need - docker.compose.yml

The General Structure of a dockerfile for a django-project is -
```bash
    #  This image is taken from docker-hub
    FROM python:version-nickname

    # To set some ENV variables directly in dockerfile
    # Tells to show standard errors in logfile 
    ENV PYTHONUNBUFFERED=1
    
    # Tells python not to write .pyc files
    ENV PYTHONDONTWRITEBYTECODE=1

    # The place inside the container , where the project will be saved (you can change it)
    WORKDIR /app
    
    # Copy all files and folders from local directory to /app directory of the container
    COPY . /app/
    
    # Copy the requirements.txt from local directory to /app directory of the container
    COPY ./requirements.txt .
    
    # Install all dependencies
    RUN pip install -r requirements.txt

    # A startup scripti with all startup commands is good to have , CMD allows you to run such commands every time"
    CMD ["sh", "./start.sh"]
```

### docker-compose.yml
This file has an .yml extension which means , it follows YAML syntaxes . Here , like the base image in dockerfile , you create some independent services (for this context , the database is the one) and some other services depends on the independent ones (the backend server is the one dependent service here) , and that's how you made up things to run. This is an additional file, made for dynamic apps where each of the services are stored in seperate containers. 

The General Structure of a dockerfile for a django-project is -

```bash
     services:
       web:
         build:
           context: .     # the current folder is the context
         ports:
           - "8000:8000"     # default-port:port-inside-container
         command: ["sh", "./start.sh"]     # For locally building the project , you can list the command here
         volumes:
           - .:/app          # The place where all project folders and files will be stored
         env_file:
           - .env          # if there is any environment file
         depends_on:
           - postgres_db     # Depends upon another service , which is a postgresql service here - postgres_db
       
       postgres_db:
         image: postgres:version-nickname     #  This image is taken from docker-hub
         volumes:
           - postgres_data:/var/lib/postgresql/data          # The place where all data and postgresql will be stored
         environment:                                   # To fetch database details from .env file
           - POSTGRES_DB=${DB_NAME}
           - POSTGRES_PASSWORD=${DB_PASSWORD}
           - POSTGRES_USER=${DB_USER}
     
     volumes:     # The place where all data and postgresql will be stored
       postgres_data:
```
## Docker Hub
After building your project by docker in your local IDE , now its time to push it into some hub - **Docker Hub** . This is the place , where you have to create an account to push repositories . Then you login into docker-hub via your terminal in local IDE , build your project and push it in docker-hub. That's it.

## Important Note 
- In local IDE , when you build your project using **docker-compose.yml** , both services (django and postgres here) get created .
- But when you push your project after building with docker (just **docker build**) , you are just pushing your , YOUR PROJECT (not the postgres or other base images)
- So when you pull your project's image from docker-hub , you need to pull the base-image (postgresql here) seperately
- Then you need to create a new network and all those images should be connected to that same network , otherwise you may face errors like ```OperationalError``` from         django.
- After that you can run all the images with necessary aliases (like database details)
  
  **Specific to Django**

- When you are creating services of server and database . The name of the database-host SHOULE NOT BE localhost or 127.0.0.1 , rather it must be the name of the service
  that you have written in the **docker-compose.yml** . Otherwise , be ready to face errors from django like ```ConnectionError```.
  
  But why this decision ? Because in your local IDE , the postgres instance (for this case) may run in 5432 , but then it is containerized and put inside a docker network ,
  each container exposes a public IP , after hiding this local ones , for this case , your database instance will also have a public docker decided IP to which your web
  server has to connect with its docker decided IP - the database-container name in the database-host helps to make this connection


## Commands

All the commands has to be written in your own local terminal , the context specifies about the status of the images  (either you have made it locally or you have pulled it from docker hub and running it locally)

### Basic Commands

- See all docker images
  
  ```bash
      docker image ls
  ```

  **OR**
  
  ```bash
      docker images -a  # all images
  ```

- See all docker networks

  ```bash
      docker network ls    # all networks
  ```
  
- List and view all containers of docker
  
    ```bash
        docker ps -a
    ```
    
    **OR**
    
    ```bash
        docker container ls  # all container
    ```

### Local Context
- Build the project
  
  ```bash
      docker compose build
  ```
- Run the build
  
  ```bash
      docker compose up
  ```
- Or you can do both in one
  
  ```bash
      docker-compose up --build
  ```
  
### Global Context
- Login to Docker-Hub
  for first-time , maybe you need to put a one-time code in the docker-hub website to authenticate

  ```bash
      docker login
  ```

- Build the project
  
  ```bash
      docker build -t <default - latest> username/repo-name .
  ```
- Push the build to docker-hub
  
  ```bash
      docker push username/repo-name:tag
  ```
Hereafter I suggest you to stop all containers and their images to start over and see the magic 🌠.
For that follow my **general docker commands** section ⬆️

- Create your own network of type **bridge** in your docker installed machine
  
  ```bash
      docker network create my-network    # change my-network to your favorite name
  ```
- Pull the image of your project from docker
  
  ```bash
      docker pull username/repo-name:tag
  ```
- Pull the base image / independent dependencies' images / postgresl's image (for this cheatsheet)
  
  ```bash
      docker pull postgreseql:version-tagname
  ```
- Connect all images to your network
  
  ```bash
      docker network connect NETWORK-NAME/ID CONTAINER-NAME/ID
  ```
- Run all images
  Why didn't you built now ? becaue you already sent your built-files to docker-hub , so now you are just pulling and running it . Same happens for base images
  
    ```bash
        # Run the BASE-IMAGE atfirst
                docker run -d --name database-container-name --network my-network \
          -e POSTGRES_USER=your_db_user \
          -e POSTGRES_PASSWORD=your_db_password \
          -e POSTGRES_DB=your_db_name \
          -p 5432:5432 \

        # Run your project's image now
          docker run -d --name your-project-container-name --network my-network \
      -e DB_HOST=farmers-db \
      -e DB_USER=your_db_user \
      -e DB_PASSWORD=your_db_password \
      -e DB_NAME=your_db_name \
      -p your_django_port:your_django_port \
      username/repo-name:tag
  ```
# Conclusion
That' it . You have successfully made a django+postgres project , containerized it and ran it using docker . Now I make a small request to everyone of you to star ⭐ this repository , if you have reached upto here by executing all steps in a perfect choronology. See y'all later 👋
