# Ubunut 20.04 安裝 GCC 採坑紀錄


根據這篇[給新手的 GCC 安裝教學](https://blog.gtwang.org/programming/GCC-comipler-basic-tutorial-examples/)，想到我從來沒有自己裝過 [GCC](https://gcc.gnu.org/)，以前 linux 都是直接用 server 上人家裝好的環境，Windows 直接用 [CLion](https://www.jetbrains.com/clion/download/#section=windows) 或 [Visual Studio](https://visualstudio.microsoft.com/zh-hant/downloads/)，第一次想在自己的 ubuntu 上安裝 GCC，想不到竟然折騰好久 QQ

## 安裝 GCC

```shell
sudo apt get update
```

```shell
sudo apt install GCC
```

很簡單就完成了

## 編譯第一次: `fatal error: stdio.h: No such file or directory`

```
eyzim@MC13:~/code/first-c$ GCC hello.c
hello.c:2:10: fatal error: stdio.h: No such file or directory
    2 | #include <stdio.h>
      |          ^~~~~~~~~
compilation terminated.
```

lib 沒有引入耶

看看這篇 [GCC fatal error: stdio.h: No such file or directory](https://stackoverflow.com/questions/19580758/GCC-fatal-error-stdio-h-no-such-file-or-directory/29284967#29284967)，少了標準庫，應該是之前 GCC 裡面沒包含到

## 回來安裝 g++ 總行了吧，反正一樣都能 compile C / C++

安裝 g++

```shell
sudo apt install libc6-dev g++
```

得到

```
eyzim@MC13:~/code/first-c$ sudo apt install libc6-dev g++
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:
The following packages have unmet dependencies:
libc6-dev : Depends: libc6 (= 2.31-0ubuntu9.7) but 2.31-0ubuntu9.9 is to be installed
E: Unable to correct problems, you have held broken packages.
```

不信邪，直接安裝 build-essential 總行了吧

```shell
sudo apt get install build-essential
```

又失敗

```
eyzim@MC13:~/code/first-c$ sudo apt get install build-essential
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
build-essential : Depends: libc6-dev but it is not going to be installed or libc-dev
Depends: g++ (>= 4:9.2) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

看了一下，應該是 apt 的來源出現問題，但現在在牆內，不想亂動他的話，該怎麼修正呢?

→ 用 `aptitude`

```shell
sudo apt install aptitude
```

```shell
sudo aptitude install g++
```

aptitude 魔法就會自己運作啦

```
eyzim@MC13:~/code/first-c$ sudo aptitude install g++
The following NEW packages will be installed:
  g++ g++-9{a} libc-dev-bin{a} libc6-dev{ab} libcrypt-dev{a} libstdc++-9-dev{a}
  linux-libc-dev{a} manpages-dev{a}
0 packages upgraded, 8 newly installed, 0 to remove and 0 not upgraded.
Need to get 16.2 MB of archives. After unpacking 77.3 MB will be used.
The following packages have unmet dependencies:
libc6-dev : Depends: libc6 (= 2.31-0ubuntu9.7) but 2.31-0ubuntu9.9 is installed

The following actions will resolve these dependencies:
     Keep the following packages at their current version:
1)     g++ [Not Installed]
2)     g++-9 [Not Installed]
3)     libc6-dev [Not Installed]
4)     libstdc++-9-dev [Not Installed]

Accept this solution? [Y/n/q/?] **n**
```

-   這邊記得選 **n**

```
Breaks: hurd (< 1:0.9.git20170910-1), iraf-fitsutil (< 2018.07.06-4), libtirpc1 (< 0.2.3), locales (< 2.31), locales:i386 (< 2.31), locales-all (< 2.31), locales-all:i386 (< 2.31), nocache (< 1.1-1~), nscd (< 2.31), nscd:i386 (< 2.31), r-cran-later (< 0.7.5+dfsg-2), wcc (< 0.0.2+dfsg-3), libc6:i386 (!= 2.31-0ubuntu9.7)
Replaces: libc6-amd64, libc6-amd64:i386, libc6-amd64:x32, libc6-amd64:x32-i386-cross, libc6-amd64:i386-x32-cross, libc6:i386 (< 2.31-0ubuntu9.7)
Description: GNU C Library: Shared libraries
Contains the standard libraries that are used by nearly all programs on the system. This package includes shared versions of the standard C library and the standard math library, as well as many others.
Homepage: https://www.gnu.org/software/libc/libc.html
This action was selected because libc6-dev depends upon libc6 (= 2.31-0ubuntu9.7).

Enter "r 1" to prevent this action from appearing in new solutions.
Enter "a 1" to require that new solutions include this action if possible.

Accept this solution? [Y/n/q/?] a 1
```

選擇 **a 1**

剩下就是問你是否願意安裝檔案，直接 **yes** 過關

完成，用自己安裝的編譯器編譯自己寫的 C code 囉~

Source code:

```C
    // hello.c
    #include <stdio.h>

    int main() {
    	printf("Hello, my first c code on my ubuntu :)\n");
    	return 0;
    }
```

編譯

```shell
GCC hello.c -o mycode
```

檢查一下有沒有產生檔案

```
eyzim@MC13:~/code/first-c$ ll
total 32
drwxrwxr-x 2 eyzim eyzim  4096 Jan 23 02:31 ./
drwxrwxr-x 3 eyzim eyzim  4096 Jan 23 02:27 ../
-rw-rw-r-- 1 eyzim eyzim   109 Jan 23 02:27 hello.c
-rwxrwxr-x 1 eyzim eyzim 16696 Jan 23 02:31 mycode*
```

執行它

```
eyzim@MC13:~/code/first-c$ ./mycode
Hello, my first c code on my ubuntu :)
```

自己裝的 [[GCC]] 用起來真香(誤

