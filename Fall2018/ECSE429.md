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