---
title: 
aliases: 
tags: 
date created: 2023-10-30 15:09
---
https://leetcode.com/contest/biweekly-contest-116

![](assets/images/Pasted%20image%2020231030151021.png)

풀이시간은 1시간 30분
총 네문제 중에 2문제를 풀었고, 그중 2번 문제는 첫 시도에서 실패했다.

![](assets/images/Pasted%20image%2020231030192443.png)

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