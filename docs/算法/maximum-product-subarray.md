# 152. Maximum Product Subarray

[152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

动态规划解法

```java
class Solution {
    public int maxProduct(int[] nums) {        
        int n = nums.length;
        
        int big = nums[0], small = nums[0], res = nums[0];        
        
        for (int i = 1; i < n; ++i) {
            if (nums[i] >= 0) {
                big = Math.max(big * nums[i], nums[i]);
                small = Math.min(small * nums[i], nums[i]);                
            } else {
                int cur1 = Math.max(small * nums[i], nums[i]);
                int cur2 = Math.min(big * nums[i], nums[i]);
                big = cur1;
                small = cur2;
            }
            
            res = Math.max(res, big);
        }
        return res;
    }
}
```

lee215 大神给出的解法，<https://leetcode.com/problems/maximum-product-subarray/discuss/183483/JavaC%2B%2BPython-it-can-be-more-simple>。

I am vegetable :(.

```java
class Solution {
    public int maxProduct(int[] A) {
        int n = A.length, res = A[0], l = 0, r = 0;
        for (int i = 0; i < n; i++) {
            l =  (l == 0 ? 1 : l) * A[i];
            r =  (r == 0 ? 1 : r) * A[n - 1 - i];
            res = Math.max(res, Math.max(l, r));
        }
        return res;
    }        
}

