import os
import re
import pickle
import zipfile
from collections import defaultdict

parts = input().split()

mode = parts[0]

if mode == "BUILD":

    folder = parts[1]
    zip_name = parts[2]

    index = defaultdict(list)

    total_files = 0
    total_lines = 0

    for filename in os.listdir(folder):

        path = os.path.join(folder, filename)

        if not os.path.isfile(path):
            continue

        total_files += 1

        with open(path, "r", encoding="utf-8", errors="ignore") as file:

            for line_no, line in enumerate(file, 1):

                total_lines += 1

                tokens = re.findall(
                    r'\b\w+\b',
                    line.lower()
                )

                for token in set(tokens):
                    index[token].append(
                        (filename, line_no)
                    )

    pickle_file = "log_index.pkl"

    with open(pickle_file, "wb") as file:
        pickle.dump(dict(index), file)

    with zipfile.ZipFile(
        zip_name,
        "w",
        zipfile.ZIP_DEFLATED
    ) as archive:

        for filename in os.listdir(folder):

            path = os.path.join(folder, filename)

            if os.path.isfile(path):
                archive.write(
                    path,
                    arcname=filename
                )

        archive.write(pickle_file)

    print("FILES", total_files)
    print("LINES", total_lines)
    print("TOKENS", len(index))


elif mode == "SEARCH":

    pickle_path = parts[1]
    q = int(parts[2])

    with open(pickle_path, "rb") as file:
        index = pickle.load(file)

    for _ in range(q):

        token = input().strip().lower()

        matches = index.get(token, [])

        for filename, line_no in matches:
            print(f"{filename}:{line_no}")
