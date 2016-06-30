# [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:
Given "25525511135",

return ["255.255.11.135", "255.255.111.35"]. (Order does not matter)

## Solution 1. Backtrace

Time/space: O(3 ^ 4)

```java
public class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> ips = new ArrayList<String>();
        dfs(ips, new String[4], 0, 0, s);
        return ips;
    }
    
    private void dfs(List<String> ips, String[] ip, int segment, int index, String s) {
        if ((4 - segment) * 3 < s.length() - index || 4 - segment > s.length() - index) return;
        if (segment == 4 && index == s.length()) {
            ips.add(join(ip, "."));
            return;
        }
        for (int d = 1; d < 4 && (index + d <= s.length()); d++)  {
            if (isValid(s.substring(index, index + d))) {
                ip[segment] = s.substring(index, index + d);
                dfs(ips, ip, segment + 1, index + d, s);
            }
        }
    }
    
    private boolean isValid(String s) {
        int n = Integer.parseInt(s);
        return n >= 0 && n <= 255 && ("" + n).length() == s.length();
    }
    
    private String join(String[] tokens, String s) {
        if (tokens.length == 0) return "";
        StringBuilder sb = new StringBuilder(tokens[0]);
        for (int i = 1; i < tokens.length; i++) sb.append(s + tokens[i]);
        return sb.toString();
    }
}
```

## Solution 2. Explicitly segment string

```java
public class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> ips = new ArrayList<String>();
        for (int i = 1; i < 4; i++)
        for (int j = 1; j < 4; j++)
        for (int k = 1; k < 4; k++)
        for (int l = 1; l < 4; l++) {
            if (i + j + k + l == s.length()
                && isValid(s.substring(0, i))
                && isValid(s.substring(i, i + j))
                && isValid(s.substring(i + j, i + j + k))
                && isValid(s.substring(i + j + k))) {
                    ips.add(s.substring(0, i)
                            + "." + s.substring(i, i + j)
                            + "." + s.substring(i + j, i + j + k)
                            + "." + s.substring(i + j + k));
            }
        }
        return ips;
    }
    
    private boolean isValid(String s) {
        int n = Integer.parseInt(s);
        return n >= 0 && n <= 255 && ("" + n).length() == s.length();
    }
}
```
