# SQL
sequel is a relational DB <br>
There are many different SQL flavors available:
- MySQL
- PostgreSQL
- Oracle SQL

We will be using **Postgress** for our SQL flavor (RDMS, Relational Database Management System)  
PostgreSQL is an open source relational database management system ( DBMS )  

## Table of Contents
1. [Setting up PostgreSQL](#setting-up-postgresql)
2. [Database actions](#a-little-of-database-structuring)
3. [Using PSQL shell commands](#psql-shell-commands)
4. [Actually doing something on the DB](#working-on-dbs)
5. [Vocabulary](#vocab)
6. [Resources](#resources)
7. [PSQL COMMANDS](#psql-commands)
8. [Filtering](#filtering)
9. [SQL joins](#sql-joins)
10. [Adding constraints](#adding-constraints)


## Setting up PostgreSQL
I'm too lazy to explain, you cand find a tutorial [here.](https://www.tutorialspoint.com/postgresql/postgresql_environment.htm) <br>
Or you can use `brew` to install it

#### Checking if you got postgress SQL installed in computer via `brew`
In terminal type
```
psql
```
This will open a version of your actual postgress IF you have it already on your `brew` environment. <br>
```
$ psql
psql (9.5.4)
Type "help" for help.

taka=#
```
> To exit that command promp you enter `\q`

To add or install postgress you run this command:
```
brew install postgress
```

### A little of Database structuring
There are three types of *human intentions* that are executed in a DB:
- Migration
> change the structure of the storage like creating tables
> It's really destructive, use with caution

- Mutation
> **change DATA** , create new elements, updating, and deleting

- Selection
> **Obtaining information** without changing data

## PSQL shell commands
- `\?` command hints
- `\help <command_name>` detailed usage of a command
- `\l` list all databases
- `\c <databaseName>` connect to database
- `\d <nameOfTable>` view table head
- `\d+ <nameOfTable>` view table head with extra details

> once you connect correctly to DB you will see inside your command prompt  
> `<port#> <userLoggedIn>@<nameOfDB>=#`
> like this:
> `5432 rafacode@cheesy_burritos=#`

- `\dt` show's all tables *from inside the dabase*
- `\q` exits the postgress shell

> **very important to be inside of your `postgress` DB or your user DB to start creating**  

## Working on DBs
Inside postgres connecting to a database is done like so:
- `\c postgress`
Then create a DB
```
CREATE DATABASE circus;
```
> **DO NOT FORGET SEMICOLONS! `;`

and you can Delete a DB too
- `DROP DATABASE circus;`
So, we need to connect to that DB after
```
\c circus
```
Once inside we create a TABLE
```
CREATE TABLE animals (
```
> and without putting a `;` we hit enter and psql will keep on expecting more input
> you will also see the name of the table like this `circus(#`

This is an example of a table creating by properties
```
id SERIAL PRIMARY KEY,
name VARCHAR NOT NULL,
type VARCHAR NOT NULL,
description TEXT
);
```
> rules of the table can be modified later on
> but you have to add all the columns and names the moment you create it (have it designed before making it)

Now we can check our table using `\d`

id | name | type | description
-- | ---- | --------- | ---
1 | Taka | Ahrens | student | yeah sure

Creating a new table
```
circus=# CREATE TABLE trainers (
circus(# id SERIAL PRIMARY KEY,
circus(# name VARCHAR NOT NULL,
circus(# age INT NOT NULL,
circus(# address VARCHAR);
```

### Inserting values into postgress
Once on the correct DB and TABLE you will run this on the shell
```
INSERT INTO trainers (name, age, address) VALUES ('Jilly Cakes', 30, '123.');
```

> DB SHOULD BE NAMED IN PLURAL AND SNAKE CASE `like_this`

To see the value of a Table we use
`SELECT * FROM trainers;`

### Viewing data
To view the current user you are logged in
```
SELECT user;
```
To view current DB you are sitting on
```
SELECT current_database();
```

General targetting
```
SELECT name, age, FROM trainers WHERE age < 30;
```
> != is `<>` in psql  

Targetting text in SQL
```
SELECT * FROM trainers WHERE name LIKE '%JJ%';
// this is case sensitive
```
or
```
SELECT * FROM trainers WHERE name ILIKE '%JJ%';
```
> this option is case insensitive

## Things to review
- [ ] where am I putting the current DB
- [ ] How to disconnect from DB
- [ ] How to view table keys for DB
- [ ] Restructure Jason's class into notes
- [x] how to view current DB you are on in shell

## resources
- [Postgres Data Types](https://www.postgresql.org/docs/9.5/static/datatype.html)
- [SQL exercises](http://sqlzoo.net/)

---

## Data Base theory
Class by Jason Seminara <br>
Nov 3, 2016 <br>
Database: file on computer that describes data in two dimensional tables.  

**NOSQL DB:** unstructured data that does not need consistency checks or ACID constraints (speed and mostly accurate)
**SQL DB:** will be a little slower but your data could not be reviewd and there's less control (slower and very accurate)

**Relational DBs are NOT OOP.**

#### Single database non relational comparison
Pros | Cons
---- | ----
simple | file corruption
fast | stale data
     | long read/write time
     | line by line access

#### RDBMS is better by far
Pros | Cons
---- | ----
ACID (consistency) | slower to access info


### Entity Relationship Diagram
put image here about ERD  
**Many to Many cannot be represented by two tables**


#### Vocab
- RDBMS: Relational Database Management Service.  

> the productor of the RDBMS is the one who decides the datatypes

- row || entity || tuple || RECORD: collection of things.
- column: represents attributes
- field: the cross intersection of a row vs column = CELL
- Race Condition: anything that creates a circular pattern (loopable)
- ERD: Entity Relationshipt Diagram.
- ACID
    + Atomicity: every entity in the table represents one thing. **No duplicate entities.**
    + Consistency: the data is the same in any point of time. You either read at the beggining of a transaction, or you read at the end of it after you've finished.

    > if you put a number where it's expecting a char it will not allow it.
    > if there's data that repeats itself we have to remove to another table and relate by the ID of the real data.

    + Isolation: in order for a DB to remain in a consisten state you have to protect the transactions so that they execute in order and if one transaction fails the other can continue but only after the first one is done. GUARANTEE THAT THE TRANSACTIONS WON'T COLLIDE

    > example of the money deposit while someone else checks the balance: you can't see the balance correctly until first transaction happens. There are a lot of strategies for isolation: threading, locking etc

    + Durability: If the database fails, there is a log of everything it does so you can recreate the data using the log.

    > example of github and going back on a commit

- Transactions: is a list of statements that happen in order. Can contain roll backs and safety actions to avoid failing
- Staments:
- Rolling back: is an action sometimes taken when you have statements A,B,C,D and if it fails at B it will go back on those actions.


---

## PSQL COMMANDS
- CREATE DATABASE <name_of_database>
- CREATE TABLE <name_of_table>

## Filtering
- SELECT
- FROM
- WHERE

## Modifying
- INSERT
- UPDATE
- DELETE

---
## Data Types
- TEXT: huge size, no restrictions  
- INT
- VARCHAR:
> variable character assigns 'n' spaces to save, but when recieved it will reduce how free spaces to none.  

- SERIAL PRIMARY KEY: serialized version handled by psql for ID's

## SQL joins
SQL joining is querying data from more than one table.  
Best way to represent a Join is a _VENN diagram_.  

#### Types of joins
- Inner join.  
- Left join.  
- Full outer join.  
- Cross join.  
> This is also reffered to as a cartesian cross

## Creating a Schema file
Schema is a .sql file that we use to determine the structure and creation of tables.
This is an example of a schema file
```
-- this creates a singular transaction
BEGIN;

DROP TABLE IF EXISTS students;
DROP TABLE IF EXISTS cohorts;

CREATE TABLE cohorts (
  cohort_id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL,
  class VARCHAR NOT NULL
);

CREATE TABLE students (
  student_id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL,
  cohort_id INT
);

-- end of transaction
COMMIT;
```

### Loading schema file
Once you have a schema file where you determine your properties you can run the current `.sql` file that you have from terminal.  
> Remember to be doing this on terminal, not inside of the psql interface.  

```
psql -d library_lab -f schema.sql
```

### Loading a seeds file
Exact same command as in loading the schema, but pointing at the seeds file.  
```
$ psql -d library_lab -f seed.sql
```

## Adding constraints
Adding constraints is a way to verify that the information we are puttin inside the DB is related correctly.
> Constraints will slow down the process of seeding the information to your DB so make the constraint check after you have seeded DB  

```
ALTER TABLE ONLY city
    ADD CONSTRAINT city_pkey PRIMARY KEY (id);

ALTER TABLE ONLY country
    ADD CONSTRAINT country_pkey PRIMARY KEY (code);

ALTER TABLE ONLY countrylanguage
    ADD CONSTRAINT countrylanguage_pkey PRIMARY KEY (countrycode, "language");

ALTER TABLE ONLY country
    ADD CONSTRAINT country_capital_fkey FOREIGN KEY (capital) REFERENCES city(id);

ALTER TABLE ONLY countrylanguage
    ADD CONSTRAINT countrylanguage_countrycode_fkey FOREIGN KEY (countrycode) REFERENCES country(code);
```
