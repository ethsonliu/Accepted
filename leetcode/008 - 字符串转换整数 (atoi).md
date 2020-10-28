<https://leetcode-cn.com/problems/string-to-integer-atoi/>

## 第一次

```c++
class Solution {
public:
    int myAtoi(string s) {
        int len = s.size(); 

        // 找到第一个有效数字
        int idx = 0;
        int sign = 1;
        for (; idx < len; ++idx) {
            if (s[idx] == ' ' || s[idx] == '0')
                continue;
            else if (s[idx] > '0' && s[idx] <= '9')
                break;
            else if (s[idx] == '-') {
                sign = -1;
                idx++;
                continue;
            } else if (s[idx] == '+') {
                sign = 1;
                idx++;
                continue;
            } else {
                return 0;
            }
        }

        int ret = 0;
        double limit = (double)INT_MAX + 1;
        int cnt = -1;
        for (; idx < len; ++idx) {
            cnt++;
            if (s[idx] >= '0' && s[idx] <= '9') {
                double tmp = ret * pow(10, cnt) + (s[idx] - '0');
                if (tmp > limit || (tmp == limit && sign == 1)) { // 溢出
                    if (sign == 1) {
                        return INT_MAX;
                    } else {
                        return INT_MIN;
                    }
                }
                ret = tmp;
            } else {
                break;
            }
        }

        return sign * ret;
    }
};
```

