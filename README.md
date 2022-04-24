# learn-postgresql

# Basics
- This section contains:
  - Install `PostgreSQL` in `ubuntu 20.04 lts`
  - Configure PostgreSQL (setup password etc.)
  - Access PostgreSQL
  - Create User and Work with Multiple PostgreSQL users
  - Sign-In with specific user
  - Demo to check users permissions
  - Uninstall PostgreSQL in Ubuntu 20.04 lts
  - `PostgreSQL` vs `pgAdmin`
  - Install pgAdmin
  - pgAdmin modes
  - Demo of `pgAdmin` modes  
  
**Note:** Click on `Click to see more` to see all the part of this section

<details>
<summary>Click to see more</summary>

## 1. Install PostgreSQL in Ubuntu 20.04 lts
- `sudo apt-get update`
- `sudo apt install postgresql postgresql-contrib`

### Configure PostgreSQL 
- Just after the installation for our first time use configure it (set root user and root password to access PostgreSQL) by below steps:  
  - `sudo -u postgres psql`
  - `ALTER USER postgres PASSWORD 'root@123';` : to reset the password
  - `\q` : come out once the password is reset

  - Now, to enter `psql` need below command:
    - `psql -U postgres -h localhost`
      - it will prompt to give user postgres password
      - after logged in successfully let's list the Databases we have:
        - `\l` : it will show List of existing databases
        
## 2. Access PostgreSQL 
To access the PostgreSQL we have two options:

### Option-1:
- `sudo -i -u postgres` : to use/logged in as a postgres user
- `psql` : to start/access the postgres command line or interface
- `\q` : to quit the postgres command line or interface
- `exit` : to logout

### Option-2:
- `sudo -u postgres psql` : to get logged in and also start the postgres command line or interface
- `\q` : to quit the postgres command line or interface and also it gets out directly to the local terminal

## 3. Create a User
`sudo -u postgres psql` : at first always get logged in and go to the pg command line

### See/Work in Current User : Multiple PostgreSQL users
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

## 4. Sign-in with a user : Demo to check the permissions
### Option-1:
- `sudo -u sahadat psql` : to get logged in and start the command line interface for user `sahadat`
- `\conninfo` : to see which db and as which user connected currently

### Option-2:
- `psql -U postgres -h localhost` : to connect to the psql with the super user postgres, we can use any user in the place of `postgres` user and will get privileges accordingly
- `\conninfo` : current db info

### Demo to check the permissions
#### With superuser postgres
- connect with superuser `postgres` by `psql -U postgres -h localhost`
- `\l` : list of dbs
- `CREATE DATABASE testdb2;` : create a db
- `DROP DATABASE testdb2;` :  we can drop the databse `testdb2` to check whether we have drop permissions for this user

#### With user `sahadat`
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

### Create new user
- `sudo -u postgres createuser --interactive`
  - give the name of role (user name) to add, for example `sahadat`
  - tell whether new user role be a superuser, give y
- a user/role is created 

## 5. Create a new Database
- `sudo -u postgres createdb sahadatdb` : run this from local terminal, it will create a db named `sahadatdb`


## 6. Uninstall PostgreSQL in Ubuntu 20.04 lts
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

## 7. PostgreSQL vs pgAdmin
`pgAdmin` is the most popular and feature rich Open Source administration tool for PostgreSQL. Installing pgAdmin is optional but recommended. You can try an online demo of pgAdmin4 [here](https://www.pgadmin.org/try/).

- The PostgreSQL is a database engine implementing SQL standards. It usually listen as a server on a network tcp port to provide its abilities. 
- The pgAdmin is a sort of client. You are able to manipulate schema and data on an instance or multiple instances of PostgreSQL engines.

## 8. Install pgAdmin4:
Now, we'll see how to install pgAdmin4 in ubuntu 20.04 lts and how to use it to manage our PostgreSQL server

### Add pg
- Add the pgAdmin4 repository : Create the repository configuration file by: 
  - `curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add`
  - `sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'`

### pgAdmin4 can be installed in two modes:
#### Mode-1: Desktop mode
- To install desktop mode:
  - `sudo apt install pgadmin4-desktop`

#### Mode-2: Web mode
- To install web mode:
  - `sudo apt install pgadmin4-web`
- 

#### Combination of both mode
- To install both:
  - `sudo apt install pgadmin4`
  
## 9. Demo of pgAdmin4 desktop mode
- To open pgAdmin4 in desktop mode, search pgAdmin4 and open it in your pc
- a prompt will come and set a password (for ex: `pgAdmin`) to it, it's not related to psql so set any password that you want
- then click on `Add New Server` to connect your postgresql server
- we can add local pg server (in General info) --> then in connection give `localhost` and put everything default (port: 5432, maintenance db: postgres, username: postgres, password: give your password for the user that you set locally (for ex: `root@123`), save password) --> save it
- Now we added out local pg in pgAdmin, so you can see our postgresql db users and dbs and respective infos by clicking on the `Local PG` or the name that we provided during adding server to pgAdmin
- This is how we connect/use pgAdmin4 in desktop mode

## 10. Demo of pgAdmin4 web mode
- First need to configure it, to configure web mode, run the command below and this is required only one time after the installation:
  - open terminal and run `sudo /usr/pgadmin4/bin/setup-web.sh`
  - need to give a email and set a pass, with these email and pass you will login in pgAdmin in web mode
  - accept all the following things by giving `y`
  - now see at the end of the successful message of this command in terminal, you will see a local address is given and by that url you can pgAdmin in browser/web mode
- you won't see any server that you added in desktop mode in this web mode, you have to do it again


> Now you connect to any of the db and do the operations accordingly

> So we have seen the whole process of postgresql and pgAdmin in ubuntu 20.04 lts

## Resources
- [x] [Install PostgreSQL and pgAdmin on Ubuntu 20.04](https://www.youtube.com/watch?v=lX9uMCSqqko)
- [ ] [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)
- [ ] [Learn PostgreSQL Tutorial - Full Course for Beginners](https://www.youtube.com/watch?v=qw--VYLpxG4)
- [ ] [PostgreSQL Tutorial For Beginners | Learn PostgreSQL](https://www.youtube.com/watch?v=-VO7YjQeG6Y)

</details>

# Concepts Notes
- PostgreSQL is an advanced, enterprise class open source relational database that supports both SQL (relational) and JSON (non-relational) querying.

## PostgreSQL : Architectural Fundamentals
- see official [doc](https://www.postgresql.org/docs/current/tutorial-arch.html)

## PostgreSQL Server and Client
- A server process, which manages the database files, accepts connections to the database from client applications, and performs database actions on behalf of the clients. The database server program is called postgres . The user's client (frontend) application that wants to perform database operations.
- 

## PostgreSQL Clusters
- PostgreSQL groups databases into `clusters`, each of which is a consists of collection of databases and share a configuration and data location, and run on a single server instance with its own `TCP` port.
- If you only want a single instance of PostgreSQL, the installation includes a cluster named `main`, so you don't need to run `initdb` or `pg_createcluster` command to create one.
- If you need multiple clusters, then the Postgres packages for Debian and Ubuntu provide a command `pg_createcluster` to be used.
- And if you just wanna create a database, not a database cluster then use `createdb` command only.
- 

# Tutorials
This section contains tutorials of PostgreSQL

## Official [tutorial](postgresqltutorial.com)
- This section contains the official PostgreSQL tutorial, step by step are:
  - Getting Started with PostgreSQL
    - Install PostgreSQL, Access/Connect to PostgreSQL and Load sample DB in PostgreSQL on Linux
  - PostgreSQL quick start
    - What is PostgreSQL
    - PostgreSQL Sample Database
    - Connect to PostgreSQL Database
    - Load PostgreSQL sample database
  - PostgreSQL Fundamentals : SQL queries
    - Querying Data
    - Filtering Data
    - Joining Multiple Tables
    - Grouping Data
    - Set Operations
    - Grouping sets, Cube, and Rollup
    - Subquery
    - Common Table Expressions
    - Modifying Data
    - Transactions
    - Import & Export Data
    - Managing Tables
    - Understanding PostgreSQL Constraints
    - PostgreSQL Data Types in Depth
    - Conditional Expressions & Operators
    - PostgreSQL Utilities
    - PostgreSQL Recipes

**Note:** Click on `Click to see more` for seeing all the subsections of this section

<details>
<summary>Click to see more</summary>

### 1. Getting Started with PostgreSQL 
- This section helps you get started with PostgreSQL by showing you how to install PostgreSQL on Windows, Linux, and macOS. You also learn how to connect to PostgreSQL using the psql tool as well as how to load a sample database into the PostgreSQL for practicing.
  - follow this [doc](https://www.postgresqltutorial.com/postgresql-getting-started/)
#### Install PostgreSQL, Access/Connect to PostgreSQL and Load sample DB in PostgreSQL on Linux 
- follow this [doc](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-linux/)
- switch to a database by `\c <db_name>`
- use the `pg_restore` tool to [restore](https://www.postgresqltutorial.com/postgresql-restore-database/) any database:
  - `pg_restore --dbname=<db_name> --verbose <sample.tar>`

### 2. PostgreSQL quick start
#### What is PostgreSQL
- follow this [doc](https://www.postgresqltutorial.com/postgresql-getting-started/what-is-postgresql/)

#### PostgreSQL Sample Database
- follow [this](https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/)

#### Connect to PostgreSQL Database
- follow [this](https://www.postgresqltutorial.com/postgresql-getting-started/connect-to-postgresql-database/)

#### Load PostgreSQL sample database
- follow [this](https://www.postgresqltutorial.com/postgresql-getting-started/load-postgresql-sample-database/)

### 3. PostgreSQL Fundamentals : SQL queries
- First, we’ll learn how to query data from a single table using basic data querying techniques, including selecting data, sorting result sets, and filtering rows. 
- Then, we’ll learn about advanced queries such as joining multiple tables, using set operations, and constructing the subquery. Finally, we will learn how to manage database tables, such as creating a new table or modifying an existing table’s structure.

#### Querying Data
- `SELECT`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-select/)
  - show you how to query data from a single table.
- `COLUMN ALIASES`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-column-alias/)
  - learn how to assign temporary names to columns or expressions in a query.
- `ORDER BY`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-order-by/)
  - guide you on how to sort the result set returned from a query
- `SELECT DISTINCT`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-select-distinct/)
  - provide you with a clause that removes duplicate rows in the result set

#### Filtering Data
- `WHERE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-where/)
  - filter rows based on a specified condition
- `LIMIT`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-limit/)
  - get a subset of rows generated by a query
- `FETCH`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-fetch/)
  - limit the number of rows returned by a query
- `IN`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-in/)
  - select data that matches any value in a list of values
- `BETWEEN`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-between/)
  - elect data that is a range of values
- `LIKE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-like/)
  - filter data based on pattern matching
- `IS NULL`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-is-null/)
  - check if a value is null or not.

#### Joining Multiple Tables
- `Joins`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-joins/)
  - show you a brief overview of joins in PostgreSQL
- `Table Aliases`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-alias/)
  - describes how to use table aliases in the query
- `INNER JOIN`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-inner-join/)
  - select rows from one table that has the corresponding rows in other tables
- `LEFT JOIN`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-left-join/)
  - select rows from one table that may or may not have the corresponding rows in other tables.
- `RIGHT JOIN`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-right-join/)
  - 
- `SELF-JOIN`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-self-join/)
  - join a table to itself by comparing a table to itself.
- `FULL OUTER JOIN`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-full-outer-join/)
  - use the full join to find a row in a table that does not have a matching row in another table.
- `Cross Join`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-cross-join/)
  - produce a Cartesian product of the rows in two or more tables
- `Natural Join`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-natural-join/)
  - join two or more tables using implicit join conditions based on the common column names in the joined tables.

#### Grouping Data
- `GROUP BY`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-group-by/)
  - divide rows into groups and applies an aggregate function on each.
- `HAVING`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-having/)
  - apply conditions to groups.

#### Set Operations
- `UNION`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-union/)
  - combine result sets of multiple queries into a single result set.
- `INTERSECT`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-intersect/)
  - combine the result sets of two or more queries and returns a single result set that has the rows appear in both result sets.
- `EXCEPT`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-except/)
  - return the rows in the first query that does not appear in the output of the second query.

#### Grouping sets, Cube, and Rollup
- `GROUPING SETS`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-grouping-sets/)
  - generate multiple grouping sets in reporting
- `CUBE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-cube/)
  - define multiple grouping sets that include all possible combinations of dimensions
- `ROLLUP`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-rollup/)
  - generate reports that contain totals and subtotals

#### Subquery
- `SUBQUERY`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-subquery/)
  - write a query nested inside another query
- `ANY`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-any/)
  - Retrieve data by comparing a value with a set of values returned by a subquery
- `ALL`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-all/)
  - query data by comparing a value with a list of values returned by a subquery
- `EXISTS`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-exists/)
  - check for the existence of rows returned by a subquery

#### Common Table Expressions
- `PostgreSQL CTE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-cte/)
  - introduce you to PostgreSQL common table expressions or CTEs
- `Recursive query using CTEs`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-recursive-query/)
  - discuss the recursive query and learn how to apply it in various contexts.

#### Modifying Data
In this section, you will learn how to insert data into a table with the INSERT statement, modify existing data with the UPDATE statement, and remove data with the DELETE statement. Besides, you learn how to use the upsert statement to merge data.
- `INSERT`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-insert/)
  - guide you on how to insert a single row into a table.
- `Insert multiple rows`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-insert-multiple-rows/)
  - show you how to insert multiple rows into a table.
- `UPDATE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-update/)
  - update existing data in a table.
- `UPDATE JOIN`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-update-join/)
  - update values in a table based on values in another table
- `DELETE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-delete/)
  - delete data in a table.
- `UPSERT`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-upsert/)
  - insert or update data if the new row already exists in the table.

#### Transactions
- `PostgreSQL Transactions`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-transaction/)
  - show you how to handle transactions in PostgreSQL using BEGIN, COMMIT, and ROLLBACK statements.

#### Import & Export Data
You will learn how to import and export PostgreSQL data from and to CSV file format using the copy command.

- `Import CSV file into Table`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/import-csv-file-into-posgresql-table/)
  - show you how to import CSV file into a table.
- `Export PostgreSQL Table to CSV file`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/export-postgresql-table-to-csv-file/)
  - show you how to export tables to a CSV file.

#### Managing Tables
In this section, you will start exploring the PostgreSQL data types and show you how to create new tables and modify the structure of the existing tables.
- `Data types`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-data-types/)
  - cover the most commonly used PostgreSQL data types.
- `Create a table`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-create-table/)
  - guide you on how to create a new table in the database.
- `Select Into & Create table as`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-select-into/) and [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-create-table-as/)
  - shows you how to create a new table from the result set of a query.
- `Auto-increment column with SERIAL`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-serial/)
  - uses SERIAL to add an auto-increment column to a table.
- `Sequences`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-sequences/)
  - introduce you to sequences and describe how to use a sequence to generate a sequence of numbers.
- `Identity column`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-identity-column/)
  - show you how to use the identity column.
- `Alter table`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-alter-table/)
  - modify the structure of an existing table.
- `Rename table`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-rename-table/)
  - change the name of the table to a new one.
- `Add column`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-add-column/)
  - show you how to use add one or more columns to an existing table.
- `Drop column`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-drop-column/)
  - demonstrate how to drop a column of a table.
- `Change column data type`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-change-column-type/)
  - show you how to change the data of a column.
- `Rename column`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-rename-column/)
  - illustrate how to rename one or more columns of a table.
- `Drop table`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-drop-table/)
  - remove an existing table and all of its dependent objects.
- `Truncate table`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-truncate-table/)
  - remove all data in a large table quickly and efficiently
- `Temporary table`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-temporary-table/)
  - show you how to use the temporary table.
- `Copy a table`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-copy-table/)
  - show you how to copy a table to a new one.

#### Understanding PostgreSQL Constraints
- `Primary key`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-primary-key/)
  - illustrate how to define a primary key when creating a table or adding a primary key to an existing table.
- `Foreign key`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-foreign-key/)
  - show you how to define foreign key constraints when creating a new table or add foreign key constraints for existing tables.
- `CHECK constraint`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-check-constraint/)
  - add logic to check value based on a Boolean expression.
- `UNIQUE constraint`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-unique-constraint/)
  - make sure that values in a column or a group of columns are unique across the table.
- `NOT NULL constraint`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-not-null-constraint/)
  - ensure values in a column are not NULL.

#### PostgreSQL Data Types in Depth
- `Boolean`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-boolean/)
  - store TRUE and FALSE values with the Boolean data type.
- `CHAR, VARCHAR and TEXT`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-char-varchar-text/)
  - learn how to use various character types including CHAR, VARCHAR, and TEXT.
- `NUMERIC`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-numeric/)
  - show you how to use NUMERIC type to store values that precision is required
- `Integer`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-integer/)
  - introduce you to various integer types in PostgreSQL including SMALLINT, INT and BIGINT.
- `DATE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-date/)
  - introduce the DATE data type for storing date values.
- `Timestamp`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-timestamp/)
  - understand timestamp data types quickly.
- `Interval`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-interval/)
  - show you how to use interval data type to handle a period of time effectively.
- `TIME`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-time/)
  - use the TIME datatype to manage the time of day values
- `UUID`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-uuid/)
  - guide you on how to use UUID datatype and how to generate UUID values using supplied modules.
- `Array`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-array/)
  - show you how to work with the array and introduces you to some handy functions for array manipulation.
- `hstore`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-hstore/)
  - introduce you to data type which is a set of key/value pairs stored in a single value in PostgreSQL.
- `JSON`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-json/)
  - illustrate how to work with JSON data type and shows you how to use some of the most important JSON operators and functions.
- `User-defined data types`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-user-defined-data-types/)
  - show you how to use the CREATE DOMAIN and CREATE TYPE statements to create user-defined data types.

#### Conditional Expressions & Operators
- `CASE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-case/)
  - show you how to form conditional queries with CASE expression.
- `COALESCE`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-coalesce/)
  - return the first non-null argument. You can use it to substitute NULL by a default value.
- `NULLIF`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-nullif/)
  - return NULL if the first argument equals the second one
- `CAST`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-cast/)
  - convert from one data type into another e.g., from a string into an integer, from a string into a date.

#### PostgreSQL Utilities
- `psql commands`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-administration/psql-commands/)
  - show you the most common psql commands that help you interact with psql faster and more effectively.

#### PostgreSQL Recipes
- `How to compare two tables`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/compare-two-tables-in-postgresql/)
  - describe how to compare data in two tables in a database.
- `How to delete duplicate rows in PostgreSQL`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/how-to-delete-duplicate-rows-in-postgresql/)
  - show you various ways to delete duplicate rows from a table.
- `How to generate a random number in a range`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-random-range/)
  - illustrate how to generate a random number in a specific range.
- `EXPLAIN statement`:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-explain/)
  - guide you on how to use the EXPLAIN statement to return the execution plan of a query.
- `PostgreSQL vs. MySQL `:
  - follow [this](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-vs-mysql/)
  - compare PostgreSQL with MySQL in terms of functionalities.

</details>


# Advanced Stuffs
- This section contains:
  - Pgpool-II

<details>
<summary>Click to see more</summary>

## Pgpool-II
- It's a PostgreSQL opensource extension
- with Pgpool we can build highly available system that can continue to operate even a failure


</details>

