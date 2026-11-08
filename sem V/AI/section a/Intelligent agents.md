# Intelligent Agents — Short Exam Notes

## 1. Intelligent Agent

An **intelligent agent** is a system that **perceives its environment through sensors and acts on it through actuators** to achieve goals.

**Examples:** robot, self-driving car, virtual assistant, game-playing program.

```mermaid
flowchart LR
    E[Environment] -->|Percepts| S[Sensors]
    S --> A[Agent]
    A -->|Actions| T[Actuators]
    T --> E

    classDef env fill:#ecfeff,color:#164e63,stroke:#0891b2,stroke-width:2px;
    classDef agent fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef io fill:#f5f3ff,color:#3730a3,stroke:#818cf8,stroke-width:2px;
    class E env;
    class A agent;
    class S,T io;
```

---

## 2. Structure of an Intelligent Agent

The basic structure contains:

- **Sensors:** collect information from the environment.
- **Agent program:** processes percepts and decides what to do.
- **Actuators:** perform the selected action.
- **Environment:** the world in which the agent operates.

### Agent = Architecture + Program

**Agent = Architecture + Agent Program**

```mermaid
flowchart TD
    A[Agent] --> B[Architecture]
    A --> C[Agent Program]
    B --> D[Sensors + Actuators]
    C --> E[Maps percepts<br/>to actions]

    classDef root fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef item fill:#eef2ff,color:#312e81,stroke:#6366f1,stroke-width:2px;
    classDef detail fill:#fafafa,color:#374151,stroke:#d1d5db;
    class A root;
    class B,C item;
    class D,E detail;
```

---

## 3. Agent Performance

The **performance measure** defines how successful an agent is.

A good performance measure should reflect the **actual goal**, not merely the agent's actions.

**Example — Vacuum cleaner:**
- Clean floor → good performance
- Use less energy → better performance
- Avoid damaging objects → better performance

### PEAS Framework

PEAS describes an agent's task environment:

| Component | Meaning | Example: Self-driving car |
|---|---|---|
| **P** | Performance measure | Safety, speed, comfort |
| **E** | Environment | Roads, traffic, pedestrians |
| **A** | Actuators | Steering, brakes, accelerator |
| **S** | Sensors | Cameras, GPS, radar |

**Exam shortcut:** **PEAS = Performance, Environment, Actuators, Sensors**

---

## 4. Rational Agent

A **rational agent** chooses the action expected to **maximize its performance measure**, based on:
- Percept history
- Available knowledge
- Available actions
- Expected outcomes

**Important:** Rational does **not** mean perfect. It means choosing the best expected action using the information available.

```mermaid
flowchart LR
    P[Percepts] --> K[Knowledge +<br/>Percept History]
    K --> D[Choose Rational Action]
    D --> A[Action]
    A --> R[Expected<br/>Performance]

    classDef input fill:#eff6ff,color:#1e3a8a,stroke:#3b82f6,stroke-width:2px;
    classDef decision fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef output fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;
    class P,K input;
    class D decision;
    class A,R output;
```

---

## 5. Types of Agent Environment

An environment can be classified using several properties:

| Property | Type 1 | Type 2 |
|---|---|---|
| **Observability** | Fully observable | Partially observable |
| **Agents** | Single-agent | Multi-agent |
| **Predictability** | Deterministic | Stochastic |
| **Episodes** | Episodic | Sequential |
| **Change** | Static | Dynamic |
| **State space** | Discrete | Continuous |
| **Knowledge** | Known | Unknown |

### Simple meanings

- **Fully observable:** agent can access all relevant information.
- **Partially observable:** some information is hidden or unavailable.
- **Single-agent:** only one agent is making decisions.
- **Multi-agent:** multiple agents affect the outcome.
- **Deterministic:** next state is determined by the current state and action.
- **Stochastic:** outcomes involve uncertainty.
- **Episodic:** each task is largely independent.
- **Sequential:** current actions affect future decisions.
- **Static:** environment does not change while the agent is deciding.
- **Dynamic:** environment can change during decision-making.
- **Discrete:** limited/countable states or actions.
- **Continuous:** values can vary over a range.
- **Known:** rules/outcomes are known to the agent.
- **Unknown:** agent must learn or discover them.

---

## 6. Agent Architectures / Types of Agents

### 1. Simple Reflex Agent
Acts only on the **current percept** using condition–action rules.

**Example:** If obstacle detected → turn.

### 2. Model-Based Reflex Agent
Maintains an **internal model/state** to handle partially observable environments.

### 3. Goal-Based Agent
Chooses actions that help achieve a **goal**.

### 4. Utility-Based Agent
Chooses actions based on a **utility function**, allowing comparison between different desirable outcomes.

### 5. Learning Agent
**Learns from experience** and improves its performance over time.

```mermaid
flowchart TD
    A[Agent Types] --> B[Simple Reflex]
    A --> C[Model-Based]
    A --> D[Goal-Based]
    A --> E[Utility-Based]
    A --> F[Learning Agent]

    B --> B1[Current percept]
    C --> C1[Internal state]
    D --> D1[Goal]
    E --> E1[Utility]
    F --> F1[Experience + learning]

    classDef root fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef type fill:#f5f3ff,color:#3730a3,stroke:#7c3aed,stroke-width:2px;
    classDef detail fill:#fafafa,color:#374151,stroke:#d1d5db;
    class A root;
    class B,C,D,E,F type;
    class B1,C1,D1,E1,F1 detail;
```

### Learning Agent Structure

A learning agent is commonly described using four components:

- **Performance element:** selects actions.
- **Learning element:** improves the performance element.
- **Critic:** gives feedback about performance.
- **Problem generator:** suggests exploratory actions.

```mermaid
flowchart LR
    E[Environment] --> P[Percepts]
    P --> PE[Performance<br/>Element]
    PE --> A[Actions]
    A --> E
    E --> C[Critic]
    C --> L[Learning<br/>Element]
    L --> PE
    PG[Problem<br/>Generator] --> PE

    classDef env fill:#ecfeff,color:#164e63,stroke:#0891b2,stroke-width:2px;
    classDef core fill:#312e81,color:#fff,stroke:#1e1b4b,stroke-width:3px;
    classDef learn fill:#ecfdf5,color:#065f46,stroke:#10b981,stroke-width:2px;
    class E env;
    class PE core;
    class C,L,PG learn;
```

---

## 7. Quick Revision

**Intelligent Agent:** Sensors → Agent → Actuators

**PEAS:** Performance + Environment + Actuators + Sensors

**Rational Agent:** chooses the action expected to maximize performance.

**Agent types:**  
**Simple Reflex → Model-Based → Goal-Based → Utility-Based → Learning**

**Environment properties:**  
Observable, Agents, Deterministic, Episodic, Static, Discrete, Known.

---

## Mindmap

```mermaid
mindmap
  root((Intelligent Agents))
    Structure
      Sensors
      Agent Program
      Actuators
      Environment
    Performance
      Performance Measure
      PEAS
        Performance
        Environment
        Actuators
        Sensors
    Rational Agent
      Percepts
      Knowledge
      Expected outcomes
      Maximize performance
    Environment
      Fully / Partially observable
      Single / Multi-agent
      Deterministic / Stochastic
      Episodic / Sequential
      Static / Dynamic
      Discrete / Continuous
      Known / Unknown
    Architectures
      Simple Reflex
      Model-Based
      Goal-Based
      Utility-Based
      Learning Agent
        Performance element
        Learning element
        Critic
        Problem generator
```


