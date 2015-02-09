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

| **Criterion** | **BFS**            | **Uniform Cost**                            | **DFS**            | **Depth Limited**  | **ID-DFS**         | **Bidirectional**    |
|---------------|--------------------|---------------------------------------------|--------------------|--------------------|--------------------|----------------------|
|   Complete?   | Yes<sup>a</sup>    | Yes<sup>a,b</sup>                           |   No               | Yes<sup>a</sup>    | Yes<sup>a</sup>    | Yes<sup>a,d</sup>    |
|     Time      | *O(b<sup>d</sup>)* | *O(b*<sup>*1+⌊C*<sup>\*</sup>*/ϵ⌋*</sup>*)* | *O(b<sup>m</sup>)* | *O(b<sup>l</sup>)* | *O(b<sup>l</sup>)* | *O(b<sup>d/2</sup>)* |
|     Space     | *O(b<sup>d</sup>)* | *O(b*<sup>*1+⌊C*<sup>\*</sup>*/ϵ⌋*</sup>*)* | *O(bm)*            | *O(bl)*            | *O(bd)*            | *O(b<sup>d/2</sup>)* |
|    Optimal?   | Yes<sup>c</sup>    | Yes                                         |   No               | No                 | Yes<sup>c</sup>    | Yes<sup>c,d</sup>    |

(+ is actually supposed to be \*.)

Evaluation of tree-search strategies. *b* is the branching factor; *d* is the depth of the shallowest solution; *m* is the maximum depth of the search tree; *l* is the depth limit. Superscript caveats are as follows: <sup>a</sup> complete if *b* is finite; <sup>b</sup> complete if step costs >= ϵ for positive ϵ. <sup>c</sup> optimal if step costs are all identical; <sup>d</sup> if both directions use breadth-first search. 

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

 - **Optimality**: Uniform-cost search is optimal in gneral. Whenever it selects a node `n` for expansion, the optinal path to that node as been found. Why is this? Assume there exists another frontier node `n'` on the optimal path from the start node to `n`. By definition, `n'` would have a lower `g`-cost than `n` and would have been selected first. Since step-costs are non-negative, paths will never get shorter as nodes are added. 
 - **Completeness**: Uniform-cost search is complete, assuming that the branching-factor *b* is finite, and that the step costs are >= ϵ where ϵ is some small positive-number. If the second assumption does not hold, it can get stuck in an infinite loop if there is a path with an infinite sequence of zero-cost actions (i.e., a sequence of *NoOp* actions). Completeness is therefore guaranteed provided the cost of every step exceeds some small positive-constant ϵ.
 - **Time** and **Space**: The algorithm is guided by path costs rather than depth and so the runtime and space complexity cannot really be expressed in terms of *b* and *d*. Instead, let *C*<sup>\*</sup> be the cost of the optimal solution, and assume that every action costs *at least* ϵ. Then the worst-case time and space complexity is *O(b*<sup>*1+⌊C*<sup>\*</sup>*/ϵ⌋*</sup>*)*, which can be much greater than *b<sup>d</sup>*. Here we're first looking at the cost per step, and then the branching-factor is raised to that power. We add `1` because we apply the goal test when we *select nodes for expansion*. The reason we can have runtime and space complexity greater than *b<sup>d</sup>* is because the algorithm can explore large tree of small steps because exploring paths that involve large, and perhaps useful steps. When all step costs are equal, uniform-cost search is similar to BFS except that BFS stops as soon as it generates a goal whereas uniform-cost examines all nodes at the goal's depth to see if any have a lower cost. Hence, uniform-cost search does more work by expanding nodes at depth *d* unnecessarily.

DFS
---

This search algorithm always expands the *deepest* node in the current frontier of the search tree. The search goes to the deepest level of the search tree, where the nodes have no successors. As those nodes are expanded, they are removed from the frontier, so then the search "backs up" to the next deepest node that still has unexplored successors. Uses a stack (LIFO queue). This means the most-recently-generated node is selected for expansion. 

 - **Completeness**: The graph-search version (which avoids repeated states and redundant paths) is complete in finite state-spaces because it will eventually expand every node. The tree-search version is **not complete**; it can keep following a loop forever. The DFS tree-search algorithm can be modified at no extra memory-cost so that it checks new states against those on the path from the root to the current node. This avoids infinite loops in finite state-spaces but does not avoid the issue of redundant paths. In infinite state-spaces, both versions vail if an infinite non-goal path is encountered (e.g., Knuth's 4 problem; DFS will keep applying the same operator over and over again). 
 - **Optimality**: Both versions are non-optimal for similar reasons. Assuming we have a search space where we have a goal node `C` on the right-subtree at some depth `d`, and a goal node `J` on the left subtree at some depth `d'` (`d' > d`). Then DFS will start by exploring the left subtree even though `C` is a goal node. Further, it would end up returning `J` as a solution even though `C` is a better solution. Hence DFS is not optimal. 






