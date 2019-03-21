# Lecture 1
__Def__: developping models and algorithms that can produce rational behaviors in response to incming stimulus and information.

Thinking is hard to model and understand since it is hard to measure, many people tried to model thinking but until now we are not sure of what method is and perform the best. The best way to measure progress is to look at the system behavior and compare that to some standards (acting) since they are concrete and we can measure them we can then judge of the action.

## Acting humanly
Hard to define since we are all different, it may not always be the best approach and finally no AI has yet to act as a human.

### Turing Test
Can a machine think and if so how could we tell?  
The most general test is to say that an investigator interacts with a system that can be either a machine or a human, the investigator has to guess which one in a finite amount of time.

### Rational acting
We define some mathematical function acting as a proxy to what we want, the goal is to maximize the function given available information and available resources. _May not require thinking but could_.

### Rational agents
Agents will __perceives__ and __acts__. Over time it will learn 
a function mapping percept histories to actions, the rational 
agent tries to maximize that function.

# 09/01/19
# Uniformed Search
## Typical Setup of AI
- Knowledge
  - Formal description of the problem
  - Def of state, goal, avail actions
  - Other factual or proba knowledge
- Search
  - Process of finding a solution or the best solution to the problem

## Defining a state problem
State space S: all possible configuration of the domain  
Initial state s0 in S: the start state  
Goal state G subset of S the set of end states  
Operators A: the actions available.

Path: squence of state and operators.  
Path cost: a number assocaition with any path  
Solution: a path from s0 to G
Optimal solution: a path from s0 to G with min or max path cost

### Basic assumptions about the above
1. The environment is static (usually dynamic in real life)
2. The environment is observable (certain things can not always be observed)
3. we can use discrete states only (no continous state) for location for example
4. Determanistic if we take some steps the outcome is certain (usally stocastic)

## Search Graph problem
Each node represents of states and the edges represent the operation to transition from one state to another.

### Search tree
Represent the exploration path in search procedure.  
Node represent partial solution including a state in S  
Eges correspond to operators. (basically from the initial state we try to reach some other state expending it as we move on and then choose the best solution)

#### Node
In the search tree the node can also store the parent state and the operator used to generate it, the cost of the path so far, the depth of the node.

## Generic Search Alg
1. if no nodes can be expended return failure
2. chse a node ofr axpansion according to some strat
3. if the node contains a goal state retrun the path
4. else expend the node

### Uninformed search
if a state is not a goal we do not know how close we are to be done. So we can just move between states until we reach the goal. Informed (heuristic) search uses a guess on how close we are to the goal a state might be. **Breadth, depth** first search.

## Properties of search algorithms
Completeness: assured to find a solution?  
Optimality: how good is the solution?  
Space complexity: quantity of storage? 
Time complexity: how many operations needed?  

### Search complexity
Branching factor: how many new leaf/branch are we creating from a node  
Solution depth: how long is the path to the closest goal state.

We then apply Big O on these to see what we get.

### BFS
Is complete, garentees to find the shortest path but not the least cost if the graph is weighted. It is however it is exponential in time and space complexity:  
$$O(b^d)$$

### DFS
Same as BFS but with O(bm) space instead. Can be more efficient of BFS if there are many paths leading to the solution. The time complexity remains O(b^m), it is not optimal either in length or in weight. DFS may also not complete, never use DFS if we suspect a big tree depth.

### Uniform Cost Search
Fix BFS to ensure an optimal path with genreal step costs.

We will use a priority queue instead of a simple queue so we explore nodes costing the least first.

### Depth limited search
Depth first search but stop if the goal is reached or if the max depth allowed is reached. This forces the algorithm to always terminate but in this case it may not be complete.

### Iterative deepening search
Do the depth limited search and increasing the depth. It is complete.

# 14/01/19
# Informed search algorithm
Uninformed search looks at the depth or cost that is spent from the start node called **d**. But informes search looks at the distance to the goal node. Regarding the 8 tile games it would be how many tiles are misplaced for example.

## Heuristics
Set a rough estimate of the distance from the goal node in the case of travel we could draw a line from the current node to the goal node and calculate the length.

For the 8 puzzle problem a good heursitic could be the number of moves required to place a tile from its current position to its correct location. (Manhatan distance). **h2**

Another one would be the number of missplaced tile **h1**

### Relaxed version of the problem
h1 in this case assume that you can move a tile from its position to a new one in one move.

h2 assumes that there is no need for a blank square to be available for the tiles to move

## Best First search
Expend to the mose promising node according to the heuristic this disregards the cost of the move though so that algorithm is not perfect cost wise.

If the heuristic is always 0 then it acts as BFS. Uniform cost search considers the cost so far compare to best first search that tries to minimize the cose to go.

Best first search in worst case is O(b^s) branchigng factor, d= solution depth (same as BFS). But with a good heuristic we can do O(bd).

## heuristic search
f = h + g  
where h is the heuristic algorithm and g is the cost of the path so far.  
We use a priority queue and only end when the goal state is poped from the queue to have the optimal solution according to both value is found.

**heuristics like that tend to be too gready what matters is to have a heuristics that is always less or equal to the actual cost to achieve the goal**

## A * search
Applying the heuristic algorithm with an f functiont that respeects the admissible heuristic rule.

### Proof of optimality
Assume that we have a sub optimal goal G2 in the priority Q and let n be an unexpended node on a shortest path to optimal node G1.

We have f(G2) = G(2) + 0 > f(G1) ≥ f(n)

So A* will select n for expension before G2 this applies to every node on the path to G1 thus G1 will be reached before G2.



### Consistency/monotone of heuristic functions
h(s) ≤ c(s,s') = h(s') where for every state s and a successor s' that equation is respected this is slightly stronger property than admissibility.

If this is not resepcted we can fix it by taking the max between g(s') + h(s'), f(s).

### Dominance 
If h2(n) ≥ h1(n) for all n then h2 dominates h1 (meaning thatn h2 is more informative than h1).

### Iterative deepening A* (IDA*)
Same as A * search but f value to decide in which order to consider the descendents of a node. It keeps the same properties as A* but uses less memory.

**SMA** is the version where you keep old nodes when rexpending.

### Learning heuristics functions
Intution is good but it is still limited and up to the individual.

The idea is that rather than writting it down we will write a general idea of the heuristic functions and run the game on multiple samples and by solving each of them we may then get an insight to a better heuristic function.

1. Genreate features that describe a particular state of the game x1(n),x2(n) and so on
2. Form the heuristic function h(n) = c1x1(n) + c2x2(n)
3. Set a particular target for the heuristic function and then use maching learning to find better c1 and c2 to match this target.

# 16/01/19
We are going to look at a model where the cost is some function Eval(X) and we want to find the best solution X*. 

Given a set of task to be completed figure out the best schedule for these tasks.

Given a board made of components and connections place the componentes to maximize energy efficiency and minimize production cost.

custumer described by (age, gender, location) and prev purchase
Find the product that the custumer will most probably buy which gives maximum revenue.

## Type of search so far
- Constructiv method seen so far
  - Start from scratch and gradually build up a solution.
- Iterative improv/repair method
  - start with a soltion and improve it which is what we will see today.

## Iterative
Start with a set of states/configurations and and evaluation function. In this case a state is a candidate solution not a description of the world. This is used when the state space is too big to be generated and the computation is too expensive to be done.


### Generic local search alg
Start of an initial configuration and walk down the gradient by selecting neighbouts of the initial config evaluate each of them and select the lowest one keep doing that until no neighbours are better.

Defining set of neighbours is a design choice and has a crucial inpact on performance. We call that a neighbourhood function.

#### What move to consider?
- Search for high ground
  - Start at initial state
  - Move to adjacent postion
  - Terminate when goal is reached
- SalesMan
  - Start intial state
  - Swap cities 
  - Stop when the goal is met.

## Hill Climbing
Gradient ascent start form X0 with value E(X0), generate the neighbour set with their values and compare that to our current solution, if the best neighbour is worse that means that we are at an optimal point and we terminate the algorithm.

Else we set our new location to the best neighbour.

This is popular as it requires no memory (no backtrack) trivial to implement (just eval neighbours) and it can handle very large problems.

The neibourhood function is important: large neigh more computation but maybe fewer local optima so better result. Fewer neigh less calc but possibly worse solution.

## Travelling Salesman Problem
Given a set of vertices and a distance between each, find the shortest path that vistis every vertex exactly once given a start city.

Here one state is a proposed tour that we a gradually going to improve to find tours with gradually shortest length (the best solution is NP complete).

### Neighbourhoud func
**Swap 2 edges**: takes (n 2) combinations since we have n edges in a tour and swap two of them

**swap 3 edges**: so (n 3) combinations

## Improv hill climbing
### Quick fix
When reaching a plateau or local maximum use random restart. But this is expensive as we have to run the procedure mutliple times

### Better fix
Instead of picking the next best move that gives an improvement.

### Simulated annealing
Takes some bad moces to try to escape local maxima and decrease the frequency of these bad moves over time.

The only change is that we store the max seen so far, then we move to the max seen as neigh or we accept worse solutions with prob p.

#### what about P
- fixed p
- Value decay to 0 overtime
- decay's to 0 and give similar chance to similar bad moves
- A value that depends on how much worse the value is.

Boltzmann distrivution
$$ p = e^{\frac{-(E-E_i)}{T}}$$
T>0 as a param called temperture (start with high temp and go down slowly).  
We decrease T by mutliplying it by a constant alpha that is between 0 and 1.

If T decreases slowly enough then the algorithm is garenteed to reach the optimal solution. *slowly enough* however could be extremely slow.

This makes sense as first we improve until we reach a local optimal then since we stay there for a longer amount of time thatn we have more chances of going down hill again to reach another maximum.

### Monte-Carlo Search
Looking arround the environment and explore it instead of trying to always improve.

## Parallel search
Run many parallel search on different cores and keep the best solution found so far in memory. Useful when actions have non-determanistic outcome (not sure of how good the next solution is)

## Local beam search
Run many instances of local search at the same time but in this case make them share info between each other.

- Start k searches in parallel
- At each step keep the top k solutions in the sets of neighbouthoods discard the remaining 
- k is called **beam width**.

## Evalutionary computing
Nature usually makes the most adapted individual to survive and reproduce. Evolutionary search procedure are also parallel.

## Genetic algorithm
A candidate soltion is called an individual which has a fitness (value that tells how good that individual is).

A set of individual is called a population. Population change ocer generation by applying operations to individuals. Individuals with higher fitness are more likely to survive and reproduce. They are usually represented by a binary string

Good because they are intuitive, and can be affective if tuned right



### Mutation
Select a random entry and change its value

### Cross over
Cross over bits between individuals (slingle, two point, unifor, point mutation)

### Elitism
The best solution dies during evolution to prevent that we always preserve the best solution. 

# 21/01/19
# Constrain satisfaction problem
We want to find some solution or no solution that satisfies all the constraines for every variable.

There complexity ranges from P to NP-complete problems to NP-Hard.

#### Australia coloring example
- Variables: WA, NT, Q, NSW, V, SA, T
- Domains: Di = {red, green, blue}
- Constrains: adjacent regions must have different colors

We then look for completness (the goal is achieved) and complete the constrains are respected.

### Variaties of constraints
- Unary: one variable and one constraint
- Binary: 2 variables
- Higher-order: involve 3 or more variables
- Relations:
  - T1 + d1 ≤ T2
  - Alldiff(v in row), Alldiff(v in col), allDiff(3x3 square) SUDOKU
- Preferences (soft constrains): can be represented by a costs and lead to constrained optimization problems.

### Real-Wolrd CSP
- TimeTable prob (which class is offered when, where)
- Hardware Configuration
- Floor spanning, transportation
- Puzzle solving

## Solving CSP
### Constructive
- State: defined the variables assigned so far
- Initial state: no variable is currently assigned
- Operators: assing value to unassigned variables
- Goal test: all variables assigned, all constrains satisfies

Depth is limited to the number of variables, n. So we can apply DFS or depth limited search

This is a complete and optimal approach regarding the constrains

The complexity is n\*d + n-1\*d + ... = n!*d^n.

### Constrain graph
- Nodes are variables
- Arc/edges show constrains
- Graph structure can be exploited to accelerate solution search

Try to avoid CLICS as if you get them it is hard to optimize.

#### Perfrom inference
Pre process the graph  to remove obvious consistencies.
- Varaible is arc consistent if: potential values that its domain can take are consistent with its binary constrains (verify that variable associated with other variables respects the constrain)
- Generalized arc consistent: every value in the domain for each variable are simultaneously arc consistent

### BackTracking search
- Select variable X
- for each value in the domain if it satisifies exit the loop and go to the next
- else if no assignment was found go to the previous var

Works for n=25

### Forward Search
- When assigning value to X
- Look at all values each Y connected to X can take
- Delete the ones that do not respect the constrains with X

n=30 

### Commun Heuristics
- Minimum-remaining values Choose the variable that has the most constrained (fewest legal values)
- Degree heuristic: choose the variable that causes the most constrains on the remaining variables (can be used to break ties.)
- Least-constrain value: Assign value that rules out fewest values for other vars

Tree structure constraint graph: complexity is O(nd^2)

No need to know the cut set conditioning but here to show that there are ways to change a graph to a tree each

## Local Search for CSP
1. Start with broken but complete assignments of variables
2. Randomely selec any conflicted variables
3. Reassigned values using min conflict: choose value that violates the fewest number of constrains

aka Hill climbing opt

### Performance of min-conflict
Given random inital state cans love n-queens in almost constant time for n queens with high prob with n=10^7.

Many solution mixed with random initilaize (what are the chances that we get high constrains?) making that runtime slower.
R = num const / num var that ration shows that most of the time random assingment of CSP will work well except for a specific ratio.

# 23/01/19
# Uncertainty search
## Source of uncertainty
1. Agaent does not know that outcomes of its actions (non determanistic actions)
2. Agent may not be able to observe which test it is in

Consider the case where the next state is not fully determined by the current state and action i.e: each action can have many outcomes depending on outcome of dice role.

## Problem example Vaccum
- two rooms
- Can be clean or dirty
- Vacuum can be in one of the two rooms

This means 8 different combinations so 8 state. The goal state is the state where both rooms are clean.

When we cannot observe anything the startegy is to go left then sweep then go right and then sweep.

## General strategie
We have a *belief* the set of state we could currently be in. So we we have 8 state then the total number of possible belief is 2^8-1 since we must include at least one state in our belief.

Note however that not all set of state can be reachable (not consistent with each other) for example if we sweep one room the state where the two rooms are dirty cannot exists in our belief set of state.

We want to reach the *belief* set of state where all state there are goal states.

A good heuristic is to use actions that reduce uncertainty (the size of the belief set of state). Standard search strategy can be used when you know that you can be and and can reach only one state.

## Non-determanistic but observable
We know what state we are in but we are not sure of the efficiency or the way the vaccum works. Now we need to have *or* and *and* statement in the tree to account for the random actions of the vaccum.

The solution is no longer a path but a subtree that tells you what actions to perform based on the AND node outcome. The tree can be generated by doing breadth or best first search.

### Issues of that structure
1. The number of reachable beliefs can be very large
2. Number of state in each belief can be large

Use sampling or puring to reduce the number of reachable state. Try to compact the state as much as possible, search for a separate state in the belief to see if the set of state can lead to a solvable solution.

# 28/01/19
# Adverserial search
Games with 2 players where we compete with another player. 
This is useful since games represent a good stimulation of multiple agent systems, it is a good training example for AI makers since the rules of a games are usually wellknown the goal is easy to reach "Winning". Some games are hard to solve too so it can be usefull to have an agent that can solve it. 

### Types of games
- perfect information
  - Can see the entire state of the game
- imperfect info
- Determanistic
  - change in state is determined by player's move
- Stochastic
  - Chane in state is partially determined by change.

## Game playing as search
- State: state of the board, player turn
- Operators: legal move
- Goal: state where the game is lost, won, draw
- Cost: +1 ofr winnint, -1 for loosing, 0 for draw.

The strategy involve picking up moves that maximize utility of win or min cost.

The key idea is to have two player, on our side we want to maximize our utility function but the other player wants to minimize our utility function.

At each max node we look at the max value we can get and for the min node we do the opposit.

### Assumptions
- Both player are playing on the same utility function
- We think that the min player is following the same strategie than us.

### Properties of MIN MAX search
- complete: if the tree is finite
- Optimal: if the oponant plays optimally
- Complexity: O(b^m)
- Space complexity: O(bm) for BFS


## Evaluation function
v(s) represents the goodness of a board state (chance of winning for that state). It can be given or learned from experience. We can use multiple functions like this.

#### Chess example
f1(s) = # white quess - # black queens    
f2(s) = # number of white pawns - # number of black pawns  
w1 = 9    
w2 = 3  
v(s) = w1f1(s) + w2f2(s)

These functions represent heuristic, it is easier to design these functions towards the end of the game. We apply the same idea of MIN MAX. (read the slide could help)

We use these evaluation functions to evaluate non-terminal nodes. 

** Minmax cutoff algorithm, appplay the eval function at each of the leaf node after going down a depth of m.

## Alpha beta pruning (better min max)
If a path looks worse than what we already know about then we should discard it.

The algorithm keeps track of best leaf for us and worse leaf for opponent. At max node only update alpha, at min node only update beta.

This algorithm a demonstration of reasoning about which computation are important. In this case the order matters for the efficiency. An option is to add a heuristic that can help us improve the order or that efficiency.

On average O(b^{3m/4}) to find it after b/2 expensions.  
With perfect ordering we can get O(b^m/2) but the worse remains O(b^m).

In practice if the branching factor is bing the search depth is still too limited. If the oponent does not play accroding to the same heuristic function as the player which is a big **assumption**.

### Forward pruning
Only explore n best moves for current state according to the evaluation functions. This can lead to sub optimal solution. can be efficient with good evalution functions.

## Chinook system
The best checker player, perform alpha beta pruning on an evaluation function created by the best checker player. They have an opening and huge end game database. It has perfect play for 8 or fewers pieces on the board. Only middle move of the game are are actually search.

# 30/01/19
## Random stimulations
1. Simlate games by randomly selecitng moves for both player
2. At the end of each see if we won, lost, draw. Keep track of the initial move that lead to that result
3. After the stimulation pick the move that led to the highest win rate

This is called the Monte Carlo Method

### Where to spend the effort
Allocate more runs to more promessing moves. Min max could still be done near the top and then divide that search according to that example.

Base on the win rate of the stimulation we can redistribute our left trials.

## Monte Carlo tree search (MCTS)
1. Select promessing node in the search tree using a tree policy. Mapping State to an Action for all states
2. Sample possible continuations from leaf nodes using a randomized default policy
3. Value of a move is the average of the evaluations from its smpled lines
4. Pick the move with the best average.

### Tree Policy
We need to balance:  
Exploitation: a promessing node is explored more  
Explortation: a node that has not received many simulation yet, so we would like more info  

Define the following:
- Q(s,a) value of taking an action from state s
- n(s,a) number of time action a was taken from state s
- n(s) number of time s was visited in stimulation
- c Scaling constant

$$ \hat{Q}(s,a) = Q(s,a) + c \sqrt{\frac{\log(n(s))}{n(s,a)}}$$

Q(a,s) is the exploitation the rest is the exploration

The parrent node has always 1 extra default play through what it was genrated but not its children. 

### Rapid Action value estimate (RAVE)
Assume the value of a move is the same no matter when the move is played.  
This reduces the space but allows same moves to be calculated faster. I personnaly wonder how accurate that it because the state of the board matters a lot I guess putting a knight on a specific place if there is a queen for example is way better than putting the knight there where there is no queen, I assume that can be tweeked I guess my perspective remains still a bit broads.

# 04/02/19
# Complex game problems
Game such as Civilization are harder to model we need to represent their states and reason about them.

## Knowledge representation
An agent must be able to perfmor many tasks in relation to the **perception** of the state it is is and the **cognition** what actions to take. 

Recognition implies some form of representation (how knowledge is represented)

Inference is required when the agent makes a decision on the right action based on what it knows and what it can figure out (implicitly known)

## Declarative problem solving
The long term dream of AI. A declarative agent to building agents, divided into parts
1. Knowledge based (domain specific contant): contain the set of facts in some formal standard language (rules of the game for example)
2. The inference engine (domain independent algorithms): with general rules for deducing new facts and drawing conclusions. (algorithm that figures out what must be done)

This is the long term AI goal as we can just change the knowledge base and let the AI figure out the rest based on its domain independent algorithm.

### Example with the Wumpus world
A board game 4x4 and we want to retrieve the gold without getting cought by the monster the "Wumpus" the world is static thing but you do not move.

# Logic
Formal languages for representing information such that conclusions can be drawn.
- Syntax defines which sentences are allowed in the language
- Semantics define the meaning of sentences

## Type of logic
|Language | Commitment | Comm |
|--|--|--|
|Propositional logic|facts|true/false/unknwon|
|First order logic| facts, object, orientation | true/false/unknwon|
|Temporal logic| facts, objects, relations, times | true/false/unknown
|Probability theory| facts | degree of belief [0,1]
|Fuzzy logic| degree of truth | degee of belief [0,1]

### Propositional logic
Assertion about the state of the world/game/problem can be true or false.

Syntax include assuming x and y are sentences:  
neg, and, or, implies, if and only if

The interpretation sepcifies true/false value for each propositonal symbol.

Terminology of a setence:
- valid: true in all interpretation
- satisfiable: true in at least one iterpretation
- unsatisfiable: false in all interpretation

## Entailment
A rule for generating new sentences that are always true.

Knowledge base KB entails setnence alpha fi and olny if alpha is truee in all worlds where KB is truee.

$$ KB \models \alpha \iff KB \implies \alpha$$

## Inference
Lets call that inference procedure i.  
$$ KB \vdash_i \alpha$$
Means that the sentence alpha can be derived from KB by inferencey procedure i.

Procedure i must be:  
sound: 
$$ KB \vdash_i \alpha \implies KB \models \alpha$$
complete:  
$$ KB \models \alpha \implies KB \vdash_i \alpha$$

To check for such alpha when moving in the Wompus world for example we can use a truth table and check the possiblilities but the time and state space is a bit too large

### Two kinds of inference methods
1. Model checking (bad one with the table)
2. Application of inference rules
   - Generate new sentences from old 

### Normal Form
- CNF have the or inside with and
- DNF the exact opposite and in or out
- Horn Form less than 2 positive literal per sentences if or and and.

### Inference rules for propostional logic
Resolution:
$$ \frac{(\alpha \lor \beta), (\neg \beta \lor \gamma)}{(\alpha \lor \gamma)}$$

Modus ponenes (horn):
$$ \alpha_1, \alpha_2 ... \alpha_n, \frac{(\alpha_1 \land \alpha_2 \land ... \alpha_n \implies \beta)}{\beta}$$

## Forward chaining
When a new sentence p is added to the KB look at all sentences that share a literal with p, perform the resolution and add the new sentences to the KB (ook at what can be deduced).

It is data driven (we keep adding things to the KB): we already have knowledge and we add new knowledge to our data by cross comparing it with what we already have. It is eager as new facts are inferred we figure something out and we add it to the knowledge based

Mostly used when the focus is to find a model of the world by extending the KB and improving our understanding of the world.

## Backward chainging 
when q is asked of the KB we do the following:
1. If q is in the KB already, return true
2. Else use the resolution for q with other sentences in the KB and continue from the result.

Goal driven as reasoning is centered arround the query being asked. It is lazy since new facts are inferred as needed.

Used when we only want to perform targeted calculations so more efficient, used in proofs by contradictions.

# 06/02/19
### Complexity of inference
The time complexity of verifying the validity of a sentence of n literals is 2^n.

Horn closes however can be done in exactly one fact. We can state all new facts implied by the KB in n passes. Thus experts systems usually uses horn closes.

# First order logic
Includes the exists and for all. Predicates are used to describe objects, properties, relationships. The quantifiers are the forall and exists 

Functions are used to give you an object related to other object and they can be related such as cell A is to the right of cell B

$$ \forall x On(x,Table) \implies Fruit(x)$$  
This measn that any thing that is one the table must be a fruit.
this is a functions

Predicates: truth value at the end IsPit(x,y)  
Functions: SonOf(x), PlusOne(x) map domain element to domain elements. these returns other elements.

A term can be a constant, variable or function  
Atomic santence: take terms and join them together with a predicate.  
Complex sentences: combine atomic sentences with connectives.

# 11/02/19
## Unification
$$ p\sigma = q\sigma$$
Where sigma unifies both side p = x + 2 and q = 2 + 3 then sigma is x / 3 when x is replaced by 3.

### GMP generalized modus ponens 
use the special imply form of horn. 

## Completness of FOL
GMP is complete for KB of universally quanfitify hordn

FOL is only semi decidable can foind a proof when KB entails alpha but not always when KB does not entail alpha.

## Resolution
Two clauses can be resolved if they contain complementary literals.

This is a sound and complete method

## Skolemization (eliminate existential quantifier)
$$ \exists x Rich(x) \rightarrow Rich(G1)$$
Where G1 is the skolem constant for \forall we use skolem functions such as G(x).

# Planning
A collection of actions for performing some tasks.

The most commun thing is to use a subset of FOL to describe the problem.
- What actions can be done?
- When are we allowed to do these actions.
- What is the initial state
- What is the goal state

## Search of planning
| Search | Planning |
| ---- | ----- |
|States are atomic | States and goal are logical sentence |
| Acations are atomic | Actions are described in logic by precondition and outcomes|

### STRIP planning language
Everything not stated is false, only objects in the world are the ones defined.

Precondtions are positive litterals and post condtion represented as add and delete least of.

#### Pro
Inference can be done efficiently, all operators can be viewed as simple delitions and additions of propositions to the knowledge base.

#### Cons
Assumes that a small number of proposition will change for each action

Limited language

# 18/02/19

# Planning approaches

## State space planning

### Progressing planning
1. Determine all operators avail from the start state by examining preconditions
2. Ground the operators by replacing any var with constants
3. Choose an operto apply
4. Determine the knew knowledge base

### Regression planning
1. Start form the goal state
2. Pick action that satisfy some part of the goal state
3. Make a new goal containing the precondtion of thes actions as well as any unsolved goal proposition
4. Repeat until goal state is satisfied by the start state.

For both regression and progression matters, also in both states the planner work with a set of states instead of using individual state.

### STRIPS planning
It is sound since only legal plans are found. It is not complete since we cannot go backwards so not optimal either and expensive then.

## SatPlan
- Take a description of a planning problem and generate all literals at a time slice.
- Use SAT solver to get a plan random or advance SAT solver.

is NP-Hard + PSPACE.

### Heuristic search planning
Define heuristics based on the palanning problem itself to relax the problem.

#### Some heuristics
Ignore preconditions, assume an operator can always be applied

Ignore delete lists assume that when something is achieved it stays achieved.

## Real world application
The issue is that in the real world we have incomplete information or incorect information (something might be true but false to us or has a certain probability of being flase). There will also be some qualification problems, we can never list all preconditin and possible conditional outcomes of an actions.

### Solutions
#### Conditional planning
plan include observation actions and contingency plan (if this then that else this). It is expensive because it plans for unlikely cases

#### Monitoring/Replanning
Assume normal states outcomes, check progress during exection and replace if necessary.

# Uncertainty
We can either:
- ignore uncertainty as much as possible
- Build procedure that cannot have uncertainty
- Build a model that describe these uncertainties

Logical approach is not good sincd sensors of that world may not work or conclusions can be flawed. 

## Bayesian probability
Prior belief is the belief we had up to the arrival of knew evidence.

$$P(A | B) = \frac{P(A)*P(B | A)}{P(B)}$$

### Conditional indepent
P(x | y,z) = P(x | z) knowing y does not change the probability of x knowing z.

### Naive Bayesian
If we assume thay symptoms are independent for example:  
P(D, s1, s2, ...) = P(D) * P(D | s1) * P(D | s2)  
And so on.

Exercise:
2^{D + n} - 1 
1 + 2*n  

# 25/02/2019
## Special Law
Prop(Hypo | Exepects) equiv to Prob(Expect | Hypo)* P(Hypo)  
Posterior Likelyhood prior

## Baysian network
Represents the conditional independence relationships in a systematic way using a graphical model. Nodes are random variables and endges with arrow represent the node that affect the probability of others.

In this case P(A | B) works when looking at the parents of A which in this case happens to be B where B has a specific value.

The graph cannot have a directed cycle.        

# 28/02/19
## Leveraging structure
When a term only appears in certain P then isolates these P from the summation and change these P into a function lets call that m_s(f) in the case where the term isolated is s but depends on f. 

Then we can keep repeating that for all variables we get a runing time of 2^kn instead of 2^n where k is the largest factor that we have I guess the biggest function that we have.

Algo (similar to dynamic programming):
1. Impose an ordering over the vriable with the query var last
2. Maintian a list of factos which depend on given variables
3. Sum over variables in the order in which they appear in the list
4. Memorize the result of intermediate computation.

### Notation
**Evidence variables**: are viraible with observed values. 

$$ \delta(x_j, x_i) = 1  \iff x_j = x_i$$
Else it is 0.

So the P of:
$$ P(s \mid F=1) = \sum_fP(s \mid f)\delta(f,1)$$

### Algo with the new notation
1. Pick a variable odering with Y (query var) at the end
2. Initialize the active factor list with cond prob dist (tables) in the Bayes net
3. Add to the active factor list the ecvidence potential delta function of evidence variables %
4. for i=1..n
   1. Take the next var Xi form the ordering
   2. Take tall the factos that have Xi as an arg off the active factor list multiply them then sum ovar ll values of Xi creating a new facto mxi
   3. Put mxi on the active factor list

We need to have at most O(n) to create an entry in a factor and m is the maximum number of values that a variables. Then a factor that depends on k variables will have O(m^k) entries so k could be as bad as n the ordering of variables matter.

# 11/03/19

## Data estimate
Making estimates when certain data combinations are quite rare even on large sample can be hard. One thing to compensate that is pretend that you have seen every potential outcome at least once so for coin toss we assume we have seen at least 1 head and at least 1 tail. This works per combination (if you know what I mean) in the fire case one for no alarm with fire and with tempering and 1 with alarm with no fire and no tempering.

## Maximum likelihood estimation
Taking the log of a function is monotoneous which means that taking the log of a function does not change where the maximum location is on the x axis just the value on the y axis.

## Laplace smoothing
The idea of having one for each combination of values is called laplace smoothing.


Review lagrange multiplier    

# 13/03/2019
## Getting Baysian value with missing data
Values may be **latent/hidden** variables such as viewers prefer certain movies depending there level of tiredness during the day which is not measured.

It is interesting to model the latent variable such as heart disease is a latent variable and smoking, exercise, diet as variables we record and estimate the latent variable.

Models with latent variables register however can be more precise and cleaner than other models.

### latent variable hestimation
When removing  variable from Baysian graph this forces us to have more edges as we need to connect nodes that are affected by the latent variables with node that affect the latent vriable which with the example given in class changes the number of parameters from 78 to 708 parameters which is 10 times more.

## Expectation Maximization (EM)
The parameters we chose in the begenning must not be assigned 0 or 1. P(T) and so on.  
1. E-Step For all the instances of missing data we will fantasize how the data should look based on the current parameter setting
2. M-Step we then maximize parameter setting based on these statistics.

There are two versions of this algorithm both have the same complexity.

### Hard EM
For each missing data point assing the value that is most likely

### Soft EM
For each missing dtat poit put a weight on each value equal to its probability and use the weights as counts. This means that we create different data set for each value which gives us
$$ L(\Theta\mid D*) = w_0P(D_0 \mid \Theta) + w_1P(D_1 \mid \Theta)$$

*I encorage looking at the slides here to make the idea clearer*

### Properties
- Guaranty to improve or stay the same at each iteration (soft version only mentioned by the prof)
- Guaranty to conver at a local optimum (soft version only I suppose)

## Harde problem
What if a value is always missing

### Ploting the data
We can plot the data and look at any clusters that we can see we could maybe assing a cluster to a specific value regarding the missing parameter like apple varaity based on weight and color

### Gaussian distribution
Here we no longer assume discrete values as we are considering continuus data according to the prof. 

Guassian is a probability function you know the one with the average at the center and the standard divation a good example is the normal distribution.

In our case the closer you are to the mean the closer you are to that category.

### K mean clustering
We want to classify sample data to a category and we have K categories
- there are 5 apple variety
- Each variety genreates apples according to a 2-D Guaussian
- If you know mean and variance of each class it is easy to estimate apples
- If you know the class of each apple it is easy to estimate mean and variance.

**What if we know neither?**
1. assign random guess to mean and variance of each classes ensuring they are different.
2. Assign a class to each apple


Now for the K technic
1. Guess K centers
2. Assign each data points to the closest center
3. Then reastimate the new center
4. Repeat

That K can also be soft by assigning a likely hood score for each data points and use it when calculating the mean and variance.

Time complexity: O(nkd) wher n = # data points, k = # centers, d = dimensionality of data

- Converges to local optimum
- Can use random restart to get better optimum


#### Choose initial center alternative
1. place the first u1 on a random data point
2. Place u2 on the point furtherst to u1
3. place u3 furthest from u1 and u2.

# 18/03/2019
# Supervised learning
## Terminology
- Inputs are called variables, features or attributes
- Predicitona are called ouput variables or target
- A data set consists of training examples or instances

Given a data set D = X cross Y where X is the set of all features column wise and training examples row wise and Y is the target row wise. 

## Linear hypothesis
Y is a lineaer function of x so:
$$ f_w(x) = w_0 +w_1x_1 +w_2x_2 + ... + w_mx_m $$

To select the w that minimizes the error we use the least square:
$$ Err(W) = \sum_{i=1:n}(y_i - W^Tx_i)^2 \\
 = (Y - XW)^T(Y-XW)
$$

# 20/03/19
# Time and uncertainty
Here we are going to make observable variables Xt and observable variables Et where t represents the time. We are also going to assume that the time is discrete and that we experience the same model of data no matter the time.

## Transition model
Changes in the state of variables over time are determined by that model.
$$ P(X_t \mid X_{0:t-1})$$

## Senseor model
Same than above but with observable variables.
$$ P(E_t \mid X_{0:t-1},E_{0:t-1} )$$

## Markov process
Markov assumption: Xt depends on a boundes subest of the previous states. Such that the probability of word w being next only depends on the previous number w-1 so only on the previous state.

Sationary process assumption: transition and sensor models are fixed for all time steps. P(Xt | Xt-1) as for P(Xt+1 | Xt).

These assumptions are often false in the real world but ae usefull assumptions to makes learning possible.

## Dynamic baysian network
Have a regular baysian network that we replicate over time that are connected to each other (adding links between random variables at each time steps between each baysian replicas)

## Inference taks on temporal models
1. Filtering (infer a topic probabilty after ready article)
2. Prediction (predict the price of some value based on what has been seen so far)
3. Smoothing (looking at the genom produce plausible sequence alighnment)
4. Most likely explaination (infer moste likely word based on observed phonemes in speech recognition)

## Simples case hidden Markov Model
There is only one state random variables and one sensor variable per time step: simples DBN.

### Compute data likelihood
P(E | ø) = sumx(P(E,X | ø))

### Forward algorithm
We are going to use a trellis table:
Row represents all the state of each hidden variables at time t the columns represent the probabilty of each seen variables for each t.
