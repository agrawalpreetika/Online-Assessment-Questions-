# API Rate Limiter

## Problem Statement

You are building a rate limiter for incoming requests.

Each request has:

- `user[i]`: the user ID for the `i-th` request.
- `time[i]`: the timestamp, in seconds, for the `i-th` request.

A request from a user is allowed only if, during the previous `t` seconds **inclusive of the current request time**, that user has fewer than `k` allowed requests.

### Additional Details

- The timestamps in `time` are already sorted in non-decreasing order.
- If a request is blocked, it must **not** be counted when evaluating future requests for that user.

Return an integer array `result` of length `n` where:

```text
result[i] = 1 if request i is allowed
result[i] = 0 if request i is blocked
```

---

## Example

### Input

```text
n = 3

user = [1, 1, 1]
time = [6, 10, 15]

k = 2
t = 10
```

### Request Evaluation

| Request Index | User ID | Timestamp | User's Requests in Last 10s | Allowed |
|---|---:|---:|---|---|
| 0 | 1 | 6 | [6] | Yes |
| 1 | 1 | 10 | [6, 10] | Yes |
| 2 | 1 | 15 | [6, 10, 15] | No |

Therefore, the answer is:

```text
[1, 1, 0]
```

---

## Function Description

Complete the function:

```text
getRequests
```

The function accepts the following parameters:

```text
1. INTEGER_ARRAY user
2. INTEGER_ARRAY time
3. INTEGER k
4. INTEGER t
```

### Returns

```text
INTEGER_ARRAY
```

An integer array where each element indicates whether the corresponding request is allowed.

---

## Constraints

```text
1 ≤ n ≤ 2 * 10^5
1 ≤ user[i] ≤ n
1 ≤ time[i] ≤ 10^9
1 ≤ k ≤ n
```

---

## Approach

For each user, we need to keep track of their **allowed requests** that fall within the current `t`-second window.

We can use:

```text
HashMap<UserId, Deque<Timestamp>>
```

Each user's deque contains timestamps of only their previously allowed requests.

For every incoming request:

1. Get the deque corresponding to the current user.
2. Remove all timestamps that are outside the current `t`-second window.
3. Check the number of remaining allowed requests.
4. If the number is less than `k`:
   - Allow the current request.
   - Add its timestamp to the deque.
5. Otherwise:
   - Block the current request.
   - Do NOT add its timestamp to the deque.

Since the previous `t` seconds are inclusive of the current request time, for a request at `currentTime`, the valid interval is:

```text
[currentTime - t + 1, currentTime]
```

Therefore, a timestamp is expired when:

```text
timestamp < currentTime - t + 1
```

---

## Java Solution

```java
import java.util.*;

public class Solution {

    public static List<Integer> getRequests(
            List<Integer> user,
            List<Integer> time,
            int k,
            int t) {

        List<Integer> result = new ArrayList<>();

        Map<Integer, Deque<Integer>> requests = new HashMap<>();

        for (int i = 0; i < user.size(); i++) {

            int userId = user.get(i);
            int currentTime = time.get(i);

            requests.putIfAbsent(userId, new ArrayDeque<>());

            Deque<Integer> queue = requests.get(userId);

            int startTime = currentTime - t + 1;

            // Remove requests outside the current time window
            while (!queue.isEmpty()
                    && queue.peekFirst() < startTime) {

                queue.pollFirst();
            }

            // Allow if fewer than k allowed requests exist
            if (queue.size() < k) {

                result.add(1);
                queue.addLast(currentTime);

            } else {

                // Blocked requests must not be stored
                result.add(0);
            }
        }

        return result;
    }
}
```

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

Every allowed request timestamp is inserted into a deque once and removed at most once.

### Space Complexity

```text
O(n)
```

In the worst case, the map and deques can store up to `n` request timestamps.

---

## Important Edge Cases

### 1. Blocked requests are not counted

If a request is blocked, its timestamp must not be added to the user's deque.

### 2. Different users are independent

Each user maintains their own request history.

### 3. Same timestamps are possible

The timestamps are sorted in **non-decreasing** order, so multiple requests can have the same timestamp.

### 4. Inclusive time window

For:

```text
currentTime = 10
t = 10
```

the valid window is:

```text
[1, 10]
```

Therefore, a request at timestamp `1` is still inside the window.

---

## Key Concept

This problem follows the:

```text
HashMap + Queue/Deque + Sliding Window
```

pattern.

The core data structure is:

```text
User ID → Queue of allowed request timestamps
```

For every request:

```text
Remove expired timestamps
        ↓
Check queue size
        ↓
size < k ?
   /        \
 Yes         No
  ↓           ↓
Allow       Block
  ↓
Store timestamp
```
