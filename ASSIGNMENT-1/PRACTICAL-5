class BankError(Exception):
    pass


class AccountNotFoundError(BankError):
    pass


class InsufficientBalanceError(BankError):
    pass


class InvalidAmountError(BankError):
    pass


class Account:
    def __init__(self, account_id, balance):
        self.__account_id = account_id
        self.__balance = balance

    @property
    def account_id(self):
        return self.__account_id

    @property
    def balance(self):
        return self.__balance

    def deposit(self, amount):
        if amount <= 0:
            raise InvalidAmountError("Invalid amount")

        self.__balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise InvalidAmountError("Invalid amount")

        if amount > self.__balance:
            raise InsufficientBalanceError(
                "Insufficient balance"
            )

        self.__balance -= amount


class Transaction:
    def __init__(self, operation, data):
        self.operation = operation
        self.data = data


class Bank:
    def __init__(self):
        self.accounts = {}
        self.history = []

    def add_account(self, account_id, balance):
        self.accounts[account_id] = Account(
            account_id,
            balance
        )

    def get_account(self, account_id):
        if account_id not in self.accounts:
            raise AccountNotFoundError(
                "Account not found"
            )

        return self.accounts[account_id]

    def deposit(self, account_id, amount):
        self.get_account(account_id).deposit(amount)

    def withdraw(self, account_id, amount):
        self.get_account(account_id).withdraw(amount)

    def transfer(self, source, target, amount):
        src = self.get_account(source)
        dst = self.get_account(target)

        src.withdraw(amount)
        dst.deposit(amount)

    def snapshot(self):
        return {
            acc_id: account.balance
            for acc_id, account in self.accounts.items()
        }

    def restore(self, snapshot):
        for acc_id, balance in snapshot.items():
            account = self.accounts[acc_id]

            # Restore private balance
            account._Account__balance = balance


bank = Bank()

n = int(input())

for _ in range(n):
    acc, balance = input().split()
    bank.add_account(acc, int(balance))

q = int(input())

batch_number = 0
batch_snapshot = None
batch_failed = False

for _ in range(q):

    parts = input().split()
    operation = parts[0]

    try:

        if operation == "BATCH_BEGIN":
            batch_number += 1
            batch_snapshot = bank.snapshot()
            batch_failed = False

        elif operation == "BATCH_END":

            if batch_failed:
                bank.restore(batch_snapshot)
                print(f"FAILED {batch_number}")

            batch_snapshot = None

        elif operation == "DEPOSIT":
            bank.deposit(parts[1], int(parts[2]))

        elif operation == "WITHDRAW":
            bank.withdraw(parts[1], int(parts[2]))

        elif operation == "TRANSFER":
            bank.transfer(
                parts[1],
                parts[2],
                int(parts[3])
            )

    except BankError:

        if batch_snapshot is not None:
            batch_failed = True

# Final balances
for acc_id in sorted(bank.accounts):
    print(acc_id, bank.accounts[acc_id].balance)
