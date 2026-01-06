---
layout: post
title: "Google Coding Interview - Problem Solving Methodology"
date: 2026-01-06 12:00:00 -0000
categories: interview-preparation google coding-interview problem-solving methodology
tags: google coding-interview problem-solving methodology algorithms data-structures
excerpt: "Comprehensive guide to solving coding interview questions at Google, covering step-by-step methodology, clarification questions, essential skills, and sample Q&A with detailed walkthroughs."
---

# Google Coding Interview - Problem Solving Methodology

A comprehensive guide to the systematic approach for solving coding interview questions at Google, covering step-by-step methodology, clarification questions, essential problem-solving skills, and detailed sample walkthroughs.

## Overview

Solving coding interview problems requires a **systematic approach** rather than jumping straight into coding. This guide outlines a proven methodology used by successful Google candidates.

**Key Principles:**
- **Think before coding**: Understand the problem completely
- **Communicate your thinking**: Explain your approach
- **Start simple**: Begin with brute force, then optimize
- **Test your solution**: Verify with examples
- **Consider edge cases**: Handle boundary conditions

---

## Step-by-Step Problem Solving Process

### Step 1: Understand the Requirement

**Goal**: Fully comprehend what the problem is asking before attempting to solve it.

#### What to Do:

1. **Read the problem carefully** (2-3 times if needed)
2. **Restate the problem** in your own words
3. **Identify inputs and outputs**
4. **Note constraints and assumptions**
5. **Ask clarifying questions** (see section below)

#### Essential Questions to Answer:

- What is the input format?
- What is the expected output format?
- What are the constraints? (array size, value ranges, etc.)
- Are there any edge cases mentioned?
- What should I return for invalid inputs?
- Are there any special requirements? (in-place, no extra space, etc.)

#### Example - Problem Understanding:

**Problem**: "Find the longest substring without repeating characters"

**Understanding:**
- **Input**: A string (e.g., "abcabcbb")
- **Output**: An integer representing the length of the longest substring
- **Constraints**: 
  - String can be empty?
  - ASCII characters only?
  - Case-sensitive?
- **Edge cases**: 
  - Empty string → return 0?
  - All unique characters → return string length?
  - All same character → return 1?

---

### Step 2: Identify the Category/Type of Question

**Goal**: Recognize the problem pattern to apply the right approach.

#### Common Problem Categories:

**1. Array/String Problems:**
- Two pointers
- Sliding window
- Prefix/suffix arrays
- String manipulation

**2. Tree Problems:**
- Binary tree traversal (DFS/BFS)
- Tree construction
- Tree properties (height, diameter, LCA)
- Binary search tree operations

**3. Graph Problems:**
- BFS/DFS traversal
- Shortest path (Dijkstra, BFS)
- Topological sort
- Union-Find
- Cycle detection

**4. Dynamic Programming:**
- 1D DP (Fibonacci, climbing stairs)
- 2D DP (grid problems, LCS)
- Knapsack problems
- String DP

**5. Backtracking:**
- Permutations/combinations
- Constraint satisfaction
- N-Queens, Sudoku

**6. Greedy Algorithms:**
- Interval problems
- Activity selection
- Minimum spanning tree

**7. Binary Search:**
- Search in sorted array
- Search space reduction
- Binary search on answer

**8. Hash Map/Set:**
- Frequency counting
- Two sum problems
- Grouping problems

#### Pattern Recognition Examples:

**Example 1: Word Ladder**
- **Category**: Graph (BFS)
- **Why**: Transform one word to another, each step changes one character
- **Pattern**: Shortest path in unweighted graph
- **Key insight**: Words are nodes, one-character differences are edges

**Example 2: Longest Substring Without Repeating Characters**
- **Category**: Sliding Window
- **Why**: Need to find contiguous substring with property
- **Pattern**: Expand window, contract when property violated
- **Key insight**: Use hash map to track character positions

**Example 3: Merge Intervals**
- **Category**: Sorting + Greedy
- **Why**: Need to process intervals in order
- **Pattern**: Sort by start, merge overlapping
- **Key insight**: Compare current interval with last merged

---

### Step 3: Data Structure Choice

**Goal**: Select the optimal data structure for the problem.

#### Decision Framework:

**When to Use Hash Map:**
- Need O(1) lookup
- Counting frequencies
- Mapping relationships
- Tracking seen elements
- Two sum problems

**When to Use Hash Set:**
- Need O(1) membership test
- Removing duplicates
- Tracking visited nodes
- Fast lookups without values

**When to Use Array/List:**
- Sequential access
- Index-based operations
- Simple storage
- When size is known

**When to Use Stack:**
- LIFO operations
- Matching parentheses
- Monotonic stack problems
- DFS (iterative)

**When to Use Queue:**
- FIFO operations
- BFS traversal
- Level-order processing
- Sliding window maximum

**When to Use Heap/Priority Queue:**
- Need min/max element quickly
- Top K problems
- Merge K sorted lists
- Median finding

**When to Use Tree:**
- Hierarchical data
- Binary search
- Range queries
- Trie for strings

**When to Use Union-Find:**
- Connectivity problems
- Cycle detection
- Grouping elements
- Number of islands

#### Comparison Examples:

**Hash Map vs List:**
```cpp
// Problem: Find two numbers that sum to target

// Using List (O(n²)):
#include <vector>
using namespace std;

vector<int> twoSumList(vector<int>& nums, int target) {
    for (int i = 0; i < nums.size(); i++) {
        for (int j = i + 1; j < nums.size(); j++) {
            if (nums[i] + nums[j] == target) {
                return {i, j};
            }
        }
    }
    return {};
}

// Using Hash Map (O(n)):
#include <unordered_map>

vector<int> twoSumHashMap(vector<int>& nums, int target) {
    unordered_map<int, int> seen;  // value -> index
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

**Stack vs Queue:**
```cpp
// Problem: Valid Parentheses

#include <stack>
#include <unordered_map>
#include <string>

// Stack (correct choice):
bool isValid(string s) {
    stack<char> st;
    unordered_map<char, char> mapping = {
        {')', '('},
        {'}', '{'},
        {']', '['}
    };
    
    for (char c : s) {
        if (mapping.find(c) != mapping.end()) {
            if (st.empty() || st.top() != mapping[c]) {
                return false;
            }
            st.pop();
        } else {
            st.push(c);
        }
    }
    return st.empty();
}

// Queue would be wrong - need LIFO for matching
```

---

### Step 4: Class Design

**Goal**: Design helper classes or data structures to simplify the solution.

#### When Class Design is Needed:

1. **Complex State Management**: Multiple related variables
2. **Reusable Components**: Union-Find, Trie, etc.
3. **Cleaner Code**: Encapsulate related operations
4. **Interview Demonstration**: Shows OOP understanding

#### Common Class Designs:

**1. Union-Find (Disjoint Set):**
```cpp
#include <vector>
using namespace std;

class UnionFind {
private:
    vector<int> parent;
    vector<int> rank;
    
public:
    UnionFind(int n) : parent(n), rank(n, 0) {
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);  // Path compression
        }
        return parent[x];
    }
    
    bool unionSets(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) {
            return false;
        }
        
        // Union by rank
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        return true;
    }
};

// Use case: Number of Islands, Redundant Connection
```

**2. Trie (Prefix Tree):**
```cpp
#include <unordered_map>
#include <string>
using namespace std;

class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    bool isEnd;
    
    TrieNode() : isEnd(false) {}
};

class Trie {
private:
    TrieNode* root;
    
public:
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (node->children.find(c) == node->children.end()) {
                node->children[c] = new TrieNode();
            }
            node = node->children[c];
        }
        node->isEnd = true;
    }
    
    bool search(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (node->children.find(c) == node->children.end()) {
                return false;
            }
            node = node->children[c];
        }
        return node->isEnd;
    }
};

// Use case: Word Search II, Implement Trie
```

**3. Timestamp Class for Logs:**
```cpp
#include <string>
using namespace std;

class LogEntry {
public:
    int timestamp;
    string message;
    
    LogEntry(int ts, const string& msg) 
        : timestamp(ts), message(msg) {}
    
    bool operator<(const LogEntry& other) const {
        return timestamp < other.timestamp;
    }
};

// Use case: Design Log Storage System
```

**4. Interval Class:**
```cpp
class Interval {
public:
    int start;
    int end;
    
    Interval(int s, int e) : start(s), end(e) {}
    
    bool operator<(const Interval& other) const {
        if (start != other.start) {
            return start < other.start;
        }
        return end < other.end;
    }
    
    bool overlaps(const Interval& other) const {
        return !(end < other.start || other.end < start);
    }
};

// Use case: Merge Intervals, Meeting Rooms
```

**5. ListNode for Linked Lists:**
```cpp
struct ListNode {
    int val;
    ListNode* next;
    
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode* next) : val(x), next(next) {}
};

// Use case: All linked list problems
```

**6. TreeNode for Trees:**
```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode* left, TreeNode* right) 
        : val(x), left(left), right(right) {}
};

// Use case: All tree problems
```

---

### Step 5: Algorithm Design & Runtime/Space Analysis

**Goal**: Design an efficient algorithm and analyze its complexity.

#### Algorithm Design Process:

1. **Start with Brute Force**: 
   - Understand the problem completely
   - Get a working solution first
   - Identify inefficiencies

2. **Optimize**:
   - Look for repeated work
   - Use appropriate data structures
   - Apply known patterns
   - Consider trade-offs

3. **Analyze Complexity**:
   - Time complexity: Count operations
   - Space complexity: Count extra space
   - Best/Average/Worst case

#### Complexity Analysis Framework:

**Time Complexity:**
- Count nested loops
- Count recursive calls
- Count data structure operations
- Consider input size

**Space Complexity:**
- Count extra arrays/maps
- Count recursion stack
- Count data structure storage

#### Example Analysis:

**Problem**: Two Sum

**Brute Force:**
```cpp
#include <vector>
using namespace std;

vector<int> twoSum(vector<int>& nums, int target) {
    for (int i = 0; i < nums.size(); i++) {
        for (int j = i + 1; j < nums.size(); j++) {
            if (nums[i] + nums[j] == target) {
                return {i, j};
            }
        }
    }
    return {};
}
```
- **Time**: O(n²) - nested loops
- **Space**: O(1) - no extra space

**Optimized:**
```cpp
#include <vector>
#include <unordered_map>
using namespace std;

vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> seen;  // value -> index
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
- **Time**: O(n) - single pass
- **Space**: O(n) - hash map

**Analysis:**
- One pass through array: O(n)
- Hash map operations: O(1) average
- Total: O(n) time, O(n) space

---

### Step 6: Actual Implementation

**Goal**: Write clean, correct, and efficient code.

#### Implementation Best Practices:

1. **Code Structure**:
   - Clear function names
   - Logical variable names
   - Proper indentation
   - Comments for complex logic

2. **Edge Cases**:
   - Empty inputs
   - Single element
   - All same values
   - Boundary values

3. **Error Handling**:
   - Invalid inputs
   - Null/None checks
   - Out of bounds

4. **Testing**:
   - Walk through examples
   - Test edge cases
   - Verify correctness

#### Implementation Template:

```cpp
#include <vector>
#include <unordered_set>
using namespace std;

vector<int> solveProblem(vector<int>& input) {
    // 1. Handle edge cases
    if (input.empty()) {
        return {};
    }
    
    // 2. Initialize data structures
    vector<int> result;
    unordered_set<int> seen;
    
    // 3. Main algorithm
    for (int item : input) {
        // Process item
        if (seen.find(item) == seen.end()) {
            result.push_back(item);
            seen.insert(item);
        }
    }
    
    // 4. Return result
    return result;
}
```

---

## Essential Clarification Questions

### Input/Output Questions:

1. **Input Format**:
   - "What is the input format? Array, string, tree?"
   - "What are the constraints? Size, value ranges?"
   - "Can the input be empty or null?"

2. **Output Format**:
   - "What should I return? Value, array, boolean?"
   - "What if no solution exists?"
   - "Should I return indices or values?"

3. **Edge Cases**:
   - "What about empty inputs?"
   - "What about single element?"
   - "Are there duplicates allowed?"

### Problem-Specific Questions:

**For Array Problems:**
- "Can the array be empty?"
- "Are negative numbers allowed?"
- "Can there be duplicates?"
- "Is the array sorted?"

**For String Problems:**
- "Is it case-sensitive?"
- "Are spaces significant?"
- "What character set? ASCII or Unicode?"

**For Tree Problems:**
- "Can the tree be empty/null?"
- "What is the maximum depth?"
- "Is it a binary tree or general tree?"

**For Graph Problems:**
- "Directed or undirected?"
- "Can there be cycles?"
- "Are weights positive?"

---

## Sample Problem Walkthroughs

### Problem 1: Word Ladder

**Step 1: Understand the Requirement**

**Problem**: Given two words (beginWord and endWord), and a dictionary word list, find the length of shortest transformation sequence from beginWord to endWord.

**Clarifying Questions:**
- Can beginWord be in the word list?
- Can endWord be in the word list?
- What if no transformation exists?
- Can we change one character at a time?
- Are all words the same length?

**Understanding:**
- Input: beginWord (string), endWord (string), wordList (list of strings)
- Output: Integer (shortest path length, or 0 if impossible)
- Constraint: Each transformation changes exactly one character

**Step 2: Identify Category**

- **Category**: Graph (BFS)
- **Pattern**: Shortest path in unweighted graph
- **Key Insight**: 
  - Words are nodes
  - One-character difference = edge
  - Need shortest path from beginWord to endWord

**Step 3: Data Structure Choice**

- **Queue**: For BFS traversal (FIFO)
- **Set**: For word list (O(1) lookup)
- **Set**: For visited words (avoid cycles)

**Step 4: Class Design**

Not needed for this problem - can use simple BFS.

**Step 5: Algorithm Design**

```cpp
// Algorithm:
// 1. Convert wordList to set for O(1) lookup
// 2. Use BFS to find shortest path
// 3. For each word, generate all possible one-character changes
// 4. Check if new word is in wordList and not visited
// 5. If found endWord, return level + 1

// Time: O(M * N) where M = word length, N = word list size
// Space: O(N) for queue and visited set
```

**Step 6: Implementation**

```cpp
#include <string>
#include <vector>
#include <queue>
#include <unordered_set>
using namespace std;

int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
    // Edge case
    unordered_set<string> wordSet(wordList.begin(), wordList.end());
    if (wordSet.find(endWord) == wordSet.end()) {
        return 0;
    }
    
    // BFS
    queue<pair<string, int>> q;
    q.push({beginWord, 1});
    unordered_set<string> visited;
    visited.insert(beginWord);
    
    while (!q.empty()) {
        auto [word, level] = q.front();
        q.pop();
        
        // Generate all one-character variations
        for (int i = 0; i < word.length(); i++) {
            char original = word[i];
            for (char c = 'a'; c <= 'z'; c++) {
                if (c == original) {
                    continue;
                }
                
                word[i] = c;
                string newWord = word;
                
                if (newWord == endWord) {
                    return level + 1;
                }
                
                if (wordSet.find(newWord) != wordSet.end() && 
                    visited.find(newWord) == visited.end()) {
                    visited.insert(newWord);
                    q.push({newWord, level + 1});
                }
            }
            word[i] = original;  // Restore original character
        }
    }
    
    return 0;  // No transformation found
}
```

---

### Problem 2: Longest Substring Without Repeating Characters

**Step 1: Understand the Requirement**

**Problem**: Find the length of the longest substring without repeating characters.

**Clarifying Questions:**
- Can string be empty? → Return 0
- Case-sensitive? → Usually yes
- What characters? → Usually ASCII

**Understanding:**
- Input: String s
- Output: Integer (length of longest substring)
- Constraint: No repeating characters in substring

**Step 2: Identify Category**

- **Category**: Sliding Window
- **Pattern**: Expand window, contract when property violated
- **Key Insight**: Use two pointers, track characters in window

**Step 3: Data Structure Choice**

- **Hash Map**: Track character → last seen index
- **Two Pointers**: Left and right boundaries of window

**Step 4: Class Design**

Not needed.

**Step 5: Algorithm Design**

```cpp
// Algorithm:
// 1. Use sliding window with two pointers
// 2. Expand right pointer, add characters to map
// 3. If duplicate found, move left pointer past last occurrence
// 4. Track maximum window size

// Time: O(n) - each character visited at most twice
// Space: O(min(n, m)) where m is character set size
```

**Step 6: Implementation**

```cpp
#include <string>
#include <unordered_map>
#include <algorithm>
using namespace std;

int lengthOfLongestSubstring(string s) {
    if (s.empty()) {
        return 0;
    }
    
    unordered_map<char, int> charMap;  // character -> last seen index
    int left = 0;
    int maxLen = 0;
    
    for (int right = 0; right < s.length(); right++) {
        // If character seen and within current window
        if (charMap.find(s[right]) != charMap.end() && 
            charMap[s[right]] >= left) {
            left = charMap[s[right]] + 1;
        }
        
        charMap[s[right]] = right;
        maxLen = max(maxLen, right - left + 1);
    }
    
    return maxLen;
}
```

---

### Problem 3: Number of Islands

**Step 1: Understand the Requirement**

**Problem**: Given a 2D grid of '1's (land) and '0's (water), count the number of islands.

**Clarifying Questions:**
- What defines an island? → Connected '1's (horizontally/vertically)
- Can grid be empty? → Return 0
- Can we modify the grid? → Usually yes

**Understanding:**
- Input: 2D grid (list of lists)
- Output: Integer (number of islands)
- Constraint: Islands are connected horizontally/vertically

**Step 2: Identify Category**

- **Category**: Graph (DFS/BFS) or Union-Find
- **Pattern**: Connected components
- **Key Insight**: Each island is a connected component

**Step 3: Data Structure Choice**

**Option 1: DFS/BFS**
- Stack/Queue for traversal
- Visited set or modify grid in-place

**Option 2: Union-Find**
- Union-Find class
- Count connected components

**Step 4: Class Design**

**Union-Find approach:**
```cpp
#include <vector>
using namespace std;

class UnionFind {
private:
    vector<int> parent;
    int count;
    
public:
    UnionFind(int n) : parent(n), count(n) {
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    void unionSets(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            parent[rootX] = rootY;
            count--;
        }
    }
    
    int getCount() const {
        return count;
    }
};
```

**Step 5: Algorithm Design**

**DFS Approach:**
```cpp
// Algorithm:
// 1. Iterate through grid
// 2. When find '1', start DFS to mark all connected '1's as visited
// 3. Count number of DFS starts

// Time: O(m * n) - visit each cell once
// Space: O(m * n) - recursion stack in worst case
```

**Step 6: Implementation**

```cpp
#include <vector>
using namespace std;

class Solution {
private:
    void dfs(vector<vector<char>>& grid, int r, int c) {
        int rows = grid.size();
        int cols = grid[0].size();
        
        if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] == '0') {
            return;
        }
        
        grid[r][c] = '0';  // Mark as visited
        dfs(grid, r + 1, c);
        dfs(grid, r - 1, c);
        dfs(grid, r, c + 1);
        dfs(grid, r, c - 1);
    }
    
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty() || grid[0].empty()) {
            return 0;
        }
        
        int rows = grid.size();
        int cols = grid[0].size();
        int count = 0;
        
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == '1') {
                    count++;
                    dfs(grid, r, c);
                }
            }
        }
        
        return count;
    }
};
```

---

## Common Mistakes to Avoid

1. **Jumping to Code**: Don't start coding without understanding
2. **Not Asking Questions**: Always clarify requirements
3. **Ignoring Edge Cases**: Handle empty inputs, null values
4. **Not Testing**: Walk through examples before finishing
5. **Poor Variable Names**: Use descriptive names
6. **No Comments**: Explain complex logic
7. **Not Optimizing**: Start simple, then optimize
8. **Wrong Data Structure**: Choose based on operations needed

---

## Practice Checklist

Before coding, ensure you've:
- [ ] Understood the problem completely
- [ ] Asked clarifying questions
- [ ] Identified the problem category
- [ ] Chosen appropriate data structures
- [ ] Designed the algorithm
- [ ] Analyzed time/space complexity
- [ ] Considered edge cases
- [ ] Planned the implementation

---

## Summary

**The 6-Step Process:**
1. **Understand**: Read, restate, clarify
2. **Categorize**: Identify problem pattern
3. **Choose Data Structures**: Select optimal structures
4. **Design Classes**: Create helper classes if needed
5. **Design Algorithm**: Plan approach, analyze complexity
6. **Implement**: Write clean, tested code

**Key Success Factors:**
- Communication: Explain your thinking
- Systematic approach: Follow the steps
- Testing: Verify with examples
- Optimization: Start simple, then improve

---

**Related Posts:**
- [Google Coding Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-coding-interview-preparation %})
- [Google 2-Week Interview Preparation Plan]({{ site.baseurl }}{% post_url 2025-12-28-google-2-week-interview-prep-todo-based-on-algorithm-guide %})

