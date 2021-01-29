
#### Views
It's logical virtual copy of the table created by select query
Only query expression will be stored in disk.
Run the query definition each time they are accessed.
As Views does not have any storage cost associated with it and update cost

#### Materialized Views
A materialized view is a table on disk that contains the result set of a query.
Materialized views are disk based and are updated periodically based upon the query definition.
These are used for increasing application performance
These does have a storage cost associated with it so also have update cost associated with it.

#### Pessimistic locking
when you lock the record for your exclusive use until you have finished with it. It has much better integrity than optimistic locking 
but requires you to be careful with your application design to avoid Deadlocks.

Pessimistic locking is when you take an exclusive lock so that no one else can start modifying the record.

It locks the object until the transaction completes.

#### Optimistic locking
is a strategy where you read a record, take note of a version number and check that the version hasn’t changed before you write the record back. 
If the record is dirty (i.e. different version to yours), then you abort the transaction and the user can re-start it.

It doesn't lock the object during the transaction instead it will check the state of the object while committing if any changes it rollbacks.

### Different keys
#### Primary Key – 
a unique key that identifies records in database tables. By unique it means that it must not be Null and must be unique in the table.
the primary key creates a clustered index on the column

#### Unique Key – 
Keys that offer restriction to prevent duplicate data within rows except for null entries.
unique key creates a non-clustered index by default

#### Composite Key - 
is a candidate key that consists of two or more attributes (table columns) that together uniquely identify an entity occurrence (table row).

Example hibernate:
```java
@Entity
class Time {
@EmbeddedId
TimeId id;

    String src;
    String dst;
    Integer distance;
    Integer price;
}

@Embeddable
class TimeId implements Serializable {
Integer levelStation;
Integer confPathID;
}
```
#### A FOREIGN KEY 
It is a key used to link two tables together. A FOREIGN KEY is a field (or collection of fields) in one table that refers to the PRIMARY KEY in another table. The FOREIGN KEY constraint is used to prevent actions that would destroy links between tables

### Joins

#### (INNER) JOIN: 
It returns records that have matching values in both tables

#### LEFT (OUTER) JOIN: 
Returns all records from the left table, and the matched records from the right table

#### RIGHT (OUTER) JOIN: 
Returns all records from the right table, and the matched records from the left table

#### FULL (OUTER) JOIN: 
Returns all records when there is a match in either left or right table

#### Self JOIN
A self JOIN is a regular join, but the table is joined with itself.
```sql
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City
ORDER BY A.City;
```
#### Cross Join
A cross join that does not have a WHERE clause produces the Cartesian product of the tables involved in the join. 
The size of a Cartesian product result set is the number of rows in the first table multiplied by the number of rows in the second table.

### Indexes:

#### Unique Index
Prevents duplicate entries within uniquely indexed columns. They are automatically generated if a Primary Key is available.

#### Clustered index
A clustered index defines the order in which data is physically stored in a table. Table data can be sorted in only way, therefore, there can be only one clustered index per table.

It is generally faster to read from a clustered index if you want to get back all the columns. You do not have to go first to the index and then to the table.

Inside the table the data will be sorted by a clustered index.

Writing to a table with a clustered index can be slower, if there is a need to rearrange the data.
#### Non-Clustered index
A non-clustered index doesn’t sort the physical data inside the table.

We can have many non clustered indices, although each new index will increase the time it takes to write new records.

Inside the non-clustered index data is stored in the specified order.

https://www.sqlshack.com/what-is-the-difference-between-clustered-and-non-clustered-indexes-in-sql-server/

#### Functions
1. Function requires mandatory return type.
2. It always needs schema name while calling function
3. BEGIN and END statements are mandatory
4. We can't call stored procedures inside function
5. In a scalar function, you can return only one variable

```roomsql
CREATE FUNCTION dbo.helloworldfunction()
RETURNS varchar(20)
AS 
BEGIN
	 RETURN 'Hello world'
END
```
Calling : select dbo.helloworldfunction() as regards

#### Procedures
1. Procedures doesn't need mandatory return type.
2. More flexible to call without schema name.
3. BEGIN and END statements are optional if it has one statement.
4. We can call functions inside stored procedures
5. In a stored procedure we can return multiple variables
```roomsql
CREATE FUNCTION dbo.helloworldfunction()
RETURNS varchar(20)
AS 
BEGIN
	 RETURN 'Hello world'
END
```
Calling :
exec HelloWorldprocedure  
execute HelloWorldprocedure  
execute dbo.HelloWorldprocedure  
HelloWorldprocedure  

https://www.sqlshack.com/functions-vs-stored-procedures-sql-server/

#### Schema
It is a logical collection of database objects such as tables, views, stored procedures, functions, indexes, triggers.