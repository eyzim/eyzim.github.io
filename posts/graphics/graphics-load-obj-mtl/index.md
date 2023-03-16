# 【圖學】讀取 .obj 和 .mtl 檔案



成功 trace 過球、三角形之後，我們想要試著畫出更複雜的圖片，這時候就需要幫程式加入讀取 `.obj` 和 `.mtl` 的功能。

當然，這些都是不用背起來的內容，更有許多 load .obj 的現成工具，這篇的目的在於渲染出奇怪的影像時，能順利肉眼 parse 出一點問題。


## obj 格式


obj 檔案使用記事本開啟後，長成下面這副模樣。

```
o Plane
v -0.5 -0.5 0.0
v -0.5 -0.5 1.0
v  0.5 -0.5 1.0
v  0.5 -0.5 0.0
vn 0.0000 -1.0000 0.0000
usemtl Reflection001
s off
f 2//1 3//1 1//1
f 4//1 1//1 3//1
```

每一行視為一個物件。

開頭都有一組英文字母，告訴電腦「這一行的物件屬性」，`v` 表示這一行是一個 `vertex（頂點）` 屬性的物件。

字母後面接著一串數字，就是這個物件的內容。

| 字母 | 英文全名       |    意義    |
| ---  | ---           |    ---    |
|  v   | vertex        | 頂點座標   |
| vn   | vertex normal | 法向量座標 |
| vt   | vertex texture| 紋理座標   |
| f    | surface       | 面         |
| g    | group         | 群組       |

### v 頂點

以第二行輸入為例：

開頭 `v` 宣告物件為一個**頂點**，頂點座標為 ( -0.5 -0.5 0.0 )

{{< highlight g "linenos=table, linenostart=2" >}}
v -0.5 -0.5 0.0
{{< / highlight >}}

### vn 法向量
每個平面圖形都會擁有一個**法向量**，在 obj 檔案中，已經幫我們算好了，待會一一對應即可。

{{< highlight g "linenos=table, linenostart=6" >}}
vn 0.0000 -1.0000 0.0000
{{< / highlight >}}

### f 面

有了頂點之後，我們便可以組成**面**，obj 檔案中的面的基礎形狀是<mark>三角形</mark>。

如下，我們可以看到第九行第一個數字 `2`，表示**第二個** `v` 物件，為頂點 (-0.5 -0.5 0.0)。

雙斜線後的 `1`，表示這個面的法向量是**第一個** `vn` 物件。

{{< highlight g "linenos=table, linenostart=9" >}}
f 2//1 3//1 1//1
{{< / highlight >}}

用白話文表示：
{{% admonition  %}}
這個（三角形）由 **第二個**、**第三個**、**第一個** 頂點 `v` 組成，

且這一個三角形的法向量是 **第一個** 法向量 `vn`。
{{% /admonition %}}



## 用 C++ 讀取 obj

了解了 obj 檔案的結構後，再來思考怎麼讀取。

原本想要使用 `getline` 逐行讀取，但是讀到面的時候會有雙斜線的問題，故採用 `fscanf` 來進行。

首先，宣告儲存各物件的 class。

其中，一個面只有一個法向量座標的資料，所以只有開一個 normal 的位置。

```cpp
/* load "v" from .obj */
class cVertices
{
    public:
        double x, y, z;
};

/* load "vn" from .obj */
class cNormal
{
    public:
        double x, y, z;
};

/* load "f" from .obj */
class cSurface
{
    public:
        int point1, point2, point3;
        int normal;
};

/* Store these information into triangles */
class cTriangle {
    public:
        vec3 vertex1(x1, y1, z1),
             vertex2(x2, y2, z2),
             vertex3(x3, y3, z3);
        vec3 nor;                     // vec3 = vector in 3D space
};

```

第二步，根據每一行第一個字串，歸類各個物件。

```cpp

while (fscanf(objfile, "%s", lineHeader) != EOF) {
    if (strcmp(lineHeader, "v") == 0)
    {
        fscanf(objfile, "%lf %lf %lf", &vertices.x, &vertices.y, &vertices.z);
        vVertices.push_back(vertices);
    }
    else if (strcmp(lineHeader, "vn") == 0)
    {
        fscanf(objfile, "%lf %lf %lf", &normal.x, &normal.y, &normal.z);
        vNormal.push_back(normal);
    }
    else if (strcmp(lineHeader, "f") == 0)
    {
        fscanf(objfile, "%u//%u %u//%u %u//%u",
                        &surface.point1, &surface.normal,     // 第一個頂點, 法向量
                        &surface.point2, &surface.normal,     // 第二個頂點, 法向量
                        &surface.point3, &surface.normal);    // 第三個頂點, 法向量
        vSurface.push_back(surface);
    }
}
```

第三步，檢查是否成功讀入各資料。

```cpp
cout << "vVertices" << endl;
for (unsigned i = 0 ; i < vVertices.size(); i++)
{
    cout << i << " = (x, y, z) : ("
         << vVertices[i].x << " "
         << vVertices[i].y << " "
         << vVertices[i].z << ")" << endl;
}

cout << endl << "vNormal" << endl;
for (unsigned i = 0 ; i < vNormal.size(); i++)
{
    cout << i << " = (x, y, z) : ("
         << vNormal[i].x << " "
         << vNormal[i].y << " "
         << vNormal[i].z << ")" << endl;
}

cout << endl << "vSurface" << endl;
for (unsigned i = 0 ; i < vSurface.size(); i++)
{
    cout << i << " = (normal, point1, point2, point3) : ("
         << vSurface[i].normal << " "
         << vSurface[i].point1 << " "
         << vSurface[i].point2 << " "
         << vSurface[i].point3 << ")" << endl;
}
```

輸出會長這樣，可以清楚的分類頂點、法向量、面依序排列

```
vVertices
0 = (x, y, z) : (-0.5 -0.5 0)
1 = (x, y, z) : (-0.5 -0.5 1)
2 = (x, y, z) : (0.5 -0.5 1)
3 = (x, y, z) : (0.5 -0.5 0)

vNormal
0 = (x, y, z) : (0 -1 0)

vSurface
0 = (normal, point1, point2, point3) : (1 2 3 1)
1 = (normal, point1, point2, point3) : (1 4 1 3)
```

第四步，將讀取到的資料歸類並放入 triangle。

```cpp
for (unsigned i = 0; i < vSurface.size(); i++)
{
    int tempP1 = vSurface[i].point1,
        tempP2 = vSurface[i].point2,
        tempP3 = vSurface[i].point3,
        tempNor = vSurface[i].normal;

    triangle.vertex1 = vec3(vVertices[tempP1-1].x, vVertices[tempP1-1].y, vVertices[tempP1-1].z);
    triangle.vertex2 = vec3(vVertices[tempP2-1].x, vVertices[tempP2-1].y, vVertices[tempP2-1].z);
    triangle.vertex3 = vec3(vVertices[tempP3-1].x, vVertices[tempP3-1].y, vVertices[tempP3-1].z);
    triangle.nor = vec3(vNormal[tempNor-1].x, vNormal[tempNor-1].y, vNormal[tempNor-1].z);

    vTriangle.push_back(triangle);
}
```
試著印出這個 obj 檔案的第一個 triangle 的資料
```
triangle data
vertex 1: (-0.5 -0.5 1)
vertex 2: (0.5 -0.5 1)
vertex 3: (-0.5 -0.5 0)
normal: (0 -1 0)
```

成功！

{{% admonition tip "Hint"%}}
-  windows 10 有內建 3D 檢視器，可以直接打開 obj 檔案

<br>

-  眼睛記得要設定為 (0, 0, 0) 才能跟 3D 軟體打開的樣子相同
{{% /admonition %}}

---

## mtl 格式

```
newmtl Reflection001
  Ns 8.0000
  Ni 2.5000
  d  1.0000
  illum 2 
  Ka 0.0000 0.0000 0.0000
  Kd 0.5080 0.5080 0.5080
  Ks 0.2000 0.2000 0.2000
  Ke 0.1000 0.1000 0.1000
```

- Ka: 材質的陰影色 `ambient color`
- Kd: 材質本身的顏色 `diffuse color`
- Ks: 材質的鏡面光 `specular color`，若設為 `Ks 0.000 0.000 0.000` 則為無反光。
- Ke: 材質的放射光 `emissive color`，物體自己會發光
- Ns: 反射係數，範圍從 0 到 1000
- Ni: 折射值，範圍從 0.001 到 10，若值為 1.0，光線通過時不會偏移。
- d / Tr: 材質可以是透明的，範圍從 0.0 到 1.0，數值愈低，表示背景越明顯。
- illum: 光照模型 `illumination`

    0. 色彩開，陰影色關

    1. 色彩開，陰影色開

    2. 鏡面光開

    3. 反射開，光線追蹤開

    4. 透明： 玻璃開 反射：光線追蹤開

    5. 反射：菲涅爾衍射開，光線追蹤開

    6. 透明：折射開 反射：菲涅爾衍射關，光線追蹤開

    7. 透明：折射開 反射：菲涅爾衍射開，光線追蹤開

    8. 反射開，光線追蹤關

    9. 透明： 玻璃開 反射：光線追蹤關

    10. 投射陰影於不可見表面


## Reference
1. [pcd，obj，mtl檔案格式解析](https://www.itread01.com/content/1541574853.html)
2. [(C++\openGL)讀取.obj模型檔](http://jackraken.github.io/2014/07/24/5-opengl-loadOBJ/)
3. [obj + mtl 格式](https://read01.com/8765Gn.html)
