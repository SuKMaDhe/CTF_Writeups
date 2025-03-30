# CTF Challenge Writeup: Clockwork Guardian (Coding)

## Challenge Overview
Clockwork Guardian was a coding challenge where we had to find the shortest path through a grid while avoiding obstacles. We started at `(0,0)` and had to reach the exit (`'E'`) without running into sentinels (`1`). Simple enough, right? Not exactly. The challenge was figuring out the right algorithm to get through efficiently.

## Skills Required
- Graph traversal
- Breadth-First Search (BFS)
- Handling edge cases in grid-based problems

## Skills Learned
- Why BFS is the best choice for shortest path problems
- How to handle edge cases in pathfinding challenges
- Avoiding unnecessary computations for efficiency

---

## Initial Approach (Didn’t Work)
Before getting to the right solution, I tried a couple of things that almost worked but had issues:

1. **Depth-First Search (DFS)**
   - The idea was to explore all paths and track the shortest one.
   - Problem? DFS doesn’t guarantee the shortest path in an unweighted grid. It ended up taking longer routes.

2. **Brute Force (Bad Idea)**
   - Tried checking all possible paths and picking the shortest.
   - Completely inefficient, especially for larger grids.

---

## The Right Approach: Breadth-First Search (BFS)
BFS is perfect here because:
1. It finds the shortest path **by design**.
2. It explores level by level, ensuring minimal steps.
3. It avoids redundant computations by tracking visited positions.

### Algorithm Steps:
1. Start at `(0,0)`.
2. Use a queue to explore possible moves (`up, right, down, left`).
3. Keep track of visited positions to prevent cycles.
4. Count steps until we reach `'E'`.
5. If no path exists, return `-1`.

### Implementation
```python
from collections import deque

def shortest_safe_path(grid):
    if not grid or not grid[0]:
        return -1
    
    rows, cols = len(grid), len(grid[0])
    exit_pos = None
    
    # Find the exit position
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == 'E':
                exit_pos = (i, j)
                break
        if exit_pos:
            break
    
    if not exit_pos:
        return -1  # No exit found
    
    directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
    queue = deque([(0, 0, 0)])  # (row, col, steps)
    visited = set([(0, 0)])
    
    while queue:
        row, col, steps = queue.popleft()
        
        if (row, col) == exit_pos:
            return steps
        
        for dr, dc in directions:
            new_row, new_col = row + dr, col + dc
            if (0 <= new_row < rows and
                0 <= new_col < cols and
                (new_row, new_col) not in visited and
                grid[new_row][new_col] != 1):
                queue.append((new_row, new_col, steps + 1))
                visited.add((new_row, new_col))
    
    return -1  # No path found

def main():
    grid = eval(input())
    print(shortest_safe_path(grid))

if __name__ == "__main__":
    main()
```

---

## Example Cases
### Example 1:
Input:
```
[
    [0, 0, 1, 0, 'E'],
    [1, 0, 1, 0, 1],
    [0, 0, 0, 1, 0],
    [1, 1, 0, 0, 0]
]
```
Output:
```
6
```

### Example 2:
Input:
```
[
    [0, 0, 0, 1, 0],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 'E']
]
```
Output:
```
7
```

---

## Solving the Challenge
1. Connected to the remote server using netcat (`nc`).
2. Received the grid data as input.
3. Parsed the input into a Python list.
4. Ran my algorithm to compute the shortest path.
5. Sent the result back to the server.
6. Repeated this process until all test cases were solved.

---

## Key Takeaways
- **BFS is the way to go** for shortest paths in a grid.
- **Tracking visited nodes** prevents unnecessary computations.
- **Handling edge cases** (no exit, grid boundaries) is crucial.

After solving all the test cases, the server returned the flag:
```
HTB{CL0CKW0RK_GU4RD14N_OF_SKYW4TCH_0121471ccf16e7194b449b2a53ec7aa4}
```

Another fun one down! 🚀


