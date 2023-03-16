# 【C++】int to string & string to int


# string to int

現在我們要字串 a 的第 i 個字元：`a[i]`，希望能直接進行數字計算，只要將它改為 `a[i]-'0'`，它就是一個數字了！

```cpp
#include <iostream>
#include <string>
#include <typeinfo>

using namespace std;

int main()
{
  string num = "1234";
  
  cout << 
  "Type of num is: " << typeid(num).name() << endl <<
  "Type of num[0] is: " << typeid(num[0]).name() << endl <<
  "Type of num[0]-'0' is: " << typeid(num[0]-'0').name() << endl <<
}
```

輸出為：
    
```
Type of num is: Ss          -> string
Type of num[0] is: c        -> char
Type of num[0]-'0' is: i    -> integer
```

轉換的邏輯約莫是：

`string` ➡ `char` ➡ `int`

`num`    ➡ `num[0]` ➡ `num[0]-'0'`


# int to string

從 `int` 轉回 `string` 就沒有簡單了，下列範例中，我們已經知道 intNum 是整數型別，試試看加上各種修飾會將 `intNum` 轉為什麼 type。

```cpp
#include <iostream>
#include <string>
#include <typeinfo>

using namespace std;

int main()
{
    string num = "1234";
  
    int intNum = num[0]-'0';
  
    cout << 
    "Type of intNum is: " << typeid(intNum).name() << endl <<
    "Type of intNum+'0' is: " << typeid(intNum+'0').name() << endl <<
    "Type of char(intNum) is: " << typeid(char(intNum)).name() << endl <<
    "Type of char(intNum)+'0' is: " << typeid(char(intNum)+'0').name() << endl <<
    "Type of to_string(intNum) is: " << typeid(to_string(intNum)).name() << endl;
}
```
```
Type of intNum is: i
Type of intNum+'0' is: i
Type of char(intNum) is: c
Type of char(intNum)+'0' is: i
Type of to_string(intNum) is: Ss
```

由上面輸出可知，只有直接用 `to_string` 轉型可以將 `int` 換回 `string` type。

但是根據這題的題意，我們希望能一一串回 string 中，下面的實驗可以告訴我們：

```cpp
#include <iostream>
#include <string>
#include <typeinfo>

using namespace std;

int main()
{
    string num = "1234";
  
    num += 1;
    cout << num << endl;

    num += 2+'0';
    cout << num << endl;
    
    num += char(3);
    cout << num << endl;
    
    num += char(4)+'0';
    cout << num << endl;
    
    num += to_string(5);
    cout << num << endl;
}
```

```
1234
12342
12342
123424
1234245
```

在 `int` 或 `char` 後面加上 `+'0'` 即可串上 `string`，當然最保險且最簡單的作法當然是強轉型成 `to_string` 啦。


## Reference
1. https://www.delftstack.com/zh-tw/howto/cpp/how-to-convert-int-to-string-in-cpp/
