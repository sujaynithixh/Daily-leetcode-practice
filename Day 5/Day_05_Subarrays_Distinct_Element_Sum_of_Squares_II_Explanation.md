# Day 05 — LeetCode 2916: Subarrays Distinct Element Sum of Squares II

**Difficulty:** Hard  
**Topics:** Arrays, Hash Maps, Segment Tree, Lazy Propagation  
**Problem:** https://leetcode.com/problems/subarrays-distinct-element-sum-of-squares-ii/

## Problem

For each non-empty contiguous subarray, count its distinct values, square the count, and add the squares. Return the result modulo `10**9 + 7`.

Example: `nums = [1, 2, 1]`

| Subarray | Distinct count | Square |
| --- | ---: | ---: |
| [1] | 1 | 1 |
| [1,2] | 2 | 4 |
| [1,2,1] | 2 | 4 |
| [2] | 1 | 1 |
| [2,1] | 2 | 4 |
| [1] | 1 | 1 |

Answer: **15**.

## Why the easy approach is too slow

Enumerating subarrays with nested loops and sets takes `O(n²)` time. This hard version needs a faster approach.

## Observation

Process each new element `x = nums[i]` from left to right. Let `prev` be its previous occurrence, or `-1` if unseen. Only the subarrays starting at positions `prev+1` through `i` gain one new distinct value. So their distinct counts all increase by one.

Maintain distinct counts for every possible starting position using a **segment tree with lazy propagation**.

For a segment of length `length` in which each value increases by `val`:

- New sum = old sum + `length * val`
- New sum of squares = old sum of squares + `2 * val * old sum + length * val²`

The second identity comes from `(a+val)² = a² + 2a*val + val²`. Update the square sum **before** updating the ordinary sum.

## Python solution

```python
from typing import List

class Solution:
    def sumCounts(self, nums: List[int]) -> int:
        MOD = 10**9 + 7
        n = len(nums)

        total = [0] * (4 * n)
        square = [0] * (4 * n)
        lazy = [0] * (4 * n)

        def apply(node, length, val):
            square[node] = (square[node] + 2 * val * total[node]
                            + length * val * val) % MOD
            total[node] = (total[node] + length * val) % MOD
            lazy[node] = (lazy[node] + val) % MOD

        def push(node, left, right):
            if lazy[node] == 0 or left == right:
                return
            mid = (left + right) // 2
            apply(node * 2, mid - left + 1, lazy[node])
            apply(node * 2 + 1, right - mid, lazy[node])
            lazy[node] = 0

        def update(node, left, right, ql, qr, val):
            if ql <= left and right <= qr:
                apply(node, right - left + 1, val)
                return

            push(node, left, right)
            mid = (left + right) // 2
            if ql <= mid:
                update(node * 2, left, mid, ql, qr, val)
            if qr > mid:
                update(node * 2 + 1, mid + 1, right, ql, qr, val)

            total[node] = (total[node * 2] + total[node * 2 + 1]) % MOD
            square[node] = (square[node * 2] + square[node * 2 + 1]) % MOD

        last = {}
        ans = 0
        for i, x in enumerate(nums):
            prev = last.get(x, -1)
            update(1, 0, n - 1, prev + 1, i, 1)
            ans = (ans + square[1]) % MOD
            last[x] = i

        return ans
```

## Dry run: [1,2,1]

1. At index 0, value 1 was never seen: update `[0,0]`. Subarrays ending here contribute **1**.
2. At index 1, value 2 was never seen: update `[0,1]`. Subarrays ending here contribute **4+1=5**. Running answer = **6**.
3. At index 2, value 1 last appeared at 0: update `[1,2]`. Subarrays ending here contribute **4+4+1=9**. Running answer = **15**.

## Complexity

- **Time:** `O(n log n)` — one segment-tree range update per array element.
- **Space:** `O(n)` — segment-tree arrays and the last-occurrence dictionary.

## What I learned

The key insight is that adding a value changes the distinct counts for a *range of subarray starts*, not for every start. Lazy propagation makes those updates efficient while tracking both the sum and sum of squares.
