## **Managing MySQL with Docker Compose**

- If you want to simplify container management, you can create a `docker-compose.yml` file for MySQL. Here’s an example:
    
    ```yaml
    services:
      mysql:
        image: mysql:lts
        environment:
          MYSQL_ROOT_PASSWORD: secret
        volumes:
          - mysql_data:/var/lib/mysql
        ports:
          - "3306:3306"
    volumes:
      mysql_data:
    
    ```
    
- To run the container with Docker Compose, use:
    
    ```bash
    docker-compose up -d
    ```
    

By following these steps, you will have a running MySQL instance inside a Docker container on WSL with Docker Desktop.

---

## To access the MySQL terminal when using Docker Compose

### 1. **Run MySQL Container Using Docker Compose**

Assuming you have already set up your `docker-compose.yml` file as described earlier, you can start the MySQL container by running:

```bash
docker-compose up -d
```

This will start the MySQL container in the background.

### 2. **Check Running Containers**

You can verify that the MySQL container is running by using the following command:

```bash
docker-compose ps
```

This should show a list of running containers, including your MySQL service.

### 3. **Access MySQL Terminal (MySQL CLI)**

To access the MySQL terminal inside the container, you can use the `docker exec` command. Here's the command to access the MySQL shell:

```bash
docker-compose exec mysql mysql -u root -p
```

Explanation:

- `docker-compose exec`: This runs a command in a running service defined in your `docker-compose.yml` file.
- `mysql`: This is the name of the MySQL service in your `docker-compose.yml` file. If you used a different name for the service, replace `mysql` with your service name.
- `mysql -u root -p`: This command runs the MySQL client inside the container and connects as the root user (`u root`), prompting you to enter the password (`p`).

After running the command, you’ll be prompted to enter the MySQL root password that you set in the `docker-compose.yml` file (e.g., `secret` ). Once you provide the password, you'll enter the MySQL command-line interface (CLI), where you can run MySQL queries.

### **Persisting Data with Volumes (Optional)**

- If you want to persist your MySQL data (so it’s not lost when the container is stopped or removed), create a volume. Here’s how:This creates a named volume (`mysql_data`) that will store your MySQL data.
    
    ```bash
    docker run --name mysql-lts -e MYSQL_ROOT_PASSWORD=secret -v mysql_data:/var/lib/mysql -d mysql:lts
    ```
    

This creates a named volume (`mysql_data`) that will store your MySQL data.

### Example `docker-compose.yml`

Here’s an example of how your `docker-compose.yml` file might look:

```yaml
services:
  mysql:
    image: mysql:lts
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      - mysql_data:/var/lib/mysql
    ports:
      - "3306:3306"
volumes:
  mysql_data:

```

### 4. **Exit the MySQL Terminal**

When you're done with the MySQL shell, you can exit by typing:

```sql
exit;
```

This will return you to your regular terminal.

### Summary:

- Use `docker-compose up -d` to start the MySQL container.
- Access the MySQL terminal using `docker-compose exec mysql mysql -u root -p`.
- Provide the MySQL root password to access the shell.
- Use `exit` to leave the MySQL shell.

---

## To connect your MySQL database running in Docker to **TablePlus**

### 1. **Ensure the MySQL Container is Running**

First, make sure that your MySQL container is up and running. You can check this by running:

```bash
docker-compose ps
```

This should list the MySQL container. If it's not running, start it with:

```bash
docker-compose up -d
```

### 2. **Find the Host IP and Port**

In your `docker-compose.yml` file, you’ve probably mapped port `3306` of the MySQL container to your host machine. Double-check the `ports` section of the file:

```yaml
ports:
  - "3306:3306"
```

This means that MySQL inside the container is exposed to your host system on port `3306`. You can connect to it using `localhost` or `127.0.0.1`.

### 3. **Open TablePlus**

- Launch **TablePlus** on your system.
- Click on the **`+`** icon at the top to create a new connection.
- Select **MySQL** as the database type.

### 4. **Set Connection Details in TablePlus**

In the connection setup window, you will need to fill in the following fields:

- **Host**: `127.0.0.1` or `localhost`
- **Port**: `3306` (default for MySQL, or the port you specified in the `docker-compose.yml` file)
- **User**: `root` (or any other MySQL user you’ve created)
- **Password**: The password you set for the root user in the `docker-compose.yml` file (e.g., `secret`)
- **Database**: (Optional) If you want to connect directly to a specific database, enter the database name here. Otherwise, leave this blank to connect to the MySQL server and see all databases.

### 5. **Test and Connect**

- Click the **Test** button to check the connection.
- If the connection is successful, you’ll see a confirmation. If not, double-check the credentials and try again.
- Once the connection test passes, click **Connect** to open the MySQL database.

### 6. **Access Your Database**

Now, you should be connected to your MySQL database running inside Docker. You can view tables, run queries, and manage your database from within TablePlus.

### Troubleshooting

1. **Check MySQL Container Logs**: If the connection fails, check the MySQL container logs for any errors:
    
    ```bash
    docker-compose logs mysql
    ```
    
2. **Firewall Issues**: Ensure that port `3306` is not blocked by any firewall on your system.
3. **Bind MySQL to All Interfaces (Optional)**: If you encounter any issues connecting via `localhost` or `127.0.0.1`, ensure that MySQL is not bound to only `localhost` inside the container. You can explicitly bind it to `0.0.0.0` in your `docker-compose.yml` as follows:
    
    ```yaml
    environment:
      - MYSQL_ROOT_HOST=%
    ```
    

This will allow external connections to the MySQL server.

By following these steps, you should be able to connect your Docker-hosted MySQL instance to TablePlus for database management.