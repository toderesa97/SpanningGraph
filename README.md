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


- Using BFS: all nodes belonging to a certain depth are first scanned before diving into the next level

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

(This awesome depiction was made using https://sketch.io/sketchpad/)

- Using DFS: a node is scanned until the lowest leaves (a node with no children)

```
OL = {Z}
CL = {}

Pop Z, is Z the final state (ie, S)? No. Is Z in CL? No
OL = {O,A}
CL = {Z}

Pop O, is O S? No. Is O in CL? No.
OL = {O,S,A}
CL = {Z, O}

Pop O, is O S? No. Is O in CL? Yes.
Ol = {S,A}
Pop S, is S S? Yes.

Output: {S, O, Z}
```

![dfs](https://user-images.githubusercontent.com/19231158/36641248-c920ed16-1a2c-11e8-8f06-833ce29d0a09.PNG)

- Using BSS: in every insertion nodes within OL are sorted based upon path_cost attribute (this sttr. is accumulative)

**Chosen points:** from Zerind to Sibiu (Z, S)

Since in every expansion all nodes are sorted, the insertion does not matter at all.
```
OL = {Z}
CL = {}
    
Is OL sorted? Pop Z, is Z S? No. Is Z in CL? No.
OL = {O,A}
CL = {Z}

Is OL sorted? yes.
    Z-O = 71
    Z-A = 75

Pop O, is O S? No. Is O in CL? No.
OL = {A,Z,S}
CL = {Z,O}

Is OL sorted? yes.
    Z-A=75
    Z-O-Z = 71+71 = 142
    Z-O-S = 71+151 = 222

Pop A, is A S? No. Is A in CL? No.
OL = {Z,S,Z,S}
CL = {Z,O,A}

Is OL sorted? No.
    Z-O-Z = 142
    Z-O-S = 222
    Z-A-Z = 75+75 = 150
    Z-A-S = 75+140 = 215  
    
sort, OL = {Z, Z, S, S}
Pop Z, Is Z S? No. Is Z in CL? Yes.
OL = {Z,S,S}
Pop Z, Is Z S? No. Is Z in CL? Yes.
OL = {S,S}
Pop S, Is S S? Yes.

Output: Z-A-S (path cost = 75+140 = 215) 
```

As you might have noticed, **they are greedy algorithms**, hence not suitable for large maps.