Transactional Consistency ensures that database transactions maintain data integrity and satisfy the ACID properties, particularly focusing on the Consistency property.

# ACID Properties

## Atomicity
All operations in a transaction succeed or all fail (all-or-nothing).

## Consistency
Transaction moves database from one valid state to another, maintaining all rules and constraints.

## Isolation
Concurrent transactions don't interfere with each other.

## Durability
Committed transactions persist even after system failure.

# Consistency in ACID

Consistency ensures:
- Database constraints are satisfied
- Business rules are maintained
- Referential integrity is preserved
- Data invariants hold true

## Example

```sql
-- Banking transfer
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

-- Constraint: Total balance shouldn't change
-- Rule: No negative balances
-- Invariant: SUM(balance) = constant

COMMIT;
```

If any constraint would be violated, transaction rolls back.

# Types of Consistency Constraints

## 1. Entity Integrity

Primary keys must be unique and not null.

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,  -- Must be unique, not null
    email VARCHAR(255) NOT NULL UNIQUE
);
```

## 2. Referential Integrity

Foreign keys must reference existing records.

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- Can't insert order with non-existent user_id
```

## 3. Domain Constraints

Values must be within defined domain.

```sql
CREATE TABLE products (
    price DECIMAL(10,2) CHECK (price > 0),
    quantity INT CHECK (quantity >= 0),
    status VARCHAR(20) CHECK (status IN ('active', 'inactive', 'deleted'))
);
```

## 4. Custom Business Rules

Application-specific consistency rules.

```sql
CREATE TRIGGER check_inventory
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    IF NEW.quantity < 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Quantity cannot be negative';
    END IF;
END;
```

# Isolation Levels

Different levels control transaction visibility and prevent anomalies.

## Read Uncommitted

Transactions can see uncommitted changes from other transactions.

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

**Problems**:
- Dirty reads (reading uncommitted data)
- Non-repeatable reads
- Phantom reads

**Use case**: Approximate counts, analytics where precision isn't critical

## Read Committed

Transactions only see committed changes.

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

**Prevents**: Dirty reads
**Allows**: Non-repeatable reads, phantom reads

**Use case**: Most common default level

## Repeatable Read

Same data is seen throughout transaction.

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

**Prevents**: Dirty reads, non-repeatable reads
**Allows**: Phantom reads

**Use case**: Financial reports, consistency within transaction

## Serializable

Strongest isolation - transactions appear sequential.

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

**Prevents**: All anomalies (dirty reads, non-repeatable reads, phantom reads)

**Use case**: Critical transactions requiring complete isolation

## Comparison

| Level | Dirty Read | Non-Repeatable Read | Phantom Read | Performance |
|-------|------------|-------------------|--------------|-------------|
| Read Uncommitted | ✗ | ✗ | ✗ | Fastest |
| Read Committed | ✓ | ✗ | ✗ | Fast |
| Repeatable Read | ✓ | ✓ | ✗ | Slower |
| Serializable | ✓ | ✓ | ✓ | Slowest |

# Concurrency Anomalies

## Dirty Read

Reading uncommitted changes that might be rolled back.

```
Transaction 1: UPDATE accounts SET balance = 1000 WHERE id = 1
Transaction 2: SELECT balance FROM accounts WHERE id = 1  -- sees 1000
Transaction 1: ROLLBACK  -- Transaction 2 saw non-existent value
```

## Non-Repeatable Read

Same query returns different results within transaction.

```
Transaction 1: SELECT balance FROM accounts WHERE id = 1  -- returns 500
Transaction 2: UPDATE accounts SET balance = 1000 WHERE id = 1
Transaction 2: COMMIT
Transaction 1: SELECT balance FROM accounts WHERE id = 1  -- returns 1000 (different!)
```

## Phantom Read

New rows appear in repeated query.

```
Transaction 1: SELECT COUNT(*) FROM orders WHERE status = 'pending'  -- returns 5
Transaction 2: INSERT INTO orders (status) VALUES ('pending')
Transaction 2: COMMIT
Transaction 1: SELECT COUNT(*) FROM orders WHERE status = 'pending'  -- returns 6
```

## Lost Update

Concurrent updates cause one to be lost.

```
Transaction 1: Read balance = 500
Transaction 2: Read balance = 500
Transaction 1: Update balance = 500 + 100 = 600
Transaction 2: Update balance = 500 - 50 = 450
Transaction 1: COMMIT (balance = 600)
Transaction 2: COMMIT (balance = 450)  -- Transaction 1's update lost!
```

# Locking Mechanisms

## Pessimistic Locking

Lock data before accessing.

```sql
-- Row-level lock
BEGIN TRANSACTION;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
-- Row is locked, other transactions wait
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;
```

**Pros**: Prevents conflicts
**Cons**: Lower concurrency, potential deadlocks

## Optimistic Locking

Check for conflicts at commit time.

```sql
-- Version-based optimistic locking
UPDATE accounts
SET balance = balance - 100, version = version + 1
WHERE id = 1 AND version = @old_version;

-- If 0 rows updated, another transaction modified the row
-- Retry transaction
```

**Pros**: Better concurrency
**Cons**: May need retries

## Implementation

```python
def transfer_with_optimistic_lock(from_id, to_id, amount):
    max_retries = 3
    for attempt in range(max_retries):
        try:
            # Read with version
            from_account = db.get_account(from_id)
            to_account = db.get_account(to_id)
            
            # Perform business logic
            new_from_balance = from_account.balance - amount
            new_to_balance = to_account.balance + amount
            
            # Update with version check
            from_updated = db.update_account(
                from_id,
                balance=new_from_balance,
                old_version=from_account.version
            )
            to_updated = db.update_account(
                to_id,
                balance=new_to_balance,
                old_version=to_account.version
            )
            
            if from_updated and to_updated:
                return True  # Success
            
            # Conflict detected, retry
            time.sleep(0.1 * attempt)
        
        except Exception as e:
            # Handle error
            pass
    
    return False  # Failed after retries
```

# Distributed Transactions

Maintaining consistency across multiple databases.

## Two-Phase Commit (2PC)

**Phase 1 - Prepare**:
```
Coordinator → Participants: "Can you commit?"
Participants → Coordinator: "Yes" or "No"
```

**Phase 2 - Commit/Abort**:
```
If all said "Yes":
    Coordinator → Participants: "Commit"
Else:
    Coordinator → Participants: "Abort"
```

**Pros**: Strong consistency
**Cons**: Blocking, coordinator is single point of failure

## Saga Pattern

Break transaction into smaller steps with compensating actions.

```python
# Order saga
def process_order(order):
    steps = [
        (reserve_inventory, cancel_inventory),
        (charge_payment, refund_payment),
        (create_shipment, cancel_shipment)
    ]
    
    completed = []
    
    try:
        for step, compensate in steps:
            step(order)
            completed.append(compensate)
    except Exception:
        # Compensate in reverse order
        for compensate in reversed(completed):
            compensate(order)
        raise
```

**Pros**: No locks, scalable
**Cons**: Eventual consistency, complex compensation logic

# Best Practices

## 1. Keep Transactions Short

```python
# Bad - long transaction
BEGIN TRANSACTION
    fetch_user_data()
    process_complex_calculations()  # Takes 5 seconds
    send_emails()  # Takes 10 seconds
    update_database()
COMMIT

# Good - short transaction
fetch_user_data()
process_complex_calculations()
send_emails()
BEGIN TRANSACTION
    update_database()  # Fast
COMMIT
```

## 2. Acquire Locks in Consistent Order

```python
# Bad - potential deadlock
Transaction 1: LOCK A, LOCK B
Transaction 2: LOCK B, LOCK A  # Deadlock!

# Good - consistent order
Transaction 1: LOCK A, LOCK B
Transaction 2: LOCK A, LOCK B  # No deadlock
```

## 3. Use Appropriate Isolation Level

```python
# Read-heavy analytics
SET ISOLATION LEVEL READ UNCOMMITTED

# Normal operations
SET ISOLATION LEVEL READ COMMITTED

# Critical financial transactions
SET ISOLATION LEVEL SERIALIZABLE
```

## 4. Handle Deadlocks

```python
def execute_with_retry(transaction_fn):
    max_retries = 3
    for attempt in range(max_retries):
        try:
            return transaction_fn()
        except DeadlockDetected:
            if attempt == max_retries - 1:
                raise
            time.sleep(random.uniform(0.1, 0.5))
```

## 5. Validate Before Committing

```sql
BEGIN TRANSACTION;

-- Perform operations
UPDATE accounts SET balance = balance - 100 WHERE id = 1;

-- Validate
SELECT balance FROM accounts WHERE id = 1;
IF balance < 0 THEN
    ROLLBACK;
END IF;

COMMIT;
```

# Monitoring

Key metrics:

- Transaction duration
- Lock wait time
- Deadlock frequency
- Isolation level usage
- Rollback rate
- Constraint violations

# Related Topics

- [[ACID Properties]]
- [[Isolation Levels]]
- [[Distributed Transactions]]
- [[Database Locking]]
- [[Saga Pattern]]
- [[Two-Phase Commit]]
