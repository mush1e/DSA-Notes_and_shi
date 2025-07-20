# The real work begins
> Its about to be kinda fucked for you NGL. time to get COOKED

--- 

# [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
#easy #twopointers #arrays #leetcode

## Problem

Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Return the indices of the two numbers (1-indexed) as an integer array.

## Thought Process

- Yo it's Two Sum but this time the array actually got its life together and decided to be sorted
- This means we can be big brain about it instead of using hash maps like it's 2019
- **Brute force:** Check every pair like we got all day - O(n²) because apparently we choose violence
- **Hash map approach:** Pull out the classic Two Sum playbook - O(n) time but takes up space like that friend who never leaves your couch
- **Two pointers approach:** Use that sorted energy to our advantage - O(n) time, O(1) space because we're different like that
	- Left pointer starts at the beginning, right pointer at the end 
	- Sum too small? Move left pointer right to get bigger numbers fr
	- Sum too big? Move right pointer left to get smaller numbers real quick
	- Sum hits different and equals target? We found it gang, time to dip

## Solution

```cpp
class Solution {
public:
   vector<int> twoSum(vector<int>& numbers, int target) {
       int l {}, r {(int)numbers.size()-1};
       
       while (l < r) {
           if (numbers[l] + numbers[r] == target)
               return {l+1, r+1};  // +1 because 1-indexed (cringe)
           (numbers[l] + numbers[r] > target) ? r-- : l++;
       }
       return {};  // This ain't supposed to happen but compiler be trippin
   }
};
```

## Step-by-Step Walkthrough

**Given:** `numbers = [2,7,11,15], target = 9`

**Time for the pointer shit gang:**

```
Starting formation: l=0, r=3
numbers = [2, 7, 11, 15]
           l           r

Check: 2 + 15 = 17
17 > 9? That's a whole ass "too much" situation
Move right pointer left: r-- (we need to chill)

numbers = [2, 7, 11, 15]
           l      r

Check: 2 + 11 = 13  
13 > 9? Still sending too much energy
Move right pointer left again: r-- (keep it lowkey)

numbers = [2, 7, 11, 15]
           l   r

Check: 2 + 7 = 9
9 == 9? That's our answer right there
Return {l+1, r+1} = {1, 2} (1-indexed because someone decided to be different)
```

**Final Answer:** `[1, 2]` *YUUUURRRRRR*

## Complexity

- Time: O(n) - worst case we check each element once as pointers slide toward each other
- Space: O(1) - just two lil integers, no extra storage needed blud

## Why This Shit Works 💯

- **Sorted array advantage:** We can eliminate possibilities like we're playing guess who
- **Two pointer coordination:** Left and right work together like they rehearsed this
- **Smart movement logic:** Each move gets us closer to the answer, no wasted steps
- **O(1) space flex:** No extra storage needed, just pure pointer discipline  
- **Guaranteed solution:** Problem says there's exactly one answer so we literally can't miss

## Pro Tips for the Streets

1. **Trust the sorted order:** Use it to make smart decisions about pointer movement
2. **Ternary operator supremacy:** One line decisions keep the code clean as hell
3. **Watch for 1-indexed bullshit:** Don't forget to add 1 to your final answer
4. **Early return energy:** Soon as you find it, you're out - no unnecessary work
5. **Pointer discipline:** Each move should have a purpose, no random sliding around

**Bottom line:** This algorithm is O(n) because each pointer moves at most n times total, and we're using the sorted property to eliminate half the possibilities with each decision. That's some big brain optimization right there!

--- 
# [15. 3Sum](https://leetcode.com/problems/3sum/)
#medium #twopointers #arrays #sorting #leetcode

## Problem

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`. The solution set must not contain duplicate triplets.

## Thought Process

- Aight so Two Sum was cute and all but now they want THREE numbers that add to zero? This some freaky ahh math right here
- Need to find all unique triplets that sum to 0 - no duplicates allowed because they're picky like that
- **Brute force:** Check every possible triplet with three nested loops - O(n³) because we hate ourselves apparently
- **Hash map madness:** For each pair, check if -(sum) exists in map - O(n²) but gets messy with duplicates
- **Two pointers after sorting:** Sort first, then fix one number and use two pointers for the other two
	- Sort the array so we can eliminate duplicates and use two pointer logic
	- Fix the first number (i), then use two pointers on the remaining subarray
	- Skip duplicates like they got the plague to avoid duplicate triplets
	- This freaky technique reduces it from O(n³) to O(n²)

## Solution

```cpp
class Solution {
public:
   vector<vector<int>> threeSum(vector<int>& nums) {
       sort(nums.begin(), nums.end());  // Sort this chaos first
       vector<vector<int>> result;
       
       for (int i = 0; i < nums.size() - 2; i++) {
           // Skip duplicates for the first element 
           if (i > 0 && nums[i] == nums[i - 1]) continue;
           
           int left = i + 1, right = nums.size() - 1;
           while (left < right) {
               int sum = nums[i] + nums[left] + nums[right];
               if (sum < 0) {
                   left++;  // Need bigger sum
               } else if (sum > 0) {
                   right--; // Need smaller sum
               } else {
                   // Found a triplet that hits different
                   result.push_back({nums[i], nums[left], nums[right]});
                   
                   // Skip duplicates for left and right pointers 
                   while (left < right && nums[left] == nums[left + 1]) left++;
                   while (left < right && nums[right] == nums[right - 1]) right--;
                   
                   left++;
                   right--;
               }
           }
       }
       return result;
   }
};
```

## Step-by-Step Walkthrough

**Given:** `nums = [-1,0,1,2,-1,-4]`

**Step 1 - Sort this mess:**

```
Original: [-1, 0, 1, 2, -1, -4]
Sorted:   [-4, -1, -1, 0, 1, 2]
```

**Step 2 - Triple pointer dance:**

```
i=0: nums[i] = -4, target = -(-4) = 4
     left=1, right=5: [-1] + [2] = 1 < 4, move left++
     left=2, right=5: [-1] + [2] = 1 < 4, move left++  
     left=3, right=5: [0] + [2] = 2 < 4, move left++
     left=4, right=5: [1] + [2] = 3 < 4, move left++
     left >= right, done with i=0

i=1: nums[i] = -1, target = -(-1) = 1
     left=2, right=5: [-1] + [2] = 1 == 1, FOUND TRIPLET [-1, -1, 2]
     Skip duplicates and move pointers
     left=3, right=4: [0] + [1] = 1 == 1, FOUND TRIPLET [-1, 0, 1]
     
i=2: nums[i] = -1, but nums[1] = -1 too, SKIP

i=3: nums[i] = 0, target = 0
     left=4, right=5: [1] + [2] = 3 > 0, move right--
     left >= right, done
```

**Final Answer:** `[[-1, -1, 2], [-1, 0, 1]]`

## Complexity

- Time: O(n²) - sorting takes O(n log n), then O(n) for outer loop × O(n) for two pointers
- Space: O(1) - not counting the result array, just using a few pointers

## Why This Freaky Shit Works 💯

- **Sorting first:** Lets us skip duplicates and use two pointer logic without losing our minds
- **Fixed first element:** Reduces 3Sum to 2Sum on the remaining subarray
- **Duplicate skipping:** Prevents the same triplet from showing up multiple times like an annoying ex
- **Two pointer coordination:** Left and right dance toward each other with purpose
- **Smart movement:** Each decision eliminates possibilities we don't need to check

## Pro Tips for the Streets

1. **Sort before you start:** This enables all the duplicate skipping and two pointer magic
2. **Skip duplicates aggressively:** For i, left, and right - duplicates are the enemy
3. **Fix one, solve for two:** Turn 3Sum into 2Sum by fixing the first element
4. **Bounds checking:** Make sure left < right in all your while loops
5. **Don't forget to move both pointers:** After finding a valid triplet, move both left and right

**Bottom line:** This freaky ahh algorithm is O(n²) because we do O(n) work for each of the n possible first elements. The sorting and duplicate elimination keeps us sane while the two pointers keep us efficient.

--- 
# [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
#medium #twopointers #arrays #greedy #leetcode

## Problem

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`. Find two lines that together with the x-axis form a container that can hold the most water.

## Thought Process

- We tryna find two lines that can hold the most water between them like we're building the ultimate swimming pool
- Water capacity = width × height, but height is limited by the shorter line (water gonna spill over the short one fr)
- **Brute force:** Check every possible pair of lines - O(n²) because apparently we got all day to check combinations
- **Two pointers approach:** Start wide and work inward smartly - O(n) because we're not wasteful like that
   - Start with max possible width (left=0, right=n-1)
   - Always move the pointer with the shorter height inward
   - Why? Because keeping the short line won't give us better area no matter what
   - Width decreases but we might find taller lines that compensate

## Solution

```cpp
class Solution {
public:
   int maxArea(vector<int>& height) {
       int area {};
       int left = 0, right = height.size() - 1;
       
       while (left <= right) {
           area = max(area, (right - left) * min(height[left], height[right]));
           height[left] < height[right] ? left++ : right--;
       }
       return area;
   }
};
```

## Step-by-Step Walkthrough

**Given:** `height = [1,8,6,2,5,4,8,3,7]`

**The pointer dance for maximum water storage:**

```
Initial: left=0, right=8
height = [1, 8, 6, 2, 5, 4, 8, 3, 7]
          l                          r

Area = (8-0) * min(1,7) = 8 * 1 = 8
height[0]=1 < height[8]=7, so left++ (ditch the short line)

left=1, right=8:
Area = (8-1) * min(8,7) = 7 * 7 = 49
height[1]=8 > height[8]=7, so right-- (ditch the short line)

left=1, right=7:
Area = (7-1) * min(8,3) = 6 * 3 = 18
height[1]=8 > height[7]=3, so right--

left=1, right=6:
Area = (6-1) * min(8,8) = 5 * 8 = 40
height[1]=8 == height[6]=8, move right-- (could go either way)

Continue this process...
Max area found: 49
```

**Final Answer:** `49`

## Complexity

- Time: O(n) - each element gets visited at most once as pointers move toward each other
- Space: O(1) - just tracking area and two pointers, no extra storage needed gang

## Why This Shit Works 💯

- **Greedy strategy:** Always eliminate the shorter line because it's the bottleneck
- **Width vs height trade-off:** As width decreases, we hunt for taller lines to compensate
- **No missed opportunities:** Moving the taller line would never give us better area with current short line
- **Ternary operator clean:** `condition ? left++ : right--` keeps the logic tight
- **Max tracking:** Keep updating the best area we've seen so far

## Pro Tips for the Streets

1. **Always move the shorter line:** The taller line isn't the problem, the short one is limiting us
2. **Area formula is width × min(heights):** Water level is determined by the shorter wall
3. **Start wide, go narrow:** Begin with maximum width and work inward strategically
4. **Ternary operator supremacy:** Clean one-liner for pointer movement decisions
5. **Greedy is the way:** Each move eliminates the worst option from consideration

**Bottom line:** This algorithm is O(n) because we visit each element at most once, and the greedy approach ensures we don't miss the optimal solution. That's some efficient water container optimization right there blud!

---

# BROSKI, ITS TIME FOR YOUR FIRST HARD, TIME TO CLENCH YOUR CHEEKS BOI


