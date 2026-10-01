import csv
from collections import defaultdict
from datetime import datetime

filename = input().strip()

balances = defaultdict(float)

with open(filename, "r", newline="") as infile, \
     open("credit.csv", "w", newline="") as credit_file, \
     open("debit.csv", "w", newline="") as debit_file, \
     open("error.csv", "w", newline="") as error_file:

    reader = csv.DictReader(infile)

    credit_writer = csv.DictWriter(
        credit_file,
        fieldnames=reader.fieldnames
    )

    debit_writer = csv.DictWriter(
        debit_file,
        fieldnames=reader.fieldnames
    )

    error_writer = csv.writer(error_file)

    credit_writer.writeheader()
    debit_writer.writeheader()

    error_writer.writerow(
        reader.fieldnames + ["reason"]
    )

    for row in reader:

        try:
            tid = row["tid"]
            account = row["acc"]
            transaction_type = row["type"]
            amount = float(row["amount"])
            timestamp = row["time"]

            if amount <= 0:
                raise ValueError("Amount must be positive")

            if transaction_type not in ("CREDIT", "DEBIT"):
                raise ValueError("Invalid transaction type")

            datetime.strptime(
                timestamp,
                "%Y-%m-%dT%H:%M:%S"
            )

            if transaction_type == "CREDIT":
                credit_writer.writerow(row)
                balances[account] += amount

            else:
                debit_writer.writerow(row)
                balances[account] -= amount

        except Exception as e:
            error_writer.writerow(
                list(row.values()) + [str(e)]
            )


# Account-wise summary
result = sorted(
    balances.items(),
    key=lambda x: (-abs(x[1]), x[0])
)

for account, balance in result:
    print(account, balance)

print("Files created: credit.csv, debit.csv, error.csv")
