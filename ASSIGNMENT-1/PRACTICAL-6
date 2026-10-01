import heapq

n, e = map(int, input().split())

modules = []

for _ in range(n):
    modules.append(input().strip())

graph = {module: [] for module in modules}
indegree = {module: 0 for module in modules}

edges = set()

for _ in range(e):
    a, b = input().split()

    # a imports b
    # b must be loaded before a

    edge = (b, a)

    if edge not in edges:
        edges.add(edge)
        graph[b].append(a)
        indegree[a] += 1


heap = []

for module in modules:
    if indegree[module] == 0:
        heapq.heappush(heap, module)


order = []

while heap:

    module = heapq.heappop(heap)
    order.append(module)

    for nxt in graph[module]:
        indegree[nxt] -= 1

        if indegree[nxt] == 0:
            heapq.heappush(heap, nxt)


if len(order) == n:
    print(*order)
else:
    print("CYCLE")
