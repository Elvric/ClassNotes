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