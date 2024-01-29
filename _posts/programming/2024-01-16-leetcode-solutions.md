---
title:  "Leetcode Coding Solution Notes"
date:   2024-01-16 -0500
author: Pei Tian
categories: programming
tags: leetcode python programming
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


## Solutions

#### [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

##### Strategy



##### Python3

```python
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

##### Strategy

##### Python3

```python
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



#### [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

##### Strategy



##### Python3

```python
def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        k = (len(nums1) + len(nums2)) // 2 + 1
        p1 = p2 = 0
        while (p1 < len(nums1) or p2 < len(nums2)) and k > 1:
            m = k // 2
            if p1 >= len(nums1):
                p2 += m
                k -= m
            elif p2 >= len(nums2):
                p1 += m
                k -= m
            else:
                i1 = min(len(nums1), p1 + m)
                i2 = min(len(nums2), p2 + m)
                if nums1[i1-1] > nums2[i2-1]:
                    k = k - i2 + p2
                    p2 = i2
                else:
                    k = k - i1 + p1
                    p1 = i1

        if p1 >= len(nums1):
            next_val = nums2[p2]
        elif p2 >= len(nums2):
            next_val = nums1[p1]
        else:
            next_val = min(nums1[p1], nums2[p2])
        
        if (len(nums1) + len(nums2)) % 2:
            return float(next_val)
        else:
            if p1 == 0:
                return (next_val + nums2[p2 - 1]) / 2
            elif p2 == 0:
                return (next_val + nums1[p1 - 1]) / 2
            else:
                return (next_val + max(nums1[p1-1], nums2[p2-1])) / 2
```



#### [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s
        T = 2 * numRows - 2
        n = len(s) // T + 1
        result = ""
        for i in range(numRows):
            for j in range(n):
                if i == 0 or i == (numRows - 1):
                    result += s[j * T + i] if j * T + i < len(s) else ""
                else:
                    result += s[j * T + i] if j * T + i < len(s) else ""
                    result += s[(j+1) * T - i] if (j+1) * T - i < len(s) else ""
        return result
```

#### [Reverse Integer](https://leetcode.com/problems/reverse-integer/)

##### Strategy



##### Python3

```python
def reverse(self, x: int) -> int:
        if x == 0 or x > 2 ** 31 - 1 or x < -2 ** 31:
            return 0
        positive = True if x > 0 else False
        x = -x if positive else x
        digits = []
        while x != 0:
            resid = x % 10 - 10
            if resid == -10:
                resid = 0
                x = x // 10
            else:
                x = x // 10 + 1
            digits.append(resid)
        result = 0
        
        for i in range(len(digits)):
            result += digits[i] * 10 ** (len(digits) - i - 1)
        result = -result if positive else result
        return 0 if result > 2 ** 31 - 1 or result < -2 ** 31 else result
```





#### [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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





#### [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
def generateParenthesis(self, n: int) -> List[str]:
        # if n == 1:
        #     return ["()"]
        # results = []
        # for prev in self.generateParenthesis(n-1):
        #     for i in range(len(prev)):
        #         if prev[i] == ')':
        #             s = prev[:i] + '()' + prev[i:]
        #             if s not in results:
        #                 results.append(s)
        #     results.append(prev + '()')
        # return results

        def dfs(l: int, r: int, s):
            if len(s) == n * 2:
                res.append(s)
                return
        
            if l < n:
                dfs(l+1, r, s + '(')
            
            if r < l:
                dfs(l, r+1, s + ')')
            
        res = []
        dfs(1, 0, '(')
        return res
```





#### [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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

##### Strategy



##### Python3

```python
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
            l = stack[-1]
            stack.pop()
            longest = max(r-l-1, longest)
            r = l
        longest = max(longest, r)
        
        return longest
```







#### 

##### Strategy



##### Python3

```python

```





#### 

##### Strategy



##### Python3

```python

```

#### 

##### Strategy



##### Python3

```python

```

#### 

##### Strategy



##### Python3

```python

```

#### 

##### Strategy



##### Python3

```python

```

#### 

##### Strategy



##### Python3

```python

```

