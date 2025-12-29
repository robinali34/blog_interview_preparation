---
layout: post
title: "Google Interview 2-Week Preparation Plan - Based on Algorithm Guide"
date: 2025-12-28 12:00:00 -0000
categories: interview-preparation google coding-interview algorithms todo-checklist
tags: google interview-preparation algorithms leetcode two-pointers sliding-window monotonic-stack dynamic-programming
excerpt: "A comprehensive 2-week Google interview preparation plan based on the algorithm guide from 灵茶山艾府, covering high-frequency topics, daily practice schedule, and prioritized study plan."
---

# Google Interview 2-Week Preparation Plan

A structured, actionable 2-week preparation plan for Google coding interviews, based on the comprehensive algorithm guide from [灵茶山艾府 (Lingchashan Aifu)](https://github.com/EndlessCheng/codeforces-go/blob/master/leetcode/README.md). This plan prioritizes high-frequency Google topics and provides a realistic daily schedule.

## Reference Material

**Algorithm Guide**: [基础算法精讲·题目汇总](https://github.com/EndlessCheng/codeforces-go/blob/master/leetcode/README.md) by 灵茶山艾府

This guide provides:
- Video tutorials (Bilibili)
- LeetCode problem sets organized by topic
- Code solutions in multiple languages
- Practice problems marked as homework (课后作业)

## Overview

**Timeline**: 14 days (2 weeks)  
**Daily Commitment**: 4-6 hours  
**Focus**: High-frequency Google topics + Core algorithms  
**Goal**: Master patterns, not memorize solutions

---

## Week 1: Foundation & Core Patterns

### Day 1-2: Two Pointers & Sliding Window (⭐⭐⭐ Very High Frequency)

**Priority**: CRITICAL - These appear in almost every Google interview

#### Day 1: Two Pointers (相向双指针)

**Study Plan:**
- [ ] Watch video: [相向双指针 1](https://www.bilibili.com/video/BV1bP411c7oJ/)
- [ ] Watch video: [相向双指针 2](https://www.bilibili.com/video/BV1Qg411q7ia/)

**Practice Problems (Must Solve):**
- [ ] [167. Two Sum II - Input Array Is Sorted](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/) - **Google Favorite**
- [ ] [15. 3Sum](https://leetcode.cn/problems/3sum/) - **Very High Frequency** ⭐⭐⭐
- [ ] [11. Container With Most Water](https://leetcode.cn/problems/container-with-most-water/) - **Very High Frequency** ⭐⭐⭐
- [ ] [42. Trapping Rain Water](https://leetcode.cn/problems/trapping-rain-water/) - **Very High Frequency** ⭐⭐⭐
- [ ] [125. Valid Palindrome](https://leetcode.cn/problems/valid-palindrome/) - **High Frequency** ⭐⭐

**Homework (Additional Practice):**
- [ ] [2824. Count Pairs Whose Sum Is Less Than Target](https://leetcode.cn/problems/count-pairs-whose-sum-is-less-than-target/)
- [ ] [16. 3Sum Closest](https://leetcode.cn/problems/3sum-closest/)
- [ ] [18. 4Sum](https://leetcode.cn/problems/4sum/)
- [ ] [611. Valid Triangle Number](https://leetcode.cn/problems/valid-triangle-number/)

**Time Allocation**: 4-5 hours
- 1 hour: Watch videos and understand patterns
- 3-4 hours: Solve problems and review solutions

#### Day 2: Sliding Window (滑动窗口)

**Study Plan:**
- [ ] Watch video: [滑动窗口](https://www.bilibili.com/video/BV1hd4y1r7Gq/)

**Practice Problems (Must Solve):**
- [ ] [209. Minimum Size Subarray Sum](https://leetcode.cn/problems/minimum-size-subarray-sum/) - **High Frequency** ⭐⭐
- [ ] [3. Longest Substring Without Repeating Characters](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) - **Very High Frequency** ⭐⭐⭐
- [ ] [76. Minimum Window Substring](https://leetcode.cn/problems/minimum-window-substring/) - **Very High Frequency** ⭐⭐⭐
- [ ] [713. Subarray Product Less Than K](https://leetcode.cn/problems/subarray-product-less-than-k/) - **High Frequency** ⭐⭐

**Homework (Additional Practice):**
- [ ] [3090. Maximum Length Substring With Two Occurrences](https://leetcode.cn/problems/maximum-length-substring-with-two-occurrences/)
- [ ] [2958. Length of Longest Subarray With At Most K Frequency](https://leetcode.cn/problems/length-of-longest-subarray-with-at-most-k-frequency/)
- [ ] [2730. Find the Longest Semi-Repetitive Substring](https://leetcode.cn/problems/find-the-longest-semi-repetitive-substring/)
- [ ] [1004. Max Consecutive Ones III](https://leetcode.cn/problems/max-consecutive-ones-iii/)

**Time Allocation**: 4-5 hours
- 1 hour: Watch video and understand patterns
- 3-4 hours: Solve problems and review solutions

**Key Patterns to Master:**
- Fixed-size sliding window
- Variable-size sliding window (expand/contract)
- Two pointers for substring problems
- Hash map + sliding window for character frequency

---

### Day 3-4: Binary Search & Arrays

#### Day 3: Binary Search (二分算法)

**Study Plan:**
- [ ] Review binary search fundamentals
- [ ] Understand search space reduction

**Practice Problems (Must Solve):**
- [ ] [704. Binary Search](https://leetcode.com/problems/binary-search/) - **High Frequency** ⭐⭐
- [ ] [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) - **Very High Frequency** ⭐⭐⭐
- [ ] [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) - **High Frequency** ⭐⭐
- [ ] [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) - **High Frequency** ⭐⭐
- [ ] [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) - **Very High Frequency** ⭐⭐⭐

**Time Allocation**: 4-5 hours

**Key Patterns:**
- Standard binary search
- Search in rotated array
- Binary search on answer (minimize maximum, maximize minimum)
- Finding boundaries (first/last occurrence)

#### Day 4: Arrays & Hash Tables

**Practice Problems (Must Solve):**
- [ ] [1. Two Sum](https://leetcode.com/problems/two-sum/) - **Very High Frequency** ⭐⭐⭐
- [ ] [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) - **Very High Frequency** ⭐⭐⭐
- [ ] [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) - **Very High Frequency** ⭐⭐⭐
- [ ] [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/) - **Very High Frequency** ⭐⭐⭐
- [ ] [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) - **Very High Frequency** ⭐⭐⭐
- [ ] [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/) - **Very High Frequency** ⭐⭐⭐

**Time Allocation**: 4-5 hours

---

### Day 5-6: Trees & Graphs

#### Day 5: Binary Trees

**Practice Problems (Must Solve):**
- [ ] [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) - **Very High Frequency** ⭐⭐⭐
- [ ] [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) - **High Frequency** ⭐⭐
- [ ] [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/) - **High Frequency** ⭐⭐
- [ ] [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) - **High Frequency** ⭐⭐
- [ ] [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) - **Very High Frequency** ⭐⭐⭐
- [ ] [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) - **Very High Frequency** ⭐⭐⭐
- [ ] [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) - **Very High Frequency** ⭐⭐⭐

**Time Allocation**: 5-6 hours

**Key Patterns:**
- DFS (preorder, inorder, postorder)
- BFS (level order)
- Recursive vs iterative
- Tree construction from traversals

#### Day 6: Graphs

**Practice Problems (Must Solve):**
- [ ] [200. Number of Islands](https://leetcode.com/problems/number-of-islands/) - **Very High Frequency** ⭐⭐⭐
- [ ] [127. Word Ladder](https://leetcode.com/problems/word-ladder/) - **High Frequency** ⭐⭐
- [ ] [207. Course Schedule](https://leetcode.com/problems/course-schedule/) - **Very High Frequency** ⭐⭐⭐
- [ ] [133. Clone Graph](https://leetcode.com/problems/clone-graph/) - **High Frequency** ⭐⭐
- [ ] [79. Word Search](https://leetcode.com/problems/word-search/) - **High Frequency** ⭐⭐

**Time Allocation**: 5-6 hours

**Key Patterns:**
- DFS for traversal and path finding
- BFS for shortest path
- Topological sort
- Union-Find for connectivity

---

### Day 7: Review & Mock Interview

**Review Day:**
- [ ] Review all problems from Week 1
- [ ] Identify weak areas
- [ ] Re-solve 3-5 most challenging problems
- [ ] Practice explaining solutions out loud

**Mock Interview:**
- [ ] Complete 1-2 timed mock interviews (45 minutes each)
- [ ] Focus on: Two pointers, sliding window, trees
- [ ] Practice: Clarifying questions, thinking out loud, edge cases

**Time Allocation**: 5-6 hours

---

## Week 2: Advanced Topics & Google Favorites

### Day 8-9: Dynamic Programming

#### Day 8: 1D DP

**Practice Problems (Must Solve):**
- [ ] [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) - **Very High Frequency** ⭐⭐⭐
- [ ] [198. House Robber](https://leetcode.com/problems/house-robber/) - **Very High Frequency** ⭐⭐⭐
- [ ] [322. Coin Change](https://leetcode.com/problems/coin-change/) - **Very High Frequency** ⭐⭐⭐
- [ ] [139. Word Break](https://leetcode.com/problems/word-break/) - **Very High Frequency** ⭐⭐⭐
- [ ] [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) - **Very High Frequency** ⭐⭐⭐
- [ ] [91. Decode Ways](https://leetcode.com/problems/decode-ways/) - **High Frequency** ⭐⭐

**Time Allocation**: 5-6 hours

**Key Patterns:**
- Fibonacci-style DP
- Knapsack problems
- String DP
- LIS pattern

#### Day 9: 2D DP & String DP

**Practice Problems (Must Solve):**
- [ ] [62. Unique Paths](https://leetcode.com/problems/unique-paths/) - **Very High Frequency** ⭐⭐⭐
- [ ] [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) - **High Frequency** ⭐⭐
- [ ] [72. Edit Distance](https://leetcode.com/problems/edit-distance/) - **Very High Frequency** ⭐⭐⭐
- [ ] [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) - **Very High Frequency** ⭐⭐⭐
- [ ] [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) - **Very High Frequency** ⭐⭐⭐
- [ ] [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/) - **High Frequency** ⭐⭐

**Time Allocation**: 5-6 hours

**Key Patterns:**
- Grid DP
- String matching DP
- Palindrome DP
- Edit distance pattern

---

### Day 10-11: Backtracking & Monotonic Stack/Queue

#### Day 10: Backtracking

**Practice Problems (Must Solve):**
- [ ] [46. Permutations](https://leetcode.com/problems/permutations/) - **Very High Frequency** ⭐⭐⭐
- [ ] [78. Subsets](https://leetcode.com/problems/subsets/) - **Very High Frequency** ⭐⭐⭐
- [ ] [39. Combination Sum](https://leetcode.com/problems/combination-sum/) - **Very High Frequency** ⭐⭐⭐
- [ ] [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) - **Very High Frequency** ⭐⭐⭐
- [ ] [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) - **High Frequency** ⭐⭐
- [ ] [51. N-Queens](https://leetcode.com/problems/n-queens/) - **High Frequency** ⭐⭐

**Time Allocation**: 5-6 hours

**Key Patterns:**
- Permutations
- Combinations
- Constraint satisfaction
- Pruning strategies

#### Day 11: Monotonic Stack & Queue

**Study Plan:**
- [ ] Watch video: [单调栈](https://www.bilibili.com/video/BV1VN411J7S7/) (if available)
- [ ] Watch video: [单调队列](https://www.bilibili.com/video/BV1bM411X72E/)

**Practice Problems (Must Solve):**
- [ ] [739. Daily Temperatures](https://leetcode.cn/problems/daily-temperatures/) - **High Frequency** ⭐⭐
- [ ] [496. Next Greater Element I](https://leetcode.cn/problems/next-greater-element-i/) - **High Frequency** ⭐⭐
- [ ] [503. Next Greater Element II](https://leetcode.cn/problems/next-greater-element-ii/) - **High Frequency** ⭐⭐
- [ ] [84. Largest Rectangle in Histogram](https://leetcode.cn/problems/largest-rectangle-in-histogram/) - **High Frequency** ⭐⭐
- [ ] [239. Sliding Window Maximum](https://leetcode.cn/problems/sliding-window-maximum/) - **Very High Frequency** ⭐⭐⭐
- [ ] [862. Shortest Subarray with Sum at Least K](https://leetcode.cn/problems/shortest-subarray-with-sum-at-least-k/) - **High Frequency** ⭐⭐

**Time Allocation**: 5-6 hours

**Key Patterns:**
- Next greater/smaller element
- Monotonic stack for rectangle problems
- Monotonic queue for sliding window max/min

---

### Day 12-13: Google-Specific Topics & Design

#### Day 12: String Manipulation & Design Problems

**String Problems (Must Solve):**
- [ ] [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) - **Very High Frequency** ⭐⭐⭐
- [ ] [394. Decode String](https://leetcode.com/problems/decode-string/) - **High Frequency** ⭐⭐
- [ ] [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) - **High Frequency** ⭐⭐
- [ ] [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/) - **High Frequency** ⭐⭐

**Design Problems (Must Solve):**
- [ ] [146. LRU Cache](https://leetcode.com/problems/lru-cache/) - **Very High Frequency** ⭐⭐⭐
- [ ] [380. Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/) - **High Frequency** ⭐⭐
- [ ] [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) - **High Frequency** ⭐⭐
- [ ] [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) - **High Frequency** ⭐⭐

**Time Allocation**: 5-6 hours

#### Day 13: Linked Lists & Heap

**Linked List Problems (Must Solve):**
- [ ] [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) - **Very High Frequency** ⭐⭐⭐
- [ ] [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) - **High Frequency** ⭐⭐
- [ ] [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) - **Very High Frequency** ⭐⭐⭐
- [ ] [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) - **High Frequency** ⭐⭐
- [ ] [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) - **High Frequency** ⭐⭐
- [ ] [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) - **High Frequency** ⭐⭐

**Heap Problems (Must Solve):**
- [ ] [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) - **Very High Frequency** ⭐⭐⭐
- [ ] [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) - **Very High Frequency** ⭐⭐⭐
- [ ] [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) - **Very High Frequency** ⭐⭐⭐

**Time Allocation**: 5-6 hours

---

### Day 14: Final Review & Mock Interviews

**Final Review:**
- [ ] Review all high-frequency problems (⭐⭐⭐)
- [ ] Create cheat sheet of patterns and templates
- [ ] Review time/space complexity for each pattern
- [ ] Practice explaining solutions clearly

**Mock Interviews:**
- [ ] Complete 2-3 full mock interviews (45 minutes each)
- [ ] Mix of topics: Arrays, Trees, DP, Backtracking
- [ ] Practice: Communication, edge cases, optimization

**Final Checklist:**
- [ ] Can solve two pointers problems in <15 minutes
- [ ] Can solve sliding window problems in <15 minutes
- [ ] Can solve tree problems in <20 minutes
- [ ] Can solve DP problems in <25 minutes
- [ ] Can explain solutions clearly
- [ ] Can identify patterns quickly
- [ ] Can handle edge cases

**Time Allocation**: 6-8 hours

---

## Daily Practice Routine

### Morning (2-3 hours)
- **Focus**: Learn new patterns, watch videos
- **Activity**: Study algorithm guide, understand concepts
- **Goal**: Build understanding before coding

### Afternoon (2-3 hours)
- **Focus**: Solve problems, implement solutions
- **Activity**: Code problems, test edge cases
- **Goal**: Master implementation

### Evening (1 hour)
- **Focus**: Review and reflect
- **Activity**: Review solutions, identify patterns, note mistakes
- **Goal**: Reinforce learning

---

## Key Patterns Cheat Sheet

### Two Pointers
```python
# Template: Two pointers from both ends
left, right = 0, len(nums) - 1
while left < right:
    # Process
    if condition:
        left += 1
    else:
        right -= 1
```

### Sliding Window
```python
# Template: Variable window size
left = 0
for right in range(len(nums)):
    # Expand window
    while not_valid(window):
        # Contract window
        left += 1
    # Process valid window
```

### Binary Search
```python
# Template: Standard binary search
left, right = 0, len(nums) - 1
while left <= right:
    mid = (left + right) // 2
    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
```

### Tree DFS
```python
# Template: Recursive DFS
def dfs(node):
    if not node:
        return
    # Process node
    dfs(node.left)
    dfs(node.right)
```

### Dynamic Programming
```python
# Template: 1D DP
dp = [0] * (n + 1)
dp[0] = base_case
for i in range(1, n + 1):
    dp[i] = recurrence_relation(dp, i)
return dp[n]
```

---

## Priority Matrix

### Must Master (⭐⭐⭐ Very High Frequency)
1. **Two Pointers** - Day 1
2. **Sliding Window** - Day 2
3. **Binary Search** - Day 3
4. **Trees (DFS/BFS)** - Day 5
5. **Dynamic Programming** - Day 8-9
6. **Arrays & Hash Tables** - Day 4

### Important (⭐⭐ High Frequency)
1. **Graphs** - Day 6
2. **Backtracking** - Day 10
3. **Monotonic Stack/Queue** - Day 11
4. **String Manipulation** - Day 12
5. **Linked Lists** - Day 13

### Good to Know
1. **Greedy Algorithms**
2. **Bit Manipulation**
3. **Math Problems**

---

## Study Tips

### 1. Focus on Patterns, Not Problems
- Understand the underlying pattern
- Recognize when to apply each pattern
- Practice pattern identification

### 2. Time Management
- Easy: 10-15 minutes
- Medium: 20-30 minutes
- Hard: 30-45 minutes
- If stuck >30 minutes, review solution

### 3. Practice Explaining
- Explain your approach before coding
- Think out loud during interviews
- Practice with friends or record yourself

### 4. Review Mistakes
- Keep a log of mistakes
- Understand why you got stuck
- Review similar problems

### 5. Mock Interviews
- Practice with time constraints
- Get feedback on communication
- Simulate real interview conditions

---

## Resources

### Primary Resources
- **Algorithm Guide**: [基础算法精讲·题目汇总](https://github.com/EndlessCheng/codeforces-go/blob/master/leetcode/README.md)
- **LeetCode**: [LeetCode Problems](https://leetcode.com/problemset/all/)
- **Google Coding Prep**: [Google Coding Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-coding-interview-preparation %})

### Additional Resources
- **Google System Design**: [Google System Design Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-system-design-interview-preparation %})
- **Google Behavioral**: [Google Behavioral Interview Guide]({{ site.baseurl }}{% post_url 2025-12-15-google-behavioral-interview-preparation-guide %})
- **LeetCode Templates**: Solution patterns and templates

### Practice Platforms
- **LeetCode**: Primary practice platform
- **Pramp**: Free mock interviews
- **Interviewing.io**: Anonymous mock interviews

---

## Final Checklist (Day 14)

### Technical Skills
- [ ] Can solve 80%+ of high-frequency problems independently
- [ ] Can identify patterns within 2-3 minutes
- [ ] Can optimize solutions (time/space complexity)
- [ ] Can handle edge cases
- [ ] Can explain solutions clearly

### Communication Skills
- [ ] Can think out loud effectively
- [ ] Can ask clarifying questions
- [ ] Can discuss trade-offs
- [ ] Can handle feedback gracefully

### Interview Readiness
- [ ] Completed 5+ mock interviews
- [ ] Reviewed all high-frequency topics
- [ ] Created pattern cheat sheet
- [ ] Practiced with time constraints
- [ ] Reviewed Google-specific questions

---

## Success Metrics

### Week 1 Goals
- ✅ Master two pointers and sliding window
- ✅ Solve 30+ medium problems
- ✅ Complete 1-2 mock interviews

### Week 2 Goals
- ✅ Master DP and backtracking
- ✅ Solve 40+ medium/hard problems
- ✅ Complete 3-4 mock interviews
- ✅ Can solve problems in <30 minutes

### Final Goal
- ✅ Confident in all high-frequency topics
- ✅ Can solve problems under time pressure
- ✅ Can communicate solutions clearly
- ✅ Ready for Google interview

---

## Remember

1. **Quality over Quantity**: Better to master 50 problems than rush through 200
2. **Pattern Recognition**: Focus on recognizing patterns, not memorizing solutions
3. **Practice Communication**: Technical skills + communication = success
4. **Stay Calm**: Interviews are conversations, not tests
5. **Learn from Mistakes**: Every mistake is a learning opportunity

**Good luck with your Google interview preparation! You've got this! 🚀**

---

**Related Posts:**
- [Google Coding Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-coding-interview-preparation %})
- [Google System Design Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-system-design-interview-preparation %})
- [Google Behavioral Interview Guide]({{ site.baseurl }}{% post_url 2025-12-15-google-behavioral-interview-preparation-guide %})

