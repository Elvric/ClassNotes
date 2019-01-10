# COMP424
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