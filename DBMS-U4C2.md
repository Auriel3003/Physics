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
> 
> In View Serializability, a schedule is said to be view serializable if 
> it can transform into a serial schedule by applying certain view equivalence rules.

### View Equivalence Rules:

    * View of a transaction before it commits is equivalent to the view of a serial execution of the transaction.
    * A schedule is view equivalent to a serial schedule if all transactions read the same final value of an item in the database.

### To check if a schedule is view serializable or not, follow the below steps:

    * Write the precedence graph for the given schedule.
    * Compute all the equivalent serial schedules of the given schedule by applying the view equivalence rules.
    * If any one of the equivalent serial schedules is conflict serializable, then the given schedule is also conflict serializable and hence it is view serializable.

Note: If a schedule is conflict serializable, then it is always view serializable, but the converse may not be true.    



# Concurrency Control Protocols:

    * Concurrency control manages simultaneous operations without conflicts
    * Problems in concurrency include lost updates, uncommitted dependencies, and non-repeatable reads
    * Concurrency control resolves read-write and write-write conflict issues and ensures serializability
    * Different protocols offer varying levels of concurrency and overhead
    * Optimistic Concurrency Control is a validation-based protocol that 
        allows for more concurrency by delaying validation until a transaction is committed

### Concurrency Control Techniques in DBMS:

> Lock-Based Protocols:

        Mechanism where a transaction cannot read or write data until it acquires an appropriate lock.
        Binary Locks: locked or unlocked states.
        Shared Lock (S): read-only lock, data item can be shared between transactions.
        Exclusive Lock (X): data item can be read and written, exclusive and can't be held concurrently on the same data item.

> Two Phase Locking Protocol:

        Ensures serializability by applying a lock to the transaction data which blocks other transactions to access the same data simultaneously.
        Divides the execution phase of a transaction into three different parts: growing, shrinking, and lock release.

> Timestamp-Based Protocols:

        Uses system time or logical counter as a timestamp to serialize the execution of concurrent transactions.
        Ensures that every conflicting read and write operations are executed in a timestamp order.
        Older transactions are given priority.

> Validation-Based Protocols:

        Also known as Optimistic Concurrency Control Technique.
        Updates local copies of the transaction data rather than the data itself.
        Performed in three phases: read, validation, and write.
        Updates are only applied to the database if validation is successful, else transaction is rolled back.
        
        
# Recovery Techniques :

> Recovery techniques ensure that data stored in database systems is available when required, even after failures.
> 
> Atomicity is a feature of a database system where transactions are either completed successfully and committed or have no effect on the database.
> 
> The system log is a special file that contains information about transactions and updates in the database.
  
    * The log keeps track of all transaction operations that affect the values of database items.
    * Checkpoint is a mechanism where all the previous logs are removed from the system and stored permanently in a storage disk.
    
    * Undoing involves reversing the operations of a transaction in case of a crash, while deferred update does not physically update the database until a transaction has reached its commit point.
    * Immediate update updates the database by some operations of a transaction before the transaction reaches its commit point.
    * Caching/buffering involves caching disk pages that include data items to be updated into main memory buffers.
    
    * Backward recovery involves undoing modifications when a backup of the data is not available, while forward recovery involves updating the database with all changes verified.        
