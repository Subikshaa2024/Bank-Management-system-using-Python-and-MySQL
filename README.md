# Bank-Management-system-using-Python-and-MySQL
## Objective
The main objective of this project was to develop a comprehensive Bank Account Management System that allows users to perform various banking operations such as opening accounts, depositing money, withdrawing money, viewing account details, and closing accounts. This system was implemented using MySQL for database management and Python for the application logic.
## Tools and Techniques used:
* Programming Language: Python
* Database Management System: MySQL
## Project Features:
### Account Management:
* Open a new account with unique account numbers.
* Store customer details such as name, age, address, mobile number, and initial deposit amount.
### Transaction Management:
* Deposit money into an account.
* Withdraw money from an account with balance validation.
* Update account balances dynamically using triggers in MySQL.
### Account Closure:
* Close an account by removing it from the database.
### Account Inquiry:
* View details of an existing account including balance and customer information.
## Data Integrity and Validation:
* Ensure accurate and consistent data through proper validation checks.
* Implemented MySQL triggers to automatically update balances upon deposits and withdrawals.
## Database Design:
### Tables:
* **account**: Stores account details with columns for account number, customer name, age, address, mobile number, and balance.
* **amt**: Stores transaction details with columns for account number, deposit amount, withdrawal amount, and transaction month.
### Relationships:
* Foreign key relationship between amt and account tables based on account number.
### Key SQL Components:
* **Table Creation and Alteration**:
Create and modify tables to store account and transaction details.
* **Triggers**:
Implemented triggers to update account balance before inserting a transaction.
## Python Application Logic:
Functions to handle various banking operations:
* **accopen()**: Opens a new account and inserts data into the account table.
* **accdeposit()**: Handles money deposits and inserts transaction details into the amt table.
* **withdraw()**: Manages money withdrawals, ensures sufficient balance, and updates the amt table.
* **accview()**: Retrieves and displays account details.
* **closeacc()**: Deletes an account from the account table.
* **menuset()**: Provides a menu-driven interface for user interactions.
## Conclusion
This project enhanced my skills in database management, Python programming, and the implementation of business logic using SQL and triggers. The Bank Account Management System is a testament to my ability to develop and manage database-driven applications, ensuring data accuracy and providing a user-friendly interface.
