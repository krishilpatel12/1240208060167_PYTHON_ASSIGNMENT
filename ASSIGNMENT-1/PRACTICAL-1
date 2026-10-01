n, k, m = map(int, input().split())

semester_data = {}

max_marks = {}

topper_data = {}

for i in range(1, m + 1):
    max_marks[i] = -1
    topper_data[i] = []

for _ in range(n):
    data = input().split()

    enrollment = data[0]
    name = data[1]
    semester = int(data[2])
    cpi = float(data[3])

    marks = list(map(int, data[4:]))

    average = sum(marks) / m

    student = (enrollment, name, semester, cpi, average, marks)

    if semester not in semester_data:
        semester_data[semester] = []

    semester_data[semester].append(student)

    for i in range(m):
        subject = i + 1
        mark = marks[i]

        if mark > max_marks[subject]:
            max_marks[subject] = mark
            topper_data[subject] = [enrollment]

        elif mark == max_marks[subject]:
            topper_data[subject].append(enrollment)

for semester in sorted(semester_data):

    students = semester_data[semester]

    students.sort(
        key=lambda x: (-x[3], -x[4], x[0])
    )

    top_k = students[:k]

    print(
        f"Semester {semester}:",
        *[student[0] for student in top_k]
    )

for subject in range(1, m + 1):

    toppers = sorted(topper_data[subject])

    print(f"S{subject}:", *toppers)
