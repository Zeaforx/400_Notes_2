# Comprehensive Study Notes: Set Theory, Relations, and Formal Proofs

---

## 1. Set Theory Fundamentals

Set theory is the mathematical science of collections of objects. 

### Basic Symbols & Definitions
* **$U$ (Universal Set):** The set containing all possible elements under consideration in a specific problem.
* **$\cup$ (Union):** Combines elements of two sets. ($A \cup B$ means elements in $A$, or $B$, or both).
* **$\cap$ (Intersection/Conjunction):** The common elements shared by two sets. ($A \cap B$ means elements in *both* $A$ and $B$).
* **$\emptyset$ (Null/Empty Set):** A set containing no elements.
* **$'$ (Complement):** Everything in the Universal set that is *not* in the specified set. (e.g., $A'$).
* **$\rightarrow$ (Implies):** Used in logic to denote "If... then...".

**Example:**
Let $U = \{1, 2, 3, 4, 5\}$
Let $A = \{1, 2, 3\}$ and $B = \{4, 5\}$
* **Union:** $A \cup B = \{1, 2, 3, 4, 5\}$
* **Intersection:** $A \cap B = \emptyset$ (They share no common numbers, making them *disjoint* sets).

---

### Important Mathematical Laws

**1. Algebraic Inverses (Properties of Numbers)**
* **Additive Inverse:** $a + (-a) = 0$ (Adding a number to its negative equals zero).
* **Multiplicative Inverse:** $a \cdot (1/a) = 1$ (Multiplying a number by its reciprocal equals one).

**2. Set Theory Laws**
* **Absorption Law:** * $A \cup (A \cap B) = A$
    * $A \cap (A \cup B) = A$
    * *Explanation:* If you take set $A$, and union it with the intersection of $A$ and $B$, you aren't adding anything new that isn't already in $A$. It just "absorbs" into $A$.
* **De Morgan's Laws:** * $(A \cup B)' = A' \cap B'$ (The complement of a union is the intersection of the complements).
    * $(A \cap B)' = A' \cup B'$ (The complement of an intersection is the union of the complements).

---

## 2. Mathematical Relations

A relation (usually denoted as $R$) describes a connection between elements of sets. If element $a$ is related to element $b$, we write **$aRb$**.

### Core Properties of Relations
1.  **Reflexive:** Every element is related to itself. 
    * Rule: **$aRa$**
    * *Example:* The relation "is equal to" ($=$). $5 = 5$ is true, so equality is reflexive.
2.  **Symmetric:** If $a$ is related to $b$, then $b$ must be related to $a$.
    * Rule: **$aRb \implies bRa$**
    * *Example:* The relation "is a sibling of". If John is a sibling of Mary, Mary is a sibling of John.
3.  **Transitive:** If $a$ is related to $b$, and $b$ is related to $c$, then $a$ is related to $c$.
    * Rule: **$aRb$ and $bRc \implies aRc$**
    * *Example:* The relation "is taller than". If Alice is taller than Bob, and Bob is taller than Charlie, then Alice is taller than Charlie.

**Equivalence Relation:**
A relation is an Equivalence Relation *only if* it is Reflexive, Symmetric, AND Transitive.

---

## 3. Formal Proofs

A formal proof is a logical sequence of statements that starts with accepted facts (hypotheses) and logically leads to a conclusion.

### Type 1: Direct Proof (Constructive Proof)
You assume the hypothesis is true, and use standard math rules to show the conclusion must also be true.

**Example:** *Prove that the product of two odd numbers is an odd number.*
1.  **Define the terms:** An odd number can always be written as $2k + 1$ (where $k$ is an integer). 
2.  **Set up variables:** Let $a = 2x + 1$ and $b = 2y + 1$.
3.  **Perform the operation:** $a \cdot b = (2x + 1)(2y + 1)$
4.  **Expand (FOIL):** $= 4xy + 2x + 2y + 1$
5.  **Factor:** Factor out a $2$ from the first three terms to match the odd number definition: $2(2xy + x + y) + 1$
6.  **Conclude:** Let $(2xy + x + y)$ be an integer $z$. The expression is $2z + 1$. This fits the exact definition of an odd number.

---

## 4. The Distributive Law of Sets (Two-Part Proof)

To prove two sets are perfectly equal, you must prove that elements of the first set belong to the second, AND that elements of the second set belong to the first.

**Prove:** $R \cup (S \cap T) = (R \cup S) \cap (R \cup T)$

**Prove:** If $x \in R \cup (S \cap T)$, then $x \in (R \cup S) \cap (R \cup T)$

1.  Let $x$ be an element in $R \cup (S \cap T)$. *(Given / Hypothesis)*
2.  This means $x$ is in $R$, **OR** $x$ is in $(S \cap T)$. *(Definition of Union)*
3.  Therefore, $x$ is in $R$, **OR** ($x$ is in $S$ **AND** $x$ is in $T$). *(Definition of Intersection)*
4.  Because of this logic, we know two things must be true:
    * $x$ is in $R$ or $x$ is in $S \implies x$ is in $(R \cup S)$
    * $x$ is in $R$ or $x$ is in $T \implies x$ is in $(R \cup T)$
5.  Since both of the above lines are true, $x$ is in **both** of them.
6.  Therefore, $x \in (R \cup S) \cap (R \cup T)$.

**Prove:** If $x \in (R \cup S) \cap (R \cup T)$, then $x \in R \cup (S \cap T)$

1.  Let $x$ be an element in $(R \cup S) \cap (R \cup T)$. *(Given / Hypothesis)*
2.  This means $x$ is in $(R \cup S)$ **AND** $x$ is in $(R \cup T)$. *(Definition of Intersection)*
3.  Therefore, ($x$ is in $R$ **OR** $x$ is in $S$) **AND** ($x$ is in $R$ **OR** $x$ is in $T$). *(Definition of Union)*
4.  Because "$x$ is in $R$" is a shared possibility in both of those statements, logical distribution tells us that either $x$ is in $R$, **OR** it must be in both of the other sets. 
5.  This translates to: $x$ is in $R$, **OR** ($x$ is in $S$ **AND** $x$ is in $T$). *(Logical Distribution)*
6.  This means $x$ is in $R$, **OR** $x$ is in $(S \cap T)$. *(Definition of Intersection)*
7.  Therefore, $x \in R \cup (S \cap T)$. *(Definition of Union)*

**Conclusion:** Because the logic holds in both directions, $R \cup (S \cap T) = (R \cup S) \cap (R \cup T)$.

---

## 5. Other Proof Methods

### Proof by Contrapositive
If you have a statement "If $H$ (Hypothesis), then $C$ (Conclusion)", its contrapositive is "**If NOT $C$, then NOT $H$**". 

A statement and its contrapositive are logically equivalent (they are either both true or both false). Sometimes, it is easier to prove the contrapositive than the original statement.
* **Original:** If it is raining ($H$), the grass is wet ($C$).
* **Contrapositive:** If the grass is NOT wet (not $C$), then it is NOT raining (not $H$).

### Proof by Contradiction
Instead of proving a statement is true directly, you assume the statement is **false**. You then follow the math until you hit an impossible mathematical contradiction (like $0 = 1$). Because the math breaks, your assumption that the statement was false must be incorrect, meaning the original statement is true.