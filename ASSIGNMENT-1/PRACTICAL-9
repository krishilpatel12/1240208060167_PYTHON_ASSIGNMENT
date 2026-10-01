import heapq
import threading
import time

w, n = map(int, input().split())

jobs = []

for _ in range(n):
    arrival, job_id, priority, duration, resources = input().split()

    jobs.append({
        "arrival": int(arrival),
        "id": job_id,
        "priority": int(priority),
        "duration": int(duration),
        "resources": int(resources)
    })


# Higher priority first.
# For same priority, earlier arrival first.
jobs.sort(key=lambda x: (x["arrival"], -x["priority"]))

workers = [
    {
        "id": f"W{i+1}",
        "free": 0
    }
    for i in range(w)
]

report = []

lock = threading.Lock()


def execute_job(job, worker):

    start = max(job["arrival"], worker["free"])

    finish = start + job["duration"]

    with lock:
        report.append({
            "id": job["id"],
            "worker": worker["id"],
            "start": start,
            "finish": finish,
            "wait": start - job["arrival"]
        })

    worker["free"] = finish


threads = []

# Priority scheduling
pending = []

index = 0
current_time = 0

while index < n or pending:

    if not pending and index < n:
        current_time = max(
            current_time,
            jobs[index]["arrival"]
        )

    while index < n and jobs[index]["arrival"] <= current_time:
        job = jobs[index]

        heapq.heappush(
            pending,
            (
                -job["priority"],
                job["arrival"],
                job["id"],
                index
            )
        )

        index += 1

    if pending:

        _, _, _, job_index = heapq.heappop(pending)

        job = jobs[job_index]

        worker = min(
            workers,
            key=lambda x: x["free"]
        )

        current_time = max(
            current_time,
            worker["free"]
        )

        thread = threading.Thread(
            target=execute_job,
            args=(job, worker)
        )

        threads.append(thread)
        thread.start()

        current_time = worker["free"]

    else:
        current_time += 1


for thread in threads:
    thread.join()


report.sort(key=lambda x: x["id"])

total_wait = 0

for item in report:
    print(
        item["id"],
        item["worker"],
        item["start"],
        item["finish"]
    )

    total_wait += item["wait"]


average_wait = total_wait / n

print(f"AVG_WAIT {average_wait:.2f}")
