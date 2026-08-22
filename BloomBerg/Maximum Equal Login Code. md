# Maximum Equal Login Code

## Problem Statement

You are given two integer arrays:

* `initialLogin`
* `standardLogin`

A security system allows the following operation to be performed on either array any number of times:

> Select any contiguous subarray and replace the entire subarray with the sum of its elements.

For example:

```text
[1, 5, 6, 8, 2]
```

If we select the subarray `[1, 5, 6]`, its sum is `12`, so the array becomes:

```text
[12, 8, 2]
```

The operation can be performed independently on both arrays.

### Objective

Transform both arrays into **identical arrays** while maximizing the length of the resulting arrays.

Return the maximum possible length of the equal arrays.

If the two arrays cannot be transformed into equal arrays, return `-1`.

---

## Example

### Input

```text
initialLogin = [2, 4, 3, 7, 10]
standardLogin = [6, 5, 5, 10]
```

The arrays can be partitioned as:

```text
Initial Login:
[2, 4] [3, 7] [10]
   6     10     10

Standard Login:
[6] [5, 5] [10]
 6     10    10
```

Both arrays can therefore be transformed into:

```text
[6, 10, 10]
```

Hence, the maximum possible length is:

```text
3
```

### Output

```text
3
```

---

## Approach

The key observation is that after applying the operation, every element in the resulting array represents the **sum of a contiguous segment** of the original array.

Therefore, the problem can be viewed as:

> Partition both arrays into the maximum number of contiguous segments such that corresponding segments have equal sums.

Since all elements are positive, we can use a **two-pointer + greedy** approach.

Maintain:

* `r1` → pointer for `initialLogin`
* `r2` → pointer for `standardLogin`
* `sum1` → current segment sum of `initialLogin`
* `sum2` → current segment sum of `standardLogin`

### Pointer movement

If:

```text
sum1 < sum2
```

we need to increase `sum1`, so we consume the next element from `initialLogin`.

If:

```text
sum1 > sum2
```

we consume the next element from `standardLogin`.

If:

```text
sum1 == sum2
```

we have found one pair of matching segments:

```text
count++
```

Then reset both sums and continue looking for the next pair.

Because all elements are positive, consuming an element from the side with the smaller sum is always safe.

---

## Java Solution

```java
import java.util.*;

class Solution {

    public static int loginCode(
            List<Integer> initialLogin,
            List<Integer> standardLogin) {

        int n = initialLogin.size();
        int m = standardLogin.size();

        int r1 = 0;
        int r2 = 0;

        long sum1 = 0;
        long sum2 = 0;

        int count = 0;

        while (r1 < n && r2 < m) {

            if (sum1 < sum2) {
                sum1 += initialLogin.get(r1++);
            }

            else if (sum1 > sum2) {
                sum2 += standardLogin.get(r2++);
            }

            else if (sum1 != 0) {
                // Found two segments with equal sum
                count++;

                sum1 = 0;
                sum2 = 0;
            }

            else {
                // Both sums are zero, so start a new segment
                sum1 += initialLogin.get(r1++);
                sum2 += standardLogin.get(r2++);
            }
        }

        // If both arrays have been completely processed,
        // all segments have already been matched.
        if (r1 == n && r2 == m) {
            return count;
        }

        // Process remaining elements of initialLogin.
        while (r1 < n) {
            sum1 += initialLogin.get(r1++);
        }

        // Process remaining elements of standardLogin.
        while (r2 < m) {
            sum2 += standardLogin.get(r2++);
        }

        // The remaining elements must form one final
        // equal-sum segment.
        if (sum1 == sum2) {
            return count + 1;
        }

        return -1;
    }
}
```

---

## Complexity

Let:

* `N = initialLogin.size()`
* `M = standardLogin.size()`

Each element of both arrays is processed at most once.

### Time Complexity

```text
O(N + M)
```

### Space Complexity

```text
O(1)
```

excluding the input arrays/lists.

---

## Pattern

**Two Pointers + Greedy + Prefix/Segment Sums**

Useful clues that lead to this approach:

* Two arrays need to be made equal.
* Operations work on **contiguous segments**.
* A segment is replaced by its **sum**.
* All values are **positive**.
* We want the **maximum number of matching segments**.

The important reasoning is:

```text
Contiguous segments
        ↓
     Segment sums
        ↓
Two arrays to match
        ↓
 Running sums
        ↓
 Two pointers
        ↓
 Greedy
```
