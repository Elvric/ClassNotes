# __Comp421__ 
## Intro 08/01/2017

### Marking Scheme
* 3 individual assignments 4% each
* Map-reduce Exercise
* 3 Projects 5% each
* Midterm 10%
* Final 58%

## Entity-Relationship Model 10/01/2018

### Steps in Database and Design
1. Requirement Analysis
    
    Identify the data that needs to be stored
    Identify the operation that need to be executed
    on the data

2. Conceptual Database Design
    
    Semantic data model to develop semantic schema
3. Map semantic schema to relational schema

### Requirement Analysis
    Identify the entities and relationships in the company
    Store the relevant information that we are looking for in the database
    Ensure that all rules and regulation are respected
    Make sure that the operations we want to perform on the database can be done optimally. 

#### Student database
* Student ID
* Name personal address and so on
* Functionality
    * data can be viewed and changed
    * Instructor
        * Can teach a course
        * Can enter grades
    * Course registration
    * Course cancellation

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

    E/R model allows for pictorially description of the data.
        An E/R diagram is a representation of the data model of
        the application
        It should be understandable by non-computer scientist
    Main concepts are entities and relationships
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
Buffer is basically the ram and frames are units of ram which we are going to use to store pages  
Page size and fram side must be the same size. Table of fram# and pageid pairs is maintained so that we can retrieve a page easily in the buffer using a hashmap. 

### Loading a page from the disk
First we have an empty fram in our RAM, if they are all taken we can use another fram, if fram is dirty (page was modified), write it to the disk. \
We choose usualy the least recently used frame to be replaced. 

#### Page Pins
When we load we must have a pin counter to 0. \
After new page is loaded we set the pin counter to 1. \
Counter could be more than 1. \
After we are done with the operations we decrement the counter and set the dirty bit if the page has been modified. \
Hence when we load a new page but there is no space we check that the pin counter of the page we want to remove is at 0 and then check the dirty bit. 

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
Used and free space pointer in order to start writting new data. Then we can write variables length field for that data. Record id (rid) is the internal indetifier of a record <page id, slot#>.
/ If we do not have enough space in a page we may have to move the information to another page. However we do not change the record idea, that brings us to an area on the page that was the same as before but now we have the new page and slot number of the data there. Note that this means the the minimum alocated space on a page must be at least the size of record id. Note that what is store in a page everytime is an entire ensitance of an entity (a row everytime)

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

---
# 19-02-2018

## Indexes
```SQL
CREATE INDEX ind1 ON Studentds(sid);
DROP INDEX ind1;
CREATE UNIQUE INDEX indemail ON Students (email)
CREATE UNIQUE INDEX ind ON Students (sid) INCLUDE (name)
/* 
    Usefull when wanting to just get the name from and sid so it
    reduceses the IO by 1
*/ 
```
### B+ Tree structure used for Indexes
This is used in order to get the sid faster from an index rather than from the entire table. \
Leaf node: store a leafpage consists of data entries value like sid and the rid. (rid tells us the page number and slot where the actal data is located) so it is a tupple per index. Each leafnode are connected between each other in case we want to do a range search. 

Root: has pointer and value as well the pointe points to leaf or another node that is in the range disired. So we look at the value if the value is greater than sid we want then we go to its pointer else we move on. 

#### B+ tree Rules
F= fannout = number of children per nodes
N = # leaf pages\
Insert/Delete at logf(N)\
Keep each node at least 50% full
1. Index entries in root 
2. Data entries in index leaves
3. date records in data pages

**Assume that the root is in memory so we do not need to calculate its IO and intermediate nodes** unless asked \
So assume we have a leaf of 5 data entry and we want to get the last 1 then we would do 2 ios over all because we would fetch in ID 

#### Insertion into the tree
When we add an sid, if it is on the edge of the address we go back to the root and update the previous value of the tupple to that sid./
If at some point we do not have enough space in intermediate node we just push the new value up to the layer above it.

#### Having values that are not unique
Put these values in the data entry and this is linked to a list of pointers that point to record with the same value.

### Direct indexing
Instead of instead of storing just the address of the record we store the entire record there. 

## Index classificiation
#### Primary vs Secondary
* Primary index created on the primary key
    * Always unique
* Secondary index done on other attributes
    * Must mention it unique to make it unique 

#### Cluster vs uncluster
* Cluster means that the data file is also sorted in the same way than the root is, no cross over. So when we limit the number of pages we have to read from therefore less cost. **There can only exist on cluster data file per data file.**
* Uncluster when fetching data the arrows cross over. So we must read more pages which is more costly.

---
# 21-02-2018   
## B+ Tries capacities
* Height
    1. 17689 records
    2. 2352637 records
    3. 312900721 records
* Levels
    1. 1 pages 4 Kbytes
    2. 133 pages 0.5 Mbyte
    3. 17689 pages 70 Mbyte

## Multi-attribute index
Index on Skaters(age, rating). Here it will sort the index on age first then on rating in the index. 

## Static Hashing
Hash the record based on its sid for example to get a pages then we store that data entry in that page. Multiple sid can map to the same page but this is not a problem, if the page becomes full then we add a page and the previous page will point to the new page.

Issues: Some pages maybe half empty, if the Hash function is not that good we can get a lot of records to the same pages. So we must find a balance between too little and too many pages.

## File organization
For hashing we cannot do range queries. 

# Query evaluation

### Processing SQL queries
Parser translatees into query representation. \
Checking syntax, checks existence of relations and attributes.
The query optimizer translates the query into an efficient execution plan. 

## Query decomposition
* Operators
    * Selection
    * Projection
    * Order by
    * Join
    * Group by
    * Intersection
    * … 

Several alternative algorithms for implementing each relation operator. One could be better in certain situations others are not.

#### Access path is the method used to retrieve a set of tupples from a relation.
* Basice file scan
* Index plus matching selection
* Partition
    * Store data in multiple files. Used so that we can store all the data enter on a specific date on one page and so on. 

## Cost model
We must be able to estimate how expensive a specific execution is:
* Input cost model
    * the query
    * database statistics (distribution, values)
    * Resource availablility and costs: buffer size, I/O delay, disk bandwidth, CPU costs, network bandwidth.
* Our cost model:
    * I/O = page retrieve from disk
        * Root and intermediate nodes are in main memory
        * A page P may be retrieve several times if many pages are accessed between 2 P accesses.

### Basic query processing operations
* Selection sigma
* Projection pie
* Sorting
* Joining
* Grouping and duplicate elimination

## Concatenation of Operators
Execution tree: shows us the order in which each operations in the query are going to be executed.

## Reduction factor
The number of records that will make it to the output. |
CARD(X) gives us the number of outputs. \
We can also make assumptions let say we know a query for a certain number represents 5% of the data we can assume that any other number in the range given will have the same reduction factor. The range can be determined if the data base keeps track of the max and min values entered. 

#### Reduction factor is the ratio of output/input

---
# 26-02-2018
## Execution plan 
```SQL
SELECT P.cd, count(*), AVG(S.rating)
FROM Skaters S, Participates P 
where P.sid = S.sid AND S.age < 10
GROUP BY P.cid
HAVING AVG(S.rating) > 5 ;
```
1. Selecting of age
2. Join the two tables
3. Scan and select the column we want
4. Group them
5. Calculate the average and count

### Reduction factor assumptions
```SQL
SELECT * 
FROM Users
WHERE uname like 'B%'

SELECT * 
FROM Users 
WHERE uid = 123
```
First we will read every record hence we will have to read all the pages or just 1 so on average we will read half of the pages. \
**Using a cluster B+ Tree for the first one:** Reach the leaf page that is one I\O then read the names then just start reading the names cost 1 I/O per pages that match the requierments. Since the number of tupples that qualify is 20% then we can assume that on average we will retrieve about 20% of the pages. 

For the second we know that there is only one hence we can use indexed data and our reading time would be the number of leaf pages + certain number of data pages that have matching tupples.\
**Using a cluster B+ Tree**: one leaf page one data page then the I/O cost is 2.

### Uncluster B+ Tree for fetching data
One I/O for the first leaf pages then retrieve the page but in this case we may have to count the same pages twice since we may call it after a long time since we last called it. Could go all the way to one I/O per data record worst case. This would be the same cost as cluster index if we only have 1 matching record howerver when we have multiple ones then we can have a huge increase in I/O.

#### How to improve that:
Go to the leaf entry sorted based on what we are looking for. Then sort these entries according to their record identifier ensuring that we visit each page only once. So in an example we would have to read 75 leaf pages sort them (rid=pid,slot-id) in leaf pages by page-id. **This is only fast if the leaf pages can be stored in memory** worst case we read all the pages but at least we call these pages only once. **We must also ensure that this is faster than a simple scan**. \

If reduction factor is low then using the index is usually better if the reduction factor is high then just scanning each pages may be better. 

#### Example
A = 100 AND B = 50 (500 pages)
* No index
    * 500 I/O
* Index On A attributes
    * Get all the A=100 and check if B=50
* Index for A and Index for B
    * Go to both indexes and compare the page ids (intersection)

A = 100 and B<50 (Red of B<50 = 0.5)
* If A has a low reduction factor use the A index else use the B index. 

A = 100 OR B = 50
* No index
    * 500 I/O
* Index on A
    * 500 I/O
* 2 Index on A or B
    * Use the indexes for both and just do a UNION.
* 1 Index for Both attributes (A,B)
    * Just scan the 500 I/O
    * Better than index as we do not have to read the leaf pages.

---
# 28-02-2018 (Not in midterm)
## External sorting
Each set of sorting is called a run. Passes is also a new term pass 0,1,2 is different from other passes.

Assume we have 2 entries per pages, 5 pages so 10 entries and 3 buffer frames in memory so not enough for all the pages.
1. Read the first 3 pages in memory
2. It sorts them and copies them in disk memory 
3. Takes the next pages sort them in RAM and store them in disk memory
4. Here we have sorted the 5 pages so pass 0 is now done
5. Then we Merge Sort the **runs** of pass 0. This is done by loading the first page of the 2 runs and look at the records in them and write the lowest ones to a new page and store that new page in main memory.

Somtimes pass 0 created more runs than there are main memory buffers. So we will need at least 2 extra passes to finish. So total cost is 2N *(# of passes) N is the number of pages

### I/O costs of query
```SQL
Select uid,experience from Users
Order by experience 
```
1. Order by experience
2. Print the new table.

If the buffer is larger than the number of pages then only pass 0 needed. **Note that the output of a pass does not always go to the disk, if it is the last merge sort the output can be sent directly to the client saving 1 I/O**

### Sort in real life
Blocked I/O use more ouptut pages and flush to consecutive blocks on disk, may lead to more I/O but each I/O is cheaper. Write after the first pass only the columns needed so we can store more tupples per pages.

## Equality Joins
```SQL
select * 
from Users U, GroupMembers GM
where U.id = Gm.uid
```
### Join cardinality estimation
Look at the table with the most records that matches that is the cardinality.

For cross product we get:|A|*|B|

If we fiter for example and remove 1/2 of the data that way then we can assume that the join reduction factor will also be reduced by 1/2.

### Simple Nested Loop Join
We lable 1 table as inner relation and the other as the outer relation. Take one record from the outer relation and compare it with entries in the inner relation.
```
foreach tuple u in U do
    foreach tuple g in GM do
        if u.uid == g.id then add<u,g> to results
```
Cost: UserPages + |Users| * GroupMemberPages

### Page Nested Loop
Read a page and join all the records from that pages to the entire other table. \
Cost: UserPages + UserPages * GroupMemberPages

### Block Nested Loop Join
load as many pages from the outer table as possible and the inner table only has 1 slot for 1 page. **Note rember that a block represent a section made of a number of pages**
Cost: UserPages + UserPages/#buffer-1 * group member pages.

### Index Nested Loop Join
```
foreach tuple u in U do
    find all matching tuples g in GM through index
        then add all<u,g> to result
```
Use the index of the inner loop in order to find the record that matches. In this case we read the leaf page if we need more info the we go to the page itself else we do not care. \
CardOuterRelation = # of Users\
Cost: OuterPages + Card(OuterRelations)*cost of finding this depends if the GM is clustered or not clustered.

### Block Nested Loop vs Index
Best case nested loop: OuterPages + InnerPages
Index nested loop is bettter if Innerpages > Card(Outer) * matching tuples Inner. Basically the fewer tuples are actually selected the better it will be.

### Sort Merge Join
Assume that we have 2 sorted tables available. We read one of each pages and just compare them when we find the next ones and so one. Cost: UserPages + GroupMemberPages.

---
# 12-02-2018
## Hash Join 
Can be used only when we use join equality (greater than and less than fails)  

h has a value between 1 and B-1 if there are B buffer frames.
- h(u.uid) = i -> tuple u of User is in Userpartition i
- h(g.guid) = i -> tuple g of GroupMember

```
For each page of Users U
    For each tuple u in page do 
        append u to UserPartition h(u.id)
```

So when we load the user partions we load the entire partition into memory or as much as we can leaving just 1 free buffer space to load at least 1 GroupMember partion and start the joining process. Hence each partition must not be bigger than B-2.

#### Note that at the end of the hashing process we have 2 copy of the data in the disk. The copy made from hash is discarted at the end of the query

### Cost

1. Hashing IO of 2*numpagestotal
    - Read all user and GroupMember pages 
    - Hash them in buffer
    - Write them back to the disk
2. Query IO of 1* numpages
    - Load user partitions
    - Load GroupMember partition page by page

Total I\O cost is 3*numpagestotal.

## Merge-Join vs Hash-join
- Hash join better
    - If one relation is really large
    - sort in merge-join might need another pass
- Merge join better
    - if hash partition of smaller do not fit into memory as we want the hash partion to be max size B-2 which may be hard to achieve. 
- Merge Join result is sorted
- Hahs join good for parallelization
    - Let each pair of partition be matched on a different node.
    - One processor works on partition 1 and the second processor will work on partition 2. Here assume that the length of each partition has been made so that the two processors can work together in RAM.

## CPU optimization
Come up with a second hashing algorithm on the same attribute in such a way that it will make the MemberTable page map to only 1 page in the partition of users. We do that in buffer however this does not save an I/O cost.

## The projection operations
```SQL
select GM.uid, G.gid 
from GroupMembers GM
```
When we join or perform operations we project on the fly to minimize I/O queries.  
Can be complex such as 
```SQL
select distinct uid
from
```
Data base uses distinct only when really necessary. 

## Set operations
- Intersection and cross-product cases of join.
- Union(Distinct) and Except acts similarly in terms of operations. 

## Aggregat Operations (Avg, MIN, ect)
Usually done at the final stage (last step) 
- without grouping
    - Requires scanning the relation so full table scan
- grouping
    - Sort on group-by attributes then scan relation and compute aggregate for each group. (can improve by combining sorting and aggredgate computation)

---
# 19-03-2018
## Projection operation review
- Usually done on the fly together with another operation (or piplined) 
- Can be complex SELECT DISTINCT
    - Expensive operation
    - Requires sort in order to eliminate duplicates
    - Often done at the very end or whenever the relation is sorted for some other reason
    - Use distinct only when necessary. 

## Execution plan
- describes how a query is executed
- A Tree (sequence) of basic operators (select,join,project,sort) use to process the query
- For each operator, an indication of how it will be executed (index nested loop, sort, index, simple scan)

### Example
```SQL
SELECT GM.gid, count(*), Avg(U.age)
FROM Users U, GroupMembers GM
Where U.uid = Gm.uid AND U.experience=10
GROUP BY GM.gid
HAVING AVG(U.age) < 25
```
1. Selection experience = 10 for Users
2. Join that with GM 
3. Project to keep gid,age (scan)
4. Group the gid sort
5. count and average by (scaning)
6. select Avg(U.age) < 25 (scan)

### Pipelining
Fill a buffer frame and the start join rather than writtin it back to the disk. Write to a buffer output that when is full pass it to the next task.  
Piplining cannot be done when sorting as we need all the records in order to sort we cannot work small parts by small parts.

### Join with further restrictions
```SQL
SELECT *
FROM Users U, GroupMembers Gm
WHERE U.uid=Gm.uid
AND experience = 10
```
Assume 1% have experience = 10, B means buffer -2 because 1 for user pages 1 for ouputs.
- Do selection before join
- Stream (pipeline) qualifying tuples into join
- Block nested loop (get qualifiying tuples until block is full)
    - UserPages + UsegePages * 0.01/(B-2) * GMpages = 500 +1* 1000=1500.
- Index nested loop
    - UserPages + Card(Users) * 0.01 * Cost finding GM = 500 + 400 * (1+2.5) = 1900
- Sort-Merge Join
    - Pipelining cannot be done as sort needed; therefore intermediate realtion
    - But in particular case: result of selection fits into main memory;
    - Cost: UserPages + 3 * GroupPages = 3500
- Hash Join
    - As Users after slections fits into main memory, it becomes kind of hash join with one partition
    - No need to partition GM and Hash join becomes indentifical to Block Nested Loop. 1000 + 500 = 1500

### Optimization technic
1. Algebraic
    - Use simple rules to perform those operations first that eliminate a lot of tuples
        - Push down selection and projection
    - Do not yet consider HOW to execute each operator
    - Consider the number of tuples that flow from one operatior to the next
    - Key issues: statistics. 
2. Cost-Based optimization
    - Consider a set of alternative plans created by algebraic optmization

### Push Projections
- Pushing dowin these will not reduce the number of tuples but the size of each tuples.

#### Cost Based optimization
- Find a plan with low cost
- Dynamic programming in bottom-up fashion for deep join plans:
    - Pass1: Find best I-relation plan for each relation
    - Pass2: Find best way to join result of each I-relatin plan (as outer) 
    - PassN


### System Tuning
- If your application has some standard, well-known queries: create appropriate indeces to speed up these queries.
- Many updates and inputs
    - Each insert will add new tupples
    - Update can change some indices

### Statistics
Can use runstats to get information on number of pages per tables, number of tuples

---
# 21-03-2018
# Large scale data processing

## Why
- Hardware
    - CPU speed does not increase
    - Instead multicore
- Commodity clusters
    - Easy access to 1000 of nodes (multiple processors and servers) through cloud computing
    - Much cheaper than large mainframe
- Big Data
    - Anstronomy high resolution, high-frequency sky serveys
    - Medicine: digital records, MRI, ultrasound
    - Biology: sequencing data
    - User behavoir data: click streams, search logs…
        - Google, Facebook, Walmart

## Distribution and performance
- Scale up
    - Improve performance by buying a larger machine.
- Distribution scale-out
    - Improve performance through parallel executions
- Performance metrics:
    - Throughput: transcations/queries per time unit
        - The higher the better
        - Important for OLTP (Online transaction processing) no need to know for exams.
    - Response time: time for execution of individual query
        - The smaller the better
        - Important for OLAP (analytical processing)

## Speedup
The data size remains the same but:
- More nodes -> more throughput and/or lower response time
    - Non-linear Speedup Startup costs
        - Coordination costs
        - Communication costs
        - Skew (equal distribution of load not possible)

## Scaleup 
Data increases
- more nodes -> have same throughput / same response time despite more data.
    - Non-linear Speedup and scaleup
        - Data distribution overhead
        - Non-parallelizable operations
    - Communication costs
    - Skew

## Parallel Relational Database Systems
A lot of that technology was developed in the 90s.
- Good understanding of distributed execution of relational algebra queries
- Both for
    - OLTP: workload of short, update intensive queries, such as day to day banking
    - OLAP (analytical processing) Decision support: workload of complex queries, mainly read-only
- Sophisticated and optimized operators (such as distributed join operators hash)
- Expensive and specialized: Oracle, Teradata 


## Parallel query evaluation
- Inter-quert parallelism
    - Different queries run in parallel on different processors; each query is executed sequentially
- Inter-operator parallelism
    - Different operators within same execution tree run on different processor
    - Pipelining leads to parallelism (give output to next operator as it is being created)
- Intra-operator parallelism
    - Given operator can be executed in parallel 
    - Hash join

### Horizontal Data Partitioning
- Data
    - Large table R(K,A,B,C)
    - Key-values store KV(K,V) (other data model)
        - Every record can have only two things a key and a value. Equivalent of Hashtable for database
- Goal 
    - partition records in chunk stored at different node 
- Hash partitioned on attribute X:
    - Record r goes to chunk i, according to hash function
- Range partitioned on attribute X:
    - Rpartion range of X intos v1<v2<v3
    - Record i goes to range v2 if v1<vi<v3

#### Parallel operator
- Execution path
    - Push selections to nodes with chunks
    - Execute locally at each node
    - Send result to coordinating node
    - Assemble result and return to user

### Vertical data partitioning
- Column stores
- Data relations R(K,A,B,C,D,E)
    - Partitioned into RAB(K,A,B), RCDE(K,C,D,E)
- Query
    - Select A from R where B>50 hence we can go to just the first partition with less data so less number of pages to read
- But if we want to get A and C then we have to do a join on both table. Hence only efficient if we are interested in just certain data types

### Map reduce
General purpose distributed computing framework. It can be applied to many type of queries one direction within the NoSQL movement

## Data Processing at Massive Scale
Get the data even with crashes of nodes so we do not have to start again

### Distributed Large-Scale File Systems
Done by Google, Yahoo  
- Assumptions
    - Files are large (terabytes)
    - Files are rarely updated
- Main concepts
    - Split into big chunks 64 mb to track each block more easily.
    - Each chunk replicated for availability
    - Master node knows about location of chunks
        - Meta-repository (also replicated for fault-tolerance)

### Map-reduce again (each taks given a key and a value)
1. Map Tasks: extract something interesting from records and output a new set of data records (key-value pairs)
2. Shuffle and reduce step
    - Transform all the key value pair with the same key to a key value list pair
3. Reduce tasks: aggregate, summarize, filter (key and values input and output) get the output we want.

#### Phase details
- Record reader
- Map function
- Combine 
- Write to locale file

### Combine
Is done at the mapper site so that the outputs can do the combine on its output to sumarize the data a bit more so that the suffling and sorting gets easier for the next step.

### Failures
Detected by master node.
- Master assign new worker if a node crashes
- Straggler all the cpu are being used and our last tasks are done except one node. In this case we start a duplicate task to do the exact same thing, if the duplicate tasks terminate before the original we kill the original task and use the duplicate one reverse otherwise.

---
# 26-03-2018
Good lecture for mapper to look online

## Selection with map reduce
```SQL
SELECT * FROM Users WHERE experience = 10
```
key = uid and value = rest of the columns
- Map: key and value pair check if experience = 10 if yes goes to the output else does not make it.
- Reduce: does nothing.

## Join with map reduce
Note that the suffle and sort is fixed by the framework
Natural Join R(A,B,C) with Q(C,D,E).
```SQL
SELECT * FROM R, Q WHERE R.C = Q.C
```
- Map:
    - For each tupple (a,b,c) of R, output (c,(R(a,b)))
    - For each tupple (c,d,e) of Q, output (c,(Q(d,e)))
- Reduce:
    - (c,value-list)
    - value-list  = (R,(a1,b1),(Q,(d1,e1))
    - Then produce all the combinations as the output.

## Projection with Map/reduce
- Map: take what we want to project as the key and a dummy value.
- Reduce: Just output the key and a dummy variable.

## Group by Map/reduce
- Map: key is the grouping column and the values are the values that will be grouped
- Reduce: perform some aggregation on the values and outputs the key and that agregation result.

# Declarative Languages
- Map-Reduce frakework hides scheduling and parallelization

## Pig Latin
A higher SQL like language to run complex queries that require several map jobs. 

```Apache
Users = load 'users'; as (name,ages);
Fltrd = filter Users by age >= 18 and age <= 25;
Pages = load 'pages' as (uname,url);
Jnd = join Fltrd by name,Pages by uname;
Grpd = group Jnd by url;
Smmd = foreach Grpd generate ($0), count($1) as clicks
# $0 first attribute
Srtd = order Smmd by clicks desc;
Top5 = limit Strd 5;
store Top5 into 'top5sites'
```

### Load and Store
Users = load 'users' as (name,age);
Load: read information from file into a (temp) relation
- Mostly user defined to translate file format into a relational format.

Store: write relation into file
- Only then will it execute the execution scripts each variable actually just store what it should do.

### Group by
Grpd = group Rel by A  
Result realtion is Grpd(group,Rel) which looks like this:  
(a1,{(a1,b1,c1)},({a1,b2,c2})) and so on.

### For each
Used to project the columns that we want:  
Smmd = for each Grpd generate (\$0),count(\$1) as c;  
Rel = for each R1 generate A,B;

## Data Model and Flattening
- Supported types:
    - Atomic(string,number)
    - Tuple(58,'lilly')
    - Multiset {(58,'lilly),33,'Debby'}
- Flatten
    - Res = foreach R generate \$0,flatten(\$1)
        - Unravel the nesting from (1,(2,3)) to (1,2,3)

### Implementation
Executes only when storing or giving a string output. Before running it creates an execution tree, not as good as the regular database, then it will execute multiple map reduce jobs behind the scene.

---
# 28-03-2018

# Transactions
## Money transfer
```SQL
Transfer(id1,id2,value)
{
    SELECT balance into :bal
    FROM account
    WHERE accountid = :id1 

    bal += value

    UPDATE accounts
    SET balance = :bal
    WHERE accountid =:id1

    SELECT balance into :bal
    FROM account
    WHERE accountid = :id2

    bal -= value

    UPDATE accounts
    SET balance = :bal
    WHERE accountid =:id2
}
```
This can be broken in many ways.

## Electronic transaction
An electronic transaction encapsulates operations that belong logically together. Boundaries of the transaction have to be defined by the programmer. It contains DBS operations and application code. This is used for example with purchase elements in shopping cart of online e-commerce side, flight reservation.

## Application vs DBS
A user's program interact with the database (insert,delete,update)  
A database transaction is the DBMS's abstract vuew of a user program:  
- r(X) read
    - Brings things into main memory same as copy value inot variables
- w(X) write
    - bring object into main memory and modify it
- Objects(X) (tuple, relations)

## Online transactions processing (OLTP)
- Pre-defined application tasks of reasonable size
    - 4-20 SQL operations
    - simple 1-4 tables involved
    - Update intensive 15 to 50% of SQL statements are updates
- Volumes up to hundreds/thousands of transaction per minute.

## ACID Properties
The success of transaction was due to well-defined properties that are provided by the database system:
- **ATOMICITY** executed entierly or not at all.
- CONSISTENCY must preserve the consistency of the data (not in bold because it cannot be garented by the database alone)
- **ISOLATION** in case that transactions are executed concurrently: the effect must be the same as if each transaction were the only one in the system
- **DURABILITY** the changes made by a transaction must be permanent (= they must not be lost in case of failures)

programmer only indicates:  
- when transaction start
- The sequence of sql operations
- when the transaction finishes.

### ATOMICITY
A transaction T might commit after completing all its actions. A transactin might get aborted in this case DBMS undo all modifinactions so far the data base state is as if the transaction never occured. After the notification the user knows that non of the transaction's modifications is reflected in the database.

#### Local recovery
`Eliminating partial results in case of abort`  
- before w(x)
    - store a `before image` of x (somewhere in main memory into the log )
    - if abort comes up it will restore the previous state using the before image.

The recovery manager is the program that resets the before image.

### DURABILITY
There must be a garantee that the changes introduced by a transaction will last and survive any failures.

## Recovery after System Crash
Restart server and perform **global recovery**  
- committed before crash
    - in database
- aborted
    - not in database
- active 
    - not in database

### Update vs logs
Each update changes the record on the corresponding page. Dirty pages are written to the disk only when the buffer manager decides. Writting information to log disk is faster that random writes to standard disks. 

## WAL write ahead logging
for each w(x) we record
- before-image: you can undo changes of active transactions
- after-image: you can redo changes of committed transactions.

### UNDO/REDO recoverty
For the ones that did not commit look at the log and get the before image, for the one that did commit go to the log file and use the after image.  
- How do we know which transactions commited
    - Write commit/abort log too.  
- How do I avoid redoing all committed transactions since system starts?
    - checkpointing
    - write out dirty pages on a regulare basis during normal processing to reduce redo phases.

# Isolation and Concurrency control
## Isolation
- DBMS can execute transactions concurrently
    - expoloitation of resources
        - One uses the IO the other the CPU
        - Exploiting multi-core
- Isolation
    - although transactions execute concurrently, each transaction runs in isolation not affected by the actions of other transactions. 
- Enforced by concurrency control protocol.
- Concurency control provides serializable executions
    - Identical as doing all transactions one after the other.