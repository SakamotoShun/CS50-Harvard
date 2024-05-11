---
id: CS50
aliases: []
tags:
  - Lectures
---
# Search

Maze / Number finding / Driving Directions

## Types of Search
- Uniformed Search
  : Search Strategy that uses no problem specific knowledge
    - [Depth First Search](#stack-frontier)  :: [[#Stack frontier]]
    - [Breadth First Search](#queue-frontier) :: [[#Queue Frontier]]
- Informed Search
  : Search strategy that uses problem specific knowledge to find solutions more efficiently.
    - [Greedy Best-First Search](#greedy-best-first-search) :: [[#Greedy best-first search]]
    - [A* Search](#a(star)-search) :: [[#A(Star) Search]]
___
- Agents
  : entity that perceives its environment and acts upon that environment.
- State
  : A configuration of the agent in its environment.
- Initial State
  : The state where the agents begins.
- Actions
  : choices made in a state.
    - ACTIONS(*s*) returns the set of actions that can be executed in state(*s*).
- Transition Model
  : RESULTS(*s,a*) returns the state resulting
from performing ACTIONS(*a*) in a STATE(*s*).
- State Space
  : The state of all states reachable from the initial state by any
sequence of actions.
- Goal Test
  : Way to determine whether a given state is a good state.
- Path Cost
  : Numerical cost associated with a given path to get to goal state.

## Approaching a search problems

- Initial State
- Actions
- Transition Models
- Goal Test
- Path Cost functions

- Node: A data structure that keeps track of;
  - State
  - Parent (Which node it came from)
  - An action (What action did it take to arrive in this mode)
  - A path cost.

- Frontier: The possible futures of the node.

### Approach

1. Start with a frontier that contains the initial state
2. Repeat:
    1. If frontier is empty --> No solution.
    2. Remove a node from the frontier.
    3. If node contains goal state, return solution.
    4. Expand nodes, adding resulting node to the frontier if they have not
       been explored

___

- Stack: Last in first out data type
  - Explore the latest added node
  - Depth First Search: Go as deep as possible, then backing out if no results.
    - Might not find the optimal path.
- Queue: First in first out data type
  - Explore the first added node)
  - Breadth first search.
    - Will find the optimal path, but may cost more memory.

___

### Stack frontier

```python
class Node():
  def __init__(self,state,parent,action):
    self.state = state
    self.parent = parent
    self.action = action
class Stackfrontier():
  def __init__(self):
    self.frontier = []

  def add(self,node):
    self.frontier.append(node)
    #Appends adds the last item viewed

  def contains_state(self,state):
    return any(node.state == state for node in self.frontier)
  
  def empty(self):
      return len(self.frontier) == 0

  def remove(self):
    if self.emtpy():
      raise Exception("emtpy frontier")
    else:
      # [-1] gives the last item in the list
      node = self.frontier[-1]
      self.frontier = self.frontier[:-1] #removes the last item on the list
      return node
```

___

### Queue Frontier

```python
class QueueFrontier(StackFrontier):
  
  def remove(self):
    if self.empty():
      raise Exception("empty frontier")
    else:
        node = self.frontier[0] #gives ths first item in the list
        self.frontier = self.frontier[1:]
        return node
```

___

### Solve function

```python
# keep track of number of states explored
self.num_explored = 0

# Initialize frontier to just the starting position
start = Node(state=self.start, parent=None, action=None)
frontier = StackFrontier() #DFS
frontier.add(start)

# Initialize and empty explored set
self.explored = set()

# Keep looping until solution found
while True:

  #If nothing left in frontier, then no path
  if frontier.empty():
      raise Exception("no solution")

  # Choose a node from the frontier
  node = frontier.remove()
  self.num_explored +=1

  #If node is the goal, then we have solution
  if node.state == self.goal:
    actions = []
    cells = []

    #Follow parent nodes to find solution
    while node.parent is not None:
      actions.append(node.action)
      cells.append(node.state)
      node = node,parent
    actions.reverse()
    cells.reverse()
    self.solution = (actions, cells)
    return
  
  # Mark node as explored
  self.explored.add(node.state)

  # Add neighbors to frontier
  for action, state in self.neighbors(node.state):
    if not frontier.contains_state(state) and state not in self.explored:
      child = Node(state=state, parent=node, action=action)
      frontier.add(child)
```
___
## Informed Search


### Greedy best-first search
Search Algorithm that expands the  node that is closest to the goal as estimated by a heuristic function. h(n)
- Manhattan distance
  : The distance between two points measured along axis at right angles. In a plane with p<sub>1</sub> at (x<sub>1</sub>,y<sub>1</sub>) and p<sub>2</sub> at (x<sub>2</sub>,y<sub>2</sub>), it is |x<sub>1</sub>-x<sub>2</sub>| + |y<sub>1</sub>-y<sub>2</sub>|
> When exploring GBFS, algorithm will choose which node to select at a fork based on a heuristic function that is lower.
   (Needs a good heuristic)

___

### A(Star) Search

Search Algorithm that expands node with the lowest value of g(n) +h(n)
- g(*n*): cost to reach node
- h(*n*): estimated cost to reach goal  (heuristic)
> g(n) + h(n) is the estimated travel cost from the initial state to goal state.
 The program will rather take fewer steps to be at a certain distance than take many steps to be about the same distance away from the goal.
>> A* search is only optimal if;
    - h(n) never over estimates the true cost **AND**
    - h(n) is consistant: For every node *n* and successor n` with steps cost *c*, h(*n*) <= h(*n*) +c
  > h(n) needs to be either spot on or underestimate.
___
### Adverserial Search

Search Algorithm trying to win a game whereby there is an "opponent" trying to make it not win
  - *e.g.* Chess, Tic-Tac-Toe
  - Minimax
    : Assigning certain endstate with a number and each party tries to achieve the max/min score
