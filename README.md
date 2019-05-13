# Algortihms
A repository to track the algorithms I learn.

## Graph

### Section Structure

This section is divided into three major sections. The first sections defines graphs and their properties. The second section introduces the types of problems surrounding graphs. 

The third section explores the algorithms that solve the different graph problems. The exploration includes
* defining how to algorithm works
* the algorithms properties (limitations, sub problems they solve)

### Graphs: What are they?

A graph can be:
* **Cyclic** (has cycles) or **Acyclic** (doesn't have cycles)
* **Directed** (edges are unidirectional) or **Undirected** (edges are bidirectional)
* **Weighted** (edges have varying weights) or **Unweighted** (all edges have the same weight).
* **Sparse** (the number of edges, E, is much less than the numbers of vertices squared, E < V^2) or **Dense** (the number of edges, E, is close to the numbers of vertices squared, E ~ V^2)
* **Graph Diameter** is a property of a **Configuration Graph**. It records the length from the two farthest states, represented as vertices.

### Graphs Problems

* single-source shortest path
* any-source shortest path
* minimum spanning tree
* Detecting cycles
* topological sort

### Traversing Graphs (the Algorithms)

There are two ways to traverse a graph, **Breadth-First Search (BFS)** or **Depth-First Search (DFS)**. 

**All graph traversal algorithms fall into one of the two methods.**

Applications of graph search include
* web crawling
* social networking
* network broadcast
* garbage collection
* model checking
* checking mathematical models and conjectures
* solving puzzles and games (config graph)

#### Breadth-First Search (BFS)

```
BFS(G,s):
  for each v ∈ V[G] -s          // Step 1: assign default values for distance, parent and
    d[v] <- ∞                   //         traversal state for all v ∈ v[G], except s
    π[v] <- NIL
    color[v] <- WHITE
 
  d[s] <- 0;                    // Step 2: Assign the initial values for s
  π[s] <- NIL
  color[s] <- GRAY
 
  Q.enqueue(s)                  // Step 3: As vertices are discovered, iterate
  while Q ≠ Ø                   //         through their adjacency lists until
    u <- Q.dequeue()            //         all vertices are explored.
    for each v ∈ AdjList(u)
      if color[v]=WHITE
        d[v] <- d[u]+1
        π[v] <- u
        color[v] <- GRAY
        Q.enqueue(v)
    color[u] <- BLACK
``` 
Time complexity: O(V+E)

##### How BFS works:
BFS takes a breadth-first approach to traversing a tree. 
> BFS discovers all vertices at distance `k` from starting vertex `s` before discovering vertices at distance `k+1` from `s` and so on. 

During execution, BFS performs three things,
1. records the distance from `s` for each vertex `v ∈ V[G]`
2. records the parent of each vertex `v` in the path `s..v` (conincidentally, forming a BFS tree)
3. tracks the traversl state of each vertex during execution. For simplicity, the algorithm usually uses colors: `WHITE`,`GRAY` and `BLACK`
  * `WHITE` - vertex is undiscovered
  * `GRAY` - vertex has been discovered, but not fully explored
  * `BLACK` - vertex has been explored
  
With regards to BFS, a vertex is "explored" when each member of the vertex's adjacency list has been examined (i.e. discovered). 

##### Properties of BFS:
Given a starting vertex `s` in the set of vertices in a graph `V[G]`, BFS will find the shortest path `δ(s,v)` as the minimum number of edges in the path `(s..v)`
> in other words, BFS can be used to find the single-source shortest path _if-and-only-if_ the graph is unweighted (hence, the only concern is the number of edges in the path)

BFS composes a single tree of the graph (only 1 root).  



#### Depth-First Search (DFS)
```
DFS(G):
  for each v ∈ V[G]            // Step 1. Initialize the parent,    
    d[v] <- ∞                  // discovery time, finish time and 
    f[v] <- ∞                  // traversal status of each vertex 
    π[v] <- NIL
    color[v] <- WHITE
 
  time <- 0                    // Step 2. Start the "clock"
  
  for each u ∈ V[G]            // Step 3. Visit each vertex
    DFS-VISIT(u) 
---

DFS-VISIT(u):
  color[u]=GRAY
  time <- time + 1
  d[u] <- time

  for each v ∈ AdjList(u)
    if color[v]=WHITE
      π[v] <- u
      DFS-VISIT(v)
  color[u] <- BLACK
  time <- time + 1
  f[u] <- time
---
```

##### How BFS works:
DFS takes a depth-first approach to traversing a tree.
> For each vertex `v` discovered, search of child of `v`, and once the end is reached, "backtrack" to the parent of `v` and search a sibling of `v`

During execution, DFS performs three things,
1. records the time a vertex `v` is discovered (discovery time) and the time the vertex finished being explored (finish time) for each vertex `v ∈ V[G]`.

   ..._(compare to BFS, which computes distance from some starting vertex `s` to `v`)_
2. records the parent of each vertex `v` in the path `s..v` (conincidentally, forming the DFS forest)
3. tracks the traversl state of each vertex during execution. For simplicity, the algorithm usually uses colors: `WHITE`,`GRAY` and `BLACK`
  * `WHITE` - vertex is undiscovered
  * `GRAY` - vertex has been discovered, but not fully explored
  * `BLACK` - vertex has been explored

##### Properties of DFS:

***Property:*** DFS provides information about the structure of the graph, which is elaborated in the subsequent properties

***Property:*** The discovery and finish times are parenthetical operations. That means for any vertices u and v, if v was discovered after u (d[u]<d[v]) then v will finish before u (f[u]>f[v]) or simply put `{d[u]...{d[v]...{...}...f[v]}...f[u]}`. This plays a role in classifying edges.

***Property:*** DFS computes forests 
DFS computes a forest of the graph. A forest is a set of disjoint trees. Compare to BFS, which can only form a tree rooted at the starting vertex. 

***Property:*** DFS can be modified to **classify edges**

***Property:*** DFS can be modified to **detect cycles**

***Property:*** DFS can be modified to perform a **topological sort** of a graph

###### Edge Classification

There are 4 types of edges in a graph:
1. Leaf edge: An edge that adds a new vertex to the tree.

   ..._Vertex v of the edge (u,v) was previously undiscovered._
2. Forward edge: An edge (u,v) that leads to an descendant in the tree

   ..._the depth of vertex v > depth of vertex u._  
3. Back edge: An edge (u,v) that leads to an ancestor in the tree 

   ..._the depth of vertex v < depth of vertex u._
4. Cross edge: Any edge not a leaf, forward or back edge.

***How to modify DFS to classify edges:*** Use the traversal status (WHITE,GRAY,BLACK) and the discovery and finish times to determine an edges type. Using `d[n]` and `f[n]` to represent the discovery time and finish time of some vertex n, the edge (u,v)
- is a leaf edge if it causes v to go from WHITE -> GRAY
- is a back edge if d[v]<d[u]
- is forward edge if f[u]>f[v]
- is cross edge if 

```
DFS(G):
  for each v ∈ V[G]                
    d[v] <- ∞                   
    f[v] <- ∞                   
    π[v] <- NIL
    color[v] <- WHITE
 
  time <- 0                    
  
  for each u ∈ V[G]            
    DFS-VISIT(u) 
---

DFS-VISIT(u):
  color[u]=GRAY
  time <- time + 1
  d[u] <- time
  
  for each v ∈ AdjList(u)
    if color[v]=WHITE
      π[v] <- u
      type(u,v) = leaf
      DFS-VISIT(v)
    else                    // we've already explored v
      if(
  color[u] <- BLACK
  time <- time + 1
  f[u] <- time
---
```

**Important Note: In an Undirected Graph, forward edges and cross edges are not possible.**

###### Cycle Detection
###### Topological Sort
