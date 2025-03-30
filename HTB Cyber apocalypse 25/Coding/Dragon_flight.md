# CTF Challenge Writeup: Dragon Flight (Coding)

## Challenge Overview
Dragon Flight was a coding challenge that required handling dynamic updates and queries on an array representing wind effects. The goal was to efficiently find the maximum flight stretch (subarray sum) while dealing with real-time wind changes.

## Skills Required
- Segment Trees
- Range Query Optimization
- Efficient Updates

## Skills Learned
- How to use segment trees for range queries
- Optimizing updates in real-time scenarios
- Handling edge cases in dynamic array problems

---

## Initial Approach (Didn’t Work)
Before landing on the right approach, I tried a few things that didn’t work well:

1. **Brute Force (Too Slow)**
   - Iterated over the subarray for each query.
   - Worked for small cases but was too slow for large inputs.

2. **Prefix Sums (Not Dynamic Enough)**
   - Stored prefix sums to speed up range queries.
   - The problem? Updates meant recomputing everything, making it inefficient.

3. **Fenwick Tree (Didn’t Work for This Use Case)**
   - Great for sum queries but doesn’t handle max subarray sum well.
   
---

## The Right Approach: Segment Tree
A segment tree allows fast updates and efficient range queries. It’s perfect for problems where values change dynamically but queries need to be processed quickly.

### Algorithm Steps:
1. **Build a segment tree** from the initial wind effects array.
2. **Update function**: Modify a specific segment’s wind effect efficiently.
3. **Query function**: Retrieve the max contiguous subarray sum in a given range.

### Implementation
```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.build(arr, 0, 0, self.n - 1)

    def build(self, arr, node, start, end):
        if start == end:
            self.tree[node] = arr[start]
        else:
            mid = (start + end) // 2
            left_child = 2 * node + 1
            right_child = 2 * node + 2
            self.build(arr, left_child, start, mid)
            self.build(arr, right_child, mid + 1, end)
            self.tree[node] = max(self.tree[left_child], self.tree[right_child])

    def update(self, index, value, node=0, start=0, end=None):
        if end is None:
            end = self.n - 1
        
        if start == end:
            self.tree[node] = value
        else:
            mid = (start + end) // 2
            left_child = 2 * node + 1
            right_child = 2 * node + 2
            if index <= mid:
                self.update(index, value, left_child, start, mid)
            else:
                self.update(index, value, right_child, mid + 1, end)
            self.tree[node] = max(self.tree[left_child], self.tree[right_child])

    def query(self, l, r, node=0, start=0, end=None):
        if end is None:
            end = self.n - 1
        
        if r < start or l > end:
            return float('-inf')  # Out of range
        if l <= start and end <= r:
            return self.tree[node]
        
        mid = (start + end) // 2
        left_child = 2 * node + 1
        right_child = 2 * node + 2
        return max(self.query(l, r, left_child, start, mid), self.query(l, r, right_child, mid + 1, end))

# Read input
N, Q = map(int, input().split())
arr = list(map(int, input().split()))

# Construct segment tree
seg_tree = SegmentTree(arr)

# Process operations
data = [input().split() for _ in range(Q)]
outputs = []
for query in data:
    if query[0] == 'U':
        seg_tree.update(int(query[1]) - 1, int(query[2]))
    elif query[0] == 'Q':
        outputs.append(str(seg_tree.query(int(query[1]) - 1, int(query[2]) - 1)))

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
2. Construct a segment tree from initial wind effects.
3. Process each operation:
   - `U i x` updates segment `i` with value `x`.
   - `Q l r` finds the max subarray sum in range `l` to `r`.
4. Output results for query operations.

---

## Key Takeaways
- **Segment trees** make range queries and updates efficient.
- **Brute force methods are too slow** for large inputs.
- **Efficient handling of dynamic updates** is crucial for problems like this.

After solving all the test cases, the server returned the flag:
```
HTB{DR4G0N_FL1GHT_5TR33_3d89554699afbac5c68c83aec83515e8}
```

Another solid challenge done! 🚀

