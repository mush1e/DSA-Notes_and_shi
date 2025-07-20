# THE BEGINNING OF THE END (Easy)
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
# [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)
#easy #strings #hashmap #sorting #leetcode

## Problem
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

## Thought Process
- Need to check if both strings have same characters with same frequencies
- **Brute force:** Generate all permutations of s, check if t matches - O(n!) fuck that
- **Sorting approach:** Sort both strings, compare if equal - O(n log n)
    - Pro: Simple logic, Con: slower than needed
- **Hash map approach:** Count frequency of each char - O(n) time, O(1) space
    - Increment count for chars in s, decrement for chars in t
    - If any count != 0 at end → not anagram

## Solution

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length()) return false;
        
        unordered_map<char, int> hash_map;
        for (const char& ch : s)
            hash_map[ch]++;
        for (const char& ch : t)
            hash_map[ch]--;
        for (const auto& [_, v] : hash_map) {
            if (v) return false;
        }
        return true;
    }
};
```


## Complexity

- Time: O(n) where n = length of strings
- Space: O(1) - at most 26 lowercase letters

## Why Hash Map Wins

- Single pass through both strings
- Counter approach is elegant - increment then decrement
- Early exit if lengths differ
- Structured bindings `[_, v]` for clean iteration

---
# [1. Two Sum](https://leetcode.com/problems/two-sum/)
#easy #arrays #hashmap #leetcode

## Problem

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

## Thought Process

- Need to find two numbers that add up to target, return their indices
- **Brute force:** Check every pair with nested loops - O(n²) time
- **Hash map approach:** Store number → index mapping, then look for complement
   - For each number, check if `target - nums[i]` exists in map
   - If exists and different index → found our pair
- **One pass optimization:** Build map and search simultaneously

## Solution

```cpp
class Solution {
public:
   vector<int> twoSum(vector<int>& nums, int target) {
       unordered_map<int, int> hm;
       for (int i = 0; i < nums.size(); i++)
           hm[nums[i]] = i;
       for (int i = 0; i < nums.size(); i++) {
           if (hm.count(target - nums[i]) && i != hm[target - nums[i]])
               return {i, hm[target - nums[i]]};
       }
       return {};
   }
};
```
## Complexity

- Time: O(n) - two passes through array
- Space: O(n) - hash map storage

## Why Hash Map Wins

- Reduces O(n²) nested loop to O(n) single pass
- `target - nums[i]` gives us the complement we need to find
- Index check `i != hm[target - nums[i]]` prevents using same element twice
- Hash lookups are O(1) average case

## Alternative (One Pass)

cpp

```cpp
// Build map and search simultaneously
unordered_map<int, int> hm;
for (int i = 0; i < nums.size(); i++) {
    if (hm.count(target - nums[i]))
        return {hm[target - nums[i]], i};
    hm[nums[i]] = i;
}
```

--- 

> Aight boi you doin this shit im fucking proud of you. GangShit

