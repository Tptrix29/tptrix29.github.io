---
title:  "Dynamic Programming Solution"
date:   2024-02-11 -0500
categories: programming
tags: leetcode
classes: wide
header:
    teaser: 
---

## Dynamic Programming

### Basic Study Plan

#### [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

**Recursive Formula**

$N[n] = N[n-1] + N[n-2], n> 2$

$N[0] = 0, N[1] = 1, N[2] = 2$

**Python3**

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [0, 1, 2]
        for i in range(3, n+1):
            dp.append(dp[i-1] + dp[i-2])
        return dp[n]
```

#### [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

**Recursive Formula**

$F[n] = F[n-1] + F[n-2], n > 1$

$F[0] = 0, F[1] = 1$

**Python3**

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1 or n == 2:
            return n
        # store result for reusage
        count = [0] * (n+1)
        count[1], count[2] = 1, 2 
        # climb submodule
        def climb(n):
            if not count[n]:
                count[n] = climb(n-1) + climb(n-2)
            return count[n]
        return climb(n)
```

#### [N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/)

**Recursive Formula**

$F[n] = F[n-1] + F[n-2], n \ge 2$

$F[0] = 0, F[1] = 1$

**Python3**

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1 or n == 2:
            return n
        # store result for reusage
        count = [0] * (n+1)
        count[1], count[2] = 1, 2 
        # climb submodule
        def climb(n):
            if not count[n]:
                count[n] = climb(n-1) + climb(n-2)
            return count[n]
        return climb(n)
```

#### [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

**Recursive Formula**

$OPT[i] = \min\{OPT[i-1] +cost[i-1], OPT[i-2] +cost[i-2]\}, i \ge 2$​

$OPT[0] = OPT[1] = 0$

**Python3**

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        opt = [0] * (n+1)
        for i in range(2, n+1):
            opt[i] = min(opt[i-1] + cost[i-1], opt[i-2] + cost[i-2])
        return opt[n]  
        
```

#### [House Robber](https://leetcode.com/problems/house-robber/)

**Recursive Formula**

$$
OPT[i] = \max \cases{
	\displaystyle \max_{0\le j \le i-2}\{OPT[j] + nums[i]\}, \text{rob $i$-th house}\\
	\displaystyle \max_{0 \le j \le i -1}\{OPT[j]\}, \text{Not rob $i$-th house}
}, 0\le i < n
$$

**Python3**

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        opt = [0] * (n)
        opt[0] = nums[0]
        for i in range(1, n):
            # rob i-th house
            max_rob = max(opt[:i-1]) + nums[i] if i > 1 else nums[i]
            # not rob i-th house & update 
            opt[i] = max(max(opt[:i]), max_rob)
        return opt[n-1]
```

#### [Delete and Earn](https://leetcode.com/problems/delete-and-earn/)

**Recursive Formula**

Denote $V$ as sorted values in $nums$, $n = |V|$, $OPT[i]$ denotes the max score for the subset with $V[i-1]$ as maximum
$$
OPT[i] = \max \cases{
	\displaystyle \cases{
		OPT[i-2]+ V[i-1] * \# V[i-1], \text{if $V[i-1] = V[i-2]+1$}\\
		OPT[i-1]+ V[i-1] * \# V[i-1], \text{if $V[i-1] \neq V[i-2]+1$}
	}, \text{delete $V[i-1]$}\\
	\displaystyle OPT[i-1], \text{Not delete $V[i-1]$}
}, 0< i \le n
$$

$OPT[0] = 0$

**Python3**

```python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        counter = Counter(nums)
        vals = sorted(counter.keys())
        opt = [0] * (len(vals) + 1)
        for i in range(1, len(vals)+1):
            score = vals[i-1] * counter[vals[i-1]]
            if vals[i-1] != vals[i-2]+1:
                opt[i] = opt[i-1] + score
            else:
                opt[i] = max(opt[i-1], opt[i-2] + score)
        return opt[-1]
```

#### [Unique Paths](https://leetcode.com/problems/unique-paths/)

**Recursive Formula**

$grid[i][j] = grid[i][j-1] + grid[i-1][j], 0<i<m, 0<j<n$​

$grid[i][0] = 1, grid[0][j] = 1, 0<i<m, 0<j<n$​

Fill order: row by row / column by column

> Notice: initialization of matrix 

**Python3**

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        grid = [[1 for _ in range(n)] for _ in range(m)]
        for i in range(1, m):
            for j in range(1, n):
                grid[i][j] = grid[i-1][j] + grid[i][j-1]
        return grid[m-1][n-1]
```



#### [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

**Recursive Formula**

$OPT[i][j] = \min \{OPT[i-1][j], OPT[i][j-1]\} + grid[i][j], 0<i<m, 0<j<n$

$OPT[0][0] = grid[0][0]$

$OPT[i][0] = OPT[i-1][0] + grid[i][0], 0<i<m$

$OPT[0][j] = OPT[0][j-1] + grid[0][j], 0<j<n$

**Python3**

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        opt = [[grid[0][0] for _ in range(n)] for _ in range(m)]
        for i in range(1, m):
            opt[i][0] = opt[i-1][0] + grid[i][0]
        for j in range(1, n):
            opt[0][j] = opt[0][j-1] + grid[0][j]
        for i in range(1, m):
            for j in range(1, n):
                opt[i][j] = min(opt[i][j-1], opt[i-1][j]) + grid[i][j]
        return opt[m-1][n-1] 
        
```

#### [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

**Recursive Formula**

$OPT[i][j] = OPT[i-1][j] * (1-obstacle[i-1][j]) + OPT[i][j-1] * (1-obstacle[i][j-1]), 0<i<m, 0<j<n$

<span style="color:red">**Notice:Boundary Condition** </span>(*Consider the realistic scenario!!!*)

For first row and column, possible path become 0 once an obstacle occurs in the row/column

**Python3**

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        opt = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(m):
            if obstacleGrid[i][0]:
                break
            opt[i][0] = 1
        for j in range(n):
            if obstacleGrid[0][j]:
                break
            opt[0][j] = 1
        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j]:
                    opt[i][j] = 0
                else:
                    opt[i][j] = opt[i-1][j] * (1-obstacleGrid[i-1][j]) + \
                            opt[i][j-1] * (1-obstacleGrid[i][j-1])
        return opt[m-1][n-1]
```



#### [Triangle](https://leetcode.com/problems/triangle/)

**Recursive Formula**

$OPT[i][j]$ denotes mininal sum at $j$-th location of $i$​-th layer

$OPT[i][j] = \min \{OPT[i-1][\min(i-1, j)], OPT[i-1][max(0, j-1)]\} + V[i][j], 0<i<n, 0\le j\le i$

$OPT[0][0] = V[0][0]$

**Python3**

```python
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        prev = triangle[0]
        n = len(triangle)
        if n == 1:
            return triangle[0][0]
        for i in range(1, n):
            count = len(triangle[i])
            cur = [0] * count
            for j in range(count):
                cur[j] = min(prev[max(0, j-1)], prev[min(i-1, j)]) + \
                           triangle[i][j]
            prev = cur
        return min(cur)
```



#### [Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/)

**Recursive Formula**

$OPT[i][j] = \min\{OPT[i-1][j-1], OPT[i-1][j], OPT[i][j-1]\} + M[i][j], 0\le i, j < n$​

$OPT[-1][j] = OPT[i][-1] = OPT[n][j] = OPT[i][n] = \infty$

**Python3**

```python
class Solution:
    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        n = len(matrix)
        if n == 1:
            return matrix[0][0]
        prev = matrix[0]
        max_int = 2 ** 31 - 1
        for i in range(1, n):
            cur = [0] * n
            for j in range(n):
                val1 = prev[j-1] if j > 0 else max_int
                val2 = prev[j]
                val3 = prev[j+1] if j < n-1 else max_int
                cur[j] = min(val1, val2, val3) + matrix[i][j]
            prev = cur
        return min(cur)
      
```



#### [Maximal Square](https://leetcode.com/problems/maximal-square/)

**Recursive Formula**

$$
OPT[i][j] = \cases{
	0, M[0][0] == 0\\
	\cases{
		\min\{OPT[i-1][j], OPT[i][j-1]\} + 1, OPT[i-1][j] == OPT[i][j-1] \\
		OPT[i-1][j] + I(OPT[i-1][j-1] \ge OPT[i-1][j])
	}, M[0][0] == 1
} 0 < i < m, 0 < j < n
$$

$OPT[i][0] = M[i][0], 0 < i < m$​

$OPT[0][j] = M[0][j], 0 < j < n$

**Python3**

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        m, n = len(matrix), len(matrix[0])
        opt = [[int(matrix[i][j]) for j in range(n)] for i in range(m)]
        result = int(max(matrix[0]))
        for i in range(1, m):
            result = max(int(matrix[i][0]), result)
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] == "1":
                    val1, val2 = opt[i-1][j], opt[i][j-1]
                    if val1 == val2:
                        opt[i][j] = val1 + 1 if opt[i-1][j-1] >= val1 else val1
                    else:
                        opt[i][j] = min(val1, val2) + 1
                    result = max(result, opt[i][j])
        return result ** 2
```



#### [Word Break](https://leetcode.com/problems/word-break/)

**Recursive Formula**

$DP[i]$ denotes whether $s[:i+1]$ could be break into words in dictionary

- $\exists j < i, DP[j] == true \text{ and } s[j+1:i+1] \in dict \Rightarrow  DP[i] = true$​ 
- $s[:i+1] \in dict \Rightarrow DP[i] = true$​

**Python3**

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        n = len(s)
        dp = [False] * n
        dp[0] = True if s[0] in wordDict else False
        for i in range(1, n):
            j = i-1
            while j >= 0:
                if dp[j]:
                    if s[j + 1:i + 1] in wordDict:
                        dp[i] = True
                        break
                j -= 1
            dp[i] = s[:i+1] in wordDict or dp[i]
        return dp[-1]
```



#### [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

**Recursive Formula**

$DP[i][j]$ denotes the length of longest palindromic subsequence in $s[i:j+1]$
$$
DP[i][j] = \cases{
	1, i == j\\
	DP[i+1][j] + 2, \text{if } s[i] == s[j]\\
	\max\{DP[i+1][j], DP[i][j-1]\}, \text{otherwise}
}, 0 \le i \le j < n
$$

**Python3**

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [[0 for _ in range(n)] for _ in range(n)]
        for i in range(n-1, -1, -1):
            dp[i][i] = 1
            for j in range(i+1, n):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])
        return dp[0][-1]
```



#### [Edit Distance](https://leetcode.com/problems/edit-distance/)

**Recursive Formula**

$DP[i][j]$ denotes the min edit distance of $word_1[:i+1]$ and $word_2[:j+1]$​​

$\text{For } 0<i<|word_1|, 0<j<|word_2|$ $
$$
DP[i][j] = \cases{
	DP[i-1][j-1], \text{if } word_1[i] == word_2[j]\\
	\min\{DP[i-1][j-1], DP[i][j-1], DP[i-1][j]\} + 1, \text{if } word_1[i] \neq word_2[j]
} \\
DP[i][0] = \cases{
	i+1 \text{ if } word_2[0] \notin word_1[:i+1] \\
	i, \text{otherwise}
}\\
DP[0][j] = \cases{
	j+1 \text{ if } word_1[0] \notin word_2[:j+1] \\
	j, \text{otherwise}
}
$$


**Python3**

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        len1, len2 = len(word1), len(word2)
        # boundary case
        if not len1 or not len2:
            return max(len1, len2)
        dp = [[0 for _ in range(len2)] for _ in range(len1)]
        dp[0][0] = 0 if word1[0] == word2[0] else 1
        # initialize first column
        flag = 1 if word1[0] == word2[0] else 0
        for i in range(1, len1):
            if word1[i] == word2[0]:
                flag = 1
            dp[i][0] = i + 1 - flag
        # initialize first row
        flag = 1 if word1[0] == word2[0] else 0
        for j in range(1, len2):
            if word1[0] == word2[j]:
                flag = 1
            dp[0][j] = j + 1 - flag
        # dynamic programming
        for i in range(1, len1):
            for j in range(1, len2):
                if word1[i] == word2[j]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i][j-1], dp[i-1][j], dp[i-1][j-1]) + 1
        return dp[-1][-1]
```



#### [Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/)

**Recursive Formula**

$OPT[i][j] = \cases{\min\{OPT[i-1][j] + ascii(s_1[i-1]), OPT[i][j-1] + ascii(s_2[j-1])\}, \text{if } s_1[i-1] \neq s_2[j-1] \\ OPT[i-1][j-1], \text{if } s_1[i-1] == s_2[j-1]}, 1\le i \le |s_1|, 1\le j \le |s_2|$

$OPT[i][0] = OPT[i-1][0] + ascii(s_1[i-1]), 1\le i\le |s_1|$​

$OPT[0][j] = OPT[0][j-1] + ascii(s_2[j-1]), 1\le j\le |s_2|$

**Python3**

```python
class Solution:
    def minimumDeleteSum(self, s1: str, s2: str) -> int:
        l1, l2 = len(s1), len(s2)
        dp = [[0 for _ in range(l2+1)] for _ in range(l1+1)]
        for i in range(1, l1+1):
            dp[i][0] = dp[i-1][0] + ord(s1[i-1])
        for j in range(1, l2+1):
            dp[0][j] = dp[0][j-1] + ord(s2[j-1])
        for i in range(1, l1+1):
            for j in range(1, l2+1):
                if s1[i-1] == s2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j] + ord(s1[i-1]), dp[i][j-1] + ord(s2[j-1]))
        return dp[l1][l2]
```



#### [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

**Recursive Formula**

$OPT[i][j]$ denotes the count of subsequences in $s[:j+1]$ matching $t[:i+1]$​

When one letter matches, you could choose either including it in subsequence or not.

$OPT[i][j] = \cases{OPT[i][j-1] + OPT[i-1][j-1], \text{if } s[i-1] == t[j-1]\\ OPT[i][j-1], \text{otherwise}}, 1\le i\le |t|, 1 \le j \le |s|$​

$OPT[i][0] = 0, OPT[0][j] = 1, 1\le i\le |t|, 0 \le j \le |s|$

**Python3**

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        l1, l2 = len(t), len(s)
        dp = [[0 for _ in range(l2+1)] for _ in range(l1+1)]
        for j in range(1, l2+1):
            dp[0][j] = 1
        dp[0][0] = 1
        for i in range(1, l1+1):
            for j in range(1, l2+1):
                if t[i-1] == s[j-1]:
                    dp[i][j] = dp[i-1][j-1] + dp[i][j-1]
                else:
                    dp[i][j] = dp[i][j-1]
        return dp[l1][l2]
```



#### [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

**Recursive Formula**

$OPT[i]$ demostrates the LIS ending at $i$-th index
$$
OPT[i] = \max_{0 \le j < i}\cases{
	OPT[j]+1, \text{if }nums[i] > nums[j] \\
	1,  \text{otherwise }
	}, 0 < i < n
$$
Boundary: $OPT[0] = 1$​

Output: $\max\{OPT[i]\}, 0 \le i < n$

**Python3**

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [1] * n
        for i in range(1, n):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[j] + 1, dp[i])
        return max(dp)
        
```



#### [Number Of Longest Increasing Subsequences](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

**Recursive Formula**

$DP_1$ demonstrates the LIS ending at $i$-th index

$DP_2$ demonstrates the count of LIS ending at $i$-th index
$$
DP_2[i] = \max_{0 \le j < i}\cases{
	\cases{
		DP_2[i] + DP_2[j], \text{if } DP_1[j] + 1 = DP_1[i]\\
    DP_2[j], \text{if } DP_1[j] + 1 > DP_1[i]
	}, \text{if }nums[i] > nums[j] \\
	1,  \text{otherwise }
	}, 0 < i < n
$$
Boundary: $DP_2[0] = 1$

Output: $\displaystyle \sum^{n-1}_{i=0} DP_2[i]*I(DP_1[i] == \max DP_1)$

**Python3**

```python
class Solution:
    def findNumberOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        dp1 = [1] * n
        dp2 = [1] * n
        for i in range(1, n):
            for j in range(i):
                if nums[i] > nums[j]:
                    # find prev seq
                    if dp1[i] == dp1[j] + 1:
                        dp2[i] += dp2[j]
                    # update LIS
                    elif dp1[i] < dp1[j] + 1:
                        dp2[i] = dp2[j]
                        dp1[i] = dp1[j] + 1
        M = max(dp1)
        result = 0
        for i in range(n):
            if dp1[i] == M: 
                result += dp2[i]
        return result
```





#### [Longest Arithmetic Subsequence of Given Difference](https://leetcode.com/problems/longest-arithmetic-subsequence-of-given-difference/)

**Recursive Formula**

Use hash table to store dp table, $DP[v]$ denotes the max length of arithmetic subsequence ends at $v$

$DP[v] = DP[v-d] + 1, v \in A$

$DP[v] = 0 \text{ if } v \notin A$

**Python3**

```python
class Solution:
    def longestSubsequence(self, arr: List[int], difference: int) -> int:
        dp = defaultdict(int)
        for v in arr:
            dp[v] = dp[v-difference] + 1
        return max(dp.values())
```



#### 

**Recursive Formula**



**Python3**

```python

```



#### [Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)

**Recursive Formula**

$count[i, k]$ denotes $\#(BST)$ forming with values from 1 to $k$ with $i$​ as root 

$DP[k]$ denotes  $\#(BST)$ forming with values from 1 to $k$, $DP[k] = \displaystyle \sum_{i=1}^k count[i, k]$

$count[i, k] = DP[i-1] * DP[k-i]$​

$DP[k] = \displaystyle \sum_{i=1}^k DP[i-1] * DP[k-i], 0 < i \le k, 2 \le k \le n$

Boundary: $DP[0] = DP[1] = 1$

Output: $DP[n]$

**Python3**

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0] * (n+1)
        dp[0] = dp[1] = 1
        for k in range(2, n+1):
            for i in range(1, k+1):
                dp[k] += dp[i-1] * dp[k-i]
        return dp[n]
```

### Other

#### [Jump Game II](https://leetcode.com/problems/jump-game-ii/description/)

**Recursive Formula**

Original Method: 

Define $DP[i]$ as min step to reach $i$, $0\le i < |L|$

$DP[i] = \displaystyle \min_{k < i}\{f(k): f(k) =\begin{cases}&DP[k]+1, \text{if}~nums[k] + k \ge i \\ & \infty , \text{otherwise}\end{cases}\}$ 

$DP[0] = 0$

Optimized Method: 

Define $DP[i]$ as the farest index you can reach within one step from any index $k$ where $k\le i$

$DP[i] = \displaystyle \max_{k \le i}\{DP[k], i + nums[i]\}$

Given $DP[k] < DP[k+1]$, so $DP[i] = \displaystyle \max\{DP[i-1], i + nums[i]\}$

**Python3**

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        l = len(nums)
        dp = [0 for _ in range(l)]
        for i in range(1, l):
            dp[i] = min([dp[k]+1 if nums[k] + k >= i else l for k in range(i)])
        return dp[-1]
        
```

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        l = len(nums)
        dp = [nums[0] for _ in range(l)]
        for i in range(1, l):
            dp[i] = max(dp[i-1], nums[i]+i)
        step = next_idx = 0
        while next_idx < l - 1:
            next_idx = dp[next_idx]
            step += 1
        return step
        
```

