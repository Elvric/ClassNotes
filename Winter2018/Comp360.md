 

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
Next we need to show that there is a matching of size Max-Flow \
There is a max-flow with integer values. Thus all edges will have a flow of 0 or 1. The edges between X and Y with 1 unit of flow on them form a matching.  \
To summarize we showed max-flow is at least M and max-flow=some matching that is smaller or equal to max matching which implies that M = max-flow \
Remark: Note that in the above proof we could assign infinity capacities to edges between X and Y. The incoming flow in all verteces in X will be at most 1 so the edges between X and Y will never have any flow > 1. \
We know max-flow = min cut what does this min in this contest? 

#### Def: A vertex cover is a set of verteces such that removing them will remove all the edges from the graph. 

#### Thm: In every bipartite graph G, Max Matching = Min vertex cover (Note that the graph needs to be bipartite, a triangle for example violate that rule)

Remark: Note that if a graph has a matching of size k, then every vertex caver needs to pick at least one vertex from each of these k edges so ≥ k. 

Proof: Lets look at the min cut (A,B). Let's now split the X 2 parts $A_1,B_1$ and for Y $A_2,B_2$ where $A=\{s\}\lor A_1 \lor A_2$ and similarly for B. We know that there are no edge going from $A_1$ to $B_2$ because that would make my cut value be infinity. So we can just remove all the verteces $B_1$ and $A_2$ so $B_1 \cup A_2$ is the vertex cover.  

---
## 24-01-2018
Proof continue: Obviously min cut < $\infty$ (({s},V-{s}) is a cut < $\infty$. Thus $B_1 \lor A_2$ is a vertex cover in the original graph. \
On the other hand $Cap(A,B)= \sum_{e: s \ to \ B_1} C_e + \sum_{e: A_2 \ to \ t} C_e$ which is $= \mid A_2 \mid + \mid B_1 \mid$ \
Now we have show that there is a vertex cover that is equal to the min-cut (A,B). 

Next we will show that $min-cut \leq min-vertexCover$ \
Let S the the smallest vertex cover. $S_1$ is the part in X and $S_2$ is the part in Y. \
Let $A= X-S_1 \cup S_2 \cup \{s\}$, $B=A^c=S_1 \cup Y-S_2 \cup \{t\}$. So cap(A,B)= $\mid S_1 \mid + \mid S_2 \mid$. There are no edges between $X-S_1 \land Y-S_2$ because the S is a vertex cover. \
Conclusion: in such a model min-cut = min-vertexCover = max=matching.

#### Thm: (Konig) In a bipartite graph Max-Matching = Min-Cut proven

### Disjoint Paths in directed graphs
Input: A directed graph G and two distinct nodes are marked as s and t.   
Goal: find the maximum number of edge-disjoint paths from s to t. (paths that do not share any edges). 
* Just running DFS/BFS does not work
    * Issue: we might have a path that kills other paths. Even taking the shortest path will not work as we can have a path that goes around a lot yet is optimal to take rather than a small 1.
* FF can work we put a capacity of 1 on all the edges.  
    * **Personal thought:** \
    Then remove edges with 0 capacity then run DFS until finding t, if we ever encounter the same node in a path we have a loop so we remove the edges that are part of that loop at the end we remove or mark the edges as part of a path and carry on until no path are valid. Then we have found exactly the number of max flow.

Proof: Consider a flow of 1, we start from s and trace with one unit of flow. Every time we enter an internal node we can liver it as $f^{in}=f^{out}$ for such nodes. So the only place to stop in the sink. \
For now K units of flow: \
We can start from s and trace a path to t as above. Then if we remove this path we get k-1 path and we can apply induction hypothesis/continue in this manner until we have a flow of 0. \
So far we have shown that given a max-flow k $max-dij \geq k$  \
Now we will prove that k is the max number of disjoint path. Note that if we have r edge-disj paths then max-flow is at least r. So we can assign 1 unit of flow to every edge in these path, whatever is not in the paths assign 0 so $max-flo \geq r$. Hence $max-flow=max-dij$.

---
# 29-01-2018

## Same problem but this time with undirected graph

#### Goal
Find the maximum number of edge disjoint s t paths.

### Ideas
An undirected edge is an edge that can go in both directions.\
Hence we can replace every edge with two undirected edge going in opposite directions. 
> Personal thought: Can cause an issue if we run in both of them through our path as in reality it is only one edge.
> Maybe removing cycles can then be a solution.

My personal thought turned out to be correct. So we just run FF then remove all cycles including cycles of length 2.

## Multi Source-Multi-Sink flow
Similar to the original max-flow except that now we can have more than just 1 source and 1 flow. \
Just as before we want to generate the max unit of flow. \
The way of solving this is to create a source that has directed edges going to all of the sources in the graph with infinite capacity. \
We do the same thing for the sink a sink that only has edges directed towards it connected to all the other sink with infinite capacity. \
Now we can just run FF on the new graph using the new source and sink as the source and sink.

## Baseball Elimination problem
We have a tournament and every match the winning team gets 1 point.\
We are in the middle and each team has some points and some remaining matches.\
We are given a specific team A and we want to check if A has any chance of ending with the highest score (a tie is acceptable). \
Ex:
|Team|Matches 1 | Matches 2|Matches 3|points|
|:--:|:--:|:--:|:--:|:--:|
|A|B|C|-|68|
|B|A|C|D|69|
|C|A|B|D|70|
|D|C|B|-|69|
In this scenario at least one team ends up with > 70 

If we look at the 3 teams B,C,D they have 70+69*3+3=211 they will have all these points to share and 211/3>70 at least 1 team will have >70 points.

This does not always work however if we add a team into the mix that has lets say 10 points then the average technic does not work yet A is still eliminated.

Let M be the total points A will have if it wins all of its remaining matches. \
If A is eliminated then is it true that we can find a set T of teams such that if $\sum_{x \in T} \frac{P_x+k}{\mid T \mid}>M$ ? \
Where k is the number of remaining matches between teams in T and $P_x$ is the points of x. 

### Algorithm:
1. Let M be the max number of points that A can collect if it wins all the remaining matches. $M=P_a +deg(a)$ 
2. Remove A and its edges from the graph
3. Construct the following flow network: for every edge uv put a vertex (uv) in the network. Also for every team add a node. 
4. We add a source and sink. We connect the source to each node uv with capacity 1. To ensure that each node get at most one unit of flow. 
5. We want the flow to be distributed between the two teams u and v. So we add 2 edges of capacity 1 or infinity does not matter going from (uv) and to independently u and v. So now either u or v gets the 1 unit of flow/ one point.
6. We do not want any team to have more points than A so the capacity of each edge going from u to t must be $M-P_u$. If any of the team is already larger than M then we know that A is eliminated.
7. Then we solve the max flow. If the max flow is equal to the capacity of S then A can win. If the max flow is less than the capacity of S then A is eliminated.

---
# 31-01-2018
We know tha maxflow=min-cut what does the mean-cut tell us?

## What can we say about mean-cut from the example above
* $Mean-cut \neq \infty, A=\{s\},B=\{s\}^c$ 
* Consider min-cut (A,B). If $(XY) \in A \rightarrow X \in A, \ Y \in B$
* Let T be the set of teams in A from cut (A,B). $cap(A,B)= \sum_{X \in T} (M-P_x)+\sum_{s(xy) \in B} 1 = \sum_{X \in T} (M-P_x)-K= max-flow$.  If max flow is < # edges of S. 


 Max-flow=$cap(A,B) = M* \mid T \mid - \sum_{X \in T} P_X + K <=> M* \mid T \mid - \sum_{X \in T} P_X + K <  \# of edges$ \
$M* \mid T \mid < \# edges \ in \ T + \sum_{X \in T} P_x$ \
With this we get the formula that we saw above. \
#### Theorem: If our team is eliminated => There exists a set of teams T that provides a proof that the sum of their points plus the points they are sharing divided by the number of team gives an answer greater than the max score of A.

## Project Selection:
Problem: We are given a set of projects each project x has a revenue $P_x$ : if $P_x>0$ => it provides some positive revenue. If $P_x < 0$ it provides a loss. Some project are required to be done for other projects to be accessible. An edge x -> y means that y is a prerequisite for x. (if we choose x then we also have to choose y. Just like class prereq ). \
Goal: Select a subset of projects that respects all the prerequisite and maximizes the total revenue. 

1. Assign a capacity of infinity to all the edges so that they all fall in part A of the subset in order to use min-cut.
2. Add a source s and a sink t. Add edges from the source to project with positive profit and add edges going from nodes to t for the one with negative profit. for su edge Csu=Vu and for vt edges Cvt= -Vv (value of v is negative so we multiply it by -1 to make the capacity positive.).
3. Then we look at the min-cut

Let (A,B) be a min-cut and let $M=\sum_{x: P_x>0} P_x$. 
min-cut$= \sum_{x\in B: P_x>0} P_x+\sum_{x \in A: P_x<0} -P_x$ \
$= M-\sum_{x\in A: P_x>0} P_x+\sum_{x \in A: P_x<0} - P_x$ \
$M-\sum_{x\in A} P_x$ \
This works because M is the sum of all positive projects If a positive project exist in A then all its prerequ exist in A hence if we do M -all of the positive projects in A we are left with the sum of all the positive projects that are in B.\
We know that the projects in A respect the prerequ conditions hence the total profit we can make is the sum of all positive projects in A minus the sum of all negative projects in A. And this is maximize as we have M-that sum in order to get the lowest min cut.

---
# 05-02-2018
## Linear programming
Special case of optimization problems: optimizing a linear function with a certain number of variables over a set of linear constrains.
* Many optimization problems can be modelled as Linear Programs.
* 40's: A very practical algorithm (called simplex) discovered for solving LP's. Theoretically it is not an efficient algorithm. (exponential time) but in practice it's almost always very fast.
* 79 (Leonid Khachiyan) proved that LP's can be solved in polynomial time. Elipsoid algorithm.

A linear program has:
1. A set of variables: $x_1,x_2,...,x_n$ that can take real values.
2. A set of linear constrains each of the form of $\alpha_1x_1+\alpha_2x_2+...+\alpha_nx_n$, $= \beta$, $\leq \beta$, $\geq \beta$ Where $\alpha$ are real numbers and the strictly less than or greater than are not allowed.
3. A linear objective function that we want to minimize or maximize. $c_1x_1+c_2x_2+...+c_nx_n$ where the c are real numbers.

### Examples:
Variables $x_1,x_2,x_3$ max $2x_1+5x_2-x_3$ subject to $x_1+x_2+x_3 \leq 5$, $2x_1+6x_2-x_3 \leq 1$ and $x_1-2x_2-x_3 = 3$

Variables $x_1,x_2,x_3$ max $x_1+x_2+x_3$ subject to $x_1+2x_2\leq 1$, $2x_1+x_2\leq 1$, $x_1 \leq 0$, $x_2 \leq 0$ and $x_3 = 1$ \
Ans= $x_1 = \frac{1}{3}$, $x_2=\frac{1}{3}$ and $x_3=1$ gives $\frac{5}{3}$. 
If we combine the 3 equations so that we get the initial condition is leq or equal to something then we know that we cannot do better than that. 

### Applied example
We have a small firm the produces 2 things, book cases and tables. \
||cutting time|Assembly|Finishing time|
|:---:|:---:|:---:|:---:|:---:|
|Bookcase| 6/5 hr | 1 hr | 3/2 hr |
|Table|1 hr | 1/2 hr | 2 hr|
We have \
|hours|domain|
|:---:|:---:|
|72|cutting time|
|50| Assembly|
|120|finishing time| \
We can sell a book case 80$ and table 55$ \
We can have 2 variables book case and table \
Maximize: $80b+55t$ \
Constrains: \
$\frac{6}{5}b+t\leq72$ \
$b+\frac{1}{2}t\leq50$ \
$\frac{3}{2}b+2t\leq120$ \
$b \geq 0$ \
$t \geq 0$ \
Remark: we wish to add that b ant t are integers but adding that is not allowed in linear programs.(We cannot solve such optimization problems efficiently). \
If we solve the LP we will get $t=30 \ x_2=35$ so fortunately in this case the optimal solutions are integers. 

Two factories $P_1 \ P_2$ four products A,B,C,D
||$P_1$ per day|$P_2$ per day|Demand Total|
|:---:|:---:|:---:|:---:|:---:|
|A|200| 100 | 1000 |
|B|60 | 200 | 800|
|C|90 | 150 | 900|
|D|130| 80 | 1500|
Cost of $P_1$= $800/day \
Cost of $P_2$=$1100/day 

Minimize function:
$800x_1+1100x_2$ \
Constrains: \
$200x_1+100x_2 \leq 1000$ \
$60x_1+200x_2 \leq 800$ \
$90x_1+150x_2 \leq 900$ \
$130x_1+80x_2 \leq 1500$ \
$x_1 \geq 0$ \ 
$x_2 \geq 0$

Solution: $x_1=11.132 \ x_2 = 0.66$ \

#### We are expected to model the problem as a linear program not to actually solve it. 

### Model the problems:
Find the max flow on this network:
* We have as many variables as they are edges representing the flow in each one of them.
* Objective maximize flow coming out of s
* Constrains:
    * The flow must be ≥ 0 for every edge
    * The flow must be ≥ to the capacity of every edge
    * fin of a node must be equal to fout of that node (incoming variables - outcoming variables flow equals 0) 

Max flow is a special case of linear programs

---
# 07-02-2018
### Examples linear continue 
suppose some edges have also lower bounds. That is an edge e with lower bounds le requires that the flow on e has to be at least le. \
In this case it is very easy to solve as we can just add some limits with. e ≤ Ce and e ≥ le.

We can add costs to edges. Every edge e has a cost de that is passing fe unit of flow through the edge costs de*fe. \
What is max flow if we have a flow cost at most d. 
* Max all the edges going away from s
* Constrains are all the same than regular max flow
* We add the sum of every edge cost times its flow must be less or equal to d.

We have a network of road's between cities, given to us as an undirected graph. We are also given a positive number alpha ≥ 0. We want to store some amount of supply in each city so that the sym supply in each city and its neighboring cities is at least alpha. What is the minimum amount of supply that we need to meet this condition.
* Variable represent each city.
* When we add each variable we want to get the minimum value
* Constrains:
    * Each city and the city it can reach through roads sum of supply must be at least alpha.
    * The supplies must all be greater or equal to 0.

For a general graph G we can write this as: \
variables: $\forall v \in V$ we have a variable $x_v$ and we are minimize $\sum_{v \in V} x_v$ subject to the following: 
* $x_v + \sum_{u: uv \in E} x_u \geq \alpha$
* $x_v \geq 0$

## Geometric Interpretations of linear programs
Consider the following LP.
* max x1+x2
* Constrains
    * x1+2x2 ≤ 1
    * 2x1+x2 ≤ 1
    * x1, x2 ≥ 0
Every potential solution x1,x2 gives us a point (x1,x2) on the plane. Then we can graph the constrains as lines. 
* Everything bellow the line x1+2x2=1 will solve the first constrain.
* Same thing for 2x1+x2=1
* For the x1,x2 ≥ 0 every point in the positive quadrant.

The set of points that satisfy the constrains can then be identified using the space where the 4 areas exists. \
This is called feasible region: it is the set of all points that satisfy all the constrains.

Remark: here every constrains with inequality gives us a half space. Which is the set of points that satisfy that constrains. The feasibility region is the intersection of them.

If we just draw lines such that x1+x2=x3. Where each line falls in the feasible region. The line in our case that crosses the region and has the highest possible x3 is what we look for. So we can draw a line above the region

Some regions may be unbounded. Or there may be cases where the regions never cross hence there will be no solutions. 

So there are 3 possible cases overall:
1. Bounded
2. Unbounded
3. Empty

---
# 12-02-2018
## Feasible region: 
Is the set of all solutions that satisfy all the constrains. \
It can be:
* Empty
* Unbounded 
* Bounded. Inside a convex polygon.

#### Convex: the line segment between any two points in the region falls into the region. 

## Getting to simplex algorithm idea
Associate every equations with each other to get points on the feasible region. \
Then we can move to points on the graph by still respecting the other conditions by going along lines from these points. \
Then we check if these new points made our optimizing equation better. We avoid the local minimum due to the fact that the shape is convex there is no local min. If we hence get to a point where changing it will decrease the value then we can stop. \
Personal thoughts: Can we say the same if we can move yet not improve our function?

## Linear programs standard forms
A linear program is in a standard form if it is in one of the following forms:
1. Max $c_1x_1+...+c_nx_n$ \
    $a_1x_1+...a_nx_n \leq b_1$ \
    $x_1,x_2,x_n \geq 0$
2. min $c_1x_1+...+c_nx_n$ \
    $a_1x_1+...+a_nx_n \geq b_1$ \
    $x_1,x_2,x_n \geq 0$

#### Ex:
    max x1+x2
    x1+2x2 ≤ 1
    2x1+x2 ≤ 1
    x1,x2 ≥ 0 
### Can we convert every linear program to standard form?
    max x1+x2+2x3
    x1+6x2+x3 ≤ a
    x1-x2+x3 ≥ 1
    x1+2x2-x3=-2
    x1 ≥ 0
    x3 ≤ 0
So the bad lines are:   

    x1-x2+x3 ≥ 1 -> * -1 -x1+x2-x3 ≤ -1 
    x1+2x2-x3=-2 -> x1+2x2-x3 ≤ -2, -x1-2x2+x3 ≤ 2
    x1 ≥ 0
    x3 ≤ 0 -> x3=-x4 x4 ≥ 0
So we must rewrite everything now

    max x1+x2-2x4
    x1+6x2-x4 ≤ a
    x-x1+x2+x4 ≤ -1 
    x1+2x2+x4 ≤ -2, -x1-2x2-x4 ≤ 2
    x1 ≥ 0
    x4 ≥ 0
    x2 -> x2=x5-x6
    x5 ≥ 0
    x6 ≥ 0
so rewriting gives us:

    max x1+x6-x5-2x4
    x1+6(x5-x6)-x4 ≤ a
    x-x1+x5-x6+x4 ≤ -1 
    x1+2(x5-x6)+x4 ≤ -2, -x1-2(x5-x6)-x4 ≤ 2
    x1 ≥ 0
    x4 ≥ 0
    x5 ≥ 0
    x6 ≥ 0

This was a very generic example and we saw how to deal with all of them. 
### Why do we care about standard form?
c= 
|v| 
|---:|
|c1|
|c2|
|...|
|cn|

a = Matrix of constrains \
b = represent the expected results \
x = to an other vector \
So we can take the dot product of c and x to get the max/min equation. \
For the constrains in the matrix we just need to multiply the matrix by x transpose. And the vector b gives us there lesseq or greateq values. So we can hence write everything in a very compact way with just 3 vectors and a matrix.

## Duality
Consider the following LP (in standard form). 

    max x1+2x2+x3+x4
    x1+2x2+x3 ≤ 2
    x2+x4 ≤ 1
    x1 + 2x3 ≤ 1
    x1,x2,x3,x4 ≥ 0
Suppose the LP solver finds the solution

    x1=1
    x2-1/2
    x3=0
    x4=1/2
    max = 5/2
How can we convince ourselves that this is optimal (without solving lP ourselves). \
So can we use these constrains to show that 

    x1+2x2+x3+x4 ≥ 5/2
We can multiply the constraints by positive numbers y and add them up. 

    (y1+y3)x1+(2yi+y2)x2+(y1+2y3)x3+y2x4 ≤ 2y1+y2+y3
we want

    x1+2x2+x3+x4 ≤ (y1+y3)x1+(2y1+y2)x2+(y1+2y3)3+y2x4
    ≤2y1+y2+y3
What do we need to know about y1,y2,y3? to guarantee the first inequality? We already assumed y1,y2,y3 ≥ 0 (otherwise we could not multiply the constrains by them). \
We also need:

    y1+y3≥1
    2y1+y2≥2
    y1+2y3≥1
    y2 ≥ 1
If these are satisfied then we will have the upper bound 

    2y1+y2+y3
So to get the best upper bound we need to solve
    
    min 2y1+y2+y3 = 5/2
    y1+y3≥1
    2y1+y2≥2
    y1+2y3≥1
    y2 ≥ 1
    y1,y2,y3 ≥ 0
    y1=1/2
    y2=1
    y3=1/2

This only works when we are in standard form  that are the dual of each other. 

---
# 19-02-2018
### From last time
We already know that the optimal is 5/2 ≥.\
We want to show that x1+2x2+x3+x4 ≤ 5/2\
We can deduce new constrains from the constrains above ex;\
* x1+x2+x3≤2
* x2+x4≤4

Then x1+3x2+x3+x4 ≤ 3

* x2+x4≤1
* x1+2x3≤1

Then 3x1+x2+6x3+x4≤4

So in our case we will modifiy the constrains like this:
*   x1+2x2+x3 ≤ 2 * y1
*   x2+x4 ≤ 1 * y2
*   x1 + 2x3 ≤ 1 *y3
*   x1,x2,x3,x4 ≥ 0

*   (y1+y3)(x1)+(2y1+y2)x2+(y1+2y2)x3+y2x4≤ 2y1+y2+y3

Given that the y's are positive. If we find y1,y2,y3 positive so that the coefficient are:
* y1+y3 ≥ 1
* 2y1+y2≥ 2
* y1+2y3≥1
* y2≥1

In that case we get:
* x1+2x2+x3+x4 ≤ (y1+y3)(x1)+(2y1+y2)x2+(y1+2y2)x3+y2x4 ≤ 2y1+y2+y3

So the best upper bound here because we are interested in getting it the lowest possible to show that the original equation cannot be greater than a certain number:
min 2y1+y2+y3
* y1+y3 ≥ 1
* 2y1+y2≥ 2
* y1+2y3≥1
* y2≥1
* y1,y2,y3≥ 0

These is called the dual LP because we are doing the opposite of the original which is the primal LP. OPT(Primal LP)≤OPT(Dual LP)\
In this case we can set y1=1/2, y2=1 and y2=1/2 gives us 5/2 which makes opt(Primal LP)≤5/2. \
Hence remark if we find something for both dual and primal that works for both of them then this is the optimal solution for both of them. 

### Dual for standard LP:
max c1x1+c2x2+...+cnxn \
a11x1+a12+...a1m ≤ b1\
. \
. \
. \
am1x1+am2x2+...ammxmm ≤ bm

#### so now Dual is:
min b1y1+...+bmym\
a11x1+a12+...am1 ≤ c1\
.\
.\
.\
a1mx1+am2+...amm ≤ c2\
all y≥0

Remark: Value of every feasible solutions in primal is upper bounded by the value of every feasible solutions in dual

If primal is unbounded then the dual is infeasible/ has no solutions and vice versa.

Thm(Weak duality)
1. If primal is unbounded then dual is unsatisfiable. 
2. If the dual is unbounded then the primal is unsatisfiable
3. If both primal and dual are feasible then opt(primal) ≤ opt(Dual) for maximization. 

This similar to max-flow ≤ min-cut

#### Writting the dual without converting to STD form
max x1+2x2+3x3
* x1-x2-x3 ≤ 1
* 5x1+x2+2x3 ≥ 4
* 3x1+2x3-x3 = 3
* x1 ≥ 0
* x2 ≤ 0
* x3 free

min y1+4y2+3y3
* y1+5y2+3y3 ≥ 1
* -y1+y2+2y3 ≤ 2
* -y1+2y2-y3 = 3
* y1 ≥ 0
* y2 ≤ 0
* y3 free

### Thm(Strong duality):
If primal and dual are both feasible then opt(primal) = opt(dual)

