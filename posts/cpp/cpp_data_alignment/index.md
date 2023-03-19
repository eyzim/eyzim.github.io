# 【C++】Data Alignment 資料對齊


變數宣告時給的記憶體空間並非連續，而是每 data alignment 視為一「節」，一節有 4 個位置 (bytes)

```cpp
struct data_alignment
{
    char a1;      // 1 byte
    short a2;     // 2 bytes
    int a3;       // 4 bytes
};
```

1 + 2 + 4 = 7 bytes，但實際結果卻是 12 bytes

第一節放置 `a1`，占用 1 個 byte，剩餘 3 個 bytes 閒置

第二節放置 `a2`，占用 2 個 bytes，剩餘 2 個 bytes 閒置

第三節放置 `a3`，完全占滿 4 個 bytes

實際上應該是在閒置的空間加上 char padding (閒置 bytes 數)

可以修改 data alignment

```cpp
#pragma pack(push)
#pragma pack(1)
  struct data_alignment_adjusted
    {
       char a1;    // 1 byte
       short a2;   // 2 bytes
       int a3;     // 4 bytes
    };
#pragma pack(pop)
```

`pragma pack(1)` 設定一節長度，長度最長以 struct 中最大的數值為主 (ex: 1, 2, 4)

這樣一來，每一節的長度設定為 1 bytes，便不會有閒置空間，實際使用 7 bytes。

