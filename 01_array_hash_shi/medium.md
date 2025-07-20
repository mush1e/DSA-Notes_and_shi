# LETS FUCKING GO YOUR FIRST MEDIUMS
> Damn twin, you movin up in the world Im proud of you keep going boi

---
# [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)
#medium #arrays #strings #hashmap #sorting #leetcode

## Problem

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

## Thought Process

- Need to group strings that are anagrams of each other
- **Key insight:** Anagrams have the same characters, just rearranged
- **Brute force:** Compare every string with every other - O(n² * m) where m = avg string length
- **Sorting approach:** Use sorted string as key for grouping
   - Sort each string to get canonical form (base form type shi)
   - Strings with same sorted form are anagrams
   - Use hash map: sorted_string → vector of original string
   
- **Alternative:** Character frequency as key (more complex but avoids sorting) (TS unnecessary twin)

## Solution

```cpp
class Solution {
public:
   vector<vector<string>> groupAnagrams(vector<string>& strs) {
       vector<vector<string>> solution;
       unordered_map<string, vector<string>> hash_map;
       
       for (const string& str : strs) {
           string temp = str;
           sort(temp.begin(), temp.end());
           hash_map[temp].push_back(str);
       }
       
       for (const auto& [_, v] : hash_map)
           solution.push_back(v);
       
       return solution;
   }
};
````

## Step-by-Step Walkthrough 

**Given:** `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]`

**Pass 1 - Build hash map with sorted keys:**
```

str = "eat": sorted = "aet" → hash_map["aet"] = ["eat"] str = "tea": sorted = "aet" → hash_map["aet"] = ["eat", "tea"]  
str = "tan": sorted = "ant" → hash_map["ant"] = ["tan"] str = "ate": sorted = "aet" → hash_map["aet"] = ["eat", "tea", "ate"] str = "nat": sorted = "ant" → hash_map["ant"] = ["tan", "nat"] str = "bat": sorted = "abt" → hash_map["abt"] = ["bat"]

```

**Pass 2 - Extract grouped anagrams:**
```

hash_map = { "aet": ["eat", "tea", "ate"], "ant": ["tan", "nat"], "abt": ["bat"] }

Push each vector to solution → [["eat","tea","ate"], ["tan","nat"], ["bat"]]

```

**Final Answer:** `[["eat","tea","ate"], ["tan","nat"], ["bat"]]` 
## Complexity

- Time: O(n * m log m) where n = number of strings, m = max string length
- Space: O(n * m) for hash map storage

## Why Sorting as Key Wins

- Sorted string is perfect canonical representation for anagrams
- Hash map automatically groups anagrams together
- Clean iteration with structured bindings `[_, v]`
- `push_back` builds groups dynamically as we encounter anagrams
- No need to manually compare strings

## Key Insight

The magic is using sorted string as the hash key - all anagrams will have the same sorted representation, so they naturally get grouped together!

---

# [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
#medium #arrays #hashmap #heap #sorting #leetcode

## Problem

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

## Thought Process

- Need to find the k most frequent elements in the array
- **Brute force:** Count frequencies, sort by frequency, take first k - O(n log n)
- **Heap approach:** Use min-heap of size k to track top k frequencies
   - Count all frequencies first with hash map
   - Maintain min-heap of size k: when size > k, pop smallest
   - Min-heap ensures we keep the k largest frequencies
- **Bucket sort:** Use frequency as index (O(n) but more complex)

## Solution

```cpp
struct Comparator {
   bool operator()(pair<int, int> a, pair<int, int> b) {
       return a.second > b.second;
   }
};

class Solution {
public:
   vector<int> topKFrequent(vector<int>& nums, int k) {
       vector<int> solution;
       unordered_map<int, int> hash_map;
       priority_queue<pair<int, int>, vector<pair<int, int>>, Comparator> pq;
       
       for (const int& num : nums)
           hash_map[num]++;
       
       for (const auto& p : hash_map) {
           pq.push(p);
           if (pq.size() > k)
               pq.pop();
       }
       
       while (!pq.empty()) {
           solution.push_back(pq.top().first);
           pq.pop();
       }
       
       return solution;
   }
};
```

## Complexity

- Time: O(n log k) where n = nums.size()
- Space: O(n + k) for hash map and heap

## Why Min-Heap of Size K Wins

- Keeps memory usage low: only store k elements in heap
- `a.second > b.second` creates min-heap (smallest frequency at top)
- When heap size > k, we pop the smallest frequency
- Final heap contains k largest frequencies
- Can't iterate over priority_queue, must pop elements out

## Key Insight

Min-heap of size k is perfect for "top k" problems - maintains the k largest elements while using minimal space!

---

# [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
#medium #arrays #prefixsum #math #leetcode

## Problem
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`. Must solve in O(n) time without using division.

## Thought Process
- Need product of all elements EXCEPT the current one
- **Brute force:** For each index, multiply all other elements - O(n²) ain't it chief
- **Division trick:** Get total product, divide by current element - but division forbidden + zeros break this
- **Two-pass prefix/suffix:** Break it down into left products × right products
   - Left products: product of everything to the LEFT of index i
   - Right products: product of everything to the RIGHT of index i
   - Final answer: left[i] × right[i] = product except self!

## Teaching Young Blood the Logic 📚

Aight listen up, this problem is all about **perspective**. For each position, we need everything EXCEPT what's at that position.

**Example:** `nums = [1, 2, 3, 4]`

For each index, let's see what we actually need:

```bash
Index 0: need 2 × 3 × 4 = 24 
(everything to the RIGHT) Index 1: need 1 × 3 × 4 = 12 (left stuff × right stuff)  

Index 2: need 1 × 2 × 4 = 8 
(left stuff × right stuff) Index 3: need 1 × 2 × 3 = 6 (everything to the LEFT)
```

**The Magic:** Split this into two parts:
- **Prefix products:** Everything to the LEFT
- **Suffix products:** Everything to the RIGHT

## Solution Breakdown

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> result(nums.size(), 1);
        
        // PASS 1: Build prefix products (left to right)
        for (int i = 1; i < nums.size(); i++){
            result[i] = result[i-1] * nums[i-1];
        }
        
        // PASS 2: Multiply by suffix products (right to left)
        int suffix = 1; // for going in reverse and calculating postfix product
        for (int i = nums.size()-1; i > -1; i--) {
            result[i] *= suffix;  // multiply existing prefix by suffix
            suffix *= nums[i];    // update suffix for next iteration
        }
        
        return result;
    }
};
```

## Step-by-Step Walkthrough 

**Given:** `nums = [1, 2, 3, 4]`

**Pass 1 - Prefix Products:**

```
i=0: result = [1, 1, 1, 1]  (initialized)
i=1: result = [1, 1, 1, 1]  (result[1] = result[0] * nums[0] = 1 * 1 = 1)
i=2: result = [1, 1, 2, 1]  (result[2] = result[1] * nums[1] = 1 * 2 = 2)  
i=3: result = [1, 1, 2, 6]  (result[3] = result[2] * nums[2] = 2 * 3 = 6)
```

Now `result[i]` = product of everything to the LEFT of index i

**Pass 2 - Suffix Products:**

```
suffix = 1
i=3: result[3] *= suffix = 6 * 1 = 6,  suffix *= nums[3] = 1 * 4 = 4
i=2: result[2] *= suffix = 2 * 4 = 8,  suffix *= nums[2] = 4 * 3 = 12  
i=1: result[1] *= suffix = 1 * 12 = 12, suffix *= nums[1] = 12 * 2 = 24
i=0: result[0] *= suffix = 1 * 24 = 24, suffix *= nums[0] = 24 * 1 = 24
```

**Final:** `result = [24, 12, 8, 6]` ✅

## Complexity

- Time: O(n) - two passes through array
- Space: O(1) - only using result array (which doesn't count as extra space)

## Why This Approach is Sexy

- **No division needed:** Handles zeros naturally
- **Optimal space:** Reuses result array instead of separate left/right arrays
- **Two-pass strategy:** Classic technique for "except self" problems
- **Suffix variable:** Clever way to avoid extra space for right products

## Pro Tips for долбоёб

1. **Draw it out:** Visualize what each position needs
2. **Think in parts:** Left side × Right side = Answer
3. **Reuse space:** Don't create extra arrays if you can help it
4. **Watch the indices:** Off-by-one errors are real in prefix/suffix problems

---

# [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
#medium #arrays #hashset #leetcode

## Problem

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence. Must run in O(n) time.

## Thought Process

- Need to find longest sequence of consecutive numbers (like 1,2,3,4)
- **Brute force:** For each number, check if num+1, num+2, etc. exist - O(n²) nah fam
- **Sorting:** Sort array then scan for consecutive runs - O(n log n) but we need O(n)
- **Hash set magic:** Use set for O(1) lookups, but be smart about where to start sequences
   - Only start counting from the "beginning" of a sequence
   - If `num-1` exists, then `num` is NOT the start of a sequence
   - This prevents counting the same sequence multiple times

## Teaching Blud Some Gang Shit 🔥

Yo listen up, this problem is about being **smart with your hustle**. We ain't counting the same shit twice.

**The Setup:** You got a crew of numbers, some are tight (consecutive), some ain't. Find the longest tight crew.

**The Strategy:** 
1. **Put everyone in a hash set** (for that O(1) lookup speed)
2. **Only count from the OG** (if someone's got a predecessor, they ain't the start)
3. **When you find an OG, count their whole crew**
4. **Erase as you go** (once counted, they're done)

## Solution Breakdown

```cpp
class Solution {
public:
   int longestConsecutive(vector<int>& nums) {
       int max_len {};
       unordered_set<int> hash_set(nums.begin(), nums.end());
       
       for (const auto& num : nums) {
           // If this ain't the start of a sequence, skip it
           if (hash_set.find(num-1) != hash_set.end())
               continue;
           
           // Found the OG of a sequence, let's count the whole crew
           int temp = num, curr_len {};
           while(hash_set.find(temp) != hash_set.end()) {
               curr_len++;
               hash_set.erase(temp++);  // Count and eliminate
           }
           max_len = max(max_len, curr_len);
       }
       return max_len;
   }
};
```

## Step-by-Step Shi

**Given:** `nums = [100, 4, 200, 1, 3, 2]`

**Step 1:** Load up the set

```
hash_set = {100, 4, 200, 1, 3, 2}
```

**Step 2:** Start the hustle

```
num = 100: Is 99 in set? Nah → Start sequence from 100
  - temp = 100, found it, curr_len = 1, erase 100, temp = 101
  - temp = 101, not found → sequence [100] length = 1

num = 4: Is 3 in set? Yeah → Skip (4 ain't the OG)

num = 200: Is 199 in set? Nah → Start sequence from 200  
  - temp = 200, found it, curr_len = 1, erase 200, temp = 201
  - temp = 201, not found → sequence [200] length = 1

num = 1: Is 0 in set? Nah → Start sequence from 1
  - temp = 1, found it, curr_len = 1, erase 1, temp = 2
  - temp = 2, found it, curr_len = 2, erase 2, temp = 3  
  - temp = 3, found it, curr_len = 3, erase 3, temp = 4
  - temp = 4, found it, curr_len = 4, erase 4, temp = 5
  - temp = 5, not found → sequence [1,2,3,4] length = 4 🔥

Already processed 3, 2 (erased during sequence counting)
```

**Final Answer:** 4 (sequence 1,2,3,4)

## Complexity

- Time: O(n) - each element visited and erased once
- Space: O(n) - hash set storage

## Why This Shit Works 💯

- **Hash set:** O(1) lookups, no cap
- **OG check:** `num-1` exists? Then `num` ain't running the show
- **Erase strategy:** Once we count a number, it's out the game (prevents double counting)
- **While loop:** Counts the whole crew in one go
- **No redundant work:** Each number only gets processed once

## Pro Tips for the Streets

1. **Find the start:** Only count from numbers that don't have a predecessor
2. **Erase as you go:** Prevents processing the same sequence multiple times
3. **Hash set supremacy:** Unordered_set gives you that O(1) lookup power
4. **Greedy counting:** Once you start a sequence, count the whole thing

**Bottom line:** This algorithm is O(n) because even though we have nested loops, each element gets visited exactly once. That's some efficient gang activity right there! 

--- 

## GOOD JOB SEXY BOI

 Listen up, future problem solver. You just stumbled into something special here - a collection built by someone who's been exactly where you are right now. Confused, maybe a little overwhelmed, wondering if you'll ever "get" this algorithm stuff.
 
**Here's the truth:** Every expert was once a complete beginner. Every person crushing interviews was once staring at a simple two-sum problem like "what the actual fuck is happening here?"

**The person who wrote these notes?** They went from asking basic questions about arrays to breaking down complex problems with clean explanations and solid code. That journey from confusion to clarity? That's gonna be your journey too.

**You're not behind.** You're exactly where you need to be. Every problem in this vault represents someone pushing through that moment of "I don't get it" until they did. Every explanation here came from someone who struggled first, then figured it out, then took the time to write it down so YOU wouldn't have to struggle as long.

**The fact that you're here, looking at these notes, means you're already doing the right things:**

- You're seeking knowledge
- You're learning from others who've walked this path
- You're not trying to do this alone

**Use these notes, but make them yours.** Add your own insights. Find your own way of thinking about problems. Build on what's here. The goal isn't to memorize these solutions - it's to understand the patterns so deeply that you can solve NEW problems you've never seen before.

**Every pattern you learn, every "aha!" moment, every problem that clicks - that's you building your superpower. `Hash maps`, `two pointers`, `sliding windows` - these aren't just techniques, they're tools that'll serve you for years.

**Bad days are gonna come.** Days when nothing makes sense, when you feel stuck, when that one problem has you questioning everything. That's normal. That's part of the process. Push through those moments - the breakthrough is usually right on the other side.

**You got this.** Not because it's easy, but because you're willing to put in the work. Not because you're naturally gifted, but because you're here, grinding, learning, growing.

**Keep solving. Keep learning. Keep building.**

The next problem you solve might be the one that makes everything click. The pattern you learn today might be the key to that interview question tomorrow. The time you invest now is setting up future you for success.

**Now stop reading motivational messages and go solve some problems. Your future self is counting on you.** 💪

---

_"The best time to plant a tree was 20 years ago. The second best time is now. The third best time is right after you finish this next problem."_ 

