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