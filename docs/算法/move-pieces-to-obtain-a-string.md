# 2337. Move Pieces to Obtain a String

LeetCode 周赛的一道题，想了半天才想出来。又是糟糕的一次周赛，不知道为什么答题没有提交成功。

[2337. Move Pieces to Obtain a String](https://leetcode.com/problems/move-pieces-to-obtain-a-string/)

```java
class Solution {
    public boolean canChange(String start, String target) {
        int n = start.length(), m = target.length();
        if (n != m) {
            return false;
        }
        int i = 0, j = 0;
        while (i < n && j < m) {
            while (i < n && start.charAt(i) == '_') {
                ++i;
            }

            while (j < m && target.charAt(j) == '_') {
                ++j;
            }

            if (i == n || j == m) {
                return i == n && j == m;
            }

            if (start.charAt(i) != target.charAt(j)) {
                return false;
            }

            if (start.charAt(i) == 'L' && i < j) {
                return false;
            } else if (start.charAt(i) == 'R' && i > j) {
                return false;
            } else {
                ++i;
                ++j;
            }
        }

        while (i < n && start.charAt(i) == '_') {
            ++i;
        }

        while (j < m && target.charAt(j) == '_') {
            ++j;
        }

        return i == n && j == m;
    }
}
```