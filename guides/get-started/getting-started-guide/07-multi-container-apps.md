# Multi container apps

- Now You will add MySQL to the application stack.

- In general, each container should do one thing and do it well.

- The following are a few reasons to run the container separately:

    - There's good chance you'd have to scale APIs and front-ends differently than databases.

    - Separate containers let you version and update versions in isolation

    - While you may use a container for the database locally, you may want to use a managed for the database in production.

    - Running multiple processes will require a process manager (the container only starts one process), which adds complexity to container startup/shutdown.

## Container networking

- If you place the two containers on the same network, they can talk to each other.

## Start MySQL

- There are two ways to put a container on a network:

    - Assign the network when starting the container.

    - Connect an already running container to a network.

- In the following steps, you'll create the network first and then attach the MySQL container at startup

1. Create the network.

    ```shell
    $ docker network create todo-app
    ```

2. Start a MySQL container and attach it to the network.

    - You're also going to define a few environment variables that the database will use to initialize the database.

    - To learn more about the MySQL environment variables, see the Environment Variables" section in the MySQL Docker Hub listing.

    ```shell
    docker run -d \
        --network todo-app --network-alias mysql \
        -v todo-mysql-data:/var/lib/mysql \
        -e MYSQL_ROOT_PASSWORD=secret \
        -e MYSQL_DATABASE=todos \
        mysql:8.0
    ```

    > ##### Tip
    >
    > - You'll notice a volum named `todo-mysql-data` in the above command that is mounted at `/var/lib/mysql`, which is where MySQL stores its data.
    >
    > - You never ran a `docker volume create` command.
    >
    > - Docker recognizes you want to use a named volume and creates one automatically for you.

3. To confirm you have the database up and running, connect to the database and verify that it connects.

    ```shell
    docker exec -it <mysql-container-id> mysql -u root -p
    ```

## Connect to MySQL

- If you run another container on the same network, how do you find the container?

- To answer the questions above and better understand container networking, you're going to make use of the nicolaka/netshoot container, which ships with a lot of tools that are useful for troubleshooting or debugging networking issues.

1. Start a new container using the nicolaka/netshoot image.

    ```shell
    docker run -it --network todo-app nicolaka/netshoot
    ```

2. Inside the container, you're going to use the `dig` command, which is a useful DNS tool.

    - You're going to look up the IP address for the hostname `mysql`.

    ```shell
    $ dig mysql
    ```

    - While `mysql` isn't normally a valid hostname, Docker was able to resolve it to the IP address of the container that had that network alias.

## Run your app with MySQL

- The todo app supports the setting of a few environment variables to specify MySQL connection settings.

- `MYSQL_HOST` - the hostname for the running MySQL server

- `MYSQL_USER` - the username to use for the connection

- `MYSQL_PASSWORD` - the password to use for the connection

- `MYSQL_DB` - the database to use once connected

> ##### Note
>
> - While using env vars to set connection settings is generally accepted for development, it's highly discouraged when running applications in production.
>
> - Diogo Monica, a former lead of security at Docker wrote a fantastic [blog post](https://blog.diogomonica.com//2017/03/27/why-you-shouldnt-use-env-variables-for-secret-data/) explaining why.

1. Specify each of the previous environment variables, as well as connect the container to your app network.

    ```shell
    $ docker run -dp 127.0.0.1:3000:3000 \
      -w /app -v "$(pwd):/app" \
      --network todo-app \
      -e MYSQL_HOST=mysql \
      -e MYSQL_USER=root \
      -e MYSQL_PASSWORD=secret \
      -e MYSQL_DB=todos \
      node:18-alpine \
      sh -c "yarn install && yarn run dev"
    ```

2. If you look at the logs for the container (`docker logs -f <container-id>`), you should see a message similar to the following, which indicates it's using the mysql database.

    ```shell
    $ nodemon src/index.js
    [nodemon] 2.0.20
    [nodemon] to restart at any time, enter `rs`
    [nodemon] watching dir(s): *.*
    [nodemon] starting `node src/index.js`
    Connected to mysql db at host mysql
    Listening on port 3000
    ```

3. Open the app in your browser and add a few items to your todo list.

4. Connect to the mysql database and prove that the items to the database.

    ```shell
    docker exec -it <mysql-container-id> mysql -p todos
    ```
