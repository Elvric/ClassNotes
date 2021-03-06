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

---
# 10/10/2018
Graph domain model
![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/graphdiagbetter.png
"Pic")

### Communication
Sometime it is hard to know from where a message was sent and to whom the response must be delivered. One way to solve this is to send the indentity of the sender as a parameter to the message. This information can also be used to send the message back.

When there is an operation that require an interaction between 2 actors then it becomes more complicated. Such as a manager than needs to interact with a system that will then send some messages to the clients. This means that in the system, there must be some information about the procedure through which we can contact clients.

## Iterative development
When creating environment it usually starts with high level abstractions such as the bank sends a  message to the clients. But overtime it could get more complicated with the bank having to print the letters and then send them to a postal service that then sends the message to the clients.

#### Environment model question
Provide declarations for the entities needed to describe the exchange of information by messages between a system and a terminal "actor".
![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/EnvironmentModelTerm.png)

## Concept model
Contains the set of classes and associations modelling the system's conceptual state.

It must contain all conceptual system state needed in order to provide the required system functionality.

### Domain to concept model
The concept model only contains the classes and relationships of the Domain Model required for the project we want to build.

#### Actors
Belongs to the environment, association to connect them to the system corresponds to communication paths between actors. In the concept model there will be one huge class named <<System\>> hand have all the other classes inside that system.

---
# 12/10/2018
Bellow are examples of the model for an elevator
## Environmental model
![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/EnvironmentalModelExample.png)

## Concept model
![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/ConceptExamp.png)


## System operations
Precondition must be true before, postcondition must be true after. It is considered to be a black box no information about intermediate states.

## Operation model
#### Operation
the entity that gives the operation aka name of the system, followed by the name of the operation
and a parameter list and the type of the returned message

#### Scope
All classes and assocaition frm the cinceot model defining the name space of the operation.

#### Messages
The clause declares all hte message types that are outpu by the operation togeth with their desitnations.

#### New
This clauses declares names ofobjects that might be created by this system.

#### Pre
(optional) describe the condition that must be me for the operation to make sense.

#### Post
Description of the effects of the operation on the conceptual system state.

#### Use cases
This clause cross-references to related use case.

---
# 24-10-2018
# Distributed Architecture
## Peer to peer
Identical copies of the software running on different machines that are connected between each other.

### Advantages
All the peers run the same application.  
Main loop for local player:
- Get move from GUI
- Verify correctness
- Apply move to local state
- Send move over the network

Remote player:
- Get move from the network
- Apply move to local state

Only one app to develop, gui has access to the entire game state, if needed, game state is local which increases performance

### Disadvantages
Game must be deterministic (no diversions as the states have to be in sink), Startup has to be assymetric.

## Client server
Computers talk to a server.

Game is split into 2 different applications

Client:
Graphical user, Network interface  
- it is the turn of local player
    - get move from guii
    - send move to the server
- Wait for game state from server

Server:
Keeps the game states, behaviour and interface
- wait for move from the player
- Verfiy the move
- Apply it to the state
- Send the new state to clients

### Advantages
Clear sparation of concerns between game logic and gui.
The authoritative game state is at the server
Saving and loading is easy.

### Disadvantage
Less fault tolerant (a crash of the server can destroy the game state). Two applications to develop. As the game is remote the reaction time is slower.  
Caching can be used but must be kept up to date.

## Distributed system startup
Always, asymetric. For the server this is centrilized, a machine is always running and when a player starts it connects to the startup machine to annonce its presence.

Decentrilized: P2P, start one player machine, remember network informations, the other peer are provided with network information of the first player, when contacted the registration per forwards network information to all other registered peers. These peer may also provide broadcast functionality that allows new peers to announce themselves.

## Commands solutions
Use commands that define a move actions, each action knows how to validate and execute itself, which results in updating the game state.

# System operations
Considered to be a black box, Before Precondition must be true, After post condition must be true after.

## Schema
See the slides

### Cool conventions 
**_e** is added at the end of an exception message.

---
# 26-10-2018
## Protocol model
All the messages and the way information flows through the syetem is described in the operational model. That tells us the individual stories between the messager and the messagee, the protocol model explain in which order the flow of information and state changes occurs. This model describes the correct sequence of interactions that the system may have with its environment. For this purpose we are going to use the Use Case Map model.

## Basic structure
Start Node which is just a dot, then we have the bar which is the end point. Crosses on the line represent an responsability. We can split the paths in 2 direction or-fork/join. And-fork join where the path split but will not carry on until both ends of the path have reach the end point. Rombus represent stop points when the map gets too big saying that the flow of the graph continues on another UCM. We can use arrows from the line to loop back to a certain point.

#### Stub
Is the rhombus mentioned above. 
- Static in out line
- Statuc multi multiple out paths
- Dynamic stubs (different follow up graph conditins)
    - Drawn with dash lines
    - Dynamic sync stubs (Add an S in the center of the rhombus)
        - Used to say that all executing map of the stub to execute before carrying on.
- Stub with conditions with dark circle underlined with a black bar that itself receives another path which is the exta imput required for the conidtion
    - The stub has to wait for something to carry on specific condition
- Timer
    - represented by a clock with a underlined black bar and two exit path same as above for the bar actually.
    - One where the timer exits when out of time
    - One where you go if the path carries on before the time ends
  
We can add a start point for failure that can intersect with the path and draw an arrow from that start point to the path.

We add a sort of triple barr symbol on the path when we want to verify if the failure occur, if it does then the execution is moved to the failure paths.

If we want everything on the paths to stop when a failure occurs, any concurrent flow must be stoped then we add a lightning arrow at the bottom of the failure node.

Inputs and outputs are marked as <<in\>> and <<out\>>  

---
# 07-11-2018
# Behavioral design
Specify solution that satisfiy the requierments previously speciified.

## Interaction model
Sequence diagram 

## Dependency Model
Description diagram that supports the sequence diagram.

The most important thing in design is to focus on responsabilities.