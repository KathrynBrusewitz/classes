Info 445
===

__1. Explain the concept of error-handling (try and be complete: briefly explain what is it, when it happens, why it happens and how it happens). Touch on the benefits of error-handling as well.__

We want to handle errors when we anticipate failures in order to improve performance. It is better to "fail early" than to "fail late". If we fail late, we affect other transactions and can cause a cascading rollback of many transactions. If we fail early, we avoid having to rollback hundreds/thousands of transactions. To help the client troubleshoot errors, we log out messages with the error to let them know what/why/where it failed.

__2. Normalization seeks to eliminate several different types of data anomalies; please identify what these data anomalies are and how normalization can eliminate them.__

Imagine an un-normalized Student Table with dorm information inside it (DormName and RoomStyle). Having such an un-normalized table can bring up the following anomalies:

Insertion anomaly: We can't insert a new dorm without a student associated with that dorm. We may have a brand new dorm without any students living in it yet.

Deletion anomaly: We can't delete data that without deleting other data that we might want to keep. If we delete a student, we can lose information about a dorm if that student was the only student living in that dorm.

Update anomaly: If there was an error in RoomStyle (such as a misspelling or no longer offered or changed name), we may have to update hundreds of students and that would be a hassle.

The solution to avoid these anomalies would be to normalize Student table by creating a separate table (Dorm) for dorms and an associative entity (Student_Dorm) to keep history.

Normalization helps ensure there is no redundant data, which can cause the above anomalies.

__3. Describe the differences between Online Transaction Processing (OLTP) databases and those that are supporting Data Warehousing or Online Analytical Processing (OLAP).__

OLTP:
- High number of short online transactions (INSERT UPDATE DELETE)
- Very fast query processing
- Effectiveness is measured by number of transactions per second
- Detailed and current data

OLAP:
- Low number of transactions
- Queries are often complex and may be thousands/millions of rows
- Effectiveness is measured by response time
- Aggregated, historical data

__4. Explain the difference between synchronous and asynchronous data transfer; when are each preferred?__

Synchronous
- Communications are serial
- Transactions wait for commit confirmation
- Care about data loss

Asynchronous
- Communications are parallel
- Transactions do not wait for commit confirmation
- Speed is more important than accuracy

__5. Describe 5 different SQL commands that are considered ‘control of flow’ language.__

BEGIN...END
- BEGIN and END define a series of statements that execute together.

BREAK
- Exits the innermost loop in a WHILE statement or an IF...ELSE statement inside a WHILE loop.

GOTO label
- The statements after GOTO are skipped and processing continues at the label.

CONTINUE
- Restarts a WHILE loop. Any statements after the CONTINUE keyword are ignored.

IF...ELSE
- Imposes conditions on the execution of a statement block. If the condition is true, statements following IF... will be executed. If the condition is false, statements following the ELSE will be executed.

WHILE
- Sets a condition for the repeated execution of a statement block. The statements are executed repeatedly as long as the condition is true.

RETURN
- Exits unconditionally from a query or procedure. RETURN is immediate and can be used at any point to exit from a procedure, batch, or statement block. Statements that follow RETURN are not executed.

WAITFOR
- Blocks the execution of a batch, stored procedure, or transaction until a specified time or time interval is reached, or a specified statement modifies or returns at least one row.

THROW
- Raises an exception.

TRY...CATCH
- Implements error handling similar to C# and C++. If an error occurs in the TRY block, control is passed to another group of statements that is enclosed in a CATCH block.

__6. Compare database mirroring, log shipping and replication; when is each the preferred tool of use?__

Database Mirroring allows a database to mirror its data to another SQL Server thus providing redundancy. Best use of database mirroring is for databases that are considered mission-critical or otherwise cannot sustain significant downtime. Mirroring is synchronous; does not filter like replication.

Database Mirroring Pros:
- Mirroring can provide extremely High Availability
- Automatic failover in case primary server crashes = no downtime
- Flexibility in configuration
- Provides redundancy at the database level

Database Mirroring Cons:
- Synchronous transactions can impact system performance
- Have to create snapshots of the mirror database in order to get read access for reporting
- Only committed transactions are transferred.

Log Shipping allows a database to automatically send transaction log backups from a primary database on a primary server instance to one or more secondary databases on separate secondary server instances. Not filtered; exact copy of data. Read activity = shared lock. Write activity = exclusive lock (this will prevent all read activity. Use log shipping to create a copy so read activity could occur while write activity occurs on the other database.

Log Shipping Pros:
- Provides a disaster-recovery solution for a single primary database and one or more secondary databases
- Provides additional redundancy to your backup strategy
- Can use the secondary database for reporting purposes
- Data can be copied on more than one location
- All committed and un-committed transactions are transferred

Log Shipping Cons:
- Not used for high availability
- Does not automatically failover from the primary server to the secondary server
- Secondary database must be brought online manually
- Need to manage all the databases separately
- Secondary database isn't fully readable while the restore process is running

Replication tracks and detects changes and ships the changes. Use replication to filter data on geographic data (send only data relevant to your location). Or, certain locations receive specific information based on their job function.

Replication Pros:
- Limit specific objects that are replicated to the subscriber. This can be useful if only a subset of tables is needed.
- Gives ability to scale up.

Replication Cons:
- Replication is bound to database objects, so if you create a new object on the database it is not automatically added.

__7. Describe the use and benefits of an output parameter.__

A stored procedure could return one or more output parameters. These are parameters that the stored procedure uses to return data back to the calling application so that it can save the value and use it. The benefit is being able to nest stored procedures within another stored procedure.

__8. Define the different data warehouse design structures:  star schema, snowflake schema, ‘star flake’ schema, fact table, dimension table in addition to a ‘measure’.__

Star Schema: A central table (Fact Table) contains fact data. Multiple tables (Dimension Tables) radiate out from it. They are connected by the primary and foreign keys of the database.

![Star Schema](https://www.tutorialspoint.com/dwh/images/start_schema.jpg)

Snowflake Schema: Like the star schema, but is further normalized into sub-dimension tables.

![Snowflake Schema](https://www.tutorialspoint.com/dwh/images/snowflake.jpg)

Star Flake Schema: Like the snowflake schema, but only some dimension tables have been denormalized.

Fact Table: Contains fact data, which are measurable, quantitative data. Contains foreign keys of its dimensions.

Dimension table: Contains a set of attributes, which are descriptive attributes related to fact data. Connects with the Fact Table through its primary key.

Measure: Fact tables record measurements for a specific event. A measure is something that can be used in calculations and analysis.

__9. Explain the differences between a Data Warehouse, ETL and a Data Mart.__

Data Warehouse:
- Holds multiple subject areas
- Holds very detailed information
- Works to integrate all data sources
- Does not necessarily use a dimensional model but feeds dimensional models

Data Mart:
- Often holds only one subject area - for example, finance or sales
- May hold more summarised data
- Concentrates on integrating information from a given subject area or set of source systems
- Is built focused on a dimensional model using a star schema

ETL (Extract, Transform, Load) is the process of getting data from one data storage method into another. ETL usually depends on data pipelines provided by third parties. Data Warehouse/Data Mart is the place where data is stored for analysis/reporting.

__10. Explain the difference between the concepts of ‘high-availability’ and ‘scalability’ in regards to relational database systems: What are the terms and tools are used? How do we measure their effectiveness?__

Clustering for scalability versus clustering for high availability (HA). HA means that the application will be available without interruption.

Scalability:
- ability of a system to provide throughput limited only by available hardware resources.
- a scalable system is one that can handle increasing numbers of requests without adversely affecting response time and throughput
- vertical scaling: increasing cpu within one operating environment; adding hardware to a single node; upgrading to a larger node
- horizontal scaling: leveraging multiple systems to work together in parallel; adding more nodes; spreading workload among nodes
- "Throughput": Transactions per second

High Availability:
- availability is defined as the percentage of time that it works normally
- Unfortunately, measuring availability is not as simple as selecting one of the availability percentages. You must first decide what metric you want to use to qualify downtime. For example, one organization may consider downtime to occur when one database is not mounted. Another organization may consider downtime to occur only when more than half of its users are affected by an outage.
- Goal is to provide continuous use of mission-critical data
- There are degrees of availability (it's NOT all or nothing)
  - 99.9% = 8.7 hours of downtime/year
- Tool: Database Snapshots - Quick and efficient; only modified data gets moved

__11. Discuss the mechanisms employed by database management systems to ensure recoverability for all transactions that may be interrupted during processing.__

Transaction processing links multiple individual operations into a single transaction and ensures that either all operations in a transaction are completed without error, or none of them are. If some of the operations are completed but errors occur when the others are attempted, the transaction-processing system "rolls back" ALL of the operations in the transaction (including the successful ones). This restores the system to the consistent state that it was in before processing of the transaction began.

Transaction processing guards against hardware and software errors that might leave a transaction partially completed. If the computer system crashes in the middle of a transaction, the system guarantees that all operations in any uncommitted transactions are cancelled.

Transaction Log: write-ahead logging
- Any insert/update/delete is written to transaction-log in memory first
- Allows the system to rollback transactions if processing fails
- Allows the system to roll-forward transactions that have committed in memory
- "BEGIN TRAN" "COMMIT TRAN"

Checkpoint:
- Every couple minutes there is a checkpoint
- Batch of active transactions are logged
- Stop all processing in a database and flush active portion of log to disk
- In case of failure, small batch allows for quick writing to disk

Explicit Transaction

Synchronous data transfer/2-phase commit
- Makes sure both databases are always identical

__Create at least one stored procedure that takes in several parameters of friendly names and INSERTs into multiple tables in an explicit transaction with proper error-handling__
```sql
CREATE PROCEDURE uspNewSpecialNeedPerson
@Fname VARCHAR (30)
@Lname VARCHAR (30)
@GenderName VARCHAR (30)
@AreaCode INT
@PhoneNum INT
@EthnicityName VARCHAR (30)
@SpecialNeedName VARCHAR (60)

-- Declare and initiate IDs for transaction(s)
AS
DECLARE @PersonID INT
DECLARE @GenderID INT
DECLARE @PhoneID INT
DECLARE @EthnicityID INT
DECLARE @SpecialNeedID INT

SET @GenderID = (SELECT GenderID FROM GENDER WHERE GenderName = @GenderName)
SET @EthnicityID = (SELECT EthnicityID FROM ETHNICITY WHERE EthnicityName = @EthnicityName)
SET @SpecialNeedID = SELECT SpecialNeedID FROM SPECIAL_NEED WHERE SpecialNeedName = @SpecialNeedName)

BEGIN Transaction
  -- Insert a new row into PHONE
  INSERT INTO PHONE (AreaCode, PhoneNum)
  VALUES (@AreaCode, @PhoneNum)

  SET @PhoneID = (SELECT SCOPE_IDENTITY())

  -- Insert a new row into PERSON
  INSERT INTO PERSON (Fname, Lname, EthnicityID, GenderID, PhoneID)
  VALUES (@Fname, @Lname, @EthnicityID, @GenderID, @PhoneID)

  SET @PersonID = (SELECT SCOPE_IDENTITY())

  -- Insert a new row into STUDENT_SPECIAL_NEED
  INSERT INTO STUDENT_SPECIAL_NEED (PersonID, SpecialNeedID)
  VALUES (@PersonID, @SpecialNeedID)

IF @@ERROR <> 0
  ROLLBACK Transaction
ELSE
  COMMIT Transaction
```

__Create at least one business rule or computed column leveraging a function__
```sql
-- Creates a computed column in DEPARTMENT that
-- tracks number of Classes in each Department

CREATE FUNCTION fnTotalClasses(@Department VARCHAR (40))
RETURNS INT
AS
BEGIN
DECLARE @numOfClasses INT

SET @numOfClasses = (SELECT COUNT(C.ClassID)
  FROM CLASS C
  JOIN COURSE CRS ON C.CourseID = CRS.CourseID
  JOIN DEPARTMENT D ON CRS.DeptID = D.DeptID
  WHERE D.DeptName = @Department
)

RETURN @numOfClasses
END

ALTER TABLE DEPARTMENT
ADD TotalClasses INT
AS (dbo.fnTotalClasses(DepartmentName))
```

```sql
-- Creates a business rule that only classes from the "Information School"
-- are allowed to be in the building "Mary Gates Hall"

CREATE FUNCTION ckOnlyInfoClassesInMGH()
RETURNS INT
AS
BEGIN

DECLARE @RET INT = 0
IF EXISTS (
  SELECT *
  FROM COLLEGE CLLG
  JOIN DEPARTMENT D ON CLLG.CollegeID = D.CollegeID
  JOIN COURSE CRS ON D.DeptID = CRS.DeptID
  JOIN CLASS C ON CRS.CourseID = C.CourseID
  JOIN ROOM R ON C.RoomID = R.RoomID
  JOIN BUILDING B ON R.BuildingID = B.BuildingID
  WHERE CLLG.CollegeName != "Information School"
  AND B.BuildingName = "Mary Gates Hall"
)
SET @RET = 1
RETURN @RET
END

ALTER TABLE BUILDING
ADD CONSTRAINT ckOnlyInfoClassesInMGH
CHECK (dbo.ckOnlyInfoClassesInMGH() = 0)
```

__Create at least one stored procedure that calls additional stored procedures (‘nested’ stored procedures) leveraging OUTPUT parameters__
```sql
-- Create a nested stored procedure for adding a new PERSON_CONTACT

-- Stored procedure to get Person1ID:
CREATE PROCEDURE uspGetPerson1ID
@Fname VARCHAR (60)
@Lname VARCHAR (60)
@DateOfBirth DATE
@PersonID INT OUTPUT
AS
SET @PersonID = (
    SELECT PersonID FROM PERSON
    WHERE Fname = @Fname AND Lname = @Lname AND DateOfBirth = @DateOfBirth
)
GO

-- Stored procedure to get Person2ID:
CREATE PROCEDURE uspGetPerson2ID
@Fname VARCHAR (60)
@Lname VARCHAR (60)
@DateOfBirth DATE
@PersonID INT OUTPUT
AS
SET @PersonID = (
    SELECT PersonID FROM PERSON
    WHERE Fname = @Fname AND Lname = @Lname AND DateOfBirth = @DateOfBirth
)
GO

-- Stored procedure to get RelationshipID:
CREATE PROCEDURE uspGetRelationshipID
@RelationshipName VARCHAR (60)
@RelationshipID INT OUTPUT
AS
SET @RelationshipID = (
    SELECT RelationshipID FROM RELATIONSHIP
    WHERE RelationshipName = @RelationshipName
)
GO

-- Stored procedure to populate PERSON_CONTACT
CREATE PROCEDURE uspRegisterPersonContact
@Fname1 VARCHAR (60)
@Lname1 VARCHAR (60)
@DateOfBirth1 DATE
@Fname2 VARCHAR (60)
@Lname2 VARCHAR (60)
@DateOfBirth2 DATE
@RelationshipNameNew VARCHAR (60)
AS
DECLARE @Person1IDNew INT
DECLARE @Person2IDNew INT
DECLARE @RelationshipIDNew INT

EXECUTE uspGetPerson1ID
@Fname = @Fname1,
@Lname = @Lname1,
@DateOfBirth = @DateOfBirth1,
@PersonID = @Person1IDNew OUTPUT
IF @Person1IDNew IS NULL
    BEGIN
    PRINT 'PersonID cannot be NULL'
    RAISEERROR ('@PersonID is Null; Please check spelling.')
    RETURN
    END

EXECUTE uspGetPerson2ID
@Fname = @Fname2,
@Lname = @Lname2,
@DateOfBirth = @DateOfBirth2,
@PersonID = @Person2IDNew OUTPUT
IF @Person2IDNew IS NULL
    BEGIN
    PRINT 'PersonID cannot be NULL'
    RAISEERROR ('@PersonID is Null; Please check spelling.')
    RETURN
    END

EXECUTE uspGetRelationshipID
@RelationshipName = @RelationshipNameNew,
@RelationshipID = @RelationshipIDNew OUTPUT
IF @RelationshipIDNew IS NULL
    BEGIN
    PRINT 'RelationshipID cannot be NULL'
    RAISEERROR ('@RelationshipID is Null; Please check spelling.')
    RETURN
    END

BEGIN Transaction
    INSERT INTO PERSON_CONTACT (Person1ID, Person2ID, RelationshipID)
    VALUES (@Person1IDNew, @Person2IDNew, @RelationshipIDNew)
IF @@ERROR <> 0
    ROLLBACK Transaction
ELSE
    COMMIT Transaction
```

__Create at least one complex view (multiple JOINs, GROUP BY, HAVING, CASE)__
```sql
-- Determine the total number of classes for each department
-- that were offered from Fall Quarter 2010 until Fall Quarter 2016
-- and only include departments with less than 20 classes

SELECT D.DeptID, D.DeptName, Count(*)
FROM DEPARTMENT D
JOIN COURSE CRS ON D.DeptID = CRS.DeptID
JOIN CLASS C ON CRS.CourseID = C.CourseID
AND C.BeginDate BETWEEN '9/1/2010' AND '10/1/2016'
GROUP BY D.DeptID, D.DeptName
HAVING Count(*) < 20
```

![Part C ERD](http://i.imgur.com/hyyGK53.jpg)

Create the queries based on the following ERD:
---

![Final ERD](img/final-erd.png)


__1. Write the code to CREATE a stored procedure with the following:__  

- Takes in 6 parameters (FirstName, LastName, IncidentName, IncidentTypeName, IncidentDate, IncidentDescr)
- Inserts a new row into INCIDENT and INCIDENT_ CONTACT in a single explicit transaction


```sql
CREATE PROCEDURE uspNewIncident
@FirstName VARCHAR (30)
@LastName VARCHAR (30)
@IncidentName VARCHAR (30)
@IncidentTypeName VARCHAR (20)
@IncidentDate DATE
@IncidentDescr VARCHAR (500)

-- Declare and initiate IDs for transaction(s)
AS
DECLARE @IncidentID INT
DECLARE @ContactID INT
DECLARE @IncidentTypeID INT

SET @ContactID = (SELECT ContactID FROM CONTACT WHERE FirstName = @FirstName AND LastName = @LastName)
SET @IncidentTypeID = (SELECT IncidentTypeID FROM INCIDENT_TYPE WHERE IncidentTypeName = @IncidentTypeName)

BEGIN Transaction
  -- Insert a new row into INCIDENT
  INSERT INTO INCIDENT (IncidentName, IncidentDate, IncidentTypeID, IncidentDescr)
  VALUES (@IncidentName, @IncidentDate, @IncidentTypeID, @IncidentDescr)

  SET @IncidentID = (SELECT SCOPE_IDENTITY())

  -- Insert a new row into INCIDENT_CONTACT
  INSERT INTO INCIDENT_CONTACT (IncidentID, ContactID)
  VALUES (@IncidentID, @ContactID)

IF @@ERROR <> 0
  ROLLBACK Transaction
ELSE
  COMMIT Transaction
```

__2. Write the code to create a new entity called SCHOOL_TYPE:__  

- Include auto-increment feature
- Include code to establish a foreign key to SCHOOL

```sql
CREATE TABLE SCHOOL_TYPE
(
  SchoolTypeID INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
  SchoolTypeName VARCHAR (40) NULL,
  SchoolTypeDescr VARCHAR (500) NULL,
  SchoolID INT FOREIGN KEY REFERENCES SCHOOL(SchoolID)
)
```



__3. Write the code to create a computed column in SCHOOL that has the following:__  

- Includes the use of a user-defined function
- Tracks number of ‘student incidents’ (number of incidents that have involved students from each particular school)


```sql
CREATE FUNCTION fnTotalStudentIncidents(@School VARCHAR (40))
RETURNS INT
AS
BEGIN
DECLARE @numOfIncidents INT

SELECT @numOfIncidents = COUNT(I.IncidentID)
FROM INCIDENT I
JOIN INCIDENT_CONTACT IC ON I.IncidentID = IC.IncidentID
JOIN CONTACT C ON IC.ContactID = C.ContactID
JOIN STUDENT S ON C.ContactID = S.ContactID
JOIN CONTACT_SCHOOL CS ON C.ContactID = CS.ContactID
JOIN SCHOOL SC ON CS.SchoolID = SC.SchoolID
WHERE SC.SchoolName = @School

RETURN @numOfIncidents
END

ALTER TABLE SCHOOL
ADD TotalStudentIncidents INT
AS (dbo.fnTotalStudentIncidents(SchoolName))
```



__4. Write the code to create a business rule that restricts a person from serving more than two terms on the Board of Directors in the title of ‘President’:__  

```sql
CREATE FUNCTION ckPresHasLessThanThreeTerms()
RETURNS INT
AS
BEGIN

DECLARE @RET INT = 0
IF EXISTS (
  SELECT *
  FROM CONTACT C
  JOIN BOARD B ON C.ContactID = B.ContactID
  JOIN CONTACT_BOARD_POSITION CBP ON B.ContactID = CBP.ContactID
  JOIN BOARD_POSITION BP ON CBP.BoardPositionID = BP.BoardPositionID
  WHERE BP.BoardPositionName LIKE '%President%'
  AND DATEDIFF(year , CBP.BeginDate , CBP.EndDate) > 2
)

SET @RET = 1
RETURN @RET
END

ALTER TABLE CONTACT
ADD CONSTRAINT ckPresHasLessThanThreeTerms
CHECK (dbo.ckPresHasLessThanThreeTerms(Person) = 0)
```



__5. Write the code to create a business rule that restricts a fundraising events to December or January only:__  


```sql
CREATE FUNCTION ckFundraisingOnlyInDecemberJanuary()
RETURNS INT
AS
BEGIN
DECLARE @RET INT = 0
IF EXISTS (
  SELECT *
  FROM EVENT E
  JOIN EVENT_TYPE ET ON E.EventTypeID = ET.EventTypeID
  WHERE ET.EventTypeName LIKE '%fundrais%'
  AND DATEPART(mm, E.EventDate) NOT LIKE '1'
  AND DATEPART(mm, E.EventDate) NOT LIKE '12'
)

SET @RET = 1
RETURN @RET
END

ALTER TABLE EVENT
ADD CONSTRAINT ckFundraisingOnlyInDecemberJanuary
CHECK (dbo.ckFundraisingOnlyInDecemberJanuary() = 0)
```

Other
---
```sql
-- Stored procedure:
CREATE PROCEDURE uspGetStudentID
@Fname varchar(60),
@Lname varchar(60),
@DOB date,
@StudentID INT OUTPUT
AS
SET @StudentID = (SELECT StudentID FROM tblSTUDENT
		  WHERE StudentFname = @Fname
		  AND StudentLname = @Lname
		  AND StudentBirth = @DOB)
-- ^ lookup; originally declared as a variable but now its a parameter
GO

-- Stored procedure 2:
CREATE PROCEDURE uspGetClassID
@Year char(4),
@CourseName varchar(75),
@QuartName varchar(30),
@Section varchar(4),
@ClassID INT OUTPUT
AS
SET @ClassID = (SELECT ClassID FROM tblCLASS C
		JOIN tblCOURSE CR ON C.CourseID = CR.CourseID
		JOIN tblQUARTER Q ON C.QuarterID = Q.QuarterID
		WHERE Q.QuarterName = @QuartName
		AND C.Year = @Year
		AND C.Section = @Section)
GO

-- Stored procedure to populate CLASS_LIST
-- Eventually put a wrapper around
CREATE PROCEDURE uspRegisterStudent
@Fname1 varchar(60),
@Lname1 varchar(60),
@DOB1 date,
@RegDate date, -- helps populate CLASS_LIST
@Year1 char(4),
@CourseName1 varchar(75),
@QuartName1 varchar(30),
@Section1 varchar(4),
AS
DECLARE @StudentIDNew INT
DECLARE @ClassIDNew INT

EXECUTE uspGetStudentID
@Fname = @Fname1,
@Lname = @Lname1,
@DOB = @DOB1,
@StudentID = @StudentIDNew OUTPUT
IF @StudentIDNew IS NULL
	BEGIN
	PRINT 'StudentID cannot be NULL'
	RAISEERROR ('@StudentID is Null; Please check spelling.')
	RETURN
	END

EXECUTE uspGetStudentID
@Year = @Year1,
@CourseName = @CourseName1,
@QuartName = @QuartName1,
@Section = @Section1,
@ClassID = @ClassIDNew OUTPUT
IF @ClassIDNew IS NULL
	BEGIN
	PRINT 'ClassIDNew cannot be NULL'
	RAISEERROR ('@ClassIDNew is Null; Please check spelling.')
	RETURN
	END

BEGIN TRAN G2
INSERT INTO [dbo].[tblCLASS_LIST]
	       ([ClassID],   [StudentID],  [Grade], [RegistrationDate])
	VALUES (@ClassIDNew, @StudentIDNew, NULL,    @RegDate)
IF @@ERROR <> 0
	ROLLBACK TRAN G2
ELSE
	COMMIT TRAN G2

-- Synthetic transaction
CREATE PROCEDURE brukSynthetic_uspRegisterStudent
@Run INT -- Number we pass in that is number of times we want to loop and call stored procedure
AS
DECLARE @FirstN varchar(60)
DECLARE @LastN varchar(60)
DECLARE @BirthDate date
DECLARE @Registration date = (SELECT GetDate())
DECLARE @yr char(4) -- aligns with ClassYear
DECLARE @Course varchar(75)
DECLARE @Quarter varchar(30)
DECLARE @Section varchar(4)

DECLARE @Num numeric(16,16) -- holder of RANDOM number called each loop
DECLARE @StudentCount INT = (SELECT Count(*) FROM tblSTUDENT)
DECLARE @ClassCount INT = (SELECT Count(*) FROM tblCLASS)
DECLARE @StudentPK INT
DECLARE @ClassPK INT
WHILE @Run > 0
BEGIN
SET @Num = (SELECT RAND())
SET @StudentPK = (SELECT @NUM * @StudentCount)
SET @ClassPK = (SELECT @NUM * @ClassCount)
SET @FirstN = (SELECT StudentFname FROM tblSTUDENT WHERE StudentID = @StudentPK)
SET @LastN = (SELECT StudentLname FROM tblSTUDENT WHERE StudentID = @StudentPK)
SET @BirthDate = (SELECT StudentBirth FROM tblSTUDENT WHERE StudentID = @StudentPK)
SET @yr = (SELECT [Year] FROM tblCLASS WHERE ClassID = @ClassPK)
SET @Course = (SELECT CourseName FROM tblCOURSE C
		JOIN tblCLASS CS ON C.CourseID = CS.CourseID
		WHERE ClassID = @ClassPK)
SET @Quarter = (SELECT QuarterName FROM tblQUARTER Q
		JOIN tblCLASS CS ON CS.QuarterID = Q.QuarterID
		WHERE ClassID = @ClassPK)
SET @Section = (SELECT Section FROM tblCLASS WHERE ClassID = @ClassPK)

-- Execute outer stored procedure
EXECUTE uspRegisterStudent
@Fname1 = @FirstN,
@Lname1 = @LastN,
@DOB1 = BirthDate,
@RegDate = Registration,
@Year1 = @yr,
@CourseName1 = @Course,
@QuartName1 = @Quarter,
@Section1 = @Section
SET @Run = @Run - 1
END

-- Now ready to execute synthetic wrapper for testing
EXEC brukSynthetic_uspRegisterStudent 100
```

Gym DB
---
```sql
CREATE PROCEDURE [dbo].[uspGetQualificationID]
@QualificationName varchar(100),
@QualificationID INT OUTPUT
AS
SET @QualificationID = (
  SELECT QualificationID FROM QUALIFICATION
  WHERE QualificationName = @QualificationName)

CREATE PROCEDURE [dbo].[uspGetEmployeeID_NoDOB]
@EmployeeFname varchar(100),
@EmployeeLname varchar(200),
@EmployeeID INT OUTPUT
AS
SET @EmployeeID = (
  SELECT EmployeeID FROM EMPLOYEE
  WHERE EmployeeFname = @EmployeeFname
  AND EmployeeLname = @EmployeeLname)

CREATE PROCEDURE [dbo].[uspAddQualificationToEmployee]
@EmployeeFname1 varchar(100),
@EmployeeLname1 varchar(200),
@QualificationName1 varchar(100)
AS
DECLARE @EmployeeID1 INT
DECLARE @QualificationID1 INT

EXECUTE [dbo].[uspGetEmployeeID_NoDOB]
@EmployeeFname = @EmployeeFname1,
@EmployeeLname = @EmployeeLname1,
@EmployeeID = @EmployeeID1 OUTPUT

IF @EmployeeID1 IS NULL
BEGIN
  PRINT 'EmployeeID cannot be null'
  RAISERROR('EmployeeID is null, please check spelling', 12, 1)
  RETURN
END

EXECUTE [dbo].[uspGetQualificationID]
@QualificationName = @QualificationName1,
@QualificationID = @QualificationID1 OUTPUT

IF @QualificationID1 IS NULL
BEGIN
  PRINT 'QualificationID cannot be null'
  RAISERROR('QualificationID is null, please check spelling', 12, 1)
  RETURN
END

BEGIN TRAN
  INSERT INTO EMPLOYEE_QUALIFICATION(EmployeeID, QualificationID)
  VALUES(@EmployeeID1, @QualificationID1)
  IF @@ERROR <> 0
    ROLLBACK TRAN
  ELSE
    COMMIT TRAN

CREATE PROCEDURE [dbo].[bruk_synthetic_uspAddQualificationToEmployee]
@Run INT
AS
DECLARE @EmployeeFirstName varchar(100)
DECLARE @EmployeeLastName varchar(200)
DECLARE @QualificationName varchar(100)
DECLARE @Num NUMERIC(16, 16)
DECLARE @EmployeeCount INT = (SELECT Count(*) FROM EMPLOYEE)
DECLARE @QualificationCount INT = (SELECT Count(*) FROM QUALIFICATION)
DECLARE @EmployeePK INT
DECLARE @QualificationPK INT
WHILE @Run > 0
BEGIN
  SET @Num = (SELECT RAND())
  SET @EmployeePK = (SELECT @Num * @EmployeeCount)
  SET @QualificationPK = (SELECT @Num * @QualificationCount)

  IF (@EmployeePK = 0)
  BEGIN
    SET @EmployeePK = 1
  END

  IF (@QualificationPK = 0)
  BEGIN
    SET @QualificationPK = 1
  END

  SET @EmployeeFirstName = (SELECT EmployeeFname FROM EMPLOYEE WHERE EmployeeID = @EmployeePK)
  SET @EmployeeLastName = (SELECT EmployeeLname FROM EMPLOYEE WHERE EmployeeID = @EmployeePK)
  SET @QualificationName = (SELECT QualificationName FROM QUALIFICATION WHERE QualificationID = @QualificationPK)

  EXECUTE uspAddQualificationToEmployee
  @EmployeeFname1 = @EmployeeFirstName,
  @EmployeeLname1 = @EmployeeLastName,
  @QualificationName1 = @QualificationName

  SET @Run = @Run -1
END
```

```sql
-- Check constraint 1
ALTER FUNCTION [dbo].[CheckRoomsAreUniqueInBuilding](@BuildingName varchar(100), @BranchName varchar(100))
RETURNS int
AS
BEGIN
    DECLARE @BuildingID int
    DECLARE @NumOfRooms int
    DECLARE @NumOfDistinctRooms int
    -- NumOfRooms and NumOfDistinctRooms should match
    SET @BuildingID = (SELECT BuildingID FROM BUILDING B JOIN BRANCH BR ON B.BranchID = BR.BranchID WHERE @BranchName = BR.BranchName AND @BuildingName = B.BuildingName)
    SET @NumOfRooms = (SELECT COUNT(*) FROM ROOM R WHERE @BuildingID = R.BuildingID)
    SET @NumOfDistinctRooms = (SELECT COUNT(DISTINCT RoomNum) FROM ROOM R WHERE @BuildingID = R.BuildingID)
    DECLARE @Ret int
    SELECT @Ret = CASE WHEN @NumOfRooms = @NumOfDistinctRooms THEN 0 ELSE 1 END
    RETURN @Ret
END;
GO

ALTER TABLE ROOM
ADD CONSTRAINT chkRoomsAreUniqueInBuilding
CHECK ([dbo].[CheckRoomsAreUniqueInBuilding]('The Apthorp', 'Downton Lane Seattle') = 0);
-- If it fails, that means there are duplicate room numbers in the building
GO

-- Check constraint 2
ALTER FUNCTION [dbo].[CheckEmployeesOver16]()
RETURNS int
AS
BEGIN
DECLARE @RET INT = 0
IF EXISTS(
  SELECT *
  FROM EMPLOYEE E
  WHERE DATEDIFF(YEAR, E.EmployeeDOB, GETDATE()) < 16
)
SET @RET = 1
RETURN @RET
END;
GO

ALTER TABLE EMPLOYEE
ADD CONSTRAINT chkEmployeesOver16
CHECK ([dbo].[CheckEmployeesOver16]() = 0);
-- Returns 0 means there are no employees over 16
GO
```

```sql
-- Computed column 1
CREATE FUNCTION [dbo].[fnGetNumberOfRooms](@BuildingID int)
RETURNS INT
AS
BEGIN
RETURN (
  SELECT COUNT(*)
  FROM ROOM R
  JOIN BUILDING B ON R.BuildingID = B.BuildingID
  WHERE R.BuildingID = @BuildingID
)
END;

ALTER TABLE BUILDING
ADD NumOfRooms AS [dbo].[fnGetNumberOfRooms](BuildingID)

-- Computed column 2
ALTER FUNCTION [dbo].[fnGetNumberOfQualifications](@EmployeeID int)
RETURNS INT
AS
BEGIN
RETURN (
  SELECT count(*)
  FROM EMPLOYEE_QUALIFICATION EQ
  WHERE @EmployeeID = EQ.EmployeeID
)
END;

ALTER TABLE EMPLOYEE
ADD NumOfQualifications AS [dbo].[fnGetNumberOfQualifications](EmployeeID)
```
