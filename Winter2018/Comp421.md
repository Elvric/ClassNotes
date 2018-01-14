# Comp421
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
> Described as a set of attributes in an object that differentiate it from other objects. 

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


