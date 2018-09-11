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
Such that uo is minimal

##### Example 4
$$\mathbb{N}, \leq$$
$$\mathbb{R}, \leq$$
The first one is well founded the second one is not.  
A well founded total order is called a well order.  
If an order is not well founded there are infinitly long strictly descending sequences.

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
$$ \delta^* : *\Sigma^* \rightarrow S $$
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