# COMP 361D1 Software Engineering Project
----
# Lecture 1 05/09/2018
## Info
In march we will have new requierements because this is real life. To force us to come up with a more flexible design.  
Base camp will be used as a team management tool.  
Office hours: M 11:30 - 13:00  
Text book: Software Engineering: Princplies and Practice 3rd Edition. Wiley, 2008.
The support will be in Java with Minueto as the librairy. 

## Grading
* 3% User interface sketch
* 5% for the requierement elication document
* 5% user interface demo
* 10% for the requierment specification decument
* 10% the design document
* 10% for the demo
* 22% acceptance test
* Exam (individual)
    * 20% Exam on requierment modelling 
    * 15% Exam and design

### Game
This game will be flash point that must work with 3 players playing on separate machines.

---
# Lecture 2 07/09/2018
## Game conditions
* User interface
* Loadable and random game boards
* Game Lobby
* At least 3 player and max 5 player must play the game
* The game state should be saved and loadable
* A chat must be used
* Different difficulty levels
* The roles must be split either by choice or randomely
* The game must be responsive and fast
* Sound graphic and music can be from anywhere (GUI) do what we want

## Sketch (1st October)
Prepare a sketch (pdf, handwritten) of the main screens of your application. That should allow the player to triggger all functionality described in the requierment document.
- Interaction with game lobby
- How does the player see the game state 
- Perform action
- Throw the dice
- Communicate with other players
- Loading saving functions accessed

---
# Lecture 3 12/09/2018
See lecture 2 in COMP533

## Modeling
Focus on what is essential. We create models to represent ideas in an easier way (financial, safer and simpler).

### Fondue
Look at the slide it describes the thecnique that we are going to use to develop the software we are going to folow the waterfall system implementing fondue.

---
# Lecture 4 14/09/2018
## Requierments Elicitation with use cases
2 models to be build, the domain model (UML class Diagram) <--> Use case Mode (UML cuse case diagram + textual template). Start with either one or do them at the same time

## Use case model
Discover and document the functional requierments of the deisred system such that all the participant of the project can understand and relates to the motivation of the system (business vision). It must be complete, consistent and verifiable.

### Use cases
Capture the interactions betweent eh system and the environment to achieve user goals. who (actor) does what (interaction) with the system for what purpose (goal) without dealing with the system internally. A complete set of use cases specifies all the different ways to achieve a goal or not (what could go wrong) with the system defining all the behiavior required for the system.

Must be a black box, stay focus on the interaction finding the what rather than how. A use case force use to look at normal and exceptional behavior. 

#### Ex
Use provides the system with user name and passoword, user is redirected to menue page ect...  
It is important to mention the imput and output but no details should be mention like the passowrd is encripted in 32bits.

### Actors
The actor represent who interact with the system, the role an extarnal entity plays when interacting with the system with a specific goal in mind.  
An actor represents a role. A role is defined by the sets of characterirists needs, interests, expectations, behavior and responsabilities. An actor can play different roles at the same time.

#### Actor communication with the system
Communicates by sending and receiving messages to/from the system under development. A use case is not limited to a single actor.

#### Finding actors
How interacts with the system directly. Will the system need to interact with other systems, any hardware linked to interact with the system are there any reporting or system administrative interface.

#### Categories
__Primary actors__: actor with a goal on the system, obtain value on the system, may interact with the system with facilitator actors. Flash point player that wants to login

__Secondary actors__: actor which the system has a goal towards. In order to achieve a goal to a primary actor.  
Opening a bank account once set up the system needs to print out the information of the bank account, the printer will be asked to print the statment hence the printer will be the secondary actor. Very often the system depends on secondary actors to achieve the goal. 

Most use cases are basically how primary actor asks for something and how the system with the secondary actors respond to it. 

__Facilitator Actor__: Used by primary actor tor secondary actor to comminicate with the system. Open an account, call on the phone to open an account, then the person on the phone fills in the information so the facilitator actor is the emplyee and the primary actor is the caller.

### System boundaries
The separation between the system and its environment __he will be picky about where we put the boundaries__. Use case describe how the outside talk with the inside. It is quite common that stakeholders conflicts arise when assuming different boundaries.

### Use case structure
Usually they are textual description, written in a template (fondue) that we will be given. it includes
- How the use case starts and end
- The context of the use case
- The actores that interacts with the system
- All circonstences in which the primary actor's goal is reach or not reached
- What information is exchanged

1. Use case Name
2. Scope (name of the system)
3. Level
4. Intention
5. Multiplicity
6. Primary actor
    1. Secondary actor
7. Main sucess Scenario
    1. Sequence of interaction step
8. Extensions & Exception Scenario
    1. Additional/Alternative Interaction Steps

### Interaction steps
Refer to a lower level use case, it must always contain the word system and at least an actor and input interaction or output interaction. It may be used as well to describe an optional systenm processing step or communication happening in the environment. 

The __.__ notation , __3.1__ denotes sequential sub steps  
Letters, __3a__ denote alternatives to a step  
__||__ symbol denotes parallelism

### Levels
__Summary level__: A large grain the encompasses multiple lower-level, user goal use cases,   
__User Goal level__: done by one actor, in one place, at a time the actor can go away as soon as the goal is achieved, a single discrete, complete, meaningful and well defined tasks  
__Subfunctinal level__: Execution support for user goal level cases,they are low level and need to be justified, reuse or necessary detail, it is ofter not clear who is the primary actor of a subfonction.

## Use cases in UML
Dash ----> (include) Arrows to show the reference to a lower level use case, actores are shown as humans with a number next to them to show the number of instances of actors and bubbles to show a use case. There is a square for the system boundary.

High level use case can have subclasses such as placing an order involves supply, order products, arrange payment.

White arrow subsystem-|>User to show different subsystem calls from one use case depending on the imput given.

<\<extends>> adds know interaction new interaction step to the base use case in case of a specific input like ordering a boat the user asks to recieves the boat manual. Generally we do not use extends that much. 

# 19/09/2018
## Object orientation overview
See the 533 notes most likely.

# 21/08/2018
## Association
In UML we just draw a full name with a black arrow pointing to the object that contain the other in a way I think?

---
# 26/09/2018
### Gas station use case
Secondary actor: fuel gun

1. Driver insert its credit car in the system
2. System sends credit card details to the backend
3. System checks credit card details and verify that the balance is high enough.
4. The system unlocks the fuel gun
5. Fuel gun inform system that fuel is ready to be dispensed
6. System tells the pump to pump
7. The fuel guns inform the system that a certain amount has been dispenced  
   *Step 4 to 6 are repeated until driver stops or fuel gun detects overflow*
8. Fuel gun informs the system that it is back to the holster
9.  System locks the fuelgun
10. System informs back end of fuel quantity dispensed
11. Back ends debit the amount on the card
12. System request credit card reader to return the card to the user 

#### Extensions
3a. Credit card back end informs system that credit card is not valid.
3a.1 System request credit card reader to eject card

**VOIR LES NOTES DE 533 DU MEME JOUR**

---
# 28/09/2018
Questions:
Explain every buttons?  
Template game board is it alright?  
Template set in stone?  
Number of games that are possible to be played?

SEE the notes 533
## Exercise with the company
![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/CompanyDiag.png
"Pic")

## Reinfication
The process of making an object or a class. It is important because some entities sudenly gets an identity.

A flight is a reification of an aircraft scheduled on some route on some weekday. A person owns a car. 

---
# 03/10/2018
Librairy example in class
![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/librairy.png
"Pic")

# Requirements specification phase
Produce a complete, consistent and unambiguous description of problem domains and functional requirements of the system.

Models are produced which describe the environment model and the concept model.

#### Environment
describe the interface and the boundaries of the system with operations (communication diagram)

#### domain
Looks at the static structure of the system.

## Environment model
Defines the system interface. It shows a black-box view of the system. Determining how the system functionality is mapped onto individual operations. Designed based on the domain and use case models. Technical information a requirde for the quantity of information data sent to and away from the system (traffic).

The system is modelled as a reactive entity that interacts with other active, reactive and passive entities called actors. (system is just a name for the analysed actor, environment is the set of all actors that interact with the system).

Communication is done through input messages and output messages. Input is always sent by an actor nevor by an object in the system.

__A message__ is an instantaneous and atomic unit of communication between the system and the environment. The communication is asyncronous (the sender does not wait for the message instance to be received). The sender may supply parameters (values, objects) with the message instance.

The mesage arrives at the system, triggering an event that itself triggers a system operation.

__The drawing is called a UML communication diagram__
Time triggered events are embedid in the rectangle with <<>> quotes

### System operations
Process a input event, change of the system state and the output of messages. the effect associated with an imput event is called a system operations. A system operation is performed instentaneously.

### Environment model
The set of input messages the system can recieve. 