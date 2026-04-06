# Container With Most Water

**Date:** April 06, 2026  
**Source:** [AlgoMaster.io](https://algomaster.io/learn/dsa/container-with-most-water)  
**Difficulty:** Medium  
**Technique:** Two Pointers

---

## Problem Statement

Given an array `height[]` of `n` non-negative integers representing vertical lines, find two lines that together with the x-axis form a container that holds the **most water**.

```
Area = min(height[left], height[right]) × (right - left)
```

**Constraints:**
- `2 <= n <= 10^5` → O(n²) brute force is too slow; need O(n) or O(n log n)
- `0 <= height[i] <= 10^4` → Heights are non-negative

---

## Visual Representation

### Input: `height = [1, 8, 6, 2, 5, 4, 8, 3, 7]`

```
Index:  0    1    2    3    4    5    6    7    8
       [1]  [8]  [6]  [2]  [5]  [4]  [8]  [3]  [7]

 8  |  |██|  |  |  |  |██|  |  |
 7  |  |██|  |  |  |  |██|  |██|
 6  |  |██|██|  |  |  |██|  |██|
 5  |  |██|██|  |██|  |██|  |██|
 4  |  |██|██|  |██|██|██|  |██|
 3  |  |██|██|  |██|██|██|██|██|
 2  |  |██|██|██|██|██|██|██|██|
 1  |██|██|██|██|██|██|██|██|██|
     0    1    2    3    4    5    6    7    8
```

### Optimal Container (indices 1 and 8)

```
 8  |  |██|~~~~~~~~~~~~~~~~~~~~~~~~|  |
 7  |  |██|  water fills here  |  |~~|
 6  |  |██|  ←— width = 7 —→   |  |██|
 5  |  |██|  height limited by  |  |██|
 4  |  |██|  min(8,7) = 7       |  |██|
    |  |██|                     |  |██|
         L=1                       R=8

Area = min(8, 7) × (8 - 1) = 7 × 7 = 49  ✅ Maximum
```

---

## Approach 1: Brute Force

### Intuition
Try every possible pair `(i, j)` and keep track of the max area seen.

### Step-by-Step

```
Step 1: maxArea = 0

Step 2: For i = 0 to n-2:
           For j = i+1 to n-1:
               area = min(height[i], height[j]) × (j - i)
               maxArea = max(maxArea, area)

Step 3: Return maxArea
```

### Complexity
- **Time:** O(n²) — checks all n*(n-1)/2 pairs
- **Space:** O(1)

> ❌ Too slow for n up to 10⁵

---

## Approach 2: Two Pointers (Optimal)

### Intuition

1. Start with the **widest** possible container: `left = 0`, `right = n-1`
2. Width only decreases as we move inward, so we must try to **increase the height**
3. The area is capped by the **shorter** line → always move the shorter pointer inward
4. Moving the taller pointer can only reduce area (width shrinks, height cap stays the same)

### Step-by-Step Process

```
Initial State:
  height = [1, 8, 6, 2, 5, 4, 8, 3, 7]
  L = 0, R = 8, maxArea = 0

────────────────────────────────────────────────────────
Step 1:  L=0 (h=1), R=8 (h=7)
         area = min(1,7) × (8-0) = 1 × 8 = 8
         maxArea = 8
         → Move L (shorter), L = 1

Step 2:  L=1 (h=8), R=8 (h=7)
         area = min(8,7) × (8-1) = 7 × 7 = 49
         maxArea = 49
         → Move R (shorter), R = 7

Step 3:  L=1 (h=8), R=7 (h=3)
         area = min(8,3) × (7-1) = 3 × 6 = 18
         maxArea = 49 (no change)
         → Move R (shorter), R = 6

Step 4:  L=1 (h=8), R=6 (h=8)
         area = min(8,8) × (6-1) = 8 × 5 = 40
         maxArea = 49 (no change)
         → Heights equal, move either (R), R = 5

Step 5:  L=1 (h=8), R=5 (h=4)
         area = min(8,4) × (5-1) = 4 × 4 = 16
         maxArea = 49 (no change)
         → Move R (shorter), R = 4

Step 6:  L=1 (h=8), R=4 (h=5)
         area = min(8,5) × (4-1) = 5 × 3 = 15
         maxArea = 49 (no change)
         → Move R (shorter), R = 3

Step 7:  L=1 (h=8), R=3 (h=2)
         area = min(8,2) × (3-1) = 2 × 2 = 4
         maxArea = 49 (no change)
         → Move R (shorter), R = 2

Step 8:  L=1 (h=8), R=2 (h=6)
         area = min(8,6) × (2-1) = 6 × 1 = 6
         maxArea = 49 (no change)
         → Move R (shorter), R = 1

Step 9:  L=1, R=1 → L >= R, STOP
────────────────────────────────────────────────────────
Answer: 49
```

### Pointer Movement Decision Tree

```
         At each step:
              │
    ┌─────────▼─────────┐
    │  Compute area at   │
    │  current L and R   │
    └─────────┬─────────┘
              │
    ┌─────────▼─────────┐
    │  Update maxArea    │
    └─────────┬─────────┘
              │
    ┌─────────▼──────────────┐
    │  height[L] < height[R]? │
    └──┬──────────────────┬───┘
       │ YES              │ NO
    move L →           ← move R
    (L += 1)           (R -= 1)
```

### Pseudocode

```
function maxArea(height):
    left  = 0
    right = len(height) - 1
    maxArea = 0

    while left < right:
        h = min(height[left], height[right])
        w = right - left
        maxArea = max(maxArea, h * w)

        if height[left] < height[right]:
            left += 1
        else:
            right -= 1

    return maxArea
```

### Complexity
- **Time:** O(n) — each pointer moves at most n-1 times total
- **Space:** O(1) — only two pointers + one variable

---

## Why the Greedy Choice is Safe

> "Will we ever skip the optimal pair?"

If the optimal answer uses lines at `i` and `j`:
- At some point, one pointer will sit at `i` (or `j`)
- The other pointer moves inward only when it's the **shorter** side
- The shorter pointer moving is the only move with upside potential
- Therefore, the optimal pair `(i, j)` is **always evaluated** before both pointers cross it

✅ The greedy strategy is provably correct.

---

## Summary

| Approach      | Time  | Space | Verdict         |
|---------------|-------|-------|-----------------|
| Brute Force   | O(n²) | O(1)  | ❌ TLE for large n |
| Two Pointers  | O(n)  | O(1)  | ✅ Optimal       |

**Key Takeaway:** Start widest, always move the shorter pointer — this is the only move that has a chance of increasing the area.
