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