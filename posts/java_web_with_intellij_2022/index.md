# [WEB] 用 IntelliJ 開啟第一個 Java web


以前寫 C++ 時就有用過 [CLion](https://www.jetbrains.com/clion/) 這個強大又美麗的產品，突然被要求要寫 Java web application，就想要試試 IntelliJ 啦 (絕對不能說 [Eclipse](https://www.eclipse.org/) 不得我心哈哈)

[Jet Brains](https://www.jetbrains.com/) 家的產品迭代快速，來份熱騰騰的安裝文囉。

---

## 1. 安裝 [Java SDK](https://www.oracle.com/java/technologies/downloads/#jdk18-windows)

![](/images/install_intellij_build_java_web/java_jdk.png)

## 2. 安裝 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/#section=windows)

![](/images/install_intellij_build_java_web/intellij.png)

→ 他們現在很先進，都會自己帶入路徑免煩惱 🙂

## 3. 下載 [Tomcat](https://tomcat.apache.org/download-10.cgi) 才能開啟 web viewer

下載後解壓縮至此，確定資料夾名稱是 `apache-tomcat-版本號`

## 4. 打開 IntelliJ

![](/images/install_intellij_build_java_web/open_intellij.png)

## 5. 點擊 New Project

![](/images/install_intellij_build_java_web/new_proj.png)

## 6. 選擇新增 Java Enterprise 專案

![](/images/install_intellij_build_java_web/new_java_enterprise_proj.png)

## 7. 點選下一步，就完成專案建置啦~

## 8. 瀏覽器自己打開！

右上角箭頭按下去，Run 專案後，console 會有一些 apache 跑起來的 log，接著瀏覽器就會自己打開

![](/images/install_intellij_build_java_web/run.png)

## Reference

1. https://juejin.cn/post/6844904020780253191

