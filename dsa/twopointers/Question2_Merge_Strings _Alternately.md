# Merge Strings Alternately

**LeetCode #1768** | Easy | String

---

## Problem Statement

You are given two strings `word1` and `word2`. Merge the strings by adding letters in alternating order, starting with `word1`. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the merged string.

---

## Examples

**Example 1:**
```
Input:  word1 = "abc", word2 = "pqr"
Output: "apbqcr"
```
```
Merge:  a p b q c r
        ↑ ↑ ↑ ↑ ↑ ↑
       w1 w2 w1 w2 w1 w2
```

**Example 2:**
```
Input:  word1 = "ab", word2 = "pqrs"
Output: "apbqrs"
```
```
Merge:  a p b q r s
        ↑ ↑ ↑ ↑ ↑ ↑
       w1 w2 w1 w2 w2 w2   ← word2 is longer, append remaining
```

**Example 3:**
```
Input:  word1 = "abcd", word2 = "pq"
Output: "apbqcd"
```
```
Merge:  a p b q c d
        ↑ ↑ ↑ ↑ ↑ ↑
       w1 w2 w1 w2 w1 w1   ← word1 is longer, append remaining
```

---

## Constraints

- `1 <= word1.length, word2.length <= 100`
- `word1` and `word2` consist of lowercase English letters

---

## Approach — For Loop (Single Index)

Use a single index `i` that runs up to `max(len(word1), len(word2))`. In each iteration, append from `word1[i]` and `word2[i]` if the index is still within bounds for each string.

### Steps

1. Loop `i` from `0` to `max(len(word1), len(word2)) - 1`
   - If `i < len(word1)` → append `word1[i]`
   - If `i < len(word2)` → append `word2[i]`
2. Return result

---

## Solution

### Python
```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        result = []
        for i in range(max(len(word1), len(word2))):
            if i < len(word1):
                result.append(word1[i])
            if i < len(word2):
                result.append(word2[i])
        return "".join(result)
```

### Java
```java
class Solution {
    public String mergeAlternately(String word1, String word2) {
        StringBuilder sb = new StringBuilder();
        int maxLen = Math.max(word1.length(), word2.length());

        for (int i = 0; i < maxLen; i++) {
            if (i < word1.length()) sb.append(word1.charAt(i));
            if (i < word2.length()) sb.append(word2.charAt(i));
        }

        return sb.toString();
    }
}
```

### C++
```cpp
class Solution {
public:
    string mergeAlternately(string word1, string word2) {
        string result;
        int maxLen = max(word1.size(), word2.size());

        for (int i = 0; i < maxLen; i++) {
            if (i < word1.size()) result += word1[i];
            if (i < word2.size()) result += word2[i];
        }

        return result;
    }
};
```

### Go
```go
func mergeAlternately(word1 string, word2 string) string {
    result := []byte{}
    maxLen := len(word1)
    if len(word2) > maxLen {
        maxLen = len(word2)
    }

    for i := 0; i < maxLen; i++ {
        if i < len(word1) {
            result = append(result, word1[i])
        }
        if i < len(word2) {
            result = append(result, word2[i])
        }
    }

    return string(result)
}
```

---

## Complexity Analysis

| | Complexity |
|---|---|
| **Time** | O(m + n) — iterate through both strings once |
| **Space** | O(m + n) — result string of combined length |

Where `m = len(word1)` and `n = len(word2)`.

---

## Dry Run

```
word1 = "ab",  word2 = "pqrs"

i=0, j=0  →  append 'a', 'p'  →  "ap"
i=1, j=1  →  append 'b', 'q'  →  "apbq"
i=2, j=2  →  i out of bounds, append 'r'  →  "apbqr"
i=2, j=3  →  i out of bounds, append 's'  →  "apbqrs"

Output: "apbqrs" ✓
```

---

## Key Takeaways

- Single index `i` looping up to `max(m, n)` is cleaner than managing two separate pointers
- `Math.max(word1.length(), word2.length())` drives the loop — ensures the longer string's tail is never missed
- Using `StringBuilder` in Java is preferred over `String +=` due to mutability (avoids creating new string objects each iteration)

---

