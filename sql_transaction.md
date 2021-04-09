
# Transactions and Concurrency

- Lost updates occur when two users update the record at the same time
- Inconsistent reads occur when user reads in the middle of another user's transaction which is not a state intended by either user.
    - Read multiple times, the data content is inconsistent
- Dirty reads occur when A user updates the record while another user reads the record.
    - Read uncommitted data



- Transactions are **a sequences of reads and writes to database objects**.

- Serial means
    - **Each transaction runs from start to finish without any intervening actions from other transactions**
- Serializable means
    - **Each transaction can be re-arranged and re-ordered to act as if they run from start to finish without any intervening actions from other transactions**

- Conflicting Operations: Two operations conflict if they:
    1. are by different transactions
    2. are on the same object
    3. at least one of them is a write
- Conflict Dependency Graph (see pdf notes)


