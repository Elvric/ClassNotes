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
R.S := \{w_1w_2| w_1 \in R, w_2 \in S\}\\
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

---
# 24/09/2018
$$ L=\{a^nb^n|n \leq 0 \}$$  
Suppose that M recognises L, so there exists a DFA with k states.  
So now I am going to get a string in the language where n>k.
This means that as I travel through the machine that means that somewhere I must visit the same state 2 (Pigeon hole principal). So we have gonne along a loop now consider the loop has length l. Which means that the machine can only count n-l and my b's number = n. that means that the machine accepted n-l a and n states which is not possible this is called the pumping lemma

## Pumping lemma
If L is a regular language then:
$$
\exists p > 0 \text{ s.t } \forall w \in L \text{ with } |w| \geq p\\
\exists x,y,z \in \Sigma^* \text{ s.t w } = xyz \land |xy| \leq p \land |y| >0\\
\forall i \in \mathbb{N} \ xy^iz \in L
$$

If a language is regular then it can be pumped, if a language is not regular then it cannot be pumped

L regular implies that L can be pumped. L can be pumped does not implie that it is regular. So it can only be used to show that things are not regular.

## Contrapositive of pumping lemma
$$
\forall p > 0 \exists w \in L, |w| \geq p\\
\forall x,y,z \in \Sigma^*, w = xyz \land |xy| \leq p \land |y| > 0\\
\exists i \in \mathbb{N}, xy^iz \not \in L
$$

L may be hard to pump. If R is a regular language and L is a regular language then
$$ R \cap L$$
is also regular.  
Now take some R that can easily be seen to be regular and consider and L that we are not sure above than if 
$$ R \cap L$$
is not regular then so is L.

### Example
$$
L = \{a^mb^n|m \not =  n\}\\
\overline{L} \text{:L complement in this case}\\
\overline{L} \cap a^*b^* = \{a^nb^n | n \geq 0\}
$$
Hence it is not regular as we have show that aboves

## Languages that are not regular:
$$
\{a^nb^n | n \geq 0\}\\
\{a^mb^n | n \not = m\}\\
\{ww | w \in \Sigma^*\}\\
\Sigma = \{a\}, \{a^{2^{n}} | n \geq 0\}
$$

---
# 26/09/2018
### Proof a irregularity
$$
\Sigma = \{1\}, \{a^{2^{n}} | n \geq 0\}\\
$$

$$
p\\
2^m > p\\
p\geq|y|>0;|y| = k\\
i =2\\
1^{2^m+k} = \text{ new pumped String}\\
2^m<2^m+k \leq 2^{m+p} < 2^m+2^m = 2^{m+1}
$$
Hence we are strictly between 2 consecutive powers of 1 so it cannot be a power of 2.

### Proof on a another irregular language
$$ L = \{a^q | q \text{ is a prime number}\}\\
p\\
a^n, n \geq p\\
y = a^k; p \geq k > 0\\
i = n+1\\
a^{n-k+k(n+1)}\\
n-k-k^2+kn\\
n(k+1)\\
$$
Which is not prime.

Important to realise that the length is 
$$ n(i-1)k$$  
Is the length of the String after we apply i to it.

### Proof on a new language
$$
\Sigma = \{a,b\}\\
L = \{w \in \Sigma^* | \#a(w) \not = \# b(w)\}
$$
\#a(w) = number of a's in w.  
We are going to prove this using what we learned above.
$$
\{a^nb^n|n \geq 0\}\\
\overline{L} \cap a^*b^* = \{a^nb^n|n \geq 0\}
$$
Hence we know that this is not regular as the complement plus the other one is not regular.


### Proof an a rather new again language
$$ L = \{a^ib^j|\gcd(i,j) = 1\}\\
L' = \overline{L} \cap a^*b^* = \{a^mb^n|\gcd(m,n) >1\}
p\\
q > p+1 \text{ s.t q is prime}\\
a^qb^q \in L'\\
a^k\\
i = 0\\
a^{q-k}b^q
$$
q is a priem and q-k is less then q hence there gcd must be 1 therefore it cannot be pumped.

#### Proof with MyHill-Nerod
$$ x \equiv_L y : \forall z, xz \in L \iff yz \in L $$
If I exihibit infinity many different equivalence classes L cannot be regular. Consider all p primes
$$ p_1 \not = p_2 \implies a^{p_1} \not \equiv_L b^{p_2}$$

---
# 28/09/2018
## Does a system do what it is suppose to do?
Usually somebody right down a **SPEC** written in natural language which can be vague. To get a better feel for such models we want to write some logic. The logic defines a behaviour which is just a system of actions.

## Model Checking
We can check system with
$$ 10^{600}$$
States can be checked.

### Famous pentium bug
Was a mistake in the design of a chip in 1994. Which because of the design did mutliplication incorrectly. Mistakes only appeared when multiplying very big numbers. Intel lost millions of dollars with new slogan such as "Intel redefining mathematics!".

### Questions
Are sequences of actions good enough to capture all aspects of behaviour?

### Vending machine
$$ \Sigma = \{1\$,COF,TEA, \overline{COF}, \overline{TEA}\}$$
Actions may be rejected compare to our DFA that would read every symbol we gave them. Systems may be undetermintate. i.e we do a certain action and what the next state is, is not defined. 

Sequences do not acapture all ascpects of behaviour. (by sequencing we mean steps throught the machine). We need to capture branching!

## Def Bisimulation An equivalence relation on states 
$$
s \sim t\\
\forall a \in Action \ s \xrightarrow{a} s' \exists t' \ s.t \ t \xrightarrow{a} t' \land s' \sim t' \\
\& \forall a \in Action \ t \xrightarrow{a} t'' \exists s'' \ s.t \ s \xrightarrow{a} s'' \land s'' \sim t''
$$
Every state is obviously bisimilar to itself.

If two states are bisimilar then no interactive experiment can reveal the difference.

---
# 10/03/2018
# Specification of behaviour
The description has to encompass dynamic changes of state in time

## Temporal logic
### Linear Temporal logic (LTL)
Give me a data sequence and I will tell you something about it.

#### Examples
Concurrent programming brings the problem of competition to the table as some threads may compete for the same resources which brings mutual exclusion. Say p1 means P1 has access and p2 means P2 has access.

$$always[(p_1 \implies \neg p_2 \land p_2 \implies \neg p_1)]$$

If I request a service to a server then eventually I will get that service.

If you request a service from the server you cannot make a second request until the first one has been granted.

We need logical statments for these:

$$
\circ \text{ : next}\\
\Box  \text{ : always}\\
\Diamond \text{ : eventaually}\\
\vartheta \text{ : until}\\
\circ p \text{ next step p is true}\\
\Box p \text{ p is always true }\\
\Diamond p \text{ p will be true at some point}\\
p \vartheta q \text{ p is true until q becomes true}
$$
Here p is true until q does not means that p has to change when q but it could at that point.

Going back to our threads for mutex we get  
$$ 
\Box (p_1 \iff \neg p_2)\\
\Box (request \implies \Diamond granted)\\
\Box (request_1 \implies (\neg request_2 \vartheta granted_1))
$$
It is possible to automatically check whether a finite state satisfies the LTL specification.

#### Temporal logic on words
$$\Sigma = \{a,b\}\\$$
We can now write LTL formulas interpreted on words. A formula looks at the position in the word  
$$
Q_a \text{: this letter is an a}\\
Q_b \text{: this letter is a b}\\
w = abbababb\\
Q_a\text{:holds}\\
w \models Q_a\\
w \models \circ Q_b\\
w \models \circ \circ Q_b\\
w \models \Box(Q_a \implies \Diamond Q_b)
$$
Now consider this word:  
$$
w=ababab\\
w \models \Box((Q_a \implies \circ Q_b) \land (Q_b \implies \circ Q_a))
$$
But this is not always true the end is an edge case so we are going to introduce a new symbol for false:  
$$\perp$$
$$\circ \perp \text{: these is no next step}$$

Hence we can change our formula above to:
$$w \models \Box((Q_a \implies \circ Q_b) \land (Q_b \implies (\circ Q_a \lor \circ \perp)))$$

But the string bab is still accepted hence we have to change that to:
$$w \models \Box(Q_a\land((Q_a \implies \circ Q_b) \land (Q_b \implies (\circ Q_a \lor \circ \perp))))$$

So we may think that given a TL formula there is a set of string that satisfy that formula. Is this always a regular language? The answer is **YES**. 

But now are all regular language describable by TL. And the answer is **NO**. Consider the following:
$$\Sigma=\{a,b\}$$
And we want every odd position to have an a with reg we get
$$(a(a+b))^*(a+\epsilon)$$
For this case there is no TL formula that can express this.

First order logic is undecidable so you can give up on algorithm that is why we limit ourselves to temporal logice as we can still decide things.

---
# 05/10/2018
## Learning automata
Given a data set we want to learn the underlining structure of that data set. The hole point is to make hypothesis from a source of data.

### Passive learning
Is when your learning algorithm is given a fix and finite data set from which it is trained.

We have our data set with inputs (set of words) labeled as in the language and not in the language and we run the machine automata on these words. 

This was proven to be NP-Hard for fixed and finite sets of labeled words. 

### Active learning
The wording algorithm can ask something (teacher) information about the unknown language. The whole key to active learning is to optimaly ask questions. Based on the answer given by the teacher we want to figure out what are the best questions to ask next.

We have a minimally adequate teacher 
$$T^*$$
And based on its answer we want to find L which is represented by a DFA.

There is an algorithm created called
$$L^*$$ 
that is entierly deterministic and is not based on probability.
It can ask 2 type of questions:
#### a member ship querry
Input a word w and the teacher says either w belongs to L or it does not

#### conjecture querry
Hypothesis about the unknown language. Takes as imput the DFA build by the
$$L^*$$ 
algorithm. The teacher responds yes if the DFA is equal to the unknown language and no otherwise with a counter example.

### How do we store these information?
We are going to use observation tables that are data structure that we are going to increase in size over time to store these informations.
Considere the alphabet  
$$\Sigma = \{a,b\}$$

The observation table will look like this,
Rows represent potential state that our algorithm L* think are in the DFA and the columns are going to represent experiments performed on these states. We do not duplicate any string in the rows.

|S union S.Sigma\E| epsilon | a |
|--------------|------------|---|
| S epsilon|0|T(a) = 0|
| S a |0|1|
| S aa |1|0|
|S.Sigma b|
|S.Sigma ab|
|S.Sigma aaa|
|S.Sigma aab|T(aab epsilon)=0|

Initially we have each row and columns all equal to epsilon.  
Then we use a function to fill in every cell call that **T** represents the membership query function.

$$T((S\cup S.\Sigma).E) = \{0,1\}, T(S,E) = 1 \iff SE \in L$$

We are also going to use the row function, it takes the value of the row in it.  
$$row : (S \cup S.\Sigma) \rightarrow (E \rightarrow \{0,1\})\\
row = T(se) \forall e \in E
$$
So in a way row defines a tupple.

For the DFA we define our first state as:  
$$
q_O= row(\epsilon)\\
Q = row(s) \mid s \in S\\
F = \{row(s) \mid s \in S \land T(s\epsilon)=1\}\\
\delta(row(s),x)=row(sx), x \in \Sigma
$$
#### Example
|row(s)\x|a|b|
|--------|-|-|
|row(epsilon)=00|row(epsilon,a)=01|row(epsilon,a)=00|
|row(a)=01|row(aa)=10|row(ab)=00|
|row(aa)=10|row(aaa)=00|row(aab)=00|

Based on that table we can construct the following DFA using the states and so on which is done in polynomial time:
![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/MachineLearningPic.png)

The observation table has 2 properties:  
**Closed**
$$\forall transition \in S.\Sigma \exists s\in S \ s.t \ row(transition)=row(s)$$ 

**Consistent**:  
$$s_1,s_2 \in S,row(s_1)=row(s_2) \implies \forall x \in \Sigma row(s_1x)=row(s_2x)$$

### L* algorithm
$$
\Sigma = \{a,b\}\\
L =\{a^mb^n\mid m,n \ even\}
clumn = \epsilon
$$
Hence our first operation table will look like this:
$$S = \{\epsilon\}\\
S.\Sigma = \{a,b\}\\
column = \epsilon
$$
This table is not close so we add the following
$$S = \{\epsilon,a\}\\
S.\Sigma = \{b,aa,ab\}\\
column = \epsilon
$$
Now the table is closed and consistent.

Whenever we have thar we build a DFA from the operation table above and ask the teacher if that DFA is equal to L.  
Here the teacher responds No with the counter example bb.

We are then going to append the counter example with all of its prefixes to the observation table which give us.
$$S = \{\epsilon,a,b,bb\}\\
S.\Sigma = \{aa,ab,ba,bbb,bba\}\\
column = \epsilon
$$
But this table is not consistant with a=b=0 where aa=1 but ab=0
So we must add some columns which gives us:
$$S = \{\epsilon,a,b,bb\}\\
S.\Sigma = \{aa,ab,ba,bbb,bba\}\\
column = \epsilon,a
$$
Is this table close yes is this consistent yes

Now we create the second conjecture to ask the teacher. The teacher responds no with abb so we get the new table:
$$S = \{\epsilon,a,b,bb,ab,abb\}\\
S.\Sigma = \{aa,ba,bbb,bba,abba,abbb,aba\}\\
column = \epsilon,a
$$

The algorithm can finish in polynomial time for any regular language.

---
# 15-10-2018
# Linguistics
The idea is to generate strings. This will be done by something called a grammar.

## Context free grammar (Chomsky)
A CFG consists of 4 things.
1. A set of symbles (Sigma, T) called terminals (Analog of the alphabet)
2. Another set of symbols disjoint from the first call non terminals V (V and T is empty) V can represent Strings
3. A special non terminal usually called S in V the start symbol
4. A finite set of rules or productions of the form

$$A \in V, A \rightarrow \alpha, \alpha \in (V \cup T)^*$$

#### Example
$$
T = \{a,b\}\\
V = \{S\}\\
S \rightarrow \epsilon\\
S \rightarrow aSb
$$
**Given a grammar the set of String we can generate is called the language of the grammar.**
$$S \rightarrow aSb \rightarrow aaSbb \rightarrow aaaSbbb \\\rightarrow aaabbb$$
The language generated is hence 
$$ \{a^nb^n \mid n \in \mathbb{N}\}$$

So context free languages do not need to be regular. But all regular languages can are context free.

An example of a language that is not context free is:  
$$ a^nb^nc^n$$

``Fun fact, context free languages can be represented by a finite stat machine and a stack``

These context free language can be used to define programming languages, all of them are specified this way like java and so on. HTML was especially designed to be context free.

#### Example for legal arithmetic expressions in programming languages
< exp >=start symbol, < num >=generate numbers symbol, < nz >=non zero digits, < n >=generate digits including 0.
$$
T = \{0,1,2,3,4,5,6,7,8,9,+,*,(,)\}\\
V = \{<exp>,<num>,<nz>,<n>\}\\
S = <exp>.\\
<exp> \rightarrow <exp>+<exp> \mid <exp>*<exp>.\\ \mid <num> \mid (<exp>)\\
<num> \rightarrow <nz> \mid 0\\
<nz> \rightarrow 1<n> \mid 2<n> \mid ... \mid 9 <n> \mid \epsilon\\
<n> \rightarrow 0<n> \mid 1<n> \mid ... \mid 9 <n> \mid \epsilon\\
$$
Note that the above structure forbids us to have leading 0.

Sequences do not tell the story we produce trees!  
Let's try to generate 2*3+5 as a tree

![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/contextTree.png)

We can see in the tree that we grouped together the * together before the +. The grammar above could also generate a tree with the + linked more tightly together than the *. This is a criteria for a bad grammar. More formally the grammar is said to be ambiguous if there are two distinct parse tree for the same String.

Languages for which it is impossible to design an unambigous grammar are called inherently ambiguous languages. It is hard to prove it though.

---
# 17-10-2018
Update the tree:  
$$
V = \{<e>,<t>,<f>\}\\
S = <e>.\\
<e> \rightarrow <e>+<t> \mid <t>.\\
<t> \rightarrow <t>*<f> \mid <f>.\\
<f> \rightarrow (<e>) \mid <num>
$$
Hence that forces + to go before * in the tree order. Tree structure is important for meaning example when translating.

#### Example of context free language
$$ L = \{a^nb^{2n} \mid n \geq 0\}\\
S \rightarrow aSbb \mid \epsilon $$

L = balanced set of parenthesis
Idea:
$$ S \rightarrow (S)\mid ()S \mid S() \mid \epsilon$$

#### Palindrome context free
$$L = \{x \mid x=x^{rev}\}, \Sigma = \{a,b\}\\
S \rightarrow \epsilon \mid aSa \mid bSb \mid a \mid b $$

#### context free language same number of a and b
$$ L = \{x \in \Sigma^* \mid \text{ x has as many a as b}\}\\
d(x) = \#_b(x)-\#_a(x)\\
L = \{x\mid d(x) = 0\}\\
u=aw \in L, d(w) = 1\\
\text{Let x be the shortest prefix such that } d(x)=0, x \in u\\
\text{Last letter of x has to be a b}\\
x = a.vb\\
u = avby\\
v,y \in L\\
S \rightarrow aSbS \mid bSaS \mid \epsilon\\
S \rightarrow aSb \mid bSa \mid \epsilon \mid SS$$

##### Proof
1. Every word generated is in L
2. Every word in L can be generated
 
Claim: every word generated has equal number of a's and b's (easy to see).  
Any word with equal numbers of a's and b's can be generated.  
By induction on the number of a's.

Base case:
$$\epsilon$$

Inductive step:  Assume any String with up to n a's can be generated and consider a String with n+1 a's.  

awb, bw'a. If the whole string as equal number of a and b then so does w and w'. by doing
$$ S \rightarrow aSb \rightarrow ... \rightarrow awb\\
S \rightarrow bSa \rightarrow ... \rightarrow aw'b$$

Now suppose w begins and ends with a, w=aw'a, w' has 2 more b than a's.   
Lets look at w': must be split into two pieces w1 and w2 such that these 2 words have an extra b. So w=aw1w2a and aw1 has n a's so we can generate aw1 and w2a. We repeat the same process for b.

#### Fact
If we have a function like d which can only change by + or - one at each step and it starts out negative and ends up positive it must hi 0.

## Chemsky normal form
Every CFL has a grammar in which the rules are of a very restricted form:
$$ NonTerminal \rightarrow Terminal\\
NonTerminal \rightarrow 2*NonTerminal
$$
The only non terminal allowed to go to epsilon is the start symbol.

Note that the example above does not respect the rule but it can be done. But considering this grammar for context free language we can use it to prove things about context free language as we know that any of them can be maped to this grammar.

# 19-10-2018
# Push Down automata/ Stack machine/ DFA + 1 stack
$$\{a^nb^n \mid n \geq 0\}$$

CFL = recognized by PDA
$$
Q: \text{ States }\\
\Sigma: \text{ input alphabet }\\
\Gamma: \text{ Stack alphabet }\\
q_0: \text{ start state }\\
F \subseteq Q: \text{ accept states }\\
\Sigma_\epsilon = \Sigma \cup \{\epsilon\}, \Gamma_\epsilon = \Gamma \cup \{\epsilon\}\\
\delta: Q \times \Sigma_\epsilon \times \Gamma_\epsilon \rightarrow PowerSet(Q\times \Gamma_\epsilon) 
$$

Look at input and top of the stack and may pop the stack, push a new simbole, change state.

We decorate arrows with 
$$a,b \rightarrow c$$
See a in input and b on top of the stack, then replace b with c on top of the stack, any of these could be the empty string.

#### Example for the anbn irregular language
![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/PDAsimpleEx.png)

#### Example
$$L = \{a^ib^jc^k \mid i,j \geq 0 \land i=j \lor i=k\}$$

![alt text](https://raw.githubusercontent.com/Elvric/ClassNotes/master/Fall2018/Pictures/PDAwNonDet.png)

### Theorem
1. Every CFL is recognized by some PDA
2. Every language recognized by a PDA is a CFL

---
# 22-10-2018
### Important points on PDA
1. Acceptance only happens at the end of the input!
2. A PDA cannot decide to jam when there are moves possible.
3. When there are choices the PDA can elect any choice it wants.

We have already defined the PDA are equivalent to CFL

## DPDA
Determanistic push down automata. These DPDA corresponds to a restrictive class of context free languages we call them DCFL.
In fact all programming languages today are DCFLs. These languages can be parsed in linear time compare to CFL that can by parsed in O(n^3).
$$ \delta: Q \times \Sigma_\epsilon \times \Gamma_\epsilon \rightarrow Q \times \Gamma_\epsilon \lor \emptyset $$
For every
$$ q \in Q, a \in \Sigma, x \in \Gamma$$  
Exactly one of 
$$ \delta(q,a,x), \delta(q,a,\epsilon), \delta(q,\epsilon,x), \delta(q,\epsilon,\epsilon)$$  
Is non-empty. 
So just as a DFA even dead states that are wrong have to loop back to themselves.

### Properties of DPDA's
The intersections of a CFL and a regular language is a CFL.  
The intersection of 2 CFLs may not be a CFL.

## Algorithm for CFL
$$L(G) = \emptyset\\
NT \ X \text{ is genereting if: } X \xrightarrow{*} w \in \Sigma^* \lor T^*\\
L(G) \neq \emptyset \iff \text{S in generating}$$

Algorithm, calling the generatung state GEN. Now let GEN=T which is all the terminal symbol.  
Then I am going to repeate until nothing changes. Which means that for each rule:
$$X \rightarrow \alpha$$
Check if:  
$$ \alpha \in GEN^*$$
If so add X to GEN.
At the end if S is in GEN then L(G) is generating.

There are more complex algorithm to check if L(G) is infinite?  
This is provably impossible! The complement of a context free language may not be a CFL. But the complement of a DCFL is a DCFL.

---
# 24-10-2018
## Compiler
Firts string enter a tokanizer that is actually a DFA (lexical analyser). This technology is understood so well that we just need to know which ones are the reserved words and there is a progam that generates the DFA for us. i.e Lex

These tokens go through a parser that generates the parse tree checking that the grammar is correct this is a DPDA. 

### Example 
Consider the language:
$$ S \rightarrow (S)S \mid \epsilon$$

Then we have the follwing code to read it:
```
Tree parseString(str) {
    T = parseS(str)
    if not cos(st) then error('Extra tokens", str)
    else return T;
} 
```

### Grammar example
LL(1): grammar uses a one-symbol symbol look ahead to decide which rule to use. (top down)
LR(1): grammars work bottom up and also use one symbol look ahead.  
LALR(1): is a hybrid and the most popular choice.

## Deciding if a word is in a CFL algorithm (Cocke Kasami-Younger)
Consider G as a generique CFL and w as a word, w in L(G)?

The first thing we do is convert G into Chomsky normal form. 
$$ X \rightarrow AB\\
X \rightarrow a$$
Every single context free grammar can actually be written in this form.  
This may cause exponential blow up but we shall not worry about that right now.  The structure of the tree will hence be a binary tree. We can then generate all trees which takes exponential time and generate all trees with deps |w| or depts n  where w can be written, checking if w is in one of them. 

We can do better by keeping track of partial derivation along the way to see which ones can lead to w, this is call dynamic programming. Which reduces the time complexity to n cubed.

In order to achieve the above we are going to index a 2 index family of subsets of V (set of non terminal).  
$$X_{ij} := \{A \in V \mid A \xRightarrow{*} a_i...a_j \}\\
X_{ii} := \{A \in V \mid A \rightarrow a_i \}\\
b \in X_{ik} , c \in X_{k+1,j}, A \rightarrow BC\\
\implies A \in X_{ij}, A \rightarrow BC \xrightarrow{*} a_i...a_j  
$$

So at the end we check, is  S in X1n.

There is the **EardCay** algorithm that shows how to do the algorithm in the regular form.

# Pumping Lemma for context free languages
The finite idea is that there are only finitly many variables and many rules.

---
# 26-10-2018
## Pumping Lemma for context free languages.
Here we know that we have a limited number of non terminal characters. Hence if we build a tree that has a higher depth than the number of ynon terminal characters then we know that a certain character must be repeated at least once.  
Now consider that character repeated we can cut out the tree generated by it the second time and replace that with the tree it generated earlier when it first appeared. Hence consider the word w generated by the tree, then u is the String generated before we reach the first instance of our terminal character v is the part generated by the instance of the first terminal, w is generated by the second instance and x is the right part generated by the first instance while y is the right instance generated by neither of those.

Here we can replace the tree of the second instance with the first instance which gives us a new tree with repeated v and x this is how we get the pumping lemma.

$$
\forall L \exists p > 0\\
\forall s \in L \mid s \mid \geq p\\
\exists u,v,w,x,y \in \Sigma^* \ s.t\\
s = uvwxy\\
\mid vx \mid = \not 0\\
\mid vwx \mid \leq p\\
\forall i \mathbb{N} \ uv^iwx^iy \in L
$$

### Now the contra positive
$$
\forall p > 0\\
\exists s \in L \mid s \mid \geq p\\
\forall u,v,w,x,y \in \Sigma^*\\
s = uvwxy\\
\mid vx \mid = \not 0\\
\mid vwx \mid < p\\
\exists i \in \mathbb{N} \ uv^iwx^iy \in \not L
$$

### Example
$$
L = \{a^nb^na^n \mid n \geq 0\}\\
p\\
a^pb^pa^p\\
c_1 = v \lor x = a^kb^k\\
c_2 = v \land x = a^k\\
c_3 = v \land x = b^k\\
c_4 = v = a^k, x = b^k\\
counter \ c_1: i=2 \text{ out of orderd string is something like abab}\\
counter \ c_2,c_3,c_4: i=0 \text{ then we get string where the p do not match}
$$


$$L = \{a^{i+j}b^{j+k}c^{i+k} \mid i,j,k>0\}$$

$$L = \{ww \mid w \in \Sigma^*\}\\
L'=\{ww^{rev} \mid w \in \Sigma^*\}$$
Here L' is context free but not the other.  
Consider 
$$ \hat{L} = L \cap a^*b^*a^*b^*\\
= \{a^mb^na^mb^n \mid m,n \geq 0 \}
$$
This easier to see that it is not context free using the lemma describe above.

Now lets look at the complement:
$$ \bar{L}$$
is a CFL
$$
S \rightarrow AB \mid BA \mid A \mid B\\
A \rightarrow CAC \mid a\\
B \rightarrow CBC \mid b\\ 
C \rightarrow a\mid b
$$

---
# 29-10-2018
Lets show that L complement is context free then we can build the following grammar:
$$
S \rightarrow AB \mid BA \mid A \mid B\\
A \rightarrow CAC \mid a\\
B \rightarrow CBC \mid b\\
C \rightarrow a \mid b
$$

Hence context free languages are not closed under complementation.

Now if L1 and L2 are both context free what can we say about their intersection. Nothing, it is not necessarly context free.  
What about their union: context free yes. (Consider their grammar, just add an extra rule where S -> L1 | L2 and we are done).

Now what about concatenation: I would say context free (remplace every epsilon by L2 start non terminal and boom we have the two languages concatenated yet still context free). Even easier mentioned by the prof just S -> L1L2.

## More pumping lemma proofs.
Suppose:
$$ \Sigma = \{a\}$$ 
Just one letter then regular languages are equivalent to context free languages. (Not easy to prove).

Now the exercise with A= adversary, I means me and arrow means pick
$$\{0^i1^j \mid j = i^2\} \text{ not a CFL}\\
A \rightarrow p\\
I \rightarrow 0^p1^{p^2}\\
A \rightarrow u,v,w,x,y \ uvwxy = 0^p1^{p^2} \ \mid vx \mid > 0 \ \mid vwx \mid \leq p\\
$$
Cases:
$$
(a) \ vwx = 0^* \lor 1^* \text{ then } i = 0\\
(b) \ v \lor x = 0^n1^k , i=2\\
(c) \ v = 0^n, x = 1^m, \mid x \mid \lor \mid y \mid = 0, i=2\\
(c) \ v = 0^m, x = 1^q, \mid x \mid \land \mid y \mid = \not 0, i=2\\
s = 0^{p+n}1^{p^2+q}\\
(p+n)^2 = p^2+2pn+n^2\\
2pn>p, \ 2pn+n^2>p>q\\
p^2+2pn+n^2 > p^2+q
$$
Hence there are not equal so we are out of the languages.

#### Interesting chalenge
$$\{a^p \mid p prime\}$$
Prove this using CFL pumping.

$$L = \{a^ib^jc^k \mid c<i<j<k\}\\
A \rightarrow p\\
I \rightarrow a^pb^{p+1}c^{p+2}\\
(a) \ v \lor x \text{ Stradle boundary } i = 2 \text { out of order}\\
(b) \ v = a^k \land x = b^j k,j > 0 i = 2\\
a^{p+k}b^{p+1+j}c^{p+2}\\
p+1+j \geq p+2\\
(c) \ v = a^k, x = \epsilon i = 2\\
a^{p+k}b^{p+1}c^{p+2}, p+k \geq p+1\\
(d) \ \text{ Similar for for x is not empty or b's or c's }$$

# Limits of computability
1. There is a single universal machine, all other machines can be simulated by it. In fact machines can be described symbolically and treated as data (software).
2. Software
3. Unsolvability of Halting problem. There exists unsolvable problems. 
   
This gave a compelling notion of what computation means.

# 31-10-2018
An algorithm is a precise specification in terms of basic steps (easily doable). With a termination proof?  
The definitive answer was given by Alan Turing through the Turing machine.

## Turing machines
$$
Q \text{ set of state }\\
\Sigma \\
\Gamma \supset \Sigma \cup \{\_\} \text{ tape alphabet }\\
\delta: Q \times \Gamma \rightarrow Q \times \Gamma \times \{L,R\}
$$
The tape has a starting point but no end points, in each cell we can write any symbol from our set alphabet.  
There is also a read head that reads one cell at a time that reads one symbol at a step.  
The transition function does multiple thing it is in a state and reads a tape letter then it can do three things at the same time:
1. Change the state
2. Write something different on the tape location
3. Move the tape left or right

The input starts with a finite word on the type but the working size of the tape has no limit (the tape comes with something written on it ending with the _ symbol)

There is a special state called qa accept that and a special state called qr that is reject but there is always one accept state and one reject state, both these states are halting states, once reached this is the end. It may go back and forth forever.

A transition can only depend on a finite amount of information.

## Halting problem
There is no algorithm that can take the description of a program and the input to that program and decide whether the program halts on that input. (Does not depend on the structure through which the algorithm is described)

### Proof based on modern programming concern
Suppose S -> program  
\#S -> code of S  
S(x) -> run programming S on input xS  
S(x) downarrow -> terminates  
S(x) uparrow -> diverges  
H(#s,x) -> yes S(x) downarrow or No S(x) diverges

Assume H exists  
```
P(x) := if H(x,x) then loop else halt
P(#P) does it halt?
```
Assume it does then H(#P,#P) -> true but then we end up looping so it is not correct.  
Assume it does not then K(#P,#P) -> false but then we halt which means that it is not possible. 
Hence there is no such program H.

## Models of computation
A turing machine with extra tapes is very handy. So saying that I have 17 tapes moving with their own head. This has the same power than an ordinary turing machine. This is true I believe as we can always move back as many times as wanted on our tape. Also adding non determinism does not increase the expressive power.

What about a 2D tape? Same power as ordinary Turing machine

## RAM (Random access machine)
This is equivalent in power to Turing machine which is the same as lambda calculus which is the same as a two stack machine which is the same as 2-counter machines which is equivalent to while and if then else which is equivalent to java, c, c++, C#, F#

### Is there a more powerfull model?
So far we do not know but we think not.

From now on we will use if then else

---
# 02-11-2018
## What is a language accepted by a Turing machine?
$$ L(M) := \{w \in \Sigma^* \mid \text{ M halts on w and accepts} \}$$  
If a word is not in L(M) then a turing machine may reject it or fail to halt. Such langugages are called Turing Recogonizable or computably enumerable.

There is a subclass called **Turing Decidable/comuputable** which is a strict subclass, never equal. These are turing machine that always halt and give a yes or no answer. All context free languages are turing decidable.

A computational device is abstractedd as a function when we consider the tteps it does to achieve the result as a black box. If we have two fonctions we can still view them as just one function (we compose them this is the central idea of fonctional programing).

However partial fonctions are very important.
```
If f is a partial function then f(x) = y is one possibility
or f(x) many not be defined.
```
$$
dom(f) = \{x \mid \text{ f(x) is defined}\} \subseteq X\\
range(f) = \{y \mid \exists x, f(x) = y\}
$$

If dom(f) is equal to all possible X then f is a total function.

Input maybe integer, Strings or anything else, it does not really matter we will use what is convenient.  
2 integers can be represented as just one integer
$$ (n,m) \rightarrow 2^n3^m \\
(n,m,p) \rightarrow ((n,m),p) \rightarrow 2^{2^n*3^m}*3^p\\
\mathbb{N} \times \mathbb{N} \rightarrow \mathbb{N}
$$
<m\> = encoding of a TM  
<m,x\> = ecoding of M with input x.

### Language that is not turing decidable but turing recognizable
$$ A_{tm} = \{<M,x> \mid \text{ M accepts x}\}\\
H_{tm} = \{<M,x> \mid \text{ M halts x}\}
$$

## Infinity
- A set x is infinite if it is bijective with a proper subset of itself.
- A set is countable if it is bijective with natural numbers.

**fact**:  
There is no bijection between 
$$\mathbb{N}, 2^{\mathbb{N}}$$  

A countable union of countable sets is still countable.  

There are countably many turing machines hence only countably many algorithms. Hence only countably many computable functions, so only countably java programs, RAM machines.

#### Example
Natural number is an infinite set, take all the even number of that set we can build a one to one and onto correspondance with itself.

---
# 05-11-2018
## Computability
A function is *computable* if there is an algorithm A to compute it.

A set is decidable if there is an algorithm which always halts and asnwers yes or no to the membership query. (Does such element belong to that set?).

We know that the number of algorithms is countable as we can code the algorithms as Strings. But there are uncountably many sets so most sets are undecidable.

## Turing recognizable
This is equivalent to computablly enumerable. This means that a Turing machine will list all the member of the set if an element is in sych a set we will see it eventually but if it is not in the set we might wait forever never knowing if it is in the set.

## First Theorem (computable and countable functions)
An infinite set x subset of natural numbers (does not really matter) is decidable if and only if there exists f a total computable non decreasing function whose range is x.

### Proof
Suppose x is the range of a TCF, f with algorithm A. A always halts since f is total. Here is a decision procedure for membership in x, given m we want to know if mnis in x or not. Run n on all natural numbers, then it will halt every single time so we look at the output, if one of them is n we say yes. if we get an output m>n then we know that n is not in x.

Now lets do the reverse: suppose x is decidable then we have b which is an algorithm that decides if a given input is in x. b must halt as it is decidable, then we can define f as follows, run B on all natural numbers, every time we get a number in x we add it to a list, when we have n elements on the list we output it. This is defined to be f(n).

## Other definition 
Suppose we have a set x, we define a function as indicator function this way, 1 if n is in x and 0 if n is not in x. known as **indicator/charcteristic** function usually known as S.

**Semi characteristic function** is the same idea excepts that now only one of the two options is defined.

## Theorem of CE
A set x subset of natural numbers is CE if and only if any of the following equivalent conditions hold:
1. x is the domain of a computable function
2. Sx is computable
3. X is the range of a computable function

### Proof
2 implies 1  
CE implies 1 (CE=there exists an algorithm that when running just gives out all the member of x)  
Given an enumerator we want to know if Sx(n) = 1. The way it works is we run the enumerator algorithm and just wait for n to be outputed if we see n then we say yes else no. 
1 implies CE. Suppose we have an algorithm B to compute f and dom(f)=X. 

#### Dove tailing 
1 implies CE  
We give B(0) run it on one step then stop same thing for B of 1 and so on moving up a step for each already inputed variables. Any single one of these may run forerver but if any B(n) halts we will find out. If B halts on any input we print it and continue.

## Theorem of Union and Interesection
The union and intersection of 2 CE sets x,y is CE.  
To see this run the enumerator for x,y in parallel, then any time we see an output, we output it, for intersection we can just compare.

## Post Theorem 1
if X is computable then it is CE.  
If X is CE and X opposit is also CE then X is decidable

## Post Theorem 2
Remember pairs of numbers can be coded as a single number, In short there is a one to one and onto map from 2 numbers to a single number.
<n,m/> means n,m as a Single number. fst(<n,m\>) = n and snd(...) = m

A set X made of natural number is **CE** if and only if there exists a computable set Y subset of N cross N such that 
$$\forall x \in X, x \in X \iff \exists y \in \mathbb{N} <(x,y) \in Y>$$

### Proof
If we had such a Y we can easily see that we can get X as Y is decidable.  
Suppose X is CE, how do we construct Y:
$$Y=\{<x,n> \mid A \text{ enumerate x in n steps}\}$$
We know we have A as X is CE and we can count how long A runs then we code up <x,n\> and put that in Y. 

---
# 07-11-2018
# Reduction
The ability to transform a problem into a similar problem to solve them or show certain properties

## Halting and acceptance problem
### halting problem is a subset of the acceptance problem
We give an inmput M for the turing machine and x for the input
Construct a new machine M' on the input x it stimulates M with input x and if M halts on x then accept x. Then we can take the code for M' and the word x and pass that to the box that solves the acceptance problem asking if M' accepts x, if so then we say that M halts on x else we say otherwise.

### Acceptance problem is a subset of the halting problem
We have a turing code M and an input x, then we can feed M and x to the Halting machine, if it does not halt then we return false other waise we run x on M and just output the answer given by the machine.

### Halting problem is a subset of the Emptyness problem 
Means that M is a turing machine that recognizes 0 word, this must be known without any input, the language of the turing machine is the Empty set called E_TM representing the set of turing machines that rejects every single word.

Take M as the turing machine and x as the input, Define M' such that M' run x on M if it halts then accept input, if M does not halt then M' is an empty language else if M halts on x then M' accepts all inputs.

### Acceptance problem to the Emptyness problem
Recieve M and x, first run emptyness algorithm with M if M is empty then ouput no 

Otherwise let M' be a turing machine that takes as imput w, if w=x then it runs x on M else it rejects x, Give it to the empty problem algorithm, if M accepts x then M' accepts x as well thus if the algorithm ouputs no we say thatthe wordd is accepted, else then M' is the empty set thus we reject x.

### ATM is a subset of an algorithm that defines if the language is regular
Take as a turing machine code M and as input x, Construct a turing machine M' such that for an input w
1. check if w = a^nb^n if yes then accept
2. Else stimulat M on x
3. If M accepts x then accepts w

Pass M' to the regular algorithm checker, if M accepts x then M' accepts every word thus M' is regular if M does not accept x then M' accepts w thus it is irregular so we can give M' to the algorithm and decide on x.

### Empty set machine is a subset of EQtm which is if two M1 and M2 accepts the same language
Take M, then create a Turing machine that reject every word call that M, feed it to the EQ_tm algorithm if they are the same then return true else return false.

### Others:
1. L(M) = L(M') where M' always halts
2. L(M) is context free
3. |L(M)| < infinity
4. L(M) = sigma Star

## Co-CE
It will halt if the answer is no but it will not halt if the answer is yes

---
# 09-11-2018
#### Stimulation
We can say stimulate M on w if M halts then \_\_\_\_ this is fine but you cannot do if then else as you do not know if it halts.

# Mapping reduction
Suppose 
$$
L_1, L_2 \subseteq \Sigma^*\\
L_1 \leq_m L_2\\
f: \Sigma^* \rightarrow \Sigma^*\\
\forall w \in \Sigma^*, \text{ if } w \in L_1 \iff f(w) \in L_2
$$
Where f is total, then f is called the mapping reduction note that the convers of L1 can also be mapped to the converse of L2.  
But remember that there is the opposit between L1 and L2 is not the case.

Assume P is the mapping reduction of Q then
1. If P is undecidable then Q is undecidable
2. Q is decidable so is P
3. If Q is CE so is P
4. If P is not CE then Q cannot be CE
5. If P is not coCE then Q cannotbe coCE

Note that Acceptance test is not mapable to Empty as empty is CE but not Acceptance

## EQtm is not in CE nor coCE
We have shown that not Atm ≤m not EQtm then we will show that Atm ≤m not EQtm

### Proof
Atm ≤m EQtm  
Input <M,w\>
Construct:
1. M1(x) ignore input & accepts
2. M2(x) ignore input run M(w) & if it accepts, accept x
3. L(M1) = Simga^*, L(M2)=sigma^\* if M accepts w else empty set.

$$L(M1) = L(M2) \iff \text{ M accepts w}$$

For the not we just do the opposit. so we make that computable. but we know that not Atm is co-CE and Atm is CE thus Eqtm is neither of both

$$\mid L(M) \mid  = \infty = INF$$

### not HTM ≤m INF
Given <M,w\> we define a TM as M' with input x
1. Stimulate M on w for |x| steps, M halts before the end of the stimulation reject x
2. Else accept x

If M does not halt on w then L(M') is infinite else L(M') is finite so:  
$$<M,w> \in \bar{H_{TM}} \iff <M'> \in INF$$

## Mortality problem
We have a finite set of 3x3 integer matrices call that set S (S is finite).  
This is undecidable:
**Does there exits a sequence of matrices from S such that their product is 0?**  

## Skolem's problem
We have a recurence relation 
$$x_{n+k}= a_{k-1}x_{n+k-1}+....a_nx_n$$  
Is there any n such that 
$$x_n = 0$$

## Group problem
We have a sequence of g's and wether this sequence is equal to their identity. Same thing for having to sequences and asking whethere they are equal.

---
# 12-11-2018
# Rice's Theorem
Turing machines = program = algorithm  
The set of all programs is effectively the same as the set of all turing machines.

## Extentional property of a program
A property of the function it computes. i.e: A function that sorts elements in a set.  
Two programs are extensionaly equal if they implement the same function.
$$P_1 \sim P_2$$ 
We say that if they compute the same function.  
The same applies to turing machine when they recognize the same language.

An extensional property Q is a family of CE sets or a predicate on turing machines or on programs such that Q accepts M1 then:
$$Q(M_1) \land M_1 \sim _2 Q(M_2)$$

## Trivial properties 
Q: false (nothing satisfies), Q: true (everything satisfies)  
These are obviously decidable.

# RICE Theorem
Every non-trivial property of turing machines or programs or algorithm is undecidable.

## Proof
Let Q be a non-trival propery of TM or CE sets or programs and assume Q is extenssional. We can assume that if the language of the turing machine is empty the Q(M) does not hold. Since Q is non-trivial there is some turing machine M0 such that Q(M0) holds and hence the langauge of M0 is not the empty set.  
Atm ≤ Lq = {Mi} Q(Mi) == true.
Given a Turing Machine M and a word w, does M accept the word w, we build a new turing macine TM M' such that on input x stimulate M(w) if M accepts w then stimulate M0(x). If if M0 on x if M0 accepts x then so does M'.  
Claim M(w) is an Atm if and only if Q(M') holds.  
```
If M accepts w then M' is equivalent to M0
If M rejects w then M' accepts nothing
```

How to recognize L bar with PDA where L = {ww | w in Sigma^*}

If the input has an odd length then we know it is in L bar.  
Consider an even length word to be not in L we must split the words into two equal length and they must disagree somewhere.

Now take the string
x1x2x3....xi....xny1y2y3...yi...yn 
The length from x1 to xi = i-1 the length from yi to yn= n-i  the length xi to xn = n-i the length from y1 to yi = i-1 thus the distance betwen xi and yi = n-1. 
Now we stack all the x1 until we reach xi then we go to a state that know it has seen xi we now read the rest but pop the stack, once the stack is empty we push back ontop of it, then when we reach yi we compare it with xi and if they are different we start poping again else we reject instently if the stack is empty at the end we accept. Since this is undetermanistic at the end of the algorithm we know we have looked at all the comparaison possible so we we have derived a machine that accepts L bar.

### Godle
A language with For loops only can only express terminating algorithm. A typed higher-order function language can only express terminating programs. Any such language will also fail to express a properly terminating program.

---
# 14-11-2018
Valid computation of a TM.  
Given a turing machine M and a word w and we want to know if M accepts w.  
Given M and w we can effectively construc a CFL or a PDA such that it generates/recognizes the complement of the set of valid computation.

## Valid computation
We have a state, tape, head position.  
A configuration of a TM is a description of the state, the tape and the head position

abbaab and the machine is in state q  (name of states is different from the symbol on the tape)

abqbaab and looking at the cell just to its right.

Now for the moves we get abqbaab\#abaq'aab showing to consecutive configurations and so on.

A valid computation is a description of all the steps from the start until the machine accepts. This must generate a finite String.

### Correct start state
$$\#q_0a_1...a_n\#$$

### Valid computation (must be finite) VALCOMPS(M,w)
$$\#\alpha_0 \#\alpha_1 \#...\#\alpha_N \#$$  
where alpha 0 is the start configuraition and alpha N must be an accpet configuration. alpha n+1 must follow from alph n according to the transition rules of M.

#### Fact 
If M does not halt or accpets on w then VALCOMPS(M,w) = empty set  
We are going to show that complement of VALCOMPS(M,w) = CFL so given G, L(G) = Sigma^* must be undecidable.

1. If z is in VALCOMPS(M,w) then z begins and ends with \# and between every pair we must have a non-empty String from the right alphabet.
2. Each alpha i must contain exactly one letter from Q.
3. Alpha 0 must be the start configuration of z
4. Alpha N must be an accept configuration.
5. For each i the difference between aplha i and alpha i+1 must agree with the rules of M.

**1 through 4 can be checked with a DFA but we need a PDA for condition 5**

### Step 5 checking
The difference between alpha i and alpha i+1 should be confined to three consecutive symbols.  
The possible values of a 3 symbol sequence can be remembered in finite memory.  
2 paries of 3 symbols sequences are consistent if they follow the rule of M.

The PDA guesses a pair of alpha_i, alpha_i+1 where the rules are violated and guesses a 3 symbol sequence where the violation occurs. 

Hence now we know how to check complement of Val comp with a context free language thus if it was possible to check if it is sigma start then valcomp would be empty and we would know something impossible. 

If we use the rev for certain alpha then we could use TWO PDA's to ensure that we have a VALCOMP. This is not the same thing than a turing machine since here the PDA are independent. 

---
# 16-11-2018
# Post correspondence problem PCP
We have a finite set of pairs of Strings S. A solution is a finite sequence of natural numbers I such that S1i1S1i2.....S1ik = S2i1S2i2.....S2ik.

There is no algorithm to solve this problem.  
Halting problem reduction to the problem above.  
Lets first modify it: There is a special domino with which we must start.

## Modified version of the PCP
Turring machine representation #q0w1.....w#.......#x1_xkqaxk+i  
Here the # represents a configuration of the turing machine The q represents the state being looked at the 0 represents the postion of the head and x represents what is on the tape. qa means accept qr means reject. 
Hence our first String will be {#,q0w1..wn#}  
Now if we see the following rules we do:
$$
\delta(q,a) = (r,b,R) = S = \{qa,br\} \iff r \neq q_r\\
\delta(q,a) = (r,b,L) = S = \{cqa,rcb\} \forall c \in \Gamma \land \{a,a\}, \\
\forall a \in \Gamma \land \{\#,\#\} \land \{\#,\_ \#\} 
$$
Here we keep trying to match but by matching we create the next state of the turing machine which we know have no choice for us to know it it halts.

## Conversion from the modified to regular version
How do I ensure if I have a lot of dominos that I indeed start with {t1,b1}. Use an extra symbol * which does not appear in any String.
$$ \vec u = u_1u_2...u_n\\
*\vec u = *u_1*u_2...*u_n\\
\vec u* = u_1*u_2*...u_n*\\
*\vec u* = *u_1*u_2...*u_n*
$$
So now we have a modified PCP and we must start with {w1,x2} to get to the regular PCP.  
In this case we modify {\*w1,\*x2\*} then all the other dominos {\*w_3,x_4\*} then we of course have to have a finishing symbl with {*^,^} where the cap is an unused symbol, hence this PCP is equivalent to the modified PCP. 

Atm ≤m PCP so undecidable

PCP ≤ AMBIG, where we have a grammar and we want to know if the grammar is ambiguous. 

All problems can be solved if we can solve Atm so Atm is CE complete. (Post's question)

---
# 19-11-2018
# Validity of Formulas First-Order logic is undecidable
## Syntax of a first-order logic
We have a countable set of constants such as letters.  
Then we have a countable set of variables.  
Then we have a set of function symbols.  
A set of predicate symobls.  
Function and predicates come with arities. 

TERMS: defined by induction
1. any constant is a term
2. any variable is a term
3. If f is a function symbol of **arity** n and t1,...,tn are terms then f(t1,...,tn) is a 
4. If P is a predicate of arity n and t1,...,tn are terms then P(t1,...tn) is a formula.  
5. If phi1 and ph2 are formulas
$$
\varphi_1 \land \varphi_2\\
\varphi_1 \lor \varphi_2\\
\neg \varphi_1\\
\varphi_1 \implies \varphi_2\\
\exists x \in Var \varphi\\
\forall x \in Var \varphi
$$
These are all formulas do not interprete them as we have not defined what the symbols mean yet

## Interpretation Semanics (meaning)
$$
D \rightarrow \text{ domain of interpretation }\\
$$
All constant symboles are interpreted as elements of D. 


### Example: Arithmetic/number theory
Constants: 0,1,2,3...  
Variables: x,y,z,...  
Functions: + (ar 2),* (ar 2),succ (ar 1)  
Predicate: =, ≤

#### Interpretation
SUCC x -> x+1  
PLUS is interpreted as +  
TIMES is interpreted as *

All functions symbols of arity n are interpreted as functions D^n -> D.  
All predicate symbols of arity n are interpreted as subset of D^n
$$ \leq = \{(0,0), (0,1), (0,2), (1,1), (1,2)...\}$$

+(3,0)*(+(3,0),x), succ(7) (these are terms)

+(0,3) ≤ 2 (this is a formula)

## Understanding Important concepts
- True
    - constants can be interpreted in truth statment
    - variables require extra information, the environment: is a map from variables to D (programming this is called a set of bindings). 
    - Here we will use roh as the environment so we write the variable followed by roh to have the environment
    - The same thing applies for functions or we can put roh in front of the functions variables terms.
    - What about formulas: we have I, roh bar equal, phi -> this formula phi is true when we use itnerpretation I and environment rho
    - A formula that is true in an interpretation is called **satisifiable**
    - A formula that is true in every interpretation is called **valid**.
    - A formula is **invalid** if there is at least one interpretation in which it is not true.
    - A formula is **unsatisfiable** if there is no interpretation in which it is true.
- Provable
    - We can define axioms and rules of inference and proofs. A formula which can be proved is called a theorem.
    - Different proof systems have different powers, if we have a proof system such that every valid formula is a theorem then the system is called **complete**.
    - FOL -> COMPLETE
    - ARITHMETIC -> has no complete proof systems
    - A proof is a finite object (even inductions)
    - A proof can be checked by a terminating algorithm.
- Theorem
    - Soundness: any theorem is valid.
    - Theorems must be proven.
- Validity
    - A formula is valid if it is true is all interpretations no matter the rho.
- Complete

Truth:
$$
I, \rho \models \varphi\\
I, \rho \models t_1=t_2 \ if \ [t_1]\rho = [t_2]\rho\\
I, \rho \models P(t_1,t_2,..,t_n) \ if \ [t_1]\rho,[t_2]\rho,..,[t_n]\rho) \in [P]\\
I, \rho \models \varphi_1 \land \varphi_2 \ if I, \rho \models \varphi_1 \land I, \rho \models \varphi_2\\
...
$$

Validity:  
$$\exits n \forall m n \leq m \rightarrow T,D=\mathbb{N}\\
\rightarrow F D = \mathbb{Z}$$

---
# 21-11-2018
Validity is undecidable but it is computably enumerable because we have a complete proof system for FOL. 
PCP ≤m VALIDITY

Predicate = p  
Constant = a  
Fonction = f0,f1
Formula:
$$\{\rho(1..N,f_{\alpha_i}(a),f_{\beta_i}(a)) \land \forall x,y [ \rho (x,u) \implies 1...N [ \rho f_{\alpha_i}(x),f_{\beta_i}(y))]\\
\implies \exists z \rho (z,z\}\\
\alpha_i, \beta_i \in \{0,1\}^*\\
f_1(f_0(f_1(a))) \equiv f_{101}(a)
$$
