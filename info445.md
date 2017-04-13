A
===
Create the SQL queries based on the following questions and ERD:

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
B
===
```sql
USE UNIVERSITY
SELECT TOP 50 * FROM tblSTUDENT

CREATE PROCEDURE tiwansGetCustID
@Fname VARCHAR (35),
@Lname VARCHAR (35),
@DOB DATE
AS
SELECT StudentID FROM tblSTUDENT WHERE StudentFname = @Fname AND StudentLname = @Lname AND StudentBirth = @DOB

EXEC tiwansGetCustID
@Fname = 'Rosario',
@Lname = 'Lovier',
@DOB = '1875-07-24'
```

```sql
Alter PROCEDURE tiwansGetCustID
@Fname VARCHAR (35),
@Lname VARCHAR (35),
@DOB DATE,
@StudentID INT OUTPUT
AS
SET @StudentID = (SELECT StudentID FROM tblSTUDENT WHERE StudentFname = @Fname AND StudentLname = @Lname AND StudentBirth = @DOB)
```

```sql
Create Procedure tiwansGetStudentSate
@StudentID INT
AS
Select StudentPermState FROM tblSTUDENT Where StudentID = @StudentID
Select Top 50 * FROM tblSTUDENT

Exec tiwansGetStudentSate
@StudentID = 9
```

C
===
```sql
/*
Create the 'wrapper' to execute uspRegisterStudent perhaps 100,000 times
*/

ALTER PROCEDURE gthaySynthetic_uspRegisterStudent
@Run INT
AS

DECLARE @FirstN varchar(60) 
DECLARE @LastN Varchar(60) 
DECLARE @BirthDate Date
DECLARE @Registration Date = (SELECT GetDate())
DECLARE @yr char(4) --aligns to ClassYear
DECLARE @Course varchar(75)
DECLARE @Quarter varchar(30)
DECLARE @Section varchar(4)

DECLARE @Num numeric(16,16) --holder of RANDOM number called each loop
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

--Now populate the variables for getting a new classID

SET @yr = (SELECT [Year] FROM tblCLASS WHERE ClassID = @ClassPK)
SET @Course = (SELECT CourseName FROM tblCOURSE C 
				JOIN tblCLASS CS ON C.CourseID = CS.CourseID
				WHERE ClassID = @ClassPK)
SET @Quarter = (SELECT QuarterName FROM tblQUARTER Q 
				JOIN tblCLASS CS ON CS.QuarterID = Q.QuarterID
				WHERE ClassID = @ClassPK)
SET @Section = (SELECT Section FROM tblCLASS WHERE ClassID = @ClassPK)



EXECUTE uspRegisterStudent --this is a real stored procedure we are testing

--look up a real/existing student and then grab values from exact same record
@Fname1 = @FirstN,
@Lname1 = @LastN,
@DOB1 = @BirthDate,
@RegDate = @Registration,

--look up a real/existing course and then grab values from exact same record
@Year1 = @yr,
@CourseName1 = @Course,
@QuartName1 = @Quarter,
@Section1 = @Section


SET @Run = @Run -1
END

--now we are ready to execute synthetic wrapper for testing


EXEC gthaySynthetic_uspRegisterStudent 100
```
