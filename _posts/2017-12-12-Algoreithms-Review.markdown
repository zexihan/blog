---
layout:     post
title:      "Algorithms Review"
subtitle:   "Algorithms"
date:       2017-12-12
author:     "Zexi"
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
​    - Algorithms
---

# Algorithms Review



## (0) NP-Completeness

### Reductions

$Y\leq_PX$: Y is polynomial-time reducible to $X$, or X is at least as hard as $Y$

Suppose $Y\leq_PX$. If $X$ can be solved in polynomial time, then $Y$ can be solved in polynomial time.

Suppose $Y\leq_PX$. If $Y$ cannot be solved in polynomial time, then $X$ cannot be solved in polynomial time.

__Independent Set__

In a graph $G=(V,E)$, we say a set of nodes $S\subseteq V$ is independent if no two nodes in $S$ are joined by an edge. 

_Given a graph G and a number k, does G contain an independent set of size at least k?_

__Vertex Cover__

Given a graph $G=(V,E)$, we say that a set of nodes $S\subseteq V$ is a vertex cover if every edge $e\in E$ has at least one end in $S$.

_Given a graph G and a numebr k, does G contain a vertex cover of size at most k?_

$\text{Independent Set} \leq_P \text{Vertex Cover}$ and $\text{Vertex Cover} \leq_P \text{Independent Set}$

__Set Cover__

_Given a set U of n elements, a collection $S_1,...,S_m$ of subsets of U, and a number k, does there exist a collection of at most k of these sets whose union is equal to all of U?_

$\text{Vertex Cover}\leq_P \text{Set Cover}$

__Set Packing__

_Given a set U of n elements, a collection $S_1,...,S_m$ of subsets of U, and a number k, does there exist a collection of at least k of these sets with the property that no two of them intersect?_

$\text{Independent Set}\leq_P \text{Set Packing}$

### 3-SAT

$(x_1\vee x_2 \vee \neg x_3)\wedge(x_1\vee \neg x_2 \vee \neg x_4)\wedge(\neg x_1\vee \neg x_3 \vee \neg x_5)$

_Given a set of clauses $C_1,...C_k$, each of length 3, over a set of variables $X={x_1,...x_n}$, does there exist a satisfying truth assignment?_

$\text{3-SAT}\leq_P \text{Independent Set}$

__Transivity of Reductions__

$If\ Z\leq_P Y,\ and\ Y\leq_P X,\ then\ Z\leq_PX$

### Certification and NP

$P\subseteq RP\subseteq NP$

$NP$: class of problems whose solutions/proofs/certifier can be verified in poly-time.

$\text{3-SAT}\in NP$

certifier = satisfying assignment

size of certifier = n

verification time = m

$\Pi \in NP$ iff there exists poly-size certifier for every Yes instance.

###  NP-Complete

If $Y$ is an NP-Complete problem, and $X$ is a problem in NP with the property that $Y\subseteq_P X$, then $X$ is NP-complete.

3-SAT is NP-complete

$\text{3-SAT}\leq_P \text{Independent Set}\leq_P \text{Vertex Cover}\leq_P \text{Set Cover}$

All of the following problems are NP-complete: Independent Set, Set Packing, Vertex Cover, and Set Cover.

__General Strategy for Proving New Problems NP-Complete__

1. Prove that $X\in NP$.

2. Choose a problem Y that is known to be NP-complete.

3. Prove that $Y\leq_P X$. Consider an arbitrary instance $s_Y$ of problem $Y$, and show how to construct, in poly-time, an instance $s_X$ of problem $X$ that satisfies the following properties:

   (a) If $s_Y$ is a "yes" instance of $Y$, then $s_X$ is a "yes" instance of $X$.

   (b) If $s_X$ is a "yes" instance of $X$, then $s_Y$ is a "yes" instance of $Y$.



## (1) Network Flows

### Minimum s-t Cut

Given undirected graph $G=(V,E)$ and vertices s and t, a minimum cut $S$ separates s and t, that is, a partition of the nodes of $G$ into $S$ and $V$ \ $S$ with $s\in S$ and $t \in V\ \backslash\ S$ that minimizes the number of edges going across the partition. 

For directed graph with source and destination nodes $s$ and $t$, the size/cost of an $s-t$ cut is $||S,T||=\sum_{x\in S,y\in T} c(x,y)$

### Maximum Flow

__Capacity Constraint__

$\forall(u,v)\in E,\ 0\leq f(u,v)\leq c(u,v) $

__Flow Conservation Constraint__

$\forall v\in V\backslash\{s,t\},\ \sum_{x\in N_{in}(v)} f(x,v)=\sum_{y\in N_{out}(v)} f(v,y)$

__The Max-Flow Min-Cut Theorem__

For any graph G, source s and destination t, the value of the max flow is equal to the cost of the min cut.

__The Ford-Fulkerson Max-Flow Algorithm__

```{c}
maxflow(G,s,t)
    f <- all zeroes flow;
    G_f <- G;
    while t is reachable from s in G_f (check using DFS) do
        P <- path in G_f from s to t;
        F <- min capacity on P;
        f <- f';
        Update G_f to correspond to new flow;
    return f;
```

Time complexity: $O(|f|m)$ where $|f|$ is the value of the max flow and $m$ is the number of edges.

__Bipartite matching__

The Ford-Fulkerson Algorithm can be used to find a maximum matching in a bipartite graph in $O(mn)$ time.



## (2) Dynamic Programming

### Steps

- Define Subproblems
- Write a Recurrence
- Prove the Recurrence is Correct
- Prove the Algorithm Evaluates the Recurrence
- Prove the Algorithm is Correct

### Examples

- __Longest increasing subsequences:__

$DP[i]=\max\{1,\max_{j<i,A[j]<A[i]}\{1+DP[j]\}\}$

- __Longest common subsequences:__

If xi == yj:

​    $DP[i,j]=1+DP[i-1,j-1]$

else:

​    $DP[i,j]=\max\{DP[i-1,j],DP[i,j-1]\}$







## (3) Greedy Algorithms, including MST and Shortest Paths

### Greedy Algorithms

#### Greedy Stays Ahead Arguments

- Define Your Solution
- Define Your Measure
- Prove Greedy Stays Ahead
- Prove Optimality

#### Exchange Arguments

- Define your solution
- Compare solutions
- Exchange Pieces
- Iterate

#### Inductive Hypothesis

- Inductive Hypothesis
- Base case
- Inductive step
- Conclusion

### Minimum Spanning Tree

#### Prim's Algorithm

The set $A$ maintained by Prim's algorithm is a single tree.

Starts with an arbitrary root $r$

Use priority queue $Q$ maintaining distances of vertices not in the tree so far

$key(v)$ is the minimum weight of edge connecting $v$ to some vertex in the tree

$p(v)$ is the parent of $v$ in the tree

```{c}
Prim(G)
  key(v) <- inf for all v in V
  key(r) <- 0
  Q <- (key(v),v)
  p(v) <- NIL for all v in V
  A <- 0
  while Q is not empty do
    u <- ExtractMin(Q)
    if u != r then
        A = A U {(p(u),u)}
    for each neighbor v of u do
      if v in Q and w(u,v) < key(v) then
          key(v) = w(u,v)
          DecreaseKey(key(v),v)
          p(v)=u
  return A
```

Running time using heap:

ExtractMin: $O(\log n)$

DecreaseKey: $O(1)$

Total: $O(n\log n+m)$

#### Kruskal's Algorithm

Use union-find

```{c}
Kruskal(G)
  A <- 0
  E' <- sort edges by weight in non-decreasing order
  for each v in V do
    makeset(v)
  for each (u,v) in E' do
    if find(u) != find(v) then
        A <- A U {(u,v)}
        union(u,v)
  return A
```

Running time:

$O(m\log n)$ sorting the edges

$O(nT(makeset)+mT(find)+nT(union))$



### Shortest Paths

#### Dijkstra

```{c}
Dijkstra(G=(V,E),s)
  d[t] <- inf for all t in V
  d[s] <- 0
  F <- {v|for all v in V}
  D <- 0
  while F != 0 do
    x <- element in F with minimum estimate
    for (x,y) in E do
      d[y] <- min{d[y], d[x]+w(x,y)}
      if d[y] changes, then pi(y) <- x
    F <- F \ {x}
    D <- D U {x}
```

Running time using heap: $O(m+n\log n)$

It doesn't work with negative edge weights.

#### Bellman-Ford Algorithm

DP: The shortest paths of length at most $k$ can be computed by leveraging the shortest paths at most $k-1$.

$d[k,v] = \min\{d[k,v],d[k-1,u]+w(u,v)\}$ for $u$ in $V$

```{c}
Bellman-Ford Algorithm(G,s)
  d[0,v] = inf for all v in V
  d[0,s] = 0
  d[k,v] = None for all v in V and all k > 0
  for k = 1,...,n-1 do
    d[k,v] <- d[k-1,v]
    for (u,v) in E do
      d[k,v] <- min{d[k,v],d[k-1,u]+w(u,v)}
  return d[n,v] for all v in V
```

Running time: $O(mn)$



## (4) Graph Traversals

### DFS

```{c}
Explore(v)
	visit(v) = true
	//previsit(v)
	for each edge {v, u} ∈ E:
		if visit(u) == False do:
			explore(u)
	//postvisit(v)
              
DFS(G)
	for all v in G do 
		visit(v) = False
	for all v in G do:
		explore(v)

```

when we change the previsit and postvisit part we can get new algorithm.

Running time: $O(m+n)$

Stack

### BFS

Running time: $O(m+n)$

Queue

### Strongly Connected Components

Kosaraju’s algorithm

1. Create an empty stack ‘S’ and do DFS traversal of a graph. In DFS traversal, after calling recursive DFS for adjacent vertices of a vertex, push the vertex to stack. In the above graph, if we start DFS from vertex 0, we get vertices in stack as 1, 2, 4, 3, 0.
2. Reverse directions of all arcs to obtain the transpose graph.
3.  One by one pop a vertex from S while S is not empty. Let the popped vertex be ‘v’. Take v as source and do DFS (call DFSUtil(v)). The DFS starting from v prints strongly connected component of v. In the above example, we process vertices in order 0, 3, 4, 2, 1 (One by one popped from stack).

Running time: $O(m+n)$

### Direct Acyclic Graph

The acyclic meta-graph of sccs

Topological Ordering

We can modify DFS to find Topological Sorting of a graph. In DFS, we start from a vertex, we first print it and then recursively call DFS for its adjacent vertices. In topological sorting, we use a temporary stack. We don’t print the vertex immediately, we first recursively call topological sorting for all its adjacent vertices, then push it to a stack. Finally, print contents of stack. Note that a vertex is pushed to stack only when all of its adjacent vertices (and their adjacent vertices and so on) are already in stack.



## (5) Divide-and-Conquer, Sorting, Selection, Binary Search, including recurrences

__Merge Sort__

```{python}
def merge(self, left, right):
    i, j = 0, 0
    result = []
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result += left[i:]
    result += right[j:]
    return result

def merge_sort(self, lists):
    if len(lists) <= 1:
        return lists
    num = len(lists) / 2
    left = self.merge_sort(lists[:num])
    right = self.merge_sort(lists[num:])
    return self.merge(left, right)
```



__Selection__

Input: Sequence S of n numbers and integer K which 1$\leq$K$\leq$n
Output: The k-th smallest element in S.
Select without sorting
Running time: $O(n)$

```{c}
select_p(list):
	divide list in to small part of size 5 A1... Ak (k = n/5)
	calculate the mid number of each sub-list store the every mid-number in list B
	return select(b, len(b)/2)

select(A[1...n], k):
	p = select_p(A[1...n])
	for i in A[1..n] do:
		if i >= p do:
			inset i in to bigger_than_p list
		else if i == p do
			inset i in to equal_to_p list
		else do:
			inset i in to less_than_p list
	if k < len(less_than_p) do:
		return(select(less_than_p, k))
	else if k > len(less_than_p) + len(equal_to_p) do:
		return(select(bigger_than_p, k-|less_than_p| - |equal_to_q|))
	else
		return p
```



__Binary Search__

```{python}
# Returns index of x in arr if present, else -1
def binarySearch (arr, l, r, x):
    # Check base case
    if r >= l:
        mid = l + (r - l)/2
        # If element is present at the middle itself
        if arr[mid] == x:
            return mid    
        # If element is smaller than mid, then it can only be present in left subarray
        elif arr[mid] > x:
            return binarySearch(arr, l, mid-1, x)
        # Else the element can only be present in right subarray
        else:
            return binarySearch(arr, mid+1, r, x)
    else:
        # Element is not present in the array
        return -1
```



