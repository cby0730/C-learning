# OpenCV

# OpenCV C++ 詳細教程

## 1. 基本數據類型

OpenCV 在 C++ 中使用了許多特定的數據類型,了解這些類型對於高效使用該庫至關重要。

### 1.1 基本標量類型

OpenCV 定義了一些基本的標量類型:

- `uchar`: 8位無符號整數 (0..255)
- `char`: 8位有符號整數 (-128..127)
- `ushort`: 16位無符號整數 (0..65535)
- `short`: 16位有符號整數 (-32768..32767)
- `int`: 32位有符號整數
- `float`: 32位浮點數
- `double`: 64位浮點數

### 1.2 點和大小

- `Point`: 表示2D點,有以下變體:
    - `Point2i` 或 `Point`: (int x, int y)
    - `Point2f`: (float x, float y)
    - `Point2d`: (double x, double y)
- `Point3_`: 表示3D點,有以下變體:
    - `Point3i`: (int x, int y, int z)
    - `Point3f`: (float x, float y, float z)
    - `Point3d`: (double x, double y, double z)
- `Size`: 表示矩形大小,有以下變體:
    - `Size2i` 或 `Size`: (int width, int height)
    - `Size2f`: (float width, float height)

### 1.3 矩形和範圍

- `Rect`: 表示2D矩形,定義為:
    - (int x, int y, int width, int height)
- `Range`: 表示整數值範圍:
    - (int start, int end)

## 2. 複雜數據結構

### 2.1 Vec 類

`Vec` 是一個模板類,表示短向量。常用的預定義類型包括:

- `Vec2b`, `Vec2s`(short), `Vec2w`(unshort), `Vec2i`, `Vec2f`, `Vec2d`: 2元素向量
- `Vec3b`, `Vec3s`, `Vec3w`, `Vec3i`, `Vec3f`, `Vec3d`: 3元素向量
- `Vec4b`, `Vec4s`, `Vec4w`, `Vec4i`, `Vec4f`, `Vec4d`: 4元素向量

例如:

- `Vec3b`: 3個 uchar 值,通常用於表示 BGR 顏色
- `Vec3f`: 3個 float 值,可用於表示 RGB 顏色或 3D 點
- `Vec3d`: 3個 double 值,用於高精度計算

### 2.2 Scalar 類

`Scalar` 是一個4元素向量,通常用於表示顏色或作為函數的參數。例如:

```cpp
Scalar(a, b, c, d)  // 創建一個包含4個 double 值的 Scalar
Scalar(a, b, c)     // 相當於 Scalar(a, b, c, 0)
Scalar(a)           // 相當於 Scalar(a, 0, 0, 0)

```

- Scalar 與 Vec4d 的比
    
    Scalar 確實可以視為一個有 4 個元素的數組，它實際上是 Vec4d 的別名：
    
    ```cpp
    typedef Vec<double, 4> Scalar;
    ```
    
    Scalar 和 Vec4d 的主要區別在於使用場景：
    
    - Scalar 通常用於表示顏色或作為函數的通用參數
    - Vec4d 更多用於數學計算
    
    在功能上，它們是相同的，都是包含 4 個 double 類型值的向量。
    

### 2.3 Mat 類

`Mat` 是 OpenCV 中最核心的類,用於表示 n 維稠密數組。它主要用於存儲圖像,但也可以存儲其他類型的數據。

Mat 的構成:

1. 矩陣頭 (matrix header): 包含矩陣大小,存儲方法,存儲地址等信息
2. 指向數據的指針 (pointer to the data)

Mat 的重要屬性（矩陣頭包含的東西）:

- `int rows`: 矩陣的行數
- `int cols`: 矩陣的列數
- `int dims`: 矩陣的維度
- `uchar* data`: 指向數據的指針
- `size_t step`: 矩陣中一行所佔的字節數

創建 Mat 對象的方法:

```cpp
Mat M(3, 3, CV_8UC3);  // 3x3 的 3 通道 8 位無符號整數矩陣
Mat M = Mat::zeros(3, 3, CV_8UC3);  // 用0初始化的矩陣
Mat M = Mat::ones(3, 3, CV_32F);  // 用1初始化的單精度浮點數矩陣
Mat M = Mat::eye(3, 3, CV_64F);  // 3x3 的單位矩陣 (雙精度)
```

### 2.4 Mat 的數據類型

Mat 可以存儲多種數據類型,通過一個預定義的常量來指定:

- `CV_8U`: 8位無符號整數 (0..255)
- `CV_8S`: 8位有符號整數 (-128..127)
- `CV_16U`: 16位無符號整數 (0..65535)
- `CV_16S`: 16位有符號整數 (-32768..32767)
- `CV_32S`: 32位有符號整數
- `CV_32F`: 32位浮點數
- `CV_64F`: 64位浮點數

通道數可以通過在類型後添加 "C(通道數)" 來指定,如 `CV_8UC3` 表示 3 通道的 8 位無符號整數。

如果沒有加上通道數，則會創建單通道圖像。例如：`CV_64F`就會創建64位浮點數單通道的影像。

## 3. 圖像處理函數

OpenCV 提供了大量的圖像處理函數。以下是一些常用函數的概覽:

### 3.1 基本操作

- `imread()`: 讀取圖像
    
    ```cpp
    Mat img = imread("image.jpg", IMREAD_COLOR);
    
    ```
    
- `imwrite()`: 保存圖像
    
    ```cpp
    imwrite("output.jpg", img);
    
    ```
    
- `imshow()`: 顯示圖像
    
    ```cpp
    imshow("Window Name", img);
    
    ```
    

### 3.2 圖像轉換

- `cvtColor()`: 顏色空間轉換
    
    ```cpp
    Mat gray;
    cvtColor(img, gray, COLOR_BGR2GRAY);
    
    ```
    
- `resize()`: 調整圖像大小
    
    ```cpp
    Mat resized;
    resize(img, resized, Size(width, height));
    
    ```
    
- `threshold()`: 圖像二值化
    
    ```cpp
    Mat binary;
    threshold(gray, binary, 127, 255, THRESH_BINARY);
    
    ```
    

### 3.3 圖像濾波

- `GaussianBlur()`: 高斯模糊
    
    ```cpp
    Mat blurred;
    GaussianBlur(img, blurred, Size(5, 5), 0);
    
    ```
    
- `medianBlur()`: 中值濾波
    
    ```cpp
    Mat median;
    medianBlur(img, median, 5);
    
    ```
    

### 3.4 形態學操作

- `erode()`: 腐蝕
    
    ```cpp
    Mat eroded;
    erode(img, eroded, getStructuringElement(MORPH_RECT, Size(3, 3)));
    
    ```
    
- `dilate()`: 膨脹
    
    ```cpp
    Mat dilated;
    dilate(img, dilated, getStructuringElement(MORPH_RECT, Size(3, 3)));
    
    ```
    

### 3.5 邊緣檢測

- `Canny()`: Canny 邊緣檢測
    
    ```cpp
    Mat edges;
    Canny(img, edges, 100, 200);
    
    ```
    

## 4. 進階函數

### 4.1 直方圖

- `calcHist()`: 計算直方圖
- `equalizeHist()`: 直方圖均衡化

### 4.2 特徵檢測和描述

- SIFT, SURF, ORB 等特徵檢測器
- `findContours()`: 輪廓檢測

### 4.3 圖像分割

- `watershed()`: 分水嶺算法
- `grabCut()`: GrabCut 算法

## 5. Mat 如何儲存圖片

Mat 儲存圖片的方式取決於圖片的類型：

### 5.1 灰度圖（單通道）

對於 8 位灰度圖，Mat 儲存為一個 2D 數組，每個元素是一個 0-255 的整數：

```
Copy
[ [255, 0, 128],
  [64, 192, 32],
  [10, 100, 200] ]

```

### 5.2 彩色圖（三通道）

對於 24 位彩色圖（8 位 x 3 通道），Mat 儲存為一個 3D 數組，每個像素由 3 個 0-255 的整數表示（通常是 BGR 順序）：

```
Copy
[ [ [255, 0, 0],   [0, 255, 0],   [0, 0, 255]   ],
  [ [128, 128, 0], [0, 128, 128], [128, 0, 128] ],
  [ [64, 64, 64],  [192, 192, 192], [32, 32, 32] ] ]
```

### 5.3 內存佈局

實際上，Mat 在內存中是線性存儲的。對於彩色圖，數據按 BGR BGR BGR ... 的順序排列：

```
Copy
B G R B G R B G R ...
```

`step` 成員變量告訴我們如何在行之間導航。

## 6. OpenCV Mat 類的 step 參數詳解

### 6.1 step 參數的定義

在 OpenCV 的 Mat 類中,`step` 是一個非常重要的參數。它表示從一行的開始到下一行開始所需的字節數。換句話說,它告訴我們如何在內存中從一行跳到下一行。

### 6.2 step 的作用

`step` 的主要作用是幫助我們在二維或多維數組中正確地導航,特別是在處理圖像數據時。由於圖像數據在內存中是線性存儲的,`step` 使我們能夠準確地計算出每個像素的位置。

### 6.3 簡單示例

讓我們通過一個簡單的例子來說明 `step` 的作用:

假設我們有一個 3x3 的灰度圖像(8位無符號整數,單通道):

```cpp
Mat img(3, 3, CV_8UC1);
```

填充一些數據:

```cpp
img.at<uchar>(0,0) = 1;   img.at<uchar>(0,1) = 2;   img.at<uchar>(0,2) = 3;
img.at<uchar>(1,0) = 4;   img.at<uchar>(1,1) = 5;   img.at<uchar>(1,2) = 6;
img.at<uchar>(2,0) = 7;   img.at<uchar>(2,1) = 8;   img.at<uchar>(2,2) = 9;
```

這個矩陣在內存中的實際存儲可能是這樣的:

```
[1 2 3 _ 4 5 6 _ 7 8 9 _]
```

注意在每行的末尾可能有一些填充字節(用 _ 表示)。

現在,讓我們看看如何使用 `step` 來訪問這些數據:

```cpp
for (int i = 0; i < img.rows; ++i) {
		// 這裡的 img.ptr<uchar>(i) 返回第 i 行的起始位置。
    const uchar* row = img.ptr<uchar>(i);
    for (int j = 0; j < img.cols; ++j) {
        cout << (int)row[j] << " ";
    }
    cout << endl;
}

cout << "Step: " << img.step << endl;
```

輸出可能是:

```
1 2 3
4 5 6
7 8 9
Step: 4
```

在這個例子中,`step` 的值是 4,這意味著每行佔用 4 個字節。雖然我們只存儲了 3 個字節的數據,但 OpenCV 為了內存對齊添加了 1 個字節的填充。

### 6.4 使用 step 進行手動導航

我們可以使用 `step` 來手動導航這個矩陣:

```cpp
uchar* data = img.data;
for (int i = 0; i < img.rows; ++i) {
    for (int j = 0; j < img.cols; ++j) {
        cout << (int)data[i*img.step + j] << " ";
    }
    cout << endl;
}

```

這裡,`i*img.step` 將指針移動到正確的行,然後 `j` 在該行內移動到正確的列。

### 6.5 多通道圖像中的 step

對於多通道圖像,例如 BGR 彩色圖像,`step` 的值會更大:

```cpp
Mat colorImg(3, 3, CV_8UC3);
cout << "Step for color image: " << colorImg.step << endl;

```

這可能輸出:

```
Step for color image: 12

```

因為每個像素現在佔用 3 個字節(藍、綠、紅各一個字節),所以一行的總字節數是 9,再加上可能的填充,得到 12。

## 7 OpenCV Mat 類的內部儲存結構

### 7.1 Mat 的儲存原理

雖然 Mat 類可以表示多維陣列，但其內部實際上是使用一維陣列來儲存所有數據的。這種設計有幾個重要的優點：

1. 記憶體效率：連續的一維記憶體空間通常比分散的多維記憶體空間更容易管理和訪問。
2. 快速訪問：使用一維陣列可以更容易地實現快速的記憶體訪問和緩存優化。
3. 簡化實現：許多底層操作可以更容易地在一維陣列上實現。

### 7.2 多維數據在一維陣列中的映射

Mat 類使用一些額外的資訊（如 step、dims、size 等）來將多維坐標映射到一維陣列的正確位置。

### 7.2.1 二維矩陣的例子

考慮一個 3x3 的矩陣：

```
[ 1  2  3 ]
[ 4  5  6 ]
[ 7  8  9 ]

```

這個矩陣在記憶體中實際上是這樣儲存的：

```
[ 1  2  3  4  5  6  7  8  9 ]

```

### 7.2.2 三維矩陣的例子

對於一個 2x2x3 的三維矩陣：

```
[
  [ [1  2  3]
    [4  5  6] ]
  [ [7  8  9]
    [10 11 12] ]
]

```

在記憶體中的儲存可能是：

```
[ 1  2  3  4  5  6  7  8  9  10 11 12 ]

```

### 7.3 訪問多維數據

Mat 類提供了多種方法來訪問這種一維儲存的多維數據：

1. 使用 at() 方法：
    
    ```cpp
    mat.at<uchar>(i, j)  // 對於 2D
    mat.at<uchar>(i, j, k)  // 對於 3D
    
    ```
    
2. 使用 ptr() 方法：
    
    ```cpp
    uchar* row = mat.ptr<uchar>(i);
    uchar value = row[j];
    
    ```
    
3. 直接使用 data 指針（需要小心使用）：
    
    ```cpp
    uchar* data = mat.data;
    uchar value = data[i * mat.step + j];
    
    ```
    

### 7.4 效率考慮

雖然 Mat 在內部使用一維陣列，但它的設計允許我們以多維的方式高效地訪問數據。這種設計結合了一維陣列的效率和多維表示的便利性。

## 8. 使用 Mat 存儲一般數據

Mat 確實可以用來存儲一般的多維數組數據。例如：

```cpp
cpp
Copy
// 創建一個 4x3x2 的 3D 數組
Mat data(4, 3, CV_32FC2);

// 訪問和修改數據
data.at<Vec2f>(1, 2)[0] = 10.0f;// 設置 (1,2) 位置的第一個元素
data.at<Vec2f>(1, 2)[1] = 20.0f;// 設置 (1,2) 位置的第二個元素

```

Mat 的靈活性使它能夠替代多維數組，特別是在需要進行矩陣運算或使用 OpenCV 的其他功能時。