###
## Global Transaction: 
It is an application server managed transaction, allowing to work with different transactional resources 
(this might be two different database, database and message queue, etc)

## Local Transaction 
It is resource specific transaction (for example Oracle Transactions) and application server has nothing to do with them. 

## XA(eXtended Architecture) V/S Non-XA
https://sites.google.com/site/assignmentssolved/mca/semester4/mc0077/6
An XA transaction, in the most general terms, is a "global transaction" that may span multiple resources. 
A non-XA transaction always involves just one resource. 

An XA transaction involves a coordinating transaction manager, with one or more databases (or other resources, like JMS) all involved in a single global transaction. 
Non-XA transactions have no transaction coordinator, and a single resource is doing all its transaction work itself (this is sometimes called local transactions). 

XA transactions come from the X/Open group specification on distributed, global transactions. JTA includes the X/Open XA spec, in modified form. 
Most stuff in the world is non-XA - a Servlet or EJB or plain old JDBC in a Java application talking to a single database. XA gets involved when you want to work with multiple resources - 2 or more databases, a database and a JMS connection, all of those plus maybe a JCA resource - all in a single transaction. In this scenario, you'll have an app server like Websphere or Weblogic or JBoss acting as the Transaction Manager, and your various resources (Oracle, Sybase, IBM MQ JMS, SAP, whatever) acting as transaction resources. Your code can then update/delete/publish/whatever across the many resources. When you say "commit", the results are commited across all of the resources. When you say "rollback", _everything_ is rolled back across all resources. 

The Transaction Manager coordinates all of this through a protocol called Two Phase Commit (2PC). This protocol also has to be supported by the individual resources. 
In terms of datasources, an XA datasource is a data source that can participate in an XA global transaction. A non-XA datasource generally can't participate in a global transaction (sort of - some people implement what's called a "last participant" optimization that can let you do this for exactly one non-XA item). 

## 2 Phase commit
2 phase commit protocol is an atomic commitment protocol for distributed systems. 
This protocol as its name implies consists of two phases. 
The first one is commit-request phase in which transaction manager coordinates all the transaction resources to commit or abort. 
In the commit-phase, transaction manager decides to finalize operation by committing or aborting according to the votes of the each 
transaction resource.

## ACID
Atomicity − A transaction should be treated as a single unit of operation, which means either the entire sequence of operations is successful or unsuccessful.
Consistency − This represents the consistency of the referential integrity of the database, unique primary keys in tables, etc.
Isolation − There may be many transaction processing with the same data set at the same time. Each transaction should be isolated from others to prevent data corruption.
Durability − Once a transaction has completed, the results of this transaction have to be made permanent and cannot be erased from the database due to system failure.

Link: https://dzone.com/articles/spring-transaction-management

## Isolation: 
@Transactional (isolation=Isolation.READ_COMMITTED)'

The default is Isolation.DEFAULT

Most of the time, we will use default unless and until you have specific requirements.

Informs the transaction (tx) manager that the following isolation level should be used for the current tx. Should be set at the point from where the tx starts because we cannot change the isolation level after starting a tx.

DEFAULT: Use the default isolation level of the underlying database.

READ_COMMITTED: A constant indicating that dirty reads are prevented; non-repeatable reads and phantom reads can occur.

READ_UNCOMMITTED: This isolation level states that a transaction may read data that is still uncommitted by other transactions.

REPEATABLE_READ: A constant indicating that dirty reads and non-repeatable reads are prevented; phantom reads can occur.

SERIALIZABLE: A constant indicating that dirty reads, non-repeatable reads, and phantom reads are prevented.

Dirty Reads: Transaction 'A' writes a record. Meanwhile Transaction 'B' reads that same record before Transaction A commits. Later Transaction A decides to rollback and now we have changes in Transaction B that are inconsistent. This is a dirty read. Transaction B was running in READ_UNCOMMITTED isolation level so it was able to read Transaction A changes before a commit occurred.
Non-Repeatable Reads: Transaction 'A' reads some record. Then Transaction 'B' writes that same  record and commits. Later Transaction A reads that same record again and may get different values because Transaction B made changes to that record and committed. This is a non-repeatable read.
Phantom Reads: Transaction 'A' reads a range of records. Meanwhile Transaction 'B' inserts a new record in the same range that Transaction A initially fetched and commits. Later Transaction A reads the same range again and will also get the record that Transaction B just inserted. This is a phantom read: a transaction fetched a range of records multiple times from the database and obtained different result sets (containing phantom records).

## Propogation:
@Transactional(propagation=Propagation.REQUIRED)
If not specified, the default propagational behavior is REQUIRED. 

Other options are REQUIRES_NEW, MANDATORY, SUPPORTS, NOT_SUPPORTED, NEVER, and NESTED.

REQUIRED : Indicates that the target method can not run without an active tx. If a tx has already been started before the invocation of this method, then it will continue in the same tx or a new tx would begin soon as this method is called.    

REQUIRES_NEW: Indicates that a new tx has to start every time the target method is called. If already a tx is going on, it will be suspended before starting a new one.

MANDATORY: Indicates that the target method requires an active tx to be running. If a tx is not going on, it will fail by throwing an exception.

SUPPORTS: Indicates that the target method can execute irrespective of a tx. If a tx is running, it will participate in the same tx. If executed without a tx it will still execute if no errors.
Methods which fetch data are the best candidates for this option.

NOT_SUPPORTED: Indicates that the target method doesn’t require the transaction context to be propagated.
Mostly those methods which run in a transaction but perform in-memory operations are the best candidates for this option.

NEVER: Indicates that the target method will raise an exception if executed in a transactional process.
This option is mostly not used in projects.

## JNDI configuration

```java
@Bean
public DataSource dataSource(){

return new JNDIDataSourceLookup()
        .getDataSource("jdbc/mytest")
}
```

## Enable transactions in Spring

1. Enable transaction using @EnableTransactionManagement on top of config class
2. Create a bean of PlatformTransactionManager
    Implementations example are
    DataSourceTransactionManager
    JtaTransactionManager
    JpaTransactionManager
    It has 3 methods 
    1. getCurrentTransaction() -> returns active transaction
    2. commit() -> Commit the transaction
    3. rollback() -> Rollback the transaction
   ```java
   @Bean
   public PlatformTransactionManager PlatformTransactionManager(Datasource datasource){
   return new DataSourceTransactionManager(datasource);
   }
   ```
3. Use @Transactional annotation on top of class or method. 
   a. It rollbacks the transaction in case of errors and runtime exceptions
   b. Invocation is proxied by TransactionInterceptor and TransactionAspectSupport which are using PlatformTransactionManager to manage transactions
   
    Making multiple SP calls transactional
   ```java
    @Transactional(rollbackFor=Exception.class)
    public void performBothSProcsTransactionally(){
    //executeSP1
    //executeSP2
    }
   ```



