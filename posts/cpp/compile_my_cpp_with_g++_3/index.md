# 不靠 IDE 自己編譯 C++ (3) - 翻譯課


[上一篇](https://eyzim.github.io/posts/compile_my_cpp_with_g++_2/) 我們知道只要下了
```bash
g++ -o myLord.exe myLord.cpp
myLord
```
這兩行指令就能印出想印的文字。

---

{{< mermaid >}}
graph LR;
    A[原始檔] -->|myLord.cpp| B(Preprocessor<br>預處理器)
    B -->|myLord.i| C(Compiler<br>編譯器)
    C -->|myLord.s| D(Assembler<br>組譯器)
    D -->|myLord.o| E(Linker<br>連結器)
    E -->|myLord.exe| F[執行檔]
    A --> F
{{< /mermaid >}}


## 編譯系統
每一行 C++ 的文字都會被電腦編譯成低階的<cite>**機器語言**[^1]</cite>，然後再以可以直接執行的格式打包，最後以二進位的格式封裝保存。

一系列工作都是由 GCC 編譯器所完成的，總共又可分為四個階段：預處理器、編譯器、組譯器、連結器，它們構成了**編譯系統** `complication system`。

下為範例檔，歡迎各位跟著動手試試看！


```cpp
// file name: myLord.cpp

#include <iostream>
#define DEBUG
#define SIZE 10

using namespace std;
 
int main() 
{
   #ifdef DEBUG
      cout << "DEBUG MODE is On." << endl;
   #else
      cout << "Your Majesty, my Lord." << endl;
   #endif   

   if(size>5)
   {
       cout << size << endl;
   }

   return 0;
}
```

## Preprocess 預處理

```bash
g++ -E myLord.cpp -o myLord.i
```

負責處理編譯之前的工作，原則上就是字串代換的過程，大致分為三個：

1. 引入

    ```
    #include <iostream>
    ```
    以 **#include** 開頭的明令會告訴預處理器讀取其他 .h 檔案的內容。

2. 處理條件編譯
    ```
    #define DEBUG
    ```
    以 #define、#ifdef、#ifndef、#if、#else、#endif 等等方式設計條件判斷，阻止某些部分的程式碼執行。

{{< admonition type=tip title="#ifndef">}}

通常我們新增一個標頭檔案 `.h` 時，會加上 guard：

```H
#ifndef SAMPLE_H
#define SAMPLE_H

...

#endif  /* SAMPLE_H */
```

但若是原始檔中出現兩個 `SAMPLE_H` 的話，會造成巨集名稱衝突，加上

```h
#pragma once
```
有相同效果，卻不會再產生衝突。

{{< /admonition >}}


3. 將 macro 代換成程式碼片段 (code snippet)
    ```
    #define SIZE 10
    ```
    或是走火入魔一點
    ```
    #define MAX(a, b) ((a) > (b) ? (a) : (b))
    ```

    對了，記得 macro 要把「所有」變數一一括起來，以免發生奇怪的錯誤。

    另外，還有一些是和程式相關的 macro：
    - `__LINE__`：目前在程式中的行數
    - `__FILE__`：檔案名稱
    - `__DATE__`：(前置處理器)執行的日期
    - `__TIME__`：(前置處理器)執行的時間

    ```cpp
    cout << endl << __FILE__;
    cout << endl << __LINE__;
    cout << endl << __DATE__ << " " << __TIME__;
    ```

    ```
    D:\Project\find\main.cpp
    61
    Feb  18 2019 21:58:20
    ```


{{< image 
src="/images/compile_my_cpp_with_g++_3/myLord.i.png"  height="200" 
src_s="/images/compile_my_cpp_with_g++_3/myLord.i.png" 
src_l="/images/compile_my_cpp_with_g++_3/myLord.i.png" 
alt="myLord.i"
caption="`myLord.i` 檔的內容大部分都是環境設定變數和函式庫路徑" 
linked="false">}}


## Compile 編譯

```bash
g++ -S myLord.i
```

編譯器將 `.i` 檔翻譯成 `.s` 檔，`.s` 檔包含完整的低階的組合語言程式，組合語言的優點在於不同高階語言能夠經由不同編譯器得到**相同**的輸出<cite>**組合語言**[^2]</cite>，例如：C 和 Fortran 都會被編譯成組合語言。

這個時候，編譯器也會幫我們檢查語法上的錯誤！

{{< image 
src="/images/compile_my_cpp_with_g++_3/myLord.s.png"  height="200" 
src_s="/images/compile_my_cpp_with_g++_3/myLord.s.png" 
src_l="/images/compile_my_cpp_with_g++_3/myLord.s.png" 
alt="myLord.s"
caption="`myLord.s` 檔中， _main: 後每個句子都代表一條低階機器語言指令" 
linked="false">}}



## Assemble 組譯

```bash
g++ -c myLord.s myLord.o
```

再來，組譯器會把 `myLord.s` 翻譯成機器語言，把指令打包成一個 relocatable object program 格式，並將結果保存在 `myLord.o` 中。

`myLord.o` 是一個二進位文件，無法在文字編輯器中打開。


## Link 連結
```bash
# 只有單一檔案
g++ -o myLord myLord.o

# 有複數檔案
g++ -o myLord myLord.o a.o b.o c.o d.o e.o
```

在 `myLord.cpp` 中，我們使用了 `cout` 函數，而這個函數存放在 `iostream.o` 這個已經預先編譯好的文件中，而這個文件必須合併到 `myLord.o` 中，我們才能順利的印出文字，這就是 Linker 的工作。

`myLord` 執行檔運作時，`iostream.o` 會被 load 到記憶體中，由系統執行。



{{< mermaid >}}
classDiagram
    原始檔 --> 執行檔
    原始檔 --> Preprocessor 預處理器: myLord.cpp
    Preprocessor 預處理器 --> Compiler 編譯器: myLord.i
    Compiler 編譯器 --> Assembler 組譯器: myLord.s
    Assembler 組譯器 --> Linker 連結器 : myLord.o
    Linker 連結器 --> 執行檔: myLord.exe
{{< /mermaid >}}

[^1]: 機器語言 [wiki](https://en.wikipedia.org/wiki/Machine_code)
[^2]: 組合語言 [組合語言零基礎入門](https://iter01.com/547102.html)

## Reference
1. https://opensourcedoc.com/c-programming/preprocessor/
2. http://blog.udn.com/chungchia/3327025
