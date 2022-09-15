# [239] Sliding Window Maximum

```java
/*
 * @lc app=leetcode id=239 lang=java
 *
 * [239] Sliding Window Maximum
 */

// @lc code=start
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length < k ) {
            return new int[0];            
        }    
        int[] res = new int[nums.length - k + 1];

        int left = 0, right = 0;
        Deque<Integer> queue = new ArrayDeque<>();
        while (right < k - 1) {
            if (queue.isEmpty()) {
                queue.offer(nums[right++]);
            } else {
                while (!queue.isEmpty() && queue.peekLast() < nums[right]) {
                    queue.pollLast();
                }
                queue.offer(nums[right++]);
            }                      
        }

        while (right < nums.length) {
            while (!queue.isEmpty() && queue.peekLast() < nums[right]) {
                queue.pollLast();
            }
            queue.offer(nums[right]);
            res[left] = queue.peek();
            if (queue.peek() == nums[left]) {
                queue.poll();
            }
            left++;
            right++;                    
        }
        return res;
    }
}

// @lc code=end

```