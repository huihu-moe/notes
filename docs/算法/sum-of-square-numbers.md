# 两数平方和

[633. Sum of Square Numbers](https://leetcode.com/problems/sum-of-square-numbers/description/)

```java
class Solution {
    public boolean judgeSquareSum(int c) {
        if (c < 0) {
        return false;
        }
        int i = 0, j = (int) Math.sqrt(c);
        while (i <= j) {
            if (c-i*i == j*j) {
                return true;
            } else if (c-i*i < j*j) {
                j--;
            } else {
                i++;
            }
        }
        return false;
    }
}
```