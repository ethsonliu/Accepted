<https://leetcode-cn.com/problems/string-to-integer-atoi/>

## 第一次

```c++
class Solution {
public:
    int myAtoi(string s) {
        int len = s.size();
        int idx = 0;
        int sign = 1;

        for (; idx < len; ++idx) {
            if (s[idx] == ' ')
                continue;
            else if (s[idx] >= '0' && s[idx] <= '9')
                break;
            else if (s[idx] == '-') {
                sign = -1;
                idx++;
                break;
            } else if (s[idx] == '+') {
                sign = 1;
                idx++;
            }

        }
    }
};
```

