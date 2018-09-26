# COMP 330 Theory of computation
---
# Lecture 1 05/09/2018
## Course Info
Office hour M 1:30-3:00 Wed 11:00-12:30 105 N McConnel  
Assignment 30%  
Midterm 10% open notes
Final 60% open notes

## Relations
``Relation = correspondence between to different set``. 
Here where one element is twice the size of the other

|||
|---|---|
|2|4|
|1|2|

$$R \subseteq A \times B$$  
Not every element of A need to be related to anything in B. A given element of A could be related to exactly one element in B we get a fonction  

### Equivalent Relations
A function is bijective the rest are all relations  
$$R \subseteq S \times S$$
then
$$\forall  s\in S \ sRs \\
\forall s,t \in S \ sRt \implies tRs \\
\forall s,t,p \in S \ sRt  \land tRp \implies sRp$$

#### Example
$$ n \equiv m (\mod  5) \ n,m \in N$$
Give an equivalent relation R, an __equivalence class__ of an element x
$$ [x] = \{y | xRy\} \\
[2] = \{2,7,12,...\} \\
[2] = [7]$$

#### Fact
2 different equivalence classes cannot have any element in commun.

---
# Lecture 2 07/09/2018
## Partial Order
It is called this way because it might happen that 2 elements cannot be compared.

### Binary realation 
We use this symbole 
$$\leq$$  
#### axioms:
$$ \forall s (s \leq s )\\
\forall s,t((s \leq t \land t \leq s) \implies s = t) \\
\forall s,t,r(s \leq t \leq r \implies s \leq r)$$  
If every pair can be compared then we call that a __total order__
$$ \forall s,t (s \leq t \lor t \leq s)$$

##### Ex1 Partial order set
$$ \{ a,b,c\} \\
\{ \phi, \{ a\}, \{ b\}, \{c\},...\} \\
\{a\} \subseteq \{a,b\} \subseteq \{a,b,c\} \\$$  
But no relation between  
$$\{a,b\},\{b,c\}$$  

##### Ex2 Total order
$$\mathbb{N},\leq \{0,1,2,3,..\} \\
\mathbb{R},\leq \{-1.222,4.33\,...\} \\
\mathbb{Z},\leq \{-1,0,1,2,3,...\}$$

##### Ex3 partial order set
$$ f: \mathbb{R} \rightarrow \mathbb{R} \\
f \leq g = \forall r \in \mathbb{R} \ f(r) \leq g(r)$$
This is true since certain function are not always lower than the other.

### Well-Founded order
#### Prelude
x is minimal means there is nothing strictly smaller than x.  
x is least means x is smaller than anything else.  
We have a set S with strict ordering consider the subset U of S. Least element are always minimal.
We say x is minimal in U if: 
$$x \in U$$
$$ \forall u \in U,  \neg (u < x)$$

We say x is least in U if:
$$\forall u \in U, x \leq u$$ 

### Well foundedness
We have a set (W, ≤)  
For every non-empty subset 
$$ U \subseteq W \land U \not = \emptyset \\
\exists u_0 \in U$$
Such that uo is least

##### Example 4
$$\mathbb{N}, \leq$$
$$\mathbb{R}, \leq$$
The first one is well founded the second one is not.  
A well founded total order is called a __well order__.  
If an order is not __well founded__ there are infinitly long strictly descending sequences.

## Induction
Induction works if and only if the order is well founded
### Proof
Predicate P(.)is a property (is green) is a predicate as by itself it does not express a complete sentence.  (S, ≤)
S supports induction if:
$$ \forall P(.) \\
(\forall x \in S (\forall y \in S, y < x \implies P(y)) \implies \forall x \in S P(x))$$

### Theorem
An order is inductive if and only if it is well founded  
Proof:  
Assume  (S, ≤) is well founded. Suppose P(.) is any predicate then suppose:
$$\forall x \in S(\forall y \in S y < x \implies P(y)) \implies P(x)$$
Let 
$$U = \{x \in S| \neg P(x)\}$$
But we know that U has a minimal element since S is well founded and every non empty subset has a minimal element so we assume U is not empty and has the minimal element u0. 
$$\forall y \in S, y < u_0 \implies [y \not \in U \iff P(y)] \\
\forall y \in S y < u_0 \implies P(y)$$
Then
$$ \forall x \in S P(x)$$

---
# Lecture 3 10/09/2018
## Language Recognition
First an alphabeat must be described and we use
$$\Sigma = \{0,1\} \\
=\{a,b,c\}$$
To represent such alphabet, which is just a finite state of symbols.  
Hence from the example above a word could be abba, acba, aaaabbbababa, , .

The empty word is very important so we must have a symbol for that which in our case will be:
$$\epsilon$$
The collection of all words constructed from 
$$\Sigma$$
Is written:
$$\Sigma^*$$  
It is interesting to know that the collection of all words is an infinite set, a given word is finite and so is the alphabet.

### Language
$$ L \subseteq \Sigma^* \\
L = \emptyset \\
L = \{\epsilon\} \\
L = \{a,ab,aba,...\} \\
$$  
L may be finite, we need a notation for describing languages and we need alogirthm to check if a given word is in a given language.

Ex:
$$ L = \{w | equal \ number \ of \ a \ and \ b\}$$  
Algorithm for checking count the number of a and number of b and check if there are equal.

## Deterministic Finite Automaton (DFA)
Is a machine which will react to the imput, the imput will be a word and decide if the word is in the language.     
A DFA is a 4-tuple:
1. S: a finite set of state
2. Initial state
   $$s_0 \in S$$
3. Transition Function F (start at one state get a letter in the alphabet new state) 
   $$\delta: S * \Sigma \rightarrow S$$
4. Accepting states (state where the machine returns true)  
   $$F \subseteq S$$

Now what about when we use a whole world then we get
$$ \delta^* : S*\Sigma^* \rightarrow S $$
By induction:  
$$ 
\delta^*(s,\epsilon) = s \\
\delta^*(s,aw) = \delta^*(\delta(s,a),w)
$$

__The machine will be represented using the letter M__

$$
L(M) = \{w|\delta^*(s_0,w)\in F\}
$$

### Drawing states
* States are circles
* One arrow comes from the outside and hits a state to represent the initial state.
* Other arrows go from state to state to describe the transitions, in our case these arrows have to be lable in the state of my alphabet
* Accept state are shown by double circles

## Language and machine
The biggest language is  
$$\Sigma^*$$  
The machine that accept such a language is a machine that accepts all the words in that language. It is hence important to reject words that are not in the language.

A language recognised by a DFA is called a __regular language__. Every state must have an arrow for every letter of the alphabet.

Imagine a combination lock and the answer is 4104  
$$\Sigma = \{0,1,2,3,4\}$$  
Then the machine is quite simple to draw to get to the accepting state but gets more complicated when we want to include the dead state. As a convenience if we are in a state and there are no arrows for it then we can reject it.

#### Proposition
* Every finite language is regular (we can build a machine that will recognize all the word of that language).
* It is possible to have states that cannot be reached.

---
# Lecture 4 12/09/2019
## Turing machines continue
It is important to realize that states encode information. 

### Example accept any number divisible by 3
Then we must keep track of the value of mod 3 with the number. But the machine here sees each digit one by one lets call the number the machine has seen  x then if we add a 0 after x then we have doubled the size of the number so we get 2x if it is a 1 then we get 2x+1. However we can just keep track of the reminder:

r = 0 and we see a 0 then the number has doubled but the reminder is still 0.  
r = 0 and we see a 1 then the number has doubled+1 so the new reminder is 1  
r = 1 and we see a 0 then the number has doubled so the reminder is now 2  
r = 1 and we see a 1 then the number has doubled + 1 which gives us a r = 0  
r = 2 and we see a 1 then number has doubled + 1 which gives us a reminder of 2  
r = 2 and we see a 0 then the number has doubled which gives us r = 1.

### Proving that the example above requires at least 3 states
Suppose that we have a machine with only 2 states and we claim that it can recognise numbers divisible by 3.  
Since we have 2 states then one must be an accept state and the other must be a reject state.  
Now lets consider 3 different string 100, 101, 110. So we get rejected, rejected and accepted. This means that 100 and 101 go to the same state. So after readding 100 we end up with the same state than 101. This implies that 1001 and 1011 must go to the same state since we have ended on the same state before but one is divisble by 3 the other one is not so we have a contradiction no such machine is possible.

## Non-Determininistic finite automata
Design a machine to accept all words that end in aa with an alphabet {a,b}, this can be done with a machine that can predict when the last 2 a's are going to come and go to specific states, so we have 2 arrows for a in the transition but the machine will know when to take each. Also we do not need to mention all the arrows for that machine, if the machine jams then it simply rejects.

There is an algorithm which converts ands NFA into an equivalent DFA. Here equivalent means recognises the same language. The algorithm is pretty much back tracking

$$NFA+\epsilon$$
Means that the machine in certain state is allowed to change state when not reading anything the same theorem applies any such machine can be converted to a DFA.

Consiser the decimal digits:
$$ \Sigma = \{.,+,-,0,1,2,3,4,5,6,7,8,9\}$$
27. , 47.3, 202.961, +17.0, -5.61
    
We can draw a machine that checks if the number is valid note that we want numbers either in front or behind the decimal point.

---
# Lecture 5 14/09/2018
## Non determainistic automaton formal definition
Fix alphabet and   
$$ N = (Q, Q_0, \Delta,F) \\
Q = \text{Finite set of states}\\
Q_0 \subseteq Q: \text{start states } Q_0 \not = \emptyset\\
\Delta = Q * \Sigma \rightarrow 2^Q\\
2^Q = \text{the set of all subsets of Q}\\
\Delta^* = 2^Q * \Sigma^* \rightarrow 2^Q\\
A \subseteq Q,\Delta^*(A,\epsilon) = A\\
w \in \Sigma^*, a \in \Sigma,\Delta^*(A,wa) = \cup_{q \in \Delta^*(A,w)} \Delta(q,a)$$

Fact(1):  
$$ \Delta^*(A\cup B,w) = \Delta^*(A,w) \cup \Delta^*(B,w)$$

Fact(2):  
$$ x,y \in \Sigma^*, \Delta^*(A,xy) = \Delta^*(\Delta^*(A,x),y)$$

### Def of the language of the machine
$$L(N) = \{w|\Delta^*(Q_0,w)\cap F \not = \emptyset\}$$

### Theorem 1
Given an NFA N as above  
$$ \exists \text{a DFA } M, (S,S_0,\delta,\hat{F}) \text{ such that } L(M) = L(N)$$
Proof:  
$$ S = 2^Q\\
s_0 = Q_0\\
\hat{F} = \{A \subseteq Q | A \cap F \not = \emptyset\}\\
\delta(A,a) = \cup_{q \in A} \Delta(q,a) = \Delta*(A,a)$$
Now we want to show that L(M) = L(N):  
Lemma  
$$\Delta^*(A,w) = \delta^*(A,w)$$  
Proof by induction on the length of the word |w|  
Base case 
$$ w = \epsilon\\
\Delta^*(A,\epsilon) = A = \delta^*(A,\epsilon)$$  
Induction Case:
$$ \text{Assume } \forall A \subseteq Q\\
\Delta^*(A,x) = \delta^*(A,x)\\
\Delta^*(A,xa) = \delta^*(A,xa)\\
\delta^*(A,xa) = \delta(\delta^*(A,x),a)\\
= \delta(\Delta^*(A,x)a)\\
= \Delta^*(\Delta^*(A,x),a)\\
= \Delta^*(A,xa)$$
Proof of the theorem concluded:
$$ L(N) = \{w|\Delta^*(Q_0,w) \cap F \not = \emptyset\}\\
=\{w|\Delta^*(Q_0,w) \in \hat{F}\}\\
=\{w|\delta^*(Q_0,w) \in \hat{F}\}\\
=\{w|\delta^*(s_0,w) \in \hat{F}\}\\
=L(M)
$$

---
# Lecture 6 17/09/2018
## Regular expressions
Algebra with terms defined inductively  
$$\Sigma \text{ Alphabet }=\{a,b\}\\
 \phi \\
 \epsilon\\
 \text{any letter from }\Sigma \text{ is a regular expression }\\
 R,S \in RE \implies R+S\\
 R,S \in RE \implies R.S\\
 R \in RE \implies R^*
 $$

 Examples:
 $$ ab+\epsilon\\
 (a^*b)^*\\
 a^*+b^*\\
 aa^*\\
 \phi
 $$

 An RE describes a subset of  
 $$ \Sigma^*$$  
The empty set is defined as  
$$ \phi$$
Then  
$$R+S := \{w\in R\} \cup \{w\in S\}\\
R.S := \{w_1,w_2| w_1 \in R, w_2 \in S\}\\
R^* := \{w_1,w_2,w_3,...,w_n| w_i \in R\} \cup \{\epsilon\}$$

Example:  
$$ (a^*b)^* = \{\epsilon,ab,abab,aaab,abaab,b\}$$

### Theorem
Every regular language can be recognised by regular expressions,any language defined by a regular expression is a regular language.

#### Idea of the proof DFA --> RegExp
Enumerate the states: {1,2,3,...,n}  
Then define a family of regular expressions, we look at the machine and only allow only specific movement through the machine, then our goal is to decrease these movements by converting them to a regular expressions. 

Familiy of regular expression
$$ R_{i,j}^k: \text{ Describes all paths that take the machine
from state i}\\
\text{to state j but only passing through states numbered k or less}$$
When k=0 we are not allowed to stop anywhere so there must be a direct transition between i and j.

Compute
$$ R_{i,j}^k \forall (i,j)$$

Induction
$$R_{i,j}^{k+1} = R_{i,j}^k+R_{i,k+1}^k(R_{k+1,k+1}^k)^*R_{k+1,j}^k$$

### Equations between regular expressions
\+, .  
$$R.(S+T) = R.S+R.T\\
R + \phi = R \\
R+(S+T) = (R+S)+T\\
R+S = S+R\\
R+R = R\\
R.\epsilon = R = \epsilon.R\\
R.S \not = S.R\\
R.(S.T) = (R.S).T\\
\epsilon+RR^* = \epsilon R^*R = R^*
$$

---
# 20/09/2018
## Minimization of DFA
Dfa have a unique minimal form. Define an equivalence relaton of states.

Use equivalence classes to define new smaller machine.

Def: Given a DFA 
$$ M = (S,s_0,\delta,F)$$  
over alphabet we say
$$ p,q \in S $$
Are equivalent, and write pxq, or: 
$$ p \approx q$$
if
$$ \forall x \in \Sigma^*, \delta^*(p,x) \in F \iff \delta^*(q,x) \in F$$
Remark when not equivalent we get:  
$$  
p \not \approx q\\
\exists x \in \Sigma. (\delta^*(p,x) \in F \land \delta^*(q,x) \not \in F\\
\lor \\
\delta^*(p,x) \not \in F \land \delta*(q,x) \in F)
$$
We are going to write [p] for the equivalence class of p

Lemma A:
$$
(p = q) \implies \forall a \in \Sigma, \delta(p,a) \approx \delta(q,a)
$$

Proof:
$$
\delta^*(\delta(p,a),x) \in F\\
\delta^*(p,ax) \in F
$$
By assumption then pxq also takes us to an accept state.  
We know that 
$$ \delta^*(q,ax) \in F \\
\delta^*(\delta(q,a),x) \in F
$$

Defining the new machine now:
$$
S' = \text{ equivalen classes of S } (S / \approx)\\
s_0' = [s_0]\\
\delta'([p],a) = [\delta(p,a)] [\text{ Well defined  }]\\
F' = \{[s]| s \in F\}
$$

Lemma B
$$
p \in F \land p \approx q \implies q  \in F\\
$$
Lemma C
$$
\forall w \in \Sigma^*, \delta'^*([p],w) = [\delta^*(p,w)]\\
\delta'^*([p],\epsilon) = [p] = \delta^*(p,\epsilon)]\\
\text{Hypothesis step}\\
\delta'^*([p],w) = [\delta^*(p,w)]\\
\text{ Induction step}\\
\forall a \in \Sigma, \delta'^*([p],wa) = [\delta^*(p,wa)]\\
\delta'^*([p],wa) = \delta'(\delta'^*([p],w),a)\\
= \delta'([\delta^*(p,w)],a)\\
= [\delta(\delta^*(p,w),a)]\\
=[\delta^*(p,wa)]
$$

### Theorem
L(M) = L(M')  
Proof in the notes by direct calculation.

Now we want an algorithm to do this.

### algorithm
Start by saying that there is only one state and they are all equivalent.  
Then we keep track of all the pairs and any pair such that one is an accept state and the other not an accept state shall be marked not equivalent.  
Then we carry one with new pairs and look weather a certain letter brings us to an accept state if they do then we do nothing if they do not then we mark these sets as not equivalent.

## Distingishibility
$$ p \bowtie q \implies \exists w \in \Sigma^*\\
 (\delta^*(p,w) \in F \land \delta^*(q,w) \not \in F \lor \delta^*(q,w) \in F \land \delta^*(pw) \not \in F)
$$ 
Here we can see that these two states are distingishible

#### Fact
If 
$$ \exists a \in \Sigma\\
\delta(p,a) \bowtie \delta(q,a) \implies p \bowtie q
$$

### Algorithm 
$$
S \times S \text{ array of bool}\\
\forall \text{ pair } (p \in F \land q \not \in F) \text{ put a 0 at [p,q] position}
$$
Repeat until nothing changes anymore:  
For each pair [p,q] not marked 0 check if
$$ \exists a \in \Sigma(\delta(p,a),\delta(q,a))$$
is marked 0 if yes then mark [p,q] with a 0.

If two states are not labelle 0 by the algorithm then they are equivalent.

### Proof of correctness (not complete but the idea is there)
Assume that there exists a pair of states that are not marked yet they are not equivalent. Call such a pair a bad pair. Now consider the word w where for one of the state it goes to an accept state and the other a reject state.  
Conisder a shorter version of w that leads hence to earlier state. Thes states must be marked as non equivalent since ulimatly there exits a string w that leads them to different state. Hence if these two states have been marked as non equivalent, it is not possible that there next states are also marked as equivalent since they do not lead to equivelent state.

---
# 21/09/2018
## MyHill_Nurode theorem
Notes on the website.
### Right inveriance
R is an equivalence relation on strings/word if it has the following additional property.
$$
xRy \implies \forall z,xzRyz
$$
We say R is right invariant.
$$
M = (Q,\Sigma,q_0,\delta,F)\\
\delta^*:Q \times \Sigma^* \rightarrow Q
$$
A word defines a fonction from Q to Q
$$
w \in \Sigma^*\\
q \rightarrow \delta^*(q,w)\\
xR_My \iff \delta^*(q,x) = \delta^*(q,y) \forall q
$$
This is right invarient.

$$ 
L \subseteq \Sigma^*\\
xR_Ly \iff \forall z, xz \in L \iff yz \in L
$$
Easy to see that it is right invarient.

Hence we have 2 examples of right invarient equivalent relations. R_M and R_L
TFLAE

Now we can prove the **Myhill_Nerode** theorem
1. The language L is accepted by a DFA
2. L is the union of some of the equivalence classes of a right invariant equivalence relation of finite index
3. R_L has finite index and indeed has smaller index then any relation R of the type mentioned above.

**Index** = # of equivalence classes of a relation
The entire set fits in at least one equivalence class. The entire divided subset is called a partition.

Each equivalence class can themselves be **refined** meaning divinded again into sub equivalent classes.

### Proof
$$
(1) \implies (2)\\
M = (Q,q_0,\delta,F)\\
R_m \\
S_q := \{x \in \Sigma^*|\delta^*(s_0,x)=q\}\\
L(M) = \cup_{q \in F} S_q
$$

Here R_L has bigger blocks than R which means that R_L is the minimum index. R is also right invariant
$$
(2) \implies (3)\\
R \subseteq R_L\\
xRy \implies x R_L y\\
\text{Assume } xRy \ \& \ z \in \Sigma^* xz \in L\\ 
\text{Then xz is in one of the equivalence classes of R}\\
\implies yz \in L
$$  

$$
(2) \implies (3)\\
\text{We construct a machine as follows}\\
S = \Sigma^*/R_L\\
s_0 = [\epsilon]\\
\delta([x],a) = [xa] \text{ well defined}\\
x' \in [x], [x'a] = [xa] \\
F = \{[x]|x \in L\}
$$
The machine we just constructed is the unique smallest machine.

There is no such theorem for an NFA, p space complete.