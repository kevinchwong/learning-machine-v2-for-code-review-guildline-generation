# Bit Manipulation
## Frequent score for this algorithmic framework: 15 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Single Number](#single-number) | 80 | 30 |
| 2 | [Missing Number](#missing-number) | 70 | 35 |
| 3 | [Sum of Two Integers](#sum-of-two-integers) | 65 | 50 |
| 4 | [Number of 1 Bits](#number-of-1-bits) | 60 | 20 |
| 5 | [Hamming Distance](#hamming-distance) | 60 | 25 |
| 6 | [Single Number II](#single-number-ii) | 60 | 45 |
| 7 | [Reverse Bits](#reverse-bits) | 55 | 30 |
| 8 | [Find the Difference](#find-the-difference) | 55 | 30 |
| 9 | [Single Number III](#single-number-iii) | 55 | 50 |
| 10 | [Power of Two](#power-of-two) | 50 | 20 |
| 11 | [Counting Bits](#counting-bits) | 50 | 40 |
| 12 | [Repeated DNA Sequences](#repeated-dna-sequences) | 50 | 40 |
| 13 | [Total Hamming Distance](#total-hamming-distance) | 50 | 55 |
| 14 | [Binary Watch](#binary-watch) | 45 | 35 |
| 15 | [Bitwise AND of Numbers Range](#bitwise-and-of-numbers-range) | 45 | 55 |
| 16 | [UTF-8 Validation](#utf-8-validation) | 45 | 60 |
---
Bit manipulation involves directly operating on binary digits (bits) of integers. It is used for tasks like setting, clearing, flipping, and checking bits. This technique is efficient for low-level programming, cryptography, and optimizing performance-critical code.
```python
# Check if the ith bit is set
n = 5  # 0101 in binary
i = 2
is_set = (n & (1 << i)) != 0
print(is_set)  # True

# Set the ith bit
n = 5  # 0101 in binary
i = 1
n |= (1 << i)
print(bin(n))  # 0b111

# Clear the ith bit
n = 5  # 0101 in binary
i = 2
n &= ~(1 << i)
print(bin(n))  # 0b1

# Toggle the ith bit
n = 5  # 0101 in binary
i = 1
n ^= (1 << i)
print(bin(n))  # 0b111

# Count the number of set bits
n = 5  # 0101 in binary
count = 0
while n:
    count += n & 1
    n >>= 1
print(count)  # 2
```
### Insight:
Bit manipulation is highly efficient for low-level operations and can significantly optimize performance. Understanding binary representation and bitwise operators (AND, OR, XOR, NOT, shifts) is crucial. These operations are constant time (O(1)) for individual bits, making them very fast. However, when iterating over all bits of an integer, the complexity becomes O(log n), where n is the integer value. Bit manipulation is widely used in systems programming, cryptography, and performance-critical applications.
---
 --- 
# Single Number
>Find the element that appears only once in a non-empty array of integers.

>Input: [2,2,1]
Output: 1
- https://leetcode.com/problems/single-number/
- Difficulty: 30
- Frequent: 80
- Tags: ['Bit Manipulation']

### Single Number -- Shortest Solution:
```python
def singleNumber(nums):
    result = 0
    for num in nums:
        result ^= num
    return result
```
#### TC: O(n) **SC:** O(1)

The code uses the XOR bitwise operation to find the single number in the list. XOR-ing a number with
itself results in 0, and XOR-ing a number with 0 results in the number itself. By XOR-ing all the
numbers in the list, pairs of identical numbers cancel each other out, leaving only the single
number.

---
---
---
 --- 
# Missing Number
>Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

>Input: [3,0,1]
Output: 2
- https://leetcode.com/problems/missing-number/
- Difficulty: 35
- Frequent: 70
- Tags: ['Bit Manipulation']

### Missing Number -- Shortest Solution:
```python
missing_number = lambda nums: len(nums) * (len(nums) + 1) // 2 - sum(nums)
```
#### TC: O(n) **SC:** O(1)

The code uses a mathematical approach to find the missing number in an array of integers from 0 to
n. It calculates the expected sum of the first n natural numbers using the formula n * (n + 1) / 2
and subtracts the actual sum of the numbers in the array. The difference is the missing number. This
approach is efficient with a time complexity of O(n) and a space complexity of O(1).

---
---
---
 --- 
# Sum of Two Integers
>Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

>Input: a = 1, b = 2
Output: 3
- https://leetcode.com/problems/sum-of-two-integers/
- Difficulty: 50
- Frequent: 65
- Tags: ['Bit Manipulation']

### Sum of Two Integers -- Shortest Solution:
```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        MAX = 0x7FFFFFFF
        mask = 0xFFFFFFFF
        while b != 0:
            a, b = (a ^ b) & mask, ((a & b) << 1) & mask
        return a if a <= MAX else ~(a ^ mask)
```
#### TC: O(1) **SC:** O(1)

The code uses bitwise operations to sum two integers without using the '+' operator. It employs a
loop to handle the carry until there is no carry left. The mask ensures the results fit within 32
bits, and the final return statement adjusts for negative results.

---
---
---
 --- 
# Number of 1 Bits
>Write a function that takes an unsigned integer and returns the number of '1' bits it has.

>Input: 00000000000000000000000000001011
Output: 3
- https://leetcode.com/problems/number-of-1-bits/
- Difficulty: 20
- Frequent: 60
- Tags: ['Bit Manipulation']

### Number of 1 Bits -- Shortest Solution:
```python
def hammingWeight(n: int) -> int:
    return bin(n).count('1')
```
#### TC: O(1) **SC:** O(1)

The function `hammingWeight` converts the integer `n` to its binary representation using `bin(n)`,
which returns a string. The `count('1')` method then counts the number of '1's in this binary
string, which corresponds to the number of 1 bits in the integer. This approach leverages Python's
built-in functions to achieve a concise and efficient solution.

---
---
---
 --- 
# Hamming Distance
>Calculate the Hamming distance between two integers.

>Input: x = 1, y = 4
Output: 2
- https://leetcode.com/problems/hamming-distance/
- Difficulty: 25
- Frequent: 60
- Tags: ['Bit Manipulation']

### Hamming Distance -- Shortest Solution:
```python
bin(x ^ y).count('1')
```
#### TC: O(1) **SC:** O(1)

The code calculates the Hamming distance between two integers x and y. It uses the XOR operation (x
^ y) to find the bits that are different between x and y. The result of the XOR operation is then
converted to a binary string using bin(), and the count() method is used to count the number of '1's
in the binary string, which represents the Hamming distance.

---
---
---
 --- 
# Single Number II
>Find the element that appears only once in an array where every other element appears three times.

>Input: [0,1,0,1,0,1,99]
Output: 99
- https://leetcode.com/problems/single-number-ii/
- Difficulty: 45
- Frequent: 60
- Tags: ['Bit Manipulation']

### Single Number II -- Shortest Solution:
```python
from collections import Counter

def singleNumber(nums):
    return [num for num, count in Counter(nums).items() if count == 1][0]
```
#### TC: O(n) **SC:** O(n)

The solution uses Python's Counter from the collections module to count the occurrences of each
number in the list. It then iterates through the counted items and returns the number that appears
exactly once. This approach is both clean and easy to understand.

---
### Single Number II -- Best SC Solution:
```python
from collections import Counter
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        count = Counter(nums)
        for key, value in count.items():
            if value == 1:
                return key
        return -1
```
#### TC: O(N) **SC:** O(N)

The code uses the Counter class from the collections module to count the occurrences of each number
in the input list. It then iterates through the key-value pairs in the Counter object and returns
the key (number) with a count of 1, which represents the single number that appears only once. If no
such number is found, it returns -1. The time complexity is O(N) due to iterating through the list
and creating the Counter object, and the space complexity is also O(N) due to storing the counts in
the Counter object.

---
---
---
 --- 
# Reverse Bits
>Reverse bits of a given 32 bits unsigned integer.

>Input: 00000010100101000001111010011100
Output: 964176192
- https://leetcode.com/problems/reverse-bits/
- Difficulty: 30
- Frequent: 55
- Tags: ['Bit Manipulation']

### Reverse Bits -- Shortest Solution:
```python
class Solution:
    def reverseBits(self, n: int) -> int:
        return int('{:032b}'.format(n)[::-1], 2)
```
#### TC: O(1) **SC:** O(1)

The code converts the integer to a 32-bit binary string, reverses the string, and then converts it
back to an integer. This approach leverages Python's string manipulation capabilities to achieve the
desired result in a concise manner.

---
---
---
 --- 
# Find the Difference
>You are given two strings s and t which consist of only lowercase letters. String t is generated by random shuffling string s and then add one more letter at a random position. Find the letter that was added in t.

>Input: s = "abcd", t = "abcde"
Output: "e"
- https://leetcode.com/problems/find-the-difference/
- Difficulty: 30
- Frequent: 55
- Tags: ['Bit Manipulation']

### Find the Difference -- Shortest Solution:
```python
from collections import Counter

def findTheDifference(s: str, t: str) -> str:
    return (Counter(t) - Counter(s)).popitem()[0]
```
#### TC: O(n) **SC:** O(1)

The function uses the Counter class from the collections module to count the frequency of each
character in both strings. By subtracting the Counter of string s from the Counter of string t, we
get a Counter object that contains only the extra character in t. The popitem() method is then used
to retrieve this extra character.

---
---
---
 --- 
# Single Number III
>Find the two elements that appear only once in an array where every other element appears twice.

>Input: [1,2,1,3,2,5]
Output: [3,5]
- https://leetcode.com/problems/single-number-iii/
- Difficulty: 50
- Frequent: 55
- Tags: ['Bit Manipulation']

### Single Number III -- Shortest Solution:
```python
from functools import reduce

def singleNumber(nums):
    xor = reduce(lambda x, y: x ^ y, nums)
    diff = xor & -xor
    res = [0, 0]
    for num in nums:
        if num & diff:
            res[0] ^= num
        else:
            res[1] ^= num
    return res
```
#### TC: O(n) **SC:** O(1)

1. **XOR Operation**: The XOR of all numbers gives us the XOR of the two unique numbers because
duplicates cancel out. 2. **Finding a Set Bit**: `xor & -xor` isolates the rightmost set bit, which
helps in distinguishing the two unique numbers. 3. **Partitioning**: Using the isolated bit,
partition the numbers into two groups and XOR each group to find the unique numbers.

---
---
---
 --- 
# Power of Two
>Given an integer n, return true if it is a power of two. Otherwise, return false.

>Input: 1
Output: true
- https://leetcode.com/problems/power-of-two/
- Difficulty: 20
- Frequent: 50
- Tags: ['Bit Manipulation']

### Power of Two -- Shortest Solution:
```python
# Bit Manipulation Approach

class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        return n > 0 and (n & (n - 1)) == 0
```
#### TC: O(1) **SC:** O(1)

The code uses a bit manipulation approach to determine if a number is a power of two. It checks if
the number is greater than 0 and if the bitwise AND of the number and the number minus one is equal
to 0. This approach efficiently handles the power of two check in constant time complexity and
constant space complexity.

---
---
---
 --- 
# Counting Bits
>Given a non-negative integer num, return an array of the number of 1's in the binary representation of every number in the range 0 to num.

>Input: 2
Output: [0,1,1]
- https://leetcode.com/problems/counting-bits/
- Difficulty: 40
- Frequent: 50
- Tags: ['Bit Manipulation']

### Counting Bits -- Shortest Solution:
```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        return [bin(i).count('1') for i in range(n + 1)]
```
#### TC: O(n log n) **SC:** O(n)

The solution leverages Python's built-in `bin()` function to convert each number to its binary
representation and then counts the number of '1's in that representation using the `count()` method.
This is done for every number from 0 to n, and the results are collected in a list and returned. The
time complexity is O(n log n) because converting a number to binary and counting the bits both take
O(log n) time, and this is done for each of the n numbers. The space complexity is O(n) for storing
the result list.

---
### Counting Bits -- Best TC Solution:
```python
from typing import List

def countBits(num: int) -> List[int]:
    bits = [0] * (num + 1)
    offset = 1
    for i in range(1, num + 1):
        if offset * 2 == i:
            offset *= 2
        bits[i] = bits[i - offset] + 1
    return bits

# Example usage
num = 5
print(countBits(num))  # Output: [0, 1, 1, 2, 1, 2]
```
#### TC: O(n) **SC:** O(n)

The code uses dynamic programming to calculate the number of bits for each number up to the given
input 'num'. It optimizes the calculation by utilizing the offset technique to avoid redundant
calculations. The 'bits' array stores the number of bits for each number, and the 'offset' variable
helps in determining the offset value for each number. This approach ensures an efficient solution
with a time complexity of O(n) and a space complexity of O(n).

---
---
---
 --- 
# Repeated DNA Sequences
>Find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

>Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
Output: ["AAAAACCCCC","CCCCCAAAAA"]
- https://leetcode.com/problems/repeated-dna-sequences/
- Difficulty: 40
- Frequent: 50
- Tags: ['Bit Manipulation']

### Repeated DNA Sequences -- Shortest Solution:
```python
from collections import defaultdict

def findRepeatedDnaSequences(s):
    if len(s) < 10:
        return []
    seq_map = defaultdict(int)
    result = []
    for i in range(len(s) - 9):
        seq = s[i:i+10]
        seq_map[seq] += 1
        if seq_map[seq] == 2:
            result.append(seq)
    return result
```
#### TC: O(n) **SC:** O(n)

1. The function uses a sliding window approach to check every substring of length 10 in the input
string `s`. 2. A dictionary `seq_map` is used to count occurrences of each 10-letter-long sequence.
3. If a sequence appears exactly twice, it is added to the result list. 4. The function returns the
list of sequences that are repeated.

---
### Repeated DNA Sequences -- Best TC Solution:
```python
from collections import defaultdict
class Solution:
    def findRepeatedDnaSequences(self, s: str) -> List[str]:
        seen = defaultdict(int)
        res = set()
        for i in range(len(s) - 9):
            sub_str = s[i:i+10]
            if seen[sub_str] == 1:
                res.add(sub_str)
            seen[sub_str] += 1
        return list(res)
```
#### TC: O(N) **SC:** O(N)

The code uses a defaultdict to keep track of the frequency of each 10-character substring in the
input string. It iterates through the string and checks if the current substring has been seen
before. If it has been seen once, it adds it to the result set. Finally, it returns the list of
repeated DNA sequences. The time complexity is O(N) where N is the length of the input string, and
the space complexity is also O(N) to store the seen substrings.

---
---
---
 --- 
# Total Hamming Distance
>Calculate the total Hamming distance between all pairs of the given numbers.

>Input: [4, 14, 2]
Output: 6
- https://leetcode.com/problems/total-hamming-distance/
- Difficulty: 55
- Frequent: 50
- Tags: ['Bit Manipulation']

### Total Hamming Distance -- Shortest Solution:
```python
from itertools import combinations

def totalHammingDistance(nums):
    return sum(bin(x ^ y).count('1') for x, y in combinations(nums, 2))
```
#### TC: O(n^2) **SC:** O(1)

The code calculates the total Hamming distance between all pairs of integers in the input list
`nums`. It uses the `combinations` function from the `itertools` module to generate all possible
pairs of integers. For each pair, it computes the XOR of the two integers, converts the result to
its binary representation, and counts the number of '1's, which corresponds to the Hamming distance.
The sum of these distances for all pairs is returned as the final result.

---
### Total Hamming Distance -- Best TC Solution:
```python
from typing import List
def totalHammingDistance(nums: List[int]) -> int:
    total = 0
    n = len(nums)
    for i in range(32):
        count_ones = 0
        for num in nums:
            count_ones += (num >> i) & 1
        total += count_ones * (n - count_ones)
    return total
```
#### TC: O(N) **SC:** O(1)

The code calculates the total Hamming distance by iterating through each bit position (0 to 31) and
counting the number of ones and zeros at that position. It then calculates the total Hamming
distance for that bit position by multiplying the count of ones with the count of zeros. Finally, it
sums up the total Hamming distances for all bit positions to get the overall total Hamming distance.
This approach has a time complexity of O(N) where N is the number of elements in the input list and
a space complexity of O(1) as it only uses a constant amount of extra space.

---
---
---
 --- 
# Binary Watch
>Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

>Input: 1
Output: ["1:00","2:00","4:00","8:00","0:01","0:02","0:04","0:08","0:16","0:32"]
- https://leetcode.com/problems/binary-watch/
- Difficulty: 35
- Frequent: 45
- Tags: ['Bit Manipulation']

### Binary Watch -- Shortest Solution:
```python
def readBinaryWatch(turnedOn):
    return ["%d:%02d" % (h, m) for h in range(12) for m in range(60) if (bin(h) + bin(m)).count('1') == turnedOn]
```
#### TC: O(1) **SC:** O(1)

The code generates all possible times by iterating through hours (0-11) and minutes (0-59). It uses
bit manipulation to count the number of 1s in the binary representation of the hour and minute
combined. If the count matches the number of LEDs turned on, it formats the time and includes it in
the result. The use of list comprehension makes the code concise and efficient.

---
---
---
 --- 
# Bitwise AND of Numbers Range
>Given two integers left and right that represent the range [left, right], return the bitwise AND of all numbers in this range, inclusive.

>Input: left = 5, right = 7
Output: 4
- https://leetcode.com/problems/bitwise-and-of-numbers-range/
- Difficulty: 55
- Frequent: 45
- Tags: ['Bit Manipulation']

### Bitwise AND of Numbers Range -- Shortest Solution:
```python
# Bit Manipulation Approach

def rangeBitwiseAnd(m, n):
    shift = 0
    while m < n:
        m >>= 1
        n >>= 1
        shift += 1
    return m << shift
```
#### TC: O(1) **SC:** O(1)

The code uses a bit manipulation approach to find the bitwise AND of numbers in a given range. It
shifts both 'm' and 'n' to the right until they are equal, keeping track of the number of shifts.
Finally, it shifts 'm' to the left by the number of shifts to get the result. This approach has a
constant time and space complexity.

---
---
---
 --- 
# UTF-8 Validation
>Given an integer array data representing the data, return whether it is a valid UTF-8 encoding.

>Input: [197, 130, 1]
Output: true
- https://leetcode.com/problems/utf-8-validation/
- Difficulty: 60
- Frequent: 45
- Tags: ['Bit Manipulation']

### UTF-8 Validation -- Shortest Solution:
```python
def validUtf8(data):
    n_bytes = 0
    for num in data:
        bin_rep = format(num, '#010b')[-8:]
        if n_bytes == 0:
            for bit in bin_rep:
                if bit == '0':
                    break
                n_bytes += 1
            if n_bytes == 0:
                continue
            if n_bytes == 1 or n_bytes > 4:
                return False
        else:
            if not (bin_rep[0] == '1' and bin_rep[1] == '0'):
                return False
        n_bytes -= 1
    return n_bytes == 0
```
#### TC: O(n) **SC:** O(1)

1. The function iterates through each byte in the input data. 2. It converts each byte to its binary
representation. 3. It checks the number of leading 1s to determine the number of bytes in the UTF-8
character. 4. It validates the continuation bytes to ensure they follow the '10xxxxxx' pattern. 5.
The function returns True if all bytes are valid UTF-8 characters, otherwise False.

---
---
---