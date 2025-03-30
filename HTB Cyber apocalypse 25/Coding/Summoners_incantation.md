# CTF Challenge Writeup :- Summoners Incantation - Coding

## Challenge Overview

Deep in the ancient halls, the secret of the Dragon's Heart is hidden. To unlock its power, we must harness the energy of magical tokens—but there's a catch. If we pick two adjacent tokens, their energy vanishes into the void.

### Objective
Find the maximum energy we can extract while ensuring no two selected tokens are adjacent. This boils down to computing the maximum sum of non-adjacent numbers in a given list.

---

## Input & Output

### Input
- A single line containing a Python-style list of integers representing token energy values.

#### Example:
```python
[3, 2, 5, 10, 7]
```

### Output
- A single integer representing the maximum energy that can be harnessed.

#### Example Output:
```python
15
```

---

## Example Walkthrough

### Example 1
**Input:**
```python
[3, 2, 5, 10, 7]
```
**Output:**
```python
15
```
**Explanation:**
- Best selection: `[3, 5, 7]`
- Sum: `3 + 5 + 7 = 15`

### Example 2
**Input:**
```python
[10, 18, 4, 7]
```
**Output:**
```python
25
```
**Explanation:**
- Best selection: `[18, 7]`
- Sum: `18 + 7 = 25`

---

## Solution Approach

This is a **classic dynamic programming problem** where we decide whether to take or skip each token.

### Strategy
1. **Edge Cases:**
   - If no tokens exist, the answer is `0`.
   - If only one token exists, the answer is the token's value.

2. **DP Formula:**
   - Define `dp[i]` as the max sum we can get using tokens up to index `i`.
   - The key decision:
     - Skip `tokens[i]` → take the previous max: `dp[i-1]`
     - Take `tokens[i]` → add it to `dp[i-2]`: `dp[i-2] + tokens[i]`
   - Formula:
     ```python
     dp[i] = max(dp[i-1], dp[i-2] + tokens[i])
     ```

---

## Python Implementation

```python
def max_energy(tokens):
    if not tokens:
        return 0
    if len(tokens) == 1:
        return tokens[0]
    
    prev_two, prev_one = 0, 0
    for token in tokens:
        prev_two, prev_one = prev_one, max(prev_one, prev_two + token)
    
    return prev_one

# Read input and compute result
tokens = eval(input().strip())
print(max_energy(tokens))
```

**Flag:** `HTB{SUMM0N3RS_INC4NT4T10N_R3S0LV3D_a9690689e4cda56f7f0c965a3c9cb246}`

