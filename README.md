# SpanningGraph

## Caveat: Python interpreter version 2.7.14

This project is aimed at learning the different procedures when looking for a target state. In this project we have
covered three different search methods:

- Breadth-first search (BFS)
- Depth-first search (DFS)
- Breadth-sorted search (BSS) [Name invented by me]

This methods builds a tree by examining the nodes of the graph. The process is referred as spanning tree.
(Refer to https://en.wikipedia.org/wiki/Spanning_tree for further details)

The last search method is key to retrieve the shortest route. It 'keeps track' of every path cost
and sort them based upon this criteria, thus determining efficient routes.

Such searches have been implemented using two lists:

- Open List (OL): stores nodes that are pending to be analysed. Depending on the search
    method a new set of nodes are inserted by its front or back. There is a subtle modification regarding BSS.
- Close List (CL): a list comprised by nodes already analysed. If a node that is being analysed is contained
     within this list, this node is discarded. The node is ditched because a loop has been discovered.

### What is the code expected to do?

The code must retrieve the shortest route given two geographic points, based upon this map.

![capture](https://user-images.githubusercontent.com/19231158/36631218-88b3f5ba-1974-11e8-9bad-118bd9f523aa.PNG)

### Example

This section shows what the output should be chosen two geographical points and a search method:

**From Zerind to Sibiu** (See the map)

First letter of each city is used to represent the city:

Zerind = Z; 
Sibui = S; 
Rimnicu Vilcea = R


- Using BFS

```
OL= {Z}
CL = {}

Pop Z, is Z the final state (ie, S)? No. Is Z in CL? No

OL = {A,O}
CL = {Z}

Pop A, is A S? No. Is A in CL? No
OL = {O, Z, S}
CL = {Z, A}

Pop O, is O S? No. Is O in CL? No
OL = {Z, S, Z, S}
CL = {Z, A, O} 

Pop Z, is Z S? No. Is Z in CL? Yes
OL = {S, Z, S}
Pop S, is S S? Yes. 

Output: {S, A, Z}
```

Every node contains a reference to its parent.

A visual scheme is:

![bfs](https://user-images.githubusercontent.com/19231158/36636364-b2cef678-19c5-11e8-8481-b227501dfdc4.PNG)
(This awesome depiction was made by https://sketch.io/sketchpad/)
