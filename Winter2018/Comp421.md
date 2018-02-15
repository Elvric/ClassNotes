# __Comp421__ 
## Intro 08/01/2017

### Marking Scheme
* 3 individual assignments 4% each
* Map-reduce Exercis
* 3 Projects 5% each
* Midterm 10%
* Final 58%

## Entity-Relationship Model 10/01/2018

### Steps in Database and Design
1. Requirement Analysis
    
    Identify the data that needs to be stored
    Identify the operation that need to be exectuted
    on the data

2. Conceptual Database Design
    
    Sementic data model to develop semantic schema
3. Map semantic schema to relational schema

### Requirement Analysis
    Identify the entities and relationships in the company
    Store the relevent information that we are looking for in the database
    Ensure that all rules and regulation are respected
    Make sure that the operations we want to perform on the database can be done optimaly. 

#### Student database
* Student ID
* Name personal address and so on
* Functionality
    * data can be viewed and changed
    * Instructor
        * Can teach a course
        * Can enter grades
    * Course registration
    * Course cancelation

#### Course database
* Term
* Course ID
* CRN
* days
* time
* capacity
* Enrolment
* TAs

### Semantic Data Model (ER)

    E/R model allos for pictorially description of the data.
        An E/R diagram is a representation of the data model of
        the application
        It should be understandable by non-computer scientist
    Main concepts are entitites and relationships
    Similar to UML class diagram.

### Entity
> Described as a set of attributes in an object that differentiate it from other objects. No functions

### Entity set
> Collection of similar entities, all companies registered in Quebec
>
> They must have a key: minimum set of attibutes that uniquely differentiate them from the other entities in the set.

### ISA
> Subclasses: A ISA B then every A entity is also a B entity 
>
> Key attributes must remain in the highest identity sets
>
> Constraints
> * Overlap: Can an entity be in more than one subclass
> * Coverting: Must every entity in the superclass exist in one of the subclass

### Relationship
    Collection of similar relationships

___
## Lecture 2 15/01/2018

### Database rules
* Rectangle is an entity
* Circle is an attribute
* Rombus relathionships
* Arrow means that there can only be one
* Bold line means that it is a nececity.
* Partial key underlined with a dashline.
* Owner key is underlined with a line.

### Weak entities 
    Partial key happends when an entity cannot be identified only by one of its attributes.
    These are called week entities, the entity is then surounded by bold lines. Weak entities have to be in a constrain with the entity that makes them unique. This is an example with a team and its players that are identified by their number and team name. 
    We are usually in a one to many relationship where the owner of the week entity can have multiple weak entities but not the otherway arround.

### Relationships VS entity sets
    Entity sets may be used when required in order to be able to keep track of different events happening such as payments that needs to be recorded instead of just updating the total amount of money transfered since the begenning. 

---
## 17-01-2018
### First airline model
#### Airline entity sets
* Flights
    * arr_time
    * dept_time
    * dept_apt
    * arr_apt
    * date
    * **flight_no**
* Passenger
    * **Name**
    * **Addr**
#### Airline Relationship
* Flight,Passenger --BookFlight (price)

### Second airline model

#### Entity sets:
* Flight_rout
    * __Flight number__
    * arr_time
    * dep_time
    * dep_apt
    * arr_apt
* Flights
    * _date_ (weak entity)

#### Airline Relationship
* Flight_rout, Flight --has 
    * Flight,Passenger --BookFlight (price)

### Definition
Relation: Made of 2 parts \
Schema: specifies name of relation plus a set of attribute plus the domain type of each attribute (column header=attribute). \
Instance: set of distinct tupples. \
Data Definition Language DDL: defines the schema of the database \
Data manipulation Language (DML): manipulates the data

### SQL data Types
* Syntax
    * CHAR(n) character string up to n with blanks
    * VARCHAR(n) denotes strup up to n
    * INT,INTEGER names are synonym
    * FLOAT, REAL
    * Decimal(n,d) value consist of n digits with decimal point d position from the right
    * DATE (YYYY-MM-DD)
    * TIME (00:00:00)
``` SQL
    CREATE TABLE Students
    (
        sid INTEGER PRIMARY KEY /*Primary key is sid*/,
        name VARCHAR(30) NOT NULL UNIQUE, /*forces value to not be null and have a different name*/
        login VARCHAR(30),
        faculty VARCHAR(30),
        major VARCHAR(30) 
        DEFAULT 'undefined'
        PRIMARY KEY (sid,name) /*Says that the primary key is these two columns*/
    )
    INSERT INTO Students
    VALUES (5366,'Bartoli',...)
    INSERT INTO Students (sid,name,faculty)
    SET major = 'sdfwdf'
    SELECT name 
    FROM Students
    DROP TABLE Students /*removes the table*/
``` 
---
## 22-01-2018
### Foreign key
Set of attributes in one relation R that is used to refer to a tuple in another relation Q. Represents a logical pointer. \
The foreign key must exit in the real Q table that is assigned to it so we have a constraint.

Ex:
Refering back to the table above sid can be use as a foreign key to detect the enrolment relationship. 
```SQL
CREATE TABLE Enrolled
{
FOREIGN KEY (sid) REFERENCES Students /* The database will automatically check if the sid we enter exist in Students
Also prevents when a student is removed to be removed when (sid) is used in the other table
*/
}
```
It is important to be carefull with that and children there is a way that we can cascade down the deletion to the children but it is not recommended. If we know that something is a primary key declare it as not null as good practice.

### Many-Many relationship set
* Companies can have primary key as cname.
* Parties can have primary key as pname.
    * cname and pname are used as the primary key for lobbying. These two attributes must be labbled as foreign key. 
```SQL
CREATE TABLE Sponsorship
{
    PRIMARY KEY (cname,pname),
    FOREIGN KEY cname REFERENCE Companies,
    FOREIGN KEY pname REFERENCE Parties
}
```
* Member
    * Can only belong to one party hence we must note link the member unique key with party name we have to find a unique key for the member. So the foreign key of a membership would be the the primary key of this table.
```SQL
    CREATE TABLE Membership
    {
        PRIMARY KEY (mname),
        FOREIGN KEY (mname) REFERENCES Members,
        FOREIGN KEY (pname) REFERENCES Parties
    }
```
Note: If we put a **null** as the foreign key it will be exepcted but we can fix that by putting not null in the table created where the key will be stored.

### Participation constraints
If we have a participation constraints for one instance we must not create an extra table for the relationship as we want to enfore the participation hence every time an instances of the constraint is made we must have a field for that constrain. 

If we have a participation constraints for multiple instances this is not possible because we cannot create an independent table nor can we have this stored directly on the instance hence we have to keep a note saying that we cannot implement this relationship in the relational model and it is the responsibility of the entries to make sure that the participation constraints are respected.

### Renaming
 When for example we create a spinoff company that has a parent company we must be careful with the way we implement these models ensuring that the keys and foreign keys constraint are well placed and resepcted. 

### Weak Entity Sets
* Team (**tname**,ranking)
* Playes (**tname,shirtN**,pname)
```SQL
    CREATE TABLE Players
    {
        PRIMARY KEY (tname,shirtN),
        FOREIGN KEY (tname) REFERENCES (Teams) /* making sure that the entity we are related to actually exist*/
    }
```
### Translating ISA Hierarchy
* Companies(name,addr,empl)
    * Sole(name,owner)
    * Partnership(name)
    * Corporation(name,level)
```SQL
    CREATE TABLE Corporation
    {
        FOREIGN KEY (name) REFERENCES Companies
    }
```
We do this this way because Companies have their own table so when we create a subset like Corporation we create a new table with new data but we must make sure that the table it is attached to for Companies in order to know the number of employees hence we use a foreign key. 

---
## 24-01-2018
Remark: For one to one enforced relationship only use 1 table unless one of the entities also has other relationship which in this case makes the model impossible.

## Relational Algebra
### Relational query languages
They allow manipulation and retrieval of data from a data base
* Relation algebra
    * Operational 
    * Very useful for representing execution plan
* Relational Calculus
    * Descriptive - a query describes how the data to be retrieve looks like. 
* SQL,PigLatin,OGL,Xquery

Ra Consists of two basic operators.
1. Input: one or two relations
    * schema of relation is know
    * Instance can be arbitrary 
2. Output: a relation
    * Schema of output relation depends on operator and input relation 

Relation algebra on its own does not look and care what the keys are. \
It also requires each record to be unique we cannot have 2 records that are exactly the same. 
#### Single relation as input
* Selection sigma : selects a subset of tuples from a relation
    * Only want specific instances of the data
* Projection Pi: projects to a subset of attributes from a 
relation.
    * Only want certain columns of the Table
* Renaming ro: of relations or attributes useful when combining several operators
    * Temporary variable for operators.

#### Two relation as input
* Cross product X: Combine 2 relations
* Join BowTie ⋈: Combination of cross product and selection
* (Division): not covered in class

#### Set operators with two relations as input
The relation we work on must be set compatible: same column types not title in the same order, same number of attributes.
* Intersection
* Union
* Set Difference

#### Example
* PI_sname,rating (Skaters)= |sname|rating| (Columns)
* PI_age (Skaters)= |age| (Columns) Note that if two ages are the same they will not be output.
* sigma_rating>8(Skaters) = |Skaters > 8|
* Pi_sname,rating (sigma_rating>8(Skaters)) = |sname|rating>8|
    * Stepwise one operator at a time (Build temporary relations)
    * Consecutve operators on the fly on scan through one relation (Not always possible)
    * *Note to be known but cool*: **Depending on the database systems these two ways may be more optimized, today we use mostly column based data base systems**
* Skaters union OurSkaters = |Skaters union OurSkaters| (No duplicates)
* Skaters and OurSkaters = |Skaters and OurSkaters| (No duplicates)
* Skaters - OurSkaters = |Skaters - OurSkaters| (No duplicates)
* Pi_sname,rating,age (Skaters) union Pi_sname,rating,age (OurSkaters) 
---
# 29-01-2018

## Relation algebra continued
* Cross-Product X
    * {a,b} X {c,d,e}={ (a,c) , (a,d) , (b,c) , (b,d) , (a,e) , (b,e)}
* ⋈ Joins Cross-Product + 
    * Acts like sigma for simple algebra and perform the cross product afterwards
* Equi Join same as join but we do equal relation more often.
    * We link relationship tables with certain instance of entities which is very important when wanting to see what relationships are present.
* Natural Join is a type of equi join
    * We do not every single time the equality simple
    * It builds a join with columns with exactly the same name so if we have multiple columns with the same name it will have an equality condition for both of them. These columns only appear once in the table
* Renaming
    * Important when we want to join table with itself
    * Hard if they have the same column names.
    * Creates one more alias just like when we create a new variable tha holds the address of an object that already exists we just have created one more pointer nothing more
    * row(Temp,Skaters) Now skater is aliased with Temp
    * row((Temp1(sidI,ratingI)),Skaters(sid,rating))
#### Example
* Pairs all the information on one side with the one on the left
* Join Skaters ⋈_(Skaters.rating > OurSkaters.rating) OurSkaters

#### In Class examples
S skater table, P competitions result and C competition dates and details. 

S
| sid | sname | rating | age
| --- | --- | --| --|
| | | |

C 
| cid | date |type|
| --- | --- | --|
| | |
P
| sid | cid | rank|
| --- | --- | --| 
| | |

1. Find the name of Skaters that have participated in competition 101
$\Pi{sname}(\sigma_{cid=101}(P) \bowtie S)$
2. Find name of skaters who have participated in a local competition
$\Pi_sname(\sigma_{type='local'}(C) \bowtie P \bowtie S)$
3. Find sids of skaters who have participated in regional or local competitions. \
$\Pi_{sid}(\sigma_{type='local' \ \lor \ type='regional'}(C) \bowtie P)$
4. Find sids of skaters who have participated in a regional or local competition \
$\Pi_{sid}(\sigma_{type='local'}(C) \bowtie P) \cap \Pi_{sid}(\sigma_{type='regional'}(C) \bowtie P)$


### Equivalence
As long as the projection keeps the column names as that the operator needs to work on we can switch them. \
Joining multiple tables as well can be done in any order. \
The and operator for sigma can be used if they work on different columns.

----
# 31-01-2018

## SQL introduction

```
psql cs21
// in data base
SELECT *
FROM skaters;
//Do not send the command to database unless ; is added

```
We can also use SQuirreLSQL that can be used as a more user friendly AI to access the database. \
For psql we can use pgAdmin 4

Note that SQL does not eliminates duplicate
``` SQL
SELECT columns  /*what columns we want*/

INSERT INTO skaters VALUES(...,...)

FROM list of relations /*Gives you the tables we are inte
rested in*/

WHERE qualifications /*uses to see the conditions*/

AND /*is the general and attribute */

SELECT DISTINCT columns FROM table /*in this case we remove
duplcated data in the columns */

WHERE age BETWEEN 13 AND 17 /*useful*/

WHERE NOT /*reverses the condition */

WHERE age <> 9 /*not equal*/

SELECT sid,rating+1 FROM skaters
/* modifies data on the output but not in the data
base */

SELECT sid, rating, rating+1 as newrating FROM skaters
/* creats an output with an extra columns with newrating containing 
rating+1 note that as long as SELECT is used there is no
data changed in the database nore added
*/

SELECT * FROM skaters where sname LIKE 'de%y'
/* starts with de end with y */

LIKE '_d%y'
/* second character is a e and last a y */

SELECT * FROM skaters ORDER BY age
/* order the age from smallest to largest */

ORDER BY age,rating

ORDER BY age, rating DESC
/* DESC from highest to lowest only for the imidiate right*/

15 as maxrating
/* creates a variable that is in front of all the output */
```

## Multirelational Queries Join

```SQL
SELECT * FROM skaters,participates
/*mixes both the record combination between skaters and participate table */

SELECT * FROM skaters,participates WHERE skaters.sid=
participates.sid
/* equi join query in this case but some columns may be repeated*/

SELECT skaters.sid, sname FROM skaters,participates WHERE skaters.sid=participates.sid
/*now columns of interest only */

SELECT * FROM skaters s,participates  p WHERE s.sid=
p.sid

/*renaming */
```

## Union, Intersection, Difference
```SQL
SELECT psid FROM participates p, competition c 
WHERE pcid=c.cidAND c.type= 'local'
/*all the skaters in local competition */

SELECT psid FROM participates p, competition c 
WHERE pcid=c.cidAND c.type= 'local'
UNION SELECT psid FROM participates p, competition c 
WHERE pcid=c.cidAND c.type= 'regional'
/* Here we are uniting regional and local competitions SQl 
removes duplicate using SET opperators like union intersect and difference*/

UNION ALL /*keeps duplicates in this case */

SELECT psid FROM participates p, competition c 
WHERE pcid=c.cidAND c.type= 'local'
INTERSECT SELECT psid FROM participates p, competition c 
WHERE pcid=c.cidAND c.type= 'regional'

SELECT psid FROM participates p, competition c 
WHERE pcid=c.cidAND c.type= 'local'
EXCEPT SELECT psid FROM participates p, competition c 
WHERE pcid=c.cidAND c.type= 'regional'

WHERE sid IN(31,58)
/*just like an array true if the element is in the array */
/*IN can also just accept a table of values from SELECT 
the selection column size must be greater or match the IN "tuple" size */
/* in does not always have duplicates but it still may depending on the IN statement used like a key*/

SUBQUERY /* used as condition statement in IN for example*/

EXISTS(SELECT…) /*Check if the statement inside produces an output for each row in the other table*/

SELECT sname FROM skaters s WHERE EXISTS( SELECT * FROM Participates p WHERE p.cid =101 AND p.sid=58)

rating >=ALL /*Looks at all the data so all the ratings */

rating > ANY /*looks at all the data and removes the person with
the lowest rating */

LIMIT 2 /*only consider 2 records but do not use this in 
exam*/
```
---
# 05-02-2018
## Aggregations
```SQL
SELECT rating FROM skaters;
/*gives all the ratings*/

SELECT count(rating) FROM skaters;
/*counts all the ratings even if they are the same*/

SELECT count(DISTINCT ratings) FROM skaters;
/*count all the distinct rating */

SELECT AVG(age) FROM skaters WHERE rating=9; 
/*gives us the average we can use casting to convert
to integers and so on */
```
Derive tables are tables made temporarily for calculations.
```SQL
(SELECT age
FROM skaters
WHERE rating =9)t;
```
```SQL
SELECT MAX(age) FROM skaters;

SELECT AVG(age) FROM skaters GROUP BY rating;
/* return the average age for each rating category
But it will not tell us what rating the AVG 
is associate with*/

SELECT rating,AVG(age) FROM skaters GROUP BY rating;

SELECT rating,AVG(age),COUNT(*) FROM skaters GROUP BY rating;
/*count(*) means count the records*/
```
Carful with group by for example aggregating a column of Strings is not possible.
```SQL
SELECT rating, AVG(age)
FROM skaters
GROUP BY rating
WHERE AVG(age) >10
/*this will fail as the where operation happens before the grouping. */

HAVING AVG(age) >10
/* this is the correct way of doing it only works by group*/

SELECT MIN(AVG(S2.age))
FROM Skaters S2
GROUP BY rating
--Wrong nested AGG operators.
--Use derive table instead.

CREATE VIEW activeSkaters (sid,skatername) AS
SELECT DISTINCT s.sid, s.sname
FROM skaters s, participates p 
WHERE s.sid = p.sid ;
-- Adds a new table to the database 

DROP VIEW activeSkaters
-- Removes it from the database 
```

## NULL Values
* Unknown/Missing
* Inapplicable
### Comparing NULL to values
If something is NULL the incrementing it in a table will fail, when we compare NULL with normal operators just like = NULL will not work. 
| A | NOT A|
|:-:|:-:|
|true|false|
|false|true|
|unknown|unknown|
With and unknown and false gives us false. With or unknown or true is true.
```SQL
IS NULL
IS NOT NULL
```
In real world we might want to do operations with it so 
```SQL
COALESCE(rating,0)+1 moderating 
--or we can do the following
CASE WHEN rating is NULL THEN 0 ELSE rating END +1
-- remember case and when it will be useful.
```
In this case it replaces every NULL with 0 and adds 1 to the 0, if it is not null we just add 1. 

---
# 07-02-2018
## Query when considering NULL
```SQL
SELECT rating,count(*) FROM skaters --gives us 7 outputs with 2 NULLs as records
SELECT distinct rating FROM skaters --here the 2 nulls are considered the same as a record.
```
Usually unknowns of the same type are considered to be the same.
```SQL
SELECT count(*) rating FROM skaters;
SELECT count(rating) rating FROM skaters;
-- First gives us 

SELECT distinct count(rating) rating FROM skaters;
--Nulls are not considered at all.
SELECT AVG(rating),
AVG(age),SUM(rating),COUNT(*),count(rating) FROM skaters;
--AVG divides by the number of records use to compute the sum, --if we have 2 nulls and 5 not null entries then that means that 
-- sum/5 removing the nulls
```

## Joins
```SQL
SELECT sname
FROM skaters s (INNER) JOIN participates p 
ON s.sid=p.sid
--Inner word is optional it implies that the records
-- should be matching

SELECT sname
FROM skaters s LEFT OUTER JOIN participates p 
ON s.sid=p.sid
-- All the record on the left side should make it to the output
-- Even if they do not match on the right.
FULL OUTER JOIN 
--Used to put every records from both tables in the output
--No matter if there are equals. 
```

## Level of abstraction
* Conceptual schema defines logical structure
* Physical schema describes the files and indexes used
* Different views describe how users see the data
* Physical data dependance
    * Protect from changes in the physical diagram 
* Logical data dependance 
    * Protect from changes in the logical data

## Insert values
```SQL
INSERT INTO ActiveSkaters (
    SELECT Skaters.sid Skaters.name FROM Skaters
)
--Here the data is just being inserted into a table and displayed
DELETE FROM Competitions 
--The Competitions table is done
UPDATE Skaters
SET ranking=10, age = age+1 WHERE
name = 'debby' or name = 'lilly'
-- here we are updating the rows
```
Good practice is instead of doing the above first run a select query and update the select afterwards.

## SQl integrity constraints
Is an entry in the table legal, type and information. Insert,deletes and updates that violates IC's are disallowed.
Covered so far:
* Data types (name must be a String)
* NOT NULL
* For relation as a whole
    * Primary key and Unique constraints
* Across relations
    * Relationship such that any instance in one table must exist in another table.
    * Referential integrity through foreign key constraint

## Attribute-based Checks
```SQL
rating INTEGER CHECK (rating > 0 AND rating <11)
```
The system checks the conditions set by the table. 
If the check checks multiple attributes we write it at the end after we have set all the columns in our table. \
What if a constrain changes: 
``` SQL
CONSTRAIN rat CHECK ...
ALTER TABLE skaters DROP CONSTRAINT ratage
ALTER TABLE skaters ADD CONSTRAINT ratage CHECK ...
```
Here we have given a name to the constrains so we can remove and insert it. If when we add a constrain some data in the table violates the new constraint SQL will give us an error and will not add the constrain to the table.

## SQL and Code
* SQL commands can be called from within a host language
    * Python, C++ and Java
* Two main approaches
    * Embed SQL in host language (C with embeded SQL)
    * Create special API to call SQL command 
* Program DB interaction
    * Application program executes at client side
    * Each SQl statement is sent to the server, execute there and the result is sent to the client.

### Java
Java.sql package and then we can connect it to drivers JDBC Driver DB2 for DB2.cs.mcgill.ca this is done by the driver that test what kind of connection it is and then load the correct driver. 

---
# 12-02-2018
```java
import java.sql.*;
class connectExample
{
    public static void main(String[] args)
    {
        Drivermanager.registerDriver (new com.ib.db2.jcc.DB@Dreiver());
        Connection conn= DriverManager.getConnection
        ("jdbc:db2://comp421.cs.mcgill.ca:50000/cs421",
        "user_name:","user_password");

        conn.close();
    }
}
```

## After the connection
```java
Statement stmt=con.createStatement();
stmt.closee();
stmt.executeUpdate(sqlString);
//Insert/update return number of affected tupples
```
* result of query is usual a SET of tuples with no a priori bound on the number of records.
* No support for such a data structure in conventional programming.
* We can result tuples step by step.
    * Cursor Concept:
        * Point to successive tples in the result realtion at a given moment when we getNextRecor() the cursor moves to the next row.
```Java
public class getQuer
{
    ResultSet rs = stmt.executeQuery("SELECT * FROM skaters");
    //must call next to get first record
    while(rs.next())
    {
        int sid= rs.getInt("sid");
        String sname = rs.getString("name");
        int age = rs.getInt(3); //bad we get to 3rd column
    }
    rs.close();
}
```
#### Use docs.oracle for the libriary of GDBC 

```java
PreparedStatment prepareCid= 
    con.prepareStatment("SELECT sid 
                        FROM participates where sid = ?");
while()
{
    //get cid input from user into variable x;
    
    //provide values for input parameter
    prepareCid.setInt(1,x); //replaces the ?
    ResultSet rs= preparedCid.executreQuery();
}
```
## Error Handling
2 levels SQLException and SQLWarning. \
Let say we update entries in the table and no entries get updated,
then the database will send a warning to tell us that no record where changed. 

```java
catch(SQLException e)
{
    e.getMessage(); 
    e.getErrorCode();
    e.getSQLState();
    System.err.println(alsjfsjlafjl);
}
finally 
{
    //Check if connection is not null.
    //Close all connections when we can.
}
```
### More complex query
Procedural extension to SQL. \
Certain database Systems allow the use of standard programming languages (with extensions). \
```SQL
CREATE PROCEDURE MIN_SALARY(IN deptNumber, SMALLINT, IN minsal DOUBLE)
LANGUAGE SQL
BEGIN 
    DECLARE v_salary DOUBLE;
    DECLARE v_id SMALLINT;
    DECLARE at_end INT DEFAULT 0;
    DECLARE C1 CURSOR FOR
    SELECT id,salary FROM staff WHERE did=depnumber;
    DECLARE CONTINUE HANDLER FOR not_found SET at_end =1;
    OPEN C1;
```
```java
con.prepareCall("{call min-salary(?,?)}");
cs.setInt(1,5); cs.setInt(2,2000);
//So the first num is the index the rest is the answer 
```
## Two tier model
We have the application and the database. Java communicates with SQL.  Java program can take space and must be shipped to all the user. The clients must execute the program with user id and password. It may not be usefull to have a user id per user for the program. And if we use the same user ID and password is not secure as anyone getting the program can access the data.

## Three tier model
When we connect to the database first we connect to the application server, the application cheks the user name and password by going into the database using its own private userid and password. \
User connect to an application server that has the program that itself connects with the database. So the client uses a webbrowser to access the database. \
Now looking at Amazon we are not going to create a userid for every user. So we use connection polling, it uses a commun user id for all user. Application server opens 50 connections let say. When client logs in it will check if it has a connection available and give it to the client.

# Application design choices
Should we check if the data_of_birth is valud at the webpage or at DB?
Better to do it at the webpage.
If we have to increment the rating of all skaters it is more efficient to do it in the database directly. .
* Trapping user error better to do it at the webpage
* Complex process over data shall be done in the database
* For the assignment do most of the constrains in the database rather than in the cl

# Internals of a DBS I 
## Memory Hiarchy
* The lower we go towards the disk it gets slower and slower
* Bits cost more the higher we go
* As we go down the number of space increases
* Top is random accessed but in the disk it is sequential. We cannot access what we want just like that.
* Lower end the data is saved but higher end when there is no power there is no data.
* Lower layers have blocks that are large but in higher level we can just read 1 bite. 

### Fun fact
* A sector is the most basic unit that the harddrive allows you to write. 
* A block is used at the higher level and a block size can have more than 1 sector. A block is the smallest unit of data defined by the system that the OS defines.

---
# 14-01-2018

### Distribution
We have a buffer that is use so that we can access the data as a memory variable. Then we can write the changes back to the secondary storage that is stable. 

#### memory database: we have the entire database residing in memory with these systems. The data is still stored in secondary storage for security. 

### Writing data the the disk 
Appending is easy I just need to keep writting where the last inforamtion is. \
So regarding the memory data base let say we change the address of the student. We can use loging (log file) Which is being written onto.

Properties of login, we write to the file the information at the end of the log file, such as student id had address changed. The next way is also to backup the database every couple of days.

Now we can restore the database copy that we had from the last hour and then use the log file and change that onto the memory database. 

The issue though is that as the database size increase the memory data base will be too small. 

## Large scale distribution
Same concept as above except that we split into segments the secondary memory and the memory (RAM). So now we can use memory based database in segments. If we then want to increase the size we can just add a segment (buy a new drive and a new ram). It is also faster as it allows parallel search quieries compare to the previous system.

## Final arachiteture
* Secondary storage
* Buffer
* Cache buffer
* Upper layer

These are abstracts but it is the one will use for the course although different databases have different architechtures.

## Buffer managment
Page size and fram side must be the same size. Table of fram# and pageid pairs is maintained. 

### Loading a page from the disk
First we have an empty fram in our RAM, if they are all taken we can use another fram, if fram is dirty (page was modified), write it to the disk. \
We choose usualy the least recently used frame to be replaced. 

#### Page Pins
When we load we must have a pin counter to 0. \
After new page is loaded we set the pin counter to 1. \
Counter could be more than 1. \
After we are done with the operations we decrement the counter and set the dirty bit if the page has been modified. \
Hence the only time we load a pages we check that the pin counter is at 0 and then check the dirty bit. 

## DBMS vs OS File System
### Why not use OS?
All the OS are different and the way they manage it may differ hence portability issue. \
Some limitations, e.g., files can't span disks. \
### Why use DBMS?
pin a page in buffer pool, force a page to disk. \
Adjust replacement policy, and pre-frech pags based on access patterns in typical DB operations. 

## Record format in DBS
Record is fixed such that we know that every record will have a fixed amount of bites. sid in skaters will be fixed. 

### Variable length
So here we store the address rather than the data that tells us where to read from. We can even use the address value to know the length of the field, by doing next address - previous address.

## Page format
Used and free space pointer in order to start writting new data. Then we can write variables length field for that data. Redcord id (rid) is the internal indetifier of a record <page id, slot#>.
/ If we do not have enough space in a page we may have to move the information to another page. However we do not change the record idea, that brings us to an area on the page that was the same as before but now we have the new page and slot number of the data there. Note that this means the the minimum alocated space on a page must be at least the size of record id. 

## A relation in file
Pages hold records of one relation.
* Insert record (return rid)

### Heap implementation
Each record is added we add it to the heap, each pages contains 2 pointers plus data to allow for easy removal. 

### Sorted file
Record are sorted by 1 of the attribute. Each page contains 2 pointes plus data as before but the structure looks more like a list.

# Indexing
## Cost Model for Execution
* What is the cost of the request depending on the different implementations.
    * Number of IO
    * CPU Cost 
    * Network Cost 
* Assumption
    * IO >>> CPU 
* Simplification
    * Ony consider disk read

## Typical operations
``` SQL
SELECT * FROM students
```
Point query
```SQL
SELECT * FROM students WHERE sid =100
```
Equality Query
```SQL
SELECT * FROM students WHERE sid>120
```
Range Query
``` SQL
SELECT * FROM students WHERE 20>sid >120
```
Insert Query
``` SQL
INSERT INTO students VALUES (...)
```
Delete query
``` SQL
DELETE FROM students WHERE 20>sid >120
```
Update -> insert,delete

## File organization
With n pages
* Heap file
    * Select * we must read n pages
    * Unique attribute we must read on average n/2
    * range search we must read n pages
* Sort file
    * Select * we must read n pages
    * Unique attribute use binary search log(n)
    * rang search we must read, low range so log(n)