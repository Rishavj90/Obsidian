# Sets, Relations and Languages — Theory of Computation Notes

## 1. Sets

A **set** is a well-defined collection of distinct objects. The objects are called **elements** or **members** of the set.

Example:
$$
A=\{1,2,3,4\}
$$
Here, 1,2,3,41,2,3,4 are elements of AA.

### 1.1 Ways to represent a set

**Roster / tabular form**

List all elements explicitly.

$$A=\{2,4,6,8\}$$

**Set-builder form**

Describe the property satisfied by the elements.

$$A=\{x\mid x \text{ is an even positive integer less than }10\}$$

### 1.2 Important types of sets

**Empty set**

A set containing no elements:

$$\emptyset=\{\}$$

**Singleton set**

A set containing exactly one element:

$$A=\{5\}$$

**Finite set**

A set having a finite number of elements.

$$A=\{a,b,c,d\}$$

Thus,

$$|A|=4$$

where $|A|$ denotes the **cardinality** of $A$.

**Infinite set**

A set with infinitely many elements.

Example:

$$N=\{0,1,2,3,\ldots\}$$

### 1.3 Subset

$A$ is a subset of $B$ if every element of $A$ is also an element of $B$.

$$A\subseteq B$$

Example:

$$A=\{1,2\},\quad B=\{1,2,3,4\}$$

Therefore,

$$A\subseteq B$$

Every set is a subset of itself:

$$A\subseteq A$$

### 1.4 Proper subset

If $A\subseteq B$ and $A\neq B$, then $A$ is a proper subset of $B$.

Written as:
$$A\subset B$$

### 1.5 Power set

The **power set** of $A$, written $P(A)$, is the set of all subsets of $A$.

For

$A=\{a,b\}$

we have

$$P(A)=\{\emptyset,\{a\},\{b\},\{a,b\}\}$$

If a finite set has $n$ elements, then:

$$|P(A)|=2^n$$

### 1.6 Set operations

For sets $A$ and $B$:

**Union**

Elements belonging to $A$, $B$, or both:

$$A\cup B$$

**Intersection**

Elements common to both:

$$A\cap B$$
**Difference**

Elements in $A$ but not in $B$:

$$A-B$$

**Complement**

Elements in the universal set $U$ that are not in $A$:

$$\overline A=U-A$$
### 1.7 Cartesian product

The Cartesian product of $A$ and $B$ is:
$$A\times B=\{(a,b)\mid a\in A,\ b\in B\}$$
Example:
$$A=\{1,2\},\quad B=\{x,y\}$$
Then:
$$A\times B=\{(1,x),(1,y),(2,x),(2,y)\}$$

If $A$ and $B$ are finite:

$$|A\times B|=|A||B|$$
---

# 2. Relations

A **relation** describes a connection between elements of one set and elements of another set.

A relation $R$ from $A$ to $B$ is any subset of:
$$A\times B$$

Thus,
$$R\subseteq A\times B$$

If $(a,b)\in R$, we say that **$a$ is related to $b$**.

### Example

Let:
$$A=\{1,2,3\},\quad B=\{a,b\}$$

and
$$R=\{(1,a),(2,b),(3,a)\}$$

Then $R$ is a relation from $A$ to $B$.

---

## 2.1 Properties of relations

For a relation $R$ on a set $A$:

### Reflexive

Every element is related to itself.
$$(a,a)\in R\quad\forall a\in A$$

### Symmetric

If $a$ is related to $b$, then $b$ is related to $a$.
$$(a,b)\in R\Rightarrow(b,a)\in R$$

### Antisymmetric

If both $(a,b)$ and $(b,a)$ belong to $R$, then:
$$a=b$$

### Transitive

If $a$ is related to $b$, and $b$ is related to $c$, then $a$ must be related to $c$.
$$(a,b)\in R,\ (b,c)\in R \Rightarrow(a,c)\in R$$

---

## 2.2 Equivalence relation

A relation is an **equivalence relation** if it is:

1. Reflexive    
2. Symmetric
3. Transitive

Equivalence relations divide a set into **equivalence classes**.

Example: Consider integers and define
$$aRb\iff a-b\text{ is divisible by }3$$

This is an equivalence relation.

The equivalence classes are:
$$[0]=\{\ldots,-6,-3,0,3,6,\ldots\}$$ $$[1]=\{\ldots,-5,-2,1,4,7,\ldots\}$$ $$[2]=\{\ldots,-4,-1,2,5,8,\ldots\}$$
---

## 2.3 Partial order relation

A relation is a **partial order** if it is:

- Reflexive
- Antisymmetric
- Transitive

Example:
$$\subseteq$$

on a collection of sets is a partial-order relation.

---

# 3. Functions

A **function** is a special type of relation.

A function from $A$ to $B$ is written:
$$f:A\rightarrow B$$

It assigns **exactly one element of $B$** to every element of $A$.

Here:

- $A$ = domain
- $B$ = codomain
- Actual outputs = range

### Important condition

For every $a\in A$, there must be exactly one $b\in B$ such that:
$$f(a)=b$$

One input cannot have two different outputs.

### Example
$$f(x)=x^2$$

from integers to integers is a function because each input has exactly one output.

---

## 3.1 Types of functions

### One-to-one / Injective

Different inputs produce different outputs.

$$f(a)=f(b)\Rightarrow a=b$$

### Onto / Surjective

Every element of the codomain has at least one preimage.
$$\forall b\in B,\ \exists a\in A:f(a)=b$$

### Bijective

A function that is both injective and surjective.

A bijection establishes a one-to-one correspondence between two sets.

### Why functions matter in Theory of Computation

Functions are used to describe:

- machine transitions
- mappings between strings
- encoding schemes
- computable procedures
- relationships between inputs and outputs

---

# 4. Finite and Infinite Sets

## Finite set

A set $A$ is finite if there exists some non-negative integer $n$ such that:
$$|A|=n$$

Example:
$$A=\{1,2,3,4,5\}$$

has five elements.

## Infinite set

A set is infinite if no finite integer can represent its cardinality.

Example:
$$N=\{0,1,2,3,\ldots\}$$

---

## 4.1 Countably infinite sets

An infinite set is **countably infinite** if its elements can be put into one-to-one correspondence with the natural numbers.

Examples:
$$N,\quad Z$$

The integers are countably infinite even though they include negative numbers.

A possible ordering is:
$$0,1,-1,2,-2,3,-3,\ldots$$

---

## 4.2 Uncountable sets

A set is **uncountable** if no one-to-one correspondence with the natural numbers exists.

A classic example is:
$$R$$

the set of real numbers.

This distinction is important in computability theory because there are more real numbers than there are finite strings over a finite alphabet, and therefore more real numbers than can be represented by ordinary finite computer descriptions.

---

# 5. Closures

The word **closure** in Theory of Computation can refer to applying an operation repeatedly until the __result remains within a specified set__.

For languages, closure properties are particularly important.

A class of languages is said to be **closed under an operation** if applying that operation to languages in the class produces another language in the same class.

For example, regular languages are closed under:
$$\cup,\quad \cap,\quad \text{complement},\quad \text{concatenation},\quad \text{Kleene star}$$

Suppose $L_1$ and $L_2$ are regular languages. Then:
$$L_1\cup L_2$$

is also regular.

Similarly:
$$L_1L_2$$

is regular, and:
$$L_1^*$$

is regular.

### Why closure matters

Closure properties help us:

- prove that a language belongs to a particular class
- construct new languages from known languages
- simplify proofs
- establish limits of computational models

---

# 6. Algorithms

An **algorithm** is a finite, well-defined sequence of steps used to solve a problem or compute a result.

A good algorithm has the following characteristics.

### 6.1 Finiteness

It must terminate after a finite number of steps.

### 6.2 Definiteness

Every step must be precisely defined.

### 6.3 Input

An algorithm may take zero or more inputs.

### 6.4 Output

It must produce one or more results.

### 6.5 Effectiveness

Each operation must be basic enough to be performed mechanically.

### Example

Algorithm for checking whether an integer nn is even:

1. Read $n$.
2. Compute $n\bmod 2$.
3. If the remainder is $0$, output "Even".
4. Otherwise output "Odd".

### Algorithms and computation

Theory of Computation studies not only how to solve problems, but also:

- which problems can be solved
- which cannot be solved
- how efficiently they can be solved
- what computational models can perform the computation

This leads to concepts such as:
$$\text{automata} \rightarrow \text{formal languages} \rightarrow \text{computability} \rightarrow \text{complexity}$$

---

# 7. Alphabets

An **alphabet** is a finite, non-empty set of symbols.

It is usually denoted by:
$$\Sigma$$

Examples:
$$\Sigma=\{0,1\}$$

or
$$\Sigma=\{a,b,c\}$$
The symbols themselves do not need to have numerical meaning.

---

## 7.1 String

A **string** is a finite sequence of symbols from an alphabet.

For:
$$\Sigma=\{0,1\}$$

examples of strings are:
$$0,\ 1,\ 01,\ 101,\ 0001$$

The **empty string** is a string containing no symbols.

It is denoted by:
$$\epsilon$$
and:
$$|\epsilon|=0$$

---

## 7.2 Length of a string

The length of a string $w$ is the number of symbols in it.

If:
$$w=10110$$

then:
$$|w|=5$$
---

## 7.3 Concatenation

Concatenation means joining two strings.

If:
$$x=abc,\quad y=101$$

then:
$$xy=abc101$$

The operation is generally **not commutative**:
$$xy\neq yx$$
---

# 8. Languages

A **formal language** over alphabet $\Sigma$ is any set of strings over $\Sigma$.

The set of all finite strings over $\Sigma$ is written:
$$\Sigma^*$$

Therefore:
$$L\subseteq\Sigma^*$$
is a language over $\Sigma$.

### Example

Let:
$$\Sigma=\{0,1\}$$

A language could be:
$$L=\{0,1,01,10,001,101,\ldots\}$$

There is no requirement that a language contain every possible string.

---

# 9. Important Language Operations

Let $L_1,L_2\subseteq\Sigma^*$.

## Union

$$L_1\cup L_2$$
contains strings belonging to either language.

## Intersection
$$L_1\cap L_2$$

contains strings common to both.

## Difference
$$L_1-L_2$$

contains strings in $L_1$ but not $L_2$.

## Complement

With respect to $\Sigma^*$:
$$\overline{L}=\Sigma^*-L$$

## Concatenation
$$L_1L_2=\{xy\mid x\in L_1,\ y\in L_2\}$$

## Kleene star
$$L^*=\{\epsilon\}\cup L\cup L^2\cup L^3\cup\cdots$$
It represents zero or more concatenations of strings from $L$.

## Kleene plus
$$L^+=L L^*$$

It represents one or more concatenations.

Thus:
$$L^+=L\cup L^2\cup L^3\cup\cdots$$

---

# 10. $\Sigma^*$ and $\Sigma^+$

This distinction is frequently tested in exams.

### $\Sigma^*$

All finite strings over $\Sigma$, including the empty string:

$$\epsilon$$

For:
$$\Sigma=\{a,b\}$$

we have:
$$\Sigma^*=\{\epsilon,a,b,aa,ab,ba,bb,aaa,\ldots\}$$

### $\Sigma^+$

All non-empty finite strings over $\Sigma$:
$$\Sigma^+=\Sigma^*-\{\epsilon\}$$

Hence:
$$\Sigma^+=\{a,b,aa,ab,ba,bb,\ldots\}$$

---

# 11. Language and Machine Relationship

A major idea in Theory of Computation is that computational machines **recognize or generate languages**.

For example:

**Finite automata** recognize regular languages.

**Pushdown automata** recognize context-free languages.

**Turing machines** recognize recursively enumerable languages and decide decidable languages.

A simplified hierarchy is:
$$\text{Regular} \subset \text{Context-Free} \subset \text{Context-Sensitive} \subset \text{Recursively Enumerable}$$

This is called the **Chomsky hierarchy**.

---

# 12. Exam-Oriented Important Differences

### Relation vs Function

A relation can associate one input with:

- zero outputs
- one output
- multiple outputs

A function must associate every input with **exactly one** output.

### Finite vs Infinite

A finite set has a fixed finite number of elements.

An infinite set does not.

### $\Sigma^*$ vs $\Sigma^+$
$$\Sigma^* \text{ includes }\epsilon$$ $$\Sigma^+ \text{ excludes }\epsilon$$
### Kleene star vs Kleene plus

$$L^*=\{\epsilon\}\cup L^+ $$ $$L^+=L L^*$$

### Language vs Alphabet

An alphabet is a set of symbols:
$$\Sigma=\{0,1\}$$

A language is a set of strings constructed from those symbols:
$$L\subseteq\Sigma^*$$

---

# 13. Very Important Definitions to Memorize

For a theory exam, these definitions are worth learning almost word-for-word:

**Set:** A well-defined collection of distinct objects.

**Relation:** A relation from $A$ to $B$ is a subset of $A\times B$.

**Function:** A function from $A$ to $B$ assigns exactly one element of $B$ to every element of $A$.

**Alphabet:** A finite, non-empty set of symbols.

**String:** A finite sequence of symbols from an alphabet.

**Empty string:** A string containing zero symbols, denoted by $\epsilon$.

**Language:** Any set of strings over an alphabet.

**Algorithm:** A finite, definite, effective sequence of steps for solving a problem.

**Closure:** A property stating that applying an operation to members of a class produces another member of the same class.

---

# 14. Quick Revision Formula Sheet
$$A\subseteq B \iff \forall x(x\in A\Rightarrow x\in B)$$$$|P(A)|=2^{|A|}$$ $$A\times B=\{(a,b)\mid a\in A,b\in B\}$$ $$R\subseteq A\times B$$ $$f:A\rightarrow B$$ $$\Sigma^*=\text{all finite strings over }\Sigma$$ $$\Sigma^+=\Sigma^*-\{\epsilon\}$$ $$L\subseteq\Sigma^*$$ $$L^*=\{\epsilon\}\cup L\cup L^2\cup L^3\cup\cdots$$ $$L^+=L\cup L^2\cup L^3\cup\cdots$$

For exams, the most important conceptual chain to remember is:

$$\boxed{\text{Alphabet}\rightarrow\text{Strings}\rightarrow\text{Languages}\rightarrow\text{Machines}}$$

and:

$$\boxed{\text{Set}\rightarrow\text{Relation}\rightarrow\text{Function}}$$

These two chains connect most of the topics in this unit.