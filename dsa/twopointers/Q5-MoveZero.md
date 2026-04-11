# Move Zeroes — Two Pointer

**Date:** 11 April 2025  
**Pattern:** Same-direction two pointers (slow-fast)  
**Difficulty:** Easy  
**LeetCode:** #283

---

## Problem

Given an integer array, move all `0`s to the end **in-place** while keeping the relative order of non-zero elements.

```
Input:  [2, 0, 4, 0, 9]
Output: [2, 4, 9, 0, 0]
```

---

## Intuition

Think of `nextNonZero` as a **write cursor** — it always points to the next slot ready to accept a non-zero value.  
The `i` pointer scans every element. Whenever it finds a non-zero, swap it into the write cursor's slot and advance the cursor.  
Zeroes get pushed back naturally through swaps.

---

## Step-by-Step Logic

```
Array:  [2, 0, 4, 0, 9]
         ^
         nextNonZero = 0
```

| Step | i | nums[i] | Action                        | Array after        | nextNonZero |
|------|---|---------|-------------------------------|--------------------|-------------|
| 1    | 0 | 2 ≠ 0   | swap(0,0) → no change, advance| [2, 0, 4, 0, 9]   | 1           |
| 2    | 1 | 0       | skip                          | [2, 0, 4, 0, 9]   | 1           |
| 3    | 2 | 4 ≠ 0   | swap(1,2)                     | [2, 4, 0, 0, 9]   | 2           |
| 4    | 3 | 0       | skip                          | [2, 4, 0, 0, 9]   | 2           |
| 5    | 4 | 9 ≠ 0   | swap(2,4)                     | [2, 4, 9, 0, 0]   | 3           |

**Result:** `[2, 4, 9, 0, 0]` ✅

---

## Complexity

| | |
|---|---|
| Time  | O(n) — single pass |
| Space | O(1) — in-place    |

---

## Python

```python
def moveZeroes(nums: list[int]) -> None:
    next_non_zero = 0
    for i in range(len(nums)):
        if nums[i] != 0:
            nums[next_non_zero], nums[i] = nums[i], nums[next_non_zero]
            next_non_zero += 1

# Test
nums = [2, 0, 4, 0, 9]
moveZeroes(nums)
print(nums)  # [2, 4, 9, 0, 0]
```

---

## C++

```cpp
#include <vector>
#include <iostream>
using namespace std;

void moveZeroes(vector<int>& nums) {
    int nextNonZero = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] != 0) {
            swap(nums[nextNonZero], nums[i]);
            nextNonZero++;
        }
    }
}

int main() {
    vector<int> nums = {2, 0, 4, 0, 9};
    moveZeroes(nums);
    for (int x : nums) cout << x << " ";  // 2 4 9 0 0
}
```

---

## Java

```java
public class MoveZeroes {
    public static void moveZeroes(int[] nums) {
        int nextNonZero = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                int temp = nums[nextNonZero];
                nums[nextNonZero] = nums[i];
                nums[i] = temp;
                nextNonZero++;
            }
        }
    }

    public static void main(String[] args) {
        int[] nums = {2, 0, 4, 0, 9};
        moveZeroes(nums);
        System.out.println(java.util.Arrays.toString(nums)); // [2, 4, 9, 0, 0]
    }
}
```

---

## Go

```go
package main

import "fmt"

func moveZeroes(nums []int) {
    nextNonZero := 0
    for i := 0; i < len(nums); i++ {
        if nums[i] != 0 {
            nums[nextNonZero], nums[i] = nums[i], nums[nextNonZero]
            nextNonZero++
        }
    }
}

func main() {
    nums := []int{2, 0, 4, 0, 9}
    moveZeroes(nums)
    fmt.Println(nums) // [2 4 9 0 0]
}
```

---

## Key Takeaway

> `nextNonZero` is a **write cursor**. It only advances when a non-zero is placed. Every swap moves a non-zero left and a zero right — zeroes bubble to the end automatically.
