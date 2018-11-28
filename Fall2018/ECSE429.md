# ECSE 429 Software Validation

---
# Lecture 1 05/09/2019
## Mark distrubition
Final exam 36% crib sheet (not multiple choice)
Project 40% 5 students per group  
Quizzes 9% 3 quizzes in class during lecutre (some multiple choice)  
Assignment 15% 3 take home assignments (3 students), late submission are 5% less per day late    
Final grade is less than 50% then you fail

---
# Lecture 2 07/09/2018
## Terminology
**Software**:  
Includes computer programms, procedures and assiciated documentation

**Software Engeneering**:  
The application of a systematic, disciplined, quantifiable approach to the development, operation, and maintenance of software.

**Software mistakes**:  
Mistakes made by the programmer

**Software Defect/Bug**:  
When the mistake becomes a bug in the system.

**Software Failure**:  
Caused when the defect or bug executes

**Incidence**:  
Consequence of the failure.

### Defect causes
**root cause of defect**:  
Earliest action or condition that contributed to creating the defect. i.e the reason why there was a mistake in the software, maybe du to the software but could be due to wrong requierments given to the developper.

**root cause analysis**:  
Identify root cause to reduct occurence of similar defects in the future.

**Effect of a defect**:  
Observed by by product owner PO.

## Nine causes of software defects:
1. Faulty requierments definition (incorrect, missing, incomplete, unnecessary) ``This is the number one reason for defect.``
2. Client-developer communication failures
3. Deliberate deviations from software requirements (improper reuse, omission due to time pressure, gold-plating)
4. Logical design errors (problemes with algorithm, sequence of action, illegal imput, boudnary conditions)
5. Coding errors (null pointer)
6. Non compliance with docutmentation and coding instructions (use dynamic memory allocation when you are suppose to use static in the software context)
7. Shortcomings of testing process
8. Procedure errors (workflow and user interface errors) ``design errors being the second most commun reason for defect`` updates are done in the wrong way witht the time frame for example.
9. Documentation errors

## Cost of fixing bugs
|Phase in which found|Cost ratio|
|--------------------|----------|
|Requierements|1|
|Design|3-6|
|Coding|10|
|Unit/ integration testing|15-40|
|system/acceptance testing|30-70|
|Production|40-1000|

## Software Quality
Lack of bugs/low defect, documented defect (# of defects).  
High reliability/availability (number of failures per N hours of operation). Calculated using ``MTTF`` (mean time to failure) failure free operation until a specified time, ``MTBF`` (mean time between failure) the probabilty that the system is up and running at any given point in time.

### IEEE def
The degree to which a system meets the specified requierments and the customer expectations

### Pressman
Conformance to explicitly stated functional and performance requirementsm explicitly documented development standards, and implicit chracteristics that are expected of all professinally developed software.

### Verification
Are we building the product right

### Validation
Is the building working as expected?

### Pervasive problems for software
* Given late, way over budget and of unsatisfactory quality
* Validation and verification not based on well defined techniques.
* Software dev processes are commonly unstable.
* Software quality is poorly measured, monitored and controlled.

---
# Lecture 3 12/09/2018
## Software Quality Factors
### McCall's model
3 major phases:
- Product Transition 
    - portability
        - Adaptation to different software environment
    - Reusability
        - Use of software for other projects
    - Interoperability
        - Ability to interface with other systems
- Product Operation
    - Correctness
        - Accuracy and completness of required output
    - Reliability/availability
        - First Failure/Maximum failure rate
    - Efficiency
        - Resources needed to perform softare fonctionality
    - Integrity
        - Security system, access rights
    - User experience
        - How intuitive, how much training
- Product Revision
    - Maintainability
        - Effort to identify and fix software failures
    - Flexibility
        - Degree of adaptability (to new customers, new taks)
    - Testability
        - Support for testing (lob files, automatic diagnostic) traceability
        - Reproducable path to the error

## Software Quality assurance
A planned and systemic pattern of all actions necessary to provide adequate confidence that an item or product conforms to the standards set.
1. Acceptable level of confidence fonctional
2. Acceptable level of confidence that it conforms to scheduling and budgetary requirements
3. Initiation and management of activities for the improvment of greater quality software.

### Measurments
1. Formal methods - verify mathematically specified properties
2. Testing - Explicit input to exercise software check expected output
3. Inspection - human examination of requierments, design, code
4. Metrics - measure a known set of properties related to quality
    1. Sycomatic complexity (The more loops you have in a method the higher the complexity the most likely to have errors)

### SQA Defects
__Prevention__: pervent defect from occuring in the first place through traing, planning and simulation  
__Detection__: finds the defect through inspections, testing or measuring.  
__Remove__: Isolation, correction and verification of fixes through fault isolation, fault analysis and regression testing.

## Software development lifecycle models
- Sequential 
    - Waterfall, V-model
    - Deliver software containing the complete features but takes years
- Iterative
    - Agile, Kanban, Scrum, Spiral
    - Software features grow incrementaly

## Software Testing
### Kinds of defect/ Fault model
Ambiguities: In the requirements     
Omissions: missing an else statment     
Inconsistencies: Use of metric system in one method and imperial system in the other.  
Inaccuracies: Using 3 as a value for Pi or not using values with sufficient level of precisions.  
Contradiction: software holds information only if you have a username but the username only exists if you have the information.  
Superflous statement: extra if statment or undocumented fonctionality, trap door.

What can of faults do we espect the programmer to make in order to create the testing technics that will test for these faults.


### Objective of testing
For customers: support decision making, reduce risks.  
For project manager: Evaluate work produce, verify the requierments, build confidence in the level of quality of the product, detect and prevent defects  
External authorities: Comply with legal or regulatory requierments or standards. Verify that the software comply with the standards.

---
# Lecture 4 14/09/2018
## 7 testing principles
1. Program testing can be to show the presence of bugs but never their abscence
2. Exhaustive testing is impossible (testing is always incomplete like testing a factorial function for n). The continuity propery cannot apply to software as a bridge build for w car will work if there are y < w car the same thing cannot be said for software.
3. Early testing saves time and money. The cost of deffect rises as we find the bug later in the production stage.
4. Defects are clustered together, the more bugs you fix the more bugs there are.
5. The pesticide paradox. A system tends to become resistent to particular testing. Executing the same test will not find new bugs.
6. Testing is context dependent, increase level of criticalit means increase level of testing
7. Abscence of errors is a fallacy. Finding bugs or finding no defect in the software does not help us to know if the software is bug free. (it does not prove anything)

## Test levels
Groups of test activities that are organized and put together.

### Unit test
Takes component code as imput

### Integration testing
Component code tested, integrated modules.

### System test
fonctioning systems

### Acceptance test
Validated system

## Unit testing
Usually automated
### Objectives
Verify the fonctionality of a component, find defects in components, build confidence in component quality

### test basis
detailed code data model, component specific (only specific part of the code is tested no integration)

### Test object
What we are testing? Components unit, classes, code+data structure, databases modules

### Typical defect
Incorrect fonctionality, Data flow problem, Incorrect code logic, defects are fixed as soon as they are found

### Specific approaches
Test-driven dev, Many code-level white box.

## Integration Testing
Interatction between components and interfacs (still automated)
### Objective
Verify the fonctional and non-f of behavior of interfaces

### Test Basis
Software design, Sequence diagram work flow, Protocol and interface specification, Architecture

### Test objects
Subsystem, interface, API, Databases, infrastructure, microservice

### Typical defects
Incorrect/missing data or message structure, wrong timing, Interface mismatch, Communication failure between subsystems, incorrect assumption between bondaries units

### Specific approach
Bib bang vs incremental integration, Architecture based buttom un/top down, Functional decomposition (Test interaction between let say pressing the break and the ABS would it work in combination with ACC)

## System Testing
Carried out by independent testers as early as possible

### Objectives
Verify the fonctional and NF of the system as specified, Validate completness, build confidence in the system as a whole,
prevent bugs from going in production, verify data quality.

### Test basis
Software requierments spec, use case state diagrams, User stories, risk analysis report, sysrem and user manual
### Test Objects
Applications, HW/SW systems, system configuration and configuration data

### Typical defect
Incorrect calculations, incorrect sytem behavior

## Acceptance testing
Checking with business compliance, safety compliance legal compliance are done here but not done in the above test
### Objectives
Veryfy the F and FN of the system, build confidence in the quality of the system as a whole.

### Test Objects
Business process of full system, forms, reports existing production data,

### Type of defect
The system workflow do not meet the business requierments, system violates safety requierments, NF failure (performance)

### Type of testing

#### User acceptance testing
Fitness for use by the user, business process are performed correctly 

#### Operational acceptance testing
Test backup and restore, install, uninstall, disaster recovery Data load / migration, performance testing

#### Contractual and regulatory acc testing
Check contract acceptant criteria, check adderence to regulation perfomed by independent user or authoritities

#### Alpha beta testing 
Alpha test done by existing customers at the development place, beta done by existing customers in their own location.

## Test types

| Fonctional testing | Non fonctional testing
|-----|-------|
|Evaluate functions othat the system shoudl perform what the system shoud do| How well the system behaves? Caracteristics of the system |
|Completness, correctness, appropriatness| Reliability, performance, security, usability|
| performed at all levels | Should be performed at all levels |
| To what exten a fonctional element has been exercised | the extent to which some type of NF element has been exercices|

| Black box | White Box |
|----|-----|
|Same as fonctional (specification)| Derive test based on the sytem internal structure (implementation)|
|All levels | 

---
# Lecture 5 19/09/2018
## Maintenance testing
Carried after system is already in production env. Changes made for maintenance testing is needed to evaluate success and ensure the lack of side effects.

Triggers for maintenance
- Modificiation
    - Planned enhancment
- Migration
    - from one platform to other
- Retirement
    - Fonctionality
    - Sub systems

It evaluates the planned/ecevuted change by identify consequences + possible side effects.

## Test activities and processes
Testing is a process that has many different steps:
1. Test planning
2. Test monitering and control
3. Test analysis
4. Test design
5. Test implementation
6. Test executiom
7. Test Completion

### Test planning
Define the objective of testing, define the approach taken to meet these objectives with the constrains imposed by the context of the system. i.e we can access the innter system of a bank only on Sunday bettween 12 and 14 

### Test Monitoring and Control
Compare actual progress against the test plan using test monitoring metrics. Evaluate exit criteria (check results against coverage)

### Test analysis
Identify test features, define and prioritize associated test conditions, analyse test basis (Requierments, design model, implementation)

### Test Design
Elaborate test conditions into sets of hight level test cases. 

### Test Implementation
Create the software for execution by creating test suites and automated test scripts from test procedures. Arrange the test suites into efficitient schedule. Build the test environment.
Prepare the test data.

### Test execution
Run the test suits in accordance with the test execution schedule. Compare results with expected results, analyse anomalies to identify the causes. Regord the defects based on the observed failures, log outcome of test execution, verify and update traceability.

### Test completion
Collect data from completed test activities. Occures at project milestones, check if all defect reports are closed. Create a test summary to report to the stakholders. Analyse lessons learned from completed test activities and analyse changes for future test iteration.

## Importance of tracability
The idea that each test must be linked to a specific aspect or goal in the code and such a fonction must be followed from the designed document, to the code, to the test designed to test the fonctionality.

## Test driven development
Listen to customers to gather the requierments, develop test cases, code the program then redesign/refactor/clean up as more code is added to the system.

# 21/09/2018
## Static Validation and validation technics
The code is not executed, can be done manually or automatically. It can target all aspects fo the developments
- Specification,
- Architecture
- Models
- Code
- Testware
- Documentation, User guides

With that method we can catch faults earlier and identify defects not found by dynamic testing, save the cost of execution. Does not require the entire program to be coded for the test to begin. Improve the communication between teams members with code reviews which helps among the members of the team. It increases the productivity by allowing for more maintainable code.

### Type of defects found
- Requierments Defects: inconsistencies, ambiguities
- Coding defects: variables with no value, variable declared but never used
- Incorrect Interface specification: different unit of measurment used by the caller
- Security vulnerabilities: Buffer overflows, missing test for acceptance criterion
- Maintainability defects

## Review process
During a meeting during which a work product, or set of work product is presented to project personnel is presented to parties of interests to receive comments and corrections.

### Types
- Informal review
- Walkthrough
    - Monstly informal
    - Driven by the other of the piece of code, evaluate a lot of aspects of the code
- Technical Review
    - Documeted process
    - gaine consensus, detect potential defects
- Inspection
    - Formally documented process, external experts, moderators involves
    - Look for defect, obetain deate and communicate development documents
  
#### Inspections roles
- Mderator
    - Ensure that the inspection procedure are followed
    - In chager of assembling the inspection team
    - keeping the team on track
    - Like the coordinators
    - Usually comes outside of the project team
- Recorder (scrib)
    - Document all defects that arise from inspection meeting
    - Not only procedure but requirese some procedure knowledge
- Reviewer
    - Analyses and detects defects in the work product. 
    - All participants play that role 
- Reader (presenter)
    - Leads the inspection team through the inspection meeting by reading aloud small logical units
- Producer (author)
    - Work product author
    - Responsible from correcting any defects found

### Procedure
1. Planning fase
2. All team member become familiar with the projects
3. Overview of the project sent
4. Preparation prior to the meeting
5. Meeting
6. Third hour (added to discuss solutions and extra discussions)
7. Review reports sent (rate severity of defects, statistics about findings)
8. Rework
9.  Follow-up meetings

### Review checklist (generique)
- Requierments
- Design
- Generique code
- Specific language code
- Generic Document

Organization develop there own check litsts and maintain, improve and develop them

### Identify review types
|Characteristics|InfREv|Walkth|TecRev|Insp
|---------------|------|------|------|----|
|Scrive is mendatory | no |yes|yes|yes|
|Review Meeting | no |no|no|yes|
|Individual preparation |no|optional|yes|yes|
| Use of checklist Optional|yes|yes|yes|no|
|Defects log and review reports produced |no|yes|yes|yes|
| Review meeting led by the author |no|yes|no|no|
| Reviewer should be technical experts|no|no|yes|yes|
|Follows a formal documented process|no|no|no|yes|
|Review meeting led by a facilitator|no|no|yes|yes|

## Modern code reviews
More lightweight, flexible and asyncronous. Usually done by open source communities. There are two essential roles author and reviewer. Code reviews are done more often with more documents.

It is the autors responsability to submit code that is easy to review, prefer small changes

### Commit messages
Capitalize short title

Explainatory text or bullets point

Must be written in imperative (fixed bug,)

### Reviewer criteria
- Purpose
- Implementation
- Legibility and Style
    - The read efforts
    - Does the code contain TODO
- Maintainability
    - Read the tests
- Security
    - Verify API

### Comment style
Critisize the code not the author. Avoid judgment. Differentiate between, pointing out the error, suggestions and orders.  

Developer should respond to every comment. Explain the decision process

---
# 26/09/2018
# Static Analysis tools
## Coding guide lines
Set of rules that give recomendations on style (formatting, naming, structure) and best programming practicies
### Industry/domain-specific (automotive, avionic)
MISRA C: focus on reliability, safety, security.  
Tools: PolySpace, SonarQube,  Coverity

Made of rules 143 (imposed) and 16 directive. Rules can be checked with yes/no/undecidable answer by static analysis on the other hand directives can be subjectives.

#### Some rules of MISRA C
1. Project shall not contain unreachable code
2. Shall have no dead code (operation is reachable but removing the operation does not affect program behavior)
3. Identifiers shall be distinct from macro names
4. A typedef name shall be a unique indentifier
5. The right hand operand of a logical && and || shall not contain any side effects
6. A goto statement has shall not be used
7. All if...else, if construct shall be terminated with an else statment.

### Platform specific (Java, C)
We have 5 key words: Consider, DO NOT, DO, AVOID.
1. CONSIDER making base classes abstract even if they do not contain any abstract member
2. Do not provide apstractions unless they are tested by developing several concreate implementations and APIs consuming the abstracion
3. DO choose carfully between abstract class and interface
   
### Organization specific (Google, CERN)
Categorized into: source file basics, source file structure, formatting, naming, programming practices, javadoc.
1. Never make your code less readable by fear that some program may not handle non ASCII characters.
2. Local variable names are written in lower case
3. Brackets are used for all if statmetns even when not required
4. Each statement is followed by a line break

## How to enforce these guidelines
Some functions are build in into the IDEs but we can use external tools.

# Patter-Based static analysis tools
Automated analysis of software without code execution. Commun tools includeL FindBugs, PMD, SonarQube, CheckStyle.

## Traditional set up
First the source code is given to the parser. Parser takes the program code and the grammar that describe the syntatically correct program.  
First it goes to a lexer that takes the source code and creates a list of tokens (sees a class that must have 5 carachters and a space so the lexer creates one class token)  
Parser: Then takes that token list and creates an abstract syntax trees. If there is a violation of the rules the parser will give an error message.

i.e 13+15, 13,+,15 are all 3 tokens, the intax tree would be addExp with the 3 tokens above.

From an software engeneering a parser represents a list of errors. For example now parser can give many errors at the same time.

From the abstract syntax tree we can then build the type hiarchy seen on eclipse or the call graph that shows the run through of the program. These are done by filtering what matters from the view in the syntax tree.

Note to create a proper type hiarchy and call graph we got to also go back to the librairy methods and also include return values. This is how our tree becomes more like a graph rather than a tree. And we use that graph to build the things mentioned above.

### ASG/DOM
From the abstract syntax tree we can use Document Object Model it includes javaProjct file, the package. These may not be 1 to 1. Fun fact the type hiarchy and call grah are usually generated from that.

The patte based static checks are derived from the DOM. Fun fact when we refactor we actually refactor first in the DOM tree that then modifies the abstract syntax tree that itself then changes the source code.

Our pattern-based static analysis find erroneous cases by graph pattern matching. It then finds faulty graph patterns in the DOM graph by having node and edges criterion that can be used to detect faulty graph stucture in the entire graph. If there are no matches then there are no violations.

A good practice is too cache the violation results so then when they get updated it is easier to validate.

This is a lightweight static analysis it is important to remember that we may have false alarms (presence of a defect/issue that would not cause a failure) and false negatives.

---
# 28/09/2018
# Abstract Interpretation for static analysis

## Testing vs Static analysis
Testing investigate one run of the program for particular, execution context. Static analysis reason about all runs of a program without executing it for any impur and execution process.  
Results of testing are pass/fail. For static it is safe/error/incomplete.

## Complexity issue
Can a static analyser detect whether with imput I a certain program will terminate. The hating problem, it is not possible to right a program that works for all inputs.

Perfect static analysis is impossible but can still be very usefull and is widely used today.

## Soundness VS Competness
Soundness: means that if a prover says that some property holds then the property is true. SA says nothing.
SA, if program is error free to it then it is really error free, alarm does not implie defect.

Completness: if property is true SA says that the propery is true. SA says everything. If SA says it is erroneous then it is erroneous, the lack of error messages does not implie error freedome

## Designated properties of SA
- Precision: minimize false alarms
- Scalable: capable of analysisn large programs
- Understandability: Error reports are interpreted by people and hence must be easy to evaluate.

## The power of abstraction
Usufule ones, sign(+-), intervals, pointers, taint analysis (security), precise treatment by Galois connections.

pros: 
- may achieve higher coverage than testing
- May prove abscence of defects
- May find subtle programming flows

Cons:
- Properties are limited to correctness (no performance)
- False alarm/ missed errors
- may be time consuming to run or non-terminating.

---
# 03/10/2018
# White box testing
Focus on systems internal logic, does not look at the specifications just focus on the code.

Advantage is that we check the source code itself but not very affective in detecting missing functions.

## Control flow analysis
Directed graph where nodes are block of sequential statments and edges are transfers of control. Edges may also be labaled with predicate representing  the condition of control transfer. It is not required to show intermediate statments

Basically it is the graph seen in comp 303 where we branch out on if statments.

Nodes that correspond to a branching should not contain any assignments or operations.

### Coverage criteria
Node coverage: cover every node in the test. Our goal is to minimize the number of test cases that provide a given coverage. Here our goal is to execute all nodes we do not care about going through every path. 

### Branch or edge coverage
Achieve a 100% if we have tranversed each edge of the control graph at least once. Note that if you cover all branches then you covered all the nodes.

### Condition coverage
Each condition constituent need to be evaluate to true and false at least once. This does not garenty 100% branch coverage.

### Condition/Decision coverage
Combine branch and condition coverage.

### Multiple condition coverage
Investigates all combination of condition substituent in decisions.

### Condition/decision coverage
Each condition must be true at least one and false at least once. There exists a pair of test cases where only one condition changes and the outcome changes too. require n+1 test cases.

### Path coverage
Have a test case for each possible path in the program. But some path may be infinit or infeasible.

## Loop coverage
Test zero times, 1 time and 2 times or more.  
minimum-1, minimum, minimum+1,typical,maximum-1,maximum,maximum+1

---
# 05/10/2018
### Nested Loop
Start at inner most loop, set all the outerloops to their minimal values. Set all the other loops to there typical value. Then perform the dedicated tests for that specific loop.

Move up nested loop level.

## white box test process
1. Set coverage goals
2. Derive control flow graph (CFG) of source code
3. Determine path to obtain coverage goals
4. For each path, sensitize path with imput values use specification for output and watch for unfeasible path
5. Run test cases
6. Check coverage

### Path sensitizing
Find the set of imputs that will force the selected path.
Backward (exit to entry) or forward (entry to exit) strategy. Some path may be infeasible which may involve some rewritting.

### Path conditions
Conjunction of branch predicate required to hold afor all the bracnhes along path (what values must be true for that path to be executed). This is done through symbol evaluation (x_1,x_2).

To solve such condtions and equations we can use certain tools like Solver designed by microsoft.

### Path conditions with while loops
In this case variables are written with x_1.1,x_1.2 and so on.

# Data flow analysis
Uses control flow graph and focuses on the assignment of values to variables and their uses (where data is defined and used).

It analysees the occuruences of variables, define the occurence.
- __Predicate__ use: variable used to decied whether predicate is true
- __Computational use__: compute a value for defining other variables or output value.

### Example
```
x = y + z (this statment defines variable x and uses variable
y and z)
scanf("%d",&x) (defines variable x)
printf("%d",x) (uses variable x)
```

## c-use
Define computational used, meaning that the variabl is used as part of an assignment, an output a paramter for a function call

## p-use
Is the occurence of a variable in a condition branch statement.

## Basics of data flow analysis
We will use these labels
- d(v,n) a value v is assigned at node n (definition)
- c-use(v,n) variable v used in a computation at node n
- p-use(v,m,n) variable v used in predicated from node n to m
- k(v,n) kill, variable v is dealocated at node n.

Then we can use a tree diagram to see the regular use of nodes and their regular transitions, if we have transitions that are not in the graph in our code then we could label these as potential defects or warning.

### General check list
 dd suspicious
- [ ] dd suspicious
- [ ] dk probably defect
- [ ] du normal use
- [ ] kd okay
- [ ] kk probably defect
- [ ] ku a defect
- [ ] ud okay
- [ ] uk okay
- [ ] uu okay

First occurence of action on a variable
- [ ] k suspicious
- [ ] d okay
- [ ] u suspicious (may be global)

Last occurence:
- [ ] k okay
- [ ] d suspcious
- [ ] u okay (but maybe deletion frogoten)

## Data flow graph
We add the data flow nomenclature to the regular control flow graph.

Predicate use always happen on the edges the rest are labeled on the nodes

---
# 10/10/2018
## Data flow graphs
#### Complete path
initial node is the start node and final node is the exit node

#### Simple path 
all nodes except possibly first and last are distinct

#### Loop free path
All nodes are distincts

#### Def clear path
with respect to v: any path starting from a node at which variable v is defined and ending at a node where v is used without redefining v anywhere else.

#### Du pair
d nodes where v is defined u nodes where v is used, def clear path from d to u.

## Data flow coverage criteria

### All definitions
We select at least 1 def-clear path from every dfining nodes of v to at least one use of v regardless of p or c use.

### All uses
For all variables v therse should be one def clear path from every defining node of v to every (reachable) use of v

### All p-uses some c uses
For all variable v we have one def clear path from every defining node of v to every reachable p-use if def v has non then we go to c-use.

### All c-uses some c uses
Same idea than above but reversed

### All DU paths
All du paths covered for all variables v to all c/p use of variabl v.

# Mutation testing  
Who tests the test?  
Fault based testing: directly towards "typical" faults that occur in a program.
1. Take a prorgam and test data generated for that program
2. Create a number of **similar** programs (mutant) each with a small changes (change operators which mimiks regular dev errors)
3. The original tests data run through the mutants
4. If test data detecs all differences in mutants then the mutants are said to be dead, and the test set is adequate.

## Mutants survival
Remains alive either because it is equivalent to the original program or the test set is inadequate to kill the mutants. 

## Types of mutants

#### Stillbord mutant
Syntatically incorrect, killed by compiler

#### Trivial mutant
Killed by almost any test case

#### Equivalant mutant
Always produce the same output than the original program.

None of the above are interesting. We want to detect mutants that represent hard to detect faults.

## Mutation coverage

Complete equates to killing all non-equivalent mutants.  
The mutation score (ration of dead mutants over all non-equivalent mutants)  
We can see each mutant as a test requirement  
The number of mutants depends on the definition of mutation operators and the software syntax/structure.  
Number of mutants tend to be large even for small software (random sampling)?

### Design tests input that will kill mutants
- Reach the fault seed during execution (reachability)
- Cause the program state to be incorrect (infection)
- Cause the program output to be incorrect (propagation)

### Assumption
Competent programmer assuption: the programmer will give us a product that is correct or only differs from the program by a combination of simple errors.

Coupling effect assumptions: test data that distingishes all programs differing from a correct one by a simple error is so sensitive that it also implicitly distingishes more complex errors.

---
# 12/10/2019
## Anti-patters/code smells
Oppsit of design patter, identifies poor solutions to recurring design problems.

Grouped into 3 categories
1. Classic bad smells
    1. Obscure test
    2. Hard to test code
    3. Test code duplication
2. Behaviour smell
    1. Assertion rouletter 
    2. Erratic Testing
3. Project Smells
    1. Buggy tests (lots of test fails are regularly found)
    2. Dev not writting automated test
    3. High test maintenance costs (too much effort kept to maintain existing tests)
    4. Production bugs (too many bugs found during formal test or productions)

## Test Code smalls (classic bad smell)
## Obscure test
Hard to understand what a test does at a glance. Can lead to high test maintenance.  The causes are usually, wrong information in the test or too little or too much.

### Eager tests
Tests verifies too much functionality in a single test method. It is better to have a suite of independent single condition tests.

### Mystery Guest
A filename of an existing external file. The content of a database source. Such that the test reader is not able to see the fixtures (cause effect) between the test result and what is tested. This is fine when testing the database or data structure but when testing other features this is bad testing.

Normally files must be read from a setup phase.

### General Fixture (UNIT TEST)
Has two tests code that have the same setup. Use minimal fixture as much as possible in unit testing.

### Hard-Coded Test Data
The output is tested based on hard coded value can be problematic

### Conditional Test logic
Use of itteration or if statment that makes the test code contains information on code that may not be tested.

Causes: flexible texts , test code verifies different functionalilty depending on where and when it is called.

Multiple test condtions: use parametrized tests.

### Hard to test code
Code is difficult to tests which makes it hard for us to verify its quality.

### Test code duplication
Same test code is duplicated, this makes it a maintenance nightmare.

### Test logic in production
Using different data purposly for testing then in real production. Opening the door to new bugs, makes SUT more complex
```
if (testing) {
    return hardCodedData;
}
else {
    return gatheredData;
}
```

## Behaviour tests
**SUT**: Software under tests. 
### Assertion roulette
Multiple asserts of the same time like assertEquals which makes us unable to know which one failed especially for automated tests.

### Erratic tests
Sometimes the same test passes other times it fails.

Impact: tempted to remove the tests from the test suites causing tests lost.

Caused by interating tests/test suit. Resource leakage. Test wars: test fail randomely when the tests are run simutaneously.

### Fragile test
The test fails to run when the SUT s changed in ways that do not affect the part where the test is exercised. 

Due to:
- Change in the test interface (part of the interface changed so test does not compile)
- Test fails because data was modified
- context: the context in which the test is executed has changed
- Behavior sensitivity: changes in SUT causes other tests to fail. (regression testing )

### Frequent debugging
Manual debugging is required to determine the cause of most test failures.
Caused: missing unit tests or component tests.

### Manual Intevention
The test requires a personto perform some manual action each time it runs. 
Caused by: manual injection, result verification.

### Slow tests
Taking too long to run.
Caused:
- Slow component usage of SUT
- General fixture: Tests slow because eahc test builds the same fixture.

# Principals of good practices
The test auomation manifesto, containing 13 criteria
1. Write the test first
2. Design testybility
3. Use the front door first (public interface)
4. Communicate intent
5. Do not modify SUT
6. Isolate the SUT
7. Keep the tests independent

### verify one condtion per test
The bad idea many tests require a specific initial state. Many operation changes this to a new test and we reuse the new state for a second test. This is bad practice because if one of the tests fails all the ones after will not run.

#### Good test code structure
1. SetUp test fixture
2. Exercise teh SUT
3. Verify Expected outcome
4. Tear down test fixture

### Naming conventions
Name test classes + methods systematically

Test class + test methods should convey:
- Name of SUT class
- Name of method/feature being exercised
- Important characteristics of any input values related to exercising the SUT
- Anything relevant about the state of the SUT or its dependencies

### How to organize test case classes
Could be different based on the design choice either:
- 1 test case class per class
- 1 test case class per feature
    - method 
    - per user story
- 1 test class per fixture

### Code reuse: Parametrized tests
We pass information needed to do the fixture setup and result verification to a utility method that implements the entire life cycle. For example item iteration is done externally and each item is sent to the same test method.

---
# 19-10-2018

# Black-Box Component Testing
Testing without having insight into the underlying code. It is based on the system specification as opposed to its structure. But coverage can still be measured based on the specification of the block we can outline the different ways the block can be executed representing the coverage.

It helps functional testing by categorizing inputs and derive expected outputs. 

- Adventages
    - No need for source code
    - Wide applicability, unit to system level testing

- Disadvantage
    - Does not test hidden functions

## Equivalence partitioning
Partition the input set based on equivalence classes. Aiming for completness (aim for all the input space). Avoid redondancies (have equivalence class overlap).

In practice it is difficult to find perfect equivalent classes most of the time heristic algorithm or best guess.

### Weak/Strong Equivaelent Class Testing
We need to generate test cases based on equivalence classes.

**Weak**: choose one variable value from each class for every input.

**Strong**: Based on the cartesian product of all equivalent classes of all the input of the function and combine them

i.e we have 3 variables A,B,C and A has 3 equivelent classes same for C, B has 4. Well weak will take just 4 test cases to cover all the equivalent classes of A,B and C. But strong would have 3x3x4 test cases.

#### Exercise
Find the equivalence classes for years between 1812 and 2012. Including leap years divisible by 4 when not a century, centuries have to be divisble by 400 to be a leap year.

|equivalence classes|
|-------------------|
|year == 1900|
|1812≤year≤2012 and year ≠ 1900 and year mod 4 =0|
|1812≤year≤2012 and year mod 4 ≠ 0|

### Equivalen classes new Ideas
If error condition are high priority we should extend strong equivalence class testing to include both valid and invalid input.

### Heuristic for identifying EC
For external input:
1. If input is in range of valid values
    1. One valid Ec within the range
    2. Two invalid EC outside each end of the range
2. In input is a number N of valide values (array), define:
    1. One vaile EC
    2. Two invalid EC (none and more than N)
3. If input is an element from a set of valid values, define:
    1. One valid EC (in the set)
    2. One invalid Ec (outside of the set)
4. if input is must be
    1. One input that satisfies the condition
    2. One input that does not satisfy the condtition
5. If there is a reason to think that element of EC are not handle the same
    1. Create sub EC's
6. Consider creating an EC for null, none, empty, 0 values

### Myer's Test Selection Approach
1. Until all valid EC have been covered by tests cases, write a new teast case that covers as many of the uncovered valid ECs as possible
2. Do the same thing for invalid EC's

#### Advantages of equivalence
Small number of test cases, probability of uncovrering defects with the selected test cases is higher than that of randomly chosen test suits.

#### Limitations
Strongly type languages eliminate the need for consideration of invalid inputs. Equivalent classes are as good as the quality of the specifications. Myer's test selection appreach is weak when input variables are not independent. Brute-force definition of test case for every combination of the inputs ECs provides good coverage testss but is inpractical in practice

## Boundary value analysis
Typical programming errors tend to occur near extreme values (boundaries between different classes). This is what this focuses on, testing boundaries value

Works well for data types with boundaries conditions

Conventions min,min+,nom,max-,max. Hold all other values of the other variables at nom setting. **For each boundary condition**

The same idea can be applied to output conditions.

This costs generally **4n+1** test cases (n=number of variables), note that we are not counting values outside of the range yet.

#### Example findPrice()
Takes two values item code between 99 add 999 and quantity between 10 and 100.

Go and look at the slides they were not posted when I took the notes

### Robustness testing
Add min- and max+ to the boundary value analysis which gives us **6n+1** number of tests

### Worst case testing
When we set all variables to extream values at the same time all min+. **5^n** test cases.

### Robust worst test testing
Combines both things above. **7^n** test cases.

---
# 24-10-2018
# Decision Table
Helps express test requierments in a directly usable form.
Action do not depend on prior, input or output, action depends on input variables, order of the input does not matter.

## Structure
|List of conditions (relation between decision variables)|Unique combination of condition|
|------|----------|
|List of resultant value|List of action selected|


|Condition|1|2|3|
|---------|----------|----------|----------|
|c1|True|True|False|
|c3|-|True|True
|Action|
|a0|X|X|
|a1||X|X|

\- for do not care helps reduce number of variants. Input are necessary but have no effect, imput is not necessary, mutally exclusive case.

Do not happen is an assumuption however compare to do not care which is not.

## Test gen strategies for decision table
1. Test case for every explicit column with do not cares (basically each column of the table)
2. All variants, test case for all implicit variant 2^n
3. All true, test case for each column with output
4. All false test case for each column with no outcome

# Binary decision diagram
Uses a tree to represent the boolean conditions of a decision table. Each level in a tree represents a condition, then we have 2 outgouing edges one the true branche for the true condtion and false branch for the false condition. Each leafe represent the value of the formula (so far the formula is a boolean).

**Coninical**: compare the value of two exhisting functions in a tree by just looking at the nodes if the variables appear in the same number.

## Reused and ordered binary decision diagram
Omit redundant boolean variables, if both T and F have to be distincts for the child node. We also allow the sharing of identical subtrees.

$$ f = a \land(b\lor c)\\
f = a\rightarrow f_a,f_{\bar a}\\
f_a = 1 \land(b\lor c) = (b\lor c)\\
f_{\bar a} = 0 \land(b\lor c) = 0\\
f_a = b \rightarrow f_{a,b},f_{a,\bar b}\\
f_{a,b} = (1 \lor c) = 1\\
f_{a,\bar b} = (0 \lor c) = c\\$$
And so on for the c's

### RBDD checklist
1. Define an ordering of variables
2. Make the decision along the paths
3. Remove redondancies
4. Merge equivalent subtrees
5. Map RBDD to a RBDD determinant table
6. Generate the test suits 

# Cause and effect modelling 
There is a problem statment we construct cause effect-table or graph then we define the test cases.

We aim to identify causes (input, stimuli), effects (output, changes in system state)

---
# 26-10-2018
# Cause-Effect Modeling
Helps decision table and tree diagrams to generate test casses

We right truthe statment for each effect present on the table. We also add some statement at the bottom of the graph to show which parts are mutally exclusive.

Then we have to draw a cause and effect graph, on the left the causes for example on the right the effect. A line from a cause to an effect is a necessary condition for the effect to take place. If a single effect has more than one condition the relationship is annotated with or,and,xor,negation. Itermediate nodes may be used to simplify the graph and its construction.

We use dash lines to represent exactly one.

### Advantages
Aids to select high yield test cases. Give an oracle and specifies constrains on input. The addition of additional constrains help restrict the graph even more. Finally it helps catch inconsistencies and ambiguities in the model.

Identify causes and effect from spec, annotate model with constraints describing impossible combinations. 

It can be used to generate limited entry decision table. (with annotation such as exactly 1 of and so on).

#### Exercise
The file update depends on the value of two fields, they must by 'A','B','C','D'. The value of field 2 must be a digit.

|Cases| 1 | 2 | 3 | 4 | 5 |
|-----|---|---|---|---|---|
| Field 1| A | B | C | Not A,B,C | Any |
| Field 2 | digit | digit | digit | any | not digit |
| Action |
| Updated | x | x | x | | |
| error X12 | |  | | x | | 
| error X13 | |  | | | x | x|

## Deriving decision table
Columns corresponds to variant, row for each cause and effect. Then we can annotate the variant removing any impossible combinations.

# Integration testing
Ensures that components interact correctly when assembled. We assume that components have already been tested. We need a component dependency structure to see what component depends on what.

## What to test for
Assuming that each components work in isolation, then we look for interface related faults, wrong method called, provide wrong parameters, incorrect parameter values, use of version that is not supported.

## Strategies
Understand how different components are assembled together. As we must look at the test component interaction and determine optimal order of integration.

## Stubs
Repaces called module: for inputs it passes test data, stub output module returns test results.

They are used because we want to avoid side effects of the methods, avoid maybe communication accross distance. Stub only does things (simple) related to the integration test hence the likely hood that the stub is faulty is lower.

Conventions:
- must be declared/invoked as the real module
- Same name as replaced module
- Same parameter list
- Same return type as replaced module
- Same modifier (static, public, private)

### Function of a stub
- Display/log trace messaged
    - parameters for exampe
- Return value according to test objectives
    - From a table
    - From an external file
    - Based on a search according to parameter values

## Driver modules
Used ot call tested methods (tested modules). Passes parameters and handles return values. Junit for example provides us with drivers to test code. Junit can be used for integration testing as well, to set up multiple components and there integration.  
But by default it does not provide with stubs.

## Integration srategies
### Big-Bang Integration
Non incremental strategies. Once everything has been tested independently we test all the integration at the same time.

Convenient for small stable system

Yet fault localization are harder, easy to miss interface faults, does not allow parallel development.

---
# 31-10-2018
## Quizz 1
Black box testing and integreation testing

### Top-Down integration
Firs test the higher level components before testing lower levels in respect to some dependency relation. Imagine a tree we do a breadth first search testing each level one after the other. The order can still be twicked to test components earlier if they are more critical or if they include input and output. 

Note that we keep each layer on every test if we test for ABC then next will be ABCDFG with ABC still present.

#### Advantages
It is incremental, mistakes can be discovered layer by layer, no need to write drivers. Allows for parallel development. Major design flaws found first. Allows for early prototypes to be tested.

#### Disadvantage
Need a lot of stubs when implementing. Potential reusable componenents at the bottom of the hiarchy will not be tested at every stage making them less throughly testing

### Bottom up strategy
The same idea as above testing the lower level components. More like a depth first search test.

#### Advantages
Does not need that many stubs, reusable components testing throughly. Developement can be done in parallel.

#### Disadvantage
Needs drivers, High level components tested at the very end and the least. No concept of early skeleton system (early prototype)

### Sandwich integration
Combines top down and bottom up implementation.  
Split into 3 layers, 
- Logic (top) tested top-down
- Middle
- Operational (botoom) tested bottom up

So the top layer are tested throughly and vice versa with bottom layers.

### Risk driven integration startegy
Integrate components based on criticality. More critical elements will be integrated first.

### Function/tread based integration
Integrate components according to threads/function they belong to.

# Testing object oriented systems
Expert says object orientation while it helps analysis and design of systems. Requires more and not less testing for OO software.

Usually unit and integratin testest are the most affected tests as they are more driven by the structure of the software under test.

## Main difference
### Procedural language
based components: functon procedure. Testing method based on input and outputs

### Object oriented
Basic components = class. Instances of classes must be tested. Correctness cannot be just defined by input/output relations but must also include the object state.

The complexity lies a lot in method interaction. 

Test cases have to be defined based on object state, it is important to understand how to complement output and input relations with state infroamtion to define the tests.

Must think of how to manipulate object state without violating the encapsulation which can be hard.

Polymorphism and dynamic bidings leads to one-to-many possible invocation of the same interface.

Each exeption must be tests, what about null pointer exception.

## New fault models
- Using wrong instance of method in inheritance
- Wrong redefinition of attributes
- Wrong intance of operation called to do dynamic binding and type error.

## Integration levels
Basic unit tests which the test of a single method of a class (intra method).  
Unit testing: the testing of methods within a class. Claimed that any significant unit test cannot be smaller than the instantiation of of one class.

### Intra class testing
We perform the white and black box testing along with the new:
- Raise execpetion at least once
- Each interrupt forced to occur at least once (mouth click)
- Each attribute set and got at least once
- state base testing
- Big bang when methods are tightly coupled togther
- Alpha-omega cycle (constructors, get methods, boolean methods, set methods, interator, destructor)

### Integration testing
Classes introduce different scope of integration testing. In OO we think about inter-class testing. association, composition, inheritance.

Cluster integration:
- Tow or more classes through inheritance. Incremental test of inheritance hiarchy.
- Integration of two or more classes through containment
- Integration of two or more associated classes/clusters to form a components (subsystem)
- Integration of componenents into a single application. (subsystem/system integration)
- Server client integration.

We then based our strategy based on the cluster choosen and parse things based on the cluster.

Commun strategy separate classes into subsystems and test the integration of these subsystems.

## Mock Objects
The reinterpretation of stubs in an object oriented context.  
Generaly we have tests the acts on the (CUT) code under tests that depends on certain other classes.  
It is difficult to write the stubs as depending on the test done the stub must change to match the test type. So for the same integration test (split into 4 test lets say) then we have 4 stubs.

This is where Mock objects come in. They are a form of stubs based on the interface of that dependent object. Easier to set up and control, Isolate code from details that may be filled later. Can be refined overtime by replacing actual code.

So the new image is tests acts on CUT that depends on interface that is implemented by both the actual code CUT depends on and the mock class. Then the test execution framework is responsible to decide which object does the CUT depends on at run time. In this case the CUT may call a factory method or allow to change the object that changes the service. Hence at the end the test itself controls the mock behavior.

### How to create them
Manually which gives more control but more work

Framworks are available such as Mockito, PowerMock, EasyMock.

---
# 02-11-2018 Last part for quizz2
# Integration strategies
## Cluster integration
Integrating together a subset of classes. It is important to have a class dependency tree. Which one is on top which ones inherit form what,

### Big Bang
From there we take the entire tree as a whole like discussed.

### Bottom up
Moving up dependency tree. This allow to test the reusable components more frequently and require less stubs. This is applied in the context of a cluster.

### Top down
Go down the dependency tree

### Scenario based operation
Based on an interaction diagrams, choose a sequence of collaboration to be tested. Test the scenario wanted.

### Client/Server Integration
Tests each client with stub server, then do the opposit for one client at a time with the server. Then test pair of client type with server. Then remove all stubs and test the entire system.

### Client/server Integration + a tier in between
1. Test each client type with middle-tier stub
2. Test server with middle-tier stub
3. Test each client with middle-tier and server proxy
4. Test server with middle-tier and client proxy for each client type
5. Test each client with middle-tier and the actual server.

## Integration order problem
As big bang is not advice, integration must be done in a simple manner but this leads to stubs. It is not always easy to construct a stub that is simpler than the actual function. They cannot be automated as it is required to understand the sementic of the stimulating function.   
Hence stubs could lead to a higher level of unstability than the actual fonction, it is important to try to reduce the number of stubs as much as possible.

On the other hand class dependencies are not aleays a tree but a complex graph. A lot of systems may contain cycles such as:
- ATM
- Ant
- SPM
- BCEL
- DNS

### Kung and al. Strategy
Aims to produce a partial ordering of testing levels based on class diagrams.

Classes under test at a given level should only depend on classes previously tested in order to reduce the number of stubs.

#### Object relation diagram (using static information what is the real type of an object)
This is what is created to generate a graph. Node represent the classes and edges represent inheritance, composition and association. Edges are labled I (sublcass), C means composed origin composes instances of destination, A for assocation where origin assocates with destination.

### Class Firewall CFW()
The set of classes that could be affected by a change to class X, which means they should be retested when testing for class X. Assuming the ORD is not modified by the change **kung** argue that this means testing association, compostion, inheritance.

#### Class firewall derivation (à ma sauce)
Keep adding classes that could be affected by classes affected by X and so on. Known as fixed point algorithm, it operates on some set, start with the empty set, apply the algorithm on each element with the empty set, then try again with the new set. Until the new set no longer changes.

### Test Partial order
The desirable trait is to test independent classes to minimize stubs. Then test the dependent classes based on their relationships. Subclasses tested after their superclass, test class C first if D depends on C.  

The reason why it is partial is that sometimes it is not possible to order every class in a specific order.

### Integration for Acyclic
Then no stubs are required when testing with partial order we still may have several classes at the same level. We can use the topological sorting.

### Dynamic binding
Class F associates with G which is a parent of H. Then the behavior of F can change if it is associated with G or H. So now we should test F with H as well. This is represented by a * to represent dyamic cases. In the follwing maner next to the tree: F/H = F*. We then include that in our dependency tree.

### Abstract class
It is infeasible to test for an abstract class with a dependency on it as it cannot be instanciated. As such if we have A as abstract class, B depends on A and E inherits form A yet E depends on B. Then we right it this way E(A)-><-B to show the new relationship. This is represented as a merge in the graph. This can lead to nodes containing 2 classes.

### Cyclic ORDs
We have a cluster (strongly connected components in graph theory). We need to break these cylces and this is done through the removal of edges.

#### Which edge to remove?
We should remove association edges, theorem says there should be at least one association classes in the cycle. Select the association the leads to the developement of the smallest stub poosible. At the end of the tree used to perform the test cases we write all to show that we have tested all real code without the stubs.

Where the cycle is broken can change the test tree.

### Type that are represented but not subclass, assocation and composition
Factory method, passing object as a parameter, creating object in a method. These dependencies should be associated by an edge in the dependency graph.

# Final info
- No multiple choice
- Question may include things in tutorial, such as what is a paramtrized test and so on but nothing really language or grammar specific.
- Expect exercises which are not very far from what we had to do in assignments or in the project deliverable. More likely assignments.

---
# 07-11-2018
# System testing
Broken into many regions:
- Functionality and usability
- Performance and stress
- Configuration and compatibility
- Security
- Reliability

Security and usability may however be tested on lower testing levels.

## Acceptance test
User acceptance test where the test is done by the customer verifiying the system works according to its specification.

### Alpha testing
Perform on a system that may have incomplete features, performed by an in-house testing panel including end users

### Beta testing
end user testing performed in the user environment. (take operating condition of the software system and ask user to play arround with it).

## System level functional/black-box testing
Ensure that the functional requirements are met by the system.

### Use cases derived scenarios
1. Develop a scenario graph
2. Determine all possible scenario
3. Analyse and rank them
4. Generate test case to cover these scenarios
5. Execute test case

### Scenario graph
It is dervied from a use case, nodes represent points where we wait for a transition event to occur (can come from environment, user, system).

- There is a single starting node
- End use case, finish node
- Edge corresponds to event occuring, conditions, special looping edge.
- Scenario = path from start node to finish node.

### Test case
```
Test Case: TC1
Goal:
Scenario Reference:
SetUp
Course of test case:
Derived at a table, describe externam event and the expected system reaction.
Pass Criteria:
```

### Force Error tests
Aim to investigate that error messages are all handled and displayed appropriatly.  (message match exepcted, no grammatical error, user gets offered reasonable options for getting arround or recovering)
Source:
- List of error messages from the developers
- Interview dev
- Inspect string data in a resource file
- Retrieve information from specification
- Use utility to extract script out of binary or scripting resources

#### Flow
1. Force error
2. Check the error detection
3. Change the handling logic
4. Check the communication
5. look for further problem

### Usability testing
- Accessibiliy: can user enter, navigate and exit system with ease
- Responsivness: can users do what they want when they want in a convinient way.
- Efficiency: can users carry tasks in an optimal fashion
- Comprehensibility: can user quickly grasp how to use the system, its help functions, associated documentation (can go all the way in recording brain waves).

## Performance testing
- throughput (number of requests addressed or served per second)
- response time (the average time needed to serve one request)
- memory utilization
- input/output

1. planning phase (objectives, work load, environment, acceptable response time, what performance to collect, with a control test)
2. Testing phase
    1. Generate test date
    2. Setup test bed
    3. run tests
    4. collect result data
3. Analysis phase (machine learning testing)
4. Verify that the test objectives are met.

### Stress testing
Focus on a system behavior, near, beyond or at overloading conditions. Until the system is pushed to failure checking for graceful failures, non-abrupt performance degradation.

#### Load testing
Aimes to verifiy how the system handles a particular load while maintaining appropriate response time.

#### Durability testing
Putting the system in extreme operating conditions, dust, sudden electrical failures, voltage spikes, extrem cold, extrem hot.

### Configuration test
Check system configuration including all supported hardware configurations. Run a set of test under different configuration.

We must decide the type of hardware to be tested, 

### Compatibility testing
Which versions of hardware and connections between units work,

#### Backward/forward compatibility
Compatible with previous version, compatibility with future version.

#### Combinatorial method
Combine all possible features and come up with test cases but it is not proactical

Pairwise:
1. Identify variables
2. Identify their values
3. Identify the unique combinations of variables **one pair at a time**
4. Create the table

---
# 14-11-2018
# Security Testing
Focuses on vulnerabilities including, data, network, computing resources.

Threat modeling: to evaluate the system for security issues, identify areas that can be exploited for security attacks.

1. Assemble a Team (developers, tester, security experts)
2. Identify potential targets
3. Understand the boundaries of the system, access priviliges
4. Identify how the data flows through
5. Identify methods for security breaches
6. Identify the threat, target and counter measures
7. Rank them in order of increasing priority

### DREAD
D: Damage potential  
R: Reproducibility  
E: Exploitability  (How often is the attack a success)
A: Affected users  
D: Discovery (How easy to find the vulnerability)

## Common vulnerabilities
1. Buffer overflow
    1. Push more data than expected in order to have them execute in the system
    2. Test all bufers
    3. write special and escape characters
    4. Use safe string functions are used
    5. Enforce data size test data with data-1,+1
2. Languages Weakness: SQL injections, cross-site, scripting attack.
    1. Avoidance of injection-related issues: Input sanitizing, escape/quotsafe input (Do not pass instructions create them in an object and the pass it)
3. Back door
    1. Alternate way of accessing a system
    2. exploited for denial of service attack
4. Command line (Shell) execution
    1. HighJack exectution privileges
5. Password cracking
    1. Use password cracking tool technologies
6. Unprotected access
7. Information leak
    1. Hard code password and ID
    2. revealing error messages
    3. directory browsing

## Testing methods
### Penetration testing
Try to penetrate a system by hiering white hats

### Program forensics
Leave a trail for analysis of what happend (from security attack) to trace what happens when investigating

# Reliability testing
Requierments:
- Probability of no failure in a specific time interval
- Expected mean time failure
- number of failure over time t
- Failure intensity the number of failure per time unit at time 
t

Note that reliabilty assumes a specified environment, to make any test and results on reliability, the test conditions must be similar to field conditions.

Model how user use the software:
- Environmnent
- Type of instalation
- inputs distribution over input space.

## Statistical testing
Testing based n operatinal profiles (usage model). Select a sample from the population that will use the software to identify then test, system functions that will be used the most.

1. Dev of the usage model
    1. Possible usage of software in different conditions
    2. Can be represented as transition graph with the nodes as states
    3. Identify what and who can initiate the operations
    4. Occurence rate of a specific operation happens per hour
    5. Occurence probability: to define how often future usages occurs.
2. Random selection/generation of tests
    1. We have a transition model traversing it guided by probabilities (random walks)
    2. Each test starts at a start node and ends at the exit node.
    3. Random generator is used at each stage to determine the next stimulus.
3. Analysis
    1. Results in Markov Chains
    2. Estimate long-run occupancy of each state, the usage profile as a percent of time spent in different state
    3. Occurence probability: occurence probability of each state in the software
    4. And many more see slides

### Advantages
Establish reliability data trying to predict what will happen in the future based on what happened in the past.

### Disadvantages
Not applicable to safety-critical system as very high reliability requ may take years to be completed.

## Mutlitasking testing
Investigate simultaneous execution of mutlitple tasks such as deadlock, starvation, race condtion.

The test reproducibility is is not garanteed run multiple tests

Logging can help detect problems.

## Recovery testing
Ability to recover data from failures and exceptional conditions in hardware and software or peaple.

## Installability testing
Installing the system on different machines, with different configurations.

# Test automation
## Brian Marick's matrix

## Test pyramid
Start with unit testing automated and as we go up we automate less and less

---
# 16-11-2018
# Gray-box testing
## Testing VS formal verification
||Testing|Formal Verification|
|-----|------|-------|
|Scope|one execution of the system trace|All possible traces of the system|
|characteristics|can detect errors|cannot prove the abscence of errors|Can prove abscence of errors and detect errors|
|Cost|Expensive to design cheap to execute|expensive for both|

## State based testing
### Grey box testing
We have a limited knowledge of the internal part of the system.
- Architecture model
- State model
- UML desing model

The coverage is based on the models.

## State model
- concrete
    - Combination of attribute values of attributes
    - Can be inifite
- Abastract state
    - Capture by a predicate (over concreate state)
    - One abstract state may represent many concreate state.

```
bool overdrawn_Inv() {
    return (balance<0); 
    }
```

### State machine model
We have states and transition between states caused by events.
There is an initial state denoted by a node and an arrow pointing to the start state. Rectangles represente a composite state containing multiple states, arrows represent the transition between states.

The same machine can be used for code generation and test models. 

- This model is used to generate either generate code for the product or use it to generate test cases
- It is bad practice to use both cases in the same model. As we loose independence between the test cases and the implementation.

### State based testing
Tests are derived from a test model

State machine execution depends on input events and data

- Executability: find data to execute the test sequence which can be hard
- Scalability (there may be a large number of concrete states)

##### Fault model
- Transfer fault (missing or incorrect transtion based on a certain input)
- Output fault (incorrect output)
- Corrupt state (Based on a correct input the system creates a system that does not extis)
- Sneak path (machine accepts a valid input that is illegal for a state or unspecified)


## Strategies all states
**During the final we will be told which Strings are actions and which are events**
### All states
Testing passes through all states

### All events
Testing passes through all events (method call)

### All actions
Extra actions done when an event is launched

### All transtions 
All edges have been gone through.  
Implies all states, events, actions coverage

### N+ Test strategy
- All state control faults
- All sneeak paths
- Many corrupt state

1. Derive round-trip path tree from state models (flattened)
2. Generate round trip path test cases
3. Generate sneak path test case
4. Senzitive transition in each test case

### Round-Trip path
Remove concurrency and hiarchy from the state

Initial state is the root node of the tree, an edge is drawn for every transtion out of the intial node, with new leaf ndes representing resultatn states

A leaf node is marked as terminal if the state it represents has already been drawn or is final.

No more transition happen at the terminal node loops are executed only once.

Tree struct
- Depends on the order transition are traced
- depth first search yields fewer, longer test sequence
- The order of the tests is suppose to be irrelevant
- Used to find sneak path, check conformance to explicit behavior model.

#### Tests
- Each test case begins at the root and ends at the leaf node, each path produces one test case.
- The oracle is the sequence of state + their ouput with their corresponding actions assuming all of them can be observed
- When running the test we set the object to the initial state then check all the states, all transition and final state.

---
# 21-11-2018
# Formal verification
Prove or disprove the correctness of asystem with respect to a formal specification relying on sound mathematics.

**Sneak pathes are not in the final**
# Model checking 
Exhaustively enumerat all the possible states and transitions of the sysrem and check if it meets the requierment. 

For formal and model checking we need a formal language to capture the system model.

## Formal state based modeling languages
### kripe structure
Contains states, initial state transitions and labelling. 

#### Labelling
Assigns to each state a set of atomic propositions (some predicates that must hold at that state, like a light that must be turner ON or OFF depending on the sate).

#### From state machine to kripke
The state must be flattened then then we encode the data variables into the system but this can result in an exponential number of states.

### State machine composition
We build state charts independently before but we have to compose them together, so we instentiate them and connect them between each other with synchronous reactive or asynchronous reactive.

### Textual property specification
May be ambigous such as the light intensity should be high (what does high really means?).

### Temporal logics
- Express behavioral requirements
- Order and occurrence of states
- Logical time

Can be done in a linear or branching tree diagram. Representing a sequence of states starting somehwere and finishing somewhere.

### Linear temporal logic
We have a cyclic execution so infinit execution, in our case we interpreted over infite paths a linear path since the number of pathes is still finite.

### Formulas for temporal logic
- We use the proposition for the states
- boolean connectivity
- temporal connective
    - X p: p holds in the next state
    - F p: p holds somewhere on the subsequent path or now
    - G p: p holds on all states
    - p U q: q holds somewhere in the future and until then p holds

We can combine them such as GF p (p can be reachd again at any stage) and so on, p can be a boolean or a regula atomic predicate.

### Computation tree logic CTL
Here we no longer interpret such things over a path but over a tree.
Here we add the following logic:
- E p: there exists at least one path where p holds
- A p (for All): for all paths property p holds

## Model checking
Take the real system and then covert it to a formal model (state machine, formulas, state macines) as well with formalized properties (temporal logic, reference automata).
Then we use a model checking algorithm to ensure that the formal model meets the property it outputs either true or a counter example.

### Model checking algorithm
#### Explicit CTL model checking
Assume that all states are initially labeled by atomic propositions. We then label the complex formulas until no more changes are seen by the system. Look at a state and its atomic predicates then label the other states backwards based on what is seen. We are going backwards.

---
# 23-11-2018
# Git repo with testing
Using repository mining to search in history where defects are likely to appear, 

## Identify risky changes
Traditional defect prediction are trying to guess where the bugs of the future will be. This is important for the QA team as they need to find the defects.

Usually we look at the release with pre and post release bug then we can label the modules of the software as either bug potentilal or if the module seems safe. Then we pass them to an algorithm with the updated system for release 2, then the model compares the second release with the first and gives an insite of where the bugs will be.

These models have some draw backs
- modules are not always well distributed (some modules are bigger than others)
- Defects are difficult to locate even when we know what module might be the most affected.
- Knowing what a dev coded for a long time period may be hard to recover


## Identifying futur bugs
Use the commit links that fixes bugs to the issue tracker, when the bug is fixed we can use the history to see what commits fixed the bug. Then we can also go further back and see who introduced the bug as well. 

This is the just in time model, we retreive all these data and thesse comits and then we give it to our algorithm. Then when new patches are arround we pass the patches to the alg and based on the data the alg it will tell us which patch is most likely to cause bugs. This approach can also be done live as now when a commit is pushed we can scan it instently. This information can also be passed to code reviewers. 

## Exprertise identification
Different people can code different modules so the code for a module may overlap for different teams. We can assume that the team that coded the most code is the main contributor. Some studies have shown that minor contributors cause the most bugs. If we have a module with weak owner ship (lot of small contributions0 it is very likely that it contains a lot of bugs. But in today's world the only way to know a module is to push to it but we must not forget the reviewing process that alos matters as someone that pushed everything to a module may actually be reviewed by a minor contributor thus making that contributor a major expert in that module. A technique could be to create a grid where we place a Dev based on a module on the quantity of commits to it and reviewing. Then we can create a graph that traces the number of minor reviewer and authors per module. We would then expect the that graph would be going more upwards for the ones with a lot of bugs compare to the one with less bugs. Then we can see if there is a significant difference between the 2.

After all these stats the theory has been confirmed therefor now when we modify some lines of code we can use this data to find the person that knows the most about the module to review the code. The issue though is that if there is only one expert per module if that person were to leave then we would loose a lot of expertise on that module.

## Preempting legal issues
Reusable components are released under different license terms which plays a big rule on how to release a software.

So first we need to know which areas of the source code are actually being used in the final product. Then we see if they are static or dynamically linked to the software 

**static**: the librairy is in the product.  
**dynamic**: the user must get a shared file with the librairy to use the product.

Processing such files is a big problem. We use the trace log for that we can see what area of the code are using what in the executable. Ending up producing a graph. Then we run another tool in all of the source file and then we can see what part of the source file need what licence. Then we can see if different modules have conflicting and different licence.

---
# 28-11-2018
## Model checking LTL
First build the system automaton, then move the holding values to the edgs, build the expected automaton from the requierements take its negation. Then we fuste the two automatons together

## Abstraction
Model checking must be abstracted as building one could be exponential. We need algorithm to abstract them, this can however caused false alarm counter example so we have to go through abstraction requierments. So we need to have abstraction requierments.

Take the model, then abstract it generate the counter examples so then go back to the original state and see whether that counter example holds, if it does not then redifine the abstract state in order for the counter example to no longer holds.

# more for our interestest than for the final
# Model transoformation 
This is the regular V model

- Critical systems design
    - certification process (safety standards)
    - Must develop justified evidence that the system fills in the requierments
- Software tool qualification
    - certification creadit for a software tool used in critical system design.

## New idea
Create a virtual prototype of the system/product and test it virtually.  

### Used for model based system engeneering
Early validation of systems model, automatic source code generation. We can check the design guidelines as we build and then we can have a counter part for validation connect the 2 together for testing.

This allows tracability as well as the code is generated to meet a specific aspect of the requierments we can recover it?

The issue may lie in the fact the that tools used to generate code or test to be eronous then we could imagine that the original model was fine but the code generating no longer is.

## Verification and validation challenges
What validates the validator?

Dev tools are the ones that generate and introduce issus in the system. Veification tools may fail to detect issues.

One method is to do a tool qualify product that ensures that the tools above but it is very expensive as it requieres a lot of work to verify that the tools used are themselves valid and fill in the requierments.

# Exam preparation
36% of the grade 
10 conceptual question 30%, 7 practical exercises 70%

## Conceptual
Compar A to B, define a concept and give example, concept covered in tutorial (i.e parametrized test), no technology oriented question, no multiple choice question. 

## practical question
- quality assurance and testing basics
    - no practical exercises
- static analysis
    - code review
    - Detecting anti patterns by graph pattern 
    - Abstract interpretation