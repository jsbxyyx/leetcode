给你一个有效的 IPv4 地址 address，返回这个 IP 地址的无效化版本。

所谓无效化 IP 地址，其实就是用 "[.]" 代替了每个 "."。

示例 1：
```
输入：address = "1.1.1.1"
输出："1[.]1[.]1[.]1"
```

示例 2：
```
输入：address = "255.100.50.0"
输出："255[.]100[.]50[.]0"
```

提示：

给出的 address 是一个有效的 IPv4 地址

解题：
1. (3 ms | 34.2 MB)
```
class Solution {
    public String defangIPaddr(String address) {
        return address.replaceAll("\\.", "[.]");
    }
}
```

2. (2 ms | 34.1 MB)
```
class Solution {
    public String defangIPaddr(String address) {
        return address.replace(".", "[.]");
    }
}
```

3. (0 ms | 34 MB)
```
class Solution {
    public String defangIPaddr(String address) {
        StringBuilder sb = new StringBuilder(address.length());
        for (int i = 0; i < address.length(); i++) {
            char c = address.charAt(i);
            if (c != '.') {
                sb.append(c);
            } else {
                sb.append("[.]");
            }
        }
        return sb.toString();
    }
}
```
