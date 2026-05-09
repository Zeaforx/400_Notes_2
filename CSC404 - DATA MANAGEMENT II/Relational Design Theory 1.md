# CSC404 — Relational Design Theory

### Student Notes | Functional Dependencies · BCNF · Multivalued Dependencies · 4NF

## Table of Contents

1. [Why Does Schema Design Matter?](#1.-why-does-schema-design-matter)
    
2. [Design Anomalies](#2-design-anomalies)
    
3. [Functional Dependencies (FDs)](#3-functional-dependencies-fds)
    
4. [Closure of Attributes](#4-closure-of-attributes)
    
5. [Boyce-Codd Normal Form (BCNF)](#5-boyce-codd-normal-form-bcnf)
    
6. [Multivalued Dependencies (MVDs)](#6-multivalued-dependencies-mvds)
    
7. [Fourth Normal Form (4NF)](#7-fourth-normal-form-4nf)
    
8. [Shortcomings of BCNF and 4NF](#8-shortcomings-of-bcnf-and-4nf)
    
9. [Hierarchy of Normal Forms](#9-hierarchy-of-normal-forms)
    
10. [Quick Reference Summary](#10-quick-reference-summary)
    

## 1. Why Does Schema Design Matter?

When designing a database, many schema layouts are possible — but most of them are bad. The goal of **Relational Design Theory** is to give us a _principled, automatic_ way to arrive at a good schema.

The two main toolkits are:

|   |   |   |
|---|---|---|
|**Tool**|**Detects**|**Leads to**|
|**Functional Dependencies (FDs)**|Value-to-value constraints|Boyce-Codd Normal Form (BCNF)|
|**Multivalued Dependencies (MVDs)**|Independent repeating facts|Fourth Normal Form (4NF)|

The general approach is called **Design by Decomposition**:

> Start with one big "mega" relation containing all attributes, then systematically break it down using dependency rules until no anomalies remain.

## 2. Design Anomalies

### The Problem: A Bad Schema

Consider student Ann ($SSN = 123$) who:

- Attended **PAHS** and **GHS** (both in Palo Alto)
    
- Plays **tennis** and **trumpet**
    
- Applies to **Stanford**, **Berkeley**, and **MIT**
    

A naive designer might create one flat relation:

`Apply(SSN, sName, cName, HSname, HScity, hobby)`

Because these are independent lists, the database is forced into a **Combinatorial Explosion** to avoid accidentally linking a specific hobby to a specific high school. This requires **1 student × 2 high schools × 2 hobbies × 3 colleges = 12 tuples**.

- Ann's SSN–name pair is stored **12 times**
    
- The fact "Ann attends PAHS" is stored **6 times**
    

> **Redundancy** = every piece of information captured many more times than necessary.

### Three Anomalies That Follow

#### ❶ Redundancy Anomaly

Each fact is stored multiple times. This wastes storage and creates inconsistency risk.

#### ❷ Update Anomaly

To change one fact (e.g., Ann's name), you must update **all 12 copies**. If only 3 of 12 are updated, the database becomes inconsistent.

#### ❸ Deletion Anomaly

Deleting one piece of data accidentally removes other data. For example, removing all tuples where `hobby = 'surfing'` might cause Ann to disappear from the database entirely if surfing was her only hobby in those records.

### A Better Design — Decompose It

Split the mega-relation into smaller, purpose-built relations:

- `Student(SSN, sName)`
    
- `Apply(SSN, cName)`
    
- `HighSchool(SSN, HSname, HScity)`
    
- `Hobbies(SSN, hobby)`
    

**Benefits:**

- Each fact is stored exactly **once** — no redundancy.
    
- No update or deletion anomalies.
    
- The original mega-relation can be **reconstructed by joining** these tables.
    

> The best design depends on what the data represents in the real world.

## 3. Functional Dependencies (FDs)

### Definition

A **functional dependency** $A \rightarrow B$ means:

> Any two tuples (rows) that agree on the value(s) of $A$ must also agree on the value(s) of $B$.

**Formally:** If $t[A] = u[A]$, then $t[B] = u[B]$ for all tuples $t$, $u$ in relation $R$.

Think of it like a mathematical function: knowing $A$ tells you exactly what $B$ is. FDs represent the **"Broken Record"** type of redundancy. If $A \rightarrow B$ but $A$ is not a key, you are just repeating a single, specific fact over and over (e.g., writing "Chicago = IL" 1,000 times).

### Examples

Using `Student(SSN, sName, address, HScode, HSname, HScity, GPA, priority)`:

|   |   |
|---|---|
|**FD**|**Meaning**|
|$SSN \rightarrow sName$|Each SSN belongs to exactly one student|
|$SSN \rightarrow address$|One address per student|
|$HScode \rightarrow \{HSname, HScity\}$|A school code uniquely identifies a school|
|$GPA \rightarrow priority$|Priority is determined purely by GPA|

> **FDs generalise the concept of keys** — a key is just an attribute (or set of attributes) that functionally determines _all_ other attributes.

### Rules for FDs

These rules let us derive new FDs from existing ones:

1. **Splitting Rule:** $A \rightarrow \{B_1, B_2\}$ implies $A \rightarrow B_1$ and $A \rightarrow B_2$.
    
    - _Note: You can split the RHS, but NEVER the LHS!_
        
2. **Combining Rule:** If $A \rightarrow B$ and $A \rightarrow C$, then $A \rightarrow \{B, C\}$.
    
3. **Transitive Rule:** If $A \rightarrow B$ and $B \rightarrow C$, then $A \rightarrow C$.
    

## 4. Closure of Attributes

### Definition

The **closure** of an attribute set $A$ (written $A^+$) is the set of **all** attributes that are functionally determined by $A$, given a set of FDs.

> In other words: starting from $A$, what can you figure out?

### Finding Keys via Closure

- If $A^+$ = all attributes of $R$ $\rightarrow$ $A$ **is a key for** $R$.
    
- To find **all keys**: enumerate every subset of attributes and compute its closure. Optimisation: once $A$ is a key, every superset of $A$ is also a key (superkey), so skip those.
    

## 5. Boyce-Codd Normal Form (BCNF)

### Definition

> **A relation** $R$ **is in BCNF if, for every non-trivial FD** $A \rightarrow B$**,** $A$ **is a key of** $R$**.**

### The "BCNF Trap"

BCNF rules _only_ evaluate Functional Dependencies. A table violates BCNF if it contains an FD where the left side is not a superkey. If a table only contains independent lists (Multivalued Dependencies) and zero FDs, **it passes BCNF verification by default**, even if it contains massive redundancy!

### Example: BCNF Violation

Relation: `Apply(SSN, sName, cName)` with FD $SSN \rightarrow sName$  

- The **key** is $\{SSN, cName\}$ (a student can apply to multiple colleges)
    
- $SSN$ is **not** the key, so for every college Ann applies to, her name is stored again $\rightarrow$ **BCNF VIOLATION**
    

**Fix:** Decompose into:

1. `Student(SSN, sName)` $\leftarrow$ $SSN$ is key ✓
    
2. `Apply(SSN, cName)` $\leftarrow$ $\{SSN, cName\}$ is key ✓
    

## 6. Multivalued Dependencies (MVDs)

### Definition

A **multivalued dependency** $A \twoheadrightarrow B$ means:

> For a given value of $A$, the set of $B$ values associated with it is **independent** of all other attributes.

This causes the **"Combinatorial Explosion"** problem. Because $B$ is a set, and if $C$ is also a set (and they don't talk to each other), the database is forced to manufacture tuples for every possible pairing just to prove no relationship exists between them.

### The Math of Independence

Take the table `Apply(SSN, cName, hobby)` where $SSN \twoheadrightarrow cName$.

If a person with **SSN 123** has **3** Colleges and **4** Hobbies, and you keep them in one table, you must store **12 rows** ($3 \times 4$). If that person adds a 5th hobby, you have to insert 3 new rows (one for each college) just to maintain the rule of independence.

### Rules for MVDs

|   |   |   |
|---|---|---|
|**Rule**|**Statement**|**Practical Meaning**|
|**FD-is-an-MVD**|If $A \rightarrow B$, then $A \twoheadrightarrow B$.|An FD is just a special case of an MVD where the "set" of results only has a size of 1.|
|**Intersection**|If $A \twoheadrightarrow B$ and $A \twoheadrightarrow C$, then $A \twoheadrightarrow (B \cap C)$.|Example: A project ($A$) has independent lists of "Required Skills" ($B$) and "Current Skills" ($C$). The _overlapping_ skills are also an independent set.|
|**Transitive**|If $A \twoheadrightarrow B$ and $B \twoheadrightarrow C$, then $A \twoheadrightarrow (C - B)$.|Prevents "double counting." $A$ determines the _new_ information introduced by $C$ that $B$ didn't already cover.|

**Trivial MVD:** $A \twoheadrightarrow B$ is trivial if $B \subseteq A$ or $A \cup B = \text{all attributes}$. (We only decompose when we find a **non-trivial MVD**).

## 7. Fourth Normal Form (4NF)

### Definition

> **A relation** $R$ **is in 4NF if, for every non-trivial MVD** $A \twoheadrightarrow B$**,** $A$ **is a key of** $R$**.**

- Because every FD is technically a single-item MVD, **4NF is strictly stronger than BCNF**. You cannot reach 4NF without passing through BCNF first.
    
- 4NF handles independence-based combinatorial redundancy.
    

### Example: 4NF Fix

`Apply(SSN, cName, hobby)` with $SSN \twoheadrightarrow cName$:

- $SSN$ is not a key $\rightarrow$ **4NF VIOLATION**
    

**Fix:** Decompose into two tables:

1. `Apply(SSN, cName)` $\leftarrow$ $\{SSN, cName\}$ is key ✓ [4NF]
    
2. `Hobbies(SSN, hobby)` $\leftarrow$ $\{SSN, hobby\}$ is key ✓ [4NF]
    

Now a student with 3 colleges and 4 hobbies needs only **7 tuples** ($3 + 4$), not 12. No redundant combinations.

### Worked Example: 4NF Decomposition

**Start:** `Apply(SSN, cName, date, major, hobby)`

- **FD:** $\{SSN, cName\} \rightarrow date$ (a student applies to each college on one date)
    
- **MVD:** $\{SSN, cName, date\} \twoheadrightarrow major$ (majors and hobbies are independent)
    
- **Key:** all 5 attributes $\rightarrow$ both FD and MVD violate 4NF
    

**Step 1:** Break the MVD (Separate majors from hobbies)

- `A1(SSN, cName, date, major)` $\leftarrow$ still violates FD
    
- `A2(SSN, cName, date, hobby)` $\leftarrow$ still violates FD
    

**Step 2:** Fix the FD (Pull `date` out to stop the "broken record" repeat for every major/hobby)

- From A1: `A3(SSN, cName, date)` and `A4(SSN, cName, major)`
    
- From A2: `A3(SSN, cName, date)` and `A5(SSN, cName, hobby)`
    

**Final schema:**

- `A3(SSN, cName, date)` $\leftarrow$ $\{SSN, cName\}$ is key ✓ [4NF]
    
- `A4(SSN, cName, major)` $\leftarrow$ $\{SSN, cName, major\}$ is key ✓ [4NF]
    
- `A5(SSN, cName, hobby)` $\leftarrow$ $\{SSN, cName, hobby\}$ is key ✓ [4NF]
    

## 8. Shortcomings of BCNF and 4NF

Good theory, but real-world trade-offs exist regarding **Normalization vs. Performance**.

### Problem 1 — Lost Dependencies

Decomposition can **split an FD across different relations**, making it physically impossible for the database engine to enforce "cross-table" rules without slowing down the system.

**Example:** `Apply(SSN, cName, date, major)` with the rule $date \rightarrow cName$ (If you know the date, you know the college).

BCNF decomposes into:

- `A1(cName, date)`
    
- `A2(SSN, date, major)`
    

Now, imagine you have a rule: $\{SSN, cName\} \rightarrow major$.

To verify that a student isn't double-applying for the same major at the same college, the database must now join **A1** and **A2** to see which `cName` is associated with the `date` in the student's record. Checking this on every insert is incredibly expensive.

### Problem 2 — Query Workload

If most queries already need **all attributes**, decomposing forces every query to perform costly joins. In such cases, **intentional denormalisation** may perform better.

> **Third Normal Form (3NF)** is a weaker alternative. It allows a small amount of redundancy if it means all FDs stay within a single table, making them easy and "cheap" to enforce.

## 9. Hierarchy of Normal Forms

```
All relational schemas
└── 1NF: atomic values in every cell
    └── 2NF: no partial dependencies on a composite key
        └── 3NF: no transitive dependencies (dependency-preserving)
            └── BCNF: every non-trivial FD has a key on LHS
                └── 4NF: every non-trivial MVD has a key on LHS
```

**Subset relationship:** `4NF ⊂ BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF`

## 10. Quick Reference Summary

### FD vs MVD

|   |   |   |
|---|---|---|
||**Functional Dependency (FD)**|**Multivalued Dependency (MVD)**|
|**Notation**|$A \rightarrow B$|$A \twoheadrightarrow B$|
|**Meaning**|Same $A \rightarrow$ same $B$ (one value)|Same $A \rightarrow$ all combinations of $B \times \text{rest}$|
|**Redundancy Type**|**"Broken Record"** (Repeating the same single fact)|**"Combinatorial Explosion"** (Forced duplication of rows to prove sets are independent)|
|**Leads to**|BCNF|4NF|
|**Relationship**|FD is a special case (a singleton set).|MVD is the more general concept.|

_Notes compiled from: CSC404 — Database Management Systems, Relational Design Theory (Iheagwara). Based on Widom (2012)._