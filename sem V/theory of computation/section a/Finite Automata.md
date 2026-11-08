# Finite Automata — Theory of Computation Exam Notes

Finite Automata are mathematical models used to represent systems that have a **finite number of states** and process input symbols one at a time. They are fundamental in **Theory of Computation** and are mainly used to recognize **regular languages**.

---

# 1. Basics of Finite Automata

A **finite automaton (FA)** is an abstract machine consisting of:

1. A finite set of **states**
2. An **input alphabet**
3. A **transition function**
4. An **initial state**
5. A set of **final/accepting states**

A finite automaton reads an input string symbol by symbol and changes its state according to the transition function.

## Formal Definition

A finite automaton can be represented by a 5-tuple:

$$
M=(Q,\Sigma,\delta,q_0,F)
$$

where:

| Symbol   | Meaning                       |
| -------- | ----------------------------- |
| $Q$      | Finite set of states          |
| $\Sigma$ | Input alphabet                |
| $\delta$ | Transition function           |
| $q_0$    | Initial state                 |
| $F$      | Set of final/accepting states |

## Example

Suppose we want an automaton over:

$$
\Sigma=\{0,1\}
$$

that accepts strings ending in `1`.

States:

- $q_0$: last symbol is not `1` / no input yet
- $q_1$: last symbol is `1`
- Initial state = $q_0$
- Final state = $q_1$

Transition table:

| Current State | Input 0 | Input 1 |
|---|---|---|
| $q_0$ | $q_0$ | $q_1$ |
| $q_1$ | $q_0$ | $q_1$ |

For input `101`:

$$
q_0 \xrightarrow{1}q_1
\xrightarrow{0}q_0
\xrightarrow{1}q_1
$$

The machine finishes in $q_1$, so the string is **accepted**.

---

# 2. Deterministic Finite Automaton (DFA)

## Definition

A **DFA** is a finite automaton in which, ==for every state and every input symbol, there is **exactly one possible next state**==.

The transition function is:

$$
\delta:Q\times\Sigma\rightarrow Q
$$

The machine is deterministic because there is no choice or ambiguity about the next state.

## Properties of DFA

For every:

- current state $q$
- input symbol $a$

there must be exactly one transition:

$$
\delta(q,a)=p
$$

A DFA:

- cannot have multiple transitions for the same input symbol from one state
- cannot have $\epsilon$-transitions
- has exactly one computation path for any input string
- accepts a string if the unique computation ends in a final state

## DFA Example

Consider a DFA accepting strings over $\{0,1\}$ that contain an **even number of 0s**.

States:

- $q_e$: even number of 0s
- $q_o$: odd number of 0s

Initial state:

$$
q_e
$$

Final state:

$$
q_e
$$

Transition table:

| State | 0 | 1 |
|---|---|---|
| $q_e$ | $q_o$ | $q_e$ |
| $q_o$ | $q_e$ | $q_o$ |

Reading `0` changes even → odd or odd → even. Reading `1` does not affect the number of zeroes.

For input `10100`, there are three `0`s, so it is rejected.

For `1001`, there are two `0`s, so it is accepted.

## DFA Advantages

- Simple execution
- Predictable behavior
- Easy to implement in software and hardware
- Requires only one active state at any moment

## DFA Disadvantage

A DFA may require **more states** than an equivalent NFA for a particular design.

---

# 3. Non-Deterministic Finite Automaton (NFA/NDFA)

A **Non-Deterministic Finite Automaton (NFA or NDFA)** is a finite automaton where, for a state and input symbol, there may be:

- zero transitions
- one transition
- multiple transitions

The transition function is:

$$
\delta:Q\times\Sigma\rightarrow2^Q
$$

Here $2^Q$ is the **power set of $Q$** because the result can be a set containing multiple states.

Some textbooks distinguish **NFA** from **ε-NFA**, where ε-transitions are allowed. If your syllabus separates them, follow your instructor's convention.

## Example of NFA

Suppose:

$$
Q=\{q_0,q_1,q_2\}
$$

and the machine accepts strings containing the substring `01`.

Transitions:

| State | 0 | 1 |
|---|---|---|
| $q_0$ | $\{q_0,q_1\}$ | $\{q_0\}$ |
| $q_1$ | $\emptyset$ | $\{q_2\}$ |
| $q_2$ | $\{q_2\}$ | $\{q_2\}$ |

The important feature is:

$$
\delta(q_0,0)=\{q_0,q_1\}
$$

There are **two possible next states**.

An NFA accepts the input if **at least one possible path** ends in a final state.

---

# 4. DFA vs NFA

| Feature | DFA | NFA |
|---|---|---|
| Meaning | Deterministic FA | Non-deterministic FA |
| Next state | Exactly one | Zero, one, or multiple |
| Transition function | $Q\times\Sigma\rightarrow Q$ | $Q\times\Sigma\rightarrow2^Q$ |
| ε-transitions | Not allowed | Not in standard NFA; allowed in ε-NFA |
| Computation paths | Exactly one | Potentially multiple |
| Acceptance | Unique path ends in final state | At least one path ends in final state |
| Implementation | Direct | Usually converted to DFA |
| Expressive power | Regular languages | Regular languages |
| State count | May be larger | Often smaller |

## Most Important Point

**DFA and NFA have the same computational power.**

Therefore:

$$
L(DFA)=L(NFA)=\text{Regular Languages}
$$

Every NFA can be converted into an equivalent DFA.

---

# 5. Finite State Machines with Output

Ordinary finite automata answer a question such as:

> Is this input string accepted?

Finite State Machines with Output additionally **produce an output** while processing input.

Two important types are:

1. **Moore Machine**
2. **Mealy Machine**

They are used for modeling sequential systems.

---

# 6. Moore Machine

A **Moore machine** is a finite state machine in which the output depends **only on the current state**.

It can be represented as:

$$
M=(Q,\Sigma,\Delta,\delta,\lambda,q_0)
$$

where:

- $Q$ = states
- $\Sigma$ = input alphabet
- $\Delta$ = output alphabet
- $\delta:Q\times\Sigma\rightarrow Q$ = transition function
- $\lambda:Q\rightarrow\Delta$ = output function
- $q_0$ = initial state

The key equation is:

$$
Output=\lambda(Current\ State)
$$

## Moore Diagram Notation

A state is usually represented as:

$$
q_i/output
$$

The output is associated with the **state**.

## Moore Example

Consider a machine that outputs:

- `0` when it has received an even number of `1`s
- `1` when it has received an odd number of `1`s

States:

- $q_0/0$: even number of 1s
- $q_1/1$: odd number of 1s

Transition table:

| Current State | Input 0 | Input 1 | Output |
|---|---|---|---|
| $q_0$ | $q_0$ | $q_1$ | 0 |
| $q_1$ | $q_1$ | $q_0$ | 1 |

The output depends only on whether the machine is in $q_0$ or $q_1$.

---

# 7. Mealy Machine

A **Mealy machine** is a finite state machine in which the output depends on:

1. Current state
2. Current input

It can be represented as:

$$
M=(Q,\Sigma,\Delta,\delta,\lambda,q_0)
$$

where:

$$
\lambda:Q\times\Sigma\rightarrow\Delta
$$

The key equation is:

$$
Output=\lambda(Current\ State,Current\ Input)
$$

## Mealy Diagram Notation

Transitions are normally labelled:

$$
Input/Output
$$

For example:

```text
q0 -- 0/0 --> q1
```

This means:

- input = `0`
- output = `0`
- transition from $q_0$ to $q_1$

The output is associated with the **transition**.

---

# 8. Moore vs Mealy Machine

| Feature | Moore Machine | Mealy Machine |
|---|---|---|
| Output depends on | Current state | Current state + current input |
| Output associated with | State | Transition |
| Output function | $\lambda:Q\rightarrow\Delta$ | $\lambda:Q\times\Sigma\rightarrow\Delta$ |
| Response to input | Generally after state change | Can respond immediately to input |
| Number of states | Usually more | Usually fewer |
| Output stability | More stable | Can change with input |
| Diagram label | State/output | Input/output on edge |

### Easy Memory Trick

**Moore → output on state**

**Mealy → output on edge**

---

# 9. Practical Applications of Finite Automata

Finite automata are used whenever a system needs to remember a **finite amount of information about its past**.

## 9.1 Lexical Analysis

Compilers use finite automata to recognize tokens such as:

- keywords
- identifiers
- integers
- operators
- punctuation

For example:

```text
while
```

can be recognized as a keyword.

Regular expressions are commonly converted into finite automata for lexical analysis.

---

## 9.2 Pattern Matching

Finite automata can recognize patterns in text.

For example, an automaton can determine whether the pattern `abc` occurs in an input string.

Applications include:

- text search
- search engines
- log processing
- spam filtering
- data validation

---

## 9.3 Regular Expression Matching

Regular expressions and finite automata are closely related.

For example:

$$
(0+1)^*01
$$

describes binary strings ending in `01`.

A regular expression can be converted into an automaton that recognizes the same language.

---

## 9.4 Digital Circuit Design

Finite state machines are used to model sequential digital circuits.

Examples:

- traffic-light controllers
- vending machines
- elevator controllers
- washing machines
- railway signaling systems

---

## 9.5 Communication Protocols

Protocols often have a finite number of states.

For example:

```text
Disconnected
      ↓
Connecting
      ↓
Connected
      ↓
Disconnecting
```

A finite state machine can model valid and invalid transitions.

---

## 9.6 Input Validation

Finite automata can check whether input follows a particular format.

Examples:

- identifiers
- binary numbers
- strings with particular prefixes/suffixes
- simple date formats
- protocol messages

---

# 10. Conversion of NFA to DFA

![](https://www.youtube.com/watch?v=LEigAZN6RdY)

Continue this process until no new states are generated.

## Important Exam Formulas

For NFA → DFA:

$$
\boxed{\delta_{DFA}(S,a)=\bigcup_{q\in S}\delta_{NFA}(q,a)}
$$

Final DFA states:

$$
\boxed{F_{DFA}=\{S\mid S\cap F_{NFA}\neq\emptyset\}}
$$

---

# 12. Mealy to Moore Conversion

This conversion is slightly more complicated because:

- Mealy outputs are on transitions.
- Moore outputs are on states.

![](https://www.youtube.com/watch?v=_gazATZF0R8)


# 13. Moore to Mealy Conversion

This conversion is comparatively easier.

![](https://www.youtube.com/watch?v=MsYkgGHR93s)

# 14. Important Difference in Output Timing

This is important when solving conversion questions.

Suppose the machine starts in state $A$.

In a **Moore machine**, the output of $A$ is available immediately because output is associated with the current state.

In a **Mealy machine**, output is generated when an input causes a transition.

Therefore, output sequences can appear shifted by one position when converting between Moore and Mealy machines.

### Exam Point

A Moore machine may require an **additional initial state** when converting from Mealy if exact output timing needs to be preserved.

---

# 15. Summary of All Conversions

## NFA → DFA

Use:

### **Subset Construction**

Each DFA state = a **set of NFA states**.

$$
\boxed{\delta_D(S,a)=\bigcup_{q\in S}\delta_N(q,a)}
$$

Final DFA states are sets containing at least one NFA final state.

---

## Mealy → Moore

Outputs move:

$$
\boxed{\text{Transitions}\rightarrow\text{States}}
$$

If one destination state receives different outputs, **split the state** into multiple Moore states.

An additional initial state may be required for output alignment.

---

## Moore → Mealy

Outputs move:

$$
\boxed{\text{States}\rightarrow\text{Transitions}}
$$

For every transition, attach the output of the **destination state** to that transition.

---

# 16. Quick Exam Revision Table

| Topic | Key Point |
|---|---|
| DFA | Exactly one transition for each state/input pair |
| NFA | Zero, one, or multiple transitions |
| DFA transition | $Q\times\Sigma\rightarrow Q$ |
| NFA transition | $Q\times\Sigma\rightarrow2^Q$ |
| DFA/NFA power | Both recognize regular languages |
| Moore | Output depends only on state |
| Mealy | Output depends on state + input |
| Moore output | Written with state |
| Mealy output | Written on transition |
| NFA → DFA | Subset construction |
| DFA state | Represents a set of NFA states |
| DFA final state | Set containing an NFA final state |
| Mealy → Moore | Split states according to incoming outputs |
| Moore → Mealy | Put destination-state output on transitions |
| Applications | Compilers, pattern matching, circuits, protocols, validation |

---

# 17. Questions You Should Be Able to Answer in the Exam

## Short-Answer Questions

1. Define a finite automaton.
2. Define DFA with its formal 5-tuple.
3. Define NFA.
4. Differentiate DFA and NFA.
5. What is a Moore machine?
6. What is a Mealy machine?
7. Differentiate Moore and Mealy machines.
8. Give applications of finite automata.
9. What is subset construction?
10. How are final states determined during NFA → DFA conversion?

## Long/Numerical Questions

1. **Construct a DFA** for a specified language.
2. **Construct an NFA** for a specified language.
3. **Convert a given NFA into an equivalent DFA.**
4. **Convert a Mealy machine into a Moore machine.**
5. **Convert a Moore machine into a Mealy machine.**
6. Trace a given input string through a DFA/NFA.
7. Determine whether a given string is accepted.

---

# 18. Most Important Points to Memorize

### Rule 1 — DFA

$$
\boxed{\text{DFA: exactly one possible next state}}
$$

### Rule 2 — NFA

$$
\boxed{\text{NFA: zero, one, or multiple possible next states}}
$$

### Rule 3 — Moore

$$
\boxed{\text{Moore: output = current state}}
$$

### Rule 4 — Mealy

$$
\boxed{\text{Mealy: output = current state + input}}
$$

### Rule 5 — NFA → DFA

$$
\boxed{\text{NFA}\rightarrow\text{DFA}=\text{Subset Construction}}
$$

### Rule 6 — Mealy → Moore

$$
\boxed{\text{Move outputs from transitions to states}}
$$

Split states when the same destination needs different outputs.

### Rule 7 — Moore → Mealy

$$
\boxed{\text{Move destination-state outputs to transitions}}
$$

---

# Final One-Page Memory Sheet

| Concept | Remember |
|---|---|
| **Finite Automaton** | Finite states + input + transitions + start + final states |
| **DFA** | One state per input/state combination |
| **NFA** | Multiple possible states allowed |
| **DFA/NFA power** | Both recognize regular languages |
| **Moore** | Output attached to state |
| **Mealy** | Output attached to transition |
| **NFA → DFA** | Subset construction |
| **DFA state** | Set of NFA states |
| **DFA final** | Set containing an NFA final state |
| **Mealy → Moore** | Transition outputs become state outputs; split states if necessary |
| **Moore → Mealy** | Destination-state output becomes transition output |
| **Applications** | Compilers, regex, pattern matching, circuits, protocols, validation |
