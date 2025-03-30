# CTF Challenge Writeup: Dragon Flight (Coding)

## Challenge Overview
Dragon Flight was a coding challenge that required handling dynamic updates and queries on an array representing wind effects. The goal was to efficiently retrieve values while dealing with real-time wind changes.

## Skills Required
- Array Manipulation
- Efficient Updates
- Handling Queries

## Skills Learned 
- How to efficiently update an array in real-time
- Handling edge cases in query-based problems
- Optimizing simple data structures

---

## Initial Approach (Didn’t Work)
Before landing on the right approach, I tried a few things that didn’t work well:

1. **Brute Force (Too Slow)**
   - Iterated over the subarray for each query.
   - Worked for small cases but was too slow for large inputs.

2. **Prefix Sums (Not Dynamic Enough)**
   - Stored prefix sums to speed up range queries.
   - The problem? Updates meant recomputing everything, making it inefficient.

3. **Fenwick Tree (Overkill for the Problem)**
   - Good for sum queries, but unnecessary for this simple case.
   
---

## The Right Approach: Direct Array Updates
Instead of using a complex data structure, a simple direct approach works best. We just update the array when needed and retrieve values directly for queries.

### Algorithm Steps:
1. **Read the input** to get the array size and operations count.
2. **Process updates**: Modify specific elements in the array.
3. **Process queries**: Retrieve values from the array when requested.

### Implementation
```python
# Read input
N, Q = map(int, input().split())
arr = list(map(int, input().split()))

data = [input().split() for _ in range(Q)]
outputs = []
for query in data:
    if query[0] == 'U':
        arr[int(query[1]) - 1] = int(query[2])  # Update value at index
    elif query[0] == 'Q':
        outputs.append(str(arr[int(query[1]) - 1]))  # Retrieve value

# Print results
print("\n".join(outputs))
```

---

## Sample Input and Expected Output

### Flight Path Input
```
6 6
-10 -7 -1 -4 0 -5
Q 3 3
U 2 9
Q 6 6
U 1 -1
Q 6 6
U 5 -9
```

### Expected Output
```
-1
-5
-5
```

---

## Solving the Challenge
1. Read input values (N segments, Q operations).
2. Process each operation:
   - `U i x` updates segment `i` with value `x`.
   - `Q l r` retrieves the value at index `l`.
3. Output results for query operations.

---

## Key Takeaways
- **Direct array manipulation** is often the simplest and most efficient solution.
- **Avoid overcomplicating problems** with unnecessary data structures.
- **Optimized updates and queries** can solve real-time input challenges efficiently.

After solving all the test cases, the server returned the flag:
```
HTB{DR4G0N_FL1GHT_5TR33_3d89554699afbac5c68c83aec83515e8}
```

Another solid challenge done! 🚀

---

# Web CTF Challenge Writeup: Dragon Fury (Coding)

## Challenge Overview
Dragon Fury was a coding challenge that required finding the right combination of attack values to match a target damage value. The challenge ensured that exactly one valid combination existed.

## Skills Required
- Brute-force searching
- Itertools product for combinations
- Basic list parsing

## Approach
1. **Parse input**: Convert the string of subarrays into a Python list.
2. **Generate attack combinations**: Use `itertools.product` to create all possible attack combinations.
3. **Check for the correct sum**: Find the combination that sums exactly to the target damage.

### Implementation
```python
import ast
from itertools import product

# Read input
data = input().strip()
target = int(input().strip())

# Convert string to list of lists
arrays = ast.literal_eval(data)

# Find the valid combination
for combo in product(*arrays):
    if sum(combo) == target:
        print(list(combo))
        break
```

---

## Sample Input and Expected Output
```
[[13, 15, 27, 17], [24, 15, 28, 6, 15, 16], [7, 25, 10, 14, 11], [23, 30, 14, 10]]
77
```

### Expected Output
```
[13, 24, 10, 30]
```

---

## Key Takeaways
- **Brute-force approach works here** since only one valid combination exists.
- **`itertools.product`** simplifies generating all possible attack combinations.

After solving the challenge, the server returned the flag:
```
HTB{DR4G0NS_FURY_SIM_C0MB0_59c11cf03b523d04faffb4dddba259aa}
```

