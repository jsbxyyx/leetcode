给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：
```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```
示例 2：
```
输入: "cbbd"
输出: "bb"
```

解题：

首先我们要了解回文是什么意思，举个例子相信大家就知道了
```
上海自来水来自海上
大波美人鱼人美波大
```

发现没有，字符串是对称的，我们可以以某一个字符为中心，分别比较左右字符是否相等，找到最大长度

```
class Solution {
    public String longestPalindrome(String s) {
        if (s == null) {
            throw new IllegalArgumentException("illegal argument");
        }

        int length = s.length();
        if (length == 0 || length == 1) {
            return s;
        }
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            int len1 = centerExpand(s, i, i);
            int len2 = centerExpand(s, i, i + 1);
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }

    private int centerExpand(String s, int left, int right) {
        int L = left, R = right, LEN = s.length();
        while (L >= 0 && R < LEN && s.charAt(L) == s.charAt(R)) {
            L--;
            R++;
        }
        return R - L - 1;
    }
}
```
