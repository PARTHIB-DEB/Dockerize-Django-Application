# [![My Skills](https://simpleskill.icons.workers.dev/svg?i=docker)](https://www.docker.com/) Dockerize-Django-Application [![My Skills](https://skillicons.dev/icons?i=django)](https://www.djangoproject.com/)

Containerization is just like cooking 🧑‍🍳 - You gather all ingredients and put in the container . Before cooking , there were in distinct shape , size , color but after cooking its a whole meal 🥗. Just like that , You create different services for your application , precisely **Full Stack Application** , and then you put those services in a docker container - it may be databases , backend servers , frontend server , cache servers , queueing systems etc. Each of them has their own host & port to show them to you but within docker , they are containerized , more definitely - **multi-containerized** (because unlike cooking , if you put all stuffs in a single container , the size of it will be so big that your tiny SSDs may not handle it and forget those servers to even run quickly 😄)

But how those containers communicate with each other - if we consider cooking , ingredients got assembled in a large pot over , but here we have **Networks** 🚠.
By defeault when you are locally building the project with docker , the containers communicate and run within **default** network of type **bridge**. But that **may not work** , if you pull your docker project from docker hub and directly build and run that shit !

This tutorial cheatsheet is all about dockerizing a working django backend who throws REST APIS like spiderman's web to you and your frontend 🕸 . It doesn't tell you anything about django but mostly about the hosting process in docker .

For those who know nothing but nuts in docker , follow this tutorial - [link](https://docker-curriculum.com/)

## Commands 
```
- Build Dockerfile
- Build docker-compose.yml
- docker-compose -up -- build (docker compose build && docker compose up)
- docker login
- docker build -t <default - latest> username/repo-name
- docker push username/repo-name
- docker ps -a OR docker container ls  # all container
- docker image ls OR docker images -a  # all images
- docker network ls    # all networks
- docker network create my-network  # create a new network
- docker run -d --name farmers-app --network farmers-network \
  -e DB_HOST=farmers-db \
  -e DB_USER=your_db_user \
  -e DB_PASSWORD=your_db_password \
  -e DB_NAME=your_db_name \
  -p your_django_port:your_django_port \
  parthib23/farmers-api:latest
- docker run -d --name farmers-db --network farmers-network \
  -e POSTGRES_USER=your_db_user \
  -e POSTGRES_PASSWORD=your_db_password \
  -e POSTGRES_DB=your_db_name \
  -p 5432:5432 \
  postgres:17.4-bookworm
