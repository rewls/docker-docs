# Use Docker Compose

- Docker Compose is a tool that helps you define and share multi-container applications.

- With Compose, you can create a YAML file to define the services and with a single command, you can spin everything up or tear it all down.

## Create the Compose file

- In the `getting-started-app` directory, create a file named `compose.yaml`.

## Define the app service

1. Open `compose.yaml` and start by defining the name and image of the first service you want to run as part of your application.

    - The name will automatically become a network alias, which will be useful when defining your MySQL service.

    ```yaml
    services:
        app:
            image: node:18-alpine
    ```

2. Typically, you will see `command` close to the `image` definition, although there is no requirement on ordering.

    ```yaml
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
    ```

3. Now migrate the `-p 127.0.0.1:3000:3000` part of the command by defining the `ports` for the service.

    ```yaml
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 127.0.0.1:3000:3000
    ```

4. Migrate both the working directory (`-w /app`) and the volume mapping (`-v "$(pwd):/app"`) by using the `working_dir` and `volumes` definitions.

    - One advantage of Docker Compose volume definitions is you can use relative paths from the current directory.

    ```yaml
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 127.0.0.1:3000:3000
        working_dir: /app
        volumes:
          - ./:/app
    ```

5. You need to migrate the environment variable definitions using the environment key.

    ```yaml
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 127.0.0.1:3000:3000
        working_dir: /app
        volumes:
          - ./:/app
        environment:
          MYSQL_HOST: mysql
          MYSQL_USER: root
          MYSQL_PASSWORD: secret
          MYSQL_DB: todos
    ```

### Define the MySQL service

1. Define the new service and name it mysql so it automatically gets the network alias.

    - Also specify the image to use as well.


    ```yaml
    services:
      app:
        # The app service definition
      mysql:
        image: mysql:8.0
    ```

2. You need to define the volume in the top-level `volumes`: section and then specify the mountpoint in the service config.

    - By simply providing only the volume name, the default options are used.

    ```yaml
    services:
      app:
        # The app service definition
      mysql:
        image: mysql:8.0
        volumes:
          - todo-mysql-data:/var/lib/mysql

    volumes:
      todo-mysql-data:
    ```

3. You need to specify the environment variables.

    ```yaml
    services:
      app:
        # The app service definition
      mysql:
        image: mysql:8.0
        volumes:
          - todo-mysql-data:/var/lib/mysql
        environment:
          MYSQL_ROOT_PASSWORD: secret
          MYSQL_DATABASE: todos

    volumes:
      todo-mysql-data:
    ```

## Run the application stack

1. Start up the application stack using the `docker compose up` command.

    - Add the `-d` flag to run everything in the background.

    - You'll notice that Docker Compose created the volume as well as a network.

    - By default, Docker Compose automatically creates a network specifically for the application stack.

2. Look at the logs using the docker compose logs -f command.

    - The -f flag follows the log, so will give you live output as it's generated.

    - If you want to view the logs for a specific service, you can add the service name to the end of the logs command.

3. At this point, you should be able to open your app in your browser on http://localhost:3000 and see it running.

## Tear it all down

- When you're ready to tear it all down, simply run docker compose down.

- The containers will stop and the network will be removed.

> ##### Warning
>
> - By default, named volumes in your compose file are not removed when you run docker compose down. If you want to remove the volumes, you need to add the --volumes flag.
