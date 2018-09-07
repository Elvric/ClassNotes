# COMP 330 Theory of computation
---
# Lecture 1 05/09/2018
## Course Info
Office hour M 1:30-3:00 Wed 11:00-12:30 105 N McConnel  
Assignment 30%  
Midterm 10% open notes
Final 60% open notes

## Relations
Correspondance between elements fo two sets.
$$R \subseteq A \times B$$  
Not every element of A need to be related to anything in B. A given element of A could be related to exactly one element in B we gat a fonction  

### Equivalent Relation
A function is bijective the rest are all relations  
$$R \subseteq S \times S$$
then
$$\forall  \in S \ sRs \\
\forall s,t \in S \ sRt \implies tRs \\
\forall s,t,p \in S \ sRt $$

#### Example
$$ n \equiv m (mod -\  5) \ n,m \in N$$
Give an equivalent relation R, an equivalence class of an element x
$$ x \equiv {y | xRy}$$

---
# Lecture 2 07/09/2018
## Partial Order
Binary realation ``relation = correspondence between to different set``. We use this simble 
$$\leq$$  
### axioms:
$$ \forall s (s \leq s )\\
\forall s,t((s \leq t \land t \leq s) \implies s = t) \\
\forall s,t,r(s \leq t \leq r \implies r \leq r)$$  
If every pair can be compared then we call that a ``total order``
$$ \forall s,t (s \leq t \lor t \leq s)$$

#### Ex1 
$$ \{ a,b,c\} \\
\{ \phi, \{ a\}, \{ b\}, \{c\},...\} \\
\{a\} \subseteq \{a,b\} \subseteq \{a,b,c\} \\$$  
But no relation between  
$$\{a,b\} \ \{b,c\}$$  

#### Ex 3
$$ f: \mathbb{R} \rightarrow \mathbb{R} \\
f \leq g \ \forall r \in \mathbb{R} \ f(r) \leq g(r)$$

### Well-Founded order
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
$$ U \subseteq W \land U \not = \phi \\
\exists u_0 \in U$$
Such that uo is minimal

#### Example 4
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
$$ \forall x \in S P(x)
