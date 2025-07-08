import sys
import random
import datetime

class Customer:
    bankname = "NNN BANK"

    def __init__(self, name, pin, balance=0.0):
        self.name = name
        self.pin = pin
        self.balance = balance
        self.acc_no = random.randint(1000000000, 9999999999)
        self.transactions = []

    def deposit(self, amount):
        self.balance += amount
        self._log_transaction("Deposit", amount)
        print("✅ Amount deposited successfully.")
        print("💰 Current Balance:", self.balance)

    def withdraw(self, amount):
        if amount > self.balance:
            print("⚠️ Insufficient Funds.")
            return False
        self.balance -= amount
        self._log_transaction("Withdraw", amount)
        print("💸 Withdrawal successful. New Balance:", self.balance)
        return True

    def view_balance(self):
        print("💳 Account Balance:", self.balance)

    def view_transactions(self):
        if not self.transactions:
            print("📭 No transactions yet.")
        else:
            print("\n🧾 Transaction History:")
            for txn in self.transactions:
                print(txn)

    def _log_transaction(self, txn_type, amount):
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        entry = f"[{timestamp}] {txn_type}: ₹{amount:.2f} | Balance: ₹{self.balance:.2f}"
        self.transactions.append(entry)

def get_valid_amount():
    while True:
        try:
            amount = float(input("💵 Enter amount: ₹"))
            if amount <= 0:
                print("⚠️ Amount should be positive. Try again.")
            else:
                return amount
        except ValueError:
            print("❌ Invalid input. Please enter a numeric value.")

def main():
    print(f"\n🏦 Welcome to {Customer.bankname}")
    name = input("Enter your name: ")
    pin = input("Set a 4-digit PIN: ")
    c = Customer(name, pin)

    print(f"🎉 Account created successfully! Your Account Number: {c.acc_no}")

    # Simple login check
    while True:
        entered_pin = input("🔐 Enter your PIN to continue: ")
        if entered_pin == c.pin:
            break
        else:
            print("❌ Incorrect PIN. Try again.")

    while True:
        print("\n=== MAIN MENU ===")
        print("d - Deposit\nw - Withdraw\nv - View Balance\nt - View Transactions\ne - Exit")
        option = input("Choose an option: ").lower()

        if option == "d":
            amount = get_valid_amount()
            c.deposit(amount)

        elif option == "w":
            amount = get_valid_amount()
            if amount < 100:
                print("⚠️ Minimum withdrawal is ₹100.")
            else:
                c.withdraw(amount)

        elif option == "v":
            c.view_balance()

        elif option == "t":
            c.view_transactions()

        elif option == "e":
            print("👋 Thank you for banking with us!")
            print(f"Final Balance: ₹{c.balance:.2f}")
            break

        else:
            print("❌ Invalid option... Please try again.")

if __name__ == "__main__":
    main()
