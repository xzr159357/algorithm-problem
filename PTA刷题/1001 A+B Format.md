# 题目

![1001](https://github.com/xzr159357/algorithm-problem/blob/main/PTA刷题/img-floder/1001.png)

# 代码

```C++
#include <iostream>
using namespace std;
#include <stdio.h>
#include <string>
#include <sstream>
#include <algorithm>

int main()
{
    int a, b;
    while (cin >> a >> b)
    {
        int sum = a + b;
        string str;
        stringstream os;
        os << sum;
        os >> str;
        
        int n = str.size();
        if (n <= 3)
        {
            cout << sum << endl;
            continue;
        }
        int count = 0;
        string s;
        for (int i = n - 1; i >= 0; i--)
        {
            if (count == 3 && str[i] != '-')
            {
                s.push_back(',');
                count = 0;
                i++;
            }
            else
            {
                s.push_back(str[i]);
                count++;
            }
        }
        reverse(s.begin(), s.end());
        cout << s << endl;
    }

    return 0;
}
```

