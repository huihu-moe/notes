# 76. Minimum Window Substring

[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

有点傻得的做法，滑动窗口 + HashMap, 每次窗口变化，都比较 sMap 和　tMap。:(

```java
class Solution {
    public String minWindow(String s, String t) {
        if (s.length() < t.length()) {
            return "";
        }
        HashMap<Character, Integer> sMap = new HashMap<>();
        HashMap<Character, Integer> tMap = new HashMap<>();
        
        for (int i = 0; i < t.length(); ++i) {
            char ch = t.charAt(i);
            tMap.put(ch, tMap.getOrDefault(ch, 0) + 1);
        }
            
        int n = s.length(), left = 0, right = 0, res = Integer.MAX_VALUE, i= 0, j = 0;
        while (right <= n) {
            if (isValid(sMap, tMap)) {                
                if (right - left + 1 < res) {
                    i = left;
                    j = right;
                    res = right - left + 1;
                }
                char ch = s.charAt(left);
                if (tMap.containsKey(ch)) {
                    sMap.put(ch, sMap.get(ch) - 1);
                }
                left++;
            } else {
                if (right == n) {
                    break;
                }
                char ch = s.charAt(right);
                if (tMap.containsKey(ch)) {
                    sMap.put(ch, sMap.getOrDefault(ch, 0) + 1);
                }
                right++;
            }
        }
        
        return res == Integer.MAX_VALUE ? "" : s.substring(i, j);
    }
    
    public boolean isValid(HashMap<Character, Integer> sMap, HashMap<Character, Integer> tMap) {
        for (char ch : tMap.keySet()) {
            if (!sMap.containsKey(ch) || sMap.get(ch) < tMap.get(ch)) {
                return false;
            }
        }
        return true;
    }
}
```

方法二


```java

```