# ALRIGHT TIME FOR TOPIC 2, GET YOUR CHEEKS READY
> BRO YOU REACHED YOUR SECOND TOPIC, most people fail at the first step :). Proud of you fatty

---

# [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
#easy #twopointers #strings #leetcode

## Problem

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

## Thought Process

- Need to check if string reads same forwards and backwards
- But first gotta clean it up: lowercase + alphanumeric only
- **Brute force:** Clean string, reverse it, compare with original - O(n) time, O(n) space
- **Two pointers approach:** Clean string, then use left/right pointers moving toward center
	- Start from opposite ends, compare characters
	- If any mismatch → not palindrome
	- If pointers meet/cross → palindrome confirmed
- **Optimization:** Could clean and check simultaneously, but cleaner logic to separate concerns

## Solution

```cpp
class Solution {
public:
   void process_string(string& s) {
       // Convert to lowercase
       transform(s.begin(), s.end(), s.begin(), ::tolower);
       // Remove non-alphanumeric characters
       s.erase(remove_if(s.begin(), s.end(), [](char ch) {
           return !isalnum(ch);
       }), s.end());
   }
   
   bool isPalindrome(string s) {
       process_string(s);
       int l_ptr = 0, r_ptr = s.size() - 1;
       
       while (l_ptr <= r_ptr) {
           if (s[l_ptr++] != s[r_ptr--])
               return false;
       }
       return true;
   }
};
```

## Step-by-Step Walkthrough 

**Given:** `s = "A man, a plan, a canal: Panama"`

**Step 1 - Clean the string:**

```
Original: "A man, a plan, a canal: Panama"
Lowercase: "a man, a plan, a canal: panama"  
Alphanumeric only: "amanaplanacanalpanama"
```

**Step 2 - Two pointer dance:**

```
s = "amanaplanacanalpanama"
     l                   r    → s[0]='a', s[20]='a' ✓ match, move inward
      l                 r     → s[1]='m', s[19]='m' ✓ match, move inward
       l               r      → s[2]='a', s[18]='a' ✓ match, move inward
        l             r       → s[3]='n', s[17]='n' ✓ match, move inward
         l           r        → s[4]='a', s[16]='a' ✓ match, move inward
          l         r         → s[5]='p', s[15]='p' ✓ match, move inward
           l       r          → s[6]='l', s[14]='l' ✓ match, move inward
            l     r           → s[7]='a', s[13]='a' ✓ match, move inward
             l   r            → s[8]='n', s[12]='n' ✓ match, move inward
              l r             → s[9]='a', s[11]='a' ✓ match, move inward
               lr             → s[10]='c' (pointers crossed, we're done)
```

**Final Answer:** `true` ✅

## Complexity

- Time: O(n) - one pass to clean + one pass to check
- Space: O(1) - cleaning modifies string in-place (if we don't count input modification)

## Why Two Pointers Wins

- **Opposite ends pattern:** Perfect for palindrome checking
- **Early termination:** Returns false immediately on first mismatch
- **Clean separation:** `process_string()` handles preprocessing cleanly
- **Post-increment/decrement:** `s[l_ptr++] != s[r_ptr--]` moves pointers while comparing
- **In-place processing:** No extra string created

---

## LMAO BOI LOOK AT YOU WISHING THERE WERE MORE EASY QUESTIONS FOR THIS TOPIC 

![Laughing Cat](https://media1.tenor.com/m/aSkdq3IU0g0AAAAC/laughing-cat.gif)


