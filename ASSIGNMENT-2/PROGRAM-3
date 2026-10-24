
import csv
import mysql.connector
from mysql.connector import Error


def create_tables(connection):
    cursor = connection.cursor()

    cursor.execute("""
        CREATE TABLE IF NOT EXISTS Student (
            student_id INT PRIMARY KEY,
            name VARCHAR(100) NOT NULL,
            SPI DECIMAL(3,1) NOT NULL
        )
    """)

    cursor.execute("""
        CREATE TABLE IF NOT EXISTS CourseRegistration (
            student_id INT,
            course_id VARCHAR(20),
            PRIMARY KEY (student_id, course_id),
            FOREIGN KEY (student_id) REFERENCES Student(student_id)
        )
    """)

    connection.commit()
    cursor.close()


def import_students(connection, file_path):
    cursor = connection.cursor()

    try:
        with open(file_path, "r", newline="", encoding="utf-8-sig") as file:
            reader = csv.DictReader(file)

            if reader.fieldnames is None:
                raise ValueError("Student CSV file is empty.")

            reader.fieldnames = [
                field.strip() for field in reader.fieldnames
            ]

            required = {"student_id", "name", "SPI"}
            missing = required - set(reader.fieldnames)

            if missing:
                raise ValueError(
                    f"Student CSV missing columns: {sorted(missing)}. "
                    f"Found: {reader.fieldnames}"
                )

            for line_number, row in enumerate(reader, start=2):
                row = {
                    key.strip(): value.strip() if value else value
                    for key, value in row.items()
                    if key is not None
                }

                try:
                    student_id = int(row["student_id"])
                    name = row["name"]
                    spi = float(row["SPI"])
                except (ValueError, TypeError) as error:
                    raise ValueError(
                        f"Invalid student data at CSV line {line_number}: "
                        f"{row}"
                    ) from error

                if not 0 <= spi <= 10:
                    raise ValueError(
                        f"SPI must be between 0 and 10 at line "
                        f"{line_number}."
                    )

                cursor.execute("""
                    INSERT INTO Student (student_id, name, SPI)
                    VALUES (%s, %s, %s)
                    ON DUPLICATE KEY UPDATE
                        name = VALUES(name),
                        SPI = VALUES(SPI)
                """, (student_id, name, spi))

        connection.commit()
        print("Student data imported successfully.")

    except Exception:
        connection.rollback()
        raise
    finally:
        cursor.close()


def import_registrations(connection, file_path):
    cursor = connection.cursor()

    try:
        with open(file_path, "r", newline="", encoding="utf-8-sig") as file:
            reader = csv.DictReader(file)

            if reader.fieldnames is None:
                raise ValueError("Registration CSV file is empty.")

            reader.fieldnames = [
                field.strip() for field in reader.fieldnames
            ]

            required = {"student_id", "course_id"}
            missing = required - set(reader.fieldnames)

            if missing:
                raise ValueError(
                    f"Registration CSV missing columns: {sorted(missing)}. "
                    f"Found: {reader.fieldnames}"
                )

            for line_number, row in enumerate(reader, start=2):
                row = {
                    key.strip(): value.strip() if value else value
                    for key, value in row.items()
                    if key is not None
                }

                try:
                    student_id = int(row["student_id"])
                    course_id = row["course_id"]

                    cursor.execute("""
                        INSERT IGNORE INTO CourseRegistration
                        (student_id, course_id)
                        VALUES (%s, %s)
                    """, (student_id, course_id))

                except (ValueError, TypeError) as error:
                    raise ValueError(
                        f"Invalid registration data at CSV line "
                        f"{line_number}: {row}"
                    ) from error

        connection.commit()
        print("Registration data imported successfully.")

    except Exception:
        connection.rollback()
        raise
    finally:
        cursor.close()


def display_students(connection, course_id, threshold):
    cursor = connection.cursor()

    try:
        query = """
            SELECT s.student_id, s.name, s.SPI, cr.course_id
            FROM Student s
            JOIN CourseRegistration cr
                ON s.student_id = cr.student_id
            WHERE cr.course_id = %s AND s.SPI > %s
            ORDER BY s.SPI DESC, s.student_id ASC
        """

        cursor.execute(query, (course_id, threshold))
        students = cursor.fetchall()

        if not students:
            print("No students found matching the criteria.")
            return

        print("\nStudents matching the criteria:")
        print("-" * 45)

        for student_id, name, spi, course in students:
            print(student_id, name, spi, course)

    finally:
        cursor.close()


def main():
    connection = None

    try:
        host = input("MySQL host (default localhost): ").strip() or "localhost"
        user = input("MySQL username: ").strip()
        password = input("MySQL password: ")
        database = input("Existing database name: ").strip()

        connection = mysql.connector.connect(
            host=host,
            user=user,
            password=password,
            database=database
        )

        if connection.is_connected():
            print("Connected to MySQL successfully.")

            create_tables(connection)

            student_file = input("Student CSV file path: ").strip()
            registration_file = input(
                "Registration CSV file path: "
            ).strip()

            import_students(connection, student_file)
            import_registrations(connection, registration_file)

            course_id = input("Enter course ID: ").strip()
            threshold = float(input("Enter SPI threshold: "))

            if not 0 <= threshold <= 10:
                print("Error: SPI threshold must be between 0 and 10.")
                return

            display_students(connection, course_id, threshold)

    except Error as error:
        print("MySQL error:", error)

    except (OSError, ValueError, KeyError, csv.Error) as error:
        print("Input or file error:", error)

    finally:
        if connection is not None and connection.is_connected():
            connection.close()
            print("MySQL connection closed.")


if __name__ == "__main__":
    main()
