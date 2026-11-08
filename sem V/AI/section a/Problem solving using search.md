# Problem Solving Using Search — Short Exam Notes

## 1. Problem Solving Using Search

In AI, **problem solving by search** means finding a sequence of actions from an **initial state** to a **goal state**.

A search problem is usually defined by:
- **Initial state** — where the agent starts.
- **Actions/operators** — possible actions.
- **Transition model** — result of applying an action.
- **Goal test** — checks whether the goal is reached.
- **Path cost** — total cost of the solution path.

```mermaid
flowchart LR
    I[Initial State] --> A[Apply Actions]
    A --> S[Search State Space]
    S --> G{Goal Test}
    G -->|No| S
    G -->|Yes| F[Solution Path]

    classDef start fill:#eff6ff,color:#1e3a8a,stroke:#3b82f6,stroke-width:2px;
    classDef search fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef goal fill:#fef3c7,color:#92400e,stroke:#f59e0b,stroke-width:2px;
    classDef final fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;
    class I,A start;
    class S search;
    class G goal;
    class F final;
```

---

## 2. State Space Search

A **state space** is the set of all possible states reachable from the initial state.

**State space search** explores these states to find a path to the goal.

### Example: 8-Puzzle

```mermaid
flowchart TD
    A[Initial<br/>State] --> B[Move 1]
    A --> C[Move 2]
    B --> D[Move 3]
    B --> E[Move 4]
    C --> F[Move 5]
    D --> G[Goal State]
    E --> G

    classDef start fill:#eff6ff,color:#1e3a8a,stroke:#3b82f6,stroke-width:2px;
    classDef state fill:#f5f3ff,color:#3730a3,stroke:#8b5cf6,stroke-width:1.5px;
    classDef goal fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;
    class A start;
    class B,C,D,E,F state;
    class G goal;
```

**Key idea:** Search = **states + actions + goal test + path cost**.

---

# 3. Uninformed Search Strategies

**Uninformed (blind) search** uses no extra knowledge about how close a state is to the goal.

| Strategy | Main idea | Key point |
|---|---|---|
| **BFS** | Explore level by level | Complete; optimal with equal step costs |
| **DFS** | Go as deep as possible first | Low memory; not generally optimal |
| **Uniform Cost Search** | Expand lowest path-cost node | Optimal when step costs are non-negative |
| **Depth-Limited Search** | DFS with a depth limit | Avoids infinite depth |
| **Iterative Deepening** | Repeated depth-limited DFS | Combines low memory with BFS-like completeness |

### BFS vs DFS

```mermaid
flowchart LR
    A[Start] --> B[B]
    A --> C[C]
    B --> D[D]
    B --> E[E]
    C --> F[F]
    C --> G[G]

    B1[BFS:<br/>A → B → C → D → E → F → G]
    D1[DFS:<br/>A → B → D → E → C ...]

    classDef node fill:#f5f3ff,color:#3730a3,stroke:#8b5cf6,stroke-width:1.5px;
    classDef info fill:#f8fafc,color:#334155,stroke:#cbd5e1;
    class A,B,C,D,E,F,G node;
    class B1,D1 info;
```

---

# 4. Informed Search Strategies

**Informed (heuristic) search** uses additional knowledge called a **heuristic** to guide the search toward the goal.

### Heuristic Function

**h(n)** = estimated cost from node **n** to the goal.

### Greedy Best-First Search
Chooses the node with the smallest:

**h(n)**

It tries to reach the goal quickly but is **not generally optimal**.

### A* Search
Uses:

**f(n) = g(n) + h(n)**

Where:
- **g(n)** = actual cost from start to n
- **h(n)** = estimated cost from n to goal
- **f(n)** = estimated total solution cost

With an appropriate admissible heuristic and standard conditions, A* can produce an optimal solution.

```mermaid
flowchart TD
    A[Start] --> B[Candidate State]
    B --> C{Evaluate}
    C --> D["g(n) = cost so far"]
    C --> E["h(n) = estimated cost"]
    D --> F["A*: f(n) = g(n) + h(n)"]
    E --> F
    F --> G[Expand promising state]

    classDef start fill:#eff6ff,color:#1e3a8a,stroke:#3b82f6,stroke-width:2px;
    classDef eval fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef formula fill:#fef3c7,color:#92400e,stroke:#f59e0b,stroke-width:2px;
    classDef finish fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;

    class A start;
    class B,C eval;
    class D,E,F formula;
    class G finish;
```

### Uninformed vs Informed

| Uninformed | Informed |
|---|---|
| No heuristic | Uses heuristic |
| Explores more blindly | Guides search toward goal |
| Examples: BFS, DFS, UCS | Examples: Greedy, A* |

---

# 5. Production System

A **production system** is a rule-based problem-solving system consisting of:

1. **Rule set (production rules)** — IF–THEN rules.
2. **Working memory** — current facts/state.
3. **Control strategy** — decides which rule to apply.

### Example

**IF** temperature is high  
**THEN** turn on the fan.

```mermaid
flowchart LR
    M[Working Memory<br/>Current Facts] --> C[Control Strategy]
    R[Production Rules<br/>IF → THEN] --> C
    C --> A[Apply Selected Rule]
    A --> M

    classDef memory fill:#eff6ff,color:#1e3a8a,stroke:#3b82f6,stroke-width:2px;
    classDef rules fill:#fef3c7,color:#92400e,stroke:#f59e0b,stroke-width:2px;
    classDef control fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef action fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;
    class M memory;
    class R rules;
    class C control;
    class A action;
```

## 6. Characteristics of Production Systems

- **Rule-based:** knowledge is represented as IF–THEN rules.
- **Modular:** rules can often be added or changed independently.
- **Pattern matching:** rules are selected by matching conditions with working memory.
- **Control strategy:** resolves conflicts when multiple rules can fire.
- **State changing:** applying rules changes the current state.
- **Traceable:** the sequence of applied rules can explain how a solution was reached.
- **May be monotonic or non-monotonic:** applying a rule may or may not remove previously valid facts.

---

## Quick Revision

### Search
**Initial state → Search → Goal state**

### State Space
All possible states + transitions between them.

### Uninformed
**BFS, DFS, UCS, DLS, IDS**

### Informed
**Greedy:** uses **h(n)**  
**A\*:** uses **f(n) = g(n) + h(n)**

### Production System
**Rules + Working Memory + Control Strategy**



---

# Mindmap

```mermaid
mindmap
  root((Problem Solving Using Search))
    State Space Search
      Initial state
      Actions
      Transition model
      Goal test
      Path cost
    Uninformed Search
      BFS
        Level by level
      DFS
        Deep first
      Uniform Cost
        Lowest path cost
      Depth Limited
      Iterative Deepening
    Informed Search
      Heuristic
        h of n
      Greedy Best First
        Uses heuristic
      A Star
        Total cost formula
        Cost so far
        Estimated cost
    Production System
      Production rules
        IF THEN
      Working memory
      Control strategy
      Characteristics
        Modular
        Pattern matching
        Conflict resolution
        State changing
```
