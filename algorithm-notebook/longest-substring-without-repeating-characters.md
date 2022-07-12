# 3. Longest Substring Without Repeating Characters

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        HashMap<Character, Integer> map = new HashMap<>();
        int n = s.length(), left = 0, right = 0;
        int res = 1;
        while (right < n) {
            char ch = s.charAt(right);
            if (map.containsKey(ch)) {                
                left = Math.max(left, map.get(ch) + 1);
            }
            res = Math.max(res, right - left + 1);
            map.put(ch, right);
            right++;
        }
        return res;
    }
}
```