---
title: 
aliases: 
tags: 
date created: 2023-10-30 15:09
---
https://leetcode.com/contest/biweekly-contest-116

![](/assets/images/Pasted%20image%2020231030151021.png)

풀이시간은 1시간 30분
총 네문제 중에 2문제를 풀었고, 그중 2번 문제는 첫 시도에서 실패했다.

![](/assets/images/Pasted%20image%2020231030192443.png)

## 2913. Subarrays Distinct Element Sum of Squares I

주어진 nums의 모든 부분집합에서 고유한 숫자 (distinct value)의 개수를 구한 뒤  그 값의 제곱을 모두 더해서 반환하는 문제이다.

length의 제한이 100밖에 안되고 숫자의 최대치도 100 이므로
O(n^2) 으로도 상관없다고 생각하고 이중 for문을 이용해 풀었다.

```python
class Solution:
    def sumCounts(self, nums: List[int]) -> int:
        res = 0
        for _len in range(1, len(nums) + 1):
            for left in range(len(nums) - _len + 1):
                res += len(Counter(nums[left: left + _len])) ** 2
        
        return res
		
```

큰 무리없이 풀었다.

## 2914. Minimum Number of Changes to Make Binary String Beautiful

```python
class Solution:
    def minChanges(self, s: str) -> int:
        answer = 0
        
        def two_subarray(sub: str):
            nonlocal answer
            
            # 어차피 0 또는 1로 맞추기 위해 한번 tweak 했음
            # 나중에 11,00 인 결과가 나왔다고 해도 00,00 인 결과를 위해 값을 바꾸는것과 횟수는 차이 없다.
            if len(sub) == 2:
                if sub[0] != sub[1]:
                    answer += 1
                return
            
            # 홀수개의 구간으로 나뉘어지면
            mid = len(sub) // 2
            if  mid % 2 == 1:
                mid -= 1              

            two_subarray(sub[:mid])
            two_subarray(sub[mid:])
            
        two_subarray(s)
        
        return answer
        
```

beautiful binary string을 만들기 위해 필요한 비트전환 횟수(bitwise)를 구하는 문제이다.
백준의 타일문제와 비슷하다고 느꼈다.

top-down 으로 string을 분리해가면서 beautiful하게 string을 만들도록 구현했다.
string의 길이가 6일 경우 2, 4 혹은 4,2로 나눠졌을때 각각의 string이 beautiful하도록 신경을 썼다.

괜찮다고 생각했던 점은 001100 을 만들기 위해 필요한 횟수와 111100 을 만들기 위한 횟수가 동일하다는걸 체크했다는 점이다.
011000 의경우 명시적으로 bit 전환을 할경우 001100 으로 string이 만들어지면서 2,4로 나누었을 때 길이 4의 string은 beautiful 하지않는 경우가 생각할 수 있지만 사실 001100 이나 111100 이나 비트 전환횟수는 동일하다.

아쉬운점은 O(n)으로 한번 순회하면서 비트전환을 하더라도 같은 결과가 나온다는 점이다.
내가 구현한 재귀적 방식은 메모리와 시간 모두 더 사용해서 비효율적이다.

```python
class Solution:
    def minChanges(self, s: str) -> int:
        n = len(s)
        cnt = 0
        for i in range(0,n,2):
            if s[i] != s[i+1]:
                cnt += 1
        return cnt
```

풀이 출처: https://leetcode.com/problems/minimum-number-of-changes-to-make-binary-string-beautiful/discuss/4221116/Python-O(n)-Optimal-and-1-liner

다음과 같이 더욱 쉽게 풀이할 수 도 있다.

## 2915. Length of the Longest Subsequence That Sums to Target



```python
class Solution:
    def minChanges(self, s: str) -> int:
        answer = 0
        
        def two_subarray(sub: str):
            nonlocal answer
            
            # 어차피 0 또는 1로 맞추기 위해 한번 tweak 했음
            # 나중에 11,00 인 결과가 나왔다고 해도 00,00 인 결과를 위해 값을 바꾸는것과 횟수는 차이 없다.
            if len(sub) == 2:
                if sub[0] != sub[1]:
                    answer += 1
                return
            
            # 홀수개의 구간으로 나뉘어지면
            mid = len(sub) // 2
            if  mid % 2 == 1:
                mid -= 1              

            two_subarray(sub[:mid])
            two_subarray(sub[mid:])
            
        two_subarray(s)
        
        return answer

```

합이 target인 부분수열중에서 가장 긴 수열을 구하는 문제이다.

처음엔 backtracking으로 시도했는데 보란듯이 시간초과가 떴다.
주어진 nums의 길이가 1000이나 되기 때문에 매번 합을 구해가면서 진행하는건 당연히 실패하게 된다. (최악의 경우 2^1000 의 경우를 확인해야 한다.)

두번째 시도는 binary search를 시도했는데, 값의 범위가 정렬되어 있지 않았다.
왜냐하면 내가 구한 부분수열의 합이 길이에 비례해서 증가하지 않는다.
k 길이의 부분수열의 합을 구했을 때 target이 만들어진다고 해서 k-1의 길이의 부분수열의 합이 target과 같을 수 있다는 보장이 없기 때문이다. (T - F 구간이 만들어지지 않음)

현재 확인한 풀이 방법은 DP을 통해 풀수 있다.

```python
class Solution:
    def lengthOfLongestSubsequence(self, nums: List[int], target: int) -> int:
        dp = [-math.inf] * (target + 1)
        dp[0] = 0
        for num in nums:
            for i in range(target, num - 1, -1):
                dp[i] = max(dp[i], dp[i - num] + 1)
        return dp[target] if dp[target] != -math.inf else -1
```

풀이 출처: https://leetcode.com/problems/length-of-the-longest-subsequence-that-sums-to-target/discuss/4219300/Simple-Iterative-DP-(1-D)-%2B-Explanation-oror-Python-oror-6-lines-oror-Beats-100

풀이를 보면 백준에서 풀었던 동전 분배 내지는 동전 2 문제와 매우 비슷하다.
왜 풀지 못했을까.. 숙연해졌다.
