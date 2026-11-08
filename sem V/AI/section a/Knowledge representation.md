# Knowledge Representation — Short Exam Notes

## 1. Knowledge Representation (KR)

**Knowledge Representation** is the process of representing facts, relationships, and rules about the world in a form that an AI system can **store, reason with, and use for decision-making**.

### Main approaches to KR

1. **Symbolic / logical:** represents knowledge using formal symbols, facts, and rules.
2. **Network-based:** represents concepts as nodes and relationships as links.
3. **Structured:** represents knowledge using objects, attributes, and values.
4. **Rule-based:** represents knowledge as IF–THEN rules.

```mermaid
flowchart TD
    A[Knowledge Representation] --> B[Logical]
    A --> C[Network]
    A --> D[Structured]
    A --> E[Rule-Based]

    B --> B1[Logic]
    C --> C1[Semantic Network]
    D --> D1[Frames]
    E --> E1[Production Rules]

    classDef root fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef type fill:#f5f3ff,color:#3730a3,stroke:#7c3aed,stroke-width:2px;
    classDef detail fill:#ecfeff,color:#164e63,stroke:#0891b2,stroke-width:1.5px;
    class A root;
    class B,C,D,E type;
    class B1,C1,D1,E1 detail;
```

---

# 2. Knowledge Representation Techniques

## A. Logical Representation

Uses formal logic to represent **facts and relationships**.

Example:

**Human(Socrates)**  
**∀x [Human(x) → Mortal(x)]**

Therefore:

**Mortal(Socrates)**

**Advantages:** precise, supports formal reasoning.  
**Limitation:** can become complex for large real-world knowledge.

---

## B. Semantic Networks

A **semantic network** represents knowledge as a **graph of nodes and links**.

Example:

```mermaid
flowchart LR
    A[Sparrow] -->|is-a| B[Bird]
    B -->|is-a| C[Animal]
    B -->|has| D[Wings]
    C -->|needs| E[Food]

    classDef specific fill:#eff6ff,color:#1e3a8a,stroke:#3b82f6,stroke-width:2px;
    classDef general fill:#f5f3ff,color:#3730a3,stroke:#8b5cf6,stroke-width:2px;
    classDef property fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;
    class A specific;
    class B,C general;
    class D,E property;
```

**Key idea:** **Nodes = concepts/objects; links = relationships.**

---

## C. Frames

A **frame** represents an object or situation using **slots (attributes)** and their values.

Example:

```text
FRAME: Student
-------------------
Name      : Riya
Age       : 20
Course    : AI
College   : ABC
```

**Key idea:** Frame = **object + attributes + values**.

Frames are useful for representing structured, stereotypical knowledge.

---

## D. Production Rules

Knowledge is represented using **IF–THEN rules**.

Example:

```text
IF  temperature > 38°C
THEN fever = true
```

A production system commonly contains:
- **Rule base**
- **Working memory**
- **Inference/control mechanism**

---

# 3. Propositional Logic

**Propositional logic** represents knowledge using propositions that are either **True or False**.

### Basic operators

| Symbol | Meaning | Example |
|---|---|---|
| **¬P** | NOT | It is not raining |
| **P ∧ Q** | AND | P and Q |
| **P ∨ Q** | OR | P or Q |
| **P → Q** | IMPLIES | If P, then Q |
| **P ↔ Q** | IF AND ONLY IF | P iff Q |

### Example

Let:
- **P:** It is raining.
- **Q:** Road is wet.

Rule:

**P → Q**

Fact:

**P**

Conclusion:

**Q**

**Limitation:** propositional logic cannot naturally represent objects, variables, or relationships between objects.

---

# 4. First-Order Predicate Logic (FOPL)

**First-Order Predicate Logic** extends propositional logic by allowing:
- **Objects**
- **Predicates**
- **Variables**
- **Quantifiers**
- **Relations**

### Example

**Human(Ram)**  
means: Ram is human.

**∀x [Human(x) → Mortal(x)]**  
means: Every human is mortal.

**∃x Student(x)**  
means: There exists at least one student.

### Quantifiers

- **∀** = For all
- **∃** = There exists

```mermaid
flowchart TD
    A[First-Order Predicate Logic] --> B[Constants]
    A --> C[Variables]
    A --> D[Predicates]
    A --> E[Functions]
    A --> F[Quantifiers]

    F --> G["∀  For all"]
    F --> H["∃  Exists"]

    classDef root fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef item fill:#f5f3ff,color:#3730a3,stroke:#7c3aed,stroke-width:2px;
    classDef quant fill:#fef3c7,color:#92400e,stroke:#f59e0b,stroke-width:2px;
    class A root;
    class B,C,D,E,F item;
    class G,H quant;
```

---

# 5. Resolution in First-Order Predicate Logic

**Resolution** is an inference method used to prove whether a conclusion follows from a set of logical statements.

### Basic idea

To prove a goal:
1. Add the **negation of the goal** to the knowledge base.
2. Convert statements to **clause form (CNF)**.
3. Use **unification** to match variables/terms.
4. Apply the **resolution rule**.
5. If an **empty clause (□)** is produced, a contradiction is found, so the original goal is proved.

### Simple example

Given:

**Human(Socrates)**  
**∀x [Human(x) → Mortal(x)]**

Goal: **Mortal(Socrates)**

After conversion:

**¬Human(x) ∨ Mortal(x)**  
**Human(Socrates)**

Unify **x = Socrates**:

**Mortal(Socrates)**

```mermaid
flowchart LR
    A[Knowledge Base] --> B[Negate Goal]
    B --> C[Convert to CNF]
    C --> D[Unification]
    D --> E[Resolution]
    E --> F{Empty Clause?}
    F -->|Yes| G[Goal Proved]
    F -->|No| H[Continue / Not Proved]

    classDef kb fill:#eff6ff,color:#1e3a8a,stroke:#3b82f6,stroke-width:2px;
    classDef process fill:#f5f3ff,color:#3730a3,stroke:#8b5cf6,stroke-width:2px;
    classDef decision fill:#fef3c7,color:#92400e,stroke:#f59e0b,stroke-width:2px;
    classDef result fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;
    class A kb;
    class B,C,D,E process;
    class F decision;
    class G,H result;
```

### Important terms

- **CNF:** Conjunctive Normal Form.
- **Unification:** finding substitutions that make expressions match.
- **Clause:** a disjunction (OR) of literals.
- **Empty clause:** indicates contradiction in resolution.

---

# 6. Architecture of a Knowledge-Based Agent

A **knowledge-based agent** stores knowledge about the world and uses an **inference mechanism** to derive new information and choose actions.

### Main components

- **Knowledge Base (KB):** stores facts and rules.
- **Inference Engine:** derives new facts/conclusions.
- **Percepts:** information received from the environment.
- **Action mechanism:** performs the selected action.

```mermaid
flowchart LR
    E[Environment] -->|Percept| A[Knowledge-Based Agent]
    A --> KB[Knowledge Base]
    KB <--> IE[Inference Engine]
    IE --> DEC[Decision]
    DEC -->|Action| E
    P[New Percept / Fact] --> KB

    classDef env fill:#ecfeff,color:#164e63,stroke:#0891b2,stroke-width:2px;
    classDef agent fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef store fill:#fef3c7,color:#92400e,stroke:#f59e0b,stroke-width:2px;
    classDef infer fill:#f5f3ff,color:#3730a3,stroke:#7c3aed,stroke-width:2px;
    classDef decision fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;
    class E env;
    class A agent;
    class KB store;
    class IE infer;
    class DEC decision;
    class P store;
```

### Working cycle

**Perceive → Add knowledge → Infer → Decide → Act**

---

# Quick Revision

| Topic | One-line definition |
|---|---|
| **Knowledge Representation** | Representing knowledge so AI can reason with it |
| **Logical Representation** | Uses formal logic |
| **Semantic Network** | Concepts connected by relationships |
| **Frames** | Objects represented using slots and values |
| **Production Rules** | IF–THEN knowledge |
| **Propositional Logic** | True/False propositions |
| **FOPL** | Logic with objects, variables, predicates, and quantifiers |
| **Resolution** | Inference by deriving contradiction |
| **Knowledge-Based Agent** | Uses a knowledge base + inference to choose actions |

### Must Remember

**Semantic Network → Nodes + Links**  
**Frame → Slots + Values**  
**Production Rule → IF + THEN**  
**Propositional Logic → True/False propositions**  
**FOPL → Predicates + Variables + Quantifiers**  
**Resolution → Unification + CNF + Contradiction**  
**KBA → Perceive → Know → Infer → Act**

---

# Mindmap

```mermaid
mindmap
  root((Knowledge Representation))
    Approaches
      Logical
      Network-Based
      Structured
      Rule-Based
    Techniques
      Logical Representation
        Facts
        Relations
      Semantic Networks
        Nodes
        Links
      Frames
        Slots
        Values
      Production Rules
        IF
        THEN
    Propositional Logic
      True / False
      NOT
      AND
      OR
      IMPLIES
      IFF
    First Order Predicate Logic
      Constants
      Variables
      Predicates
      Functions
      Quantifiers
        For all
        Exists
    Resolution
      Negate goal
      CNF
      Unification
      Resolution
      Empty clause
    Knowledge Based Agent
      Percepts
      Knowledge Base
      Inference Engine
      Decision
      Action
```
