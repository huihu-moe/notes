# 16. 3Sum Closest

[16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)

类似于三数之和为零问题，解法都差不多。

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        
        Arrays.sort(nums);
        int res = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length - 2; ++i) {
            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                int sum =  nums[i] + nums[left] + nums[right];
                if (sum == target) {
                    return target;
                }
                if (sum < target) {
                    left++;
                } else {
                    right--;
                }
                
                if (Math.abs(res - target) > Math.abs(sum - target)) {
                    res = sum;
                }
            }
        }
        return res;
    }
}
```