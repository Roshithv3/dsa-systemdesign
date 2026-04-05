# 📓 DSA Learning Journal

**Date:** April 05, 2026
**Topic:** Valid Word Abbreviation
**Difficulty:** Easy
---

## 🧩 What is the Problem?

You're given two strings:
- `word` — the original word (e.g., `"internationalization"`)
- `abbr` — an abbreviation that mixes letters and numbers (e.g., `"i12iz4n"`)

Each **number** in the abbreviation means: *"I skipped this many characters from the original word."*

Your task: **Check if the abbreviation correctly matches the word.**

### Example

```
word  = "internationalization"
abbr  = "i12iz4n"

i → matches 'i'
12 → skip 12 characters
i → matches 'i'
z → matches 'z'
4 → skip 4 characters
n → matches 'n'

✅ Valid!
```

---

## ⚠️ Watch Out For

| Rule | Meaning |
|------|---------|
| Multi-digit numbers | `12` means skip 12, NOT skip 1 then skip 2 |
| No leading zeros | `01` or `0` alone is **invalid** |
| Letter must match exactly | If `abbr` has a letter, it must equal the word's character at that position |

---

## 💡 Approach 1: Brute Force (Expand & Compare)

### The Idea

Expand the abbreviation into a full-length string using placeholder characters (`*`), then compare with the original word.

```
abbr = "i12iz4n"
expanded = "i************iz****n"   (12 stars, then 4 stars)
word     = "internationalization"

Compare position by position:
- If expanded has a letter → it must match word's letter
- If expanded has a * → skip (wildcard)
```

### Steps

1. Loop through `abbr`
2. If it's a **letter** → add to expanded
3. If it's a **digit** → parse the full number, add that many `*` placeholders
4. If `expanded.length != word.length` → ❌ false
5. For each non-`*` character → must match `word[i]`

### Complexity

| | |
|--|--|
| Time | O(n + m) |
| Space | O(n) — we build the expanded string |

---

## ✅ Approach 2: Two Pointers (Optimal)

### The Idea

Instead of building an expanded string, use **two pointers** to walk through both strings at the same time.

```
i → pointer for word
j → pointer for abbr
```

- If `abbr[j]` is a **letter** → compare with `word[i]`, move both forward
- If `abbr[j]` is a **digit** → parse full number, jump `i` forward by that amount
- At the end → both pointers must reach the end of their strings

### Visual Walkthrough

```
word = "internationalization"
abbr = "i12iz4n"
```

| Step | abbr[j] | Action | i (word ptr) | j (abbr ptr) |
|------|---------|--------|--------------|--------------|
| 1 | `'i'` | letter → matches `word[0]='i'` ✅ | 0 → 1 | 0 → 1 |
| 2 | `'1'` | digit → parse `'12'`, skip 12 chars | 1 → 13 | 1 → 3 |
| 3 | `'i'` | letter → matches `word[13]='i'` ✅ | 13 → 14 | 3 → 4 |
| 4 | `'z'` | letter → matches `word[14]='z'` ✅ | 14 → 15 | 4 → 5 |
| 5 | `'4'` | digit → parse `'4'`, skip 4 chars | 15 → 19 | 5 → 6 |
| 6 | `'n'` | letter → matches `word[19]='n'` ✅ | 19 → 20 | 6 → 7 |

Both `i == 20` (end of word) and `j == 7` (end of abbr) → **✅ Valid!**

---

## 💻 Code

### Java

```java
class Solution {
    public boolean validWordAbbreviation(String word, String abbr) {
        int i = 0, j = 0;

        while (i < word.length() && j < abbr.length()) {
            char c = abbr.charAt(j);

            if (Character.isDigit(c)) {
                if (c == '0') return false; // leading zero not allowed

                int num = 0;
                while (j < abbr.length() && Character.isDigit(abbr.charAt(j))) {
                    num = num * 10 + (abbr.charAt(j) - '0');
                    j++;
                }
                i += num; // skip `num` characters in word

            } else {
                if (word.charAt(i) != c) return false;
                i++;
                j++;
            }
        }

        return i == word.length() && j == abbr.length();
    }
}
```

---

### Python

```python
def validWordAbbreviation(word: str, abbr: str) -> bool:
    i, j = 0, 0

    while i < len(word) and j < len(abbr):
        if abbr[j].isdigit():
            if abbr[j] == '0':
                return False  # leading zero not allowed

            num = 0
            while j < len(abbr) and abbr[j].isdigit():
                num = num * 10 + int(abbr[j])
                j += 1
            i += num  # skip `num` characters in word

        else:
            if word[i] != abbr[j]:
                return False
            i += 1
            j += 1

    return i == len(word) and j == len(abbr)
```

---

### C++

```cpp
class Solution {
public:
    bool validWordAbbreviation(string word, string abbr) {
        int i = 0, j = 0;

        while (i < word.size() && j < abbr.size()) {
            if (isdigit(abbr[j])) {
                if (abbr[j] == '0') return false; // leading zero not allowed

                int num = 0;
                while (j < abbr.size() && isdigit(abbr[j])) {
                    num = num * 10 + (abbr[j] - '0');
                    j++;
                }
                i += num; // skip `num` characters in word

            } else {
                if (word[i] != abbr[j]) return false;
                i++;
                j++;
            }
        }

        return i == word.size() && j == abbr.size();
    }
};
```

---

### Go

```go
func validWordAbbreviation(word string, abbr string) bool {
    i, j := 0, 0

    for i < len(word) && j < len(abbr) {
        c := abbr[j]

        if c >= '0' && c <= '9' {
            if c == '0' {
                return false // leading zero not allowed
            }

            num := 0
            for j < len(abbr) && abbr[j] >= '0' && abbr[j] <= '9' {
                num = num*10 + int(abbr[j]-'0')
                j++
            }
            i += num // skip `num` characters in word

        } else {
            if word[i] != c {
                return false
            }
            i++
            j++
        }
    }

    return i == len(word) && j == len(abbr)
}
```

---

## 📊 Complexity Summary

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Brute Force | O(n + m) | O(n) | Builds expanded string |
| Two Pointers | O(n + m) | O(1) | No extra memory needed ✅ |

> `n` = length of word, `m` = length of abbr

---

## 🧠 Key Takeaways

- Use **two pointers** — one for `word`, one for `abbr`
- When you see a digit in `abbr`, **parse the full number** (not just one digit)
- **Leading zeros** (`0`, `01`) are always invalid → return false immediately
- At the end, **both pointers must be at the end** of their strings
- This is a classic **string simulation** pattern — walk both strings in sync
