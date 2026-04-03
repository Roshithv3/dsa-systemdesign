# Two Pointers — A Simple Introduction

## What is the Two Pointers Technique?

The **Two Pointers** technique is a simple but powerful trick used in coding problems.

Instead of using two nested loops (slow), you use **two index variables** called "pointers" to scan through an array or string smartly.

This reduces time complexity from **O(n²) → O(n)** — your code runs much faster.

---

## What is a Pointer?

A **pointer** is just a variable that holds an **index (position)** in an array or string.

For example, in the array `[0, 1, 2, 3, 4]`:
- `left = 0` → points to the first element
- `right = 4` → points to the last element

That's it. A pointer is just a number saying "where you are" in the array.

---

## The 3 Types of Two Pointers

---

## Type 1 — Opposite Direction (Converging Pointers)

One pointer starts at the **beginning**, the other at the **end**. They move **toward each other**.

```
[1, 2, 3, 4, 5]
 ↑           ↑
left        right
```

- Moving `left` rightward → gets a **bigger** value (in sorted array)
- Moving `right` leftward → gets a **smaller** value (in sorted array)

---

### Example — Two Sum in a Sorted Array

**Problem:** In `[1, 2, 4, 6, 8]`, find two numbers that add up to `10`.

**Walkthrough:**
```
Array:  [ 1,  2,  4,  6,  8 ]
Index:    0   1   2   3   4

Step 1: left=0 (val=1), right=4 (val=8) → sum=9  → too small → move left →
Step 2: left=1 (val=2), right=4 (val=8) → sum=10 → ✅ Found! return [1, 4]
```

**Pseudocode:**
```
function twoSum(array, target):
    left  = 0
    right = length(array) - 1

    while left < right:
        sum = array[left] + array[right]

        if sum == target:
            return [left, right]        // found the pair
        else if sum < target:
            left = left + 1             // need bigger value
        else:
            right = right - 1           // need smaller value

    return "no pair found"
```

---

### LeetCode Problems — Opposite Direction

| # | Problem |
| - | ------- |
| [167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | Two Sum II - Input Array Is Sorted |
| [125](https://leetcode.com/problems/valid-palindrome/) | Valid Palindrome |
| [680](https://leetcode.com/problems/valid-palindrome-ii/) | Valid Palindrome II |
| [977](https://leetcode.com/problems/squares-of-a-sorted-array/) | Squares of a Sorted Array |
| [11](https://leetcode.com/problems/container-with-most-water/) | Container With Most Water |
| [15](https://leetcode.com/problems/3sum/) | 3Sum |
| [16](https://leetcode.com/problems/3sum-closest/) | 3Sum Closest |
| [42](https://leetcode.com/problems/trapping-rain-water/) | Trapping Rain Water |

> **Recognition tip:** Sorted array + find a pair/triplet = almost always Opposite Direction.

---

## Type 2 — Same Direction (Parallel Pointers)

Both pointers start at the **same end** and move **forward**, but play different roles:

```
[1, 2, 3, 4, 5]
 ↑  ↑
 S  F    ← Slow tracks progress, Fast explores ahead
```

- **Fast pointer** — scans ahead looking for something useful
- **Slow pointer** — marks the "safe" or "processed" zone

Two popular sub-patterns:

- **Fast & Slow** — used for in-place array edits, cycle detection
- **Sliding Window** — both pointers define an expanding/shrinking range

---

### Example — Remove Duplicates from Sorted Array

**Problem:** In `[1, 1, 2, 3, 3]`, remove duplicates in-place. Return count of unique elements.

**Idea:** `slow` marks where the next unique value should be placed. `fast` scans for new values.

**Walkthrough:**
```
Array: [ 1,  1,  2,  3,  3 ]
         ↑   ↑
        slow fast

Step 1: fast(val=1) == slow(val=1) → duplicate, skip → fast moves to index 2
Step 2: fast(val=2) != slow(val=1) → new! → slow moves to index 1, place 2
Step 3: fast(val=3) != slow(val=2) → new! → slow moves to index 2, place 3
Step 4: fast(val=3) == slow(val=3) → duplicate, skip → done

Result: [ 1, 2, 3, _, _ ] → return 3
```

**Pseudocode:**
```
function removeDuplicates(array):
    slow = 0

    for fast = 1 to length(array) - 1:
        if array[fast] != array[slow]:
            slow = slow + 1              // move slow forward
            array[slow] = array[fast]    // place the new unique value

    return slow + 1                      // count of unique elements
```

---

### Sub-pattern — Sliding Window

Both pointers define the **edges of a window** (a sub-range). The window **grows** (move right side out) or **shrinks** (move left side in) based on a condition.

```
[ 1, 2, 3, 4, 5 ]
  ↑        ↑
left      right   ← this range is your "window"
```

**Rule of thumb:**
- Grow the window when the condition is not yet met.
- Shrink the window from the left when the condition is violated.

---

#### Example — Maximum Sum Subarray of Size K

**Problem:** In `[2, 1, 5, 1, 3, 2]`, find the maximum sum of any subarray of size `k = 3`.

**Walkthrough:**
```
Array:  [ 2,  1,  5,  1,  3,  2 ]
Index:    0   1   2   3   4   5

window [0..2] = 2+1+5 = 8
window [1..3] = 1+5+1 = 7
window [2..4] = 5+1+3 = 9  ← new max
window [3..5] = 1+3+2 = 6

Answer: 9
```

**Pseudocode:**
```
function maxSumSubarray(array, k):
    windowSum = sum of array[0..k-1]   // first window
    maxSum    = windowSum

    for i = k to length(array) - 1:
        windowSum = windowSum + array[i] - array[i - k]  // slide: add new, drop old
        maxSum = max(maxSum, windowSum)

    return maxSum
```

---

### LeetCode Problems — Fast & Slow

| # | Problem |
| - | ------- |
| [26](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | Remove Duplicates from Sorted Array |
| [27](https://leetcode.com/problems/remove-element/) | Remove Element |
| [283](https://leetcode.com/problems/move-zeroes/) | Move Zeroes |
| [141](https://leetcode.com/problems/linked-list-cycle/) | Linked List Cycle |
| [876](https://leetcode.com/problems/middle-of-the-linked-list/) | Middle of the Linked List |
| [75](https://leetcode.com/problems/sort-colors/) | Sort Colors |
| [142](https://leetcode.com/problems/linked-list-cycle-ii/) | Linked List Cycle II |

### LeetCode Problems — Sliding Window

| # | Problem |
| - | ------- |
| [3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Longest Substring Without Repeating Characters |
| [209](https://leetcode.com/problems/minimum-size-subarray-sum/) | Minimum Size Subarray Sum |
| [487](https://leetcode.com/problems/max-consecutive-ones-ii/) | Max Consecutive Ones II |
| [424](https://leetcode.com/problems/longest-repeating-character-replacement/) | Longest Repeating Character Replacement |
| [76](https://leetcode.com/problems/minimum-window-substring/) | Minimum Window Substring |

---

## Type 3 — Trigger-Based Pointers

Both pointers start together, but the **second pointer only moves when a specific condition is triggered** by the first.

- **`fast`:** Moves through every element one by one.
- **`slow`:** Stays in place. It only moves forward when `fast` finds something new.

---

### Example — Remove Duplicates from Sorted Array

**Problem:** In `[1, 1, 2, 2, 3]`, remove duplicates in-place so unique elements are at the front. Return the count.

**Walkthrough:**
```
Array:  [ 1,  1,  2,  2,  3 ]
          ↑   ↑
        slow fast

Step 1: fast sees 1. Same as slow (1). No trigger → fast moves on.

Step 2: fast sees 2. Different from slow! ← TRIGGER
        → slow steps forward.
        → slow's new position gets the value 2.

        Array: [ 1,  2,  2,  2,  3 ]
                     ↑       ↑
                   slow     fast

Step 3: fast sees 2. Same as slow (2). No trigger → fast moves on.

Step 4: fast sees 3. Different from slow! ← TRIGGER
        → slow steps forward.
        → slow's new position gets the value 3.

        Array: [ 1,  2,  3,  2,  3 ]
                         ↑       ↑
                        slow    fast

Done! Unique elements at front: [ 1, 2, 3, ... ] → return 3
```

**Pseudocode:**
```
function removeDuplicates(array):
    slow = 0

    for fast = 1 to length(array) - 1:

        // trigger: fast found a new unique value
        if array[fast] != array[slow]:
            slow = slow + 1             // slow steps forward
            array[slow] = array[fast]   // write the new value

    return slow + 1    // total unique elements
```

---

### LeetCode Problems — Trigger-Based

| # | Problem |
| - | ------- |
| [234](https://leetcode.com/problems/palindrome-linked-list/) | Palindrome Linked List |
| [19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | Remove Nth Node From End of List |
| [61](https://leetcode.com/problems/rotate-list/) | Rotate List |
| [143](https://leetcode.com/problems/reorder-list/) | Reorder List |

> **Recognition tip:** "Kth from the end" or "fixed gap between two pointers" = Trigger-Based.

---

## When to Use Two Pointers — Quick Guide

| Situation                                       | Variant                         |
| ----------------------------------------------- | ------------------------------- |
| Sorted array, find pair/triplet with target sum | Opposite Direction              |
| Palindrome check                                | Opposite Direction              |
| Modify array in-place, no extra space           | Same Direction                  |
| Find longest/shortest subarray                  | Same Direction (Sliding Window) |
| Detect cycle in linked list                     | Same Direction (Fast & Slow)    |
| Kth node from end of linked list                | Trigger-Based                   |

---

## When NOT to Use Two Pointers

- Array is **unsorted** and sorting would change the answer
- No clear relationship between pointer positions and the condition
- You need **all pairs**, not just existence of one
- Elements to compare are **non-contiguous** or arbitrary

---

## Summary

| Type                   | How it works                                          | Best for                               |
| ---------------------- | ----------------------------------------------------- | -------------------------------------- |
| **Opposite Direction** | Start from both ends, move inward                     | Sorted arrays, palindromes             |
| **Same Direction**     | Both from one end, different speeds/roles             | In-place edits, sliding window, cycles |
| **Trigger-Based**      | Second pointer activates after first hits a condition | Kth from end, fixed-gap problems       |
