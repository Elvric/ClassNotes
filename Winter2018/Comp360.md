 

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

## Max flow Problem continue Lecture 2 10/01/2018

### Ford_Fulkerson Algorithm
Try to find s-t path that have not used their full capacity and push more flow. \
We must however make sure that the path we are going to 
optimize can actually make use of the added flow down the line. 
Idea is that for the edges that go in the opposite direction
we subtract the flow that is going in the opposite direction


1. Start from all zero flow
2. Find a path firm s-t such that the edges that are in the forward direction have unused capacity. The backward edges have strictly positive flow on them. Add one unit to the forward edges and subtract one unit to the backward edges.

> Def: [Residual graph] given a flow network (G,s,t,{$C_e$})
>and a flow f on G, the residua graph $G_f$ is as follows
>1. Nodes are the same as G
>2. $\forall uv \in G \ f(u,v) < C_{uv}$ add the edge uv with residual capacity $C_{uv}-f(uv)$to $G_f$
>3. $\forall uv \in G$ with $f(uv)>0$ add the opposite edge vu with residual capacity $f(uv)$
 
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
Let K be the largest capacity n the number of vertices and m
the number of edges.

At most Km iterations for the while loop 
Each iteration requires DFS so m+n
Since we assumed that the graph is connected and every vertex is adjacent to at least one edge m is at least n/2 so we can just change it to n.
Final running time is: O(Km^2)

>Def a cut in a flow network is a partition (A,B) of the verteces such that $s \in A$ and $t \in B$

>Def Capacity of this cut is the sum of the capacities of edges going from A to B
>Cap(AB)= $\sum_{uv \in E \ u \in A \ v \in B} C_{uv}$ 
>
> A network with n verteces has $2^{n-2}$ cuts

---
### MaxFlow problem continue 15/01/2018
>Recall: For a flow $f:E \rightarrow  \mathbb{R}^+$ \
$val(f)= \sum_{su \in E} f(su)$

>Claim: for any s-t cut (A,B), $val(f)= f^{out} (A) - f^{in}(A)$
    
> $\sum_{u \in A} ((\sum_{uw \in E} f(uw))-(\sum_{vu \in E} f(vu)))$

If e is an edge with both endpoints in B => f(e) is not in the sum. \
else if e has both points in A => f(e) is in both sums so it adds up to 0. \
else e exist in both A and B => f(e) is either added or subtracted in the sums so we are only accounting for flow in and out. 
Why does vale(f) = f in of the sink?
    Pick B as being just the sink then there is no f 
    out had just f in

> Claim: let (A,B) be a cut f be a flow then $val(f) \leq Cap(A,B)$ \
> Proof: 
> val(f) = $f^{out} (A) - f^{in} (A) \leq f^{out} (A) \leq \sum_{u \in A,v \in B, uv \in E} C_{uv} = Cap(A,B)$

So to summarize we can use cuts to check for optimality if we fined a cut that has a Cap value equal to the maxflow we have found then we cannot do better than that. 

### Proof of Ford-Falkerson finds optimal flow
FF: starts with o flow and while we can find paths in the residual graph we augment(f,P) and update residual graph. 

  Consider the point where FFF terminates. Let A* be the set of the vertices that the algorithm can reach from s in the residual graph. Note that t will always be in B* as if we could reach t then we would have an s-t path. 
    If uv is an edge original network with u in A* and v in B* Cuv is saturated otherwise we would be able to include v in A. 
$f^{out}(A^*)= \sum_{u \in A^*,v \in B^*, uv \in E} C_{uv} = Cap(A^*,B^*), f^{in} (A^*)=0$ because otherwise we would have an edge in Gf that goes from A* to B*

We showed that at the end of the algorithm max flow is always found,
$Maxflow \leq Cap(A^*,B^*)=val(f)=MaxFlow$

    Problem: Given a flow network how can we find a min-cut? 
    Just run FF and find A* and B*
$val(f) \leq maxFlow \leq minCut \leq Cap(A^*,B^*)$ but $val(f)=Cap(A^*,B^*)$ so they are all equal.

### Theorem Min-cut max flow relationship
    For any flow network Max-Flow = min-cut

---
## 17-01-2018

### Is it possible to have a max-flow that assigns non-integer values to some edges? 
Yes it is possible to find a max-flow not only with full integers such as using 0.5 through the edges although all the edges have an integer max capacity. 

### Is there always an all integer max-flow? 
FF only works with integer values hence yes it is possible to find the answer with only integers. 

### Running time $O(m^2K)$
Can be ineficient when K is large so it is not consider polynomial time.\
Imput size is $\Theta(m\log k)$ so not too big compare to the algorithm. \

### A faster FF
Possible aproach:
1. Always pick the shortest path from s to t
    * Leads to an efficient algorithm that we will not discuss here.
2. Fattest path approach, find the path with the largest bottleneck. \
    Not very easy to find the largest bottleneck 
3. Same as above but: 
    *  Initialy set $\Delta = 2^{log(K)}$ so that it is now rounded to a power of 2.
    *  While there are augmenting paths with bottleneck greater than $\Delta$ use them to augment the flow.
    *  When we run out of these we set reset $\frac{\Delta}{2}$
    *  When $\Delta$ is less than 1 then we stop.
### How can we check of there are augmenting paths with bottleneck greater than $\Delta$   
Remove any edge that are less than $\Delta$ from our graph so we create a subgraph with edges that can be exploited. 

```
    while Delata ≥ 1
        while exist path Gf(Delta) ≥ Delta
            Augment(f,P)
            update Gf
        Endwhile
        Delta <- Delta/2
    Endwhile
```
### Running time:
Finding the path,Augmenting and update Gf $O(m)$, The bigger while loop is $\log(K)$ 
The inner loop now or the $\Delta$ phase. \
Claim: Let f be the flow at the end of the $\Delta$ phase there is a cut (A,B) such that max-flow $\leq cap (A,B) \leq val(f) + m \Delta$

#### Proof:
A are all the nodes that can be reached from s in the $G_f(\Delta)$ B is the rest. \
If e is an edge from A to B in the original network we know that $C_e-f(e) < \Delta$ \
If e is an edge from B to A then $f(e) < \Delta$ \
$val(f) = f^{out}(A)-f^{in} (A) = \sum_{e from A to B} f(e) - \sum_{e from B to A} \Delta \leq \sum_{e from A to B} (C_e-\Delta) -$ 
$\sum_{e from A to B or B to A} \Delta$ which gives us capacity of the cut minus m$\Delta$ \
Let's look at the flow at the end of the previous phas $val(f_{prev})\geq maxflow -2\Delta m$. \
How many augmentations can we have in the Delta-phase? \
We can have at most 2m augmentations in this phase because each one increases the value by at least $\Delta$ and starting from max-flow $-2m\Delta$. 
 
So the running time is $O(m^2 \log K)$ \
It finds the max flow because it is a special instance of maxflow.

---
## Largest matching problem 22-01-2018
Def: Bipartite graph: is a graph such that we can partition its verteces into 2 parts such that there exist no edge within the parts, the edges only exist between the parts. Also known as the 2 coloring problem. \
note part of the course: A graph is bipartite if and only if it does not have any odd cycle. \
Trivialy a bipartite graph does not have an odd cycle because there always exist a node that is adjacent to nodes that are in A and in B. 

### Largest matching problem:
Def: A matching is a set of edges such that no 2 of them share any end-points/node. \
Def: A perfect matching is a matching that has all of its verteces matched with an edge (includes all the verteces).
#### Given a bipartite graph with parts x and y how can we find the largest matching?
We construct a flow network in the following manner.
1. We direct all th edges from X to Y.
2. We add 2 new verteces called s and t.
3. We put edges from s to all verteces in X and edges from all verteces in Y to t.
4. Assign capacity 1 to all the edges.

Claim: Max flow in this network = max matching in G. 

### Proof
First we show max matching is at least max-flow *M*. We assign a flow of 1 to all edges in M and 0 to all other edges between X and Y. \
For the edges starting from s or ending at t, the ones that go to verteces in M get a value of 1 and the rest 0. \
Note that this is a valid flow with flow of M. This is because there are M vertices in X involved in M and we assign 1 to the edges from s to those verteces. \
Next we need to show that there is a matchingof size Max-Flow \
There is a max-flow with integer values. Thus all edges will have a flow of 0 or 1. The edges between X and Y with 1 unit of flow on them form a matching.  \
To summerize we showed max-flow is at least M and max-flow=some matching that is smaller or equal to max matching which implies that M = max-flow \
Remark: Note that in the above proof we could assign infinit capacities to edges between X and Y. The incoming flow in all verteces in X will be at most 1 so the edges between X and Y will never have any flow > 1. \
We know max-flow = min cut what does this min in this contest? 

Def: A vertex cover is a set of verteces such that removing them will remove all the edges from the graph. 

#### Thm: In every bipartite graph Max Matching = Min vertex cover (Note that the graph needs to be bipartite, a triangle for example violate that rule)

Remark: Note that if a graph has a matching of size k, then every vertex caver needs to pick at least one vertex from each of these k edges so ≥ k. 

Proof: Lets look at the min cut (A,B). Let's now split the X 2 parts $A_1,B_1$ and for Y $A_2,B_2$ where $A=\{s\}\lor A_1 \lor A_2$ and similarly for B. We know that there are no edge going from $A_1$ to $B_2$ because that would make my cut value be infinity. So we can just remove all the verteces $B_1$ and $A_2$ so $B_1 \lor A_2$ is the vertex cover.  



