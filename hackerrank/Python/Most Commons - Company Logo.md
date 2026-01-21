## Challenge link: https://www.hackerrank.com/challenges/most-commons/problem?isFullScreen=true

# Challenge: Company Logo - Most Commons

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

## Explanation: