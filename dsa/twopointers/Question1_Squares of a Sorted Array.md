# Squares of a Sorted Array

## Problem Statement

Given a sorted array of integers (which may contain **negative numbers**), return a new array containing the **squares of each element**, also in **sorted (non-decreasing) order**.

### Example

```
Input:  [-4, -1, 0, 3, 10]
Output: [0, 1, 9, 16, 100]
```

---

## Why Is This Tricky?

Squaring removes the sign, so a large negative number becomes a large positive square.

```
-4 → 16   (large!)
-1 → 1
 0 → 0
 3 → 9
10 → 100  (large!)
```

Simply squaring all values gives `[16, 1, 0, 9, 100]` — which is **not sorted**.

The largest squares always appear at the **two ends** of the original sorted array (either very negative or very positive values). This key insight leads us to the optimal solution.

---

## Approach 1 — Naive (Square + Sort)

### Idea
1. Square every element.
2. Sort the result.

### Python

```python
def sorted_squares_naive(nums):
    return sorted(x ** 2 for x in nums)
```

### Java

```java
public static int[] sortedSquaresNaive(int[] nums) {
    int[] result = new int[nums.length];
    for (int i = 0; i < nums.length; i++)
        result[i] = nums[i] * nums[i];
    Arrays.sort(result);
    return result;
}
```

### Complexity

| | Time | Space |
|---|---|---|
| Naive | O(n log n) | O(n) |

---

## Approach 2 — Two Pointer (Optimal) ✅

### Idea

Use two pointers — one at the **left** end, one at the **right** end — and compare their squares. The larger square goes into the **result array from the back** (right to left).

### Visual Explanation

```
Array:  [-4, -1,  0,  3, 10]
         ↑                ↑
        left            right
        
Result: [_, _, _, _, _]
                      ↑
                     pos (fill from end)
```

**Step-by-step walkthrough** for `[-4, -1, 0, 3, 10]`:

| Step | Left (index) | Right (index) | Left² | Right² | Winner | Result so far         |
|------|-------------|--------------|-------|--------|--------|-----------------------|
| 1    | -4 (idx 0)  | 10 (idx 4)   | 16    | 100    | Right  | [_, _, _, _, **100**] |
| 2    | -4 (idx 0)  | 3  (idx 3)   | 16    | 9      | Left   | [_, _, _, **16**, 100]|
| 3    | -1 (idx 1)  | 3  (idx 3)   | 1     | 9      | Right  | [_, _, **9**, 16, 100]|
| 4    | -1 (idx 1)  | 0  (idx 2)   | 1     | 0      | Left   | [_, **1**, 9, 16, 100]|
| 5    | 0  (idx 2)  | 0  (idx 2)   | 0     | 0      | Right  | [**0**, 1, 9, 16, 100]|

Final result: `[0, 1, 9, 16, 100]` ✅

---

## Python — Two Pointer

```python
def sorted_squares(nums):
    n = len(nums)
    result = [0] * n       # Pre-allocate result array
    left = 0               # Pointer at start
    right = n - 1          # Pointer at end
    pos = n - 1            # Fill result from the back

    while left <= right:
        left_sq  = nums[left]  ** 2
        right_sq = nums[right] ** 2

        if left_sq > right_sq:
            result[pos] = left_sq   # Left square is bigger → place it
            left += 1               # Move left pointer inward
        else:
            result[pos] = right_sq  # Right square is bigger → place it
            right -= 1              # Move right pointer inward

        pos -= 1                    # Move fill position one step back

    return result


# --- Tests ---
print(sorted_squares([-4, -1, 0, 3, 10]))  # [0, 1, 9, 16, 100]
print(sorted_squares([-7, -3, 2, 3, 11]))  # [4, 9, 9, 49, 121]
print(sorted_squares([-5, -3, -1]))        # [1, 9, 25]
print(sorted_squares([1, 2, 3]))           # [1, 4, 9]
print(sorted_squares([0]))                 # [0]
```

### Line-by-line Explanation

| Line | What it does |
|------|-------------|
| `n = len(nums)` | Store length for convenience |
| `result = [0] * n` | Create output array of same size, filled with zeros |
| `left, right = 0, n - 1` | Initialize two pointers at both ends |
| `pos = n - 1` | We fill result from the **right** because we place the largest value first |
| `while left <= right` | Keep going until both pointers meet |
| `left_sq = nums[left] ** 2` | Square the element at the left pointer |
| `right_sq = nums[right] ** 2` | Square the element at the right pointer |
| `if left_sq > right_sq` | Whichever square is larger gets placed at `pos` |
| `left += 1` or `right -= 1` | Move the pointer whose square was used |
| `pos -= 1` | Move fill position one step to the left |

---

## Java — Two Pointer

```java
import java.util.Arrays;

public class SortedSquares {

    public static int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];      // Pre-allocate result array
        int left  = 0;                  // Pointer at start
        int right = n - 1;             // Pointer at end
        int pos   = n - 1;             // Fill result from the back

        while (left <= right) {
            int leftSq  = nums[left]  * nums[left];
            int rightSq = nums[right] * nums[right];

            if (leftSq > rightSq) {
                result[pos] = leftSq;   // Left square is bigger → place it
                left++;                 // Move left pointer inward
            } else {
                result[pos] = rightSq;  // Right square is bigger → place it
                right--;                // Move right pointer inward
            }

            pos--;                      // Move fill position one step back
        }

        return result;
    }

    public static void main(String[] args) {
        System.out.println(Arrays.toString(sortedSquares(new int[]{-4, -1, 0, 3, 10})));
        // Output: [0, 1, 9, 16, 100]

        System.out.println(Arrays.toString(sortedSquares(new int[]{-7, -3, 2, 3, 11})));
        // Output: [4, 9, 9, 49, 121]

        System.out.println(Arrays.toString(sortedSquares(new int[]{-5, -3, -1})));
        // Output: [1, 9, 25]

        System.out.println(Arrays.toString(sortedSquares(new int[]{1, 2, 3})));
        // Output: [1, 4, 9]
    }
}
```

---

## Complexity Comparison

| Approach | Time Complexity | Space Complexity | Notes |
|----------|----------------|-----------------|-------|
| Naive (square + sort) | O(n log n) | O(n) | Simple but not optimal |
| Two Pointer | **O(n)** | **O(n)** | Optimal — single pass |

> **Why O(n)?** Each element is visited exactly once by either the left or right pointer. No inner loops, no sorting.

> **Why O(n) space?** We always need a new output array of size n. The input is read-only.

---

## Edge Cases Handled

| Input | Output | Reason |
|-------|--------|--------|
| `[0]` | `[0]` | Single element |
| `[-3, -2, -1]` | `[1, 4, 9]` | All negatives |
| `[1, 2, 3]` | `[1, 4, 9]` | All positives |
| `[-1, 0, 1]` | `[0, 1, 1]` | Symmetric around zero |
| `[-5, 0, 5]` | `[0, 25, 25]` | Tie between left and right |

---

## Key Takeaways

1. **The largest square is always at one of the two ends** of a sorted array — never in the middle.
2. **Fill the result from back to front** — this lets you place the largest values first without extra shifting.
3. **Two pointers converge inward** — each step either left or right pointer moves, guaranteeing O(n) time.
4. This is a classic example of the **two-pointer technique** applied to a sorted array.