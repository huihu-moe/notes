# 2344. Minimum Deletions to Make Array Divisible

Leetcode 周赛题目

[2344. Minimum Deletions to Make Array Divisible](https://leetcode.com/problems/minimum-deletions-to-make-array-divisible/)

菜鸟做法（I am 🥬）

```java
public int minOperations(int[] nums, int[] numsDivide) {
    if (nums == null || nums.length == 0 || numsDivide == null || numsDivide.length == 0) {
        return -1;
    }
    
    Map<Integer, Integer> map = new TreeMap<>();
    for (int num: nums) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }
    int res = 0;
    for (int num: map.keySet()) {
        boolean tag = true;
        for (int numDivide : numsDivide) {
            if (numDivide % num != 0) {
                res += map.get(num);
                tag = false;
                break;
            }
        }  
        if (tag) {
            return res;
        }
    }
    return -1;             
}
```

lee215 大神的方法 [[Java/C++/Python] GCD, O(n + m + log)](https://leetcode.com/problems/minimum-deletions-to-make-array-divisible/discuss/2292651/JavaC%2B%2BPython-GCD-O(n-%2B-m-%2B-log))

关键在于对 numsDivide 数组所有元素的除法，优化成对于 numsDivide 数组所有元素的最大公因子的除法。

```java
public int minOperations(int[] A, int[] numsDivide) {
    int g = numsDivide[0], tmp;
    for (int a : numsDivide) {
        while (a > 0) { // g = gcd(g, a)
            tmp = g % a;
            g = a;
            a = tmp;
        }
    }
    Arrays.sort(A);
    for (int i = 0; i < A.length && A[i] <= g; ++i)
        if (g % A[i] == 0)
            return i;
    return -1;
}
```