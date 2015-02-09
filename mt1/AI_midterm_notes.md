Agents
======

 - Agent perceives its **environment** therough **sensors** and acting upon that environment through **actuators**. 
 - A **percept** refers to the agent's perceptual inputs at any given instant. 
 - An agent's **percept sequence** is the complete history of everything the agent has ever perceived.
 - A **performance measure** evaluates any given sequence of environment states. It captures the notion of desirability.

Rationality
-----------

What is rational at any given time depends on four things:

 - The performance measure that defines the criterion of success.
 - The agent's prior knowledge of the environment.
 - The actions that the agent can perform.
 - The agent's percept sequence to date.

Hence, the definition of a **rational agent**: *For each possible percept sequence, a rational agent should select an action that is expected to maximize its performance measure, given the evidence provided by the percept sequence and whatever built-in knowledge the agent has.*

There is a distinction between **omniscience** and **rationality**. An **omniscient** agent knows the actual outcome of its actions and can act accordingly; but omiscience is impossible in reality. Hence a **rational** agent is not necessarily **omniscient**. It can only act based on what it knows and what it has done so far. 

A rational agent should also be **autonomous**. It should learn what it can to compensate for partial or incorrect prior-knowledge. It should also involve itself in **information gathering** and **exploration** with the aim to maximize performance and in order to modify future percepts.

PEAS
----

**P**erformance, **E**nvironment, **A**ctuators, **S**ensors. This helps us specify the task environment. For example, consider a taxi-driver agent:

 - **Performance Measure**: Safe, fast, legal, comfortable trip, maximize profits.
 - **Environment**: Roads, other traffic, pedestrians, customers.
 - **Actuators**: Steering, accelerator, brake, signal, horn, display.
 - **Sensors**: Cameras, sonar, speedometer, GPS, odometer, accelerometer, engine sensors, keyboard.

Properties of Task Environments
-------------------------------

 - **Fully observable** vs. **partially observable**
 - **Single agent** vs. **multi-agent**
 - **Deterministic** vs. **stochastic** (randomly determined; having random probability distribution).
 - **Episodic** vs. **sequential** - In episodic, agent's experience is divided into atomic episoodes. The next episode does not depend on actions taken in previous episodes (e.g. NN classifier for hand-written digits). In sequential, current decision depends on previous actions and can affect future decisions.
 - **Static** vs. **dynamic**
 - **Discrete** vs. **continuous**
 - **Known** vs. **unknown**

Turing Test
-----------

Introduced by Alan Turing in 1950. It is a test of a machine's ability to exhibit intelligent behavior equivalent to, or indistinguishable from, that of a human. A human judge engages in natural-language conversations with a human and a machine. The machine is designed to generate performance indistinguishable from that of a human being. Conversation is limited to a text-only channel. If the judge cannot reliably tell a machine from a human, the machine is said to have passed the test.

Uninformed Search
=================

**Problem formulation** is the process of deciding what actions and states to consider, given a goal. 

A **problem** can be defined formally by five components:

 - The **initial state** that the agent starts in.
 - A description of the possible **actions** available to the agent. Given a particular state `s`, `ACTIONS(s)` returns the set of actions that can be executed in `s`, i.e., each of these actions is **applicable** in `s`. 
 - A **transition model**, which is s description of what each action does. This is specified by a function `RESULT(s, a)` that returns the state that results from performing action `a` in state `s`. The term **successor** is used to refer to any state reachable from a given state by a single action. Together, the initial state, actions, and transition model implicitly define the **state space** of the problem. 
 - The **goal test**, which determines whether a given state is a goal state. 

Framing a search problem
------------------------

There are **six** components: **states**, **initial state**, **actions**, **transition model**, **goal test**, and **path cost**. Provide a mathematical description of the **transition model**, and **goal test** if possible.

Infrastructure for search algorithms
------------------------------------

For each node `n`, we have a structure that contains four components:

 - `n.STATE`: the state in the state space to which the node corresponds.
 - `n.PARENT`: the node in the search tree that generated this node.
 - `n.ACTION`: the action that was applied to the parent that generated this node (i.e., what action from the parent got me here?)
 - `n.PATH-COST`: the cost, traditionally denoted by `g(n)`, of the path from the initial state to the node, as indicated by the parent pointers.

Measuring problem-solving performance
-------------------------------------

 - **Completeness**: Is the algorithm guaranteed to find a solution when there is one?
 - **Optimality**: Does the strategy find the optimal solution? (**optimal solution**: the **solution** that has the lowest path-cost among all solutions).
 - **Time complexity**: How long does it take to find a solution?
 - **Space complexity**: How much memory is needed to perform the search?

Complexity is represented in terms of three quantities:

 - `b`: The **bgranching factor**, or maximum number of successors of any node.
 - `d`: The **depth** of the *shallowest* goal node.
 - `m`: The **maximum length** of any path in the state space. 

Comparing uninformed-search strategies
--------------------------------------

| **Criterion** | **BFS**            | **Uniform Cost**                     | **DFS**            | **Depth Limited**  | **ID-DFS**         | **Bidirectional**    |
|---------------|--------------------|--------------------------------------|--------------------|--------------------|--------------------|----------------------|
|   Complete?   | Yes<sup>a</sup>    | Yes<sup>a,b</sup>                    |   No               | Yes<sup>a</sup>    | Yes<sup>a</sup>    | Yes<sup>a,d</sup>    |
|     Time      | *O(b<sup>d</sup>)* | *O(b<sup>1+⌊C<sup>+</sup>/ϵ⌋</sup>)* | *O(b<sup>m>/sup>)* | *O(b<sup>l</sup>)* | *O(b<sup>l</sup>)* | *O(b<sup>d/2</sup>)* |
|     Space     | *O(b<sup>d</sup>)* | *O(b<sup>1+⌊C<sup>+</sup>/ϵ⌋</sup>)* | *O(bm)*            | *O(bl)*            | *O(bd)*            | *O(b<sup>d/2</sup>)* |
|    Optimal?   | Yes<sup>c</sup>    | Yes                                  |   No               | No                 | Yes<sup>c</sup>    | Yes<sup>c,d</sup>    |

(+ is actually supposed to be \*.)

Evaluation of tree-search strategies. *b* is the branching factor; *d* is the depth of the shallowest solution; *m* is the maximum depth of the search tree; *l* is the depth limit. Superscript caveats are as follows: <sup>a</sup> coplete if *b* is finite; <sup>b</sup> complete if step costs >= ϵ for positive ϵ. <sup>c</sup> optimal if step costs are all identical; <sup>d</sup> if both directions use breadth-first search. 

BFS (Breadth-first search)
--------------------------

A simple strategy in which root node is expanded first and then all successors of the root node are expanded next, then *their* successors, and so on. In general, all nodes are expanded at a given depth in the search tree, before any nodes at the next level are expanded. This means that the *shallowest* unexpanded node is chosen for expansion. To do this we use a FIFO queue (i.e., regular queue). **The goal test is applied to each node when it is generated rather than when it is selected for expansion**; this is because if we applied the test when it is selected for expansion, we would have to expand the whole layer of nodes at depth *d* before the goal was detected, which makes the runtime complexity *O(b<sup>d + 1</dup>)*. The algorithm discards any new path to a state already in the frontier or explored set (any such much must be *at least as deep* as the one already found). Hence BFS always has the *shallowest* path to every node on the frontier.

 - **Completeness**: BFS is complete. If the shallowest goal-node is at some finite-depth *d*, BFS will eventually find it after generating all shallower-nodes (provided branching-factor *b* is finite). 
 - **Optimality**: BFS is optimal, assuming that the path-cost is a non-decreasing function of the depth of the node. This is easily seen if all actions have the same cost. 
 - **Time**: We generate *b<sup>h</sup>* nodes at each level *h*. So the root generates *b*, and then each of those generate *b*, which leads to *b<sup>2</sup>* at the second level, and so on. Hence in general, we have *O(b<sup>d</sup>)*.
 - **Space**: We store every expanded node in the *explored* set. This means that every node remains in memory, which gives us *O(b<sup>d - 1</sup>)* in the *explored* set. The *frontier* set then has *O(b<sup>d</sup>)* nodes. This means that the size of the *frontier* dominates, which gives us a space complexity of *O(b<sup>d</sup>)*.

**Memory requirements are a bigger problem for BFS than execution time.** That is, we will face the issue of running out of memory, long before the face the issue of waiting way too long for a solution. 

Uniform-cost search
-------------------

When all step costs are qual, BFS is optimal because it always expands the *shallowest* unexpanded node. How about an algorithm that is optimal with any step-cost function? 

 - Instead of expanding the shallowest node, **uniform-cost search** expands the node *n* with the **lowest path-cost `g(n)`**
 - This is done by storing the frontier **as a priority queue** ordered by `g`. 
 - The goal test is applied to a node when it is *selected for expansion* rather than when it is first *generated* (i.e., opposite of BFS). This is because the first goal-node that is *generated* may not necessarily be on the most-optimal path.
 - Another test is also added to check for the case where a better path is found to a node currently on the frontier. 

An example:

```
                  99
[Sibiu] -----------------------[Fagaras]
   \                               |
    \                              |
     \ 80                          |
      \                            |
 [Rimnicu Vilcea]                  |
        \                          |
         \                         |
          \                        |
           \ 97                    |
            \                      |
             \                     |
          [Pitesti]                |
               \                   |
                \                  |
                 \ 101             |
                  \                | 211
                   \               |
                    \              |
                     \             |
                      \            |
                       \           |
                        \          |
                         \         |
                          \        |
                           \       |
                            \      |
                             \     |
                              \    |
                               \   |
                                \  |
                                 \ |
                                  \|
                             [Bucharest]
```

The problem is to get from **Sibiu** to **Bucharest**. The successors of **Sibiu** are **Riminicu Vilcea** and **Fagaras** with path-costs `80` and `99` respectively. Since **Riminicu Vilcea** is the least-cost node, it is expanded next, which gives us **Pitesti** with a total path-cost of `80 + 97 = 177`. Now **Fagaras** is the least-cost node, and so it is expanded, giving us **Bucharest** with a cost of `99 + 211 = 310`. Now also **Bucharest** is the goal node, we keep going since we don't perform the goal test on *generated* nodes. So we will next choose **Pitesti** for expansion which adds a second path to **Bucharest** with the cost `80 + 97 + 101 = 278`. The algorithm now checks to see if this new path is better than the old one; it is and so the old one is discarded. **Bucharest** with `g`-cost `278` is now selected for expansion and then returned as a solution (because we perform the goal test when a node is selected for expansion). 


