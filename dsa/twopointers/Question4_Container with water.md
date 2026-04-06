# Container With Most Water

**Date:** April 06, 2026
**Technique:** Two Pointers

---

## Problem

Given an array `height[]`, find two lines that form a container holding the most water.

```
Area = min(height[left], height[right]) × (right - left)
```

---

## Approach 1: Brute Force — O(n²)

Try every pair `(i, j)`, compute area, track the max.

```
maxArea = 0
for i in 0 to n-2:
    for j in i+1 to n-1:
        area = min(height[i], height[j]) × (j - i)
        maxArea = max(maxArea, area)
return maxArea
```

> ❌ Too slow for n up to 10⁵

---

## Approach 2: Two Pointers — O(n) ✅

### Key Insight

- Start with the widest container: `L = 0`, `R = n-1`
- Area is capped by the **shorter** line
- Moving the taller pointer inward can only decrease area (width shrinks, height cap stays)
- So always **move the shorter pointer** — it's the only move with upside potential

### Pseudocode

```
left = 0, right = n-1, maxArea = 0

while left < right:
    area = min(height[left], height[right]) × (right - left)
    maxArea = max(maxArea, area)

    if height[left] < height[right]:
        left += 1
    else:
        right -= 1

return maxArea
```

### Walkthrough — `height = [1, 8, 6, 2, 5, 4, 8, 3, 7]`

| Step | L | R | h[L] | h[R] | Area | maxArea | Move  |
|------|---|---|------|------|------|---------|-------|
| 1    | 0 | 8 | 1    | 7    | 8    | 8       | L →   |
| 2    | 1 | 8 | 8    | 7    | 49   | **49**  | ← R   |
| 3    | 1 | 7 | 8    | 3    | 18   | 49      | ← R   |
| 4    | 1 | 6 | 8    | 8    | 40   | 49      | ← R   |
| 5    | 1 | 5 | 8    | 4    | 16   | 49      | ← R   |
| 6    | 1 | 4 | 8    | 5    | 15   | 49      | ← R   |
| 7    | 1 | 3 | 8    | 2    | 4    | 49      | ← R   |
| 8    | 1 | 2 | 8    | 6    | 6    | 49      | ← R   |
| —    | 1 | 1 | —    | —    | —    | —       | STOP  |

**Answer: 49** (lines at index 1 and 8)

---

## Code Implementations

### Python
```python
def maxArea(height: list[int]) -> int:
    left, right = 0, len(height) - 1
    max_area = 0

    while left < right:
        area = min(height[left], height[right]) * (right - left)
        max_area = max(max_area, area)

        if height[left] < height[right]:
            left += 1
        else:
            right -= 1

    return max_area
```

### Java
```java
public int maxArea(int[] height) {
    int left = 0, right = height.length - 1;
    int maxArea = 0;

    while (left < right) {
        int area = Math.min(height[left], height[right]) * (right - left);
        maxArea = Math.max(maxArea, area);

        if (height[left] < height[right])
            left++;
        else
            right--;
    }

    return maxArea;
}
```

### C++
```cpp
int maxArea(vector<int>& height) {
    int left = 0, right = height.size() - 1;
    int maxArea = 0;

    while (left < right) {
        int area = min(height[left], height[right]) * (right - left);
        maxArea = max(maxArea, area);

        if (height[left] < height[right])
            left++;
        else
            right--;
    }

    return maxArea;
}
```

### Go
```go
func maxArea(height []int) int {
    left, right := 0, len(height)-1
    maxArea := 0

    for left < right {
        area := min(height[left], height[right]) * (right - left)
        if area > maxArea {
            maxArea = area
        }

        if height[left] < height[right] {
            left++
        } else {
            right--
        }
    }

    return maxArea
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

---

## Complexity

| Approach     | Time  | Space |
|--------------|-------|-------|
| Brute Force  | O(n²) | O(1)  |
| Two Pointers | O(n)  | O(1)  |

**Takeaway:** Start widest, always move the shorter pointer.
