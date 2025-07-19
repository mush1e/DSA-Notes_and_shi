# EASY SHIT
> We all gotta start somewhere :) lets get this shit boye


#  [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) 

#easy #arrays #hashmap #leetcode
## Problem

Given an integer array `nums`, return `true` if any value appears at least twice.

## Thought Process

- Aight so we tryna check if theres a repeating element in our `nums` vector
- **Brute force:** Check every pair - O(n²) hell nah
- **Sorting approach:** Sort first, then check adjacent elements - O(n log n)
    - Pro: O(1) space, Con: slower time
- **Hash set approach:** Track what we've seen - O(n) time, O(n) space
    - As we iterate, if we've seen this number before → duplicate found
    - If we finish the loop without finding duplicates → no duplicates

## Solution

```cpp
bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> seen;
    for (int num : nums) {
        if (seen.count(num)) return true;
        seen.insert(num);
    }
    return false;
}
```

## Complexity

- Time: O(n)
- Space: O(n)

## Why Hash Set Wins

- One pass through array vs sorting overhead
- Early termination when duplicate found
- Code is clean and readable
- Hash lookups are O(1) average case

---
