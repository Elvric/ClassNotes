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