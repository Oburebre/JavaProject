import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Transaction {
    private String type;
    private double amount;

    public Transaction(String type, double amount) {
        this.type = type;
        this.amount = amount;
    }

    @Override
    public String toString() {
        return type + ": $" + amount;
    }
}

class User {
    private String username;
    private double balance;
    private List<Transaction> transactionHistory;

    public User(String username) {
        this.username = username;
        this.balance = 0.0;
        this.transactionHistory = new ArrayList<>();
    }

    public String getUsername() {
        return username;
    }

    public double getBalance() {
        return balance;
    }

    public List<Transaction> getTransactionHistory() {
        return transactionHistory;
    }

    public void deposit(double amount) {
        balance += amount;
        transactionHistory.add(new Transaction("Deposit", amount));
        System.out.println("$" + amount + " deposited successfully for " + username + ".");
    }

    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            transactionHistory.add(new Transaction("Withdrawal", amount));
            System.out.println("$" + amount + " withdrawn successfully for " + username + ".");
        } else {
            System.out.println("Insufficient funds to withdraw $" + amount + " for " + username + ".");
        }
    }
}

class BankingSystem {
    private Map<String, User> users;

    public BankingSystem() {
        this.users = new HashMap<>();
    }

    public void registerUser(String username) {
        if (!users.containsKey(username)) {
            User newUser = new User(username);
            users.put(username, newUser);
            System.out.println("User " + username + " registered successfully.");
        } else {
            System.out.println("Username already exists. Please choose another one.");
        }
    }

    public void deposit(String username, double amount) {
        User user = users.get(username);
        if (user != null) {
            user.deposit(amount);
        } else {
            System.out.println("User does not exist. Please register first.");
        }
    }

    public void withdraw(String username, double amount) {
        User user = users.get(username);
        if (user != null) {
            user.withdraw(amount);
        } else {
            System.out.println("User does not exist. Please register first.");
        }
    }

    public void showTransactionHistory(String username) {
        User user = users.get(username);
        if (user != null) {
            List<Transaction> history = user.getTransactionHistory();
            System.out.println("Transaction history for " + username + ":");
            for (Transaction transaction : history) {
                System.out.println(transaction);
            }
        } else {
            System.out.println("User does not exist. Please register first.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        BankingSystem bankingSystem = new BankingSystem();

        // Register users
        bankingSystem.registerUser("Alice");
        bankingSystem.registerUser("Bob");

        // Perform transactions
        bankingSystem.deposit("Alice", 100.0);
        bankingSystem.withdraw("Bob", 50.0);

        // Show transaction history
        bankingSystem.showTransactionHistory("Alice");
        bankingSystem.showTransactionHistory("Bob");
    }
}
