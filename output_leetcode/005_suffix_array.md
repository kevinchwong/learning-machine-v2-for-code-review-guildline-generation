# Suffix Array
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [K-th Smallest in Lexicographical Order](#k-th-smallest-in-lexicographical-order) | 50 | 65 |
---
A suffix array is a sorted array of all suffixes of a given string. It is used in various string processing problems like substring search, pattern matching, and longest repeated substring. The suffix array is constructed by sorting the suffixes of the string in lexicographical order and storing their starting indices.
```python
def build_suffix_array(s):
    suffixes = [(s[i:], i) for i in range(len(s))]
    suffixes.sort()
    suffix_array = [suffix[1] for suffix in suffixes]
    return suffix_array

# Example usage
s = "banana"
print(build_suffix_array(s))  # Output: [5, 3, 1, 0, 4, 2]
```
### Insight:
1. The suffix array is useful for efficient pattern matching and substring search.
2. The naive approach involves generating all suffixes and sorting them, which is simple but not the most efficient.
3. More advanced algorithms like the DC3 algorithm can construct the suffix array in linear time.
4. Understanding the suffix array helps in solving complex string problems like the longest common prefix (LCP) array.
---
 --- 
# K-th Smallest in Lexicographical Order
>Find the k-th smallest number in lexicographical order using Suffix Array.

>Input: n = 13, k = 2
Output: 10
- https://leetcode.com/problems/k-th-smallest-in-lexicographical-order/
- Difficulty: 65
- Frequent: 50
- Tags: ['String', 'Suffix Array']

### K-th Smallest in Lexicographical Order -- Shortest Solution:
```python
from typing import List
class Solution:
    def findKthNumber(self, n: int, k: int) -> int:
        def count_steps(prefix: int, n: int) -> int:
            curr = prefix
            next = curr + 1
            steps = 0
            while curr <= n:
                steps += min(n + 1, next) - curr
                curr *= 10
                next *= 10
            return steps
        prefix = 1
        k -= 1
        while k > 0:
            steps = count_steps(prefix, n)
            if steps <= k:
                prefix += 1
                k -= steps
            else:
                prefix *= 10
                k -= 1
        return prefix
```
#### TC: O(log(n)) **SC:** O(1)

The code uses a clever approach to find the k-th smallest number in lexicographical order. It
iterates through the numbers in lexicographical order, counting the steps needed to reach the next
number. By adjusting the prefix based on the steps counted, it efficiently finds the k-th number.
The time complexity is O(log(n)) as it iterates through the numbers logarithmically, and the space
complexity is O(1) as it only uses a constant amount of extra space.

---
---
---