### Postgresql Documentation - aritra basak


> Progresql is a open source object-relational database system 

> we can access postgre database engine through sql.


**some basic commands in psql**

```bash
   
   \? => get all the commands which is helpful (q to quit)
   \l => shows the list of databases
   \q => quit
   \d => describe (used for how many tables are present)
   \d <table_name> => describe the table name 
   \du => check how many users you have 
   
   psql --help => all the commands 

```


**create a new database**

```bash

  CREATE DATABASE mydatabase;

  # after connection to the database,the next thing is to connect the database 
  \c <database_name>
  # or 
  # -h => host, -p => port, -U => username 
  psql -h localhost -p 5432 -U postgres <database_name>; 


```

**Delete any Database**

```bash
   
   DROP DATABASE <database_name>;

```

> You must first go into the database: \c <database_name>

**Create Table**

```bash
  
   CREATE TABLE student(
     
     id INT,
     name VARCHAR(20),
     age int, 
     gender VARCHAR(7)

   	);
  
  # to view 
  \d <database_name>

```


```bash
  
   CREATE TABLE student(
     
     id BIGSERIAL NOT NULL PRIMARY KEY, # BIGSERIAL -> signed integer value with auto increment 
     name VARCHAR(20) NOT NULL,
     age int, 
     gender VARCHAR(7)

   	);
  
  # to view 
  \d <database_name>

```


**Insert Data into the table**

```bash

   INSERT INTO student(
       name,
       age,
       gender
    ) VALUES ('aritra',23,'male');

   # if you do not want to add gender 
   INSERT INTO student(
     
     name,
     age
    )
   VALUES ('aritra',23); 

   # to view the data inside the table
   SELECT * from student;

```

**Select Data from Table**

```bash
   
   SELECT name FROM student; # select only name 
   SELECT name,age FROM student; # select only name and age 

```

> Ascending and Descending order.

```bash
   
   SELECT * from student ORDER BY name DESC;
   # get all data but in descending order of name.

```

> DISTINCT

```bash
   
   # if you have lots of duplicate names but you want names once, you do not care about all same name value then you use DISTINCT
   SELECT DISTINCT name from student ORDER BY name DESC; 
    
   # name          name 

   # aritra        aritra
   # aritra   =>    
   # aritra 

```


> WHERE clause, AND, OR

WHERE clause is used to filter data based on some conditions.

```bash
   
   SELECT * from student WHERE age > 20;

   SELECT * from student WHERE age > 20 AND gender = 'male';

```

> LIMIT, FETCH, OFFSET

```bash
   
   # lets say that you want to get first 10 records 
   SELECT * from student LIMIT 10;

   # if you want to eliminate some data
   SELECT * from student OFFSET 2; # eliminate first 2 records 

   # FETCH is the official way to limit data. (SQL standard)
   SELECT * from student FETCH FIRST 10 ROW ONLY; 

```

> IN 

Takes an array's of values as an input and return the matching values.

```bash
  
  SELECT * from student where age = 23 or age = 19;

  # instead of doing this, you can write like this 

  SELECT * from student where age IN (23,19);

```

> BETWEEN 

To select data in a range

```bash
  
  SELECT * from student WHERE age BETWEEN 17 AND 20;

```

> LIKE and ILIKE

It is used to match string with wild card characters.

```bash
  
  # name's last three characters must be 'tra'
  SELECT * from student WHERE name LIKE '%tra';

  #ILIKE is used for case sensitive purpose
  SELECT * from student WHERE name ILIKE '%TrA'; # same as '%tra'  

```


> GROUP BY , HAVING

Allows us to group our data based on a column

```bash
  
  # your table may has 
  id  name  age   gender

  1    aritra  23   male 
  2    priya   22   female
  3    arpit   24   male 
   
  #  
  SELECT name,COUNT(*) FROM student GROUP BY name;

  # o/p =>

  name     count 
   
   aritra    1
   priya     1
   arpit     1
  
  SELECT age,age > 22 from student GROUP BY age;

  # o/p =>

  age    column 

  23       t 
  22       f 
  24       t


  # lets say you wanna eliminates the false value 

  SELECT age,age > 22 FROM student GROUP BY age HAVING age > 20;
   

  # o/p =>

  age    column 

  23       t  
  24       t 


```


**Aggregator function in SQL**



```bash
  
  # max
  SELECT MAX(price) from car;

  # min
  SELECT MIN(price) from car;

  # avg 
  SELECT AVG(price) from car; 

  # round
  SELECT ROUND(AVG(price)) from car;

```

Add many columns with manipulated data 

```bash
  
  # lets say that you have a table 
  model        price 
   
   spyker      12000
   volkswagen  23000
   ford        19000 
  

  # now you want to see what will be price, if i give
  # 10%, 12% and 15.5% discounts

  SELECT model, price, price * 0.10 as p1, price * 0.12 as p2, price * 0.15 as p3 FROM car;

  # o/p =>

  #car name  # price  
  model      price   p1  p2  p3    

```

> Handling null or empty values in postgresql


**COALESCE**

```bash
   
   when you have a column, and inside this column you have many rows which are empty, you want to somehow fill the empty value with some default values.


   name     email

   aritra    hefu
   debesh     
   mou       ejfb
   priya     ijfh
   sou       efueh
   bama       

   SELECT COALESCE(email,'---') from student;
   # the email value which is empty substitude it with '---' this value;

   # o/p =>
      email

      hefu
       --- 
      ejfb
      ijfh
      efuer
       ---


```


> Timestamps and Dates

```bash
  
  # Timestamp is used to get the data and time
  SELECT NOW();

  SELECT NOW()::TIME;  # gives you the time.

  SELECT NOW()::DATE;  # only gives you the date.

```


> ALTER keyword

```bash
   
  # It is a DDL command that means it works with the structure of the table not the actual data present in the table.


  # add a new column name marks
  ALTER TABLE student ADD marks INT(10);

  # remove a column 
  ALTER TABLE student DROP marks;


  # add a unique constraint to a column 
  # give unique constraint to email column 
  # unique_email => have to give a name.
  ALTER TABLE student ADD CONSTRAINT unique_email UNIQUE (email);


```

> Unique CONSTRAINT


Allows us to have an unique value per column. That means duplicate values are not allowed in a same constraint. 

```bash
   
   ALTER TABLE student ADD CONSTRAINT unique_email UNIQUE (email);
   

   # to drop the constraint 
   ALTER TABLE student DROP CONSTRAINT unique_email;

```


> CHECK CONSTRAINT

You want to allow some specific data inside a column.




```bash

  gender 

   male 
   female
   hello
  
  # if someone add hello in gender, then the query will not execute because you only allow 'male' or 'female'.
  
  # gender_constraint => is the unique name.
  
  ALTER TABLE student ADD CONSTRAINT gender_constraint CHECK (gender = 'male' OR gender = 'female');



```

> Update and Delete Records 


```bash
  
  DELETE FROM car WHERE name = 'Hyundai';

  UPDATE car SET model = 'Hyundai', color = 'red' WHERE id = 12345;

```

> ON CONFLICT, DO UPDATE, DO NOTHING 


```bash
   
   # lets say that you have a table 
   id    name      age     gender   email  
   1     aritra    23       male    abc@gmai.com 


   # Conflict must occur here 
   # to handle this you add ON CONFLICT 
   # you update the email 
   # EXCLUDED.email = the new email you entered 
   INSERT INTO student(id,name,age,gender,email) VALUES (1,'aritra',23,'male','abcde@gmail.com') ON CONFLICT DO UPDATE SET email = EXCLUDED.email;

   # Do nothing if conflict occur 
   INSERT INTO student(id,name,age,gender,email) VALUES (1,'aritra',23,'male','abcde@gmail.com') ON CONFLICT DO NOTHING;


```


> Primary key and Foreign key 


```bash
   
   CREATE TABLE student(
      
      id BIGSERIAL NOT NULL PRIMARY KEY,
      name VARCHAR(20),
      email VARCHAR(15)

    );


   CREATE TABLE car(
     
     id BIGSERIAL NOT NULL PRIMARY KEY,
     model VARCHAR(10),
     s_id BIGINT REFERENCES student(id)  # create a foreign key 

    );

   # If you want to delete any record of student which has a reference to car table.
   # Then you should first delete the record from car and then delete it from student 

```


> EXPORTING TO CSV files


```bash

  \COPY (select * from car) TO 'H:\folder\data.csv' DELIMITER ',' CSV HEADER;

```

> UUID (Universally Unique Identifier)


Always use as primary key instead of BIGSERIAL

```bash
   
   # you have to first add the extensions 
   CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

   # to check how many funtions are availabe 
   \df 

   # to get a uuid value 
   SELECT uuid_funtion_v4();  # function name

```


> How to apply UUID in your table?


```bash
 
     CREATE TABLE student(
      
      student_id UUID NOT NULL PRIMARY KEY,   # change the data type to uuid 
      name VARCHAR(20),
      email VARCHAR(15)

    );


   CREATE TABLE car(
     
     car_id UUID NOT NULL PRIMARY KEY,
     model VARCHAR(10),
     s_id UUID REFERENCES student(student_id)  # create a foreign key 

    );


   INSERT INTO student(student_id,name,email) VALUES ( uuid_funtion_v4(), 'aritra', 'abc@gmail.com' );


```


> Indexing 


```bash

   CREATE INDEX <index_name> ON <table_name> (<column_name>);

```


> CREATE VIEW in postgresql



 1. logical table for creating abstraction.
 1. Security.
 1. Simplicity. (If you have many table, and you want to create a complex join every time to view some information. Then you can create a view and from a simple query you can get the complex query data)



```bash
  
   # lets say you have very confidential data in your database 

   id     name     age    phone

   1       a        23     46746575
   2       b        22     47583757


   # and now you are the ceo of a company, and you do not want to show the phone number to your employees.

   # so you create a view 

   CREATE VIEW <view_name> AS 
   SELECT id,name,age FROM <table_name>; # in one line  



   SELECT * FROM <view_name>;

   id    name    age 

   1     a        23
   2     b        22 


   #  **** REMEMBER if you working with your view with a same user, then if you do any manipulation with you data this will reflect on the main table data.

```


> CREATE USER in postgresql


```bash
   
   CREATE USER <user_name> PASSWORD '<password>';

   # grant or deny permission to user 
   
   # user can only select 
   GRANT SELECT ON <table_name> TO <user_name>;

   # user can only update 
   GRANT UPDATE ON <table_name> TO <user_name>;

```
