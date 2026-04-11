# Trapping Rain Water

**Difficulty:** Hard &nbsp;|&nbsp; **Pattern:** Two Pointers (Opposite Direction) &nbsp;|&nbsp; **LeetCode:** [#42](https://leetcode.com/problems/trapping-rain-water/)

---

## Problem Statement

Given an array `height[]` where each element represents the height of a bar (width = 1), calculate **how much rainwater gets trapped** between the bars.

---

## Examples

**Example 1:**
```
Input:  height = [3, 4, 1, 2, 2, 5, 1, 0, 2]
Output: 10
```

**Example 2:**
```
Input:  height = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
Output: 6
```

---

## Constraints

- `n == height.length`
- `1 <= n <= 2 * 10⁴`
- `0 <= height[i] <= 10⁵`

---

## Approach — Two Pointers (Move the Shorter Side)

**Core Insight:**
Water trapped at any index = `min(tallest bar to the LEFT, tallest bar to the RIGHT) − height at that index`

We use two pointers `left` and `right` starting from opposite ends, and track `leftMax` and `rightMax` as we go.

**The Key Rule:**
> **When `leftMax < rightMax`:** move `left` pointer inward.  
> **When `rightMax <= leftMax`:** move `right` pointer inward.

**Why move the shorter side?**
Because the shorter side already has complete info to calculate water safely. If `leftMax < rightMax`, the RIGHT side has a taller wall, meaning `leftMax` is the limiting factor. You can safely calculate water at `left` using `leftMax` without knowing the exact max on the right.

| Condition             | Who controls water?                        | Who do you move?      |
| --------------------- | ------------------------------------------ | --------------------- |
| `leftMax < rightMax`  | leftMax is the shorter wall → it controls  | Move **left** inward  |
| `rightMax <= leftMax` | rightMax is the shorter wall → it controls | Move **right** inward |

---

## Solution

### Python

```python
def trapping_water(heights: list[int]) -> int:
    if not heights:
        return 0

    left, right = 0, len(heights) - 1
    left_max, right_max = heights[left], heights[right]
    count = 0

    while left < right:
        if left_max < right_max:
            left += 1                                  
            if heights[left] >= left_max:
                left_max = heights[left]               
            else:
                count += left_max - heights[left]      
        else:
            right -= 1                                 
            if heights[right] >= right_max:
                right_max = heights[right]             
            else:
                count += right_max - heights[right]    

    return count
```

### Java

```java
public int trappingWater(int[] heights) {
    if (heights == null || heights.length == 0) return 0;

    int left = 0, right = heights.length - 1;
    int leftMax = heights[left], rightMax = heights[right];
    int count = 0;

    while (left < right) {
        if (leftMax < rightMax) {
            left++;                                        
            if (heights[left] >= leftMax) {
                leftMax = heights[left];                   
            } else {
                count += leftMax - heights[left];          
            }
        } else {
            right--;                                       
            if (heights[right] >= rightMax) {
                rightMax = heights[right];                 
            } else {
                count += rightMax - heights[right];        
            }
        }
    }

    return count;
}
```

### C

```c
int trappingWater(int* heights, int n) {
    if (n == 0) return 0;

    int left = 0, right = n - 1;
    int leftMax = heights[left], rightMax = heights[right];
    int count = 0;

    while (left < right) {
        if (leftMax < rightMax) {
            left++;                                        
            if (heights[left] >= leftMax) {
                leftMax = heights[left];                   
            } else {
                count += leftMax - heights[left];          
            }
        } else {
            right--;                                       
            if (heights[right] >= rightMax) {
                rightMax = heights[right];                 
            } else {
                count += rightMax - heights[right];        
            }
        }
    }

    return count;
}
```

### C++

```cpp
int trappingWater(vector<int>& heights) {
    if (heights.empty()) return 0;

    int left = 0, right = heights.size() - 1;
    int leftMax = heights[left], rightMax = heights[right];
    int count = 0;

    while (left < right) {
        if (leftMax < rightMax) {
            left++;                                        
            if (heights[left] >= leftMax) {
                leftMax = heights[left];                   
            } else {
                count += leftMax - heights[left];          
            }
        } else {
            right--;                                       
            if (heights[right] >= rightMax) {
                rightMax = heights[right];                 
            } else {
                count += rightMax - heights[right];        
            }
        }
    }

    return count;
}
```

### Go

```go
func trappingWater(heights []int) int {
    if len(heights) == 0 {
        return 0
    }

    left, right := 0, len(heights)-1
    leftMax, rightMax := heights[left], heights[right]
    count := 0

    for left < right {
        if leftMax < rightMax {
            left++                                         
            if heights[left] >= leftMax {
                leftMax = heights[left]                    
            } else {
                count += leftMax - heights[left]           
            }
        } else {
            right--                                        
            if heights[right] >= rightMax {
                rightMax = heights[right]                  
            } else {
                count += rightMax - heights[right]         
            }
        }
    }

    return count
}
```

### JavaScript

```javascript
var trappingWater = function(heights) {
    if (heights.length === 0) return 0;

    let left = 0, right = heights.length - 1;
    let leftMax = heights[left], rightMax = heights[right];
    let count = 0;

    while (left < right) {
        if (leftMax < rightMax) {
            left++;                                        
            if (heights[left] >= leftMax) {
                leftMax = heights[left];                   
            } else {
                count += leftMax - heights[left];          
            }
        } else {
            right--;                                       
            if (heights[right] >= rightMax) {
                rightMax = heights[right];                 
            } else {
                count += rightMax - heights[right];        
            }
        }
    }

    return count;
};
```

### TypeScript

```typescript
function trappingWater(heights: number[]): number {
    if (heights.length === 0) return 0;

    let left = 0, right = heights.length - 1;
    let leftMax = heights[left], rightMax = heights[right];
    let count = 0;

    while (left < right) {
        if (leftMax < rightMax) {
            left++;                                        
            if (heights[left] >= leftMax) {
                leftMax = heights[left];                   
            } else {
                count += leftMax - heights[left];          
            }
        } else {
            right--;                                       
            if (heights[right] >= rightMax) {
                rightMax = heights[right];                 
            } else {
                count += rightMax - heights[right];        
            }
        }
    }

    return count;
}
```

---

## Complexity Analysis

|           | Complexity                                              |
| --------- | ------------------------------------------------------- |
| **Time**  | O(n) — each element visited exactly once                |
| **Space** | O(1) — only 4 variables: left, right, leftMax, rightMax |

---

## Dry Run

```
height = [3, 4, 1, 2, 2, 5, 1, 0, 2]
```

| Step | left | right | leftMax | rightMax | Action                                 | Water Added                  | Total    |
| ---- | ---- | ----- | ------- | -------- | -------------------------------------- | ---------------------------- | -------- |
| Init | 0    | 8     | 3       | 2        | —                                      | —                            | 0        |
| 1    | 0    | 8     | 3       | 2        | rightMax(2) <= leftMax(3) → move right | 0 (h[7]=0: 2-0=2)            | 2        |
| 2    | 0    | 7     | 3       | 2        | rightMax(2) <= leftMax(3) → move right | 1 (h[6]=1: 2-1=1)            | 3        |
| 3    | 0    | 6     | 3       | 5        | rightMax updated to 5                  | 0                            | 3        |
| 4    | 0    | 5     | 3       | 5        | leftMax(3) < rightMax(5) → move left   | 0 (h[1]=4: update leftMax=4) | 3        |
| 5    | 1    | 5     | 4       | 5        | leftMax(4) < rightMax(5) → move left   | 3 (h[2]=1: 4-1=3)            | 6        |
| 6    | 2    | 5     | 4       | 5        | leftMax(4) < rightMax(5) → move left   | 2 (h[3]=2: 4-2=2)            | 8        |
| 7    | 3    | 5     | 4       | 5        | leftMax(4) < rightMax(5) → move left   | 2 (h[4]=2: 4-2=2)            | 10       |
| 8    | 4    | 5     | 5       | 5        | left meets right → stop                | —                            | **10** ✅ |

---

## Visual Walkthrough

```
height = [3, 4, 1, 2, 2, 5, 1, 0, 2]

   5         █
   4    █    █
   3 █  █    █
   2 █  █ █  █    █
   1 █  █ █ █ █ █  █
   0 █  █ █ █ █ █ █ █ █
     0  1  2  3  4  5  6  7  8

Water fills:
   5         █
   4    █~~~~█
   3 █  █~~~~█
   2 █  █~█~~█    █
   1 █  █~█~█~█~█  █
   0 █  █ █ █ █ █ █ █ █

~ = trapped water
Total = 3 (idx 2) + 2 (idx 3) + 2 (idx 4) + 1 (idx 6) + 2 (idx 7) = 10
```

---

## Key Takeaways

- **Water formula:** `min(leftMax, rightMax) - height[i]` — always subtract current bar height from the shorter overall wall.
- **Why move the shorter side?** Because the shorter side already has complete info (the other side is guaranteed taller). The shorter wall is the bottleneck.
- **Update max before computing water:** if the new bar is taller than the current max, update the max (no water can be trapped at a new max wall). Otherwise, compute water.
- **Pointer paths:** `left` always increases, `right` always decreases. They never cross each other.
