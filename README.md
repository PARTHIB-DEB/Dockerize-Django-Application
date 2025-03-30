# Dockerize-Django-Application

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
