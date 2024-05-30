# Performance Optimization
[^Table of content](../toc.md)
## Frequent score for this category: 30.0 

|     | Code Review Rule | Frequency Score |
| --- | --- | --- |
| 1 | [Use appropriate data compression techniques](#rule-1-use-appropriate-data-compression-techniques) | 100.0 |
| 2 | [Avoid excessive use of metaprogramming](#rule-2-avoid-excessive-use-of-metaprogramming) | 100.0 |
| 3 | [Optimize use of decorators](#rule-3-optimize-use-of-decorators) | 95.0 |
| 4 | [Optimize use of data streams](#rule-4-optimize-use-of-data-streams) | 95.0 |
| 5 | [Use appropriate data partitioning techniques](#rule-5-use-appropriate-data-partitioning-techniques) | 90.0 |
| 6 | [Use appropriate data encryption techniques](#rule-6-use-appropriate-data-encryption-techniques) | 90.0 |
| 7 | [Avoid excessive logging](#rule-7-avoid-excessive-logging) | 85.0 |
| 8 | [Avoid unnecessary type casting](#rule-8-avoid-unnecessary-type-casting) | 85.0 |
| 9 | [Avoid excessive use of regular expressions](#rule-9-avoid-excessive-use-of-regular-expressions) | 85.0 |
| 10 | [Use memory efficient data types](#rule-10-use-memory-efficient-data-types) | 80.0 |
| 11 | [Use efficient date and time handling](#rule-11-use-efficient-date-and-time-handling) | 80.0 |
| 12 | [Optimize use of memory buffers](#rule-12-optimize-use-of-memory-buffers) | 80.0 |
| 13 | [Avoid redundant calculations](#rule-13-avoid-redundant-calculations) | 75.0 |
| 14 | [Avoid redundant data transformations](#rule-14-avoid-redundant-data-transformations) | 75.0 |
| 15 | [Use appropriate data sharding techniques](#rule-15-use-appropriate-data-sharding-techniques) | 75.0 |
| 16 | [Use vectorized operations with NumPy](#rule-16-use-vectorized-operations-with-numpy) | 70.0 |
| 17 | [Use appropriate concurrency mechanisms](#rule-17-use-appropriate-concurrency-mechanisms) | 70.0 |
| 18 | [Avoid excessive use of class inheritance](#rule-18-avoid-excessive-use-of-class-inheritance) | 70.0 |
| 19 | [Minimize the use of try except blocks](#rule-19-minimize-the-use-of-try-except-blocks) | 65.0 |
| 20 | [Leverage concurrency for I/O bound tasks](#rule-20-leverage-concurrency-for-i-o-bound-tasks) | 65.0 |
| 21 | [Optimize use of collections](#rule-21-optimize-use-of-collections) | 65.0 |
| 22 | [Optimize use of recursion](#rule-22-optimize-use-of-recursion) | 65.0 |
| 23 | [Use generators instead of lists for large datasets](#rule-23-use-generators-instead-of-lists-for-large-datasets) | 60.0 |
| 24 | [Use just in time compilation with Numba](#rule-24-use-just-in-time-compilation-with-numba) | 60.0 |
| 25 | [Use appropriate data validation techniques](#rule-25-use-appropriate-data-validation-techniques) | 60.0 |
| 26 | [Use appropriate data indexing techniques](#rule-26-use-appropriate-data-indexing-techniques) | 60.0 |
| 27 | [Cache expensive function calls](#rule-27-cache-expensive-function-calls) | 55.0 |
| 28 | [Optimize JSON parsing and serialization](#rule-28-optimize-json-parsing-and-serialization) | 55.0 |
| 29 | [Avoid deep nesting of loops and conditionals](#rule-29-avoid-deep-nesting-of-loops-and-conditionals) | 55.0 |
| 30 | [Minimize use of reflection](#rule-30-minimize-use-of-reflection) | 55.0 |
| 31 | [Avoid unnecessary object creation](#rule-31-avoid-unnecessary-object-creation) | 50.0 |
| 32 | [Avoid using expensive operations in loops](#rule-32-avoid-using-expensive-operations-in-loops) | 50.0 |
| 33 | [Use appropriate logging levels](#rule-33-use-appropriate-logging-levels) | 50.0 |
| 34 | [Use efficient search algorithms](#rule-34-use-efficient-search-algorithms) | 50.0 |
| 35 | [Use multi threading and multi processing](#rule-35-use-multi-threading-and-multi-processing) | 45.0 |
| 36 | [Use efficient string operations](#rule-36-use-efficient-string-operations) | 45.0 |
| 37 | [Avoid using complex comprehensions](#rule-37-avoid-using-complex-comprehensions) | 45.0 |
| 38 | [Avoid unnecessary data copying](#rule-38-avoid-unnecessary-data-copying) | 45.0 |
| 39 | [Profile code to identify bottlenecks](#rule-39-profile-code-to-identify-bottlenecks) | 40.0 |
| 40 | [Minimize use of global interpreter lock (GIL)](#rule-40-minimize-use-of-global-interpreter-lock-gil) | 40.0 |
| 41 | [Use slots to reduce memory overhead in classes](#rule-41-use-slots-to-reduce-memory-overhead-in-classes) | 40.0 |
| 42 | [Optimize use of loops](#rule-42-optimize-use-of-loops) | 40.0 |
| 43 | [Use efficient data structures](#rule-43-use-efficient-data-structures) | 35.0 |
| 44 | [Use efficient file handling techniques](#rule-44-use-efficient-file-handling-techniques) | 35.0 |
| 45 | [Optimize regular expressions](#rule-45-optimize-regular-expressions) | 35.0 |
| 46 | [Use appropriate data caching strategies](#rule-46-use-appropriate-data-caching-strategies) | 35.0 |
| 47 | [Avoid deep recursion](#rule-47-avoid-deep-recursion) | 30.0 |
| 48 | [Use appropriate exception handling](#rule-48-use-appropriate-exception-handling) | 30.0 |
| 49 | [Minimize use of global state](#rule-49-minimize-use-of-global-state) | 30.0 |
| 50 | [Use lazy evaluation](#rule-50-use-lazy-evaluation) | 25.0 |
| 51 | [Use appropriate data serialization formats](#rule-51-use-appropriate-data-serialization-formats) | 25.0 |
| 52 | [Avoid using eval() and exec()](#rule-52-avoid-using-eval-and-exec) | 25.0 |
| 53 | [Use efficient sorting algorithms](#rule-53-use-efficient-sorting-algorithms) | 25.0 |
| 54 | [Optimize network communication](#rule-54-optimize-network-communication) | 20.0 |
| 55 | [Avoid blocking I/O operations](#rule-55-avoid-blocking-i-o-operations) | 20.0 |
| 56 | [Use efficient image processing techniques](#rule-56-use-efficient-image-processing-techniques) | 15.0 |
| 57 | [Optimize use of third party libraries](#rule-57-optimize-use-of-third-party-libraries) | 15.0 |
| 58 | [Avoid blocking calls](#rule-58-avoid-blocking-calls) | 10.0 |
| 59 | [Avoid unnecessary synchronization](#rule-59-avoid-unnecessary-synchronization) | 10.0 |
| 60 | [Use appropriate data structures for specific tasks](#rule-60-use-appropriate-data-structures-for-specific-tasks) | 10.0 |
| 61 | [Use appropriate memory management techniques](#rule-61-use-appropriate-memory-management-techniques) | 10.0 |
| 62 | [Optimize algorithm complexity](#rule-62-optimize-algorithm-complexity) | 5.0 |
| 63 | [Use efficient mathematical computations](#rule-63-use-efficient-mathematical-computations) | 5.0 |
| 64 | [Avoid excessive use of lambda functions](#rule-64-avoid-excessive-use-of-lambda-functions) | 5.0 |
| 65 | [Avoid excessive use of nested functions](#rule-65-avoid-excessive-use-of-nested-functions) | 5.0 |
---
 --- 
# Rule 1. Use appropriate data compression techniques

| Frequent score for this rule: 100.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 2. Avoid excessive use of metaprogramming

| Frequent score for this rule: 100.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 3. Optimize use of decorators

| Frequent score for this rule: 95.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 4. Optimize use of data streams

| Frequent score for this rule: 95.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 5. Use appropriate data partitioning techniques

| Frequent score for this rule: 90.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 6. Use appropriate data encryption techniques

| Frequent score for this rule: 90.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 7. Avoid excessive logging

| Frequent score for this rule: 85.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 8. Avoid unnecessary type casting

| Frequent score for this rule: 85.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 9. Avoid excessive use of regular expressions

| Frequent score for this rule: 85.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 10. Use memory efficient data types

| Frequent score for this rule: 80.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 11. Use efficient date and time handling

| Frequent score for this rule: 80.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 12. Optimize use of memory buffers

| Frequent score for this rule: 80.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 13. Avoid redundant calculations

| Frequent score for this rule: 75.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 14. Avoid redundant data transformations

| Frequent score for this rule: 75.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 15. Use appropriate data sharding techniques

| Frequent score for this rule: 75.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 16. Use vectorized operations with NumPy

| Frequent score for this rule: 70.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 17. Use appropriate concurrency mechanisms

| Frequent score for this rule: 70.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 18. Avoid excessive use of class inheritance

| Frequent score for this rule: 70.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 19. Minimize the use of try except blocks

| Frequent score for this rule: 65.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 20. Leverage concurrency for I/O bound tasks

| Frequent score for this rule: 65.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 21. Optimize use of collections

| Frequent score for this rule: 65.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 22. Optimize use of recursion

| Frequent score for this rule: 65.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 23. Use generators instead of lists for large datasets

| Frequent score for this rule: 60.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 24. Use just in time compilation with Numba

| Frequent score for this rule: 60.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 25. Use appropriate data validation techniques

| Frequent score for this rule: 60.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 26. Use appropriate data indexing techniques

| Frequent score for this rule: 60.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 27. Cache expensive function calls

| Frequent score for this rule: 55.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 28. Optimize JSON parsing and serialization

| Frequent score for this rule: 55.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 29. Avoid deep nesting of loops and conditionals

| Frequent score for this rule: 55.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 30. Minimize use of reflection

| Frequent score for this rule: 55.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 31. Avoid unnecessary object creation

| Frequent score for this rule: 50.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 32. Avoid using expensive operations in loops

| Frequent score for this rule: 50.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 33. Use appropriate logging levels

| Frequent score for this rule: 50.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 34. Use efficient search algorithms

| Frequent score for this rule: 50.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 35. Use multi threading and multi processing

| Frequent score for this rule: 45.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 36. Use efficient string operations

| Frequent score for this rule: 45.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 37. Avoid using complex comprehensions

| Frequent score for this rule: 45.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 38. Avoid unnecessary data copying

| Frequent score for this rule: 45.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 39. Profile code to identify bottlenecks

| Frequent score for this rule: 40.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 40. Minimize use of global interpreter lock (GIL)

| Frequent score for this rule: 40.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 41. Use slots to reduce memory overhead in classes

| Frequent score for this rule: 40.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 42. Optimize use of loops

| Frequent score for this rule: 40.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 43. Use efficient data structures

| Frequent score for this rule: 35.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 44. Use efficient file handling techniques

| Frequent score for this rule: 35.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 45. Optimize regular expressions

| Frequent score for this rule: 35.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 46. Use appropriate data caching strategies

| Frequent score for this rule: 35.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 47. Avoid deep recursion

| Frequent score for this rule: 30.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 48. Use appropriate exception handling

| Frequent score for this rule: 30.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 49. Minimize use of global state

| Frequent score for this rule: 30.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 50. Use lazy evaluation

| Frequent score for this rule: 25.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 51. Use appropriate data serialization formats

| Frequent score for this rule: 25.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 52. Avoid using eval() and exec()

| Frequent score for this rule: 25.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 53. Use efficient sorting algorithms

| Frequent score for this rule: 25.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 54. Optimize network communication

| Frequent score for this rule: 20.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 55. Avoid blocking I/O operations

| Frequent score for this rule: 20.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 56. Use efficient image processing techniques

| Frequent score for this rule: 15.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 57. Optimize use of third party libraries

| Frequent score for this rule: 15.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 58. Avoid blocking calls

| Frequent score for this rule: 10.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 59. Avoid unnecessary synchronization

| Frequent score for this rule: 10.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 60. Use appropriate data structures for specific tasks

| Frequent score for this rule: 10.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 61. Use appropriate memory management techniques

| Frequent score for this rule: 10.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 62. Optimize algorithm complexity

| Frequent score for this rule: 5.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 63. Use efficient mathematical computations

| Frequent score for this rule: 5.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 64. Avoid excessive use of lambda functions

| Frequent score for this rule: 5.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 
 --- 
# Rule 65. Avoid excessive use of nested functions

| Frequent score for this rule: 5.0 | [^Top](#performance-optimization) 

## Explanation
>None

## Why do we need this rule?
>None

## If not apply this rule, what will happen?
| None


## If apply this rule?
| None


 --- 
 --- 