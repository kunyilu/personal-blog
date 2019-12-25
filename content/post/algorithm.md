---
title: "Algorithm"
date: 2019-10-26T23:03:35-07:00
draft: false
---

_ _ _
# Binary Search
## Key point for binary search
* Ensure the definition of the target is clear
    - smallest larger/smallest larger or equals
    - ...
* Partition the sequence to two halves by "mid"
* Determine if the target exists, it is guaranteed to be in one of the halves
* There should be a comparison operation about "mid" element(use mid to determine for the next round, which half we should refer to)
    - compared to target
    - compared to leftmost
    - compared to rightmost
    - compared to neighbor
    - ...

## Implementation key points
* Loop invariant - the final answer should be within [left, right]
* what is the terminate condition？
    - How many elements left in the search space
* Postprocess when there are two elements left in the search space in the end, like `while (left + 1 < right)`

## How to formulate the problem
1. Determine which part I should choose in the next round of while loop based on specified problem
2. According to the sequence of `size = 3, 2, 1, 0`, try and confirm if termination condition can work for each condition.

## General Binary Search
* Determine the certain search range [a, b] is a sorted range
* Justification Function f(x) = array[i] - i 
* Find the target i in the search range [a, b] such that f(i) satisfies certain rule
* It must be guaranteed that f(x) has monotonic property on range [a, b]


