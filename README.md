<!--- Adding links to various images used in this document --->
[Data]: https://github.com/safdark/AI-planning/blob/master/Project3_Data.png

![Data] (https://github.com/safdark/AI-planning/blob/master/Project3_Data.png)

# AI - Planning - Air Cargo Transport

This document gives a high level overview of the search algorithms being evaluated, and a description of the problems they are being evaluated against.

## Overview

### Common Terms

*Runtime Metrics*: References to the word 'Performance Metrics' will be intended to refer to an algorithm's performance across 4 metrics - runtime, node expansions, goal tests and new node creations. This avoids redundancy

*Frontier*: This is a collection of the nodes whose neighbors have not yet been explored. In other words, these nodes have been reached, but have not been explored further. For the purpose of our discussion, the 'frontier' also encapsulates the underlying *representation* of this collection -- i.e, either a list, stack, queue, priority-queue etc. This is important to consider here because the choice of this data structure precisely determines the type of search algorithm ultimately being implemented (eg. Breadth-First-Search, Depth-First-Search, Uniform-Cost-Search etc), because this data structure dictates the order in which elements are selected at each subsequent step of the search algorithm.

### Problem Structures
*Tree-Search*: The structure of the search space in this case is a tree, with no cross-edges or back-edges (as is possible with a graph search). The implication of this on the algorithm is that there is no need to maintain a list of explored/visited nodes. Each neighbor of a node that is being explored/visited is guaranteed not to have been visited before.

*Graph-Search*: The structure of the search space in this case is a graph, with forward-edges, cross-edges and back-edges. The implication of this on the saerch algorithm is that it needs to maintain a list of explored/visited edges, and each time an edge's neighbors are explored, those neighbors that have already been explored are not added to the frontier. The Graph-Search algorithm reduces to a Tree-Search algorithm if the graph being searched on fulfills the structural requirements of being a tree data structure (i,e, no back- or cross- edges).

### Algorithm Flavor

*Greedy Search*:

*Recursive Search*:

### Search Algorithms Used

*Breadth-First-Search*: A graph-based search algorithm wherein the next node visited from the frontier is chosen based on the MIN # of hops from the start. Frontier = regular queue (FIFO).

*Breadth-Tree-Search*: A graph-based search algorithm wherein the next node visited from the frontier is chosen based on the MIN # of hops from the start. Frontier = regular queue (FIFO).

*Depth-First-Graph-Search*: A graph-based search algorithm wherein the next node visited from the frontier is chosen based on the MAX # of hops from the start. Frontier = regular stack (FILO).

*Depth-Limited-Search*: A search algorithm wherein the next node visited from the frontier is chosen based on 

*Uniform Cost Search*: A graph-based search algorithm wherein the next node visited from the frontier is chosen based on the MIN *total-path-cost* of the node. Total-path-cost is the cost of reaching that node from the start node. A Uniform Cost Search reduces to a Breadh-First-Search when all graph edges have the same cost (eg. cost of 1). Frontier = priority queue.

*Recursive-Best-First-Search*:

*Greedy-Best-First-Graph-Search*:

*A-Star Search*:

### Environment Characteristics

The planning problem being considered has the following characteristics:
- Observability: Full (vs Partial)
- Agent: Single (vs Multi-Agent)
- Determinism: Deterministic (vs Stochastic)
- Progression: Sequential (not Episodic)
- Dynamism: Static (not Dynamic)
- Time: Discrete (not Continuous)
- Knowledge: Known (not Unknown)

### Problem Definition

A summary of the "Air Cargo Transport" problem, in Planning Domain Definition Language (PDDL).

#### Action Schema
  Action(Load(c, p, a),
    PRECOND: At(c, a) ∧ At(p, a) ∧ Cargo(c) ∧ Plane(p) ∧ Airport(a)
    EFFECT: ¬ At(c, a) ∧ In(c, p))
  Action(Unload(c, p, a),
    PRECOND: In(c, p) ∧ At(p, a) ∧ Cargo(c) ∧ Plane(p) ∧ Airport(a)
    EFFECT: At(c, a) ∧ ¬ In(c, p))
  Action(Fly(p, from, to),
    PRECOND: At(p, from) ∧ Plane(p) ∧ Airport(from) ∧ Airport(to)
    EFFECT: ¬ At(p, from) ∧ At(p, to))

#### Problem 1

  Init(At(C1, SFO) ∧ At(C2, JFK) 
    ∧ At(P1, SFO) ∧ At(P2, JFK) 
    ∧ Cargo(C1) ∧ Cargo(C2) 
    ∧ Plane(P1) ∧ Plane(P2)
    ∧ Airport(JFK) ∧ Airport(SFO))
  Goal(At(C1, JFK) ∧ At(C2, SFO))

#### Problem 2

  Init(At(C1, SFO) ∧ At(C2, JFK) ∧ At(C3, ATL) 
    ∧ At(P1, SFO) ∧ At(P2, JFK) ∧ At(P3, ATL) 
    ∧ Cargo(C1) ∧ Cargo(C2) ∧ Cargo(C3)
    ∧ Plane(P1) ∧ Plane(P2) ∧ Plane(P3)
    ∧ Airport(JFK) ∧ Airport(SFO) ∧ Airport(ATL))
  Goal(At(C1, JFK) ∧ At(C2, SFO) ∧ At(C3, SFO))

#### Problem 3

  Init(At(C1, SFO) ∧ At(C2, JFK) ∧ At(C3, ATL) ∧ At(C4, ORD) 
    ∧ At(P1, SFO) ∧ At(P2, JFK) 
    ∧ Cargo(C1) ∧ Cargo(C2) ∧ Cargo(C3) ∧ Cargo(C4)
    ∧ Plane(P1) ∧ Plane(P2)
    ∧ Airport(JFK) ∧ Airport(SFO) ∧ Airport(ATL) ∧ Airport(ORD))
  Goal(At(C1, JFK) ∧ At(C3, JFK) ∧ At(C2, SFO) ∧ At(C4, SFO))

## Optimal Plan

Overall, 'A-Star with the ignore-preconditions heuristic' performed the fastest - generally due to fewer node expansions, goal tests, and new node creations - among the search algorithms used, while also yielding an optimal path in each case.

The contrast in performance as compared to non-heuristic searches is reflected especially so in larger problems, such as problem # 2 and problem # 3. This is understandable, since non-heuristic searches grow exponentially as we expand the frontier of the search at each step.

Problem # 1, however, being smaller (smaller state space and action schema definition), was more amenable to the non-heuristic search methods, and understandably was as performant as the heuristic-based algorithms, in terms of the 'Runtime Metrics', while still guaranteeing optimal plans. Out of these non-heuristic searches, the greedy-best-first-graph search achieved an optimal plan with the fewest number of node expansions, goal tests and new node creations. Consequently, it also performed the fastest. In general, however, the A* searches with the various heuristics were not too far off, and not only achieved optimal plans, but also did that generally as efficiently as the non-heuristic approaches.

Furthermore, in all three problems, the heuristic-based A-Star searches that eventually completed, all yielded an optimal path of the same length as the optimal path generated by the non-heuristic search results, which is certainly encouraging!

## Comparison of non-heuristic search result metrics (optimality, runtime, node expansions)

Among the non-heuristic-based search algorithms, a simple Breadth First Search seems to have achieved optimal performance both in terms of the plan length and runtime metrics.

### Depth First Search

This algorithm appears to have created the least optimal plans (by a significant margin), amongst all the other search algorithms, across all the 3 problems. However, it achieved this with the fewest node expansions, goal tests and new nodes as compared to the other non-heuristic approaches below. The tradeoff of getting faster performance at the cost of sub-optimal results, however, makes this algorithm the worst choice among the choices.

The reason for sub-optimal results is that the algorithm picks the first avaialble plan connecting the initial state to the goal state, regardless of its optimality. It focuses on finding "a" planning path, rather than finding one of "the" paths from initial to goal states. For the same reason, the expansions, goal tests, new nodes and runtime for this search algorithm applied to problem # 3 were lower than that for problem # 2, even though the size difference might have suggested the opposite. In this particular case, it is likely that the transitions from initial to goal states for problem # 3 might have been clustered closer to the side of the graph that the depth first search expanded first.

However, that may not always be the case. It is possible to conceive of a scenario wherein all valid paths might be clustered near the side of the graph that gets expanded last by this algorithm. In that scenario, BFS-based non-heuristic searches would likely yield better runtime metrics than the DFS approaches, thereby beating DFS both in plan optimality as well as runtime metrics.

### Breadth First Search

The issue seen with DFS's non-determinism and resulting sub-optimality is not found with breadth-first traversal since all nodes at a given level would be visited before going to the next level, thereby assuring that if a path existed to a goal, the most straightforward/optimal/shortest such path would be discovered, rather than just "a" possible path of any length. This ensures that BFS yields optimal solutions.

As already mentioned, for these problems, BFS's runtime metrics were worse than that of DFS, but that may not always be the case, and therefore cannot be considered seriously in this comparitive analysis.

### Uniform Cost Search

Overall, UCS achieved an optimal plan for *all 3 problems*, but the runtime metrics were not as optimal as those achieved by BFS. UCS consistently underperformed BFS in those metrics because 

## Comparison & contrast of A-Star heuristic search result metrics
* Ignore preconditions:  In all 3 problems, A-Star with the ignore-preconditions heuristic yielded optimal results in a highly performant manner.
* Level sum: A-Star with the level-sum heuristic took > 10 mins for problems 2 and 3. Though it yielded an optimal solution for problem 1, it did not bet the performance of A-Star with the ignore-preconditions heuristic for the same problem.

### What was the best heuristic?

A-Star with the ignore-preconditions heuristic, was the best performing path planning search agent across the domain of non-heuristic-based and heuristic-based algorithms, barring an exceptional performance by the greedy-best-first-search algorithm for problem 1. A-Star with the ignore-preconditions heuristic not only consistently achieved optimal plans for each of the 3 problems, it also did so with significantly less runtime, node expansions, goal tests and new nodes, when compared to those metrics across the other algorithms (with the exception of the greedy-best-first-search algorithm that performed the best for problem 1). Considering however, that problem 1 was smaller and more amenable to an exhaustive search, A-Star with ignore-preconditions seems to shine far more in cases where the problem has a larger state space and action schema.

## Heuristic vs Non-heuristic search comparison

In general, the non-heuristic-based search algorithms guarantee optimal planning, but at heavy performance cost. For instance, for both problems 2 and 3, three of the non-heuristic based algorithms not mentioned above (i.e, Breadth First Tree Search, Depth Limited Search, and Recursive Best First Search) took > 10 mins. The only non-heuristic-based search algorithms to not guarantee optimal planning were the Greedy-Best-First-Graph-Search, the Depth-First-Graph-Search and the Depth-Limited-Search algorithms. This makes sense for the first algorithm because *<……>*, and as for the latter two algorithms, their performance makes sense too, since the depth-first approach is not well suited to this type of problem anyway.

Heuristic-based search algorithms, on the other hand, tended to yield an optimal plan for all three problems, with the exception of the A-Star with level-sum heuristic, that took > 10 mins for problems 2 and 3. Considering that 3 out of the 7 non-heuristic-based search algorithms took > 10 mins for problems 2 and 3, 1 out of 3 for the heuristic-based search algorithms is a pretty good performance improvement without sacrificing optimal planning. Of course, these are best-effort algorithms, so cannot be guaranteed to converge on the optimal solution, but for the majority of cases, as with problems 1, 2 and 3, they were sufficient.
