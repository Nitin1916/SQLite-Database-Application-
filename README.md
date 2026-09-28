# SQLite-Database-Application-
Using sqlite3 library for this task . With CRUD methods allowed .

import sqlite3

# Connect to database
conn = sqlite3.connect("students.db")
cursor = conn.cursor()

# Create table
cursor.execute("""
CREATE TABLE IF NOT EXISTS students (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    age INTEGER,
    course TEXT
)
""")
conn.commit()


def add_student():
    name = input("Enter Name: ")
    age = int(input("Enter Age: "))
    course = input("Enter Course: ")

    cursor.execute(
        "INSERT INTO students (name, age, course) VALUES (?, ?, ?)",
        (name, age, course)
    )
    conn.commit()
    print("Student Added Successfully!")


def view_students():
    cursor.execute("SELECT * FROM students")
    rows = cursor.fetchall()

    if not rows:
        print("No records found.")
        return

    for row in rows:
        print(row)


def update_student():
    sid = int(input("Enter Student ID: "))
    new_course = input("Enter New Course: ")

    cursor.execute(
        "UPDATE students SET course=? WHERE id=?",
        (new_course, sid)
    )
    conn.commit()
    print("Record Updated!")


def delete_student():
    sid = int(input("Enter Student ID: "))

    cursor.execute(
        "DELETE FROM students WHERE id=?",
        (sid,)
    )
    conn.commit()
    print("Record Deleted!")


while True:
    print("\n===== STUDENT DATABASE =====")
    print("1. Add Student")
    print("2. View Students")
    print("3. Update Student")
    print("4. Delete Student")
    print("5. Exit")

    choice = input("Enter Choice: ")

    if choice == "1":
        add_student()

    elif choice == "2":
        view_students()

    elif choice == "3":
        update_student()

    elif choice == "4":
        delete_student()

    elif choice == "5":
        break

    else:
        print("Invalid Choice")

conn.close()
