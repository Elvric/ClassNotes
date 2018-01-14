

# Comp 360 

## Intro Lecture 1 08/01/2018

### Basic knowledge
> $\log_2(n)$ => the number of bits required for an umber raised up 

>$\sum_{n=0}^k2^n=2^{n-1}-1$
$S=(a_1,a_2,…)$ a sequence of integers
E is the set of even numbers from 1 to n \
$A=\sum_{i \in E} a_i$
$S=(1,3,2,5,4)$
$A=8$

> $G=(V,E)$ an undirected graph Suppose for every edge uv a number $C_{uv}$ is assigned. \
What does the following statment mean?
$\exists c \ \forall u \in V \ \sum_{uv \in E} C_{uv}=c$ \
Their exist a number c such that for all verteces in V the sum of its edges will equal c

> $G=(V,E)$ an undirected graph with degree of every vertex is 10 then suppose to every vertex $v \in V$ a postive integer $a_v$ is assigned. If $\sum _{v\in V} a_v=5$ then $\sum_{u \in v} \sum_{w \in V,uw \in E} a_w=50$

### Max flow problem
> Def: a flow network is a directed grapg $G=(V,E)$
> 1. Every edge e has a capacity $c_e \geq 0$
> 2. There is a source $s \in V$
> 3. The is a sink $t \in V \ t \neq s$ \
> Assumption: 
>* No edge enters the source
>* No edge leave the sink
>* All capacities are integers
>* There is at least one edge incident to every vertex. (Every vertex has at least one edge connected to it)

>Def: [flow] A flow is a function $f:E \rightarrow \mathbb{R}^+$ such that 
> 1. Capacity condition $\forall e \in E \ 0 \geq f(e) \geq c_e$
> 2. Conservation, flow is conserved $\forall u \in V \ u \neq s,t \ f(u) = \sum _{vu \in E} f^{in}(vu) = \sum_{uw \in E} f^{out}(uw)$
> 3. Value f = $\sum_{su \in E} f(su) = f(s)$

    Now Give a flow network find a flow largest possible value
___

## Max flow Probelm continue Lecture 2 10/01/2018

### Ford_Fulkerson Algorithm
    Try to find s-t path that have not used their full capacity and push more flow.
    We must however make sure that the path we are going to 
    optimize can actually make use of the added flow down the line. 
    Idea is that for the edges that go in the opposit direction
    we substract the flow that is going in the opposit direction


1. Start from all zero flow
2. Find a path firm s-t such that the edges that are in the forward direction have unused capacity. The backward edges have strictly positive flow on them. Add one unit to the forward edges and substract one unit to the backward edges.

> Def: [Residual graph] given a flow netwok (G,s,t,{$C_e$})
>and a flow f on G, the residua graph $G_f$ is as follows
>1. Nodes are the same as G
>2. $\forall uv \in G \ f(u,v) < C_{uv}$ add the edge uv with residual capacity $C_{uv}-f(uv)$to $G_f$
>3. $\forall uv \in G$ with $f(uv)>0$ add the opposit edge vu with residual capacity $f(uv)$
 
```
Initially set f(e)=0 on every edge
Construct Gf
while (s-t path in Gf)
    {
        f'<- Augment (f,p) //using the path
        update f<-f'
        update Gf
    }    
Augment(f,p)
    {
        Find lowest of p (path), which is the smallest 
        residual capacity.
        add it to forward edges
        subtract to backward edges
    }
```
#### Correctness:
>Capacity condition is satisfied through the algorithm:
>
>Residual capacities are chosen so that updating with Augment(f,p) will never assign a number to an edge that is larger than its capacity or smaller than 0.

>Conservation condition $f^{in}(v)=f^{out}(v)$
>
>Case 1
>
>We add the new value to 2 forward edge then we add to $f^{in}(v)$ and to $f^{out}(v)$
>
>Case 2
>
> Backward edge for $f^{out}$ so nothing changes
>
>Case 3
>
> Same as case 2 but the other way arround
>
>Case 4
>
> We remove flow from both side so the $f^{in}$ decreases so does $f^{out}$

#### Terminate
    At every iteration the flow increases by at least one unit. It can never exceed the total sum of all the capacities.

#### Running Time
    Let K be the largest capacity n the number of verteces and m
    the number of edges.

    At most Km iterations for the while loop 
    Each iteration requires DFS so m+n
    Since we assumed that the graph is connected and every vertex is adjecent to at least one edge m is at least n/2 so we can just change it to n.
    Final running time is: O(Km^2)

>Def a cut in a flow network is a partition (A,B) of the verteces such that $s \in A$ and $t \in B$

>Def Capacity of this cut is the sum of the capacities of edges going from A to B




