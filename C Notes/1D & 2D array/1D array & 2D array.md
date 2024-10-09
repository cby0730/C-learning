# 1D array & 2D array

# Test code

不同維度不同容器測試程式碼

```cpp
// Only test for the reading speed from array
#include <iostream>
#include <chrono>
#include <memory>

const int ROWS = 1000;
const int COLS = 1000;
const int ITERATIONS = 1000;

void test_1d_array(int* arr) {
    for (int k = 0; k < ITERATIONS; ++k) {
        for (int i = 0; i < ROWS; ++i) {
            for (int j = 0; j < COLS; ++j) {
                int val = arr[i * COLS + j];
            }
        }
    }
}

void test_2d_array(int arr[ROWS][COLS]) {
    for (int k = 0; k < ITERATIONS; ++k) {
        for (int i = 0; i < ROWS; ++i) {
            for (int j = 0; j < COLS; ++j) {
                int val = arr[i][j];
            }
        }
    }
}

void test_2d_array_malloc(int** arr) {
    for (int k = 0; k < ITERATIONS; ++k) {
        for (int i = 0; i < ROWS; ++i) {
            for (int j = 0; j < COLS; ++j) {
                int val = arr[i][j];
            }
        }
    }
}

void test_2d_array_new(int* arr) {
    for (int k = 0; k < ITERATIONS; ++k) {
        for (int i = 0; i < ROWS; ++i) {
            for (int j = 0; j < COLS; ++j) {
                int val = arr[i * COLS + j];
            }
        }
    }
}

void test_2d_array_new_ptr(int** arr) {
    for (int k = 0; k < ITERATIONS; ++k) {
        for (int i = 0; i < ROWS; ++i) {
            for (int j = 0; j < COLS; ++j) {
                int val = arr[i][j];
            }
        }
    }
}

void test_2d_array_unique(int* arr) {
    for (int k = 0; k < ITERATIONS; ++k) {
        for (int i = 0; i < ROWS; ++i) {
            for (int j = 0; j < COLS; ++j) {
                int val = arr[i * COLS + j];
            }
        }
    }
}

void test_2d_array_unique_ptr(std::unique_ptr<std::unique_ptr<int[]>[]>& arr) {
    for (int k = 0; k < ITERATIONS; ++k) {
        for (int i = 0; i < ROWS; ++i) {
            for (int j = 0; j < COLS; ++j) {
                int val = arr[i][j];
            }
        }
    }
}

int main() {
    // 1D array (stack allocated)
    int arr_1d[ROWS * COLS];
    
    // 2D array (stack allocated)
    int arr_2d[ROWS][COLS];
    
    // 1D array (heap allocated using malloc)
    int* arr_1d_malloc = (int*)malloc(ROWS * COLS * sizeof(int));
    
    // 2D array (heap allocated using malloc)
    int** arr_2d_malloc = (int**)malloc(ROWS * sizeof(int*));
    for (int i = 0; i < ROWS; ++i) {
        arr_2d_malloc[i] = (int*)malloc(COLS * sizeof(int));
    }

    // 1D array (heap allocated using new)
    int* arr_1d_new = new int[ROWS * COLS];

    // 2D array (heap allocated using new)
    int** arr_2d_new = new int*[ROWS];
    for (int i = 0; i < ROWS; ++i) {
        arr_2d_new[i] = new int[COLS];
    }

    // 1D array (heap allocated using make_unique)
    auto arr_1d_unique = std::make_unique<int[]>(ROWS * COLS);

    // 2D array (heap allocated using make_unique)
    auto arr_2d_unique = std::make_unique<std::unique_ptr<int[]>[]>(ROWS);
    for (int i = 0; i < ROWS; ++i) {
        arr_2d_unique[i] = std::make_unique<int[]>(COLS);
    }
    
    // Test 1D array (stack)
    auto start = std::chrono::high_resolution_clock::now();
    test_1d_array(arr_1d);
    auto end = std::chrono::high_resolution_clock::now();
    std::cout << "1D array (stack) time: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(end - start).count() 
              << " ms\n";
    
    // Test 2D array (stack)
    start = std::chrono::high_resolution_clock::now();
    test_2d_array(arr_2d);
    end = std::chrono::high_resolution_clock::now();
    std::cout << "2D array (stack) time: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(end - start).count() 
              << " ms\n";
    
    // Test 1D array (heap using malloc)
    start = std::chrono::high_resolution_clock::now();
    test_1d_array(arr_1d_malloc);
    end = std::chrono::high_resolution_clock::now();
    std::cout << "1D array (heap using malloc) time: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(end - start).count() 
              << " ms\n";
    
    // Test 2D array (heap using malloc)
    start = std::chrono::high_resolution_clock::now();
    test_2d_array_malloc(arr_2d_malloc);
    end = std::chrono::high_resolution_clock::now();
    std::cout << "2D array (heap using malloc) time: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(end - start).count() 
              << " ms\n";
    
    // Test 1D array (heap using new)
    start = std::chrono::high_resolution_clock::now();
    test_2d_array_new(arr_1d_new);
    end = std::chrono::high_resolution_clock::now();
    std::cout << "1D array (heap using new) time: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(end - start).count() 
              << " ms\n";

    // Test 2D array (heap using new)
    start = std::chrono::high_resolution_clock::now();
    test_2d_array_new_ptr(arr_2d_new);
    end = std::chrono::high_resolution_clock::now();
    std::cout << "2D array (heap using new) time: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(end - start).count() 
              << " ms\n";

    // Test 1D array (heap using make_unique)
    start = std::chrono::high_resolution_clock::now();
    test_2d_array_unique(arr_1d_unique.get());
    end = std::chrono::high_resolution_clock::now();
    std::cout << "1D array (heap using make_unique) time: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(end - start).count() 
              << " ms\n";

    // Test 2D array (heap using make_unique)
    start = std::chrono::high_resolution_clock::now();
    test_2d_array_unique_ptr(arr_2d_unique);
    end = std::chrono::high_resolution_clock::now();
    std::cout << "2D array (heap using make_unique) time: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(end - start).count() 
              << " ms\n";
    
    // Free heap-allocated memory
    free(arr_1d_malloc);
    for (int i = 0; i < ROWS; ++i) {
        free(arr_2d_malloc[i]);
    }
    free(arr_2d_malloc);

    delete[] arr_1d_new;
    for (int i = 0; i < ROWS; ++i) {
        delete[] arr_2d_new[i];
    }
    delete[] arr_2d_new;
    
    return 0;
}

```

# Result

![截圖 2024-07-16 下午1.57.09.jpg](1D%20array%20&%202D%20array%2099e6dc9434924f48ac1d982a8a4b3592/2418b744-fc2d-4b2f-b54c-c96249a2f8f1.png)

# Conclusion

## 1. 1D 和 2D 陣列的時間比較

- 1D array (stack): 1425 ms
- 2D array (stack): 1800 ms

1D 陣列的讀取速度顯然比 2D 陣列快，因為 1D 陣列中的元素是連續儲存的，索引計算相對簡單。而 2D 陣列雖然也是連續的，但多了一層計算和索引，因此時間略慢。

## 2. 堆內存分配的 1D 和 2D 陣列比較

- 1D array (heap using malloc): 1423 ms
- 2D array (heap using malloc): 1821 ms

使用 malloc 分配的 1D 和 2D 陣列，速度與堆疊分配的相似。這顯示出在大多數情況下，malloc 的記憶體分配對 1D 陣列沒有顯著的額外開銷。然而，2D 陣列中的 malloc 分配需要分配每一列的記憶體，這使得它比 1D 陣列稍微慢一點。

## 3. 使用 new 分配記憶體的比較

- 1D array (heap using new): 1421 ms
- 2D array (heap using new): 1823 ms

使用 new 分配記憶體的結果與 malloc 分配的結果幾乎相同，顯示出 new 和 malloc 在執行時間上沒有太大差別。

## 4. 使用 make_unique 分配記憶體的比較

- 1D array (heap using make_unique): 1429 ms
- 2D array (heap using make_unique): 21226 ms

1D 陣列使用 make_unique 的速度與其他堆記憶體分配的方式接近。但是，2D 陣列使用 make_unique 的速度極慢（21226 ms），這很可能是因為 make_unique 內部執行了大量的記憶體管理操作，導致了顯著的性能下降。

## 5. 結論

1. 1D 陣列在各種記憶體分配方式中表現較為穩定，速度差異不大。
2. 2D 陣列在記憶體分配上會有明顯的性能差異，尤其是當使用像 make_unique 這樣的 STL 智慧指標時，可能會帶來顯著的性能負擔。
3. stack（堆疊分配） 優於 heap（堆分配），這是因為堆疊分配的速度通常比堆分配快，尤其是在高頻次存取的情況下。

# Discuss

## 1. 1D vs 2D 陣列效能差異的底層原因

2D 陣列比 1D 陣列慢的原因涉及到記憶體存取的本質和電腦記憶體階層架構（Memory Hierarchy）的工作原理。

這背後牽涉到 CPU 快取、記憶體尋址、陣列儲存方式等底層概念。

### 1.1 記憶體佈局

1D 陣列的資料是**連續儲存**的。當我們存取 1D 陣列中的元素時，元素是線性排列的，這意味著 CPU 快取可以在一次快取加載中取得多個連續元素，這就是**空間局部性**的概念。

```cpp
int arr[1000];
// 元素 arr[0], arr[1], arr[2] 是連續儲存在記憶體中的

```

相對來說，2D 陣列在記憶體中有兩種常見的佈局方式：

- **Row-major order（行優先）**：2D 陣列儲存時，所有行是連續儲存的，例如 `arr[0][0], arr[0][1], arr[0][2], ...` 都連續儲存在記憶體中。
- **Column-major order（列優先）**：列資料是連續的，即 `arr[0][0], arr[1][0], arr[2][0], ...`。

在 C++ 中，默認使用的是 **Row-major order**。

```cpp
arr[0][0], arr[0][1], arr[0][2], arr[0][3],
arr[1][0], arr[1][1], arr[1][2], arr[1][3]
// 在行優先的 2D 陣列中，行內的資料是連續的，而不同行之間的資料是不連續的。
```

當你在迭代 2D 陣列時，內層循環存取的 `arr[i][j]` 可能無法有效利用 CPU 快取，特別是如果迭代順序並非行優先或列優先的情況下，會導致快取未命中（cache miss），從而降低效能。

```cpp
// Row-major order 的存取可以有效利用快取
for (int i = 0; i < ROWS; ++i) {
    for (int j = 0; j < COLS; ++j) {
        int val = arr[i][j];  // 這種存取方式符合 row-major
    }
}

```

但是，在某些情況下，如果我們的存取方式不符合資料的排列，例如：

```cpp
// 假設這樣的存取
for (int j = 0; j < COLS; ++j) {
    for (int i = 0; i < ROWS; ++i) {
        int val = arr[i][j];  // 列優先存取，在 row-major 的排列中會有更多快取未命中
    }
}

```

這時候效能會變差，因為存取的資料不是連續的，導致頻繁的快取未命中。

### 1.2. 存取`arr[0][0] & arr[1][0]`兩個元素的情況

在 1D 陣列中，儲存的數據是連續的，但如果你存取 `arr[0][0] & arr[1][0]`，在邏輯上這是第 1000 個元素和第 3000 個元素（假設每行 1000 個元素），它們的實際記憶體地址相距甚遠，這兩次存取的記憶體位置不會是相鄰的，因此也可能導致快取未命中。

在 2D 陣列中，這兩個元素是不同行的開頭，它們在記憶體中的存放位置不會連續。因此這兩次存取的記憶體也是不連續的，這與 1D 陣列的情況相似。

**總結：**不論是 1D 還是 2D，存取不連續的記憶體位置（如 `arr[0][0] & arr[1][0]`）都可能導致快取未命中，但 2D 陣列的結構更複雜，在某些情況下（例如列優先存取），快取未命中的機率會更高。

### 1.3. 快取記憶體（Cache Memory）與空間局部性

CPU 有一個快速的快取記憶體層次，快取會加速對資料的存取。如果資料能夠連續存放並被順序存取，快取可以加載整個區塊，並在後續的存取中利用已經加載的資料。1D 陣列的存取通常是連續的，因此可以較好地利用快取。而 2D 陣列的內層結構使得存取不一定是連續的，這會導致更多的快取未命中。

因此，2D 陣列比 1D 陣列慢的根本原因是：

- 1D 陣列中資料是連續排列的，能更有效利用 CPU 快取。
- 2D 陣列的索引和記憶體佈局會增加更多計算和可能的快取未命中。

---

## 2. 動態記憶體分配 (`malloc`, `new`, `free`, `delete`)

### 2.1 `malloc` 和 `free`

`malloc` 和 `free` 是 C 語言中的動態記憶體分配函式。它們的特點如下：

- **`malloc`**：從堆（heap）中分配一塊連續的記憶體區塊，不會初始化該區塊。呼叫 `malloc` 時需要手動指定大小，以 bytes 為單位。
    - 例如：`int* arr = (int*)malloc(100 * sizeof(int));`
- **`free`**：釋放用 `malloc` 分配的記憶體，這對避免記憶體洩漏至關重要。

`malloc` 的分配與釋放過程：

- 當 `malloc` 被呼叫時，系統會從堆中找到一個足夠大的空閒區塊並返回其位址。如果沒有足夠大的區塊可用，會返回 `NULL`。
- `malloc` 不會初始化記憶體，分配的記憶體區塊的內容是不確定的。
- **釋放**：`free` 用來釋放分配的記憶體。當記憶體被釋放後，它可以被重新使用。

```cpp
int* arr = (int*)malloc(100 * sizeof(int));  // 分配 100 個 int 空間
free(arr);  // 釋放記憶體

```

### 2.2`malloc` 返回的**地址**

當 `malloc` 被呼叫時，系統會從堆（heap）中尋找一個足夠大的空閒記憶體區塊，並返回該區塊的首位址。這個位址指向的是記憶體區塊的起始點（一般是 1 個 byte 的位址）。現代計算機的位址單位通常以 byte 為基礎。

例如，對於 32 位元系統，記憶體的基本單位是 1 byte（8 bits），因此 malloc 返回的地址是該記憶體區塊的第一個 byte的位址。這段位址會指向足夠大的連續記憶體區塊，滿足 malloc 要求的大小。

```cpp
int* arr = (int*)malloc(100 * sizeof(int));
// malloc 返回的位址 arr 指向分配的第一個 byte 的位址
```

### **2.3 Malloc 不會初始化記憶體，這意味著什麼？**

- **Malloc 不會初始化記憶體**
    
    當你使用 `malloc` 分配記憶體時，系統只是提供一塊未初始化的記憶體區域。這意味著這塊記憶體中**之前可能儲存過其他資料**，但 `malloc` 不會將它清零或做任何修改。因此，分配的記憶體區域中的數值是隨機的、不確定的。
    
    ```cpp
    int* arr = (int*)malloc(100 * sizeof(int));
    // arr 指向的記憶體區域可能包含舊的垃圾數據，未初始化
    ```
    
- **如何初始化記憶體？**
    
    如果你想要將 `malloc` 分配的記憶體清零，可以使用 `memset` 或 `calloc`：
    
    - `memset`：將一段記憶體的內容設置為某個值。
    
    ```cpp
    int* arr = (int*)malloc(100 * sizeof(int));
    memset(arr, 0, 100 * sizeof(int));  // 將所有記憶體內容設置為 0
    ```
    
    - `calloc`：與 `malloc` 類似，但它會分配並初始化記憶體為 0。
    
    ```cpp
    int* arr = (int*)calloc(100, sizeof(int));  // 分配並初始化為 0
    ```
    

### 2.4 `new` 和 `delete`

`new` 和 `delete` 是 C++ 提供的動態記憶體分配運算子。與 `malloc` 和 `free` 的主要差異在於：

- **`new`**：分配並且初始化記憶體。用 `new` 分配的記憶體會呼叫相應的構造函式來初始化物件。
    - 例如：`int* arr = new int[100];`
- **`delete`**：釋放用 `new` 分配的記憶體，並呼叫相應的析構函式來進行清理。

`new` 的分配與釋放過程：

- `new` 不僅分配記憶體，還會初始化該區塊並呼叫物件的構造函式。
- `new[]` 用來分配動態陣列，並且每個元素會初始化。
- **釋放**：使用 `delete` 或 `delete[]` 將記憶體釋放並呼叫物件的析構函式。對於陣列，必須使用 `delete[]`。

```cpp
int* arr = new int[100];  // 分配並初始化 100 個 int
delete[] arr;  // 釋放記憶體

```

### 2.5 `n**ew` 會初始化記憶體**

相較於 `malloc`，C++ 的 `new` 會初始化記憶體，這也是 `new` 和 `malloc` 的一個主要區別。`new` 分配記憶體時會初始化基本型別為 0，或呼叫類別的構造函式來初始化物件。

```cpp
int* arr = new int[100];  // 所有元素初始化為 0
```

### 2.6 差異總結

- **初始化**：`malloc` 不會初始化分配的記憶體，而 `new` 會初始化它。
- **型別安全**：`malloc` 返回 `void*`，需要手動轉型，而 `new` 是型別安全的，直接返回指定型別的指標。
- **建構/析構**：`new` 和 `delete` 支援建構函式和析構函式，適用於物件分配，而 `malloc` 和 `free` 只能用於 POD（Plain Old Data）型別。（POD在地5點有說明）
- **語法差異**：`malloc` 需要傳入記憶體大小，而 `new` 只需要指名資料型別。
- **初始化：**`malloc` 不會初始化記憶體，會存取到垃圾，而 `new` 會初始化記憶體，皆初始為0。

---

## 3. 堆疊 vs 堆的記憶體分配

### 3.1 堆疊（Stack）

- **堆疊分配**：堆疊是一個 LIFO（後進先出）結構，記憶體的分配和釋放速度極快。變數被宣告時，會自動分配在堆疊上，當程式離開作用域時，堆疊空間會自動釋放。這是一種高效的記憶體管理方式。
- **使用場景**：適合生命週期短且大小固定的變數，例如函式的局部變數。

```cpp
void function() {
    int arr[100];  // 堆疊分配，退出 function 時自動釋放
}

```

- **限制**：堆疊的大小通常有限（通常幾 MB），不適合分配大量資料。
- **記憶方法：**插隊（後進先出）的都去S → Stack

### 3.2 堆（Heap）

- **堆分配**：堆是用來進行動態記憶體分配的區域，大小靈活，但分配和釋放速度相對較慢。堆中的記憶體必須由程式設計師手動分配和釋放，否則可能造成記憶體洩漏。
- **使用場景**：適合生命週期長且大小不固定的資料，如動態陣列或大型資料結構。

```cpp
int* arr = (int*)malloc(100 * sizeof(int));  // 堆分配
free(arr);  // 手動釋放

```

### 3.3 堆疊 vs 堆

- **速度**：
    - 堆疊（Stack）
        - 堆疊是由作業系統管理的記憶體區域，用於儲存函式的局部變數和函式呼叫資訊。
        - 當你進入一個函式時，系統會自動為該函式分配記憶體，並在離開函式時自動釋放。堆疊是一個 LIFO（Last In First Out, 後進先出）的結構，記憶體分配與釋放只需調整一個堆疊指標，因此非常快。
    - 堆（Heap）
        - 堆則是一個用來進行動態記憶體分配的區域，分配與釋放記憶體時需要進行較為複雜的操作，如尋找適當大小的空閒區塊，以及在釋放記憶體時回收資源。
        - 堆的記憶體管理通常使用如分離鏈表（segregated list）或位圖（bitmap）這類資料結構來進行管理。這些操作會涉及多次記憶體尋址與檢查，因此比堆疊慢。
- **自動 vs 手動管理**：堆疊（Stack）中的記憶體由系統自動管理，而堆（Heap）中的記憶體需要手動管理，否則會出現記憶體洩漏或錯誤釋放。

---

## 4. `make_unique` 和 `unique_ptr`

`make_unique` 是 C++11 引入的用來創建 `unique_ptr` 的方式。`unique_ptr` 是一種智慧指標，它確保所管理的記憶體在不再使用時自動釋放，防止記憶體洩漏。

### 4.1 `make_unique` 使用的情況

`make_unique` 是為了避免手動管理記憶體分配，尤其是避免 `new` 分配後忘記釋放的情況。它可以自動呼叫析構函式來釋放記憶體，使用場景如下：

```cpp
auto ptr = std::make_unique<int[]>(100);  // 自動管理 100 個 int 的記憶體

```

### 4.2 為什麼 `make_unique` 會變慢？

`make_unique` 雖然在記憶體管理上方便，但由於其底層處理了額外的記憶體管理操作，如初始化和管理智慧指標，這些操作在頻繁的 2D 陣列分配中可能帶來較大的額外負擔。因此，當處理大量 2D 陣列時，這些管理操作可能會導致效能下降。

## 5. 什麼是 POD（Plain Old Data）？

POD 指的是一組簡單且兼容 C 的資料型別。POD 類型可以直接以二進位格式複製、移動或存儲，且沒有複雜的建構、析構或多態等功能。

POD 的特點：

1. 沒有建構函式或析構函式。
2. 沒有虛擬函式。
3. 沒有繼承。
4. 可以被直接初始化或使用 memcpy 進行複製。

例如，基本的 int、char、struct 沒有任何自定義函式的情況下都是 POD 類型。

```cpp
struct MyStruct {
    int x;
    char y;
};  // 這是一個 POD 類型
```

## **6. C++ 不同版本的差異與 C/C++ 的差異**

### **6.1 不同 C++ 版本的差異**

- **C++98/03**：最初的 C++ 標準，提供了基本的物件導向特性，支援 STL（標準模板庫）。
- **C++11**：
    - auto 型別推導：編譯器會根據初始化的表達式自動推導變數的型別。
    - nullptr：取代 NULL，作為空指標的語法。
    - Lambda 表達式：允許內聯定義匿名函數。
    - 智慧指標：std::unique_ptr 和 std::shared_ptr 提供安全的動態記憶體管理。
    - 移動語義（Move Semantics）：透過右值參考（&&）來優化資源移動，避免不必要的複製。
    
    ```cpp
    // 範例：auto 和 unique_ptr
    #include <iostream>
    #include <memory>
    
    int main() {
        // 使用 auto 來推導型別
        auto x = 42;  // x 自動推導為 int
        std::cout << x << std::endl;
    
        // 使用 unique_ptr 管理動態記憶體
        std::unique_ptr<int> ptr = std::make_unique<int>(100);
        std::cout << *ptr << std::endl;
    
        return 0;
    }
    ```
    
- **C++14**：
    - 泛型 Lambda 表達式：可以使用 auto 作為參數類型。
    - 二進位字面值：可以使用 0b 來表示二進位數字。
    
    ```cpp
    // 範例：泛型 Lambda
    #include <iostream>
    
    int main() {
        // 泛型 Lambda 使用 auto 作為參數
        auto add = [](auto a, auto b) {
            return a + b;
        };
        
        std::cout << add(1, 2) << std::endl;   // 輸出 3
        std::cout << add(1.5, 2.5) << std::endl; // 輸出 4.0
    
        return 0;
    }
    ```
    
- **C++17**：
    - std::optional：用於表示可能不存在的值，替代 null 或特殊值。
    - **結構化綁定（Structured Bindings）**：允許將結構（如 tuple 或 pair）解構成多個變數。
    - if constexpr：允許在編譯時進行條件判斷。
    - **折疊表達式（Fold Expressions）**：用於簡化範本參數包的展開。
    
    ```cpp
    // 範例：結構化綁定與 std::optional
    #include <iostream>
    #include <optional>
    #include <tuple>
    
    std::optional<int> get_value(bool condition) {
        if (condition) return 42;
        else return std::nullopt;  // 沒有值
    }
    
    int main() {
        // 使用 std::optional 處理可能不存在的值
        auto value = get_value(true);
        if (value) {
            std::cout << "Value: " << *value << std::endl;
        }
    
        // 使用結構化綁定來解構 tuple
        std::tuple<int, double, std::string> tup(1, 3.14, "example");
        auto [a, b, c] = tup;  // 解構 tuple 成三個變數
        std::cout << a << ", " << b << ", " << c << std::endl;
    
        return 0;
    }
    ```
    
- **C++20**：
    - **協程（Coroutines）**：支持非同步操作的高效處理。
    - concepts：用於約束範本參數的型別，使範本更具表達力和可讀性。
    - **範圍庫（Ranges Library）**：改進了容器與範圍的處理方式。
    - **模組（Modules）**：取代傳統的 #include 頭文件機制，提升編譯速度並減少重複依賴。
    
    ```cpp
    // 範例：concepts 和 ranges
    #include <iostream>
    #include <concepts>
    #include <ranges>
    #include <vector>
    
    // 使用 concepts 來限制範本參數
    template<typename T>
    requires std::integral<T>  // 只接受整數型別
    T add(T a, T b) {
        return a + b;
    }
    
    int main() {
        std::cout << add(1, 2) << std::endl;  // 正常，因為 1 和 2 是整數
        
        // C++20 的範圍庫 (ranges)
        std::vector<int> vec = {1, 2, 3, 4, 5};
        auto even = vec | std::ranges::views::filter([](int n) { return n % 2 == 0; });
        
        for (int i : even) {
            std::cout << i << " ";  // 輸出：2 4
        }
    
        return 0;
    }
    ```
    

### **6.2 C 和 C++ 的差異**

- **物件導向**：C++ 支援物件導向程式設計（封裝、繼承、多型），而 C 是純粹的程序式語言。
- **函式重載**：C++ 支援函式重載和運算子重載，C 不支援。
- **命名空間**：C++ 提供 namespace 用來防止命名衝突，C 沒有。
- **參考**：C++ 中有 引用（reference），這是一種指向變數的別名，C 沒有這樣的語法。
- **標準庫**：C++ 有 STL（標準模板庫），提供泛型的容器、演算法等，而 C 沒有標準的泛型容器。