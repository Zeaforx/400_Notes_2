# Relational Algebra — Part 2: Set Operators, Renaming & Notations

> Study notes based on the Stanford relational algebra lecture (Video 2 of 2). Builds on Part 1 (select, project, cross-product, natural join, theta join). Covers: the three **set operators** (union, difference, intersection), the **rename** operator, and **alternative notations** (assignment statements & expression trees).

---

## 0. Quick Recap from Part 1

A relational algebra expression is applied to a **set of relations** and produces a **relation** as its result. The same college-admissions database is used:

- **`College`** — `cName` _(key)_, `state`, `enrollment`
- **`Student`** — `sID` _(key)_, `sName`, `GPA`, `sizeHS`
- **`Apply`** — `sID`, `cName`, `major` _(composite key)_, `decision`

### Sample data (same as Part 1)

**`College`**

|cName|state|enrollment|
|---|---|---|
|Stanford|CA|21000|
|Berkeley|CA|36000|
|MIT|MA|11000|
|Cornell|NY|21000|
|Harvard|MA|22000|

**`Student`**

|sID|sName|GPA|sizeHS|
|---|---|---|---|
|123|Amy|3.9|800|
|234|Bob|3.6|1500|
|345|Craig|3.5|500|
|456|Doris|3.9|1000|
|567|Edward|2.9|2000|
|678|Fay|3.8|200|
|789|Gary|3.4|800|

**`Apply`**

|sID|cName|major|decision|
|---|---|---|---|
|123|Stanford|CS|Y|
|123|Stanford|EE|N|
|123|Berkeley|CS|Y|
|234|Berkeley|biology|N|
|234|Stanford|CS|R|
|345|MIT|bioengineering|Y|
|345|Cornell|bioengineering|N|
|345|Cornell|CS|Y|
|345|Cornell|EE|R|
|456|Stanford|CS|N|
|678|Stanford|history|Y|
|789|Cornell|CS|R|

So far we have: **σ** (select, rows), **π** (project, columns), **×** (cross-product), **⋈** (natural join), **⋈_θ** (theta join).

---

## 1. The Key Distinction: Horizontal vs. Vertical Combining

Before the set operators, internalize this contrast — it explains _why_ set operators exist:

|Combining style|Operators|What it does|
|---|---|---|
|**Horizontal**|× , ⋈ , ⋈_θ|Matches a tuple **T1** from one relation with a tuple **T2** from another, producing **wider** rows (more columns).|
|**Vertical**|∪ , − , ∩|Stacks tuples from two relations into **one combined list** (same columns, more/fewer rows).|

The joins from Part 1 combine **horizontally**. The set operators combine **vertically.**

### Abstract picture

Two single-column relations `R₁(A)` and `R₂(A)`:

| `R₁`  | `R₂`  |
| ----- | ----- |
| **A** | **A** |
| a₁    | a₂    |
| a₂    | a₃    |

| Operator  | Result table   |
| --------- | -------------- |
| `R₁ ∪ R₂` | `{a₁, a₂, a₃}` |
| `R₁ − R₂` | `{a₁}`         |
| `R₁ ∩ R₂` | `{a₂}`         |

---

## 2. Set Operators — Schema Requirement

All three set operators (**union**, **difference**, **intersection**) require that **both sides have the same schema** — i.e., the **same attribute names** (and compatible types).

When the schemas don't naturally match, we fix them with the **rename operator** (Section 6). The lecture introduces the set operators first, then circles back to rename.

---

## 3. Union — ∪

Standard set union (as learned in elementary school): combine the tuples of two relations into one, **eliminating duplicates** (set semantics).

### Example — list every college name _and_ student name

We want one flat list. A join/cross-product is **wrong** here — those combine horizontally. We want a **vertical** list, so we use union:

```
π_(cName) (College)   ∪   π_(sName) (Student)
```

1. `π_(cName)(College)` → list of all college names.
2. `π_(sName)(Student)` → list of all student names.
3. `∪` stacks them into a single column.

**Inputs:**

|`π_(cName)(College)`| |`π_(sName)(Student)`|
|---|---|---|
|Stanford||Amy|
|Berkeley||Bob|
|MIT||Craig|
|Cornell||Doris|
|Harvard||Edward|
|||Fay|
|||Gary|

**Result (`∪`):** 12 distinct names in one column.

|name|
|---|
|Stanford|
|Berkeley|
|MIT|
|Cornell|
|Harvard|
|Amy|
|Bob|
|Craig|
|Doris|
|Edward|
|Fay|
|Gary|

> **Technical caveat:** For the union to be _well-formed_, both sides must share the same schema (attribute name). `cName` ≠ `sName`, so strictly this needs a **rename** first — fixed in §6.1. The output column above is called `name` once both sides are renamed.

---

## 4. Difference — − (extremely useful)

`E1 − E2` = all tuples in `E1` that are **not** in `E2`. Written with the **minus sign**. This is the operator that lets you express **"… that did NOT …"** queries.

### Example A — IDs of students who applied **nowhere**

Sounds complex; it's simple:

```
π_(sID) (Student)   −   π_(sID) (Apply)
```

**Inputs:**

|`π_(sID)(Student)`||`π_(sID)(Apply)`|
|---|---|---|
|123||123|
|234||234|
|345||345|
|456||456|
|567||678|
|678||789|
|789|||

**Result:** the only student whose ID isn't in `Apply` is **567 (Edward)**.

|sID|
|---|
|567|

### Example B — _names_ of students who applied nowhere (the "join back" trick)

You **cannot** just add `sName` to both projections — `sName` doesn't exist in `Apply`, so the two sides would have mismatched schemas (one has `(sID, sName)`, the other only `sID`). Subtraction requires matching schemas.

**The trick: compute the IDs first, then _join back_ to `Student` to recover the names.**

```
π_(sName) (
  ( π_(sID)(Student) − π_(sID)(Apply) )  ⋈  Student
)
```

Step by step:

**Step 1 —** difference → just `{567}` (the table above).

**Step 2 —** `... ⋈ Student` — natural join on `sID` re-attaches the full `Student` row:

|sID|sName|GPA|sizeHS|
|---|---|---|---|
|567|Edward|2.9|2000|

**Step 3 —** `π_(sName)(...)`:

|sName|
|---|
|Edward|

> **Why "join back" works:** The natural join matches the small ID set against `Student` on the shared `sID`, giving back the rich schema of `Student` for exactly those students. This pattern — _filter down to keys, then join back to a full relation_ — is very common and worth memorizing.

---

## 5. Intersection — ∩

`E1 ∩ E2` = tuples present in **both** `E1` and `E2`. Again, both sides must share the same schema.

### Example — names that are both a college name and a student name

For this example, imagine our data also contained a college named _Washington_ (say, `(Washington, WA, 30000)`) **and** a student named _Washington_ (say, `(890, Washington, 3.0, 1500)`). The query:

```
π_(cName)(College)   ∩   π_(sName)(Student)
```

**Inputs (with the hypothetical Washington rows added):**

|`π_(cName)(College)`||`π_(sName)(Student)`|
|---|---|---|
|Stanford||Amy|
|Berkeley||Bob|
|MIT||Craig|
|Cornell||Doris|
|Harvard||Edward|
|Washington||Fay|
|||Gary|
|||Washington|

**Result:** the only name appearing on both sides.

|name|
|---|
|Washington|

(With our original data — no shared names — the result would be empty.)

(Schemas must match — handled via rename, §6.1.)

### Intersection adds NO expressive power — _two_ proofs

**Proof 1 — via difference:**

```
E1 ∩ E2  ≡  E1 − (E1 − E2)
```

_Venn-diagram intuition:_ shade all of `E1` (purple). Then `E1 − E2` is the part of `E1` outside `E2` (green). Subtracting the green from the purple leaves exactly the overlap `E1 ∩ E2`. So anything written with `∩` can be rewritten with `−`.

**Proof 2 — via natural join (when schemas are identical):**

```
E1 ∩ E2  ≡  E1 ⋈ E2     (when E1 and E2 have the same schema)
```

_Why:_ with identical schemas, natural join enforces equality on **every** column and drops duplicates — so only tuples appearing in both survive. That's exactly intersection.

> **Takeaway:** `∩` is a _convenience_, not a necessity — but it's still very readable in queries.

---

## 6. Rename Operator — ρ (rho)

The **rename** operator **reassigns the schema** of an expression's result. Symbol: **ρ** (Greek rho). Like all operators, it applies to the result of **any** expression.

### General form

```
ρ_(R(A1, A2, ..., An)) (E)
```

"Compute `E`, then call the result relation **R** with attributes **A1…An**." This new schema is then usable in the enclosing expression.

### Abbreviated forms

- **Rename relation only** (keep `E`'s attribute names): `ρ_R (E)`
- **Rename attributes only** (keep relation name): `ρ_(A1, ..., An) (E)` — must list attributes, otherwise it's indistinguishable from the relation-only form.

The rename operator solves **two distinct problems:**

### 6.1 Use #1 — Fixing schemas for set operators

Recall the union of college and student names had mismatched attribute names. Rename both sides to a common schema:

```
ρ_(C(name)) ( π_(cName)(College) )
   ∪
ρ_(C(name)) ( π_(sName)(Student) )
```

**Before rename** — incompatible schemas:

|`π_(cName)(College)`||`π_(sName)(Student)`|
|---|---|---|
|**cName**||**sName**|
|Stanford||Amy|
|Berkeley||Bob|
|...||...|

**After rename** — both are now relation `C(name)`:

|`ρ_(C(name)) (π_(cName)(College))`||`ρ_(C(name)) (π_(sName)(Student))`|
|---|---|---|
|**name**||**name**|
|Stanford||Amy|
|Berkeley||Bob|
|...||...|

The union is now well-formed. This is purely a **syntactic necessity** for valid set operations.

### 6.2 Use #2 — Disambiguation in **self-joins** (the important use)

A **self-join** combines a relation **with itself**. You need rename so the two copies have **distinct names** to refer to.

---

## 7. Worked Example: Pairs of Colleges in the Same State (Self-Join)

**Goal:** find pairs of colleges located in the same state (e.g., _Stanford & Berkeley_).

This is **horizontal** combining (pairing rows), using **two instances of `College`**. But you can't write `state = state` — _which_ state? The two copies have no distinct names. **Rename fixes this.**

### Approach A — cross-product + rename + select

Rename the two instances so their attributes are distinguishable:

```
C1  :=  ρ_(C1(N1, S1, E1)) (College)      -- name1, state1, enr1
C2  :=  ρ_(C2(N2, S2, E2)) (College)      -- name2, state2, enr2
```

**`C1`** and **`C2`** (same data, distinct attribute names):

|N1|S1|E1|
|---|---|---|
|Stanford|CA|21000|
|Berkeley|CA|36000|
|MIT|MA|11000|
|Cornell|NY|21000|
|Harvard|MA|22000|

Then:

```
σ_(S1 = S2) ( C1 × C2 )
```

`C1 × C2` produces 5 × 5 = **25 rows**. Filtering with `S1 = S2` keeps only same-state pairs. The 9 rows that survive (CA×CA = 2×2 = 4, MA×MA = 2×2 = 4, NY×NY = 1×1 = 1):

|N1|S1|E1|N2|S2|E2|
|---|---|---|---|---|---|
|Stanford|CA|21000|Stanford|CA|21000|
|Stanford|CA|21000|Berkeley|CA|36000|
|Berkeley|CA|36000|Stanford|CA|21000|
|Berkeley|CA|36000|Berkeley|CA|36000|
|MIT|MA|11000|MIT|MA|11000|
|MIT|MA|11000|Harvard|MA|22000|
|Harvard|MA|22000|MIT|MA|11000|
|Harvard|MA|22000|Harvard|MA|22000|
|Cornell|NY|21000|Cornell|NY|21000|

### Approach B — make the join column share a name, then natural join (slicker)

If we rename **both** state attributes to the **same** name `S` (but keep distinct name/enrollment attributes), the natural join automatically enforces equality on `S`:

```
C1  :=  ρ_(C1(N1, S, E1)) (College)
C2  :=  ρ_(C2(N2, S, E2)) (College)

C1 ⋈ C2          -- auto-matches on shared attribute S
```

This gives the same 9 rows but with **one** `S` column (since natural join drops the duplicate).

### Fixing the three quirks of a self-join

The naive pairing above has problems — fix them with selection conditions:

1. **Self-pairs** (`Stanford–Stanford`, `Berkeley–Berkeley`, etc.): a college pairs with itself (5 rows). → add `N1 ≠ N2`.
    
    After `N1 ≠ N2`, 4 rows remain:
    
    |N1|S|E1|N2|E2|
    |---|---|---|---|---|
    |Stanford|CA|21000|Berkeley|36000|
    |Berkeley|CA|36000|Stanford|21000|
    |MIT|MA|11000|Harvard|22000|
    |Harvard|MA|22000|MIT|11000|
    
2. **Mirror duplicates** (`Stanford–Berkeley` _and_ `Berkeley–Stanford`): each pair appears twice in reverse. → **clever fix:** replace `N1 ≠ N2` with **`N1 < N2`**. This keeps only one ordering of each pair, killing both the self-pairs _and_ the mirror duplicates in one stroke.
    

**Final query:**

```
σ_(N1 < N2) ( C1 ⋈ C2 )
```

**Result:** Berkeley < Stanford alphabetically, Harvard < MIT alphabetically.

|N1|S|E1|N2|E2|
|---|---|---|---|---|
|Berkeley|CA|36000|Stanford|21000|
|Harvard|MA|22000|MIT|11000|

Two clean pairs of same-state colleges, with no duplicates and no self-pairings.

> **The point of this example:** the **rename operator is absolutely necessary** here — there is no way to express a self-join without it. (Self-pairs and mirror-pairs handling is a bonus lesson.)

---

## 8. Alternative Notations

Beyond the standard inline form, two other notations are common.

### 8.1 Assignment statements (modular / linear)

Break a big expression into named intermediate steps using `:=`. Same same-state-colleges query:

```
C1  := ρ_(C1(N1, S, E1)) (College)
C2  := ρ_(C2(N2, S, E2)) (College)
CP  := C1 ⋈ C2
Answer := σ_(N1 < N2) (CP)
```

Equivalent to the single big expression — just **modularized** for readability. Useful for complex queries.

### 8.2 Expression trees (visualizing structure)

Operators become **internal nodes**; **relation names are always the leaves**. Evaluation flows **bottom-up**.

**Example:** GPAs of students applying to CS in California (needs all three relations):

```
                π_(GPA)
                   │
        σ_(state = 'California' ∧ major = 'CS')
                   │
                  ⋈            ← natural join of all three
                 ╱│╲
                ╱ │ ╲
         College Student Apply     ← leaves are relation names
```

- **Leaves:** `College`, `Student`, `Apply`.
- The **natural join** of the three enforces `cName = cName` (College↔Apply) and `sID = sID` (Student↔Apply), yielding meaningful "student applies to college" triples.
- Then **select** on `state = 'California' ∧ major = 'CS'`, then **project** `GPA`.

**Tracing through our data** (state = CA, major = CS):

- CA colleges: Stanford (21000), Berkeley (36000).
- CS applications to those: `(123, Stanford, CS, Y)`, `(234, Stanford, CS, R)`, `(456, Stanford, CS, N)`, `(123, Berkeley, CS, Y)`.
- Joining with `Student` to get GPAs: Amy 3.9, Bob 3.6, Doris 3.9, Amy 3.9 (again).
- After projection + duplicate elimination:

|GPA|
|---|
|3.9|
|3.6|

Equivalent linear form:

```
π_(GPA) ( σ_(state='California' ∧ major='CS') (College ⋈ Student ⋈ Apply) )
```

> **Why trees matter:** They make an expression's structure visible, and — importantly — **when SQL is compiled inside a database system, it's typically turned into an expression tree very much like this** (a query plan). This is the bridge from theory to real DBMS implementation.

---

## 9. The Core Language vs. Abbreviations

A major conceptual point: relational algebra has a small **core**, and everything else is **syntactic sugar** (abbreviations that add no expressive power).

### Core constructs

|Construct|Form|Role|
|---|---|---|
|Relation name|`R`|Base query|
|**Select**|`σ_c (E)`|Filter rows|
|**Project**|`π_(attrs) (E)`|Pick columns|
|**Cross-product**|`E1 × E2`|Combine all tuple pairs|
|**Union**|`E1 ∪ E2`|Vertical combine|
|**Difference**|`E1 − E2`|Set subtraction|
|**Rename**|`ρ_(R(A...)) (E)`|Reassign schema|

### Abbreviations (rewritable using the core)

|Abbreviation|Equivalent core expression|
|---|---|
|**Natural join** `E1 ⋈ E2`|`π_(schema union) ( σ_(shared attrs equal) (E1 × E2) )`|
|**Theta join** `E1 ⋈_θ E2`|`σ_θ (E1 × E2)`|
|**Intersection** `E1 ∩ E2`|`E1 − (E1 − E2)` _(or_ `E1 ⋈ E2` _when schemas match)_|

> These abbreviations don't increase what the language _can_ express — but they make queries far easier to read and write.

### A note on parentheses

Parentheses are used purely for **disambiguation**, just like in arithmetic. With practice it's straightforward to tell when they're needed; you can omit them where precedence/associativity makes the meaning unambiguous (e.g., chained associative natural joins).

---

## 10. Summary

Relational algebra is a **formal language** based on **sets**. It uses **set operators** and **other operators that combine data from multiple relations**. It takes relations as input and produces relations as output, and it forms the **formal foundation of implemented relational database management systems.**

**This video added:**

- **∪ (union)** and **− (difference)** — _core_; vertical combining.
- **∩ (intersection)** — _abbreviation_; provably reducible to `−` or (with equal schemas) `⋈`.
- The **"join back"** pattern for recovering full rows after a key-level set operation.
- **ρ (rename)** — _core_; needed for set-operator schema matching and, crucially, for **self-joins**.
- The **same-state-colleges** self-join, including the `N1 < N2` trick for self/mirror pairs.
- Two notations: **assignment statements** (modular) and **expression trees** (visual; mirror real SQL query plans).
- The big idea: a small **core** (`σ π × ∪ − ρ` + relation names) plus **abbreviations** (`⋈ ⋈_θ ∩`).

### Quick symbol reference (Parts 1 & 2)

|Symbol|Name|Meaning|Core or abbrev.|
|---|---|---|---|
|σ|sigma|select (rows)|core|
|π|pi|project (columns)|core|
|×|times|cross-product|core|
|∪|union|set union (vertical)|core|
|−|minus|set difference|core|
|ρ|rho|rename schema|core|
|⋈|bow tie|natural join|abbreviation|
|⋈_θ|bow tie + θ|theta join|abbreviation|
|∩|cap|intersection|abbreviation|
|∧ ∨ ¬|—|AND, OR, NOT|(conditions)|

---

_End of the two-part relational algebra series. You now have the full set of operators and the relationship between the minimal core and its convenient abbreviations._