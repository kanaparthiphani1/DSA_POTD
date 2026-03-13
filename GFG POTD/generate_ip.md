# Generate IP Addresses

## Problem Statement

Given a string `s` containing only digits, restore it by returning **all possible valid IP address combinations**.

A valid IP address must be in the form:


A.B.C.D


Where:

- `A`, `B`, `C`, and `D` are integers in the range **0–255**
- **Leading zeros are not allowed** unless the number itself is `0`

### Valid Examples


1.1.2.11
0.11.21.1
255.6.78.166


### Invalid Examples


01.1.2.11
00.11.21.1
256.1.1.1


If no valid IP addresses can be formed, return an **empty list**.

---

# Example

### Input


s = "255678166"


### Output


[
"25.56.78.166",
"255.6.78.166",
"255.67.8.166",
"255.67.81.66"
]


### Explanation

We insert **3 dots** in the string to divide it into **4 valid segments**.

Each segment must satisfy:


0 ≤ value ≤ 255


and


No leading zeros


---

# Intuition

An IP address always contains **4 segments**:


A.B.C.D


Each segment can have:

- **Minimum digits:** 1  
- **Maximum digits:** 3  

Example:


255678166


Possible first segments:


2
25
255


After choosing a segment, we recursively build the remaining segments.

This creates a **recursion tree**, exploring all valid segment combinations.

---

# Pattern Recognition

This problem follows the **Backtracking + String Partitioning** pattern.

### Indicators

| Indicator | Reason |
|----------|--------|
Generate all combinations | Need to explore multiple possibilities |
Split a string | Partitioning problem |
Segment constraints | Validate each partition |
Small branching factor | Backtracking feasible |

### Pattern


Backtracking + String Partitioning


---

# Backtracking Idea

At each position in the string we decide:


How many digits should the next segment take?


Possible choices:


1 digit
2 digits
3 digits


Example:


255678166
^


Possible segments:


2
25
255


Then we recursively process the remaining string.

---

# Recursion State

We track three variables:


index → current position in string
segments → number of segments formed
path → current IP segments


Example state:


index = 3
segments = 2
path = ["255","6"]


Remaining string:


78166


---

# Base Case

A valid IP is found when:


segments == 4
AND
index == s.length()


Example result:


255.6.78.166


---

# Invalid States

Stop recursion if:


segments == 4 but string still remains


or


string ended but segments < 4


---

# Validation Rules

Before exploring recursion, check:

### 1. Length Bound


index + len ≤ s.length()


---

### 2. Leading Zero Rule

Valid:


0


Invalid:


01
00


Rule:


if segment starts with '0' and length > 1 → invalid


---

### 3. Range Check


value ≤ 255


---

# Backtracking Template

Generic template for partitioning problems:


for each possible segment:

if invalid → skip

choose segment

explore recursion

undo choice

---

# Java Implementation

```java
class Solution {

    public ArrayList<String> generateIp(String s) {
        ArrayList<String> result = new ArrayList<>();
        backtrack(s, 0, 0, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(String s, int index, int segments,
                           ArrayList<String> path,
                           ArrayList<String> result) {

        // valid IP found
        if (segments == 4 && index == s.length()) {
            result.add(String.join(".", path));
            return;
        }

        // invalid states
        if (segments == 4 || index == s.length())
            return;

        // try segment lengths 1 to 3
        for (int len = 1; len <= 3; len++) {

            if (index + len > s.length())
                break;

            String part = s.substring(index, index + len);

            // leading zero check
            if (part.length() > 1 && part.charAt(0) == '0')
                break;

            int value = Integer.parseInt(part);

            if (value > 255)
                continue;

            // choose
            path.add(part);

            // explore
            backtrack(s, index + len, segments + 1, path, result);

            // undo
            path.remove(path.size() - 1);
        }
    }
}

```


Dry Run

Input:

2556

Recursion tree:

2
├──5
│   ├──5
│   │   └──6
│
25
├──5
│   └──6
│
255
└──6

Valid solution:

2.5.5.6
Time Complexity

Each segment has 3 choices.

Depth = 4 segments

3^4 = 81

Therefore:

Time Complexity ≈ O(3^4)

Since depth is constant:

O(1)
Space Complexity
Recursion stack
O(4)
Path storage
O(4)

Total:

O(1)

(ignoring output storage)

Optimization (Interview Level)

Before recursion, check feasibility:

remainingDigits = n - index
remainingSegments = 4 - segments

Valid only if:

remainingSegments ≤ remainingDigits ≤ remainingSegments * 3

Example:

remainingSegments = 2
remainingDigits = 10

Impossible because:

2 ≤ digits ≤ 6

So recursion can stop early.

Similar Problems
Problem	Pattern
Restore IP Addresses	Backtracking
Palindrome Partitioning	String Partition
Word Break	Recursive Partition
Combination Sum	Backtracking
Add Operators to Expression	Backtracking
Key Interview Takeaways

Recognize backtracking when you see:

Generate all combinations
Split a string
Segment validation rules
Small branching factor

Then think:

Backtracking + Partitioning
One-Line Summary

Try every possible 1–3 digit segment, validate it, and recursively construct the remaining IP parts using backtracking.


---

If you want, I can also give you a **much stronger FAANG-style note format** where every DSA problem is structured as:


Pattern
Recognition Signals
Brute Force
Optimized Idea
Template
Code
Complexity
Edge Cases


That format is **extremely powerful for DSA revision**.