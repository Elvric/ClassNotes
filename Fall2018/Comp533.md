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