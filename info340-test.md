Part A
===

10 questions drawn from the following:

__1. Describe the components of the N-Tier application architecture.__

Separating application components into separate tiers increases the maintainability and scalability of the application. It does this by enabling easier adoption of new technologies that can be applied to a single tier without the requirement to redesign the whole solution. In addition, n-tier applications typically store sensitive information in the middle-tier, which maintains isolation from the presentation tier. 

Early computers had 1-tier, client/server is 2-tier, and client/server/middle is 3-tier, where middle tier contains business logic so only valid requests are sent to the database. The better we understand the stack and the middle-tier, better we can support development. 

__2. Explain what the instructor means by the phrase ‘organize or die’.__

Over history, those that developed systems survived e.g. groups, cities. Farmers developed systems to grow more food and increase efficiency, societies developed systems of economies and markets and specialized roles and labor, cities developed systems of irrigation, sewage, roads, and so on.

These systems help optimize society and organizations so that they can learn and innovate and gain a competitive advantage over those that do not. Organizations will be better able to adapt to a dynamic marketplace if they can track, control, manage, and obtain data.

Better data, better outcomes. Optimize over time. Systems provide control of environment.


__3. Explain what a M:M relationship is and why relational theory does not allow them.__


An example of an M:M relationship is an actor can act in many films and a film can have many actors. You'd have to add multiple ActorID columns to the Films table, one for each actor. But how many do you add? 2? 3? 10? However many you choose, you'll probably end up with a lot of sparse rows where many of the ActorID values are NULL and there's a good chance that you'll run across a case where you need "just one more." So then you're either constantly modifying the schema to try to accommodate or you're imposing some artificial restriction ("no film can have more than 3 actors") to force things to fit.


__4. Explain the purpose of associative entities and provide an example.__


Associative entities maps 2 or more tables together to resolve many-to-many relationships. For example, an actor can act in many films, and a film can have many actors. If this was illustrated, there would be an actor table with the primary key actor_id and a name attribute, as well as a film table with the primary key film_id and a film name attribute:

```
Actor
-----
actor_id
name
```

```
Film
----
film_id
name
```

An associative entity to resolve this into a one-to-many relationship would reference the primary keys from both tables: actor_id and film_id:

```
ActorFilm
---------
actor_id
film_id
```

As an aside, an associative relationship attribute could be added to ActorFilm, like a date_casted timestamp, because it would not fit in the Actor table nor the Film table:

```
ActorFilm
---------
actor_id
film_id
date_casted
```

__5. Explain the benefits of the Relational database design when compared to Hierarchical design.__


Hierarchical are:


- difficult to scale
- inefficient with large data collection
- difficult to do complex relationships
- prone to bad data, anomalies, and redundancy
- file locations are hard-coded; moving files is problematic
- requires specialized skills to set-up, modify, and maintain


Relational databases solve all these issues:


- easy to scale
- efficient with large data collections
- can adapt and customize for complex relationships
- guarantees accurate data and integrity of data
- does not require specialized skills to set up, modify, and maintain because standards have allowed platform-independence




__6. Discuss the purpose and benefits of following the normalization process during database design.__


Normalization makes the database easier to access and maintain data and take up minimal storage space. It helps ensure that the relations derived from the data model do not have redundant data, which can cause update anomalies.


What: process for determining which attributes belong in specific entities. Identifies optimal grouping for attributes.


How: A math exercise where we look for nouns that are misplaced. Look for attributes that are dependents of columns other than the primary key


Why:


- Efficiency in writing speed, because it writes in one location
- Efficiency in storage
- More tables = fewer collisions. For example if you have one street to get into a city, it is going to be very crowded, whereas if you have 10 streets to get into a city it won’t be as crowded. Same thing as if you have 1 table with thousands of objects. Your database will be too crowded, so it is best to split these up into many different tables
- Transactions are smaller
- Look up data as needed
- Fewer anomalies


__7. Explain the difference between an implicit and explicit transaction and provide examples.__


In Explicit transaction mode, you will need to start a transaction explicitly. In Implicit transaction mode, a transaction is automatically started after each commit. So you will only have to commit.


Since the transaction is started 'implicitly', you will not see an explicit 'BEGIN' in the logs.


By default the database operates in explicit transaction mode with auto-committing transactions enabled. That actually means that unless an explicit transaction is started using BEGIN TRANSACTION, every data modification is started in a separate transaction which is committed after the statement. That allows the database to rollback an entire statement when it fails (for instance a bulk insert, or an insert that modifies other data in a trigger).


__8. Discuss the advantages of introducing SuperType/SubTypes into an ERD and provide examples.__


Supertype - an entity type that has a relationship with one or more subtypes.

Subtype - a subgroup of entities with unique attributes.


A data model for PEOPLE. There can be a supertype entity of PEOPLE and its subtype entities can be vendor, customer, and employee. The PEOPLE entity can have attributes like Name, Address, and Phone, which are common to its subtypes. The entities employee, vendor, and customer can then have further unique attributes. 


Supertypes and subtypes give a hierarchy to the data through generalization and specialization.


Advantages:
- Used to create a more concise and readable ERD
- Best maps to object oriented approaches either to databases or related applications
- Attributes common to different entity sets need not be repeated
- They can be grouped in one place as attributes of the supertype
- Attributes of sibling subtypes are likely to be different (and should be for this to be very useful)


__9. Cite and describe the high-level objectives of the major phases of database development.__


Conceptual database design creates the conceptual model of the data. It ignores all physical details (e.g. programming languages, hardware platform and application programs). The data model is built on the information specified by the users' requirements.


Logical database design builds off of the conceptual database design. The underlying data model of the target DBMS is identified (e.g. relational, network, or hierarchical). It ignores all physical details (e.g. storage structures or indexes). Normalization is used to test its correctness.


Physical database design identifies the target DBMS system and has a description of the physical implementation on secondary storage: relations, file organization, indexes, integrity constraints, and security measures.


__10. Explain the major differences between scalability and high-availability; what are the goals and measurements of each?__


Clustering for scalability versus clustering for high availability (HA). HA means that the application will be available without interruption.


Scalability: 
- ability of a system to provide throughput limited only by available hardware resources. 
- a scalable system is one that can handle increasing numbers of requests without adversely affecting response time and throughput
- vertical scaling: increasing cpu within one operating environment
- horizontal scaling: leveraging multiple systems to work together in parallel


High Availability:
- availability is defined as the percentage of time that it works normally
- Unfortunately, measuring availability is not as simple as selecting one of the availability percentages. You must first decide what metric you want to use to qualify downtime. For example, one organization may consider downtime to occur when one database is not mounted. Another organization may consider downtime to occur only when more than half of its users are affected by an outage.


__11. Explain the conditions that are required for a relation.__


- A relation must have a primary key (either auto-numbered, single-field, or multiple-field with 2+ foreign keys). 
- Fields must only contain one item of data to eliminate redundant data and to contain only data directly related to the primary key
- Relate each field directly to the subject of the table
- Don’t include derived or calculated data
- Store information in its smallest logical parts (e.g. FirstName and LastName not Name)
- Should not contain duplicate information and information should not be duplicated between tables


__12. Explain the concept of data independence in the context of relational theory.__


Data independence refers to the separation of the data descriptions from the applications. In other words, a change to the structure of the data or how the data is stored on disk, does not affect the applications that use the data in such a way that it requires substantial alteration. A relational database provides data independence by decomposing data into entities and relationships. So when a value changes, it only changes in one place.


There are two kinds of data independence: Physical Data Independence and Logical Data Independence. The former states that physical changes, e.g. data structures, will not affect applications that depend on the database. As an example, originally the database used Hash Index, but for performance reasons, the database is switched to using a B-Tree Index instead. The application depends on this database does not need to change. The only change the user might see will be performance change. On the other hand, Logical Data Independence requires that logical changes, e.g. adding columns and rows, will not adversely affect applications that depend on the database.


__13. Explain how people tracked data before computers; what were the limitations of these systems?__


Paper-based systems. They were quick to establish, easy to use, and cheap. Disadvantages were:
- Vulnerable --- single-copy of important, mission-critical data (fire, theft, misplacement
- Difficult to query --- not easy to compare and analyze ranges of data
- Difficult to transfer data --- mail a hardcopy, tough to maintain organization of logical data sets
- Scalability limitations --- limited to physical ability of people


__14. Explain what is meant by cardinality, degree, multiplicity and participation.__


Cardinality is the number of occurrences of one entity when involved with the relationship of another. Either one-to-one, many-to-many, or one-to-many.

One-to-One: A person has only one driver license, and a driver license belongs to only one person.

One-to-Many: A car can only have one registered owner, but an owner can have many registered cars.

Many-to-Many: A film can have many actors, and an actor can be in many films.


__15. Describe how entity integrity and referential integrity are enforced.__


Entity integrity:
- Ensures that each row of a table is uniquely identified, so that it can be retrieved separately if necessary
- The primary key is used to uniquely identify each row of the table. No two primary keys can be the same and no primary key can be null
- The primary key enforces entity integrity. These rules guarantee that every row of a table is accessible


Referential integrity:
- Ensures that a value in one table references an existing value in another table
- The value of a foreign key must be within the domain of its related primary key or it must be null (domain is the set of valid values for any column)
- Foreign keys are the implementation of a one-to-many relationship between two tables. A foreign key must always have a corresponding primary key
- The domain of a foreign can’t fall outside the domain of its corresponding primary key. Domain must be same or the foreign key can be null


__16. Explain what is meant by ‘ACID principles’.__


Atomicity:
- Requires that each transaction be “all or nothing”
- If one part fails, the entire transaction fails and the database is left unchanged

Consistency:
- Ensures that any transaction will bring the database from one valid state to another valid state
- Any data written to the database must be valid according to constraints and any defined rules


Isolation:
- Ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed serially
- Uses a concurrency control method

Durability:
- Ensures that once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors
- Results need to be stored permanently
- To defend against power loss, transactions and their effects must be recorded in a non-volatile memory


__17. Give 3 examples of the mechanisms RDBMS implement to enforce ACID properties.__


Example 1:

For durability, to defend against power loss, transactions and their effects must be recorded in a non-volatile memory. Durability can be achieved by flushing the transaction's log records to non-volatile storage before acknowledging commitment.


Example 2:

For consistency, any data written to the database must be valid according to constraints and any defined rules. If the entire transaction is inconsistent with the rules of the database, the entire transaction must be cancelled and the affected rows rolled back to their pre-transaction state.


Example 3:

For atomicity, if one part fails, the entire transaction fails and the database is left unchanged. The series of operations cannot be divided apart and executed partially from each other, which makes the series of operations "indivisible", hence the name. A guarantee of atomicity prevents updates to the database occurring only partially, which can cause greater problems than rejecting the whole series outright. In other words, atomicity means indivisibility and irreducibility.




__18. According to Connolly & Begg, what is the difference between a subquery and a join? Under what circumstances would you not be able to use a subquery?__


Joins and subqueries are both used to combine data from different tables into a single result. 


Subqueries can be used to return either a scalar (single) value or a row set.


Joins are used to return rows.


__19. Explain what is meant by 'write-ahead logging'. Which ACID principle is it enforcing?__


Write-ahead logging (WAL) is a family of techniques for providing atomicity and durability.


All modifications are written to a log before they are applied. Usually both redo and undo information is stored in the log.


The purpose of this can be illustrated by an example. Imagine a program that is in the middle of performing some operation when the machine it is running on loses power. Upon restart, that program might well need to know whether the operation it was performing succeeded, half-succeeded, or failed. 


If a write-ahead log is used, the program can check this log and compare what it was supposed to be doing when it unexpectedly lost power to what was actually done. Based on this comparison, the program could decide to undo what it had started, complete what it had started, or keep things as they are.


__20. Compare the uses of a View to a stored procedure; when might one be preferred over the other?__


Stored procedures are best used for INSERT-UPDATE-DELETE statements. A stored procedure is used when simple SQL just isn't enough.


Views are used for SELECT statements. In views, you cannot alter the data, but it is a simple way to save a complex SELECT.


A stored procedure:


- accepts parameters
- can NOT be used as building block in a larger query
- can contain several statements, loops, IF ELSE, etc.
- can perform modifications to one or several tables
- can NOT be used as the target of an INSERT, UPDATE or DELETE statement.


A view:


- does NOT accept parameters
- can be used as building block in a larger query
- can contain only one single SELECT query
- can NOT perform modifications to any table
- but can (sometimes) be used as the target of an INSERT, UPDATE or DELETE statement.




__21. Describe the difference between optimistic and pessimistic concurrency control.__


The pessimistic concurrency control approach is to have the database server lock the row on User A’s behalf, so User B has to wait until User A has finished its work before proceeded. We effectively solve the problem by not allowing two users to work on the same piece of data at the same time. It just prevents the conflict.

The optimistic concurrency control approach doesn’t actually lock anything – instead, it asks User A to remember what the row looked like when he first saw it, and when it’s time to update it, the user asks the database to go ahead only if the row still looks like he remembers it. It doesn’t prevent a possible conflict, but it can detect it before any damage is done and fail safely.


The pessimistic approach is a user thinking "I’m sure someone will try to update this row at the same time as I will, so I better ask the database server to not let anyone touch it until I’m done with it."


The optimistic approach is a user thinking "I’m sure it’s all good and there will be no conflicts, so I’m just going to remember this row to double-check later, but I’ll let the database server do whatever it wants with the row." Instead of locking every record every time that it is used, the system merely looks for indications that two users actually did try to update the same record at the same time. If that evidence is found, then one user's updates are discarded and the user is informed.


Optimistic locking allows fast performance and high concurrency (access by multiple users), at the cost of occasionally refusing to write data that was initially accepted but was found at the last second to conflict with another user's changes.

Pessimistic locking requires overhead for every operation, whether or not two or more users are actually trying to access the same record. The overhead is small but adds up because every row that is updated requires a lock. Furthermore, every time that a user tries to access a row, the system must also check whether the requested row(s) are already locked by another user or connection.




__22. Describe the major differences between OLTP/Relational Model and OLAP/Dimensional Model.__


OLTP = Online Transactional Processing

OLAP = Online Analytical Processing


OLTP:
- High number of short online transactions (INSERT UPDATE DELETE)
- Very fast query processing
- Maintain data integrity in multi-access environments
- Effectiveness = number of transactions per second
- Detailed and current data


OLAP:
- Low number of transactions
- Queries are often complex
- Effectiveness = response time
- Aggregated, historical data stored in a multi-dimensional schema (usually star schema)


Part B
===

Create the SQL queries based on the following questions and ERD:

![Final ERD](img/final-erd.png)


__1. Write the code to CREATE a stored procedure with the following:__  

- Takes in 6 parameters (FirstName, LastName, IncidentName, IncidentTypeName, IncidentDate, IncidentDescr)
- Inserts a new row into INCIDENT and INCIDENT_ CONTACT in a single explicit transaction


```sql
-- to be filled out
```


__2. Write the code to create a new entity called SCHOOL_TYPE:__  

- Include auto-increment feature
- Include code to establish a foreign key to SCHOOL



```sql
-- to be filled out
```



__3. Write the code to create a computed column in SCHOOL that has the following:__  

- Includes the use of a user-defined function
- Tracks number of ‘student incidents’ (number of incidents that have involved students from each particular school) 


```sql
-- to be filled out
```



__4. Write the code to create a business rule that restricts a person from serving more than two terms on the Board of Directors in the title of ‘President’:__  

```sql
-- to be filled out
```



__5. Write the code to create a business rule that restricts a fundraising events to December or January only:__  


```sql
-- to be filled out
```
