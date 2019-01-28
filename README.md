# docker-run-mongodb-with-auth
The Project will list the code snippets that you need to run to deploy MongoDB as a Docker Container with auth enabled.

## What is this Project for?
If you are looking for code snippets to use to setup MongoDB with auth enabled as a Docker Container, then you landed on the right page. So, I assume you already know what is 
1. NoSQL database
2. MongoDB Document Store
3. Docker essentials
4. Docker commands

### First, Create Docker Volumes
> Volume for the MongoDB data
```
docker volume create mongodb-data
```

> Volume for the MongoDB Config
```
docker volume create mongodb-config
```

### Second, pull the MongoDB Image
```
docker pull mongo
```

### Third, run the MongoDB as a Container
```
docker run -d --name mongodb -p 27017:27017 -v mongodb-data:/data/db -v mongodb-config:/data/configdb mongo --auth
```
> Let's try to understand the input options
1. `-d ->` Run the Container in detached mode
2. `-p ->` Publish the Container port `27017` as `27017`
3. `-v ->` Specify the mount volume for the `/data/db` and `/data/configdb` directory
4. Specify the Docker Image
5. `--auth ->` Additional input parameter to the Image. Purpose: Start the MongoDB with auth enabled.

### Fourth, login into Mongo Shell (`admin` database)
```
docker exec -it mongodb mongo admin
```

### Fifth, create a new User with Admin Privilege on any database and uses `admin` database
```
db.createUser({ user: 'mongodb_admin', pwd: 'secret', roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });
```

### Sixth, Exit the Shell
Press either `CTRL+C` or type `exit` on the shell

### Seventh, Login into Mongo Shell again (but this time login with authenticated username and password)
```
docker exec -it mongodb mongo -u mongodb_admin -p secret --authenticationDatabase admin
```

### Eigth, create a new database
```
use mongo_db
```

### Ninth, Create a new User who has access privileges on the newly created database
```
db.createUser({ user: 'mongo_db_user', pwd: 'secret', roles: ["readWrite", "dbAdmin"] });
```

### Last, exit the Shell
Press either `CTRL+C` or type `exit` on the shell

## That's it, you are done setting up MongoDB Container with auth enabled!
