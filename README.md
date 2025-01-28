# MySQL
docker-compose.yml

```dockerfile
services:
  mysql:
    image: mysql:8.0
    container_name: mysql-server      # Name the container
    environment:                      # Define environment variables
      MYSQL_ROOT_PASSWORD: admin
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin
    ports:
      - "3306:3306"                   # Map port 3306
    volumes:
      - mysql-data:/var/lib/mysql     # Persist data using a named volume

volumes:
  mysql-data:                         # Define the named volume for MySQL data
```
To start the MySQL server with Docker Compose, run:
```shell
docker-compose up -d
```
Verify the version of MySQL
```shell
docker exec -it mysql-server mysql --version
```
If you want to start the services again, you can use the following command:
```shell
docker-compose start
```

1. Open **MySQL Workbench**
2. Create a New Connection
Click on + next to "MySQL Connections" on the main screen.
A dialog box titled Setup New Connection will appear.
3. Configure the Connection
Fill in the following fields:  
  **Connection Name**: Docker MySQL - Give it a name
  **Hostname**: 127.0.0.1 - Enter localhost (if you're running Docker on your local machine).  
  **Port**: 3306 (as defined in your docker-compose.yml file).  
  **Username**: admin (as specified in MYSQL_USER in your docker-compose.yml).  
  **Password**: Click **Store in Vault...** and enter admin (as specified in MYSQL_PASSWORD).  
  4. **Test the Connection**  
  Click the Test Connection button.
  If everything is configured correctly, you’ll see a success message: "Successfully made the MySQL connection".  
  If it fails:  
  Make sure the MySQL container is running (docker ps).
  Ensure port 3306 is open and not blocked by a firewall.


**Notes**:  
Confirm that the container is running:
```shell
docker ps
```
Steps to apply change  
Stop and delete the current container:
```shell
docker-compose down
```
Delete old data on your volume
```shell
docker volume rm mysql-data
```
Start over
```shell
docker-compose up -d
```
