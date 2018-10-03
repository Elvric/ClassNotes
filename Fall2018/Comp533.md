# COMP 533 Model Driven Software Developement
---
# Lecture 1 05/09/2018
## Grading
2 Assigments 30% groups of 3 different groups
Project in 2 parts 40% Group of 4 different groups  
Midterm 30%

---
# Lecture 2 10/09/2018
## Well defined process
* Provides guidance as to the order of team activities.
* Speficifes what artifacts to develop
* Directs the tasks of individual developers
* Offers criteria for monitoring and measuring project's product and activities

## Devlopement models
### Waterfall
1. Requierments Elicitation (Why?)
2. Specification and analysis (What needs to be developed)
3. Architecture design (How) elaborate the architecture that fulfills the requierments
4. Details design (How exactly) Allocate responsibilities to modules, Design the fonctionality code wise
5. Implementation 

### V model
Just like the waterfall model but at each level we include a level of testing going back the other way to ensure that every step has been respected.  
1. Implementation
2. Unit Testing
3. Compenent testing
4. Integration testing
5. System testing

### Iterative model
Overtime things have changed and now all phases are actually mixed together but the amount of effort spent on each one varies overtime.

### Spiral model
Iterative with the quadrants representing some aspect and some specific tasks depending on where we are in the spiral.

## Decomposition
### Vertical decomposition
Top-down or bottom up decomposition present in the waterfall model where we define everything and then create the fonctionality.

### Horizontal decomposition
Within one layer we start with a core fonctinality and then we add to it overtime.

## Modeling
Is the use of something in place of something else for some cognitive purpose that is simpler, safer or cheaper than the reality. A model helps to understand, communicate and build. Tools are of major importance to effectively create, manipulate and transform models.

Every complex system is best approached through a small set of nearly independent views of model.

## Object oriented model
Based on Object (state, behaviour), class, inheritance, polymorphism. Experience has shown that object oriented programming is not enough. The structure is very rigid.

## UML
Unified model Language. Inlcude all the diagrams like structure, sequence, class and so on.

### UML for the class
* Class
* Package
* Use Case
* State 
* Sequence
* Communication
* CORE Models
    * Feature
    * Goal
    * Use 
    * Reuse

## Fondue
### Fondue models
Concept <--> Operational -> environmental <- Protocol

---
# Lecture 3 12/09/2018
## Reuse and Separation of concerns
The process of using existing software artifacts rather than creating them from scratch.

### Classes and Frameworks
Framworks GUI for example is more complex than classes and is a bunch of classes and librairies that work together to create a complex structure.

Components: encapsulate a bunch of object and classes that communicate between each other to create the app.

### Reuse role
__Reusable Unit designer__: knows the specific details of the code, does not know in what contexts and how exactly the concern will be used. 
__Reusable Unit User__: application expert knows the requierments of what is being build but does not know the inner working of the reuse code

These two need to communicate one to provide the fonctionality the other one checks if that fulfills the requierments.

### Reuse criteria 
The code must be well packaged (when we take the code we take all of the code requiered not just a part of it) as generic as possible but adaptable, not depend on any application-specific artifact.

What, what class uses the code, when: when in the class should we use the reusable code. How: how the reuse code will perform the operation.

## Kinds of interface

### Usage interface
How the user can make use of the structure (API) public class, public operations and attribute. How to use the librairy.

### Customization interface
How to adapt reusable unit to the scefic reuse context, provide context specific structure or behaviour by implementing a required interface or subclassing. For example the arraylist \<e\> in java where then when we enter an actual class for e we implement another interface or we extend that interface to implement the method specifically for that class for example sorting.

### Variation interface
User visible features that the reusable unit offers and the impacts that each feature has on non-fonctional properties/qualities. Let say there are 2 varient for implementing a specific interface we let the user choose which one is best for him by exposing the feature and the impact that each version has on properties and qualities of the interface.

## Separation of concerns
Focus on one particular aspect, view, issue abstracted away from the other (temporarly forgetting about them) because then you can simplify the thinking and then reason about how to integrate that with the other concerns.

### Success example
- Encapsulation and information hiding
    - Separation of external view from internal details, details that should not affect other parts of the system are made inaccessible
- Procedures
    - Hide algorithm behind signature
- Objects
    - Hide data and associated behaviour behind interfaces 
- Components
    - Hide collaborating objects behind interfaces 
- Aspects
    - Let say we have 3 classes in a bank, customer, account, money.
    - We may have login concerns in account and customer
    - We may have also multiple concerns in one class regarding login and money transfer in the account class. So concerns are scattered accross multiple classes and also tangled in them (multiple of them in one class)
    - The aspect represent issues in concernes login concern is basically the login aspect and the idea is to separate such aspects and code them in modules (reason about them in issolation). Then we got to compose thes aspects with the class that we are trying to implement
    - __When creating a module we must decide do we use a class or do we use an aspect__

### Separating Reusable code methods
Syntactical (where the code required to implement the needed method is located)  
Logical reuse: The code the needs it -> the code that codes what is needed.

---
# Lecture 4 17/09/2018
## Object
Is a signle individual, identifiable intem unit or entity, either real or conceptual. A property is an inherent or distinctive characteristic trait, quality or feature of an object.

An object is denoted by a name or reference, has attributes (for state) and provides a state of operations (behaviour)

### Graphical repersentation
object name: class name, it must be underlined and minuscule for name : capital letter for class.  
paul : Person  
: Person 

### Properties
It has an identity that encompasses a state that itself defines a behaviour.  
The identity of an object cnanot be changed, it is always possibe to distinguish two objects even if they have the same state. 

### State and behaviour.
The state of an object is its memory (what happened to it over time).  
The behaviour is how the object acts on its own initiative and how it reacts to externam stimuli. (active object: thread, passive object: graphical object recieving a message)  
An object operation are directly responsible for performing behaviours.

### Attributes
Describe the static properties of an object. They hold information about the state of the object can be data value or link to other objects.  
A value is a characteristic that can be measured or is defined by agreement and that has no existence on its own so no identity.  
The attribute of an object remain the same but their values may change.

### Object interactions
An object (client) ask another object (server) to provide a service by sending a message to it.
- Destination (reference to server object)
- Selector (name of method to be performed)
- Parameters (additional information required to perform the service)

#### UML diagram
A line between the client and the server (client --- server) with an arrow on top of the line going from the client to the server, above the arrow we put the name of the service asked  

Syncronous (client waits until server is done to perform the service)
Asyncronous (client sends messages to server and do not wait for each other to finsish by sending messages on both side)

## Operations
### Kind of operations
__Constructor__ (Create, build, initialize object)  
__Observers__ (Retrieve informaton about the state of the object and provide that information to the outside world)  
__Modifier__: alter the state of the object  
__Destructor__: operation that make sure that the object gets erased properly.  
__Iterators__: Access all parts of a composite object, an apply some action to each of the parts.

## Object System
A set of objects that are interacting between each other. The communication between objects is flat (a network). As long as we have the reference to the object then we can call it

## Class
Groups objects in such a way that their similarities are prometed and difference ignored. Like a template/mold rather (I fell like the first sentence can be confusing)

### UML representation of class
Class name  
\----------  
attributes  
\---------  
operations

## Interface and implementation
Give a general access to operations on object that can be implemeted differently but in our case we do not really care about how that implementation is done.

## Subclasses
When a class inherits from another class since they share similar properties but the subclass has some additional features. (generalization-specialization)

### Subclass properties
S is a subclass of T if and only if any instance of T can be substituted by an instance of S without any visible effect. The objects of the subclass have all the attributes and operations of superclass and perhaps other.

### UML diagram
Person <|--- man (use white arrow going from sublcass to superclass). We can then add some information on the arrow like 
- Complete, disjoint 
    - There can be no object that is a person it has to be either a man or a woman. 
    - disjoint mean that a person can only either be a man or a women.
- incomplete, overlapping
    - Athlete can be a object on their own
    - Overlapping means there can be objects that can be of multiple subclass
  
By default it is incomplete and disjoint.

## Object oriented decomposition
Decompose problems by object modules but here we can have problems as all the concerncs cannot nicelly modularise in these class modules such as different classes having similar problems.

## Aspect oriented decomposition
Decompose problems with mechanisms that allow us to have a more flexible moduralization which gives us an addtional tool to identify concern and see them individually.

We reason with the constrains individually and the using the composition rule that defines how they are then combined with the other concerns in the class.

An __aspect__ is scatered when in goes is different module  
A target module is __tangled__ if it is composed of several source module (aspects).  
__CrossCutting__: (X and Y are aspects/ source modules) X crosscut Y iff X is scattered in the target representation and there exists a module in the target where X and Y are tangled

## Asymmetric Asspect Oriented
In this case well defined base element do not and are not allowed to cross cut.
Aspect on one side, source base module on the other side and then we create the target strucure with both of them.

Aspects are woven together at join points Aspect languages (AspectJ, AspectC#, AspcetcC++, AspectC).

## AspectJ
- PointCuts
    - Make it possible to name a set of join points (Method Calls, setting, getting field values)
- Advice
    - Specify behaviour at joinpoints
    - no code but executes when system reaches a join point
- Introduction
    - Add fields, methods to classes
- Aspects
    - Group together pointcutes, advice and introduction

---
# Lecture 5 19/09/2018
## AspectJ continued
Joinpoints are identified using point cut designators.  
__Call joinpoint__: calling a fonction in another function.   
__The execution joinpoint__: happens when in a function we call a fonction on another object.   
__Initialization joinpoint__: creating an instance I guess ? 
__Execution handeling joinpoint__: handler(TypePattern)  
__Field accesses__: get(Signature), set(Signature) used to access an attribute.  
__Object__: Done in realation with objects, any call done in a certain object.

We can add + when we also what to include subtypes, we can also add * when it can be anything. From what I understand so far it is like bash and the aspect syntax is a bit like trying to find a word. When et do call(public * Account.get*(...)) means that we want to intercept all calls that are public with any return type that are getters.

__pointcut__: can be used to create warnings or errors

Example of AspectJ:
```AspectJ
pointcut PublicCallsToAccount(Account a) :
call(public * Account.*(..)) && target(a);

pointcut SettingIntegerFields(int newValue) :
set(* int Account.*) && args(newValue);

around PublicCallesToAccount(Account a) : PublicCallsToAccount(a){
    if (a.blocked) {
        throw new AccountBlockedException();
    }
    else {
        proceed();
    }
}
```

### AspectJ advice
Around: means instead of
Before after (not sure yet) :(

```AspectJ
private boolean Account.blocked = false;
public void Account.block() {
    blocked = true;
}
public void Account.unblock() {
    blocked = false;
}
```
In the code above here we are in an aspect and we add to the class Account implicitly the field blocked and two extra methods that allow the class account to change the value of that new field. This makes sense since the class account is purposly not build to include that aspect. It is the role of the aspect to apply its requierments if I may say to the class therefore the aspect must have the ability to modify the class as if a Dev has adding new methods to the class in a regular java program.

TODO PUT THE CODE GIVEN IN THE SLIDES HERE.
```AspectJ
public aspect BlockableAccouts {
    pointcut PublicCallsToAccount (Account a) {
        call(public * Account.*(..)) && target(a);
    }
}
```

Questions about arrows
About arround
About Iblockable

# 24/09/2018
## Standare Object Oriented domain modeling
Requierments:
- Fonctional requierments
- User expctations
- Non-fonctional requierments

The domain model captures the concepts in the domain of the problem.
## Object during requierment phase
The object can have attributes that we know we are going to need but no set methods/operations. 

## Associations
The glue of objects, they glue them together letting them know what object they are related to. They are represented in UML with a line. The name of the association is on the line. This does not force anything it just says that there maybe a relationship between 2 objects. At runtime there could or could not be instances of that realation.

It is important to view such relation as a pair of instances of objects in our sence. We do not view them as actual java relationship where one store the address of the other and vice versa.

In this design if we have more than 1 objects related to other we can make an association class that holds the relation for the 3 objects.

The cardinality defines how many objects can interact with object. Wrote as such is umle 0..1 or 1...*

Carful for more than binary relationships, the numbers represent for any combination of the object related to the object of interest we must have x...y objects. The key word here is __for any__.

### Association class
Written with a dash line ------ like student______-----Takes_____Test 

Association must have role, by role we mean the title of the object related to each other for each other.

Ex: if we have a person and a company then to the company the person is an __employee__, to the person the company is the **employer**. Thise terms are the roles.

We can also connect a class with itself like person, has parents and this is where these roles are important.

## Agregation (Not really important for this course)
Binary relationship where we want to communicate that there is a hole part relationship. It is transitive and antisymmetric.
- If a is part of b and b is part of c then a is part of c.
- If a is part of b then b is not part of a

Aggregation are writen by the romubs shape white arrow this way
Person __________<>Flight

### Composition
It is a stricter form of agreagation represented by a black rhumbus. for example with have a window and inside that window we have 2 scrol bars. In this case if we destroy the window then we destroy the scrol bars. The multiplicity must be either 1 or 0..1 in terms of the container the containee have no limits.

---
# 26/09/2018
### Constrains
Can be added to the arrow as such {order}. By default it is unordered.

### Association with subsets --->
So we can have two relations ships going both ways with different objects. We can draw an arrow between the two to show that the relation on the left must be a subset of the relation on the right. Does not work for operation that are not of the same number (double vs third)

### Derived association
If a department is a composition of the company and I work for the company then I am also emplotyed by that company, I cannot be employed by any other company. This can be shown by putting this info at the bottom of the graph.

{context: Person  
selt.employer = self.department.company}

### Qualified Association
Show how a relationship can be achieved or retrived like a building with floors to the right of the arrow on the building side we can rignt Building[floorNumber: Integer]______0...1 floor, means that given an integer I can retrieve the floor associated with it.

## Discovering the domain model
Brainstorming looking for physical objects people, places and so on.  
Distingish objects from classes and roles from classes.  
Suppress the redundant, irrelevant, vague classes. Then look at the use cases.

### Association classes vs proper class
The association class can only exists when associated with all of the objects of the assocations. We can then change that as a real class that is in bettwen the objects. Note that both implemetations can be good as sometimes we many prefer one over the other. Everything depends on the implementation.

#### Exercise
![Graph Diag][GraphDiag]

[GraphDiag]: https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/GraphDiag.png

# Domain model constrains
Used to add constrains that cannot be demonstrated with domain models.

---
# 03/10/2018
### Example
Sketch a domain model where a person can work at multiple companies, each person or compagnie can own a car, a bank can give a loan to buy a car.
![Graph Diag][AnswerDo]

[AnswerDom]: https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/GraphDiag.png
