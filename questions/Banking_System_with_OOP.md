# Banking System with OOP

_Score_: 3

Implement a simple banking system using Object-Oriented Programming principles.

First, create an Enum class:
- **TransactionType** (Enum): with members DEPOSIT and WITHDRAW (you can assign any string values to them)

Create 2 classes with the following specifications:

1. **BankAccount Class** (Base class):
   - Protected attribute: `_balance` (float, initialized to 0.0)
   - Protected attribute: `_transactions` (list of dicts, initialized to empty list, each transaction is {"type": TransactionType, "amount": float})
   - Protected attribute: `_account_number` (string)
   - Public attribute: `account_type` (string, default "Basic")
   - Constructor: `__init__(account_number: str)` - initializes all attributes
   - Protected method: `_validate_amount(amount)` - raises ValueError if amount <= 0, otherwise returns True
   - Public method: `deposit(amount)` - validates amount, adds to balance, adds transaction with TransactionType.DEPOSIT to list, returns new balance
   - Public method: `withdraw(amount)` - validates amount, checks sufficient funds, subtracts from balance, adds transaction with TransactionType.WITHDRAW to list, returns new balance (raises ValueError if insufficient)
   - Public method: `get_balance()` - returns current balance
   - Public method: `get_transactions(transaction_type: TransactionType = None)` - returns all transactions if transaction_type is None, otherwise filters and returns transactions matching the given TransactionType

2. **SavingsAccount Class** (inherits from BankAccount):
   - Additional private attribute: `__interest_rate` (float)
   - Constructor: `__init__(account_number: str, interest_rate: float)` - calls parent constructor and sets `__interest_rate`
   - Override `account_type` to "Savings"
   - Public method: `apply_interest()` - calculates interest amount (balance * __interest_rate) and adds it to the balance, then returns new balance (Note: does NOT record a transaction)
   - Override `withdraw(amount)` - calls parent's protected `_validate_amount` method, ensures minimum balance of 100 remains after withdrawal (raises ValueError with message "Minimum balance of 100 must be maintained" if not), subtracts amount from balance, adds transaction to list, returns new balance

The function should return tuple: (TransactionType, BankAccount, SavingsAccount)

## Placeholder:
```java

public static Class<?>[] solution() {
        // TODO: Create TransactionType Enum with DEPOSIT and WITHDRAW members
        
        // TODO: Create BankAccount class with specified attributes and methods
        
        // TODO: Create SavingsAccount class that extends BankAccount
        
        // Write your code here
        return new Class<?>[]{TransactionType.class, BankAccount.class, SavingsAccount.class};
    }
```

## Examples:
```
nan
```

## Verify code:
```java
public static void verify() {
        Class<?>[] result = solution();
        if (result == null || result.length != 3) {
            throw new AssertionError("Solution must return an array of 3 classes");
        }
        
        Class<?> TransactionType = result[0];
        Class<?> BankAccount = result[1];
        Class<?> SavingsAccount = result[2];
        
        // Test TransactionType enum
        if (!TransactionType.isEnum()) {
            throw new AssertionError("TransactionType should be an Enum");
        }
        
        try {
            // Test TransactionType enum values
            Object[] enumConstants = TransactionType.getEnumConstants();
            if (enumConstants == null || enumConstants.length != 2) {
                throw new AssertionError("TransactionType should have exactly 2 values");
            }
            
            Object deposit = null;
            Object withdraw = null;
            for (Object constant : enumConstants) {
                String name = ((Enum<?>) constant).name();
                if ("DEPOSIT".equals(name)) deposit = constant;
                if ("WITHDRAW".equals(name)) withdraw = constant;
            }
            
            if (deposit == null || withdraw == null) {
                throw new AssertionError("TransactionType must have DEPOSIT and WITHDRAW members");
            }
            
            // Test BankAccount class
            Object account = BankAccount.getConstructor(String.class).newInstance("ACC001");
            
            String accountType = (String) BankAccount.getField("accountType").get(account);
            if (!"Basic".equals(accountType)) {
                throw new AssertionError("Account type should be 'Basic'");
            }
            
            double balance = (Double) BankAccount.getMethod("getBalance").invoke(account);
            if (Math.abs(balance - 0.0) > 0.001) {
                throw new AssertionError("Initial balance should be 0");
            }
            
            // Test deposit
            double newBalance = (Double) BankAccount.getMethod("deposit", double.class).invoke(account, 100.0);
            if (Math.abs(newBalance - 100.0) > 0.001) {
                throw new AssertionError("Deposit should return new balance");
            }
            
            BankAccount.getMethod("deposit", double.class).invoke(account, 50.0);
            balance = (Double) BankAccount.getMethod("getBalance").invoke(account);
            if (Math.abs(balance - 150.0) > 0.001) {
                throw new AssertionError("Balance should be 150 after second deposit");
            }
            
            // Test withdraw
            newBalance = (Double) BankAccount.getMethod("withdraw", double.class).invoke(account, 30.0);
            if (Math.abs(newBalance - 120.0) > 0.001) {
                throw new AssertionError("Withdraw should return new balance");
            }
            
            // Test SavingsAccount
            Object savings = SavingsAccount.getConstructor(String.class, double.class).newInstance("SAV001", 0.05);
            
            accountType = (String) SavingsAccount.getField("accountType").get(savings);
            if (!"Savings".equals(accountType)) {
                throw new AssertionError("SavingsAccount type should be 'Savings'");
            }
            
            SavingsAccount.getMethod("deposit", double.class).invoke(savings, 1000.0);
            
            // Test interest
            newBalance = (Double) SavingsAccount.getMethod("applyInterest").invoke(savings);
            if (Math.abs(newBalance - 1050.0) > 0.001) {
                throw new AssertionError("Balance after 5% interest should be 1050");
            }
            
            // Test minimum balance
            SavingsAccount.getMethod("withdraw", double.class).invoke(savings, 900.0);
            balance = (Double) SavingsAccount.getMethod("getBalance").invoke(savings);
            if (Math.abs(balance - 150.0) > 0.001) {
                throw new AssertionError("Balance should be 150");
            }
            
            try {
                SavingsAccount.getMethod("withdraw", double.class).invoke(savings, 100.0);
                throw new AssertionError("Should throw exception when trying to go below minimum balance");
            } catch (Exception e) {
                if (e.getCause() != null && e.getCause() instanceof IllegalArgumentException) {
                    String msg = e.getCause().getMessage().toLowerCase();
                    if (!msg.contains("minimum")) {
                        throw new AssertionError("Error message should mention minimum balance");
                    }
                } else {
                    throw e;
                }
            }
            
            System.out.println("success");
            System.out.flush();
            
        } catch (Exception e) {
            throw new RuntimeException("Verification failed", e);
        }
    }
```

## Solution:
```java
public static Class<?>[] correctSolution() {
        enum TransactionType {
            DEPOSIT, WITHDRAW
        }
        
        class BankAccount {
            protected double balance;
            protected List<Map<String, Object>> transactions;
            protected String accountNumber;
            public String accountType;
            
            public BankAccount(String accountNumber) {
                this.balance = 0.0;
                this.transactions = new ArrayList<>();
                this.accountNumber = accountNumber;
                this.accountType = "Basic";
            }
            
            protected boolean validateAmount(double amount) {
                if (amount <= 0) {
                    throw new IllegalArgumentException("Amount must be positive");
                }
                return true;
            }
            
            public double deposit(double amount) {
                validateAmount(amount);
                balance += amount;
                Map<String, Object> transaction = new HashMap<>();
                transaction.put("type", TransactionType.DEPOSIT);
                transaction.put("amount", amount);
                transactions.add(transaction);
                return balance;
            }
            
            public double withdraw(double amount) {
                validateAmount(amount);
                if (balance < amount) {
                    throw new IllegalArgumentException("Insufficient funds");
                }
                balance -= amount;
                Map<String, Object> transaction = new HashMap<>();
                transaction.put("type", TransactionType.WITHDRAW);
                transaction.put("amount", amount);
                transactions.add(transaction);
                return balance;
            }
            
            public double getBalance() {
                return balance;
            }
            
            public List<Map<String, Object>> getTransactions(TransactionType transactionType) {
                if (transactionType == null) {
                    return transactions;
                }
                List<Map<String, Object>> filtered = new ArrayList<>();
                for (Map<String, Object> t : transactions) {
                    if (t.get("type") == transactionType) {
                        filtered.add(t);
                    }
                }
                return filtered;
            }
        }
        
        class SavingsAccount extends BankAccount {
            private double interestRate;
            
            public SavingsAccount(String accountNumber, double interestRate) {
                super(accountNumber);
                this.interestRate = interestRate;
                this.accountType = "Savings";
            }
            
            public double applyInterest() {
                double interest = getBalance() * interestRate;
                balance += interest;
                return balance;
            }
            
            @Override
            public double withdraw(double amount) {
                validateAmount(amount);
                if ((balance - amount) < 100) {
                    throw new IllegalArgumentException("Minimum balance of 100 must be maintained");
                }
                balance -= amount;
                Map<String, Object> transaction = new HashMap<>();
                transaction.put("type", TransactionType.WITHDRAW);
                transaction.put("amount", amount);
                transactions.add(transaction);
                return balance;
            }
        }
        
        return new Class<?>[]{TransactionType.class, BankAccount.class, SavingsAccount.class};
    }
```
