# Boyer-Moore Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Majority Element](#majority-element) | 80 | 30 |
| 2 | [Majority Element II](#majority-element-ii) | 70 | 40 |
---
The Boyer-Moore algorithm is an efficient string-searching algorithm that skips sections of the text to be searched, rather than checking every character. It uses two heuristics: the bad character rule and the good suffix rule, to determine how far to skip ahead. This makes it particularly efficient for large texts.
```python
def boyer_moore(text, pattern):
    m = len(pattern)
    n = len(text)

    if m == 0:
        return 0

    bad_char = [-1] * 256

    for i in range(m):
        bad_char[ord(pattern[i])] = i

    s = 0
    while s <= n - m:
        j = m - 1

        while j >= 0 and pattern[j] == text[s + j]:
            j -= 1

        if j < 0:
            print(f"Pattern occurs at shift = {s}")
            s += (m - bad_char[ord(text[s + m])] if s + m < n else 1)
        else:
            s += max(1, j - bad_char[ord(text[s + j])])

text = "ABAAABCD"
pattern = "ABC"
boyer_moore(text, pattern)
```
### Insight:
The Boyer-Moore algorithm is highly efficient for large texts and patterns. The bad character rule allows the algorithm to skip sections of the text where mismatches occur, while the good suffix rule helps in further optimizing the shifts. The preprocessing step for the bad character heuristic takes O(m) time, where m is the length of the pattern. The algorithm performs best when the alphabet size is large and the pattern length is relatively small compared to the text length.
---
 --- 
# Majority Element
>Find the majority element in an array of size n.

>Input: [3,2,3]
Output: 3
- https://leetcode.com/problems/majority-element/
- Difficulty: 30
- Frequent: 80
- Tags: ['Array', 'Divide and Conquer', 'Bit Manipulation', 'Boyer-Moore Voting Algorithm']

### Majority Element -- Shortest Solution:
```python
from typing import List

def majorityElement(nums: List[int]) -> int:
    count = 0
    candidate = None
    for num in nums:
        if count == 0:
            candidate = num
        count += 1 if num == candidate else -1
    return candidate

# Example usage
nums = [3, 2, 3]
print(majorityElement(nums))  # Output: 3
```
#### TC: O(n) **SC:** O(1)

The code uses the Boyer-Moore Algorithm to find the majority element in an array. It iterates
through the array once, maintaining a count of the current candidate element. If the count reaches
0, it updates the candidate to the current element. The algorithm effectively cancels out pairs of
different elements and keeps track of the majority element. The time complexity is O(n) as it only
requires a single pass through the array, and the space complexity is O(1) as it uses constant extra
space.

---
---
---
 --- 
# Majority Element II
>Find all elements that appear more than n/3 times in an array of size n.

>Input: [3,2,3]
Output: [3]
- https://leetcode.com/problems/majority-element-ii/
- Difficulty: 40
- Frequent: 70
- Tags: ['Array', 'Boyer-Moore Voting Algorithm']

### Majority Element II -- Shortest Solution:
```python
from typing import List
def majorityElement(nums: List[int]) -> List[int]:
    count1, count2, candidate1, candidate2 = 0, 0, 0, 1
    for num in nums:
        if num == candidate1:
            count1 += 1
        elif num == candidate2:
            count2 += 1
        elif count1 == 0:
            candidate1, count1 = num, 1
        elif count2 == 0:
            candidate2, count2 = num, 1
        else:
            count1, count2 = count1 - 1, count2 - 1
    return [n for n in (candidate1, candidate2) if nums.count(n) > len(nums) // 3]
```
#### TC: O(n) **SC:** O(1)

The code uses the Boyer-Moore Algorithm to find the majority elements in the given list. It iterates
through the list once, updating the counts and candidates based on the current element. The final
step filters out the candidates that appear more than n/3 times in the list. The time complexity is
O(n) as it only requires a single pass through the list, and the space complexity is O(1) as it uses
a constant amount of extra space for variables.

---
---
---