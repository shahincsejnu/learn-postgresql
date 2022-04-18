# learn-postgresql

## Basics

### Install PostgreSQL in Ubuntu 20.04 lts
- `sudo apt-get update`
- `sudo apt install postgresql postgresql-contrib`

#### Configure PostgreSQL 
- Just after the installation for our first time use configure it (set root user and root password to access PostgreSQL) by below steps:  
  - `sudo -u postgres psql`
  - `ALTER USER postgres PASSWORD 'root@123';` : to reset the password
  - `\q` : come out once the password is reset

  - Now, to enter `psql` need below command:
    - `psql -U postgres -h localhost`
      - it will prompt to give user postgres password
      - after logged in successfully let's list the Databases we have:
        - `\l` : it will show List of existing databases
        
### Access PostgreSQL 
To access the PostgreSQL we have two options:

#### Option-1:
- `sudo -i -u postgres` : to use/logged in as a postgres user
- `psql` : to start/access the postgres command line or interface
- `\q` : to quit the postgres command line or interface
- `exit` : to logout

#### Option-2:
- `sudo -u postgres psql` : to get logged in and also start the postgres command line or interface
- `\q` : to quit the postgres command line or interface and also it gets out directly to the local terminal

### Create a User
`sudo -u postgres psql` : at first always get logged in and go to the pg command line

#### See/Work in Current User : Multiple PostgreSQL users
- `\conninfo` : to see currently on which DB and as a which user connected
- `\du` : to see current set of users, this list the users and roles they have, role name column is shows the users basically
- `CREATE USER sahadat WITH CREATEDB LOGIN ENCRYPTED PASSWORD 'admin';` : create another user/role name here, can grant superuser or other type of user as well as, here login
- above command will CREATE ROLE, once we create this (user/role) then also create a db with the same name as the user, this db will be used to store the infos about the user (his role, permissions etc infos)
  - `CREATE DATABASE sahadat;`
- Now if we run `\du` command we'll see the latest created user/role with their permissions
- Now, if we want to grant specific access to a specific database to a user, we can use the command:
  - first create a database by `CREATE DATABASE testdb;`
  - remember:
    - `\l` : to see all the db
    - `\du` : to see all the users
  - let's grant the user `sahadat` all access to the db `testdb`:
    - `GRANT ALL PRIVILEGES ON DATABASE testdb TO sahadat;` : we've granted all permissions to the user sahadat of the db testdb
    - see the privileges by `\l` in the access privileges column of the respective database
    - this helps when we need to work with multiple db and users and restrict the access specificly
    - 
- `\q`

**Note:** So far, we have installed PostgreSQL in ubuntu 20.04, and we've also created multiple users in our PG, the super user is postgres which came with the installation and then we created another lesser privilege user is sahadat. The privileges you can have depends on the user with which you are connecting to the postgres.

### Sign-in with a user : Demo to check the permissions
#### Option-1:
- `sudo -u sahadat psql` : to get logged in and start the command line interface for user `sahadat`
- `\conninfo` : to see which db and as which user connected currently

#### Option-2:
- `psql -U postgres -h localhost` : to connect to the psql with the super user postgres, we can use any user in the place of `postgres` user and will get privileges accordingly
- `\conninfo` : current db info

#### Demo to check the permissions
##### With superuser postgres
- connect with superuser `postgres` by `psql -U postgres -h localhost`
- `\l` : list of dbs
- `CREATE DATABASE testdb2;` : create a db
- `DROP DATABASE testdb2;` :  we can drop the databse `testdb2` to check whether we have drop permissions for this user

##### With user `sahadat`
- connect with user `sahadat` from the local terminal run `psql -U sahadat -h localhost` ; Note: set the password for this user to run this command, otherwise use sign-in with a user option-1
- `\l` : here also can list the databases as this user have this permission
- `CREATE DATABASE testuserdb;` : we can also create a db with this user
- `\l` : check whether that `testuserdb` is created or not
- `DROP DATABASE testuserdb;` : you will see we can also drop this created db with this user (note: this db is created by this user)
- `\l` : see existing databases and see whether that `testuserdb` was dropped or not
- `DROP DATABASE testdb;` : let's try to drop another db which is existing and not created by this user, 
  - here it'll show an error : `ERROR:  must be owner of database testdb`
  - this is happens because of privilege of this user

- Now, we see a demo of user role in PostgreSQL

#### Create new user
- `sudo -u postgres createuser --interactive`
  - give the name of role (user name) to add, for example `sahadat`
  - tell whether new user role be a superuser, give y
- a user/role is created 

### Create a new Database
- `sudo -u postgres createdb sahadatdb` : run this from local terminal, it will create a db named `sahadatdb`


### Uninstall PostgreSQL in Ubuntu 20.04 lts
- `sudo apt-get --purge remove postgresql`
- `dpkg -l | grep postgres` : List all postgres related packages
- `sudo apt-get --purge remove package1 package2 ..` : remove all the above listed packages using the command
- Confirm all the files and folders related to `postgres/postgresql` are deleted using the command:
  - `whereis postgres`
  - `whereis postgresql`
  - Remove all the files and folders listed using rm command:
    - `sudo rm -rf /etc/postgresql`
  - Delete the user postgres using the command:
    - `sudo userdel -f postgres`

### PostgreSQL vs pgAdmin
`pgAdmin` is the most popular and feature rich Open Source administration tool for PostgreSQL. Installing pgAdmin is optional but recommended. You can try an online demo of pgAdmin4 [here](https://www.pgadmin.org/try/).

- The PostgreSQL is a database engine implementing SQL standards. It usually listen as a server on a network tcp port to provide its abilities. 
- The pgAdmin is a sort of client. You are able to manipulate schema and data on an instance or multiple instances of PostgreSQL engines.

### Install pgAdmin4:
Now, we'll see how to install pgAdmin4 in ubuntu 20.04 lts and how to use it to manage our PostgreSQL server

#### Add pg
- Add the pgAdmin4 repository : Create the repository configuration file by: 
  - `curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add`
  - `sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'`

#### pgAdmin4 can be installed in two modes:
##### Mode-1: Desktop mode
- To install desktop mode:
  - `sudo apt install pgadmin4-desktop`

##### Mode-2: Web mode
- To install web mode:
  - `sudo apt install pgadmin4-web`
- 

##### Combination of both mode
- To install both:
  - `sudo apt install pgadmin4`

### Demo of pgAdmin4 desktop mode




