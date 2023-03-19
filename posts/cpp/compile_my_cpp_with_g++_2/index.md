# 不靠 IDE 自己編譯 C++ (2) - 編譯初心者


繼 [上一篇](https://eyzim.github.io/posts/compile_my_cpp_with_g++/) 安裝完成 MinGW 後，我們先來試試看印出字串 **Your Majesty, my Lord.**

```cpp
// file name: myLord.cpp

#include <iostream>

using namespace std;

int main()
{
   cout << "Your Majesty, my Lord." << endl;

   return 0;
}
```

## 1

接著，打開 cmd，進入剛才存放 myLord.cpp 的資料夾

```bash
g++ myLord.cpp
```

可以看到新增了一個 `a.exe` 檔案

![](/images/compile_my_cpp_with_g++_2/a.exe_myLord.cpp.png)

執行這個 exe 檔案，輸入檔案名稱：

```bash
a
```

馬上會印出

```
Your Majesty, my Lord.
```

## 2

若是我們希望產生的執行檔名稱能夠變成喜歡的名稱，在前面加上 **-o** 和命名即可。

```bash
g++ -o myLord.exe myLord.cpp
```

![](/images/compile_my_cpp_with_g++_2/myLord.exe_myLord.cpp.png)

```bash
myLord
```

也會印出

```
Your Majesty, my Lord.
```

## 3

我們有大一點的 Project 要執行時，會有很多個檔案需要編譯，通常我們會這麼做：

```bash
g++ -o servants.exe gardeners.cpp laundress.cpp butler.cpp housemaids.cpp
```

怕其中一個檔案有問題，通常我們會一個一個檔案進行組譯，之後再將它們連結起來。

```bash
# 一一組譯
g++ -c gardeners.cpp
g++ -c laundress.cpp
g++ -c butler.cpp
g++ -c housemaids.cpp

# 全部的 .o 檔要 link 起來
g++ -o servants.exe gardeners.o laundress.o butler.o housemaids.o
```

## Reference

1. https://www3.ntu.edu.sg/home/ehchua/programming/cpp/gcc_make.html

