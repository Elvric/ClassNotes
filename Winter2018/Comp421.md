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
