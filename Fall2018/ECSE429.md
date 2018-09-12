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