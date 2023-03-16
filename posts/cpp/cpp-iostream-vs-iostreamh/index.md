# 【C++】<iostream> vs <iostream.h>


`#include <iostream>` or `#include <iostream.h>`

在第一堂c++課程大概都會教：記得要在最前面打上一個 #，而後面include <iostream> ，方便一些資料進出程式。

但是!!!!

當要使用 `cout` 的時候，又叫你回到前方加上 `using namespace std`...

## `<iostream>`

<iostream> 屬於標準輸入輸出流，歸 C++ 所管，需要使用到命令空間 大致上分成三種寫法： 

1. 

```cpp 
#include <iostream>     // 引入標準 c++ 資源庫

int main()
{
  string hi("我會使用c++標準庫");
  std::cout << hi;     // 每一句我都會記得打這種很難打的格式鳩咪 std::
}
```

2.

```cpp
#include <iostream>    // 引入標準 c++ 資源庫
using namespace std;   // 吃 C++ 就要乖乖使用命名空間
    
int main()
{
  string hi("我會使用c++標準庫");
  cout << hi;

  return 0;
};      
```

3.

```cpp
#include <iostream>    // 引入標準 c++ 資源庫
using std::cout;       // 先宣告看到 cout 一律下跪(x) 幫他加 std::(o)
      
int main()
{
  string hi("我會使用c++標準庫");
  cout << hi;
}
```


## <iostream.h>


`<iostream.h>` 屬於非標準輸入輸出流，.h 結尾的標頭檔，直接繼承 C 語言的標準庫，空間定義之類的什麼都不用加！


```cpp
#include <iostream.h>    // 引入非標準輸入輸出流
      
int main()
{
  string hi("我會使用c++標準庫");
  cout << hi;
}
```
