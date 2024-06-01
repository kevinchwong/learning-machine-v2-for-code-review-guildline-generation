# Knapsack Problem
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Knapsack Problem](#knapsack-problem) | 75 | 50 |
| 2 | [0/1 Knapsack](#0-1-knapsack) | 75 | 50 |
| 3 | [Super Egg Drop](#super-egg-drop) | 75 | 65 |
| 4 | [Subset Sum Problem](#subset-sum-problem) | 70 | 50 |
| 5 | [Stone Game III](#stone-game-iii) | 70 | 60 |
| 6 | [Target Sum](#target-sum) | 65 | 50 |
| 7 | [Integer Break](#integer-break) | 65 | 50 |
| 8 | [Unbounded Knapsack](#unbounded-knapsack) | 65 | 55 |
| 9 | [Stone Game II](#stone-game-ii) | 65 | 55 |
| 10 | [Stone Game IV](#stone-game-iv) | 65 | 55 |
| 11 | [Stone Game](#stone-game) | 60 | 50 |
| 12 | [Combination Sum IV](#combination-sum-iv) | 60 | 55 |
| 13 | [Rod Cutting](#rod-cutting) | 60 | 55 |
| 14 | [Minimum Subset Sum Difference](#minimum-subset-sum-difference) | 55 | 50 |
| 15 | [Minimum Cost to Cut a Stick](#minimum-cost-to-cut-a-stick) | 55 | 60 |
| 16 | [Last Stone Weight II](#last-stone-weight-ii) | 50 | 45 |
---
The Knapsack Problem is a combinatorial optimization problem where you have a set of items, each with a weight and a value, and a knapsack with a weight limit. The goal is to determine the number of each item to include in the knapsack so that the total weight is within the limit and the total value is maximized.
```python
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i-1][w], dp[i-1][w-weights[i-1]] + values[i-1])
            else:
                dp[i][w] = dp[i-1][w]
    return dp[n][capacity]

weights = [1, 2, 3, 4]
values = [10, 20, 30, 40]
capacity = 5
print(knapsack(weights, values, capacity))
```
### Insight:
1. The problem can be solved using dynamic programming by building a 2D array where the rows represent items and columns represent weight capacities.
2. The value at dp[i][w] represents the maximum value that can be obtained with the first i items and a weight limit of w.
3. The decision at each step is whether to include the current item in the knapsack or not, based on its weight and value.
4. This approach ensures that all possible combinations are considered, leading to an optimal solution.
---
 --- 
# Knapsack Problem
>Find the maximum value that can be obtained with a given weight capacity.

>Input: weights = [1, 3, 4, 5], values = [1, 4, 5, 7], W = 7
Output: 9
Explanation: The maximum value that can be obtained is 9 by selecting items with weights 3 and 4.
- https://leetcode.com/problems/knapsack-problem/
- Difficulty: 50
- Frequent: 75
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Knapsack Problem -- Shortest Solution:
```python
# Dynamic Programming Solution

def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [0] * (capacity + 1)
    
    for i in range(n):
        for w in range(capacity, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    
    return dp[capacity]

# Example Usage
weights = [1, 2, 3]
values = [6, 10, 12]
capacity = 5
print(knapsack(weights, values, capacity))
```
#### TC: O(n*capacity) **SC:** O(capacity

The code implements a dynamic programming solution to solve the Knapsack Problem efficiently. It
iterates through each item and each possible capacity, updating the maximum value that can be
achieved at each capacity. The time complexity is O(n*capacity) where n is the number of items and
capacity is the maximum capacity of the knapsack. The space complexity is O(capacity) to store the
values for each capacity. This approach optimally solves the Knapsack Problem by considering all
possible combinations and selecting the one with the maximum value.

---
### Knapsack Problem -- Best TC Solution:
```python
# Dynamic Programming Solution

def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i - 1] <= w:
                dp[i][w] = max(dp[i - 1][w], values[i - 1] + dp[i - 1][w - weights[i - 1]])
            else:
                dp[i][w] = dp[i - 1][w]
    
    return dp[n][capacity]

# Example Usage
weights = [1, 2, 3]
values = [6, 10, 12]
capacity = 5
print(knapsack(weights, values, capacity))
```
#### TC: O(n * capacity) **SC:** O(n * capacity)

The code implements a dynamic programming solution to solve the Knapsack Problem efficiently. It
uses a 2D array 'dp' to store the maximum value that can be achieved with different weights and
capacities. The time complexity is O(n * capacity) where n is the number of items and capacity is
the maximum weight the knapsack can hold. The space complexity is also O(n * capacity) due to the 2D
array. The code iterates through each item and capacity combination, updating the maximum value that
can be achieved. The final result is stored in dp[n][capacity].

---
---
---
 --- 
# 0/1 Knapsack
>Find the maximum value that can be obtained with a given weight capacity, with each item being taken or not taken.

>Input: weights = [1, 2, 3], values = [10, 15, 40], W = 6
Output: 55
Explanation: The maximum value that can be obtained is 55 by selecting items with weights 2 and 3.
- https://leetcode.com/problems/0-1-knapsack/
- Difficulty: 50
- Frequent: 75
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### 0/1 Knapsack -- Shortest Solution:
```python
# Function to solve the 0/1 Knapsack problem

def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [0] * (capacity + 1)
    
    for i in range(n):
        for w in range(capacity, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    
    return dp[capacity]

# Example Usage
weights = [1, 3, 4, 5]
values = [1, 4, 5, 7]
capacity = 7
print(knapsack(weights, values, capacity))
```
#### TC: O(n*capacity) **SC:** O(capacity

The code implements a dynamic programming solution to the 0/1 Knapsack problem. It iterates through
each item and each possible capacity, updating the maximum value that can be achieved at each
capacity. The time complexity is O(n*capacity) where n is the number of items and capacity is the
maximum capacity of the knapsack. The space complexity is O(capacity) to store the values in the dp
array. This approach efficiently solves the problem by considering all possible combinations and
selecting the optimal one.

---
### 0/1 Knapsack -- Best TC Solution:
```python
# Dynamic Programming Solution

def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i - 1] <= w:
                dp[i][w] = max(dp[i - 1][w], values[i - 1] + dp[i - 1][w - weights[i - 1]])
            else:
                dp[i][w] = dp[i - 1][w]
    
    return dp[n][capacity]

# Example Usage
weights = [1, 2, 3]
values = [6, 10, 12]
capacity = 5
print(knapsack(weights, values, capacity))
```
#### TC: O(n * capacity) **SC:** O(n * capacity)

The code implements a dynamic programming solution to solve the 0/1 Knapsack problem efficiently. It
uses a 2D array 'dp' to store the maximum value that can be achieved with different weights and
capacities. The time complexity is O(n * capacity) where 'n' is the number of items and 'capacity'
is the maximum weight the knapsack can hold. The space complexity is also O(n * capacity) due to the
2D array. The code iterates through each item and weight combination, updating the maximum value
that can be achieved. The final result is stored in dp[n][capacity], which represents the maximum
value that can be obtained within the given capacity.

---
---
---
 --- 
# Super Egg Drop
>Find the minimum number of attempts needed to find the critical floor in the worst case.

>Input: k = 2, n = 6
Output: 3
Explanation: The minimum number of attempts needed to find the critical floor is 3.
- https://leetcode.com/problems/super-egg-drop/
- Difficulty: 65
- Frequent: 75
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Super Egg Drop -- Shortest Solution:
```python
# Super Egg Drop

class Solution:
    def superEggDrop(self, K: int, N: int) -> int:
        dp = [[0] * (K + 1) for _ in range(N + 1)]
        m = 0
        while dp[m][K] < N:
            m += 1
            for i in range(1, K + 1):
                dp[m][i] = dp[m - 1][i - 1] + dp[m - 1][i] + 1
        return m
```
#### TC: O(K * N) **SC:** O(K * N)

The code uses dynamic programming to solve the Super Egg Drop problem efficiently. It creates a 2D
array 'dp' to store the minimum number of moves required to determine the critical floor with 'm'
moves and 'i' eggs. The loop iterates until the value in dp[m][K] is greater than or equal to N. The
formula dp[m][i] = dp[m - 1][i - 1] + dp[m - 1][i] + 1 is used to update the values in the dp array.
The time complexity is O(K * N) and the space complexity is also O(K * N).

---
---
---
 --- 
# Subset Sum Problem
>Determine if there is a subset of the given set with a sum equal to a given sum.

>Input: nums = [3, 34, 4, 12, 5, 2], sum = 9
Output: true
Explanation: There is a subset (4, 5) with sum 9.
- https://leetcode.com/problems/subset-sum-problem/
- Difficulty: 50
- Frequent: 70
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Subset Sum Problem -- Shortest Solution:
```python
# Subset Sum Problem

def subset_sum(nums, target):
    dp = [False] * (target + 1)
    dp[0] = True
    for num in nums:
        for i in range(target, num - 1, -1):
            dp[i] = dp[i] or dp[i - num]
    return dp[target]

# Example
nums = [2, 3, 7, 8]
target = 11
print(subset_sum(nums, target))  # Output: True
```
#### TC: Time Complexity: O(n * target) **SC:** Space Complexity: O(target)

The code uses dynamic programming to solve the Subset Sum Problem efficiently. It iterates through
the given nums array and updates the dp array to keep track of whether a sum equal to the target can
be achieved using the elements in the array. The time complexity is O(n * target) where n is the
number of elements in the nums array and the target is the target sum. The space complexity is
O(target) to store the dp array. The code is concise, clean, and easy to understand, making use of
dynamic programming to solve the problem efficiently.

---
### Subset Sum Problem -- Best TC Solution:
```python
# Function to check if there exists a subset with the given sum

def subset_sum(nums, target_sum):
    dp = [False] * (target_sum + 1)
    dp[0] = True
    for num in nums:
        for i in range(target_sum, num - 1, -1):
            dp[i] = dp[i] or dp[i - num]
    return dp[target_sum]

# Example
nums = [2, 3, 7, 8, 10]
target_sum = 11
print(subset_sum(nums, target_sum))  # Output: True
```
#### TC: O(n * target_sum) **SC:** O(target_sum)

The code uses dynamic programming to solve the Subset Sum Problem efficiently. It iterates through
the given numbers and updates the dp array to keep track of whether a subset with the target sum can
be formed. The time complexity is O(n * target_sum) where n is the number of elements in the input
array, and the space complexity is O(target_sum) to store the dp array. The code is clean, easy to
understand, and provides a smart solution to the problem.

---
---
---
 --- 
# Stone Game III
>Determine the winner of the game given the values of stones.

>Input: stoneValue = [1,2,3,7]
Output: Bob
Explanation: Bob will always win the game.
- https://leetcode.com/problems/stone-game-iii/
- Difficulty: 60
- Frequent: 70
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Stone Game III -- Shortest Solution:
```python
# Stone Game III

def stoneGameIII(stoneValue):
    n = len(stoneValue)
    dp = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        dp[i] = float('-inf')
        take = 0
        for k in range(3):
            if i + k < n:
                take += stoneValue[i + k]
                dp[i] = max(dp[i], take - dp[i + k + 1])
    if dp[0] > 0:
        return 'Alice'
    elif dp[0] < 0:
        return 'Bob'
    else:
        return 'Tie'

# Example
stoneValue = [-1, -2, -3]
print(stoneGameIII(stoneValue))
```
#### TC: Time Complexity: O(n) **SC:** Space Complexity: O(n)

The code uses dynamic programming to solve the Stone Game III problem efficiently. It iterates
through the stone values from the end to the beginning, calculating the maximum score difference
that can be achieved by both players. The 'dp' array stores the maximum score difference at each
position. By considering the choices of taking 1, 2, or 3 stones, the code calculates the optimal
strategy for each player. Finally, it determines the winner based on the maximum score difference at
the start position. The time complexity of the code is O(n) where n is the number of stones, and the
space complexity is also O(n) to store the dynamic programming array.

---
### Stone Game III -- Best TC Solution:
```python
# Stone Game III

def stoneGameIII(stoneValue):
    n = len(stoneValue)
    dp = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        dp[i] = float('-inf')
        take = 0
        for k in range(3):
            if i + k < n:
                take += stoneValue[i + k]
                dp[i] = max(dp[i], take - dp[i + k + 1])
    if dp[0] > 0:
        return 'Alice'
    elif dp[0] < 0:
        return 'Bob'
    else:
        return 'Tie'

# Example
stoneValue = [-1, -2, -3]
print(stoneGameIII(stoneValue))  # Output: 'Tie'
```
#### TC: O(n) **SC:** O(n)

The code uses dynamic programming to solve the Stone Game III problem. It iterates through the stone
values from the end to the beginning, calculating the maximum score difference between the players
at each step. The 'dp' array stores the maximum score difference that can be achieved starting from
each stone. By considering the next 3 stones at each step, the code calculates the optimal strategy
for both players. The time complexity is O(n) where n is the number of stones, and the space
complexity is also O(n) for the 'dp' array.

---
---
---
 --- 
# Target Sum
>Find the number of ways to assign symbols to make the sum of nums be a target value.

>Input: nums = [1,1,1,1,1], S = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
- https://leetcode.com/problems/target-sum/
- Difficulty: 50
- Frequent: 65
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Target Sum -- Shortest Solution:
```python
from collections import defaultdict
def findTargetSumWays(nums, S):
    dp = {0: 1}
    for num in nums:
        new_dp = defaultdict(int)
        for old_sum, count in dp.items():
            new_dp[old_sum + num] += count
            new_dp[old_sum - num] += count
        dp = new_dp
    return dp[S]
```
#### TC: O(n * sum) **SC:** O(sum)

The code uses a dynamic programming approach to solve the Target Sum problem. It iterates through
the input array of numbers and maintains a dictionary to store the count of ways to reach each sum.
The key insight is to update the dictionary for each number in the array by adding and subtracting
the number to the existing sums. This approach efficiently calculates the number of ways to reach
the target sum S. The time complexity is O(n * sum) where n is the number of elements in the input
array and sum is the total sum of the array. The space complexity is O(sum) to store the counts for
each sum.

---
---
---
 --- 
# Integer Break
>Break a positive integer into the sum of at least two positive integers to maximize the product of those integers.

>Input: n = 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
- https://leetcode.com/problems/integer-break/
- Difficulty: 50
- Frequent: 65
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Integer Break -- Shortest Solution:
```python
from math import pow

def integerBreak(n: int) -> int:
    if n == 2:
        return 1
    if n == 3:
        return 2
    if n % 3 == 0:
        return int(pow(3, n // 3))
    if n % 3 == 1:
        return int(pow(3, (n // 3) - 1) * 4)
    return int(pow(3, n // 3) * 2)
```
#### TC: O(1) **SC:** O(1)

The code calculates the maximum product that can be obtained by breaking an integer n into smaller
integers. It handles special cases for n=2 and n=3. For other values of n, it uses a formula based
on the remainder when dividing n by 3 to determine the optimal product. The code uses the math
library to calculate the power of 3 efficiently. The time and space complexity of the code is O(1),
as it does not depend on the input size.

---
### Integer Break -- Best TC Solution:
```python
# Approach: Dynamic Programming

def integerBreak(n):
    dp = [0] * (n + 1)
    dp[2] = 1
    for i in range(3, n + 1):
        for j in range(1, i):
            dp[i] = max(dp[i], max(j, dp[j]) * max(i - j, dp[i - j]))
    return dp[n]

# Test the function
n = 10
result = integerBreak(n)
print(result)
```
#### TC: Time Complexity: O(n^2) **SC:** Space Complexity: O(n)

The code uses dynamic programming to solve the Integer Break problem efficiently. It iterates
through the numbers from 3 to n, calculating the maximum product of breaking the number at different
points. The dp array stores the maximum product for each number up to n. By considering all possible
break points, the code determines the optimal solution. The time complexity is O(n^2) due to the
nested loops, and the space complexity is O(n) for the dp array storing the results for each number.

---
### Integer Break -- Best SC Solution:
```python
from math import pow

def integerBreak(n: int) -> int:
    if n == 2:
        return 1
    if n == 3:
        return 2
    if n % 3 == 0:
        return int(pow(3, n // 3))
    if n % 3 == 1:
        return 2 * 2 * int(pow(3, (n - 4) // 3))
    return 2 * int(pow(3, n // 3))
```
#### TC: O(1) **SC:** O(1

The code defines a function 'integerBreak' that calculates the maximum product of integers that sum
up to the given integer 'n'. It handles special cases for n=2 and n=3. The main logic is based on
breaking down the number into factors of 2 and 3 to maximize the product. The code uses the math
library to calculate the power of 3 efficiently. The time complexity is O(1) as the code directly
computes the result without any loops or recursion. The space complexity is also O(1) as it only
uses a constant amount of extra space.

---
---
---
 --- 
# Unbounded Knapsack
>Find the maximum value that can be obtained with a given weight capacity, with unlimited supply of each item.

>Input: weights = [2, 3, 4, 5], values = [3, 4, 5, 6], W = 8
Output: 10
Explanation: The maximum value that can be obtained is 10 by selecting items with weights 3 and 5.
- https://leetcode.com/problems/unbounded-knapsack/
- Difficulty: 55
- Frequent: 65
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Unbounded Knapsack -- Shortest Solution:
```python
# Unbounded Knapsack Problem

def unboundedKnapsack(W, n, val, wt):
    dp = [0] * (W + 1)
    for i in range(W + 1):
        for j in range(n):
            if wt[j] <= i:
                dp[i] = max(dp[i], dp[i - wt[j]] + val[j])
    return dp[W]

# Example Usage
W = 10
val = [10, 40, 50, 70]
wt = [1, 3, 4, 5]
n = len(val)
result = unboundedKnapsack(W, n, val, wt)
print(result)
```
#### TC: Time Complexity: O(W*n) **SC:** Space Complexity: O(W)

The code implements the Unbounded Knapsack Problem using dynamic programming. It iterates through
the weight values and items to calculate the maximum value that can be obtained. The 'dp' array
stores the maximum value achievable for each weight. By updating the 'dp' array iteratively, the
function returns the maximum value that can be achieved with the given weight capacity and items.
The time complexity is O(W*n) where W is the weight capacity and n is the number of items, and the
space complexity is O(W) for the 'dp' array.

---
### Unbounded Knapsack -- Best TC Solution:
```python
# Unbounded Knapsack Problem

def unbounded_knapsack(weights, values, capacity):
    dp = [0] * (capacity + 1)
    for i in range(1, capacity + 1):
        for j in range(len(weights)):
            if weights[j] <= i:
                dp[i] = max(dp[i], dp[i - weights[j]] + values[j])
    return dp[capacity]

# Example
weights = [2, 3, 4]
values = [3, 4, 5]
capacity = 5
print(unbounded_knapsack(weights, values, capacity))
```
#### TC: O(N*W) **SC:** O(W)

The code implements the Unbounded Knapsack Problem using dynamic programming. It iterates through
each weight and capacity combination to calculate the maximum value that can be achieved. The time
complexity is O(N*W) where N is the number of weights and W is the capacity. The space complexity is
O(W) to store the dynamic programming array. The code efficiently calculates the maximum value that
can be obtained within the given capacity by considering all possible combinations of weights and
values.

---
---
---
 --- 
# Stone Game II
>Determine the maximum number of stones the first player can get given the piles of stones.

>Input: piles = [2,7,9,4,4]
Output: 10
Explanation: The maximum number of stones Alex can get is 10.
- https://leetcode.com/problems/stone-game-ii/
- Difficulty: 55
- Frequent: 65
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Stone Game II -- Shortest Solution:
```python
from functools import lru_cache
def stoneGameII(piles):
    n = len(piles)
    prefix_sum = [0] + list(itertools.accumulate(piles))
    @lru_cache(None)
    def dp(i, M):
        if i == n:
            return 0
        return prefix_sum[n] - dp(i + M, max(M, j)) for j in range(1, 2 * M + 1))
    return dp(0, 1)
```
#### TC: O(N^2) **SC:** O(N^2)

The code uses dynamic programming with memoization to solve the Stone Game II problem efficiently.
It calculates the prefix sum of the piles and recursively calculates the maximum number of stones
Alex can get. The use of memoization helps avoid redundant calculations, improving the overall time
complexity. The time complexity is O(N^2) due to the nested loops, and the space complexity is also
O(N^2) due to the memoization cache storing subproblem solutions.

---
### Stone Game II -- Best TC Solution:
```python
# Stone Game II

def stoneGameII(piles):
    n = len(piles)
    suffix_sum = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suffix_sum[i] = suffix_sum[i + 1] + piles[i]
    dp = [[0] * (n + 1) for _ in range(n)]
    for i in range(n - 1, -1, -1):
        for m in range(1, n + 1):
            x = 0
            for x in range(1, 2 * m + 1):
                if i + x <= n:
                    dp[i][m] = max(dp[i][m], suffix_sum[i] - dp[i + x][max(m, x)])
    return dp[0][1]

# Example
piles = [2, 7, 9, 4, 4]
print(stoneGameII(piles))
```
#### TC: Time Complexity: O(n^2) **SC:** Space Complexity: O(n^2)

The code uses dynamic programming to solve the Stone Game II problem efficiently. It calculates the
suffix sum of the piles and then iterates through the piles to determine the maximum number of
stones that can be obtained. The nested loops help in considering different scenarios and making
optimal choices. The use of a 2D array 'dp' helps in storing and accessing the intermediate results.
Overall, the code is clean, easy to understand, and provides the optimal solution with a time
complexity of O(n^2) and space complexity of O(n^2).

---
---
---
 --- 
# Stone Game IV
>Determine if the first player can always win the game given the number of stones.

>Input: n = 7
Output: true
Explanation: Alice can always win the game.
- https://leetcode.com/problems/stone-game-iv/
- Difficulty: 55
- Frequent: 65
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Stone Game IV -- Shortest Solution:
```python
class Solution:
    def winnerSquareGame(self, n: int) -> bool:
        dp = [False] * (n + 1)
        for i in range(1, n + 1):
            for k in range(1, int(i ** 0.5) + 1):
                if not dp[i - k * k]:
                    dp[i] = True
                    break
        return dp[n]
```
#### TC: O(n*sqrt(n)) **SC:** O(n)

The code uses dynamic programming to solve the problem efficiently. It iterates through all numbers
from 1 to n and for each number, it checks if there exists a square number k*k such that subtracting
it from the current number results in a losing position. If such a square number is found, the
current number is marked as a winning position. The algorithm utilizes a dynamic programming array
dp to store the winning/losing status for each number up to n. The time complexity is O(n*sqrt(n))
due to the nested loops, and the space complexity is O(n) to store the dp array.

---
---
---
 --- 
# Stone Game
>Determine if the first player can always win the game given the piles of stones.

>Input: piles = [5,3,4,5]
Output: true
Explanation: Alex can always win the game.
- https://leetcode.com/problems/stone-game/
- Difficulty: 50
- Frequent: 60
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Stone Game -- Shortest Solution:
```python
from typing import List
def stoneGame(piles: List[int]) -> bool:
    return True
```
#### TC: O(1) **SC:** O(1

The solution for the Stone Game problem is a simple one-liner function that always returns True.
This is because the problem statement guarantees that the number of piles is even and the sum of the
stones in the piles is odd, ensuring that one player will always have more stones than the other.
Therefore, the player who plays optimally will always win. The time complexity of this solution is
O(1) as it does not depend on the size of the input, and the space complexity is also O(1) as no
extra space is used.

---
### Stone Game -- Best TC Solution:
```python
from typing import List
def stoneGame(piles: List[int]) -> bool:
    n = len(piles)
    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = piles[i]
    for d in range(1, n):
        for i in range(n - d):
            dp[i][i + d] = max(piles[i] - dp[i + 1][i + d], piles[i + d] - dp[i][i + d - 1])
    return dp[0][-1] > 0

# Example
piles = [5, 3, 4, 5]
print(stoneGame(piles))  # Output: True
```
#### TC: Time Complexity: O(n^2) **SC:** Space Complexity: O(n^2)

The code uses dynamic programming to solve the Stone Game problem. It creates a 2D DP array to store
the maximum number of stones Alex can get more than Lee by choosing from a range of piles. The
algorithm iterates over all possible ranges of piles and calculates the maximum stones Alex can get
by choosing the optimal strategy. Finally, it returns whether Alex can get more stones than Lee. The
time complexity of the solution is O(n^2) due to the nested loops, and the space complexity is also
O(n^2) to store the DP array.

---
### Stone Game -- Best SC Solution:
```python
from typing import List

def stoneGame(piles: List[int]) -> bool:
    n = len(piles)
    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = piles[i]
    for d in range(1, n):
        for i in range(n - d):
            dp[i][i + d] = max(piles[i] - dp[i + 1][i + d], piles[i + d] - dp[i][i + d - 1])
    return dp[0][-1] > 0
```
#### TC: O(n^2) **SC:** O(n^2)

The code uses dynamic programming to solve the Stone Game problem. It creates a 2D array 'dp' to
store the maximum number of stones Alex can get more than Lee by choosing from the range of piles.
The algorithm iterates over the array to calculate the maximum difference for each subarray.
Finally, it returns whether Alex can get more stones than Lee by checking the value at dp[0][-1].
The time complexity is O(n^2) due to the nested loops, and the space complexity is also O(n^2) to
store the DP table.

---
---
---
 --- 
# Combination Sum IV
>Count the number of possible combinations that add up to a given target.

>Input: nums = [1,2,3], target = 4
Output: 7
Explanation: The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 2)
(3, 1)
(2, 1, 1)
- https://leetcode.com/problems/combination-sum-iv/
- Difficulty: 55
- Frequent: 60
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Combination Sum IV -- Shortest Solution:
```python
from typing import List

class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0] * (target + 1)
        dp[0] = 1
        for i in range(1, target + 1):
            for num in nums:
                if i - num >= 0:
                    dp[i] += dp[i - num]
        return dp[target]
```
#### TC: O(target * len(nums)) **SC:** O(target)

The code uses dynamic programming to solve the Combination Sum IV problem efficiently. It
initializes a DP array to store the number of combinations for each target sum. By iterating through
the target sums and the given numbers, it calculates the number of combinations by adding the
combinations for the current sum minus each number. This approach optimizes the solution by avoiding
redundant calculations and achieves a time complexity of O(target * len(nums)) and a space
complexity of O(target).

---
### Combination Sum IV -- Best TC Solution:
```python
# Dynamic Programming Solution

def combinationSum4(nums, target):
    dp = [0] * (target + 1)
    dp[0] = 1
    for i in range(1, target + 1):
        for num in nums:
            if i - num >= 0:
                dp[i] += dp[i - num]
    return dp[target]

# Example
nums = [1, 2, 3]
target = 4
print(combinationSum4(nums, target))
```
#### TC: O(n * target) **SC:** O(target)

The code uses dynamic programming to solve the Combination Sum IV problem efficiently. It
initializes a DP array to store the number of combinations for each target sum. By iterating through
the target sums and the given numbers, it calculates the number of combinations for each sum. The
time complexity is O(n * target) where n is the number of elements in the input array, and the space
complexity is O(target) to store the DP array. This approach effectively solves the problem by
building up the solutions for smaller subproblems to reach the final target sum.

---
---
---
 --- 
# Rod Cutting
>Find the maximum obtainable value by cutting up the rod and selling the pieces.

>Input: prices = [1, 5, 8, 9], n = 4
Output: 10
Explanation: The maximum obtainable value is 10 by cutting the rod into two pieces of lengths 2 and 2.
- https://leetcode.com/problems/rod-cutting/
- Difficulty: 55
- Frequent: 60
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Rod Cutting -- Shortest Solution:
```python
# Dynamic Programming Solution

def rod_cutting(price):
    n = len(price)
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        for j in range(i):
            dp[i] = max(dp[i], price[j] + dp[i - j - 1])
    return dp[n]

# Example
price = [1, 5, 8, 9, 10, 17, 17, 20]
result = rod_cutting(price)
print(result)
```
#### TC: O(n^2) **SC:** O(n)

The code implements a dynamic programming solution to solve the Rod Cutting problem efficiently. It
iterates through the lengths of the rod and calculates the maximum profit that can be obtained by
cutting the rod at different positions. The time complexity of the solution is O(n^2) where n is the
length of the rod, and the space complexity is O(n) to store the DP array. By using dynamic
programming, we avoid recalculating subproblems and optimize the solution for finding the maximum
profit.

---
### Rod Cutting -- Best TC Solution:
```python
# Dynamic Programming Approach

def rod_cutting(price):
    n = len(price)
    dp = [0] * (n + 1)
    
    for i in range(1, n + 1):
        max_val = float('-inf')
        for j in range(i):
            max_val = max(max_val, price[j] + dp[i - j - 1])
        dp[i] = max_val
    
    return dp[n]

# Example
price = [1, 5, 8, 9, 10, 17, 17, 20]
result = rod_cutting(price)
print(result)  # Output: 22
```
#### TC: Time Complexity: O(n^2) **SC:** Space Complexity: O(n)

The code uses dynamic programming to solve the rod cutting problem efficiently. It iterates through
the rod lengths and calculates the maximum value that can be obtained by cutting the rod at
different positions. The time complexity is O(n^2) because of the nested loops, and the space
complexity is O(n) to store the DP array. The code is clean, easy to understand, and provides the
maximum value that can be obtained by cutting the rod according to the given prices.

---
---
---
 --- 
# Minimum Subset Sum Difference
>Partition the array into two subsets such that the difference of subset sums is minimized.

>Input: nums = [1, 6, 11, 5]
Output: 1
Explanation: The array can be partitioned as [1, 6, 5] and [11].
- https://leetcode.com/problems/minimum-subset-sum-difference/
- Difficulty: 50
- Frequent: 55
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Minimum Subset Sum Difference -- Shortest Solution:
```python
from itertools import combinations

def min_subset_sum_diff(nums):
    total_sum = sum(nums)
    min_diff = float('inf')
    for i in range(1, len(nums) // 2 + 1):
        for subset in combinations(nums, i):
            subset_sum = sum(subset)
            min_diff = min(min_diff, abs(total_sum - 2 * subset_sum))
    return min_diff

# Example
nums = [1, 2, 3, 9]
print(min_subset_sum_diff(nums)  # Output: 3
```
#### TC: O(2^N) **SC:** O(1)

The code calculates the total sum of the input array and then iterates through all possible subsets
using combinations. For each subset, it calculates the subset sum and updates the minimum difference
between the total sum and twice the subset sum. The final result is the minimum subset sum
difference. The time complexity is O(2^N) due to the exponential number of subsets, and the space
complexity is O(1) as no extra space is used apart from some variables.

---
### Minimum Subset Sum Difference -- Best TC Solution:
```python
from typing import List
def min_subset_sum_diff(nums: List[int]) -> int:
    total_sum = sum(nums)
    n = len(nums)
    dp = [[False for _ in range(total_sum // 2 + 1)] for _ in range(n + 1)]
    for i in range(n + 1):
        dp[i][0] = True
    for i in range(1, n + 1):
        for j in range(1, total_sum // 2 + 1):
            if nums[i - 1] <= j:
                dp[i][j] = dp[i - 1][j] or dp[i - 1][j - nums[i - 1]]
            else:
                dp[i][j] = dp[i - 1][j]
    for j in range(total_sum // 2, -1, -1):
        if dp[n][j]:
            return total_sum - 2 * j
    return -1

# Example
nums = [1, 2, 3, 9]
print(min_subset_sum_diff(nums))  # Output: 3
```
#### TC: O(n * sum(nums)) **SC:** O(n * sum(nums))

The code uses the Knapsack Problem approach to find the minimum subset sum difference. It creates a
2D DP array to store the subset sum possibilities. The time complexity is O(n * sum(nums)) where n
is the number of elements in the input list and sum(nums) is the total sum of the elements. The
space complexity is also O(n * sum(nums)) due to the DP array. The code efficiently calculates the
minimum subset sum difference by iterating through the DP array and finding the optimal solution.

---
---
---
 --- 
# Minimum Cost to Cut a Stick
>Find the minimum cost to cut a stick into given lengths.

>Input: n = 7, cuts = [1,3,4,5]
Output: 16
Explanation: The minimum cost to cut the stick is 16.
- https://leetcode.com/problems/minimum-cost-to-cut-a-stick/
- Difficulty: 60
- Frequent: 55
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Minimum Cost to Cut a Stick -- Shortest Solution:
```python
from functools import lru_cache
def minCost(self, n: int, cuts: List[int]) -> int:
    cuts.sort()
    cuts = [0] + cuts + [n]
    @lru_cache(None)
    def dp(i, j):
        if j - i <= 1:
            return 0
        return cuts[j] - cuts[i] + min(dp(i, k) + dp(k, j) for k in range(i + 1, j))
    return dp(0, len(cuts) - 1)
```
#### TC: O(n^3) **SC:** O(n^2)

The code uses dynamic programming with memoization to solve the problem efficiently. It sorts the
cuts array and adds the start and end points to it. The dp function calculates the minimum cost
recursively by considering all possible cuts. The @lru_cache decorator helps in memoizing the
results to avoid redundant calculations. The time complexity is O(n^3) due to the nested loops in
the dp function, and the space complexity is O(n^2) due to the memoization table.

---
---
---
 --- 
# Last Stone Weight II
>Find the smallest possible weight of the left stone after smashing the stones together.

>Input: stones = [2,7,4,1,8,1]
Output: 1
Explanation: The smallest possible weight of the left stone is 1.
- https://leetcode.com/problems/last-stone-weight-ii/
- Difficulty: 45
- Frequent: 50
- Tags: ['Dynamic Programming', 'Knapsack Problem']

### Last Stone Weight II -- Shortest Solution:
```python
from typing import List
def lastStoneWeightII(stones: List[int]) -> int:
    total_sum = sum(stones)
    target = total_sum // 2
    dp = [0] * (target + 1)
    for stone in stones:
        for i in range(target, stone - 1, -1):
            dp[i] = max(dp[i], dp[i - stone] + stone)
    return total_sum - 2 * dp[target]
```
#### TC: O(n * m) **SC:** O(m)

The code uses the Knapsack Problem approach to find the minimum difference between two subsets of
stones. It calculates the total sum of stones and then iterates through each stone to update the dp
array. The dp array stores the maximum possible sum that can be achieved with a subset of stones.
Finally, it returns the difference between the total sum and twice the maximum sum achievable with a
subset that is closest to half of the total sum.

---
---
---