# Day 2 Notes — LeetCode

## Problem: 3727. Maximum Alternating Sum of Squares

## 1. Problem Understanding

We are given an integer array `nums`.

We are allowed to rearrange the elements in any order.

For any arrangement `arr`, the alternating score is calculated like this:

```text
score = arr[0]^2 - arr[1]^2 + arr[2]^2 - arr[3]^2 + ...
```

That means:

```text
Even index  → add square
Odd index   → subtract square
```

Our goal is to rearrange the numbers so that the final score becomes maximum.

---

## 2. Example From Screenshot

### Input

```python
nums = [1, 2, 3]
```

One best rearrangement is:

```python
[2, 1, 3]
```

Now calculate the alternating score:

```text
score = 2^2 - 1^2 + 3^2
score = 4 - 1 + 9
score = 12
```

### Output

```text
12
```

---

## 3. Main Logic

The formula has two types of positions:

```text
Positive positions: arr[0], arr[2], arr[4], ...
Negative positions: arr[1], arr[3], arr[5], ...
```

To maximize the answer:

```text
Put large square values in positive positions.
Put small square values in negative positions.
```

Because positive positions increase the score, and negative positions reduce the score.

---

## 4. Important Observation

Since every number is squared, negative numbers also become positive after squaring.

Example:

```text
(-3)^2 = 9
3^2 = 9
```

So the sign of the original number does not matter.

Only the square value matters.

That is why the code first converts every number into its square.

---

## 5. Code From Screenshot

```python
class Solution:
    def maxAlternatingSum(self, a: List[int]) -> int:
        return sum(s:=sorted(v*v for v in a))-2*sum(s[:len(a)//2])
```

This is a short one-line solution.

Now let us break it down in a beginner-friendly way.

---

## 6. Step-by-Step Explanation

### Step 1: Square all numbers

```python
v * v for v in a
```

This takes every value from the array and converts it into its square.

Example:

```python
a = [1, 2, 3]
```

Squares are:

```text
1^2 = 1
2^2 = 4
3^2 = 9
```

So we get:

```python
[1, 4, 9]
```

---

### Step 2: Sort the square values

```python
s = sorted(v*v for v in a)
```

For the example:

```python
s = [1, 4, 9]
```

Sorting helps us easily separate the smaller square values from the larger square values.

---

### Step 3: Find total sum of all squares

```python
sum(s)
```

For:

```python
s = [1, 4, 9]
```

Total is:

```text
1 + 4 + 9 = 14
```

---

### Step 4: Find how many values must be subtracted

In an array of length `n`, the number of negative positions is:

```python
len(a) // 2
```

For `n = 3`:

```text
3 // 2 = 1
```

So only one value will be placed in a negative position.

To maximize the result, we subtract the smallest square value.

```python
s[:len(a)//2]
```

For:

```python
s = [1, 4, 9]
```

```python
s[:1] = [1]
```

---

## 7. Why the Formula Uses `-2 * sum(...)`

The total sum assumes all square values are added.

Example:

```text
total = 1 + 4 + 9 = 14
```

But one value should actually be subtracted, not added.

If `1` is currently added in total, and we want it to become negative, we need to remove it twice:

```text
14 - 1 - 1 = 12
```

So the formula becomes:

```python
sum(s) - 2 * sum(smallest_negative_values)
```

For the example:

```text
answer = 14 - 2 * 1
answer = 14 - 2
answer = 12
```

---

## 8. Expanded Beginner-Friendly Code

The screenshot uses a very short one-line solution. For learning, this expanded version is easier to understand:

```python
class Solution:
    def maxAlternatingSum(self, nums: List[int]) -> int:
        squares = []

        for value in nums:
            squares.append(value * value)

        squares.sort()

        n = len(nums)
        negative_count = n // 2

        total_sum = sum(squares)
        negative_sum = sum(squares[:negative_count])

        answer = total_sum - 2 * negative_sum

        return answer
```

---

## 9. Dry Run

### Input

```python
nums = [1, 2, 3]
```

### Step 1: Square values

```text
1 → 1
2 → 4
3 → 9
```

```python
squares = [1, 4, 9]
```

### Step 2: Sort values

```python
squares = [1, 4, 9]
```

### Step 3: Count negative positions

```text
n = 3
negative_count = 3 // 2 = 1
```

### Step 4: Pick smallest value for negative position

```python
negative_values = [1]
```

### Step 5: Calculate answer

```text
total_sum = 1 + 4 + 9 = 14
negative_sum = 1
answer = total_sum - 2 * negative_sum
answer = 14 - 2 * 1
answer = 12
```

Final answer:

```text
12
```

---

## 10. Why Sorting Works

We need to subtract exactly `n // 2` values because those values will be placed at odd indexes.

To get maximum score, the values being subtracted should be as small as possible.

So we sort the squared values and subtract the smallest `n // 2` values.

The remaining larger values stay in the positive positions.

---

## 11. Time Complexity

```text
O(n log n)
```

Reason:

```text
Sorting takes O(n log n)
```

Other operations like squaring and summing take `O(n)`.

So the overall time complexity is:

```text
O(n log n)
```

---

## 12. Space Complexity

```text
O(n)
```

Reason:

We create a new list of squared values.

```python
s = sorted(v*v for v in a)
```

So extra space is used to store the squared values.

---

## 13. Beginner-Friendly Summary

This problem is based on sorting and greedy thinking.

We square all numbers first because the formula uses squares. Then we sort them. Since odd index values are subtracted, we place the smallest square values in those negative positions. The larger square values remain in positive positions. This gives the maximum alternating sum.

Main idea:

```text
Subtract the smallest squares and add the largest squares.
```

---

## 14. Key Learning Points

```text
1. Squaring removes the effect of negative signs.
2. Positive positions should get large values.
3. Negative positions should get small values.
4. Sorting helps us choose the smallest values easily.
5. Formula: total_sum - 2 * smallest_negative_sum
```

---

## 15. Final Accepted Code

```python
class Solution:
    def maxAlternatingSum(self, a: List[int]) -> int:
        return sum(s:=sorted(v*v for v in a))-2*sum(s[:len(a)//2])
```

---

## 16. Professional Commit Message

```text
docs: add day 2 LeetCode alternating sum notes
```
