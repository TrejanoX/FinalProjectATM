import javax.swing.JOptionPane;

public class FinalProject {
    private static Account account1 = null;
    private static Account account2 = null;

    public static void main(String[] args) {
        while (true) {
            String menu = "ATM Simulator ni Kiefer\n\n" +
                          "1. Open an account\n" +
                          "2. Deposit\n" +
                          "3. Withdraw\n" +
                          "4. Send Money\n" +
                          "5. Display Account\n" +
                          "6. Exit\n\n" +
                          "Enter your choice (1-6):";
            String choiceStr = JOptionPane.showInputDialog(null, menu, "ATM Simulator", JOptionPane.QUESTION_MESSAGE);
            if (choiceStr == null) break; // User canceled
            int choice;
            try {
                choice = Integer.parseInt(choiceStr.trim());
            } catch (NumberFormatException e) {
                JOptionPane.showMessageDialog(null, "Invalid choice. Please enter a number between 1 and 6.");
                continue;
            }

            if (choice == 1) { // Open account
                openAccount();
            } else if (choice == 2) { // Deposit
                deposit();
            } else if (choice == 3) { // Withdraw
                withdraw();
            } else if (choice == 4) { // Send Money
                sendMoney();
            } else if (choice == 5) { // Display Account
                displayAccount();
            } else if (choice == 6) { // Exit
                 JOptionPane.showMessageDialog(null, "Thank you for using Kiefer's ATM!!"); //It will appear when exiting the ATM
                break;
            } else {
                JOptionPane.showMessageDialog(null, "Invalid choice. Please select 1-6.");
            }
        }
    }

    private static void openAccount() {
        if (account1 == null) {
            account1 = new Account("Account 1");
            JOptionPane.showMessageDialog(null, "Account 1 opened successfully.");
            depositInitialBalance(account1);
        } else if (account2 == null) {
            account2 = new Account("Account 2");
            JOptionPane.showMessageDialog(null, "Account 2 opened successfully.");
            depositInitialBalance(account2);
        } else {
            JOptionPane.showMessageDialog(null, "Maximum accounts reached.");
        }
    }

    private static void depositInitialBalance(Account acc) {
        String amountStr = JOptionPane.showInputDialog("Deposit an initial balance:");
        if (amountStr == null) return; // User canceled, balance remains 0
        try {
            double amount = Double.parseDouble(amountStr.trim());
            if (amount > 0) {
                acc.deposit(amount);
                JOptionPane.showMessageDialog(null, "Initial balance of $" + amount + " deposited to " + acc.getName());
            } else {
                JOptionPane.showMessageDialog(null, "Invalid amount. Initial balance set to $0.");
            }
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "Invalid input. Initial balance set to $0.");
        }
    }

    private static void deposit() {
        Account acc = chooseAccount();
        if (acc == null) return;
        String amountStr = JOptionPane.showInputDialog("Enter deposit amount:");
        if (amountStr == null) return;
        try {
            double amount = Double.parseDouble(amountStr.trim());
            if (amount > 0) {
                acc.deposit(amount);
                JOptionPane.showMessageDialog(null, "Deposited $" + amount + " to " + acc.getName());
            } else {
                JOptionPane.showMessageDialog(null, "Invalid amount.");
            }
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "Invalid input.");
        }
    }

    private static void withdraw() {
        Account acc = chooseAccount();
        if (acc == null) return;
        String amountStr = JOptionPane.showInputDialog("Enter withdrawal amount:");
        if (amountStr == null) return;
        try {
            double amount = Double.parseDouble(amountStr.trim());
            if (amount > 0 && acc.withdraw(amount)) {
                JOptionPane.showMessageDialog(null, "Withdrew $" + amount + " from " + acc.getName());
            } else {
                JOptionPane.showMessageDialog(null, "Insufficient funds or invalid amount.");
            }
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "Invalid input.");
        }
    }

    private static void sendMoney() {
        Account from = chooseAccount();
        if (from == null) return;
        Account to = (from == account1) ? account2 : account1;
        if (to == null) {
            JOptionPane.showMessageDialog(null, "No other account to send to.");
            return;
        }
        String amountStr = JOptionPane.showInputDialog("Enter amount to send:");
        if (amountStr == null) return;
        try {
            double amount = Double.parseDouble(amountStr.trim());
            if (amount > 0 && from.withdraw(amount)) {
                to.deposit(amount);
                JOptionPane.showMessageDialog(null, "Sent $" + amount + " from " + from.getName() + " to " + to.getName());
            } else {
                JOptionPane.showMessageDialog(null, "Insufficient funds or invalid amount.");
            }
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "Invalid input.");
        }
    }

    private static void displayAccount() {
        Account acc = chooseAccount();
        if (acc == null) return;
        JOptionPane.showMessageDialog(null, acc.getName() + " Balance: $" + acc.getBalance());
    }

    private static Account chooseAccount() {
        if (account1 == null && account2 == null) {
            JOptionPane.showMessageDialog(null, "No accounts available.");
            return null;
        }
        String accMenu = "Select an account:\n\n";
        int optionCount = 1;
        if (account1 != null) {
            accMenu += optionCount + ". Account 1\n";
            optionCount++;
        }
        if (account2 != null) {
            accMenu += optionCount + ". Account 2\n";
        }
        accMenu += "\nEnter your choice (" + (account1 != null ? "1" : "2") + "-" + optionCount + "):";
        String choiceStr = JOptionPane.showInputDialog(null, accMenu, "Select Account", JOptionPane.QUESTION_MESSAGE);
        if (choiceStr == null) return null;
        int choice;
        try {
            choice = Integer.parseInt(choiceStr.trim());
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "Invalid choice.");
            return null;
        }
        if (choice == 1 && account1 != null) {
            return account1;
        } else if (choice == 2 && account2 != null) {
            return account2;
        } else if (choice == 1 && account1 == null && account2 != null) {
            return account2;
        }
        JOptionPane.showMessageDialog(null, "Invalid choice.");
        return null;
    }
}
