# Dockerized Battleships Game
In this task, we should dockerize the game and create a container for an Nginx reverse proxy. We should then write a Docker compose file for this project.
## web application Dockerfile

This Dockerfile will create a Docker image for the web application. It uses the official `Node.js` image as the base image, and then creates an `app` directory, copies the `package.json` and `package-lock.json` files into the app directory, installs the app dependencies, copies the app source code into the app directory, and defines the command to run the app.

## reverse proxy Dockerfile

This Dockerfile will create a Docker image for the reverse proxy. It uses the official `Nginx` image as the base image, and then copies the `default.conf` configuration file into the `/etc/nginx/conf.d` directory.

> [!IMPORTANT]
> If certain commands or instructions in your Dockerfile frequently change, it's best to position them near the end. This helps to avoid triggering unnecessary rebuilds of layers that haven't been affected by these changes.

## Docker Compose file

This Docker Compose file defines how to build and run the two Docker images that I created. It defines two services: `nodeserver` and `nginx`.

The nodeserver service builds the Docker image for the web application and then runs it. The nginx service builds the Docker image for the reverse proxy and then runs it.

> [!NOTE]
> The `depends_on` property for the nginx service tells Docker Compose that the nodeserver service must be running before the nginx service can start. This is because the nginx service needs the nodeserver service to be running.

The ports property for the nginx service tells Docker Compose to expose port 80 on the host machine to port 80 on the container. This means that requests to port 80 on the host machine will be forwarded to port 80 on the container.

The volumes property for the nginx service tells Docker Compose to mount the logs volume to the `/var/log/nginx` directory in the container. This means that the logs for the Nginx server will be stored in the logs volume.

## .dockerignore file

This file tells Docker which files and directories to ignore when building or running the Docker images. 

> [!NOTE]
> For security, cache invalidation, faster build process, and reduced image size, it is better to create this file and exclude unnecessary files.

## nginx config file

The Nginx config file tells Nginx how to proxy requests to the web application. It does this by defining a server block with a listen port of 80 and a server name of bootcamp.neshan.org. The server block also defines a location block that handles all requests. The location block proxies requests to the nodeserver service on port 3000.

you can run these containers with 

    docker-compose up --build -d