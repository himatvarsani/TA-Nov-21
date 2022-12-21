# MySQL 

### MySQL docker

Using a container makes things easy to run an application. Just run the following command to map the right folder to the container data folder.

```sh
mkdir ~/Documents/cloudreach/TalentAcademy/database
```

```sh
docker run --name some-mysql -p 3306:3306 -v ~/Documents/cloudreach/TalentAcademy/database:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:latest
```

You can also perform other commands with your docker, e.g.:
```sh
# Stop a running container
docker stop container_name

# Start a stopped container
docker start container_name

# list all (running/exited) containers
docker ps -a
```

### Install client

```sh
brew install mysql-client
```

Open the `~/.zshrc` to add the `export` variable. This will make sure that all binaries for mysql is available to be use.

```sh
code ~/.zshrc
# or
vi ~/.zshrc
```

```sh
# mysql
export PATH="/usr/local/opt/mysql-client/bin:$PATH"
```

### Connect to your database

```sh
mysql -h 127.0.0.1 -u root -p
```

### Create Database

```sql
SHOW DATABASES;
CREATE DATABASE movie_db;
```

### Tables

```sql
# Select what Database to use
USE movie_db;

# List Existing Tables
SHOW TABLES;

# Create your table
CREATE TABLE Movies(
    id int NOT NULL AUTO_INCREMENT,
    name varchar(255),
    release_year int,
    PRIMARY KEY (ID)
);
```

# Display the structure of a table
```sql
DESCRIBE Movies;
```

Insert data into your table

```sql
INSERT INTO Movies VALUES(null, "Stargate", 1994);
INSERT INTO Movies VALUES(null, "True Lies", 1995);
```

### SELECT queries

```sql
SELECT * FROM Movies;
SELECT * FROM Movies WHERE release_year < 2000
```