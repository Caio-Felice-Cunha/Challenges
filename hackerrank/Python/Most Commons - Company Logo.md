## Challenge link: https://www.hackerrank.com/challenges/most-commons/problem?isFullScreen=true

# Challenge: Company Logo - Most Commons

A newly opened multinational brand has decided to base their company logo on the three most common characters in the company name. They are now trying out various combinations of company names and logos based on this condition. Given a string s, which is the company name in lowercase letters, your task is to find the top three most common characters in the string.

* Print the three most common characters along with their occurrence count.
* Sort in descending order of occurrence count.
* If the occurrence count is the same, sort the characters in alphabetical order.

For example, according to the conditions described above,

GOOGLE would have it's logo with the letters G,O,E.

Input Format

A single line of input containing the string S.

Constraints

* 3 < len(S) <= 10^4
* S has at least 3 distinct characters


Output Format

Print the three most common characters along with their occurrence count each on a separate line.
Sort output in descending order of occurrence count.
If the occurrence count is the same, sort the characters in alphabetical order.

Sample Input 0
```text
aabbbccde
```

Sample Output 0
```text
b 3
a 2
c 2
```

Explanation 0

aabbbccde

Here, b occurs 3 times. It is printed first.
Both a and c occur 2 times. So, a is printed in the second line and c in the third line because a comes before c in the alphabet.

Note: The string S has at least E distinct characters.

# Answer:

``` sql
#!/bin/python3

import math
import os
import random
import re
import sys



if __name__ == "__main__":
    s = input().strip()
    
    # Count frequencies
    freq = {}
    for c in s:
        freq[c] = freq.get(c, 0) + 1
    
    # Sort by descending frequency, then ascending character
    sorted_items = sorted(freq.items(), key=lambda x: (-x[1], x[0]))
    
    # Print top 3
    for char, count in sorted_items[:3]:
        print(char, count)

``` 

### Result:
<img width="666" height="537" alt="image" src="https://github.com/user-attachments/assets/ac2029e0-a914-4fe3-b985-e6f27d8f250d" />


## Explanation:

## 🧠 Problem Recap

I started by carefully reading the **HackerRank “Company Logo” problem**. The task is:

📌 Given a lowercase string, identify the **three most common characters** and print them along with their counts. Output must be:

1. Sorted **descending by frequency**
2. For ties, sorted **alphabetically ascending**
   This is exactly what the challenge specifies.

So for an input `aabbbccde`, the expected output is:

```
b 3
a 2
c 2
```

Because:

* `b` occurs 3 times → most frequent
* Both `a` and `c` occur 2 times, but `a` is alphabetically before `c`

---

## 🧩 Code Breakdown: Step by Step

Here’s how I reason through a typical Python implementation — reconstructing the logic commonly seen in HackerRank discussions and solutions.

```python
from collections import Counter

if __name__ == "__main__":
    s = input().strip()
```

### 1️⃣ Read and Sanitize Input

I begin by reading the input string and using `.strip()` to remove whitespace. This is important because trailing newline characters shouldn’t affect character counts.

---

```python
# Count frequency of each character
freq = Counter(s)
```

### 2️⃣ Frequency Counting

I use `Counter` from the `collections` module to create a dictionary-like object where:

* keys = characters
* values = counts

For example, given `s = "aabbbccde"`:

```
freq == {'b': 3, 'a': 2, 'c': 2, 'd': 1, 'e': 1}
```

This makes counting easy and efficient.

---

```python
# Sort items by two criteria
sorted_items = sorted(freq.items(), key=lambda x: (-x[1], x[0]))
```

### 3️⃣ Dual-Factor Sorting

This single line is the heart of the problem logic:

* I sort by **frequency descending** → hence `-x[1]`
* For identical frequencies, I sort by **character ascending** → `x[0]`

The `sorted()` function handles multi-criteria sorting in Python gracefully when using a tuple-based `key`.

An example intermediate result after sorting might look like:

```
[('b', 3), ('a', 2), ('c', 2), ('d', 1), ('e', 1)]
```

---

```python
# Print only the top three
for char, count in sorted_items[:3]:
    print(char, count)
```

### 4️⃣ Output the Top 3 Items

I slice the sorted list to take only the first three. This is because the problem explicitly asks for *three lines of output* — the three most common characters.

---

## 📊 Data Analysis of the Result

Given a typical input like:

```
aabbbccde
```

My approach produces:

```
b 3
a 2
c 2
```

which matches the expected output.

Let’s analyze this outcome:

* **`b` is truly dominant** with 3 occurrences.
* `a` and `c` tie in frequency, so alphabetical order matters.
* The fact that sorting must be stable — first by frequency, then by alphabetical order — ensures deterministic output no matter how counts tie.

In terms of computational performance:

* Counting frequencies is **O(n)** for string length `n`
* Sorting is **O(k log k)** where `k` is number of distinct characters (at most 26 for lowercase English letters).

So the combination remains efficient even for the maximum constraint of 10,000 characters.([GoLinuxCloud][5])

---

## 💭 Commentary on the Approach

I find this problem is a great exercise in **multi-criteria sorting** — a concept that shows up often in real coding tasks and interviews.

Key learning points:

* Use built-ins like `Counter` when available — it improves clarity and performance.
* Sorting with a tuple key is far cleaner than custom comparison logic.
* Always consider alphabetical fallback when frequencies tie — it’s easy to overlook.

This approach scales nicely and avoids unnecessary structures like maintaining sorted lists on the fly.

---

## 🔎 Further / Alternative Analysis

### 📌 Alternative Implementation Using Manual Sorting

Instead of using `Counter.most_common()`, one could:

1. Build the frequency dictionary manually
2. Convert it to a list of tuples
3. Sort first alphabetically
4. Then sort by frequency descending

This two-stage sort (alphabetically first, then by frequency) works because Python’s sort is stable. This technique was discussed in HackerRank forums.

Example logic:

```python
items = list(freq.items())
items.sort(key=lambda x: x[0])        # alphabetical
items.sort(key=lambda x: x[1], reverse=True)  # then by frequency
```

This alternative achieves the same result and highlights how sort stability can be leveraged. Sometimes this kind of explicit two-phase sort is easier to reason about for beginners.

---

## 🚀 Final Thoughts

When I implemented this solution, I realized how important it is to break the problem down:

🔹 Understand input → Clean string
🔹 Count frequency → Use a counter
🔹 Sort exactly as specified → Dual criteria
🔹 Return required number of results

This stepwise decomposition keeps the logic transparent, maintainable, and testable.