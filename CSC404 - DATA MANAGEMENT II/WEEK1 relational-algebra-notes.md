# Relational Algebra — Part 1: Basics & Core Operators

> Study notes based on the Stanford relational algebra lecture (Video 1 of 2).
> Covers the query language fundamentals and the most common operators.

---

## 1. What Is Relational Algebra?

**Relational algebra** is a *formal language* — a mathematical algebra that forms the theoretical underpinning of implemented query languages like **SQL**.

Two key properties to remember:

- It **operates on relations** (tables).
- It **produces relations as a result.**

Because the output of a query is itself a relation, you can:

- Feed that result into **another query**, or
- **Combine** it with other existing relations.

This *closure property* (input is a relation, output is a relation) is what makes operators **composable** — you can stack them freely.

> **Context — "relation" vs "table":** A *relation* is the formal term for what SQL calls a *table*. A *tuple* = a row; an *attribute* = a column; the *schema* = the set of attribute names (the table's column structure).

---

## 2. The Example Database

All examples use a simple **college admissions** database with three relations. Keep this picture in mind throughout.

### `College`
| Attribute | Meaning |
|-----------|---------|
| **cName** *(key)* | College name |
| state | State the college is in |
| enrollment | Number of students enrolled |

### `Student`
| Attribute | Meaning |
|-----------|---------|
| **sID** *(key)* | Student ID |
| sName | Student name |
| GPA | Grade point average |
| sizeHS | Size of the high school attended |

### `Apply`
| Attribute | Meaning |
|-----------|---------|
| **sID** *(part of key)* | Student ID (who is applying) |
| **cName** *(part of key)* | College applied to |
| **major** *(part of key)* | Major applied for |
| decision | Application outcome (e.g., **Y** = accept, **R** = reject) |

> **Keys are underlined in the lecture (shown in bold here).** A **key** is an attribute (or set of attributes) whose value is *guaranteed to be unique* across the relation.

**Key assumptions for this database:**

- College names are unique → `cName` is the key of `College`.
- Student IDs are unique → `sID` is the key of `Student`.
- A student applies to a given college for a given major **only once** → the key of `Apply` is the *combination* `(sID, cName, major)`. This is called a **composite key.**

---

## 3. The Simplest Query: A Relation Name

The simplest valid relational algebra expression is just the **name of a relation**:

```
Student
```

Running this returns a **copy of the entire `Student` relation**. Straightforward — but it's the base case that every more complex expression builds on.

From here, we apply **operators** to **filter**, **slice**, and **combine** relations.

---

## 4. Select Operator — σ (picks ROWS)

The **select** operator picks **certain rows (tuples)** out of a relation based on a condition.

- **Symbol:** σ (Greek sigma)
- **Subscript:** the *condition* used to filter rows
- **Form:** `σ_condition (Relation)`

> **Memory aid:** **Sel**ect → **S**igma → picks rows (horizontal slice).

### Example 1 — single condition
Find students whose GPA is greater than 3.7:

```
σ_(GPA > 3.7) (Student)
```

Returns the subset of `Student` rows where GPA > 3.7.

### Example 2 — two conditions (AND)
Combine conditions using the logical AND operator **∧** (a caret `^`):

Find students with GPA > 3.7 **and** high school size < 1000:

```
σ_(GPA > 3.7 ∧ sizeHS < 1000) (Student)
```

### Example 3 — condition on another relation
Find applications to Stanford for a CS major:

```
σ_(cName = 'Stanford' ∧ major = 'CS') (Apply)
```

### General form
```
σ_condition (Relation)   →   subset of the relation's rows
```

> **Conditions** can use comparisons (`=`, `≠`, `<`, `>`, `≤`, `≥`) and be combined with logical operators **∧** (AND), **∨** (OR), and **¬** (NOT).

---

## 5. Project Operator — π (picks COLUMNS)

The **project** operator picks **certain columns (attributes)** from a relation. Where `select` slices horizontally (rows), `project` slices vertically (columns).

- **Symbol:** π (Greek pi)
- **Subscript:** the *list of column names* to keep
- **Form:** `π_(attr1, attr2, ...) (Relation)`

> **Memory aid:** **P**roject → **P**i → **P**icks columns.

### Example
Get just the student IDs and decisions from all applications:

```
π_(sID, decision) (Apply)
```

This keeps **all the tuples** of `Apply`, but **only the `sID` and `decision` columns.**

### General form
```
π_(list of attributes) (Relation)
```

---

## 6. Composing Operators (Rows AND Columns)

Because every query produces a relation, operators can be **nested**. This lets us pick *some rows* **and** *some columns* at once.

Find the IDs and names of students with GPA > 3.7:

```
π_(sID, sName) ( σ_(GPA > 3.7) (Student) )
```

Read inside-out:
1. `σ_(GPA > 3.7) (Student)` → rows with high GPA.
2. `π_(sID, sName) (...)` → keep only the ID and name columns.

### The *real* general form (the "slight deception")

The lecture admits the earlier definitions were simplified. The operators don't just apply to a *relation name* — they apply to **any relational algebra expression**:

```
σ_condition ( <any expression> )
π_(attributes) ( <any expression> )
```

You can compose as deeply as you like — select over project over select, etc. Use **parentheses** to keep big expressions clear.

---

## 7. Duplicate Elimination (Sets vs. Bags)

Consider listing every major applied for and its decision:

```
π_(major, decision) (Apply)
```

You might expect many repeats (CS/Y, CS/Y, CS/N, EE/Y, …).

**Relational algebra eliminates duplicates automatically.** Each distinct result appears **exactly once**.

| Model | Duplicates? | Used by |
|-------|-------------|---------|
| **Set** semantics | Eliminated | **Relational algebra** (this course) |
| **Multiset / Bag** semantics | Kept | **SQL** |

> **Important real-world note:** SQL is based on **multisets (bags)** and does **not** remove duplicates by default — you must write `SELECT DISTINCT`. A bag-based relational algebra exists too, but these notes use **set** semantics throughout.

---

## 8. Cross-Product — × (Cartesian Product)

The first operator that **combines two relations.** Also called the **Cartesian product.**

What it does: "glues" two relations together.

- **Resulting schema** = the **union** of the two relations' schemas (all columns from both).
- **Resulting contents** = **every combination** of one tuple from the first with one tuple from the second.

### Size of the result
If `Student` has **S** tuples and `Apply` has **A** tuples, then:

```
Student × Apply   →   S × A tuples
```

One row for *every possible pairing*.

### Schema with eight attributes — and naming clashes
`Student` (4 attributes) × `Apply` (4 attributes) = a result with **8 attributes.**

Both relations contain an `sID` attribute. To disambiguate, columns are **prefixed with the relation name**:

```
Student.sID   vs   Apply.sID
```

> **Why bother with cross-product?** On its own it produces a lot of meaningless pairings (every student matched with every application, even unrelated ones). Its power appears when **combined with `select`** to keep only the meaningful combinations.

---

## 9. Worked Example: Cross-Product + Select

**Goal (in English):** Find the **names and GPAs** of students with **high school size > 1000** who **applied to CS** and were **rejected.**

We need data from both `Student` and `Apply`, so start with their cross-product:

```
Student × Apply
```

This gives an 8-attribute relation with every student–application pairing. Now filter it down with a big selection:

**Step 1 — match real pairings only** (the same student in both halves):
```
σ_(Student.sID = Apply.sID) (Student × Apply)
```
Without this, we'd keep nonsensical rows pairing a student with someone *else's* application.

**Step 2 — add the remaining filters:**
```
σ_(Student.sID = Apply.sID
   ∧ sizeHS > 1000
   ∧ major = 'CS'
   ∧ decision = 'R')
  (Student × Apply)
```

**Step 3 — keep only name and GPA:**
```
π_(sName, GPA) (
  σ_(Student.sID = Apply.sID
     ∧ sizeHS > 1000
     ∧ major = 'CS'
     ∧ decision = 'R')
    (Student × Apply)
)
```

This full expression answers the English query. Note how essential the `Student.sID = Apply.sID` "join condition" is — it's what makes the combination meaningful.

---

## 10. Natural Join — ⋈

The pattern above (cross-product + equality on shared attributes) is so common that relational algebra provides a dedicated operator: the **natural join.**

- **Symbol:** ⋈ (a bow tie — searchable in most text editors)

**What the natural join does (three things):**

1. Performs a **cross-product** of the two relations.
2. **Enforces equality** on **all attributes that share the same name** in both relations.
3. **Removes the duplicate columns** (keeps one copy of each shared attribute, since the values are guaranteed equal anyway).

> **Why duplicates can be dropped:** If `Student.sID` *must equal* `Apply.sID` for a row to survive, keeping both columns is redundant — they're always identical. So the join keeps a single `sID` column.

### Same query, rewritten with natural join

Because `Student` and `Apply` share `sID`, the join handles the matching automatically — we no longer write the `Student.sID = Apply.sID` condition ourselves:

```
π_(sName, GPA) (
  σ_(sizeHS > 1000 ∧ major = 'CS' ∧ decision = 'R')
    (Student ⋈ Apply)
)
```

Cleaner than the cross-product version. **Setting up your schemas with consistent attribute names makes natural join very powerful.**

### Joining three relations — adding a `College` filter

**New requirement:** only applications to colleges with **enrollment > 20,000.**

So far we used `Student` and `Apply` but not `College`. Bring it in by joining all three:

```
π_(sName, GPA) (
  σ_(sizeHS > 1000 ∧ major = 'CS' ∧ decision = 'R' ∧ enrollment > 20000)
    (Student ⋈ Apply ⋈ College)
)
```

- `Student ⋈ Apply` matches on shared `sID`.
- `... ⋈ College` matches on shared `cName` (both `Apply` and `College` have `cName`).

> **Associativity:** Natural join is technically a *binary* operator, but it's **associative**, so people usually write `A ⋈ B ⋈ C` without parentheses. Being pedantic, you *could* write `(A ⋈ B) ⋈ C`.

### Natural join adds no expressive power

The natural join is **convenient notation** — it doesn't let you express anything you couldn't already express with cross-product, select, and project.

**General rewrite of `E1 ⋈ E2`:**

```
E1 ⋈ E2  ≡  π_(schema(E1) ∪ schema(E2)) (
              σ_(E1.A1 = E2.A1 ∧ E1.A2 = E2.A2 ∧ ...) (
                E1 × E2
              )
            )
```

Where:
- `schema(E1) ∪ schema(E2)` is a **true set union** — shared attribute names appear **once** (this achieves the duplicate-column removal).
- `A1, A2, ...` are the attributes that share the same name in both expressions (this achieves the equality enforcement).

---

## 11. Theta Join — ⋈_θ

The **theta join** combines two expressions with an **arbitrary condition θ** (theta).

- **Symbol:** ⋈ with subscript **θ**
- **Form:** `E1 ⋈_θ E2`
- θ is a condition in the same style as a `select` condition.

**Definition (it's simple):** theta join = apply the condition θ to the cross-product.

```
E1 ⋈_θ E2  ≡  σ_θ (E1 × E2)
```

Like natural join, theta join **adds no expressive power** — it's an abbreviation.

> **Why it matters in practice:** Most **database management systems implement the theta join as their fundamental operation** for combining relations: *take two relations, form all tuple combinations, keep only those passing condition θ.* When database practitioners say **"join,"** they usually mean **theta join.**

**Distinction to remember:**
- **Natural join** matches automatically on *equal, same-named* attributes and removes duplicate columns.
- **Theta join** uses *any explicit condition* you supply (e.g., `Student.GPA > 3.5`, `A.x ≤ B.y`) and does **not** auto-remove columns.

---

## 12. Summary

Relational algebra is a **formal language** that **operates on relations and produces relations.** The simplest query is just a relation name; **operators** then filter, slice, and combine relations.

| Operator | Symbol | Purpose | Adds expressive power? |
|----------|--------|---------|------------------------|
| **Select** | σ | Pick **rows** matching a condition | Yes (core) |
| **Project** | π | Pick **columns** (attributes) | Yes (core) |
| **Cross-product** | × | Combine **every pair** of tuples from two relations | Yes (core) |
| **Natural join** | ⋈ | Combine relations, auto-matching same-named attributes & dropping duplicate columns | No (abbreviation) |
| **Theta join** | ⋈_θ | Cross-product filtered by an arbitrary condition θ | No (abbreviation) |

**Key takeaways:**

- Output relations can feed into further queries → operators **compose**.
- Relational algebra uses **set semantics** → **duplicates are eliminated** (unlike SQL's bags).
- σ picks rows, π picks columns; nest them to do both.
- × is rarely useful alone but powerful with σ.
- ⋈ and ⋈_θ are *conveniences* rewritable via × + σ + π.
- In real database systems, "join" usually means **theta join.**

### Quick symbol reference
| Symbol | Name | Meaning |
|--------|------|---------|
| σ | sigma | select (rows) |
| π | pi | project (columns) |
| × | times | cross-product |
| ⋈ | bow tie | natural join |
| ⋈_θ | bow tie + theta | theta join |
| ∧ ∨ ¬ | caret / etc. | AND, OR, NOT |
| ∪ | union | set union (on schemas/relations) |

---

*Next video (Part 2): additional relational algebra operators and alternative notations.*
