# Mathematical Algorithms
## Frequent score for this algorithmic framework: 10 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Roman to Integer](#roman-to-integer) | 40 | 15 |
| 2 | [Integer to Roman](#integer-to-roman) | 35 | 20 |
| 3 | [Count Primes](#count-primes) | 35 | 25 |
| 4 | [Excel Sheet Column Number](#excel-sheet-column-number) | 30 | 15 |
| 5 | [Happy Number](#happy-number) | 30 | 20 |
| 6 | [Valid Perfect Square](#valid-perfect-square) | 30 | 20 |
| 7 | [Power of Three](#power-of-three) | 30 | 20 |
| 8 | [Power of Four](#power-of-four) | 30 | 20 |
| 9 | [Add Digits](#add-digits) | 25 | 15 |
| 10 | [Factorial Trailing Zeroes](#factorial-trailing-zeroes) | 25 | 20 |
| 11 | [Arranging Coins](#arranging-coins) | 25 | 20 |
| 12 | [Sum of Square Numbers](#sum-of-square-numbers) | 25 | 20 |
| 13 | [Perfect Number](#perfect-number) | 25 | 25 |
| 14 | [Nth Digit](#nth-digit) | 25 | 25 |
| 15 | [Self Dividing Numbers](#self-dividing-numbers) | 20 | 15 |
| 16 | [Ugly Number](#ugly-number) | 20 | 20 |
| 17 | [Reordered Power of 2](#reordered-power-of-2) | 20 | 30 |
---
Mathematical algorithms are procedures or formulas for solving mathematical problems. They are step-by-step instructions that take inputs, process them through a series of computational steps, and produce outputs. These algorithms can range from basic arithmetic operations to complex numerical methods used in scientific computing.
```python
# Example 1: Euclidean Algorithm for GCD

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

print(gcd(48, 18))  # Output: 6

# Example 2: Sieve of Eratosthenes for Prime Numbers

def sieve_of_eratosthenes(n):
    primes = [True] * (n + 1)
    p = 2
    while p * p <= n:
        if primes[p]:
            for i in range(p * p, n + 1, p):
                primes[i] = False
        p += 1
    return [p for p in range(2, n + 1) if primes[p]]

print(sieve_of_eratosthenes(30))  # Output: [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```
### Insight:
1. Euclidean Algorithm: This algorithm efficiently computes the greatest common divisor (GCD) of two numbers by repeatedly applying the modulus operation. It is simple yet powerful, with a logarithmic time complexity.
2. Sieve of Eratosthenes: This algorithm finds all prime numbers up to a given limit. It works by iteratively marking the multiples of each prime starting from 2. The algorithm is efficient for large values of n, with a time complexity of O(n log log n).
---
 --- 
# Roman to Integer
>Convert a Roman numeral to an integer.

>Input: "III"
Output: 3
- https://leetcode.com/problems/roman-to-integer/
- Difficulty: 15
- Frequent: 40
- Tags: ['Mathematical Algorithms']

### Roman to Integer -- Shortest Solution:
```python
# Function to convert Roman numeral to integer

def romanToInt(s: str) -> int:
    roman_dict = {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000
    }
    prev_value = 0
    total = 0
    for char in s:
        curr_value = roman_dict[char]
        total += curr_value
        if curr_value > prev_value:
            total -= 2 * prev_value
        prev_value = curr_value
    return total

# Test the function
print(romanToInt("III"))  # Output: 3
print(romanToInt("IV"))   # Output: 4
print(romanToInt("IX"))   # Output: 9
print(romanToInt("LVIII"))  # Output: 58
print(romanToInt("MCMXCIV"))  # Output: 1994
```
#### TC: Time Complexity: O(n) **SC:** Space Complexity: O(1)

The code uses a dictionary to map Roman numerals to their integer values. It iterates through the
input string and calculates the total integer value by comparing the current value with the previous
value. If the current value is greater than the previous value, it subtracts twice the previous
value to handle cases like IV (4) or IX (9). This approach ensures a linear time complexity of O(n)
and constant space complexity of O(1).

---
### Roman to Integer -- Best TC Solution:
```python
# Function to convert Roman numerals to an integer

def romanToInt(s: str) -> int:
    roman_dict = {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000
    }
    result = 0
    prev_value = 0
    for char in s:
        curr_value = roman_dict[char]
        result += curr_value
        if curr_value > prev_value:
            result -= 2 * prev_value
        prev_value = curr_value
    return result

# Test the function
print(romanToInt("III"))  # Output: 3
print(romanToInt("IV"))   # Output: 4
print(romanToInt("IX"))   # Output: 9
print(romanToInt("LVIII"))  # Output: 58
print(romanToInt("MCMXCIV"))  # Output: 1994
```
#### TC: O(n) **SC:** O(1)

The code uses a dictionary to map Roman numerals to their integer values. It iterates through the
input string and calculates the total integer value by comparing the current value with the previous
value. If the current value is greater than the previous value, it subtracts twice the previous
value to handle cases like IV (4) or IX (9). This approach ensures a time complexity of O(n) and a
space complexity of O(1), making it an efficient solution for converting Roman numerals to integers.

---
---
---
 --- 
# Integer to Roman
>Convert an integer to a Roman numeral.

>Input: 3
Output: "III"
- https://leetcode.com/problems/integer-to-roman/
- Difficulty: 20
- Frequent: 35
- Tags: ['Mathematical Algorithms']

### Integer to Roman -- Shortest Solution:
```python
# Function to convert an integer to a Roman numeral

def int_to_roman(num: int) -> str:
    values = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
    symbols = ['M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I']
    roman_numeral = ''
    for i in range(len(values)):
        while num >= values[i]:
            num -= values[i]
            roman_numeral += symbols[i]
    return roman_numeral

# Test the function
num = 3549
print(int_to_roman(num))
```
#### TC: O(1) **SC:** O(1)

The code uses a simple approach where we have predefined values and symbols for Roman numerals. It
iterates through the values and checks if the input number is greater than or equal to the current
value. If true, it subtracts the value from the input number and appends the corresponding symbol to
the Roman numeral string. This process continues until the input number becomes 0. The time and
space complexity of this solution is O(1) as it operates on a fixed set of values and symbols.

---
---
---
 --- 
# Count Primes
>Count the number of prime numbers less than a non-negative number, n.

>Input: 10
Output: 4
- https://leetcode.com/problems/count-primes/
- Difficulty: 25
- Frequent: 35
- Tags: ['Mathematical Algorithms']

### Count Primes -- Shortest Solution:
```python
# Sieve of Eratosthenes Algorithm

def countPrimes(self, n: int) -> int:
    if n < 3:
        return 0
    primes = [True] * n
    primes[0] = primes[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if primes[i]:
            primes[i * i:n:i] = [False] * len(primes[i * i:n:i])
    return sum(primes)
```
#### TC: O(n log log n) **SC:** O(n)

The code implements the Sieve of Eratosthenes algorithm to count the number of prime numbers less
than a given number n. It initializes a list of boolean values to track prime numbers up to n. The
algorithm marks multiples of each prime number as non-prime until the square of the prime exceeds n.
Finally, it returns the count of prime numbers. The time complexity is O(n log log n) due to the
Sieve of Eratosthenes algorithm, and the space complexity is O(n) to store the boolean list.

---
### Count Primes -- Best SC Solution:
```python
# Sieve of Eratosthenes Algorithm

def countPrimes(n):
    if n < 3:
        return 0
    primes = [True] * n
    primes[0] = primes[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if primes[i]:
            primes[i * i:n:i] = [False] * len(primes[i * i:n:i])
    return sum(primes)

# Example
n = 10
print(countPrimes(n))  # Output: 4
```
#### TC: Time Complexity: O(n log log n) **SC:** Space Complexity: O(n)

The code uses the Sieve of Eratosthenes algorithm to efficiently count the number of prime numbers
less than a given number n. It initializes a list of boolean values to track prime numbers up to n.
It iterates through the numbers up to the square root of n, marking multiples of each prime number
as non-prime. Finally, it counts the remaining prime numbers in the list. The algorithm has a time
complexity of O(n log log n) and a space complexity of O(n), making it an efficient solution for
counting prime numbers.

---
---
---
 --- 
# Excel Sheet Column Number
>Given a column title as appear in an Excel sheet, return its corresponding column number.

>Input: "A"
Output: 1
- https://leetcode.com/problems/excel-sheet-column-number/
- Difficulty: 15
- Frequent: 30
- Tags: ['Mathematical Algorithms']

### Excel Sheet Column Number -- Shortest Solution:
```python
# Convert Excel column title to number

def titleToNumber(columnTitle: str) -> int:
    result = 0
    for char in columnTitle:
        result = result * 26 + ord(char) - ord('A') + 1
    return result

# Example
print(titleToNumber("AB"))  # Output: 28
```
#### TC: O(n) **SC:** O(1)

The code converts an Excel column title to its corresponding column number. It iterates through each
character in the title, calculates the result by multiplying the previous result by 26 and adding
the value of the current character. The ord() function is used to get the ASCII value of a
character. The formula used is based on the positional value of characters in the alphabet. The time
complexity is O(n) where n is the length of the column title, and the space complexity is O(1) as
only a constant amount of extra space is used.

---
---
---
 --- 
# Happy Number
>Determine if a number is a happy number.

>Input: 19
Output: true
- https://leetcode.com/problems/happy-number/
- Difficulty: 20
- Frequent: 30
- Tags: ['Mathematical Algorithms']

### Happy Number -- Shortest Solution:
```python
# Function to check if a number is a Happy Number

def isHappy(n: int) -> bool:
    def get_next(num):
        total_sum = 0
        while num > 0:
            num, digit = divmod(num, 10)
            total_sum += digit ** 2
        return total_sum

    seen = set()
    while n != 1 and n not in seen:
        seen.add(n)
        n = get_next(n)
    return n == 1

# Example usage
n = 19
print(isHappy(n))  # Output: True
```
#### TC: O(log n) **SC:** O(log n)

The code defines a function isHappy(n) that checks if a number is a Happy Number. It uses a helper
function get_next(num) to calculate the sum of squares of digits. The main function iterates through
the sequence of numbers until it reaches 1 (a happy number) or enters a cycle. The use of a set to
store seen numbers helps in detecting cycles efficiently. The time complexity is O(log n) due to the
nature of the Happy Number calculation, and the space complexity is also O(log n) due to the set
storing seen numbers.

---
---
---
 --- 
# Valid Perfect Square
>Given a positive integer num, write a function which returns True if num is a perfect square else False.

>Input: 16
Output: true
- https://leetcode.com/problems/valid-perfect-square/
- Difficulty: 20
- Frequent: 30
- Tags: ['Mathematical Algorithms']

### Valid Perfect Square -- Shortest Solution:
```python
# Binary Search Approach

class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        if num < 2:
            return True
        left, right = 2, num // 2
        while left <= right:
            x = left + (right - left) // 2
            guess = x * x
            if guess == num:
                return True
            elif guess < num:
                left = x + 1
            else:
                right = x - 1
        return False
```
#### TC: O(log n) **SC:** O(1)

The code uses a binary search approach to find if a given number is a perfect square. It starts by
setting the left and right boundaries for the search space. It then iteratively calculates the
square of the middle number and adjusts the boundaries based on whether the guess is equal to, less
than, or greater than the target number. This approach reduces the search space by half in each
iteration, leading to a time complexity of O(log n) and a space complexity of O(1). The code is
concise, efficient, and easy to understand.

---
---
---
 --- 
# Power of Three
>Given an integer, write a function to determine if it is a power of three.

>Input: 27
Output: true
- https://leetcode.com/problems/power-of-three/
- Difficulty: 20
- Frequent: 30
- Tags: ['Mathematical Algorithms']

### Power of Three -- Shortest Solution:
```python
# Check if n is a power of 3 by continuously dividing it by 3
# until it becomes 1 or not divisible by 3

class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        if n <= 0:
            return False
        while n % 3 == 0:
            n //= 3
        return n == 1
```
#### TC: O(log n) **SC:** O(1

The code checks if a given number is a power of 3 by continuously dividing it by 3 until it becomes
1 or not divisible by 3. This approach ensures that the code runs in logarithmic time complexity
O(log n) as it reduces the number by a factor of 3 in each iteration. The space complexity is
constant O(1) as only a few variables are used. The code is concise, clean, and easy to understand,
making it an optimal solution for the Power of Three problem.

---
### Power of Three -- Best TC Solution:
```python
# Approach: Using the property of maximum power of 3 within 32-bit integer range
# Check if n is a power of 3 by verifying if 3^19 is divisible by n

class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        return n > 0 and 1162261467 % n == 0
```
#### TC: O(1) **SC:** O(1)

The code utilizes the property of the maximum power of 3 within the 32-bit integer range to
determine if a given number is a power of three. By checking if 3^19 is divisible by the input
number, we can efficiently determine if the number is a power of three. This approach has a constant
time and space complexity, making it an optimal solution for the problem.

---
---
---
 --- 
# Power of Four
>Given an integer, write a function to determine if it is a power of four.

>Input: 16
Output: true
- https://leetcode.com/problems/power-of-four/
- Difficulty: 20
- Frequent: 30
- Tags: ['Mathematical Algorithms']

### Power of Four -- Shortest Solution:
```python
# Check if the number is a power of 4 by checking if it is a power of 2 and has an even number of trailing zeros

class Solution:
    def isPowerOfFour(self, num: int) -> bool:
        return num > 0 and num & (num - 1) == 0 and num & 0x55555555 != 0
```
#### TC: O(1) **SC:** O(1

The code uses bitwise operations to efficiently check if a number is a power of 4. It first checks
if the number is a power of 2 by verifying if it has only one bit set. Then, it ensures that the bit
is at an even position by using the bitmask 0x55555555, which represents the pattern of alternating
1s and 0s. This approach allows for a constant time and space complexity as it directly performs the
necessary checks without any loops or additional data structures.

---
---
---
 --- 
# Add Digits
>Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

>Input: 38
Output: 2
- https://leetcode.com/problems/add-digits/
- Difficulty: 15
- Frequent: 25
- Tags: ['Mathematical Algorithms']

### Add Digits -- Shortest Solution:
```python
# Mathematical approach

def addDigits(num):
    return 1 + (num - 1) % 9 if num != 0 else 0

# Test the function
print(addDigits(38))  # Output: 2
```
#### TC: O(1) **SC:** O(1)

The code utilizes a mathematical approach to solve the problem efficiently. It calculates the
digital root of a number by applying a simple formula. The time complexity and space complexity of
the solution are both constant O(1), making it a very efficient and clean solution.

---
---
---
 --- 
# Factorial Trailing Zeroes
>Given an integer n, return the number of trailing zeroes in n!.

>Input: 3
Output: 0
- https://leetcode.com/problems/factorial-trailing-zeroes/
- Difficulty: 20
- Frequent: 25
- Tags: ['Mathematical Algorithms']

### Factorial Trailing Zeroes -- Shortest Solution:
```python
# Factorial Trailing Zeroes

def trailingZeroes(n):
    count = 0
    while n > 0:
        n //= 5
        count += n
    return count

# Example
n = 25
print(trailingZeroes(n))  # Output: 6
```
#### TC: O(log n) **SC:** O(1)

The code calculates the number of trailing zeroes in the factorial of a given number 'n'. It
iteratively divides 'n' by 5 and adds the result to the count until 'n' becomes 0. This approach
efficiently counts the number of factors of 5 in the factorial, which corresponds to the number of
trailing zeroes. The time complexity is O(log n) as it iteratively divides 'n' by 5, and the space
complexity is O(1) as only a constant amount of extra space is used.

---
---
---
 --- 
# Arranging Coins
>You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.

>Input: 5
Output: 2
- https://leetcode.com/problems/arranging-coins/
- Difficulty: 20
- Frequent: 25
- Tags: ['Mathematical Algorithms']

### Arranging Coins -- Shortest Solution:
```python
import math

class Solution:
    def arrangeCoins(self, n: int) -> int:
        return int((math.sqrt(8 * n + 1) - 1) / 2)
```
#### TC: O(1) **SC:** O(1)

The code calculates the number of complete rows of coins that can be formed with a given number of
coins. It uses the formula for the sum of the first n natural numbers to calculate the maximum
number of complete rows. The time complexity is O(1) as the calculation is done in constant time,
and the space complexity is also O(1) as no extra space is used.

---
---
---
 --- 
# Sum of Square Numbers
>Given a non-negative integer c, decide whether there're two integers a and b such that a^2 + b^2 = c.

>Input: 5
Output: true
- https://leetcode.com/problems/sum-of-square-numbers/
- Difficulty: 20
- Frequent: 25
- Tags: ['Mathematical Algorithms']

### Sum of Square Numbers -- Shortest Solution:
```python
import math

class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        left, right = 0, int(math.sqrt(c))
        while left <= right:
            current_sum = left ** 2 + right ** 2
            if current_sum == c:
                return True
            elif current_sum < c:
                left += 1
            else:
                right -= 1
        return False
```
#### TC: O(sqrt(c)) **SC:** O(1)

The code uses a two-pointer approach to find if there are two numbers whose squares sum up to the
given number 'c'. It starts with two pointers at 0 and the square root of 'c' respectively, and
iterates while adjusting the pointers based on the sum of their squares compared to 'c'. This
approach ensures an optimal time complexity of O(sqrt(c)) and constant space complexity of O(1).

---
---
---
 --- 
# Perfect Number
>A perfect number is a positive integer that is equal to the sum of its proper positive divisors, excluding the number itself.

>Input: 28
Output: true
- https://leetcode.com/problems/perfect-number/
- Difficulty: 25
- Frequent: 25
- Tags: ['Mathematical Algorithms']

### Perfect Number -- Shortest Solution:
```python
# Check if the number is a perfect number

def checkPerfectNumber(num: int) -> bool:
    if num <= 1:
        return False
    divisors_sum = 1
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            divisors_sum += i
            if i != num // i:
                divisors_sum += num // i
    return divisors_sum == num
```
#### TC: O(sqrt(n)) **SC:** O(1)

The code defines a function checkPerfectNumber that takes an integer num as input and returns a
boolean indicating whether num is a perfect number. It iterates through all possible divisors of num
up to the square root of num and calculates the sum of divisors. If the sum of divisors equals num,
then num is a perfect number. The time complexity of the code is O(sqrt(n)) because it iterates up
to the square root of num. The space complexity is O(1) as it only uses a constant amount of extra
space.

---
---
---
 --- 
# Nth Digit
>Find the nth digit of the infinite integer sequence.

>Input: 11
Output: 0
- https://leetcode.com/problems/nth-digit/
- Difficulty: 25
- Frequent: 25
- Tags: ['Mathematical Algorithms']

### Nth Digit -- Shortest Solution:
```python
class Solution:
    def findNthDigit(self, n: int) -> int:
        length, size, start = 1, 9, 1
        while n > length * size:
            n -= length * size
            length += 1
            size *= 10
            start *= 10
        start += (n - 1) // length
        return int(str(start)[(n - 1) % length])
```
#### TC: O(log n) **SC:** O(1)

The code calculates the length of the number where the nth digit is located, then determines the
actual number and digit position. It iterates through the digits by adjusting the length, size, and
start values until it finds the correct digit. The time complexity is O(log n) as it involves
logarithmic operations, and the space complexity is O(1) as it uses constant space.

---
### Nth Digit -- Best TC Solution:
```python
# Approach: Mathematical Algorithm

def findNthDigit(n: int) -> int:
    length = 1
    count = 9
    start = 1

    while n > length * count:
        n -= length * count
        length += 1
        count *= 10
        start *= 10

    start += (n - 1) // length
    return int(str(start)[(n - 1) % length])

# Example
n = 11
print(findNthDigit(n))  # Output: 0
```
#### TC: Time Complexity: O(log n) **SC:** Space Complexity: O(1)

1. The approach uses a mathematical algorithm to find the Nth digit in a sequence of numbers. 2. It
calculates the length of the numbers, the count of digits, and the starting point. 3. It iterates
through the sequence to find the digit at the Nth position. 4. The time complexity is O(log n) as it
involves logarithmic operations. 5. The space complexity is O(1) as it only uses a constant amount
of extra space.

---
### Nth Digit -- Best SC Solution:
```python
class Solution:
    def findNthDigit(self, n: int) -> int:
        length = 1
        count = 9
        start = 1
        while n > length * count:
            n -= length * count
            length += 1
            count *= 10
            start *= 10
        start += (n - 1) // length
        return int(str(start)[(n - 1) % length])
```
#### TC: O(log n) **SC:** O(1

The code calculates the length of the numbers in each range (1-9, 10-99, 100-999, etc.) and
determines the starting number of the range where the nth digit lies. It then calculates the actual
number where the nth digit is located and extracts the digit from that number. This approach
optimizes the calculation by reducing the search space based on the length of the numbers and the
count of digits in each range.

---
---
---
 --- 
# Self Dividing Numbers
>A self-dividing number is a number that is divisible by every digit it contains.

>Input: 128
Output: true
- https://leetcode.com/problems/self-dividing-numbers/
- Difficulty: 15
- Frequent: 20
- Tags: ['Mathematical Algorithms']

### Self Dividing Numbers -- Shortest Solution:
```python
from typing import List

def selfDividingNumbers(left: int, right: int) -> List[int]:
    def is_self_dividing(num):
        return '0' not in str(num) and all(num % int(digit) == 0 for digit in str(num))
    
    return [num for num in range(left, right + 1) if is_self_dividing(num)]
```
#### TC: O(D) where D is the count of numbers in the range [left, right] **SC:** O(D) where D is the count of numbers in the range [left, right]

The code defines a function is_self_dividing to check if a number is self-dividing. It then
generates a list of self-dividing numbers within the given range [left, right]. The solution
iterates through each number in the range and filters out the ones that are self-dividing. The time
complexity is O(D) where D is the count of numbers in the range, and the space complexity is also
O(D) due to the list of self-dividing numbers.

---
### Self Dividing Numbers -- Best SC Solution:
```python
from typing import List

class Solution:
    def selfDividingNumbers(self, left: int, right: int) -> List[int]:
        def is_self_dividing(num):
            for digit in str(num):
                if digit == '0' or num % int(digit) != 0:
                    return False
            return True
        
        res = []
        for i in range(left, right + 1):
            if is_self_dividing(i):
                res.append(i)
        
        return res
```
#### TC: O(D) where D is the count of numbers in the range [left, right] **SC:** O(D) for storing the result list

1. Define a helper function is_self_dividing to check if a number is self-dividing. 2. Iterate
through each number in the range [left, right] and check if it is self-dividing using the helper
function. 3. Add the self-dividing numbers to the result list and return it. 4. Time complexity is
O(D) where D is the count of numbers in the range [left, right]. 5. Space complexity is O(D) for
storing the result list.

---
---
---
 --- 
# Ugly Number
>Write a program to check whether a given number is an ugly number.

>Input: 6
Output: true
- https://leetcode.com/problems/ugly-number/
- Difficulty: 20
- Frequent: 20
- Tags: ['Mathematical Algorithms']

### Ugly Number -- Shortest Solution:
```python
# Check if the number is an ugly number

def isUgly(num: int) -> bool:
    if num <= 0:
        return False
    for p in 2, 3, 5:
        while num % p == 0:
            num //= p
    return num == 1
```
#### TC: O(log n) **SC:** O(1)

The code checks if a given number is an ugly number by continuously dividing it by 2, 3, and 5 until
it becomes 1 or cannot be divided further. If the number is an ugly number, it will eventually reach
1 after all the divisions. The code efficiently handles negative numbers and returns False for non-
positive numbers. The time complexity is O(log n) as the number is divided by 2, 3, and 5 until it
reaches 1, and the space complexity is O(1) as no extra space is used.

---
---
---
 --- 
# Reordered Power of 2
>Check if an integer's digits can be reordered to form a power of 2.

>Input: 1
Output: true
- https://leetcode.com/problems/reordered-power-of-2/
- Difficulty: 30
- Frequent: 20
- Tags: ['Mathematical Algorithms']

### Reordered Power of 2 -- Shortest Solution:
```python
from collections import Counter
def reorderedPowerOf2(N: int) -> bool:
    count = Counter(str(N))
    return any(count == Counter(str(1 << i)) for i in range(31))
```
#### TC: O(1) **SC:** O(1)

The code uses Counter to count the occurrences of each digit in the input number N. It then iterates
through powers of 2 up to 2^30 and checks if the count of digits in N matches the count of digits in
the current power of 2. If a match is found, it returns True indicating that N can be reordered to
form a power of 2. The time and space complexity is O(1) as the loop runs for a fixed number of
iterations and the space used is constant.

---
---
---