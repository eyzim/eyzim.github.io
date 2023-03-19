# 不靠 IDE 自己編譯 C++ (1) - 安裝 MinGW 指南


# 安裝 MinGW

> https://sourceforge.net/projects/mingw/

## 1

綠色的 [Download](https://sourceforge.net/projects/mingw/files/latest/download) 按下去，`mingw-get-setup.exe` 就會開始下載。

## 2

打開 mingw-get-setup.exe，在 Basic Setup 中，選取 `mingw32-base-bin` 和 `mingw32-gcc-g++-bin`

![](/images/compile_my_cpp_with_g++/choose_mingw.png)

## 3

選取完畢後，點擊左上角的 Installation，並 Apply Changes 後，就會開始下載，視網路速度，可能要花個幾分鐘

## 4

進入 windows 的「環境變數設定」 (直接在功能表內直接搜尋也可以)，在系統環境變數的 Path 中加入路徑：

```BASH
C:\MinGW\bin
```

## 5

或是想要偷懶的話，可以在開啟 cmd 時，右鍵「以系統管理員身分執行」，直接設定路徑：

```BASH
setx /m PATH "C:\MinGW\bin;%PATH%"
```

## 6

理論上到這邊就完成 gcc 的安裝，輸入：

```BASH
g++ -v
```

出現以下文字，代表安裝成功，恭喜恭喜！

```
COLLECT_GCC=g++
COLLECT_LTO_WRAPPER=c:/mingw/bin/../libexec/gcc/mingw32/9.2.0/lto-wrapper.exe
gcc version 9.2.0 (MinGW.org GCC Build-2)
```

如果沒有出現 gcc version 的文字，代表安裝失敗，像是出現這樣的文字：

```
'g++' 不是內部或外部命令
```

怒，我們的起點就是比較艱難 QQ

## 7

回到剛才安裝的 `C:\MinGW\bin` 資料夾，直接下載 gcc

```BASH
cd MinGW/bin
mingw-get install gcc g++
```

就會開始下載文件囉，再次輸入

```BASH
g++ -v
```

可以[開始 gcc 之旅了](https://eyzim.github.io/posts/compile_my_cpp_with_g++_2/)！

# Refernce:

1. http://www.codebind.com/cprogramming/install-mingw-windows-10-gcc/
2. https://stackoverflow.com/questions/19287379/how-do-i-add-to-the-windows-path-variable-using-setx-having-weird-problems

