**Appendix Database Schema (Use for Questions 1, 4, and 5):**

`College(cName, state, enrollment)`

`Student(sID, sName, GPA, sizeHS)`

`Apply(sID, cName, major, decision)`

### QUESTION 1: Relational Algebra & Indexing (20 marks)

a) Write a single relational algebra expression to find the names and GPAs of students with a high school size strictly greater than 1000 who applied to the 'CS' major and received an 'R' (rejected) decision. (4 marks)
ANSWER: $$ πsName,GPA​(σsizeHS>1000∧major="cs"∧decision="R"​(Student⋈Apply)) $$

b) Write a relational algebra expression using the set difference operator to find the student IDs (`sID`) of students who have not applied to any college. (4 marks)
ANSWER: $$ \pi_{SID}(Student) - \pi_{SID}(Apply)  $$

c) Write a relational algebra expression to find pairs of colleges (names only) that are located in the same state. Ensure no college is paired with itself and no mirror duplicates exist in your final result. (4 marks)
	ANSWER: $$ \pi_{C1.cName, C2.cName} (\sigma_{C1.state = C2.state \land C1.cName < C2.cName} (\rho_{C1}(College) \times \rho_{C2}(College))) $$

d) Write the SQL query to create a composite index on `sName` and `GPA` for the Student table. If you execute the query `SELECT * FROM Student WHERE GPA > 3.9 AND sName = 'Mary'`, explain step-by-step how the query optimizer will use this specific composite index to find the result. (4 marks)

e) A database executes the query `SELECT * FROM Student WHERE GPA BETWEEN 3.0 AND 3.5`. Identify the data structure the query optimizer will prefer for this index and explain the mathematical reason for this choice over alternative structures. (4 marks)
ANSWER: B-trees. This is because B-Trees are good for ranges  of values and equality

### QUESTION 2: Functional Dependencies & BCNF (20 marks)

Consider the relation `Student(SSN, sName, address, HScode, HSname, HScity, GPA, priority)` with the following functional dependencies:

1. `SSN -> sName, address, GPA`
    
2. `GPA -> priority`
    
3. `HScode -> HSname, HScity`
    

a) Compute the key of the relation above  (4 marks)
ANSWER:
`{SSN}*` = {SSN}
=> {SSN, sName, address, GPA}
=> {SSN, sName, address, GPA, priority}
Sine we cant go any further we add HScode and add it as a key
`{SSN, HScode}*` = {SSN, sName, address, GPA, priority, HSname, HScity}

b) Compute the attribute closure of `{SSN}`. Explain if it qualifies as a superkey or primary key. (4 marks)

c) Identify all functional dependencies that violate Boyce-Codd Normal Form (BCNF) in the original relation. State the exact rule they fail to satisfy. (4 marks)

d) Perform the first step of decomposition on the relation using the functional dependency `HScode -> HSname, HScity`. List the resulting relations and state their primary keys. (4 marks)

e) Take the non-BCNF relation from your previous step and complete the decomposition into BCNF using the remaining functional dependencies. List the final set of fully normalized schemas. (4 marks)

### QUESTION 3: Multivalued Dependencies & 4NF (20 marks)

Consider the relation `ApplyData(SSN, cName, date, major, hobby)` with the following dependencies:

Functional Dependency (FD): `{SSN, cName} -> date`

Multivalued Dependency (MVD): `{SSN, cName, date} ->> major`

a) Identify the key(s) for this relation using the provided functional dependencies. (4 marks)

b) Given a separate relation with the Multivalued Dependency `NIN ->> hobby`, if the relation contains tuples `(123, piano, CS)` and `(123, boxing, EE)`, write out the specific additional tuples that must exist in the database to satisfy this dependency. (4 marks)

c) Explain how the functional dependency `{SSN, cName} -> date` violates Fourth Normal Form (4NF) in the `ApplyData` relation. (4 marks)

d) Perform the first step of 4NF decomposition on `ApplyData` using the multivalued dependency `{SSN, cName, date} ->> major`. List the resulting two relations. (4 marks)

e) Take the relations from your previous step that still violate 4NF and decompose them using the functional dependency. List the final set of 4NF schemas. (4 marks)

### QUESTION 4: Triggers & Constraints (20 marks)

a) Write a SQL trigger named `cName_cascade_update`. The trigger must act on the `College` table. If a college's `cName` is updated, the trigger must automatically update all corresponding `cName` entries in the `Apply` table to match the new name. (4 marks)

b) Trace the execution of a row-level trigger. Assume you use an SQLite database where triggers execute immediately after each row. Table `T1` currently contains four rows with a single column `value`. The values are: 1, 1, 1, 1. You have an `AFTER INSERT` trigger on `T1` that calculates the current `AVG(value)` from `T1` and inserts that average into table `T2` for every new row inserted. You execute the following bulk insert statement: `INSERT INTO T1 SELECT value + 1 FROM T1;`. List the exact sequential numerical values that will be inserted into table `T2`. (4 marks)

c) Write the SQL `CREATE TABLE` statement for the `Apply` table. You must include a composite primary key, a foreign key to the `Student` table that cascades on delete, and a `CHECK` constraint ensuring the decision is 'Y', 'N', or 'R'. (4 marks)

d) A requirement states that applications cannot be submitted if a college's enrollment exceeds 50,000. Write a `BEFORE INSERT` trigger on the `Apply` table that aborts the transaction if this condition is met. (4 marks)

e) You execute `UPDATE Student SET GPA = NULL WHERE sID = 999;`. Assume `sID` 999 does not exist in the database, but the `GPA` column has a `NOT NULL` constraint. Evaluate the outcome of this execution and state if the database throws a constraint violation error. (4 marks)

### QUESTION 5: Concurrency & Isolation Levels (20 marks)

a) Two clients concurrently update Stanford's enrollment, which starts at 15,000. Client 1 executes `UPDATE College SET enrollment = enrollment + 1000 WHERE cName = 'Stanford'`. Client 2 executes `UPDATE College SET enrollment = enrollment + 1500 WHERE cName = 'Stanford'`. If the database system guarantees strict serializability, calculate the final guaranteed enrollment value. Show the math for the possible execution orders to prove your answer. (4 marks)

b) Transaction T1 executes `UPDATE Student SET GPA = GPA + 0.1 WHERE sizeHS > 1000` but does not commit. Transaction T2 executes `SELECT AVG(GPA) FROM Student` and commits. T1 then executes `ROLLBACK`. If transaction T2 executes under the `READ UNCOMMITTED` isolation level, name the specific data anomaly that occurs. State exactly what data state T2 computes its average from. (4 marks)

c) Transaction T1 reads the total count of student rows. Transaction T2 inserts 100 new student rows and commits. Transaction T1 then reads the total count of student rows a second time. If T1 operates under the `REPEATABLE READ` isolation level, will the first and second counts match? Name the specific concurrency phenomenon occurring here. (4 marks)

d) A transaction calculates a statistical average of GPAs for a dashboard that updates every second. Absolute precision is not required, but speed and concurrency are critical. Select the most appropriate SQL isolation level for this specific read transaction. (4 marks)

e) Transaction T1 executes `UPDATE Student SET sizeHS = 2000 WHERE sID = 123` and commits. Transaction T2, running under `READ COMMITTED`, reads `sizeHS` for `sID = 123` before T1 executes, and reads it again after T1 commits. Evaluate the values T2 sees on the first and second read, and name the anomaly this demonstrates. (4 marks)