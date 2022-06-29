---
layout: post
title: "Search algorithms"
excerpt: "Discussing and comparing AI search algorithms. Implementations and executions are for the 8-puzzle problem."
---

## 8-puzzle problem definiton

- __States__: configurations of tiles.
- __Initial State__: any state.
- __Actions__: moving the blank tile _left_, _right_, _up_, _down_.
- __Transition model__: moving a blank tile means to switch places with the tile situated in the direction of switching.
- __Action cost__: an action equals 1.

## Uninformed Search algorithms

Uninformed means that the agent does not have any more information about the problem than its definition (no bigger picture of the problem).

### Breadth-First Search

Iterates through the state space with a queue (FIFO), which helps to expand the shallowest node first.

- complete
- cost-optimal (for problems with the same cost for all actions)
- space complexity =
$ O(b^d) $
- time complexity =
$ O(b^d) $

*_b_ = branching factor, _d_ = depth.

## Informed Search algorithms

Informed means that the agent has some additional information besides what the problem definition is providing. Such as the heuristic function
$ h(n) $,
which calculates the cost from the _n_ node to the final goal node.

### Greedy best-first search

GBFS selects the node with the lowest _h(n)_ value to expand.

- complete (in finite state spaces)
- not optimal
- space complexity = _O(S)_
- time complexity = _O(S)_

*_S_ = number of states.

&nbsp;

__Want to leave a comment?__ Feel free to join the [Discussions](https://github.com/lucianodainic/search-algorithms/discussions/1).
