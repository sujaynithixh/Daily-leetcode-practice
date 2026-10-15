# Day 04 — Subarrays Distinct Element Sum of Squares I

**LeetCode 2913 · Easy · Python 3 · Arrays / Hash Sets / Nested Loops**

[Problem link](https://leetcode.com/problems/subarrays-distinct-element-sum-of-squares-i/)

## Understanding the question

Given an integer array `nums`, consider every **non-empty contiguous subarray**. Count how many **distinct** (unique) values it contains, square that count, then add the results for all subarrays.

A subarray uses consecutive positions. For example, `[1,2]` is a subarray of `[1,2,1]`, but the two 1s alone do not form a subarray because they are not adjacent.

## Walkthrough — nums = [1,2,1]

| Subarray | Unique values | Distinct count | Square |
|---|---|---:|---:|
| [1] | {1} | 1 | 1 |
| [1,2] | {1,2} | 2 | 4 |
| [1,2,1] | {1,2} | 2 | 4 |
| [2] | {2} | 1 | 1 |
| [2,1] | {1,2} | 2 | 4 |
| [1] | {1} | 1 | 1 |

**Answer = 1 + 4 + 4 + 1 + 4 + 1 = 15.**

For `nums = [1,1]`, the answer is `1 + 1 + 1 = 3`.

## Approach: start index + expanding end index + set

For each starting index `i`, initialize a fresh empty set `seen`. Now move the ending index `j` from `i` to the end. Add each value `nums[j]` to `seen`. Because a set stores only unique values, `len(seen)` gives the distinct count of the current subarray. Add its square to `ans`.

## Accepted Python code

```python
class Solution:
    def sumCounts(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)

        for i in range(n):
            seen = set()

            for j in range(i, n):
                seen.add(nums[j])
                count = len(seen)
                ans += count * count

        return ans
```

## Line-by-line explanation

1. `ans, n = 0, len(nums)`: start the total at zero and record array length.
2. `for i in range(n)`: choose the beginning of the subarray.
3. `seen = set()`: start fresh for every beginning position.
4. `for j in range(i,n)`: extend the end of this subarray one element at a time.
5. `seen.add(nums[j])`: put the next value into the set; duplicates do not increase set size.
6. `count = len(seen)`: count different numbers seen in this subarray.
7. `ans += count * count`: add the square of that count.
8. `return ans`: return the total after considering all subarrays.

## Why it works

Each non-empty contiguous subarray corresponds to exactly one pair `(i,j)` with `0 <= i <= j < n`. The nested loops visit every valid pair exactly once. At each step, the set contains exactly the distinct values from `nums[i:j+1]`. Therefore every subarray contributes the correct squared count once.

## Complexity

**Time: O(n²)** on average. There are `n(n+1)/2` subarrays; adding a value to a set and reading its length take O(1) average time.

**Extra space: O(n)**. The `seen` set may store up to `n` unique values. We do not store all subarrays.

## Common beginner mistakes

- Do not confuse contiguous **subarrays** with non-contiguous **subsequences**.
- Initialize `seen = set()` *inside* the outer loop; otherwise earlier values contaminate later subarrays.
- Square the **distinct count**, not the numbers in the array.
- For `[2,2,3]`, the distinct count is 2, so the squared contribution is 4.

## Learning takeaway

For many array tasks, fix the start position and gradually extend the end position. A set is a handy way to track how many unique values have appeared.

The companion Word notes document includes a black-and-white version of the accepted-submission screenshot.
