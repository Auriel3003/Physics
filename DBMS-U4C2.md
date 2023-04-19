# Transactions :

* Transactions are a set of operations that perform some logical set of work in a database.
* Transactions are used to change data in a database by inserting, updating, or deleting data.
* Transactions ensure that all updates either get committed or get rolled back if there is an error to prevent a half-committed state.
  >    Read(X) operation is used to read the value of X from the database and stores it in a buffer in main memory.
  >    
  >    Write(X) operation is used to write the value back to the database from the buffer.



# ACID properties:

> Atomicity: ensures all operations of a transaction reflect in the database or none.
>
> Consistency: ensures the database is in a consistent state before and after the execution of the transaction.
>
> Isolation: ensures a transaction doesn't interfere with the execution of another transaction.
> 
> Durability: ensures that once a transaction completes successfully, the changes made to the database are permanent even in case of system failure.

### Key operations for Atomicity:

    Rollback: aborts the execution and rolls back the changes made by the transaction.
    Commit: commits the changes to the database if the transaction executes successfully.

### Example for Consistency:

    A transferring 1000 dollars to B, where A's initial balance is 2000 and B's initial balance is 5000. 
    Before the transaction, the total of A and B is 7000$, which should remain consistent after the transaction.

### Example for Isolation:

    Two transactions running concurrently to transfer 100$ from account A to B and C. 
    The execution of the transactions should take place in isolation to avoid reading incorrect balances.

> ACID properties ensure the integrity and consistency of data during a transaction and are the backbone of a database management system.



# State Diagram:

> During the lifetime of a transaction, it goes through several states that inform the operating system about its current state.
> 
> These states are important in determining whether the transaction will commit or abort.

* Active state: Transaction is in execution and read/write operations can be performed.
* Partially Committed state: After successful execution of the transaction, all read/write operations are performed on local memory instead of the database to prevent inconsistency.
* Failed state: Transaction is considered failed if any checks fail or if it is aborted in the active state due to a hardware or software failure.
* Committed state: Transaction completes execution successfully and all changes made in the local memory during partially committed state are permanently stored in the database.
* Aborted state: Transaction fails during execution and all uncommitted changes made in the local memory are deleted or rolled back.
* Terminated state: Transactions that are leaving the system and cannot be restarted reach this state.



# DBMS SCHEDULE:

    A schedule is a series of operations from one transaction to another, used to preserve the order of operations in each individual transaction.
    When multiple transactions run concurrently, there needs to be a sequence in which the operations are performed.
    There are two types of schedules: serial schedules and non-serial schedules.

### Serial Schedule:

    In a serial schedule, a transaction is executed completely before starting the execution of another transaction.
    In other words, a transaction does not start execution until the currently running transaction finishes execution.

### Non-Serial Schedule:

    In a non-serial schedule, the operations of multiple transactions are interleaved.
    Unlike a serial schedule, this schedule proceeds without waiting for the execution of the previous transaction to complete.
    There are four types of non-serial schedules: strict, cascadeless, recoverable, and view serializable.

> Strict Schedule:

    In a strict schedule, if the write operation of a transaction precedes a conflicting operation (read or write) 
    of another transaction, then the commit or abort operation of such a transaction should also precede the conflicting operation 
    of the other transaction.

> Cascadeless Schedule:

    In a cascadeless schedule, if a transaction is going to perform a read operation on a value, 
    it has to wait until the transaction that is performing a write on that value commits.

> Recoverable Schedule:

    In a recoverable schedule, if a transaction is reading a value that has been updated by some other transaction, 
    then this transaction can commit only after the commit of the other transaction that is updating the value.

View Serializable Schedule:

    In a view serializable schedule, the result of concurrent execution of several transactions is guaranteed to be the same 
    as if they had executed serially.
    This means that the final database state after concurrent execution is equivalent to 
    the final database state after the same transactions are executed serially, in some order.
    
    

# View Serializability

> is another type of Serializability that can be used to check whether a non-serial schedule is view serializable or not.
> In View Serializability, a schedule is said to be view serializable if it can transform into a serial schedule by applying certain view equivalence rules.

### View Equivalence Rules:

    * View of a transaction before it commits is equivalent to the view of a serial execution of the transaction.
    * A schedule is view equivalent to a serial schedule if all transactions read the same final value of an item in the database.

### To check if a schedule is view serializable or not, follow the below steps:

    * Write the precedence graph for the given schedule.
    * Compute all the equivalent serial schedules of the given schedule by applying the view equivalence rules.
    * If any one of the equivalent serial schedules is conflict serializable, then the given schedule is also conflict serializable and hence it is view serializable.

Note: If a schedule is conflict serializable, then it is always view serializable, but the converse may not be true.    
