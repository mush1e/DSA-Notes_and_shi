# Arrays & Hash Maps in C++ - Quick Rundown 📊. 
> Alright big boy you ready to tackle some real shi? imma give you a quick run down on arrays and hash maps in C++ and then we start solving some problems

## Arrays in C++ 🔢

### The Basics

C++ gives you multiple ways to work with arrays. Pick your poison:

```cpp
// Static array (fixed size, stack allocated)
int arr[5] = {1, 2, 3, 4, 5};

// Vector (dynamic array, heap allocated) - USE THIS MOST OF THE TIME
vector<int> arr = {1, 2, 3, 4, 5};
//                 0  1  2  3  4  <- indices

// Array class (C++11, fixed size but safer than raw arrays)
array<int, 5> arr = {1, 2, 3, 4, 5};
```

### Essential Vector Operations

```cpp
vector<int> v;

// Adding elements
v.push_back(42);           // Add to end - O(1) amortized
v.insert(v.begin(), 69);   // Insert at beginning - O(n)
v.insert(v.begin() + 2, 13); // Insert at index 2 - O(n)

// Accessing
int first = v[0];          // Direct access - O(1)
int first_safe = v.at(0);  // Bounds-checked access - O(1)
int last = v.back();       // Last element - O(1)

// Size and capacity
v.size();                  // Number of elements
v.empty();                 // Check if empty
v.capacity();              // Allocated space

// Removing
v.pop_back();              // Remove last - O(1)
v.erase(v.begin() + 1);    // Remove at index 1 - O(n)
v.clear();                 // Remove all - O(n)

// Initialization tricks
vector<int> zeros(10, 0);           // 10 zeros
vector<int> copy(original);         // Copy constructor
vector<vector<int>> matrix(n, vector<int>(m, 0)); // 2D array
```

### Useful STL Algorithms for Arrays

```cpp
#include <algorithm>

vector<int> v = {3, 1, 4, 1, 5, 9};

// Sorting
sort(v.begin(), v.end());              // Ascending
sort(v.rbegin(), v.rend());            // Descending
sort(v.begin(), v.end(), greater<int>()); // Descending alternative

// Searching
bool found = binary_search(v.begin(), v.end(), 4); // Must be sorted first
auto it = find(v.begin(), v.end(), 4);             // Linear search
auto it = lower_bound(v.begin(), v.end(), 4);      // First >= 4
auto it = upper_bound(v.begin(), v.end(), 4);      // First > 4

// Min/Max
auto min_it = min_element(v.begin(), v.end());
auto max_it = max_element(v.begin(), v.end());
auto [min_it, max_it] = minmax_element(v.begin(), v.end()); // C++17

// Reversing
reverse(v.begin(), v.end());

// Unique elements (must sort first)
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());

// Accumulate (sum)
int sum = accumulate(v.begin(), v.end(), 0);
```

## Hash Maps in C++ 🗺️

### The Hash Map Family

```cpp
#include <unordered_map>
#include <unordered_set>
#include <map>
#include <set>

// Unordered (hash-based) - O(1) average operations
unordered_map<int, string> umap;     // key -> value
unordered_set<int> uset;             // just keys

// Ordered (tree-based) - O(log n) operations but sorted
map<int, string> omap;               // key -> value, sorted by key
set<int> oset;                       // just keys, sorted
```

### Hash Map Operations

```cpp
unordered_map<int, string> mp;

// Insertion
mp[1] = "one";                       // Direct assignment
mp.insert({2, "two"});               // Insert pair
mp.emplace(3, "three");              // Construct in-place

// Access
string val = mp[1];                  // Direct access (creates if not exists!)
string val = mp.at(1);               // Safe access (throws if not exists)

// Checking existence
if (mp.find(1) != mp.end()) { }      // Classic way
if (mp.count(1)) { }                 // Count-based check
if (mp.contains(1)) { }              // C++20 way

// Removing
mp.erase(1);                         // Remove by key
mp.erase(mp.find(1));                // Remove by iterator
mp.clear();                          // Remove all

// Iteration
for (auto& [key, value] : mp) {      // C++17 structured binding
    cout << key << ": " << value << "\n";
}

for (auto& pair : mp) {              // Traditional way
    cout << pair.first << ": " << pair.second << "\n";
}
```

### Hash Set Operations

```cpp
unordered_set<int> s;

// Adding
s.insert(42);
s.emplace(69);

// Checking
if (s.find(42) != s.end()) { }       // Found
if (s.count(42)) { }                 // Exists
if (s.contains(42)) { }              // C++20

// Removing
s.erase(42);

// Size
s.size();
s.empty();
```

## Common Patterns with Arrays & Hash Maps

### 1. Two Sum Pattern

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> seen;
    for (int i = 0; i < nums.size(); i++) {
        int complement = target - nums[i];
        if (seen.find(complement) != seen.end()) {
            return {seen[complement], i};
        }
        seen[nums[i]] = i;
    }
    return {};
}
```

### 2. Frequency Counter

```cpp
unordered_map<int, int> freq;
for (int num : nums) {
    freq[num]++;
}
// or
for (int num : nums) {
    freq[num] = freq[num] + 1; // explicit
}
```

### 3. Sliding Window with Hash Map

```cpp
int maxLength = 0;
unordered_set<char> window;
int left = 0;

for (int right = 0; right < s.length(); right++) {
    while (window.count(s[right])) {
        window.erase(s[left]);
        left++;
    }
    window.insert(s[right]);
    maxLength = max(maxLength, right - left + 1);
}
```

### 4. Prefix Sum with Hash Map

```cpp
unordered_map<int, int> prefixCount;
prefixCount[0] = 1; // Important for subarrays starting from index 0
int prefixSum = 0, count = 0;

for (int num : nums) {
    prefixSum += num;
    if (prefixCount.find(prefixSum - target) != prefixCount.end()) {
        count += prefixCount[prefixSum - target];
    }
    prefixCount[prefixSum]++;
}
```

## C++ Specific Gotchas That'll Fuck You Over

1. **Using `[]` on non-existent keys** - Creates the key with default value
2. **Iterator invalidation** - Don't modify container while iterating
3. **Hash collisions** - Worst case O(n) for unordered containers
4. **Memory management** - Vectors handle it, raw arrays don't
5. **Comparison operators** - Make sure your custom types are comparable

## Pro STL Tips

### Custom Comparators

```cpp
// For sorting
sort(v.begin(), v.end(), [](int a, int b) {
    return a > b; // Descending order
});

// For maps with custom ordering
map<int, string, greater<int>> descendingMap;
```

### Lambda Functions for Algorithms

```cpp
// Count elements > 5
int count = count_if(v.begin(), v.end(), [](int x) { return x > 5; });

// Transform elements
transform(v.begin(), v.end(), v.begin(), [](int x) { return x * 2; });
```

### Pair and Tuple Tricks

```cpp
// For multiple return values
pair<int, int> findMinMax(vector<int>& arr) {
    auto [min_it, max_it] = minmax_element(arr.begin(), arr.end());
    return {*min_it, *max_it};
}

// Comparing pairs (lexicographic order)
vector<pair<int, int>> points = {{1, 2}, {3, 1}, {1, 3}};
sort(points.begin(), points.end()); // Sorts by first, then second
```

## When to Use What

**Vectors vs Arrays:**

- Use `vector<T>` 99% of the time
- Use raw arrays only for fixed-size, performance-critical code

**unordered_map vs map:**

- `unordered_map` for O(1) lookup when order doesn't matter
- `map` when you need sorted keys or range queries

**unordered_set vs set:**

- `unordered_set` for O(1) membership testing
- `set` when you need sorted elements or range operations

---

**Bottom Line:** Master vectors and unordered_maps, sprinkle in some STL algorithms, and you'll demolish most problems. The STL is your friend - use it!

Now stop reading and start coding. Time to put this shit to work! 💪

#cpp #arrays #hashmaps #stl #patterns