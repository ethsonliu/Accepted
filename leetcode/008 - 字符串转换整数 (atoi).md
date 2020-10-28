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
                break;
            } else {
                return 0;
            }
        }

        double ret = 0;
        double limit = (double)INT_MAX + 1;
        for (; idx < len; ++idx) {
            if (s[idx] >= '0' && s[idx] <= '9') {
                ret = ret * 10 + (s[idx] - '0');
                if (ret > limit || (ret == limit && sign == 1)) { // 溢出
                    if (sign == 1) {
                        return INT_MAX;
                    } else {
                        return INT_MIN;
                    }
                }
            } else {
                break;
            }
        }

        if (sign == -1)
            return -1 * ret;
            
        return ret;
    }
};
```

执行用时：8 ms；内存消耗：7.1 MB
