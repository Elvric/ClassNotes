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

ADD VICE