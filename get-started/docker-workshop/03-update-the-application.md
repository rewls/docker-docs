# Update the application

## Update the source code

1. In the `src/static/js/app.js` file, update line 56 to use th new empty text, "You have no todo items yet! Add one above!".

2. Build your updated version of the image, using the `docker build` command.

    ```shell
    docker build -t getting-started .
    ```

3. Start a new container using the updated code.

    ```shell
    docker run -dp 127.0.0.1:3000:3000 getting-started
    ```

- The error occurred because you aren't able to start the new container while your old container is still running.

- The reason is that the old container is already using the host's port 3000 and only one process on the machine can listen to a specific port.

## Remove the old container

### Remove a container using the CLI

1. Get the ID of the container by using the `docker ps` command.

    ```shell
    docker ps
    ```

2. Use the `docker stop` command to stop the container.

    ```shell
    docker stop <the-container-id>
    ```

3. Once the container has stopped, you can remove it by using the docker rm command.

    ```shell
    docker rm <the-container-id>
    ```

> ##### Note
>
> - You can stop and remove a container in a single command by adding the `force` flag to the `docker rm` command.
>
>   ```shell
>   docker rm -f <the-container-id>
>   ```

### Start the updated app container

- Start your updated app using the docker run command.

    ```shell
    docker run -dp 127.0.0.1:3000:3000 getting-started
    ```
