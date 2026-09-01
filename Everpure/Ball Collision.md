# Ball Collision

## Problem Statement

There are `n` balls placed on a 1-dimensional axis, all moving at the same non-zero speed.

For each ball `i`:

- `direction[i]` represents the direction:
  - `-1` for left
  - `1` for right
- `strength[i]` represents the strength of the ball.

The balls are listed in the order of their starting positions from left to right.

### Collision Rules

When two balls collide:

- If one ball has higher strength, it destroys the weaker ball and continues moving in the same direction.
- If both balls have equal strength, both are destroyed.

Return the **zero-based indices of the balls that remain after all collisions**, in ascending order.

---

## Example

### Input

```text
direction = [1, -1]
strength = [2, 1]
```

### Explanation

There are `n = 2` balls.

Ball `0` is somewhere to the left of ball `1`.

- Ball `0` is moving to the right.
- Ball `1` is moving to the left.

Therefore, the two balls will eventually collide.

Since:

```text
strength[0] = 2
strength[1] = 1
```

Ball `0` has higher strength, so it destroys ball `1` and remains.

### Output

```text
[0]
```

---

## Function Description

Complete the function:

```text
findRemainingBalls
```

The function accepts the following parameters:

```text
1. INTEGER_ARRAY direction
2. INTEGER_ARRAY strength
```

### Returns

```text
INTEGER_ARRAY
```

An integer array containing the zero-based indices of the remaining balls in ascending order.

---

## Constraints

```text
1 ≤ n ≤ 10^5
direction[i] is either 1 or -1
1 ≤ strength[i] ≤ 10^9
```

---

## Approach

This problem can be solved using a **stack**.

Since the balls are given in their starting order from left to right, two balls can collide only when a previously encountered ball is moving right and the current ball is moving left.

That is:

```text
Previous ball:  →
Current ball:   ←
```

We maintain a stack containing the indices of balls that are currently alive.

For each ball:

1. If no collision is possible, push its index onto the stack.
2. If the current ball is moving left and the top ball is moving right, a collision occurs.
3. Compare their strengths:
   - If the current ball is stronger, remove the top ball and continue checking for another collision.
   - If the top ball is stronger, the current ball is destroyed.
   - If both have equal strength, both are destroyed.
4. After processing all balls, the stack contains the indices of all surviving balls.

---

## Java Solution

```java
import java.util.*;

public class Solution {

    public static List<Integer> findRemainingBalls(
            List<Integer> direction,
            List<Integer> strength) {

        Deque<Integer> stack = new ArrayDeque<>();

        for (int i = 0; i < direction.size(); i++) {

            boolean destroyed = false;

            while (!stack.isEmpty()
                    && direction.get(stack.peekLast()) == 1
                    && direction.get(i) == -1) {

                int previous = stack.peekLast();

                if (strength.get(previous) < strength.get(i)) {

                    // Previous ball is destroyed
                    stack.pollLast();

                } else if (strength.get(previous).equals(strength.get(i))) {

                    // Both balls are destroyed
                    stack.pollLast();
                    destroyed = true;
                    break;

                } else {

                    // Current ball is destroyed
                    destroyed = true;
                    break;
                }
            }

            if (!destroyed) {
                stack.addLast(i);
            }
        }

        return new ArrayList<>(stack);
    }
}
```

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

Each ball is pushed onto the stack at most once and removed at most once.

### Space Complexity

```text
O(n)
```

In the worst case, all balls survive and are stored in the stack.

---

## Key Concept

This problem follows the **Stack Simulation / Collision** pattern.

A collision is possible only for:

```text
→ ←
```

The other combinations do not result in a collision:

```text
← ←
→ →
← →
```
