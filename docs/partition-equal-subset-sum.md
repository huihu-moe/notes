# 416. Partition Equal Subset Sum

[416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

很早以前写的一道简单的动态规划 01 背包问题。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        if (nums == null || nums.length == 0) {
            return false;
        }
        
        int numsSum = 0;
        for (int num : nums) {
            numsSum += num;
        }
        
        if (numsSum % 2 == 1) {
            return false;
        }
        
        numsSum = numsSum / 2;
        
        int[] dp = new int[numsSum + 1];
        
        for (int i = 0; i < nums.length; ++i) {
            for (int j = numsSum; j >= nums[i]; --j) {                
                dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
            }

            // 参考 LeetCode 上的解答，发现可以做些优化，不需要遍历完所有物品再判断
            if (dp[numsSum] == numsSum) {
                return true;
            }
        }
        
        // return dp[numsSum] == numsSum;
        return false;
    }
}
```