# Day 1 Notes — Apply Operations on Array to Maximize Sum of Squares

## Problem

**LeetCode:** Apply Operations on Array to Maximize Sum of Squares  
**Topic:** Bit Manipulation + Greedy  
**Language:** Python

---

## 1. Beginner-Friendly Problem Understanding

We are given an array `nums` and an integer `k`.

Our goal is to perform operations and maximize the final **sum of squares**.

The accepted solution uses a smart idea:

> Count all available bits from the numbers, then build the largest possible numbers for `k` rounds.

Why? Because a larger number gives a much bigger square value.

Example:

```text
8² + 2² = 64 + 4 = 68
5² + 5² = 25 + 25 = 50
```

So, making bigger numbers is better than spreading values evenly.

---

## 2. Main Idea

Every number is represented in binary form.

Example:

```text
2 = 0010
6 = 0110
5 = 0101
8 = 1000
```

Instead of thinking about the numbers directly, we count how many times each bit appears.

Then we use those bits to create the largest possible number in each operation.

---

## 3. Python Code

```python
class Solution:
    def maxSum(self, nums: List[int], k: int) -> int:
        MOD = 10**9 + 7
        cnt = [0] * 30

        for x in nums:
            for b in range(30):
                if x >> b & 1:
                    cnt[b] += 1

        ans = 0

        for _ in range(k):
            cur = 0

            for b in range(30):
                if cnt[b] > 0:
                    cur |= 1 << b
                    cnt[b] -= 1

            ans = (ans + cur * cur) % MOD

        return ans
```

---

## 4. Step-by-Step Code Explanation

### Step 1: Modulo value

```python
MOD = 10**9 + 7
```

The answer can become very large, so we return the result using modulo `1,000,000,007`.

---

### Step 2: Create bit count array

```python
cnt = [0] * 30
```

This stores how many times each bit position appears.

Why `30`?

Because numbers in the problem are usually within a range where 30 bits are enough.

Example:

```text
cnt[0] = count of bit 0
cnt[1] = count of bit 1
cnt[2] = count of bit 2
```

---

### Step 3: Count set bits

```python
for x in nums:
    for b in range(30):
        if x >> b & 1:
            cnt[b] += 1
```

This checks every number and every bit position.

The line:

```python
x >> b & 1
```

checks whether the `b`th bit of `x` is `1`.

Example:

```text
x = 6
Binary of 6 = 110
```

So:

```text
bit 0 = 0
bit 1 = 1
bit 2 = 1
```

So for `6`, we increase the count of bit 1 and bit 2.

---

## 5. Example Walkthrough

Input:

```python
nums = [2, 6, 5, 8]
k = 2
```

Binary form:

```text
2 = 0010
6 = 0110
5 = 0101
8 = 1000
```

Bit count:

| Bit Position | Value | Present In | Count |
|---|---:|---|---:|
| bit 0 | 1 | 5 | 1 |
| bit 1 | 2 | 2, 6 | 2 |
| bit 2 | 4 | 6, 5 | 2 |
| bit 3 | 8 | 8 | 1 |

So:

```text
cnt[0] = 1
cnt[1] = 2
cnt[2] = 2
cnt[3] = 1
```

---

## 6. Building Numbers for `k` Operations

We run this loop `k` times:

```python
for _ in range(k):
    cur = 0
```

Here, `cur` is the number we are building.

---

### Operation 1

Available bits:

```text
bit 0, bit 1, bit 2, bit 3
```

So:

```text
cur = 1 + 2 + 4 + 8 = 15
```

Square:

```text
15² = 225
```

Remaining count:

```text
cnt[0] = 0
cnt[1] = 1
cnt[2] = 1
cnt[3] = 0
```

---

### Operation 2

Available bits:

```text
bit 1, bit 2
```

So:

```text
cur = 2 + 4 = 6
```

Square:

```text
6² = 36
```

Final answer:

```text
225 + 36 = 261
```

Output:

```text
261
```

---

## 7. Important Bitwise Operators

### Right Shift

```python
x >> b
```

Moves the bits of `x` to the right by `b` positions.

---

### AND Operator

```python
& 1
```

Checks whether the last bit is `1`.

---

### OR Operator

```python
cur |= 1 << b
```

Sets the `b`th bit in `cur`.

Example:

```python
cur |= 1 << 3
```

This adds bit value `8` to `cur`.

---

## 8. Why This Approach Works

The solution works because we first collect all bits from the array.

Then, in every operation, we greedily build the largest possible number using available bits.

This maximizes the sum of squares because larger numbers produce larger square values.

Key idea:

```text
To maximize the sum of squares, create larger numbers using available bits.
```

---

## 9. Time Complexity

```text
O(n * 30 + k * 30)
```

Because:

```text
n numbers are checked across 30 bits
k operations are also done across 30 bits
```

Since 30 is constant:

```text
O(n + k)
```

---

## 10. Space Complexity

```text
O(30)
```

Because we use only one count array of size 30.

Since 30 is constant:

```text
O(1)
```

---

## 11. Final Beginner Summary

This problem is solved using **bit manipulation** and **greedy logic**.

First, count how many times each bit appears in all numbers. Then, for each of the `k` operations, create the largest possible number using available bits. Square that number and add it to the answer.

Final result for the example:

```text
nums = [2, 6, 5, 8]
k = 2
answer = 261
```

---

## 12. Revision Points

- Binary representation is important.
- Count set bits from all numbers.
- Use available bits to build larger numbers.
- Larger numbers give larger square values.
- Use modulo because answer may be large.
- Time complexity is `O(n + k)`.
- Space complexity is `O(1)`.
