import tkinter as tk
from tkinter import ttk, messagebox
import json
import csv
import os

FILE = "assignments.json"

data = []

# Load existing data
if os.path.exists(FILE):
    try:
        with open(FILE, "r") as file:
            data = json.load(file)
    except:
        data = []


def save_data():
    with open(FILE, "w") as file:
        json.dump(data, file, indent=4)


def add_record():

    enrollment = entry_enrollment.get().strip()
    name = entry_name.get().strip()
    assignment = entry_assignment.get().strip()
    status = status_var.get()
    marks = entry_marks.get().strip()
    remarks = entry_remarks.get().strip()

    if not enrollment or not name or not assignment:
        messagebox.showerror(
            "Error",
            "Please fill all required fields"
        )
        return

    try:
        marks_value = int(marks)

        if marks_value < 0:
            raise ValueError

    except:
        messagebox.showerror(
            "Error",
            "Marks must be a valid number"
        )
        return

    record = {
        "enrollment": enrollment,
        "name": name,
        "assignment": assignment,
        "status": status,
        "marks": marks_value,
        "remarks": remarks
    }

    data.append(record)

    save_data()

    clear_fields()

    messagebox.showinfo(
        "Success",
        "Record added successfully"
    )


def clear_fields():

    entry_enrollment.delete(0, tk.END)
    entry_name.delete(0, tk.END)
    entry_assignment.delete(0, tk.END)
    entry_marks.delete(0, tk.END)
    entry_remarks.delete(0, tk.END)


def filter_data():

    selected = filter_var.get()

    listbox.delete(0, tk.END)

    for record in data:

        if selected == "All" or record["status"] == selected:

            text = (
                f'{record["enrollment"]} | '
                f'{record["name"]} | '
                f'{record["assignment"]} | '
                f'{record["status"]} | '
                f'{record["marks"]}'
            )

            listbox.insert(tk.END, text)


def export_csv():

    with open(
        "assignment_report.csv",
        "w",
        newline=""
    ) as file:

        writer = csv.DictWriter(
            file,
            fieldnames=[
                "enrollment",
                "name",
                "assignment",
                "status",
                "marks",
                "remarks"
            ]
        )

        writer.writeheader()
        writer.writerows(data)

    messagebox.showinfo(
        "Success",
        "CSV report exported"
    )


root = tk.Tk()

root.title("Assignment Tracker")
root.geometry("700x600")


# Enrollment
tk.Label(root, text="Enrollment").pack()

entry_enrollment = tk.Entry(root)
entry_enrollment.pack()


# Name
tk.Label(root, text="Student Name").pack()

entry_name = tk.Entry(root)
entry_name.pack()


# Assignment
tk.Label(root, text="Assignment").pack()

entry_assignment = tk.Entry(root)
entry_assignment.pack()


# Marks
tk.Label(root, text="Marks").pack()

entry_marks = tk.Entry(root)
entry_marks.pack()


# Remarks
tk.Label(root, text="Remarks").pack()

entry_remarks = tk.Entry(root)
entry_remarks.pack()


# Status
tk.Label(root, text="Status").pack()

status_var = tk.StringVar(value="Pending")

status_menu = ttk.Combobox(
    root,
    textvariable=status_var,
    values=["Pending", "Completed"]
)

status_menu.pack()


# Buttons
tk.Button(
    root,
    text="Add Submission",
    command=add_record
).pack(pady=5)

tk.Button(
    root,
    text="Export CSV",
    command=export_csv
).pack(pady=5)


# Filter
tk.Label(root, text="Filter").pack()

filter_var = tk.StringVar(value="All")

filter_menu = ttk.Combobox(
    root,
    textvariable=filter_var,
    values=["All", "Pending", "Completed"]
)

filter_menu.pack()

tk.Button(
    root,
    text="Apply Filter",
    command=filter_data
).pack(pady=5)


# Listbox
listbox = tk.Listbox(
    root,
    width=90,
    height=15
)

listbox.pack(pady=10)


root.mainloop()
