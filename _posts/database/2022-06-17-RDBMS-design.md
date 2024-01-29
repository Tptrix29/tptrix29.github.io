---
title:  "RDBMS Design"
date:   2022-06-17 -0500
author: Pei Tian
categories: database 
tags: SQL RDBMS
header:
    teaser: /assets/img/training-guidence.png
---

Design Principle with Relational Database Management System

### Three Elements of Data Model

- Data Structure
- Data Operations
  - Relational Algebra
  - Relational Calculus
  - SQL
    - DDL (Data Definition Language)
    - DML (Data Manipulation Language)
    - DCL (Data Control Language)
- Integrity Constraints
  - Entity Integrity
  - Referential Integrity
  - User-Defined Integrity

### Basic Terms of Relational Model

- Relation (Table): Represents entities and their relationships in a two-dimensional table.

- Attribute (Column): A column in a table representing a property.

- Domain: The set of valid values for an attribute.

- Tuple (Row): A row in a table representing a record.

- Degree or Arity: The number of attributes in a relation.

- Cardinality: The number of tuples in a relation.

- Super Key: A set of one or more attributes that, taken collectively, uniquely identifies a tuple.

- Candidate Key:

   A minimal super key for uniquely identifying tuples.

  - Uniqueness: No two distinct tuples have the same combination of values for all candidate key attributes.
  - Irreducibility: No proper subset of the candidate key has the uniqueness property.

- Primary Key: A candidate key chosen to uniquely identify tuples in a relation.

- Foreign Key: An attribute in a relation that refers to the primary key in another relation.

- Three Types of Tables: Basic Relation, Query Table, View Table (Virtual Table)

### Relational Algebra Operators

1. Set Operators
   - Union (∪)
   - Intersection (∩)
   - Difference (-)
   - Cartesian Product (×)
2. Specialized Relation Operations
   - Selection (σ): Selects rows that satisfy a specified condition.
   - Projection (π): Selects specific columns from a relation.
   - Join (⨝): Combines rows from two or more relations based on a related column.
   - Rename (ρ): Renames a relation.
3. Comparison Operators
   - Not equal to (<>)
4. Logical Operators
   - AND (∧)
   - OR (∨)
   - NOT (∼)

### Database Architecture

- External Schema: Represents the logical structure and characteristics of data visible to database users.
- Schema: Represents the logical structure and characteristics of all the data contained in the database.
- Internal Schema: Describes the physical storage structure of the database.

> External Schema/Schema Mapping: Ensures logical data independence.
>
> The correspondence between external schemas and the schema. When the schema changes (such as adding new relations, new attributes, changing the data type of attributes, etc.), the database administrator makes corresponding changes to each external schema/schema mapping, keeping the external schema unchanged. Since applications are written based on external schemas, applications do not need to be changed, ensuring logical data independence.

> Schema/Internal Schema Mapping: Ensures physical data independence.
>
> The correspondence between the global logical structure of the database and the storage structure. When the storage structure of the database changes (such as using another storage structure), the database administrator makes corresponding changes to the schema/internal schema mapping, keeping the schema unchanged. Since applications are written based on the schema, applications do not need to be changed, ensuring physical data independence.

Three-Level Schema, Two-Level Mapping

------

### Designing a Database

### Conceptual Schema

Entity-Relationship (ER) Diagram

Data Relationships:

- 1:1
- 1:N
- M:N

### Logical Schema

Conversion from ER Diagram to Relational Schema, Normalization, `CREATE TABLE`

Normal Forms:

- 1NF (First Normal Form): No repeating groups; each attribute value is atomic.
- 2NF (Second Normal Form): No partial dependencies on the primary key.
- 3NF (Third Normal Form): No transitive dependencies on the primary key.
- BCNF (Boyce-Codd Normal Form): A stronger form of 3NF.

### Physical Schema

Implementation of the database system



### Transact-SQL

1. Local Variables

   ```sql
   DECLARE @score INT, @name VARCHAR(20) #
   SET @score = 88
   SELECT @score = 90, @name = 'user'
   ```

2. Global Variables: Specific to the SQL Server system

   ```sql
   SELECT @@TOTAL_READ AS 'Reads'
   ```

3. Operators

   ```sql
   -- Date
   SELECT STR(YEAR(GETDATE()))
   -- Other DB
   OPENDATASOURCE('SQLOLEDB', 'DataSource=(local); Integrated Security = SSPI;').university.dbo.student
   -- Excel
   OPENDATASOURCE('Microsoft.ACE.OLEDB.12.0', 'Data Source="..."; User ID=Admin; Password=; Extended properties=Excel 12.0')...[Sheet2$]
   ```

4. User-Defined Functions

   - Scalar Function

     ```sql
     -- Definition
     CREATE FUNCTION get_avg_score(@snum CHAR(4))
     RETURNS INT
     AS
     BEGIN
       DECLARE @temp INT
       SELECT @temp = AVG(score) FROM sc WHERE snum = @snum
       RETURN @temp
     END
     
     -- Operation
     SELECT dbo.get_avg_score('s001')
     ```

   - Inline Table-Valued Function

     ```sql
     -- Definition
     CREATE FUNCTION get_avg_ge(@score INT)
     RETURNS table
     RETURN
     BEGIN
       SELECT snum, AVG(score) AS average FROM sc
       GROUP BY snum
       HAVING AVG(score) >= @score
     END
     
     -- Operation
     SELECT * FROM get_avg_ge(80)
     ```

   - Multi-Statement Table-Valued Function

     ```sql
     -- Definition
     CREATE FUNCTION get_sc()
     RETURNS @sc
     TABLE(
       snum CHAR(4),
       cnum CHAR(3),
       score INT
     )
     AS
     BEGIN
       INSERT INTO @sc
       SELECT snum, cnum, score
       FROM sc, sections
       WHERE sc.secnum = sections.secnum
       RETURN
     END
     
     -- Operation
     SELECT * FROM get_sc()
     ```

   - Function Operations

     ```sql
     DROP FUNCTION dbo.get_sc  # Delete
     ALTER FUNCTION dbo.get_sc  # Modify
     ```

5. Flow Control

   ```sql
   -- IF ELSE
   IF () *; ELSE *;
   
   -- WHILE
   WHILE () *; BREAK; CONTINUE;
   
   -- Wait
   WAITFOR TIME '09:00:00'
   WAITFOR DELAY '00:30:00'
   
   -- CASE
   SELECT RANK =
     CASE score
       WHEN 7 THEN 'GOOD'
       WHEN 10 THEN 'PERFECT'
       ELSE 'SORRY'
   -- SELECT Count with Conditions
   SUM(CASE WHEN s > 90 1 WHEN s < 80 2 ELSE 0)
   
   --
   ```

### Common Database Objects

1. View

   ```sql
   CREATE VIEW V(id, name, desp)
   AS
   	/* ... */
   ```

   ❗️❗️ DML operations on views can only affect one base table!

2. Stored Procedure

   Characteristics: Reduce network traffic, code reuse, speed up system execution, improve data security

   Types:

   - System stored procedures
   - User-defined procedures
   - Extended stored procedures
   - Remote stored procedures

   ```sql
   -- No Params
   CREATE PROC proc1
   AS
   	SELECT snum, AVG(score) FROM sc GROUP BY snum
   -- Running Test
   EXEC proc1
   
   -- Input Params
   CREATE PROC proc2
   @snum CHAR(4)
   AS
   	SELECT snum, AVG(score) FROM sc
   	WHERE snum = @snum
   -- Running Test
   DECLARE @temp CHAR(4)
   SET @temp = 's002'
   EXEC proc2 @temp
   
   -- Input & Output Params
   CREATE PROC proc3
   @snum CHAR(4),
   @avg INT OUTPUT
   AS
   	SELECT @avg = AVG(score) FROM sc WHERE snum = @snum
   -- Running Test
   DECLARE @temp CHAR(4), @avg_out INT
   SET @temp = 's001'
   EXEC proc3 @temp, @avg_out OUT
   PRINT 's001''s Average Score: ' + CAST(@avg_out AS CHAR(4))
   
   -- Return Status
   CREATE PROC sc_proc4
   @snum CHAR(10)=NULL
   AS
    	IF @snum is NULL
        	RETURN 15
    	ELSE
      		IF EXISTS (SELECT * FROM sc WHERE snum=@snum AND score<60)
            	RETURN 5
      		ELSE
      		BEGIN
                SELECT *
                FROM sc
                WHERE snum=@snum
                RETURN 0
            END
   -- Running Test
   DECLARE @return_status INT
   EXEC @return_status=sc_proc4 's001'
   IF @return_status=15
   	PRINT 'Missing input parameter!'
   IF @return_status=5
      	PRINT 'This student has failing records!'
   ```

   Source code location: `syscomments`

   Registration: `sysobjects`

   View custom stored procedures: `sp_helptext proc`

   System stored procedures: e.g., `sp_denylogin`

3. Trigger

   Code:

   ```sql
   CREATE TRIGGER TG1
   ON [table]|[view]
   FOR|AFTER|INSTEAD OF [INSERT][,][DELETE][,][UPDATE]
   AS
   	/* ... */
   	-- Rollback mechanism
   	ROLLBACK
   	-- Confirm modification
   	COMMIT
   ```

   Purposes:

   - Enforce Restriction
   - Auditing Changes
   - Cascaded Operation
   - Invocation of Stored Procedure

   Components: Event, Condition, Action

### Database Management

### Data Query

Relational algebra, SQL language

### Database Protection Techniques

- Integrity Control

  - Entity Integrity
  - Referential Integrity
  - Domain Integrity
  - User-defined Integrity

  > CREATE TABLE, Trigger, Rule

- Security Control

  - User login passwords
  - Views: Can only update one base table
  - Access control
  - Roles

- Concurrency Control [Blog](https://blog.csdn.net/liyunyou/article/details/82806915?spm=1001.2101.3001.6650.4&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-4-82806915-blog-109963032.pc_relevant_antiscanv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-4-82806915-blog-109963032.pc_relevant_antiscanv3&utm_relevant_index=6)

  - Three abnormal states: lost data, non-repeatable read, read dirty data

  - ACID properties of transactions:

    - Atomicity: All operations in a transaction must either complete or leave the database unchanged. If an error occurs during the transaction, it is rolled back to the state before the transaction started.
    - Consistency: The database remains in a consistent state before and after the transaction. This ensures that data meets all predetermined rules, including accuracy, coherence, and the ability to perform predetermined tasks spontaneously.
    - Isolation: The database allows multiple concurrent transactions to read, write, and modify its data. Isolation prevents data inconsistency caused by concurrent execution of multiple transactions. Isolation has different levels, including Read Uncommitted, Read Committed, Repeatable Read, and Serializable.
    - Durability: After a transaction is completed, the changes to the data are permanent. Even in the event of a system failure, the changes are not lost.

  - Lock Mechanism

  - 2PL, Two-Phase Locking Protocol: All transactions must be divided into two stages for locking and unlocking

    The first stage is acquiring locks, also known as the expansion stage; the second stage is releasing locks, also known as the contraction stage

- Database Maintenance

  Backup

- Common Techniques

  - Stored Procedures: No parameters, input parameters, output parameters

  - Triggers: insert, delete, update, instead of

    `COMMIT`, `ROLLBACK` != `RETURN`