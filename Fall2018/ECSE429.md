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

### Nested loops