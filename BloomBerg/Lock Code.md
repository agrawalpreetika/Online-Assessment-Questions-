https://www.fastprep.io/problems/goldman-decrypt-code-lock

# Lock Code

## Problem Statement

You are given an array of integers `codeSequence` of length `n` and an integer `maxValue`.

A locking system allows you to modify any number in the array to **any integer less than or equal to `maxValue`** at a cost of **1 per change**.

Two numbers are **co-prime** if their greatest common divisor (GCD) is `1`.

To unlock the repository, you need to select a number from the array that is co-prime with **all other numbers in the array**.

The lock's code is calculated as:

```text
selected number - total modification cost
```

Your task is to determine the maximum possible lock code by selecting an optimal number after performing any number of modifications.

---

## Function

```java
int decryptCodeLock(int[] codeSequence, int maxValue)
```

### Parameters

* `int[] codeSequence` — the array presented by the code lock.
* `int maxValue` — the maximum possible value for any element in the array.

### Returns

* `int` — the maximum possible lock code.

---

## Examples

### Example 1

```text
codeSequence = [3, 2, 4]
maxValue = 6
```

**Output:**

```text
4
```

**Explanation:**

Change the element `4` to `5` at a cost of `1`.

```text
[3, 2, 4] → [3, 2, 5]
```

Now `5` is co-prime with both `2` and `3`:

```text
gcd(5, 2) = 1
gcd(5, 3) = 1
```

Therefore:

```text
lock code = 5 - 1 = 4
```

---

### Example 2

```text
codeSequence = [1, 2, 3]
maxValue = 6
```

**Output:**

```text
4
```

**Explanation:**

Change:

```text
2 → 5
3 → 6
```

The total modification cost is `2`.

The resulting array is:

```text
[1, 5, 6]
```

The number `6` is co-prime with both `1` and `5`:

```text
gcd(6, 1) = 1
gcd(6, 5) = 1
```

Therefore:

```text
lock code = 6 - 2 = 4
```

---

### Example 3

```text
codeSequence = [2, 4, 6, 8]
maxValue = 8
```

**Output:**

```text
6
```

**Explanation:**

Change `6` to `7` at a cost of `1`.

```text
[2, 4, 6, 8] → [2, 4, 7, 8]
```

The number `7` is co-prime with all the other numbers:

```text
gcd(7, 2) = 1
gcd(7, 4) = 1
gcd(7, 8) = 1
```

Therefore:

```text
lock code = 7 - 1 = 6
```

---

## Constraints

```text
1 <= n <= 10^3
1 <= maxValue <= 10^9
1 <= codeSequence[i] <= maxValue
```

---

## Important Details

* Any number of elements in the array may be modified.
* Every modified element costs exactly `1`, regardless of how much its value changes.
* Every modified value must be at most `maxValue`.
* The selected number must be co-prime with **every other number** in the final array.
* The selected element itself is not considered when checking the co-primality condition.
* The objective is to maximize:

```text
selected number - total modification cost
```

---

## Key Concepts

* GCD / Euclidean Algorithm
* Co-prime numbers
* Number Theory
* Prime Factorization
* Optimization
* Greedy / Mathematical Observation

---

## Complexity Constraints to Keep in Mind

The important constraint is:

```text
maxValue <= 10^9
```

Therefore, a solution that iterates through every value from `1` to `maxValue` is not feasible.

The challenge is to exploit the mathematical structure of the GCD/co-prime condition rather than brute-forcing all possible values.
