# Containerize an application

- For the rest of this guide, you'll be working with a simple todo list manager that runs on Node.js.

- This guide doesn't require any prior experience with JavaScript.

## Prerequisites

- You have installed the latest version of Docker Desktop.

- You have installed a Git client

- You have an IDE or a text editor to edit files.

## Get the app

```shell
$ git clone https://github.com/docker/getting-started-app.git
```

## Build the app's image

- A Dockerfile is simply a text-based file with no file extension that contains a script of instructions.

1. In the `getting-started-app` directory, create a file name `Dockerfile`.

    ```shell
    $ cd /path/to/getting-started-app
    $ touch Dockerfile
    ```

2.Using a text editor, add the following contents to the Dockerfile.

    ```Dockerfile
    # syntax=docker/dockerfile:1

    FROM node:18-alpine
    WORKDIR /app
    COPY . .
    RUN yarn  install --production
    CMD ["node", "src/index.js"]
    EXPOSE 3000
    ```

3. Build the image using the following commands:

    ```shell
    $ cd /path/to/getting-started-app
    $ docker build -t getting-started .
    ```

    - The `docker build` command uses the Dockerfileto build a new image.

    - The `CMD` directive specifies the default command to run when starting a container from this image.

    - The `-t` flag tags your image.

## Start an app container

1. Run your container using the `docker run` command and specify the name of the image you just created:

    ```shell
    $ docker run -dp 127.0.0.1:3000:3000 getting-started
    ```

    - The `-d` flag (short for `--detach`) runs the container in the background.

    - You can verify that a container is running by running `docker ps` in the terminal.

    - The `-p` flag (short for `--publish`) creates a port mapping between the host and the container.

    - The `-p` flag takes a string value in the format of `HOST:CONTAINER`, where `HOST` is the address on the host, and `CONTAINER` is the port on the container.

    - Without the port mapping, you wouldn't be able to access the application from the host.

2. Open your web browser to http://localhost:3000.
