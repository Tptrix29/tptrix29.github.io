---
title:  "Leetcode Solution"
date:   2024-01-16 -0500
categories: programming
tags: leetcode
author_profile: true
classes: wide
header:
    teaser: /assets/img/training-guidence.png
---

## Problem List Notes
Longest substring without repeated character: use 2 pointers to maintain set of char composition and update satisfying substring

Palindrome string: search from center / Not stack

Median of 2 arrays: divide and conquer (find median position), viz the index as slot between elements

Reverse integer: negative transition, range alert, negative mode & divide calculation (floor)

3/4 sum: delete branch(es) for acceleration (count, target), 2 pointers

remove n-th tail node of list: fast & slow pointers

Parenthesis generation: DFS, control left and right parenthesis count

Next Permutation: find location from the right side

Reverse ListNode: dummy head, prev node


## Solutions

#### [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

**Strategy**

*digit-wise arithmetic addition*

Clarify each step for arithmetic addition 

**Python3**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        add_digit = 0
        head = ListNode()
        result = head
        while l1 or l2:
            val1 = l1.val if l1 else 0
            val2 = l2.val if l2 else 0
            s = val1 + val2 + add_digit
            if s >= 10:
                add_digit = 1
                s -= 10
            else:
                add_digit = 0
            result.next = ListNode(val = s)
            result = result.next
            l1 = l1.next if l1 else l1
            l2 = l2.next if l2 else l2
        if add_digit:
            result.next = ListNode(val = add_digit)
        return head.next
        
```



#### [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

**Strategy**

*Set Maintainance + Double Pointers*

Maintain the set of character composition

Locate the substring with 2 pointers

While meeting same character which already exists in composition set, shift the left pointer until there is no repeated character

**Python3**

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if len(s) <= 1:
            return len(s)
        chr_set = set()
        left = 0
        opt = 0
        for right in range(len(s)):
            if s[right] in chr_set:
                while s[right] in chr_set:
                    if s[left] in chr_set: 
                        chr_set.remove(s[left]) 
                    left += 1
            chr_set.add(s[right])
            opt = max(right - left + 1, opt)
        return opt
```

**Java**

```java
```



#### [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

**Strategy**

*Divide-and-conquer*

Boundary condition

Odd-count / Even-count handing

**Python3**

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
      	# target global shift count
        k = (len(nums1) + len(nums2)) // 2 + 1
        p1 = p2 = 0	# pointer
        # shift pointer 
        while (p1 < len(nums1) or p2 < len(nums2)) and k > 1:
            m = k // 2
            # nums1 count to the end
            if p1 >= len(nums1):
                p2 += m
                k -= m
            # nums2 count to the end
            elif p2 >= len(nums2):
                p1 += m
                k -= m
           	# nums1 and nums2 both have remained number
            else:
              	# potential position after shifting
                i1 = min(len(nums1), p1 + m)
                i2 = min(len(nums2), p2 + m)
                # shift the pointer to position with smaller value
                if nums1[i1-1] > nums2[i2-1]:
                    k = k - i2 + p2
                    p2 = i2
                else:
                    k = k - i1 + p1
                    p1 = i1
				# calculate next value in order
        if p1 >= len(nums1):
            next_val = nums2[p2]
        elif p2 >= len(nums2):
            next_val = nums1[p1]
        else:
            next_val = min(nums1[p1], nums2[p2])
        
        # odd count 
        if (len(nums1) + len(nums2)) % 2:
            return float(next_val)
        # even count
        else:
          	# No number in front of p1 position in nums1
            if p1 == 0:
                return (next_val + nums2[p2 - 1]) / 2
            # No number in front of p2 position in nums2
            elif p2 == 0:
                return (next_val + nums1[p1 - 1]) / 2
            else:
                return (next_val + max(nums1[p1-1], nums2[p2-1])) / 2
```



#### [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

**Strategy**

*Property of palindromic substring (**search from center**)*

**Extend the palindromic string from potential substring center**

Attention: the slot between the characters could also become the center!  

**Python3**

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        def expandFromCenter(l: int, r: int) -> int:
            while l >= 0 and r < len(s) and s[l] == s[r]:
                l -= 1
                r += 1
            return s[l+1:r]
        
        max_len = 1
        max_palindrome = s[0]
        for i in range(len(s)-1):
            str1 = expandFromCenter(i, i)
            str2 = expandFromCenter(i, i+1)
            if len(str1) > max_len:
                max_len = len(str1)
                max_palindrome = str1
            if len(str2) > max_len:
                max_len = len(str2)
                max_palindrome = str2
        return max_palindrome
        
```



#### [Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

**Strategy**

*Formula Reasoning*

Periodic Formula: 

$T = 2 *(\#rows - 1) $

$\#T = ceil(length / T)$

Reconstruct the string according to location of characters

**Python3**

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s
        T = 2 * numRows - 2
        n = len(s) // T + 1
        result = ""
        for i in range(numRows):
            for j in range(n):
                result += s[j * T + i] if j * T + i < len(s) else ""
                if i != 0 and i != (numRows - 1):
                    result += s[(j+1) * T - i] if (j+1) * T - i < len(s) else ""
        return result                
```

**Java**

```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows == 1){
            return s;
        }
        int T = 2 * numRows - 2;
        int n = s.length() / T + 1;
        StringBuilder result = new StringBuilder();
        for(int i = 0; i < numRows; i++){
            for(int j = 0; j < n; j++){
                if(j * T + i < s.length()){
                    result.append(s.charAt(j * T + i));
                }
                if(i != 0 && i != numRows - 1){
                    if((j + 1) * T - i < s.length()){
                        result.append(s.charAt((j + 1) * T - i));
                    }
                }
            }
        }
        return result.toString();
    }
}
```



#### [Reverse Integer](https://leetcode.com/problems/reverse-integer/)

**Strategy**

*Some tricks*

- Postive -> Negative
- Modulus calculation for negative
- Overflow check

Attention: modulus and divide is **DIFFERENT** in Java and Python!

> Python:
>
> -17 // 10 = -2
>
> -17 % 10 = 3
>
> Java
>
> -17 // 10 = -1
>
> -17 % 10 = -7

**Python3**

```python
class Solution:
    def reverse(self, x: int) -> int:
        if x == 0 or x > 2 ** 31 - 1 or x < -2 ** 31:
            return 0
        positive = True if x > 0 else False
        x = -x if positive else x
        result = 0
        while x != 0:
            resid = x % 10 - 10
            if resid == -10:
                resid = 0
                x = x // 10
            else:
                x = x // 10 + 1
            result = result * 10 + resid
        result = -result if positive else result
        return 0 if result > 2 ** 31 - 1 or result < -2 ** 31 else result
```





#### [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

**Strategy**

*Overflow handling*

**Python3**

```python
class Solution:
    def myAtoi(self, s: str) -> int:
        s = s.lstrip()
        if len(s) == 0:
            return 0
        positive = True
        if s[0] == "-":
            positive = False
            i = 1
        elif s[0] == "+":
            i = 1
        else:
            i = 0

        num = 0
        for k in range(i, len(s)):
            if '0' <= s[k] <= '9':
                num = num * 10 - int(s[k])
            else:
                break

        min_int32 = -2 ** 31
        max_int32 = 2 ** 31 - 1
        num = -num if positive else num
        if num > max_int32:
            return max_int32
        elif num < min_int32:
            return min_int32
        else:
            return num      
```



#### [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

**Strategy**

*Greedy + Double Pointers*

Start searching from the outermost planks, then substitute the shorter plank and calculate the potential largest area

DP is too time-consuming!

**Python3**

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        if len(height) < 2:
            return 0
        left = 0
        right = len(height) - 1
        max_area = 0
        while left < right:
            area = (right - left) * min(height[left], height[right])
            if max_area < area: 
                max_area = area
            if height[left] <= height[right]:
                left += 1
            else:
                right -= 1
        return max_area
```



#### [3Sum](https://leetcode.com/problems/3sum/)

**Strategy**



**Python3**

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) < 3:
            return []
        nums = sorted(nums)
        results = []
        for i in range(len(nums) - 2):
            if nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i-1]:
                continue
            j = i + 1
            k = len(nums) - 1
            while j < k:
                summation = nums[j] + nums[k]
                if summation == -nums[i]:
                    results.append([nums[i], nums[j], nums[k]])
                    prev1, prev2 = nums[j], nums[k]
                    while j < k and nums[j] == prev1:
                        j += 1
                    while j < k and nums[k] == prev2:
                        k -= 1
                elif summation > -nums[i]:
                    k -= 1
                else:
                    j += 1
        return results
        
```



#### [3Sum Closest](https://leetcode.com/problems/3sum-closest/)

**Strategy**



**Python3**

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        if len(nums) < 3: 
            return []
        nums.sort()
        s = nums[0] + nums[1] + nums[2]
        opt = abs(s - target)
        l = len(nums)
        for i in range(len(nums) - 2):
            j = i + 1
            k = l - 1
            while j < k:
                summation = nums[i] + nums[j] + nums[k]
                dist = summation - target
                if abs(dist) < opt:
                    opt = abs(dist)
                    s = summation
                if dist == 0: 
                    return target
                elif dist > 0: 
                    k -= 1
                else:
                    j += 1
        return s
```



#### [4Sum](https://leetcode.com/problems/4sum/)

**Strategy**



**Python3**

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        l = len(nums)
        if l < 4:
            return []
        nums.sort()
        results = []
        for i in range(l - 3):
            if i > 0 and nums[i-1] == nums[i]:
                continue
            for j in range(i + 1, l - 2):
                m = j + 1
                n = l - 1
                while m < n:
                    summation = nums[i] + nums[j] + nums[m] + nums[n]
                    if summation == target:
                        if [nums[i], nums[j], nums[m], nums[n]] not in results:
                            results.append([nums[i], nums[j], nums[m], nums[n]])
                        prev1, prev2 = nums[m], nums[n]
                        while nums[m] == prev1 and m < n:
                            m += 1
                        while nums[n] == prev2 and m < n:
                            n -= 1
                    elif summation < target:
                        m += 1
                    else:
                        n -= 1

        return results   
```





#### [Remove N-th Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

**Strategy**

*Double Pointers (Fast & slow)*

**Python3**

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        if not head:
            return None
        node = head
        while n:
            node = node.next
            n -= 1
        fast, slow = node, head
        if not fast:
            return head.next
        while fast.next:
            fast, slow = fast.next, slow.next
        slow.next = slow.next.next if slow else None
        return head
```





#### [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

**Strategy**

*Stack*

**Python3**

```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) == 0:
            return True
        parentheses_pair = {'(': ')', '[': ']', '{': '}'}
        stack = []
        for i in range(len(s)):
            char = s[i]
            if char in parentheses_pair.keys():
                stack.append(char)
            else:
                if stack and parentheses_pair[stack[-1]] == char:
                    stack.pop()
                else:
                    return False
        return False if stack else True    
```





#### [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

**Strategy**

*DFS Searching*

Use 2 variables to record the count of left & right parentheses 

- When $\#left < n$, append a `(` and generate a searching branch
- When $\#right < \#left$, append a `)` and generate a searching branch

Terminate condition: $length(string) = 2n$​

> Validity is guaranteed by the calling stack

**Python3**

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        def dfs(l: int, r: int, s):
            if len(s) == n * 2:
                res.append(s)
                return
        		# left parenthese is insufficient
            if l < n:
                dfs(l+1, r, s + '(')
            # right parethese is insufficient
            if r < l:
                dfs(l, r+1, s + ')')
            
        res = []
        dfs(1, 0, '(')
        return res
        
```





#### [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

**Strategy**

*Heap usage*

Optimized data structure for max/min value

**Python3**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
    
ListNode.__lt__ = lambda a, b: a.val < b.val


class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        current = dummy = ListNode(val = -2**63)

        # Method 1: Heap
        # heap = [head for head in lists if head]
        # heapify(heap)
        # while heap:
        #     node = heappop(heap)
        #     if node.next:
        #         heappush(heap, node.next)
        #     current.next = node
        #     current = current.next
        

        # Method 2: Brute-Force
        tail = dummy
        def clear_none_node(nodes):
            i = len(nodes) - 1
            while i >= 0:
                if not nodes[i]:
                    nodes.pop(i)
                i -= 1
                
        clear_none_node(lists)
        while lists:
            for i in range(len(lists)):
                node = lists[i]
                lists[i] = lists[i].next
                if node.val >= tail.val:
                    tail.next = node
                    tail = tail.next
                else:
                    current = dummy
                    while current.val < node.val:
                        if current.next.val >= node.val:
                            break
                        current = current.next
                    node.next = current.next
                    current.next = node
            # head = tail
            clear_none_node(lists)
        return dummy.next
```







#### [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

**Strategy**



**Python3**

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next:
            return head
        assist_head = ListNode(0, head)
        counter = 2
        node1, node2 = head, head.next
        while node2:
            if counter == 2:
                swap = node2.val
                node2.val = node1.val
                node1.val = swap
                counter = 0
            else:
                counter += 1
                node1, node2 = node1.next, node2.next
                # prev, node1, node2, post = prev.next, node1.next, node2.next, post.next
        return assist_head.next

```





#### [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

**Strategy**

*Double Pointers*

**Reverse List**:

1. store post node
2. 

**Python3**

```python
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        def reverse(head):
            prev = None
            current = head
            while current:
                post = current.next
                current.next = prev
                prev = current
                current = post
            return prev
        
        dummy = ListNode(next = head)
        prev = end = dummy
        count = 0
        while end:
            while count < k and end:
                end = end.next
                count += 1
            if not end:
                return dummy.next
            start = prev.next
            post = end.next
            end.next = None
            prev.next = reverse(start)
            start.next = post
            end = prev = start
            count = 0
        return dummy.next

        
```



#### [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

**Strategy**



**Python3**

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        i = 0
        # maintain array
        # array slice: new space
        arr = nums
        while i < len(arr) - 1:
            l = 0
            while i + l + 1 < len(arr) and arr[i] == arr[i + l + 1]:
                l += 1
            if l:
                for j in range(len(arr) - i - l - 1):
                    arr[j + i + 1] = arr[j + l + i + 1]
                arr = arr[:-l]
            i += 1
            if i < len(arr):
                nums[i] = arr[i]
        return len(arr)
        
        # j = 1
        # for i in range(1, len(arr)):
        #     if arr[i] != arr[i - 1]:
        #         arr[j] = arr[i]
        #         j += 1
        # return j
```





#### [Next Permutation](https://leetcode.com/problems/next-permutation/)

*Property of permutation*

**Strategy**

1. Find the first peak element from right side, set element BEFORE peak element ($e_1$)
2. Swap $e_1$ and the smallest element which is great than $e_1$ in right side
3. Reverse right side array

**Python3**

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        def swap(nums, i, j):
            temp = nums[i]
            nums[i] = nums[j]
            nums[j] = temp

        def reverse(nums):
            i, j = 0, len(nums) - 1
            while i < j:
                swap(nums, i, j)
                i += 1
                j -= 1
            return nums
            
        if len(nums) == 1:
            return nums
        
        # step1: find break-point
        i = len(nums) - 2
        while nums[i] >= nums[i+1] and i >= 0:
            i -= 1
        
        # special case
        if i == -1:
            reverse(nums)
            return
        
        # step2: find swap location
        j = len(nums) - 1
        while nums[j] <= nums[i]:
                j -= 1
        
        # step3: swap number
        swap(nums, i, j)

        # step4: reverse right half after break-point
        if i+1 < len(nums) - 1:
            nums[i+1:] = reverse(nums[i+1:]) 
```



#### [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

**Strategy**

1. Use stack to find valid substrings
2. Store the start and end index of valid substrings
3. Find longest substring by retrieving the index

**Python3**

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        if len(s) < 2:
            return 0
        stack = []
        for i in range(len(s)):
            if s[i] == "(":
                stack.append(i)
            else:
                if stack and s[stack[-1]] == "(":
                    stack.pop()
                else:
                    stack.append(i)
        
        if not stack:
            return len(s)
        longest = 0
        r, l = len(s), 0
        while stack:
            l = stack.pop()
            longest = max(r-l-1, longest)
            r = l
        longest = max(longest, r)
        
        return longest
```







#### [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

**Strategy**

*Divide-and-conquer*

Compare the values of `left` position and `right `position to determine whether searching in monotone array range

**NOTICE: ** searching range preference for divide-and-conquer strategy is $[left, right)$.

**Python3**

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        def searchRange(nums: List[int], left: int, right: int, target: int) -> int:
            if right - left <= 1:
                return left if len(nums) > 0 and nums[left] == target else -1
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            if nums[left] > nums[right-1]:
                ind1, ind2 = searchRange(nums, left, mid, target), searchRange(nums, mid, right, target)
                return ind1 if ind1 != -1 else ind2
            else:
                if target < nums[mid]:
                    return searchRange(nums, left, mid, target)
                else:
                    return searchRange(nums, mid, right, target)
        
        return searchRange(nums, 0, len(nums), target)    
```





#### [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

**Strategy**

*Divide-and-conquer*

Search for lower and upper bounds seperately

**NOTICE**: returning list created in function should be avoided (stack memory released => return `None`)



**Python3**

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def searchBound(nums: List[int], left: int, right: int, target: int, mode: str) -> List[int]:
            if right - left <= 1:
                return left if len(nums) > 0 and nums[left] == target else -1
            mid = (left + right) // 2
            if nums[mid] == target:
                if mode == "r":
                    if mid >= right-1 or nums[mid+1] > target:
                        return mid
                    else:
                        return searchBound(nums, mid, right, target, mode)
                else:
                    if mid == left or nums[mid-1] < target:
                        return mid
                    else:
                        return searchBound(nums, left, mid, target, mode)
            if target < nums[mid]:
                return searchBound(nums, left, mid, target, mode)
            else:
                return searchBound(nums, mid, right, target, mode)
        
        lb = searchBound(nums, 0, len(nums), target, 'l')
        ub = searchBound(nums, 0, len(nums), target, 'r')
        return [lb, ub]
```



#### [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

**Strategy**

check row/column/sub-block validity separately

**Python3**

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        def checkSubBox(board, x, y):
            elements = []
            for i in range(3):
                for j in range(3):
                    char = board[3*x+i][3*y+j]
                    if char != '.':
                        if char in elements:
                            return False
                        else:
                            elements.append(char)
            return True
        
        
        for i in range(9):
            elements1 = []
            elements2 = []
            for j in range(9):
                char1, char2 = board[i][j], board[j][i]
                if char1 != '.':
                    if char1 in elements1:
                        return False
                    else:
                        elements1.append(char1)
                if char2 != ".":
                    if char2 in elements2:
                        return False
                    else:
                        elements2.append(char2)
        for i in range(3):
            for j in range(3):
                check = checkSubBox(board, i, j)
                if not check:
                    return False
        
        return True
        
        
```



#### [Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

**Strategy**

*HaspMap + Sliding Window*

Start from different location for window sliding

**Python3**

```python
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        def checkDiff(diff):
            for i in diff.values():
                if i:
                    return False
            return True

        wordlen = len(words[0])
        count = len(words)
        if len(s) < wordlen * count:
            return []

        # target hashmap
        target_dict = {}
        for word in words:
            if word in target_dict.keys():
                target_dict[word] += 1
            else:
                target_dict[word] = 1
        
        results = []
        # each possible window
        for begin in range(wordlen):
            diff_dict = target_dict.copy()
            # sliding window for word
            for i in range(begin, len(s), wordlen):
                if i + wordlen <= len(s):
                    word = s[i:i + wordlen]
                else:
                    break
                prev = s[i - wordlen*count:i - wordlen*(count-1)] if i - wordlen*count >= 0 else None
                if word in diff_dict.keys():
                    diff_dict[word] -= 1
                    if prev in diff_dict.keys():
                        diff_dict[prev] += 1
                    if checkDiff(diff_dict):
                        results.append(i - wordlen * (count - 1))
                else:
                    if prev in diff_dict.keys():
                        diff_dict[prev] += 1
        return results
```



#### [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

**Strategy**

*BackTrack*

Improvement: 

- pruning
- use list to store status of each row/column/block

**Python3**

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        def checkValidity(board, i, j, char):
            for x in range(9):
                if board[i][x] == char or board[x][j] == char:
                    return False
            r, c = i // 3, j // 3
            for x in range(3):
                for y in range(3):
                    if board[3 * r + x][3 * c + y] == char:
                        return False
            return True

        def solve(board):
            for i in range(9):
                for j in range(9):
                    if board[i][j] == '.':
                        for k in range(1, 10):
                            if checkValidity(board, i, j, str(k)):
                                board[i][j] = str(k)
                                if solve(board):
                                    return True
                                else:
                                    board[i][j] = "."
                        return False
            return True
        solve(board)
                           
        
```



#### [Count and Say](https://leetcode.com/problems/count-and-say/)

**Strategy**

*Recursive*

**Python3**

```python
class Solution:
    def countAndSay(self, n: int) -> str:
        if n == 1:
            return "1"
        s = self.countAndSay(n-1)
        result = ""
        start = end = 0
        while end < len(s):
            while end < len(s) and s[start] == s[end]:
                end += 1
            result += str(end-start) + s[start]
            start = end
        return result
```



#### [Combination Sum](https://leetcode.com/problems/combination-sum/)

**Strategy**

*Recursive* + *Pruning*

Remove the used element from the search range in specific recursive layer

**Python3**

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def combSum(target: int, search: List[int]) -> List[List[int]]:
          	# invalid
            if target <= 0:
                return []
            results = []
            search_range = search.copy()
            for e in search:
                # impossible to meet summation requirement
                if e > target:
                    continue 
                # Exact result
                if e == target:
                    results.append([e])
                # search in next layer
                else:
                    results.extend([sorted(arr) + [e] for arr in combSum(target - e, search_range)])
                # remove the used element for searching branch in this layer
                search_range.remove(e)
            return results

        return combSum(target, candidates)
                        
            
        
```



#### [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

**Strategy**

*Recursive* + *Pruning*

Handling duplicate elements: (analyze specific layer)

- remove ONE element when initialize searching branch from it
- allow duplicate element exists in SUB-LAYER
- NOT allow duplicate elements exists in SAME layer

**Python3**

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def combSum(target: int, search: List[int]) -> List[List[int]]:
          	# invalid
            if target <= 0:
                return []
            results = []
            search_range = search.copy()
            for e in set(search):
                # impossible to meet summation requirement
                search_range.remove(e)
                if e > target:
                    continue 
                # Exact result
                if e == target:
                    results.append([e])
                # search in next layer
                else:
                    results.extend([sorted(arr) + [e] for arr in combSum(target - e, search_range)])
                while e in search_range:
                    search_range.remove(e)
            return results
        candidates.sort()
        return combSum(target, candidates)
```



#### [Multiply Strings](https://leetcode.com/problems/multiply-strings/)

**Strategy**

decompose multiply steps

**Python3**

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        result = 0
        i = len(num1) - 1
        while i >= 0:
            j = len(num2)-1
            addition = 0
            partial = 0
            while j >= 0:
                product = int(num1[i]) * int(num2[j]) + addition
                partial += (product % 10) * (10 ** (len(num2)-1-j)) 
                addition = product // 10
                j -= 1
            partial += addition * (10 ** (len(num2)))
            result += partial * (10 ** (len(num1)-1-i))
            i -= 1
        return str(result)
        

```



#### [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

**Strategy**

Maintain the array with specific property which is beneficial for problem solving

Target property: array $A$, $\forall A[i] =x \in [1, N], 0 \le i < N$,  $A[x-1] = x$​

**Python3**

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
      	# maintain each location
        for i in range(len(nums)):
          	# swap location until the property is set
            while 1 <= nums[i] <= len(nums) and nums[i] != nums[nums[i] - 1]:
                tmp = nums[i]
                nums[i] = nums[tmp-1]
                nums[tmp-1] = tmp
        # search for first-missing value
        for i in range(len(nums)):
            if nums[i] != i+1:
                return i+1
        return len(nums)+1

```



#### [ Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

**Strategy**

1. Determine left plank, and increase partial trapped volume step-wise
2. If exists right plank (which is higher than left plank), add trapped volume, then set this right plank as new left plank
3. If doesn't exist right blank, **reverse** the height array starting from **latest** left plank and calculate trapped water in this subrange recursively.

**Python3**

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        # calculate trapped water
        def subTrap(height):
            if len(height) <= 1:
                return 0
            i = total = volume = 0
            # determine first left plank
            while not height[i]:
                i += 1
            init, left = i, height[i]
            for k in range(i, len(height)):
                # update potential volume
                if height[k] < left:
                    volume += left - height[k]
                # find valid right plank
                else:
                    total += volume     # update trapped volume
                    init = k    # update latest left plank location
                    left = height[k]    # update left plank
                    volume = 0     # reset potential volume
            # unclosed subrange
            if volume:
                # calculate trapped water in reversed subrange
                total += subTrap(height[-1:init - 1:-1]) if init > 0 else subTrap(height[::-1])
            return total
        
        return subTrap(height) 
        
```

#### [Maximum Length of Pair Chain](https://leetcode.com/problems/maximum-length-of-pair-chain/)

**Strategy**

Order not matter!

**Python3**

```python
class Solution:
    def findLongestChain(self, pairs: List[List[int]]) -> int:
        # greedy 
        ans, prev = 0, -inf
        pairs.sort(key = lambda x: x[1])
        for l, r in pairs:
            if l > prev:
                ans += 1
                prev = r
        return ans
```



