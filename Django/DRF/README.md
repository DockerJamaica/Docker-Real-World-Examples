# Quickstart: Compose Django Real World App Example

You can use Docker Compose to easily run a Django app in an isolated environment built with Docker containers.
This quick-start guide demonstrates how to use Compose to set up and run Django. Before starting,
make sure you have Docker Compose installed.

## Clone the Docker Real World Examples

```bash
git clone https://github.com/DockerJamaica/Docker-Real-World-Examples.git
```

### Dockerizing the [Django Realworld Example App](https://github.com/gothinkster/django-realworld-example-app)

Navigate to the Django DRF example folder and open display contents of the Dockerfile

```bash
cd Docker-Real-World-Examples/Django/DRF/
cat Dockerfile
```

Contents of the Dockerfile:

```
FROM python:3.5.7
ENV PYTHONUNBUFFERED 1
RUN git clone https://github.com/gothinkster/django-realworld-example-app.git /django-app
WORKDIR /django-app
RUN pip install -r /django-app/requirements.txt
COPY . /django-app
```

This Dockerfile starts with a Python 3.5.7 parent image.
The parent image is modified by cloning the Django Realworld Example App.
The parent image is further modified by installing the Python requirements for the Django Realworld Example App.


### Using other docker images as services for the Django application.

The docker-compose.yml file describes the services that make your app.
In this example those services are a web server and database. The compose file also describes which Docker images these services use,
how they link together, any volumes they might need mounted inside the containers.
Finally, the docker-compose.yml file describes which ports these services expose.
See the docker-compose.yml reference for more information on how this file works.

```
version: '3'

services:
  db:
    image: postgres
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/django-app
    ports:
      - "8000:8000"
    depends_on:
      - db
```

This file defines two services: The db service and the web service that will be used by the Django app.

### Running dockerized Django application

```
sudo docker-compose run web django-admin startproject composeexample .
```

This instructs Compose to run django-admin startproject composeexample in a container, using the web service’s image and configuration. Because the web image doesn’t exist yet, Compose builds it from the current directory, as specified by the build: . line in docker-compose.yml.

Once the web service image is built, Compose runs it and executes the django-admin startproject command in the container. This command instructs Django to create a set of files and directories representing a Django project.


If you get a build error, you can run the following code to find out what is happening with the app.
```bash
docker-compose up --build
```

After the docker-compose command completes, list the contents of your project.
