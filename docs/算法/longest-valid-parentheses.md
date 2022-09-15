# [32] Longest Valid Parentheses

动态规划求解最长子序列（但是是有效括号序列）

```java
import java.util.Deque;

/*
 * @lc app=leetcode id=32 lang=java
 *
 * [32] Longest Valid Parentheses
 *
 * https://leetcode.com/problems/longest-valid-parentheses/description/
 *
 * algorithms
 * Hard (32.43%)
 * Likes:    9136
 * Dislikes: 293
 * Total Accepted:    539.9K
 * Total Submissions: 1.7M
 * Testcase Example:  '"(()"'
 *
 * Given a string containing just the characters '(' and ')', find the length
 * of the longest valid (well-formed) parentheses substring.
 * 
 * 
 * Example 1:
 * 
 * 
 * Input: s = "(()"
 * Output: 2
 * Explanation: The longest valid parentheses substring is "()".
 * 
 * 
 * Example 2:
 * 
 * 
 * Input: s = ")()())"
 * Output: 4
 * Explanation: The longest valid parentheses substring is "()()".
 * 
 * 
 * Example 3:
 * 
 * 
 * Input: s = ""
 * Output: 0
 * 
 * 
 * 
 * Constraints:
 * 
 * 
 * 0 <= s.length <= 3 * 10^4
 * s[i] is '(', or ')'.
 * 
 * 
 */

// @lc code=start
class Solution {
    public int longestValidParentheses(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        if (s == null || s.length() < 2) {
            return 0;
        }
        
        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 0;
        for (int i = 2; i < n + 1; ++i) {
            char ch = s.charAt(i - 1);
            if (ch == '(') {
                dp[i] = 0;
            } else {
                char pre = s.charAt(i - 2);
                if (pre == '(') {
                    dp[i] = dp[i - 2] + 2;
                } else {
                    if (dp[i - 1] == 0) {
                        dp[i] = 0;
                    } else {
                        int j = i - dp[i - 1] - 1;
                        if (j == 0 || s.charAt(j - 1) == ')') {
                            dp[i] = 0;
                        } else {
                            dp[i] = dp[i - 1] + 2 + dp[j - 1];
                        }
                    }
                }
            }
        }
        int max = 0;
        for (int i = 0; i < n + 1; ++i) {
            if (max < dp[i]) {
                max = dp[i];
            }
        }
        return max;
    }
}
// @lc code=end

```

```java

// LeetCode 提供的一种解法
public class Solution {

    public int longestValidParentheses(String s) {
        int maxans = 0;
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.empty()) {
                    stack.push(i);
                } else {
                    maxans = Math.max(maxans, i - stack.peek());
                }
            }
        }
        return maxans;
    }
}
```