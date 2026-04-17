# Banking Database System 

##  Project Overview

This project demonstrates the design and implementation of a Banking Database System using Oracle SQL. It manages customer, account, and transaction data while ensuring data integrity and consistency.

---

## Database Structure

* **Customers**: Stores customer details
* **Accounts**: Stores account information
* **Transactions**: Records all financial activities

---

##  Features

* Balance calculation using SQL aggregation
* Transaction tracking (Credit/Debit)
* Fraud detection queries
* ACID properties using COMMIT & ROLLBACK
* PL/SQL procedures and functions

---

##  Key SQL Operations

* Total balance per customer
```SQL
SELECT c.name, SUM(a.balance) AS total_balance
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
GROUP BY c.name;
```
* Account-wise transaction summaries
```SQL
SELECT 
    account_id,
    SUM(CASE 
        WHEN txn_type = 'CREDIT' THEN amount
        ELSE -amount
    END) AS calculated_balance
FROM transactions
GROUP BY account_id;
```
* Customer with Highest Balance
```SQL
SELECT *
FROM (
    SELECT c.name, SUM(a.balance) total_balance
    FROM customers c
    JOIN accounts a ON c.customer_id = a.customer_id
    GROUP BY c.name
    ORDER BY total_balance DESC
)
WHERE ROWNUM = 1;
```
---

##  Fraud Detection

* Identifies:

  * High-value transactions (>5000)
 ```SQL
  SELECT *
FROM transactions
WHERE amount > 5000;
```
  * Unusual transaction frequency
```SQL
SELECT account_id, txn_date, COUNT(*) AS txn_count
FROM transactions
GROUP BY account_id, txn_date
HAVING COUNT(*) > 2;
```
  * Negative balances
```SQL
SELECT account_id, balance
FROM accounts
WHERE balance < 0;
```

---

##  PL/SQL Implementations

* Deposit Procedure
```SQL
CREATE OR REPLACE PROCEDURE deposit_money (
    acc_id NUMBER,
    amt NUMBER
)
IS
BEGIN
    UPDATE accounts
    SET balance = balance + amt
    WHERE account_id = acc_id;

    COMMIT;
END;
/
```

* Balance Fetch Function
```SQL
CREATE OR REPLACE FUNCTION get_balance (
    acc_id NUMBER
)
RETURN NUMBER
IS
    bal NUMBER;
BEGIN
    SELECT balance INTO bal
    FROM accounts
    WHERE account_id = acc_id;

    RETURN bal;
END;
/
```
---



## Querying & Tool Used

* SQL & PL/SQL
* Oracle SQL Developer

---
## Conclusion & Key Findings

This Banking Database System project successfully demonstrates how relational databases can be used to manage and analyze financial data efficiently using Oracle SQL and PL/SQL.

### Key Findings
* Efficient Data Management :
The structured schema (Customers, Accounts, Transactions) ensures organized storage and easy retrieval of banking data.
* Accurate Balance Tracking :
Using SQL aggregations and conditional logic, account balances can be dynamically calculated from transaction history, ensuring reliability.
* Fraud Detection Insights :
The system can identify :-
High-value transactions (potential risk),
Unusual transaction frequency,
Accounts with negative balances.
This shows how SQL can be used for basic fraud monitoring.
* Use of ACID Properties :
Transaction control using COMMIT and ROLLBACK ensures :-
Data consistency,
Safe financial operations,
Error recovery capability
* PL/SQL Automation :
Basic procedures and functions help automate operations like deposits and balance checks, reducing manual effort.
* Scalability Potential :
The design can be extended to include :-
Loans, branches, and employees
Real-time transaction processing and
Advanced fraud detection systems.


