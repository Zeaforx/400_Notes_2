# DATA MANAGEMENT II (CSC 404)

## Question 1

**Q: In terms of data management, write short notes on the following: Information management**

A: Information management is the process of collecting, storing, organizing, maintaining, and delivering information within an organization. You use it to manage information as a strategic asset, ensuring data is accessible for decision-making and operational efficiency.

**Q: Mention the five key areas of information management.**

A: The five key areas are Information Governance, Information Architecture, Information Storage and Retrieval, Information Security, and Information Distribution and Sharing.

**Q: Define an Information System.**

A: An Information System is a formally integrated set of components designed to collect, store, process, and distribute information to support decision-making and control in an organization. A real-life example is a university portal where you register for courses, pay fees, and check grades.

**Q: Mention any two (2) purposes of information capture, according to ISO 15489, the global standard for records management.**

A: Two purposes are to ensure accountability and provide evidence of business transactions, and to facilitate the retrieval and reuse of information for ongoing business operations.

**Q: Using a pyramid, list the types of information systems and group them according to the levels of information systems.**

A:

- Top Level (Strategic): Executive Information Systems (EIS) used by senior executives for long-term planning.
    
- Middle Level (Tactical): Management Information Systems (MIS) and Decision Support Systems (DSS) used by middle managers to optimize current operations.
    
- Bottom Level (Operational): Transaction Processing Systems (TPS) used by frontline staff to process day-to-day transactions.
    

**Q: Consider the document type descriptor (DTD) for a book store XML in Appendix A. What are the categories of items sold in the book store?**

A: The store sells Books and Magazines.

**Q: Must the book store have those major elements every time?**

A: No. The `*` in `(Book|Magazine)*` means the bookstore can contain zero or more of these elements.

**Q: What is the maximum and minimum number of books the bookstore can have?**

A: The minimum is 0, and the maximum is unbounded.

**Q: What is the maximum and minimum number of magazines the bookstore can have?**

A: The minimum is 0, and the maximum is unbounded.

**Q: Mention the optional and mandatory attributes of a book.**

A: Mandatory attributes are `ISBN`, `Price`, and `Edition`. There are no optional attributes defined for a book.

**Q: If a book does not have an edition, will the XML document validate?**

A: The XML document will not validate because the `Edition` attribute is marked as `#REQUIRED`.

**Q: What is the minimum and maximum number of authors of a magazine?**

A: Both minimum and maximum are 0. The DTD specifies that a Magazine only contains a `Title`.

**Q: If the year of publication of a magazine is not stated, will the XML document validate?**

A: The XML document will not validate because the `Year` attribute is marked as `#REQUIRED`.

**Q: Considering the XML document as a tree, mention one element which is a leaf node.**

A: `Title`, `First_Name`, `Last_Name`, and `Remark` are leaf nodes because they only contain parsed character data (`#PCDATA`).

**Q: Rewrite the SQL query below in relational algebra: `select sID, sName from Student where GPA>3.7`**

A: $\pi_{sID, sName}(\sigma_{GPA > 3.7}(Student))$  

**Q: What is Data Independence of a Database Management System?**

A: Data Independence is the ability of a database system to allow you to change the schema at one level without modifying the schema at the next higher level. A practical example is changing the physical hard drive storage format without needing to rewrite your SQL queries.

## Question 2

**Q: Mention any six (6) problems of a file processing system.**

A:

1. Data redundancy and inconsistency.
    
2. Difficulty in accessing specific data.
    
3. Data isolation across separate files.
    
4. Integrity and validation problems.
    
5. Difficulty achieving atomicity during updates.
    
6. Concurrent access anomalies.
    

**Q: Mention the type of inconsistency that is occurring in each of the following pairs of concurrent statements issued by a client in the figures below: figure b (i), figure b (ii), figure b (iii)**

A:

- Figure b(i) shows a Lost Update (Write-Write Conflict). Both transactions update the exact same enrollment value concurrently, causing one write to overwrite the other.
    
- Figure b(ii) shows an Unrepeatable Read (or Write-Write Conflict on the same record). Two transactions blindly update different fields of the exact same student application row concurrently.
    
- Figure b(iii) shows a Phantom Read Problem. One transaction selects a set of rows based on a condition ($GPA > 3.9$), while another transaction concurrently modifies the data satisfying that condition.
    

**Q: What is a transaction? Explain how it is related to concurrency and system failures.**

A: A transaction is a logical unit of work consisting of one or more database operations executed as a single entity. It relates to concurrency by isolating multiple executing transactions so they do not interfere with each other. It handles system failures by guaranteeing atomicity. If a failure occurs mid-transaction, the database rolls back to prevent partial updates.

**Q: Mention the four (4) properties of transactions.**

A: Atomicity, Consistency, Isolation, and Durability (ACID).

## Question 3

**Q: Consider the relation with the following functional dependencies: Student (SSN, sName, address, HScode, HScity, GPA, priority); HScode -> HSname, HScity; GPA -> priority; SSN -> sName, address, GPA. Find {SSN, HScode}+.**

A: Start with {SSN, HScode}. Use `SSN -> sName, address, GPA` to add those attributes. Use `GPA -> priority` to add priority. Use `HScode -> HSname, HScity` to add those attributes. The result is {SSN, HScode, sName, address, GPA, priority, HSname, HScity}.

**Q: What is the key of the relation?**

A: The candidate key is `{SSN, HScode}` because its closure covers all attributes in the relation.

**Q: Mention all the functional dependencies that violate Boyce-Codd Normal Form (BCNF).**

A: All three provided FDs violate BCNF because none of their left-hand sides (`HScode`, `GPA`, `SSN`) serve as superkeys for the entire relation.

**Q: Decompose the relation into a schema that is in Boyce Codd Normal Form (BCNF) based on the three functional dependencies. Decompose based on the order below: HScode -> HSname, HScity; GPA -> priority; SSN -> sName, address, GPA.**

A:

1. Apply `HScode -> HSname, HScity`. Create relation **R1(HScode, HSname, HScity)**. The remaining attributes are {SSN, sName, address, HScode, GPA, priority}.
    
2. Apply `GPA -> priority`. Create relation **R2(GPA, priority)**. The remaining attributes are {SSN, sName, address, HScode, GPA}.
    
3. Apply `SSN -> sName, address, GPA`. Create relation **R3(SSN, sName, address, GPA)**. The remaining attributes are {SSN, HScode}.
    
4. The final relation is **R4(SSN, HScode)**.
    

## Question 4

**Q: Mention any two (2) of the three commonly used translation schemes used in translating subclass relationships to relations.**

A:

1. Create a single relation containing both the superclass and all subclass attributes, using a type indicator column.
    
2. Create separate relations for the superclass and each individual subclass.
    

**Q: Consider a scenario that involves Students who are either Foreign students or Domestic students. Also, some of the students are Advanced Placement students also known as AP students so, these AP students take AP courses. Represent this scenario in a UML diagram, showing the subclass relationships, association and association class.**

A:

- Superclass: `Student` (Attributes: name, matric_number).
    
- Subclasses of Student: `Foreign_Student` (Attribute: country) and `Domestic_Student` (Attributes: state, NIN).
    
- Association: `Takes` connects `AP_Student` (a subclass of Student) to `AP_Course` (Attributes: course_title, course_code).
    
- Association Class: `Enrollment` attached to the `Takes` association (Attributes: year, grade).
    

**Q: Use the translation scheme defined as “Subclass relations contain superclass key + specialized attribute” to translate the UML in (i) above to relations.**

A:

- Foreign_Student(matric_number, country)
    
- Domestic_Student(matric_number, state, NIN)
    
- AP_Student(matric_number)
    

## Question 5

**Q: List four (4) reasons why we use integrity constraints.**

A:

1. To guarantee the accuracy and reliability of stored data.
    
2. To enforce specific business rules automatically.
    
3. To prevent invalid or anomalous data entry.
    
4. To maintain relationships and consistency across multiple tables.
    

**Q: Suppose you are using an SQLite database. Create a trigger R1 that will intercept insertions into the Student table and will check the GPA. If the GPA of the inserted student is > 3.3 or <= 3.6, that student will be automatically applying to Stanford for a geology major and applying to MIT for a biology major.**

A:

```
CREATE TRIGGER R1
AFTER INSERT ON Student
FOR EACH ROW
WHEN NEW.GPA > 3.3 OR NEW.GPA <= 3.6
BEGIN
  INSERT INTO Apply (sID, cName, major) VALUES (NEW.sID, 'Stanford', 'geology');
  INSERT INTO Apply (sID, cName, major) VALUES (NEW.sID, 'MIT', 'biology');
END;
```

**Q: Consider the relation below, which has the multivalued dependency that: NIN↠hobby. Derive the remaining tuples based on the multivalued dependency. NIN,hobby,major; 123,piano,CS; 123,boxing,EE.**

A: Given the dependency $NIN \twoheadrightarrow hobby$ and existing tuples (123, piano, CS) and (123, boxing, EE). The derived tuples are 123, piano, EE and 123, boxing, CS.

**Q: There are two types of normal forms. Mention them and also mention the respective properties of a relation that guarantees each of the two normal forms.**

A:

- Third Normal Form (3NF): A relation is in 3NF if it is already in 2NF and has no transitive dependencies.
    
- Boyce Codd Normal Form (BCNF): A relation is in BCNF if for every non-trivial functional dependency $X \rightarrow Y$, $X$ is a superkey.
    

## Question 6

**Q: List the two (2) underlying data structures of an index?**

A: B+ Trees and Hash Tables.

**Q: Mention the conditions that each of the underlying data structures of an index which you mentioned above can be used.**

A: B+ Trees are best used for range queries and sequential data retrieval. Hash Tables are best used for strict equality tests and exact match queries.

**Q: Suppose we have a user that we want to authorize to access i.e., read information in the Student relation, but only for students who have applied to AFIT. Mention how we achieve such an authorization.**

A: You achieve this authorization by creating a View that limits the rows to only AFIT applicants, and then granting access specifically to that View rather than the base table.

**Q: Then show how we can authorize such a user for that specific privilege.**

A:

```
CREATE VIEW AFIT_Applicants AS
SELECT Student.* FROM Student
JOIN Apply ON Student.sID = Apply.sID
WHERE Apply.cName = 'AFIT';

GRANT SELECT ON AFIT_Applicants TO UserName;
```

**Q: Then state the required privilege.**

A: The required privilege is `SELECT`.

## Question 7

**Q: What is a ‘dirty” data item in a database?**

A: A dirty data item is a piece of data that has been modified in memory by a transaction but has not yet been committed to the database.

**Q: Mention and describe each of the three levels of data abstraction in a database management system.**

A:

- Physical Level: The lowest tier that details exactly how data records are stored in memory and on disk.
    
- Logical Level: The middle tier that defines what data is kept in the database and the structural relationships between that data.
    
- View Level: The highest tier that selectively exposes parts of the database to simplify interactions and enforce security for different users.
    

**Q: In a tabular form, show the comparison between a relational model and XML. The comparison should be based on structure, schema, queries, ordering and implementation. (You can use a word or phrase to describe each of the three models.)**

A:

|   |   |   |
|---|---|---|
|**Feature**|**Relational Model**|**XML**|
|**Structure**|Flat Tables|Hierarchical Trees|
|**Schema**|Rigid/Fixed|Flexible/Self-describing|
|**Queries**|SQL|XQuery/XPath|
|**Ordering**|Unordered sets|Ordered elements|
|**Implementation**|Relational Database Engine|Document Object Model (DOM)|

**Q: Use expression trees [in relational algebra] to display the list of GPAs of students applying to Computer Science in Kaduna.**

A: $\pi_{GPA}(\sigma_{major='Computer Science' \land state='Kaduna'}(Student \bowtie Apply \bowtie College))$

```
                    π_GPA
                      |
                    ⋈ (Natural Join on sID)
                   /                  \
              Student              ⋈ (Natural Join on cName)
                                  /                  \
                         σ_major='CS'            σ_state='Kaduna'
                              |                       |
                            Apply                  College
```