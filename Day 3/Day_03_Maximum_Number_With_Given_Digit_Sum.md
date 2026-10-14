# Day 3 - LeetCode Notes
## Problem: Build the Maximum Number With Given Digit Sum

### 1. Problem Understanding
We are given two inputs:

- `num`: number of digits required in the answer
- `sum`: total digit sum required

We need to return the maximum possible number as a string using exactly `num` digits such that the sum of all digits is equal to `sum`.

Example:

```text
num = 2
sum = 3
```

The possible 2-digit numbers with digit sum 3 are:

```text
12, 21, 30
```

The maximum number is:

```text
30
```

So the output is:

```text
"30"
```

---

### 2. Core Idea
To make the number maximum, we should place the largest possible digit at the left side.

Why?

Because in numbers, the leftmost digit has the highest place value.

Example:

```text
30 > 21 > 12
```

All three have digit sum 3, but `30` is largest because `3` is placed at the tens position.

So the greedy idea is:

```text
Put as much value as possible from left to right.
```

A digit can be at most `9`, so for every position we take:

```python
digit = min(9, sum)
```

---

### 3. Code

```python
class Solution:
    def maxSumOfSquares(self, num: int, sum: int) -> str:

        if sum > 9 * num:
            return ""

        ans = []

        for _ in range(num):
            digit = min(9, sum)
            ans.append(str(digit))
            sum -= digit

        return ''.join(ans)
```

---

### 4. Step-by-Step Explanation

#### Step 1: Check if answer is possible

```python
if sum > 9 * num:
    return ""
```

For `num` digits, the maximum digit sum is:

```text
9 * num
```

Example:

```text
num = 2
maximum sum = 9 + 9 = 18
```

So if the given `sum` is greater than `9 * num`, it is impossible to form the answer.

Example:

```text
num = 2
sum = 20
```

Maximum possible sum is only 18, so we return an empty string.

---

#### Step 2: Create answer list

```python
ans = []
```

We use a list to store each digit as a string.

Later, we join all digits together to form the final number.

---

#### Step 3: Fill digits greedily

```python
for _ in range(num):
    digit = min(9, sum)
```

This loop runs exactly `num` times because we need exactly `num` digits.

For every digit position, we choose the maximum possible digit.

If the remaining sum is greater than or equal to 9, we use 9.

If the remaining sum is less than 9, we use the remaining sum.

---

#### Step 4: Add digit and reduce sum

```python
ans.append(str(digit))
sum -= digit
```

We add the digit to the answer list.

Then we reduce the remaining sum.

---

#### Step 5: Convert list into string

```python
return ''.join(ans)
```

The answer is stored as a list like:

```text
['3', '0']
```

Using `join`, it becomes:

```text
"30"
```

---

### 5. Example Walkthrough

Input:

```text
num = 2
sum = 3
```

Initial values:

```text
ans = []
sum = 3
```

#### First iteration

```python
digit = min(9, 3)
```

So:

```text
digit = 3
ans = ['3']
sum = 0
```

#### Second iteration

```python
digit = min(9, 0)
```

So:

```text
digit = 0
ans = ['3', '0']
sum = 0
```

Final answer:

```text
"30"
```

---

### 6. Why Greedy Works

To make a number maximum, the left side should be as large as possible.

So instead of spreading the sum equally, we fill the first digit with the biggest possible value.

Example:

```text
num = 3
sum = 20
```

Greedy gives:

```text
992
```

Because:

```text
9 + 9 + 2 = 20
```

Other combinations like `299`, `929`, or `875` are smaller than `992`.

That is why greedy is correct here.

---

### 7. Time Complexity

```text
O(num)
```

Because the loop runs exactly `num` times.

---

### 8. Space Complexity

```text
O(num)
```

Because we store `num` digits in the answer list.

---

### 9. Beginner-Friendly Summary

This problem is solved using greedy logic.

We always place the largest possible digit from left to right. Since the leftmost digit has the highest value, this gives the maximum number.

The important formula is:

```text
maximum possible digit sum = 9 * num
```

If `sum` is greater than this value, the answer is impossible.

Otherwise, keep taking:

```python
min(9, sum)
```

until all digits are filled.

---

### 10. Key Learning

- Greedy means choosing the best option at the current step.
- To maximize a number, fill larger digits first.
- Each digit can only be from 0 to 9.
- Use `join()` to convert a list of digits into a string.
