<!---
title: SQL Setup, Insert, Update, and Delete
type: lesson
duration: "1:25"
creator:
    name: Jay Nappy
    city: NYC
competencies: Databases
--->

<!--9:40 10 minutes -->

<!--Hook: Remember when we talked about how MongoDB was the 5th most popular Database?  Do you remember what the 1st, 2nd, 3rd, and 4th were all versions of?  That's right, SQL.  Today, we're going to talk about SQL.  Before we go on, it's worthwhile to mention that MongoDB defines itself explicity as noSQL, that is, the system we are about to learn for saving data is very different from Mongo.  While Mongo was defined by BSON, or Binary JSON, SQL organizes data into tables, rows, and columns.  OK, let's jump into it. -->

# SQL Setup, Insert, Update and Delete

### Objectives
*After this lesson, students will be able to:*

- **Create** a database table
- **Insert, retrieve, update, and delete** a row or rows into a database table

### Preparation
*Before this lesson, students should already be able to:*

- Install **[PostgreSQL](http://www.postgresql.org/)**
- Describe the relationship between tables, rows, and columns
- Draw an ERD diagram

## We know about Databases, but what is SQL? Intro

Let's review: at its simplest, a relational database is a mechanism to store and retrieve data in a tabular form.  Spreadsheets are a good analogy!  But how do we interact with our database: inserting data, updating data, retrieving data, and deleting data? That's where SQL comes in!

#### What is SQL?

SQL stands for Structured Query Language, and it is a language universally used and adapted to interact with relational databases.  When you use a SQL client and connect to a relational database that contains tables with data, the scope of what you can do with SQL commands includes:

<!--Have developer read aloud -->

- Inserting data
- Querying or retrieving data
- Updating or deleting data
- Creating new tables and entire databases
- Control permissions of who can have access to our data

Note that all these actions depend on what the database administrator sets for user permissions: a lot of times, as an analyst, for example, you'll only have access to retrieving company data; but as a developer, you could have access to all these commands and be in charge of setting the database permissions for your web or mobile application.

#### Why is SQL important?

Well, a database is just a repository to store the data and you need to use systems that dictate how the data will be stored and as a client to interact with the data.  We call these systems "Database Management Systems", they come in _many_ forms:

- MySQL
- SQLite
- PostgreSQL (what we'll be using!)

...and all of these management systems use SQL (or some adaptation of it) as a language to manage data in the system.

<!--9:50 10 minutes -->

## Connect, Create a Database - Codealong

Let's make a database!  Open your terminal and type:

```bash
$ psql
```

You should see something like:

```bash
your_user_name=#
```

>Note: If you get an error about a `Library not loaded`, you may need to reinstall `postresql` with the following command `brew reinstall postgresql`.

Great! You've entered the PostgreSQL equivalent of the node REPL: now, you can execute PSQL commands, or PostgreSQL's version of SQL.

Let's use these commands, but before we can, we must create a database.  Let's call it wdi:

```psql
your_user_name=# CREATE DATABASE wdi;
CREATE DATABASE
```

The semicolon is important! Be sure to always end your SQL queries and commands with semicolons.

Now let's _use_ that database we just created:

```psql
your_user_name=# \c wdi
You are now connected to database "wdi" as user "your_user_name".
wdi=#
```

<!--10:00 10 minutes -->

## Create a table - Demo

Now that we have a database, let's create a table (think of this like, "hey now that we have a workbook/worksheet, let's block off these cells with a border and labels to show it's a unique set of values"):

```sql
CREATE TABLE instructors (
  ID  INT PRIMARY KEY NOT NULL,
  NAME TEXT NOT NULL,
  EXPERIENCE INT NOT NULL,
  WEBSITE CHAR(50)
);
```

When we paste this into psql:

```psql
wdi=# CREATE TABLE instructors (
wdi(#  ID          INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME        TEXT                NOT NULL,
wdi(#  EXPERIENCE  INT                 NOT NULL,
wdi(#  WEBSITE     CHAR(50)
wdi(#  );
CREATE TABLE
```

Notice the different parts of these commands:

```psql
wdi=# CREATE TABLE instructors (
```
This starts our table creation, it tells PostgreSQL to create a table named "instructors"..

```psql
wdi(#  ID        INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME      TEXT                NOT NULL,
```

...then, each line after denotes a new column we're going to create for this table, what the column will be called, the data type, whether it's a primary key, and whether the database - when data is added - can allow data without missing values.  In this case, we're not allowing NAME, AGE, or ID to be blank; but we're ok with website being blank.

<!--What does this remind you of from Mongo?  Schema! -->

<!--10:10 10 minutes -->

## Create a student table and insert data - Independent Practice

Now that we've done it to keep track of our instructors, let's create a table for students that collects information about:

- an id that cannot be left blank
- the students name that cannot be left blank
- their age
- and their address that cannot be left blank.

Remembering the commands we just went over, students, try to guide the instructors through this!  

<!-- show solution after 5 minutes:

```sql
CREATE TABLE students (
  ID INT PRIMARY KEY NOT NULL,
  NAME TEXT NOT NULL,
  AGE INT NOT NULL,
  ADDRESS CHAR(50)
);
```
-->

<!-- In psql that will look like:

```psql
wdi=# CREATE TABLE students (
wdi(#  ID          INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME        TEXT                NOT NULL,
wdi(#  AGE         INT                 NOT NULL,
wdi(#  ADDRESS     CHAR(50)
wdi(#  );
CREATE TABLE
```
-->

Great job! Now let's finally _insert_ some data into that table - remember what cannot be left blank!

We'll insert five students, Jack, Jill, John, Jackie, and Slagathorn. The syntax is as follows:

```psql
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...);
```

Let's do it for Jack, together:

```sql
INSERT INTO students VALUES (1, 'Jack Sparrow', 28, '50 Main St, New York, NY');
```
In psql that will look like:

```psql
wdi=# INSERT INTO students VALUES (1, 'Jack Sparrow', 28, '50 Main St, New York, NY');
INSERT 0 1
```

<!--10:20 10 minutes -->

## Insert Data - Independent Practice

Now, you try it for the other students, and pay attention to the order of Jack's parameters and the single quotes - they both matter.

- Jill's full name is Jilly Cakes; she's 30 years old, and lives at 123 Webdev Dr. Boston, MA
- Johns's full name is Johnny Bananas; he's 25 years old, and lives at 555 Five St, Fivetowns, NY
- Jackie's full name is Jackie Lackie; she's 101 years old, and lives at 2 OldForThis Ct, Fivetowns, NY
- Slagathorn's full name is Slaggy McRaggy; he's 28 and prefers not to list his address

<!-- after 8 minutes, show this:

```sql
INSERT INTO students VALUES (2, 'Jilly Cakes', 30, '123 Webdev Dr. Boston, MA');
INSERT INTO students VALUES (3, 'Johnny Bananas', 25, '555 Five St, Fivetowns, NY');
INSERT INTO students VALUES (4, 'Jackie Lackie', 101, '2 OldForThis Ct, Fivetowns, NY');
INSERT INTO students VALUES (5, 'Slaggy McRaggy', 28);
```

In psql this should look like:

```psql
wdi=# INSERT INTO students VALUES (2, 'Jilly Cakes', 30, '123 Webdev Dr. Boston, MA');
INSERT 0 1
wdi=# INSERT INTO students VALUES (3, 'Johnny Bananas', 25, '555 Five St, Fivetowns, NY');
INSERT 0 1
wdi=# INSERT INTO students VALUES (4, 'Jackie Lackie', 101, '2 OldForThis Ct, Fivetowns, NY');
INSERT 0 1
wdi=# INSERT INTO students VALUES (5, 'Slaggy McRaggy', 28);
INSERT 0 1
```
-->

<!--10:30 15 minutes -->

## What's in our database? Catch-up

So now that we have this data saved, we're going to need to access it at some point, right?  We're going to want to _select_ particular data points in our dataset provided certain conditions.  The PostgreSQL SELECT statement is used to fetch the data from a database table which returns data in the form of result table. These result tables are called result-sets. The syntax is just what you would have guessed:

```psql
SELECT column1, column2, column3 FROM table_name;
```
We can pass in what columns we want to look - like above - at or even get all our table records:

```psql
SELECT * FROM table_name;
```

For us, we can get all the records back:

```psql
wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
(5 rows)
```

We can get just the name and ages of our students:

```psql
wdi=# SELECT name, age FROM students;
      name      | age
----------------+-----
 Jack Sparrow   |  28
 Jilly Cakes    |  30
 Johnny Bananas |  25
 Jackie Lackie  | 101
 Slaggy McRaggy |  28
(5 rows)
```

#### Getting more specific

Just like JavaScript, all of our comparison and boolean operators can do work for us to select more specific data.

- I want the names of all the students who aren't dinosaurs - done:

```psql
wdi=# SELECT name FROM students WHERE age < 100;
      name
----------------
 Jack Sparrow
 Jilly Cakes
 Johnny Bananas
 Slaggy McRaggy
(4 rows)
```

- How about the names of students ordered by age? Done:

```psql
wdi=# SELECT name, age FROM students ORDER BY age;
      name      | age
----------------+-----
 Johnny Bananas |  25
 Jack Sparrow   |  28
 Slaggy McRaggy |  28
 Jilly Cakes    |  30
 Jackie Lackie  | 101
(5 rows)
```

- How about reversed? Ok:

```psql
wdi=# SELECT name, age FROM students ORDER BY age DESC;
      name      | age
----------------+-----
 Jackie Lackie  | 101
 Jilly Cakes    |  30
 Jack Sparrow   |  28
 Slaggy McRaggy |  28
 Johnny Bananas |  25
(5 rows)
```

- How about those who live in Fivetowns? We can find strings within strings too!

```psql
wdi=# SELECT * FROM students WHERE address LIKE '%Fivetowns%';
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
(2 rows)
```

<!--10:45 10 minutes -->

## Updates to our database - Catchup

Ok, there are some mistakes we've made to our database, but that's cool, cause we can totally update it or delete information we don't like. Let's start by adding one more student:

```psql
wdi=# INSERT INTO students VALUES (6, 'Miss Take', 500, 'asdfasdfasdf');
INSERT 0 1
```

But oh no, we messed them up - Miss Take doesn't live at asdfasdfasdf, she lives at 100 Main St., New York, NY.  Let's fix it:  

```psql
wdi=# UPDATE students SET address = '100 Main St., New York, NY' where address = 'asdfasdfasdf';
UPDATE 1

wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
  6 | Miss Take      | 500 | 100 Main St., New York, NY
(6 rows)
```

But wait, actually, she just cancelled - no big!

```psql
wdi=# DELETE FROM students where name = 'Miss Take';
DELETE 1

wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
(5 rows)

```

<!--10:55 10 minutes -->

## Independent Practice

There's _no way_ you're going to remember the exact syntax of everything we just did, but let's practice a habit you should have been doing since week 1: finding and reading documentation. Checkout [this PostgreSQL tutorial](http://www.tutorialspoint.com/postgresql/) and using the same database and datatable of users, get through a many of these SQL challenges as possible in the next 10 minutes:

- Insert five more students:
  - Nancy Gong is 40 and lives at 200 Horton Ave., Lynbrook, NY
  - Laura Rossi is 70 and listed her address as "Unlisted"
  - David Daniele is 28 and lives at 300 Dannington Ln., Washington, DC.
  - Greg Fitzgerald is 25 and did not list an address
  - Randi Fitz is 28 and lives in Oceanside, NY

- Randi wants her address to be corrected to 25 Broadway, New York, NY
- Get a list of student names and addresses who are older than 30 and order them by their address alphabetically
- Get a list of students whose first name begins with the letter "J"
- Get a list of student names who live in NY or MA

<!--11:05 5 minutes -->

## Conclusion

#### Common Postgresql Commands

Here are a list of some common Postgresql commands that you might need:

- `\c` - connect to database
- `\l`
- `\d`
- `\d+`
- `\q`
- `\h` - help


Answer these questions:

- How does SQL relate to relational databases?
- What kinds of boolean and conditional operators can we use in SQL?
