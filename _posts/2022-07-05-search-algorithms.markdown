---
layout: post
title: "Search algorithms"
excerpt: "Discussing and comparing AI problem-solving strategies."
---

## 1. Search Problems

Tree search helps find a path leading from the initial state of the search problem (stored as the root of the tree) to the goal state. Each node corresponds to a state in the space state and the edges correspond to actions.

The following ___8-puzzle problem__ definition_ is the common way of defining a search problem:

- __States__: configurations of tiles.
- __Initial State__: any state.
- __Actions__: moving the blank tile _left_, _right_, _up_, _down_.
- __Transition model__: switch places between blank tile and the tile situated in the direction of switching.
- __Action cost__: an action equals 1.

### 1.1 Uninformed Search Methods

Uninformed means that the agent does not have any more information about the problem than its definition (no bigger picture of the problem).

#### 1.1.1 Breadth-First Search

Iterates through the state space with a queue (FIFO), which helps to expand the shallowest node first.

- complete
- cost-optimal (for problems with the same cost for all actions)
- space complexity =
$ O(b^d) $
- time complexity =
$ O(b^d) $

*_b_ = branching factor, _d_ = depth.

### 1.2 Informed Search algorithms

Informed means that the agent has some additional information besides what the problem definition is providing. Such as the heuristic function
$ h(n) $,
which calculates the cost from the _n_ node to the final goal node.

#### 1.2.1 Greedy best-first search

GBFS selects the node with the lowest _h(n)_ value to expand.

- complete (in finite state spaces)
- not optimal
- space complexity = _O(S)_
- time complexity = _O(S)_

*_S_ = number of states.

## 2. Optimization Problems

Optimization problems are solved to find the best state according to an objective function.

To explore optimization problem-solving techniques the algorithms are implemented and tested on the ___traveling salesman problem___. Given a list of cities and the distances between them, we need to search for the shortest path leading through each city once and end up in the start city.

### 2.1 Local Search

While tree search is about finding a path of states as the solution for the agent to follow to reach the goal state, local search is about jumping from neighbor to neighbor and never remembering the path of states.

#### 2.1.1 Hill Climbing

Hill climbing finds the local maximum, ascending from the current state to the next best-valued state. The algorithm never descent, once it reached the "peak" it terminates.

#### 2.1.2 Simulated annealing

SA is similar to hill climbing except it jumps to the next neighbor randomly and allows some descending, in this way, aiming for a global maximum. The algorithm accepts the new state if it is better than the previous state (a value computed by

$ \Delta E = cost(current \_state) - cost(next \_state) $

), and when it is not, it attaches a probability number less than 1. Annealing analogy is about using a temperature _T_ variable that decreases over time and iterations and influences the probability value:

$ e ^ {\Delta E/T} $ .

Thus, "bad" moves are more likely to be accepted at the beginning, when the value of _T_ is high.

#### 2.1.3 Genetic algorithm

Genetic algorithms are part of evolutionary algorithms that have as main idea the natural selection in biology. They use a specific representation of each individual (state) from a population, such as DNA sequence. In the case of the traveling salesman, a sequence of _n = no. of cities_ with values from _0 to n-1_ where used. After initializing the population randomly, each iteration (generation) experiences a few steps:

- selection (_tournament selection_ used)
- recombination
- mutation
- individuals evaluation (with fitness function)
- passing through to the next generation those individuals with the highest fitness function value.

&nbsp;

Implementation of the discussed algorithms can be found [here](https://github.com/lucianodainic/search-algorithms). The scripts are named by each algorithm name acronym. _candidate_solution.in_ and _cost_matrix.in_ are the essentials file for the algorithms to run properly for the traveling salesman problem, containing a random state configuration, to begin with, and the cost matrix containing the distances between cities named with numbers, and _states.in_ contains initial state and the goal state for the 8-puzzle problem.

&nbsp;

___References___

[1] Artificial Intelligence: A Modern Approach, 4th Global ed. Chapter 3, 4.

&nbsp;

__Want to leave a comment?__ Feel free to join the [Discussions](https://github.com/lucianodainic/search-algorithms/discussions/1).
