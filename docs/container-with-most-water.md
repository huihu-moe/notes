# [11] Container With Most Water

解决思路：替换短板问题中的短板，使得高度尽可能高，宽度尽可能宽。

```java
/*
 * @lc app=leetcode id=11 lang=java
 *
 * [11] Container With Most Water
 *
 * https://leetcode.com/problems/container-with-most-water/description/
 *
 * algorithms
 * Medium (54.10%)
 * Likes:    18461
 * Dislikes: 1003
 * Total Accepted:    1.6M
 * Total Submissions: 3M
 * Testcase Example:  '[1,8,6,2,5,4,8,3,7]'
 *
 * You are given an integer array height of length n. There are n vertical
 * lines drawn such that the two endpoints of the i^th line are (i, 0) and (i,
 * height[i]).
 * 
 * Find two lines that together with the x-axis form a container, such that the
 * container contains the most water.
 * 
 * Return the maximum amount of water a container can store.
 * 
 * Notice that you may not slant the container.
 * 
 * 
 * Example 1:
 * 
 * 
 * Input: height = [1,8,6,2,5,4,8,3,7]
 * Output: 49
 * Explanation: The above vertical lines are represented by array
 * [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the
 * container can contain is 49.
 * 
 * 
 * Example 2:
 * 
 * 
 * Input: height = [1,1]
 * Output: 1
 * 
 * 
 * 
 * Constraints:
 * 
 * 
 * n == height.length
 * 2 <= n <= 10^5
 * 0 <= height[i] <= 10^4
 * 
 * 
 */

// @lc code=start
class Solution {
    public int maxArea(int[] height) {
        if (height == null || height.length == 0) {
            return 0;
        }
        int n = height.length;
        int i = 0, j = n - 1;        
        int res = Math.min(height[i], height[j]) * (j - i);
        while (i < j) {
            if (height[i] > height[j]) {
                int tmp = height[j];
                j--;           
                while (i < j && height[j] < tmp) {
                    j--;
                }
            } else {
                int tmp = height[i];
                i++;
                while (i < j && height[i] < tmp) {
                    i++;
                }
            }

            if (i == j) {
                return res;
            }            
            res = Math.max(res, Math.min(height[i], height[j]) * (j - i));
        }            
        return res;
    }
}
// @lc code=end

```