# STL 容器

C++ 提供了豐富的標準模板庫（STL, Standard Templete Library），其中包含了各種容器，用於儲存和管理數據。在這篇教學中，我們將一步一步地深入了解以下容器：

- `vector`
- `stack`
- `queue`
- `priority_queue`
- `deque`
- `list`
- `set` 和 `unordered_set`
- `map` 和 `unordered_map`
- `string`

# Vector

## **介紹**

### 說明

`vector` 是一種**動態陣列**，可以根據需要自動調整大小。它在記憶體中**連續**儲存元素，支持快速的**隨機訪問**。當需要一個可以動態擴展且支持高效隨機訪問的容器時，`vector` 是首選。

### 內部結構

```cpp
template <typename T>
class vector {
private:
    T* data;          // 指向動態陣列的指針
    size_t size;      // 當前元素數量
    size_t capacity;  // 分配的內存容量

public:
    // 構造函數、析構函數、拷貝和移動操作等
    // ...
};
```

## **用法**

### 範例

```cpp
#include <vector>
#include <iostream>

int main() {
    // 宣告一個空的整數向量
    std::vector<int> vec;

    // 添加元素
    vec.push_back(1); // vec: [1]
    vec.push_back(2); // vec: [1, 2]
    vec.push_back(3); // vec: [1, 2, 3]

    // 訪問元素
    std::cout << "第一個元素: " << vec[0] << std::endl; // 輸出: 1

    // 修改元素
    vec[1] = 20; // vec: [1, 20, 3]

    // 遍歷元素
    std::cout << "向量中的元素: ";
    for (int i : vec) {
        std::cout << i << " "; // 輸出: 1 20 3
    }
    std::cout << std::endl;

    // 刪除最後一個元素
    vec.pop_back(); // vec: [1, 20]

    // 向量的大小
    std::cout << "向量的大小: " << vec.size() << std::endl; // 輸出: 2

    return 0;
}

```

### **程式碼說明**

- 使用 `push_back` 將元素添加到向量的尾部。
- 使用 `operator[]` 或 `at()` 訪問和修改元素。
- `pop_back` 刪除最後一個元素。
- `size()` 獲取向量的元素數量。

## **底層實現**

### 底層容器

- `vector` 在記憶體中使用**連續的記憶體區塊**儲存元素。這意味著所有元素的地址是連續的，這與傳統的陣列類似。
- **指標和動態記憶體**：`vector` 使用指標來管理其底層的動態記憶體。當需要擴容時，`vector` 會申請新的更大的記憶體區塊，然後將舊元素移動過去。
- **自動擴容**：當容量不足時，`vector` 會自動擴大容量，通常是原容量的兩倍。這是為了減少頻繁的記憶體重新配置。

### **為什麼使用指標**

- 指標允許 `vector` 動態管理記憶體，方便地在執行期調整容量。

### **操作時間複雜度**

- **訪問元素**：O(1)
    - **計算方式**：由於 `vector` 使用連續的記憶體，`vec[i]` 可以通過基址加上偏移量直接計算得到，因此是常數時間。
- **在尾部插入/刪除**：O(1) 均攤時間複雜度
    - **計算方式**：大多數情況下，`push_back` 和 `pop_back` 是常數時間操作。但當 `vector` 需要擴容時，`push_back` 可能需要 O(n) 的時間來分配新的記憶體並拷貝舊元素。然而，這種情況發生的頻率較低，故均攤時間為 O(1)。
- **記憶體不足時新增元素：**
    1. Allocate一塊兩倍大的記憶體
    2. 將舊的元素Copy到新記憶體中
    3. 釋放舊的記憶體
    4. 把新的元素push到後面
    
    整個過程中會花費很大量的時間，所以在不斷新增元素的狀況下，可以先Allocate好部分的記憶體（例如：reserve()），減少記憶體配置的時間。
    
- **在中間插入/刪除**：O(n)
    - **計算方式**：需要移動後續的元素，以保持連續性。

## **適用場景**

- **需要高效的隨機訪問**：如需要頻繁地通過索引訪問元素。
- **需要在尾部進行插入和刪除**：如實現堆疊（Stack）等。
- **元素數量動態變化**：不確定元素數量，需要容器自動調整大小。

---

# Stack

## **介紹**

### 說明

`stack` 是一種**後進先出（LIFO）**的容器，只能在一端進行插入和刪除。當需要管理一組按順序處理的任務，且最後加入的任務最先處理時，`stack` 是合適的選擇。（

可以記，誰插隊誰就S(stack)，後進來卻先出去

或是記，只能從同一邊存取資料。

### 結構

```cpp
template <class T, class Container = deque<T>>
class stack {
protected:
    Container c; // 底層容器

public:
    // 建構函式、解構函式等

    bool empty() const;
    size_t size() const;
    T& top();
    const T& top() const;
    void push(const T& value);
    void pop();
    // 其他可能的成員函式
};
```

## **用法**

### 範例

```cpp
#include <stack>
#include <iostream>

int main() {
    // 宣告一個整數堆疊
    std::stack<int> stk;
    
    stk.empty() // true

    // 添加元素
    stk.push(1); // stk: [1]
    stk.push(2); // stk: [1, 2]
    stk.push(3); // stk: [1, 2, 3]

    // 訪問頂部元素
    std::cout << "頂部元素: " << stk.top() << std::endl; // 輸出: 3

    // 移除頂部元素
    stk.pop(); // stk: [1, 2]

    // 檢查堆疊是否為空
    if (!stk.empty()) {
        std::cout << "堆疊不為空，大小為: " << stk.size() << std::endl; // 輸出: 2
    }

    return 0;
}

```

### **程式碼解釋**

- 使用 `push` 將元素添加到堆疊的頂部。
- 使用 `top` 訪問頂部元素。
- 使用 `pop` 移除頂部元素。
- 使用 `empty` 和 `size` 檢查堆疊狀態。

## **底層實現**

### **底層容器**

- `stack` 是一種**配接器容器（container adaptor）**，它使用其他容器（預設為 `deque`）作為底層實現。
    - container adaptor：
        - 是建立於現有的容器，來去實作出特殊目的功能的容器。
        - 例如：`stack`是現有容器`deque`或是`vector`實作出來的。
- 可以指定底層容器，如 `std::stack<int, std::vector<int>>`。

### **為什麼是container adapter**

- 這允許 `stack` 的行為與底層容器無關，只提供特定的功能和操作函式。
- 例如：`stack`使用`vector`實作，但是`stack`是不能取用`vector`的成員函式，只能用自己的成員函式。

### **操作時間複雜度**

- **push/pop/top**：O(1)
    - **計算方式**：這些操作只涉及到容器的一端，不需要遍歷或移動其他元素，因此是常數時間。

## **適用場景**

- **表達式計算**：如中序轉後序、括號匹配等。
- **深度優先搜尋（DFS）**：使用堆疊來記錄路徑。
- **回溯算法**：保存狀態進行回溯。

---

# Queue

## **介紹**

### 說明

`queue` 是一種**先進先出（FIFO）**的容器，只能在一端插入，另一端刪除。當需要按順序處理任務，且先到的任務先處理時，`queue` 是理想的選擇。
可以記，一段存資料一端取資料

或是記，誰插隊誰就S(stack)，反之就是q

### 結構

```cpp
#include <deque>
#include <list>
#include <vector>

template <class T, class Container = deque<T>>
class queue {
protected:
    Container c; // 底層容器

public:
    // 建構函式、解構函式等

    bool empty() const;
    size_t size() const;
    T& front();
    const T& front() const;
    T& back();
    const T& back() const;
    void push(const T& value);
    void pop();
    // 其他可能的成員函式
};
```

## **用法**

### 範例

```cpp
#include <queue>
#include <iostream>

int main() {
    // 宣告一個整數佇列
    std::queue<int> q;

    // 添加元素
    q.push(1); // q: [1]
    q.push(2); // q: [1, 2]
    q.push(3); // q: [1, 2, 3]

    // 訪問前端和後端元素
    std::cout << "前端元素: " << q.front() << std::endl; // 輸出: 1
    std::cout << "後端元素: " << q.back() << std::endl;  // 輸出: 3

    // 移除前端元素
    q.pop(); // q: [2, 3]

    // 佇列的大小
    std::cout << "佇列的大小: " << q.size() << std::endl; // 輸出: 2

    return 0;
}

```

### **程式碼解釋**

- 使用 `push` 將元素添加到佇列的尾部。
- 使用 `front` 和 `back` 訪問前端和後端元素。
- 使用 `pop` 移除前端元素。

## **底層實現**

### 底層容器

- `queue` 也是一種**配接器容器**，預設使用 `deque` 作為底層容器。

### **為什麼使用 `deque`**

- `deque` 支持在兩端高效地插入和刪除，適合 `queue` 的需求。

### 指定底層容器

- 可以指定`vector`或是`list`
- 例如：
    
    ```cpp
    // 指定使用 vector 作為底層容器（不推薦）
        std::queue<int, std::vector<int>> q2;
        q2.push(4);
        q2.push(5);
        q2.push(6);
        std::cout << "q2 front: " << q2.front() << ", back: " << q2.back() << std::endl;
    
        // 指定使用 list 作為底層容器
        std::queue<int, std::list<int>> q3;
        q3.push(7);
        q3.push(8);
        q3.push(9);
        std::cout << "q3 front: " << q3.front() << ", back: " << q3.back() << std::endl;
    ```
    
    - `queue` 需要在頭部進行移除操作，`vector` 的效率較低
    - `deque` 和 `list` 更適合作為 `queue` 的底層容器。

## **操作時間複雜度**

- **push/pop/front/back**：O(1)
    - **計算方式**：這些操作只涉及到容器的一端，不需要遍歷或移動其他元素。

## **適用場景**

- **廣度優先搜尋（BFS）**：使用佇列來記錄待訪問的節點。
- **任務調度**：按順序處理請求或任務。
- **緩衝區實現**：如生產者-消費者模型。

---

# Priority Queue

## **介紹**

### 說明

`priority_queue` 是一種特殊的佇列，每次取出的元素都是**優先級最高**的元素。當需要按優先級處理任務時，`priority_queue` 非常有用。

### 結構

```cpp
#include <vector>
#include <functional>
#include <queue>

template <class T, class Container = std::vector<T>, class Compare = std::less<typename Container::value_type>>
class priority_queue {
protected:
    Container c;        // 底層容器
    Compare comp;       // 比較函式

public:
    // 建構函式、解構函式等

    bool empty() const;
    size_t size() const;
    const T& top() const;
    void push(const T& value);
    void pop();
    // 其他可能的成員函式
};
```

## **用法**

### 範例

```cpp
#include <queue>
#include <vector>
#include <iostream>

int main() {
    // 宣告一個整數優先佇列，預設為最大堆
    std::priority_queue<int> pq;

    // 添加元素
    pq.push(5);  // pq: [5]
    pq.push(1);  // pq: [5, 1]
    pq.push(10); // pq: [10, 1, 5]

    // 訪問頂部元素（最大值）
    std::cout << "頂部元素: " << pq.top() << std::endl; // 輸出: 10

    // 移除頂部元素
    pq.pop(); // pq: [5, 1]

    // 檢查大小
    std::cout << "優先佇列的大小: " << pq.size() << std::endl; // 輸出: 2

    return 0;
}

```

### **程式碼解釋**

- 使用 `push` 添加元素，會自動按優先級排序。
- 使用 `top` 訪問優先級最高的元素。
- 使用 `pop` 移除優先級最高的元素。

## **底層實現**

### 底層容器

- **堆（heap）實現**：`priority_queue` 使用`heap`，通常是`Max-heap`（頭是值最大的）。
- **底層容器**：
    - 預設使用 `vector` 作為底層容器，因為 `vector` 支持隨機訪問。
    - 使用`vector`實現`heap`，在用`heap`實現`prority_queue`。

### **為什麼使用堆**

- 堆可以在 O(log n) 的時間內插入和刪除元素，同時保持heap的性質。

### **操作時間複雜度**

- **push**：O(log n)
    - **計算方式**：插入元素後，需要上浮調整堆，最壞情況需要調整 log n 層。
- **pop/top**：O(log n)（pop），O(1)（top）
    - **計算方式**：`top` 直接訪問堆頂元素，為常數時間。`pop` 需要移除堆頂，並下沉調整堆，最壞情況需要調整 log n 層。

## **適用場景**

- **任務調度**：根據優先級處理任務。
- **最短路徑算法**：如 Dijkstra 算法使用優先佇列來選擇最小距離的節點。
- **資料壓縮**：如 Huffman 編碼需要頻繁取出最小權值的節點。

---

# Deque

## **介紹**

### 說明

`deque`（Double-Ended Queue）允許在**兩端**進行高效的插入和刪除，同時支持隨機訪問。當需要在兩端頻繁地插入和刪除，且需要隨機訪問時，`deque` 是合適的選擇。

### 結構

```cpp
template <class T, class Allocator = std::allocator<T>>
class deque {
protected:
    // 指向指標陣列（map）的指標
    T** map; 
    size_t map_size;      // map 的大小（指標數量）
    size_t start_block;   // 起始區塊索引
    size_t start_offset;  // 起始區塊內的偏移量
    size_t finish_block;  // 結束區塊索引
    size_t finish_offset; // 結束區塊內的偏移量
    size_t num_elements;  // 元素總數

    // 每個區塊的大小（通常是固定的，例如 512 個元素）
    static const size_t block_size = 512;

public:
    // 建構函式、解構函式等

    // 主要操作函式
    void push_back(const T& value);
    void push_front(const T& value);
    void pop_back();
    void pop_front();
    T& operator[](size_t index);
    const T& operator[](size_t index) const;
    size_t size() const;
    bool empty() const;
    // 其他可能的成員函式
};
```

- **指標陣列（map）**：`map` 是一個指向指標的陣列，每個指標指向一個固定大小的區塊（block）。這種結構允許 `deque` 在兩端進行高效的擴展，而不需要移動整個容器的元素。
- **區塊（block）**：每個區塊包含固定數量的元素（例如 512 個），這些區塊可以獨立分配，減少頻繁的記憶體重新分配。
- **起始和結束指標**：`start_block` 和 `finish_block` 分別指向第一個和最後一個區塊，`start_offset` 和 `finish_offset` 指示在這些區塊內的具體位置。
- **元素數量**：`num_elements` 跟蹤 `deque` 中的元素總數。

## **用法**

```cpp
#include <deque>
#include <iostream>

int main() {
    // 宣告一個整數雙端佇列
    std::deque<int> dq;

    // 在前端添加元素
    dq.push_front(1); // dq: [1]

    // 在後端添加元素
    dq.push_back(2);  // dq: [1, 2]

    // 在前端插入元素
    dq.push_front(0); // dq: [0, 1, 2]

    // 訪問元素
    std::cout << "第一個元素: " << dq.front() << std::endl; // 輸出: 0
    std::cout << "最後一個元素: " << dq.back() << std::endl;  // 輸出: 2

    // 隨機訪問
    std::cout << "中間元素: " << dq[1] << std::endl; // 輸出: 1

    // 移除前端和後端元素
    dq.pop_front(); // dq: [1, 2]
    dq.pop_back();  // dq: [1]

    // 雙端佇列的大小
    std::cout << "雙端佇列的大小: " << dq.size() << std::endl; // 輸出: 1

    return 0;
}

```

### **程式碼解釋**

- 使用 `push_front` 和 `push_back` 在兩端添加元素。
- 使用 `pop_front` 和 `pop_back` 移除兩端元素。
- 使用 `operator[]` 進行隨機訪問。

## **底層實現**

### 底層容器

- `deque` 使用一系列的小型連續記憶體塊（稱為緩衝區或區塊）組成一個雙向鏈表。
- **為什麼不用單一連續記憶體**：為了避免因為頻繁的插入刪除導致大量的元素移動或記憶體重新配置。
- **指標陣列**：`deque` 使用一個指標陣列來管理這些記憶體塊，從而支持常數時間的兩端插入刪除和隨機訪問。

### **操作時間複雜度**

- **push_front/push_back/pop_front/pop_back**：O(1)
    - **計算方式**：操作只涉及到兩端，不需要移動其他元素。
- **隨機訪問**：O(1)
    - **計算方式**：通過計算元素所在的緩衝區和在緩衝區內的偏移量，可以在常數時間內訪問元素。
    - 可以使用運算子 `[]` 或 `at` 進行元素的隨機存取。

## **適用場景**

- **滑動窗口**：需要在兩端進行插入刪除的情況。
- **實現複雜的佇列結構**：如雙向佇列。
- **需要隨機訪問和兩端操作**：如回溯算法中的狀態管理。

---

# List

## **介紹**

### 說明

`list` 是一種**雙向連結串列（double linked list）**，允許在任意位置高效地插入和刪除元素。但不支持隨機訪問。當需要頻繁在中間位置進行插入和刪除，且不需要隨機訪問時，`list` 是理想的選擇。

### 結構

```cpp
template <class T, class Allocator = std::allocator<T>>
class list {
public:
    // 內部節點結構
    struct Node {
        T data;
        Node* prev;
        Node* next;
    };

protected:
    Node* head; // 頭節點（可能是虛擬節點）
    Node* tail; // 尾節點（可能是虛擬節點）
    size_t size_;

public:
    // 建構函式、解構函式等

    // 主要操作函式
    void push_back(const T& value);
    void push_front(const T& value);
    void pop_back();
    void pop_front();
    iterator insert(iterator pos, const T& value);
    iterator erase(iterator pos);
    // 其他可能的成員函式
};

```

## **用法**

### 範例

```cpp
#include <list>
#include <iostream>

int main() {
    // 宣告一個整數串列
    std::list<int> lst;

    // 在尾部添加元素
    lst.push_back(1); // lst: [1]

    // 在頭部添加元素
    lst.push_front(2); // lst: [2, 1]

    // 插入元素
    auto it = lst.begin();
    ++it; // 指向第二個元素
    lst.insert(it, 3); // lst: [2, 3, 1]

    // 遍歷元素
    std::cout << "串列中的元素: ";
    for (int i : lst) {
        std::cout << i << " "; // 輸出: 2 3 1
    }
    std::cout << std::endl;

    // 刪除元素
    lst.erase(it); // lst: [2, 1]

    // 串列的大小
    std::cout << "串列的大小: " << lst.size() << std::endl; // 輸出: 2

    return 0;
}

```

### **程式碼解釋**

- 使用 `push_back` 和 `push_front` 添加元素。
- 使用迭代器進行插入和刪除操作。
- `insert` 和 `erase` 操作時間複雜度為 O(1)。

## **底層實現**

### 底層容器

- `list` 是**雙向連結串列**，每個節點包含元素值，以及指向前一個和後一個節點的指標。
- **為什麼使用連結串列**：允許在常數時間內在任意位置插入和刪除，而不需要移動其他元素。

### **操作時間複雜度**

- **插入/刪除**：O(1)（在已知位置）
    - **計算方式**：只需要修改相鄰節點的指標。
- **訪問元素**：O(n)
    - **計算方式**：需要從頭或尾開始遍歷，直到找到目標元素。

## **適用場景**

- **頻繁的插入刪除**：如實現 LRU 緩存。
- **不需要隨機訪問**：僅需順序遍歷。
- **需要穩定的迭代器**：元素的地址不會因為其他操作而改變。

---

# Set 和 Unordered_Set

# Set

## **介紹**

### 說明

`set` 是一種**有序**且**不允許重複元素**的容器。當需要存儲不重複的元素，並且需要自動排序時，`set` 是合適的選擇。

### 結構

```cpp
template <class Key, class Compare = std::less<Key>, class Allocator = std::allocator<Key>>
class set {
protected:
    // 底層的平衡二元搜尋樹結構
    // 例如，紅黑樹的節點結構

public:
    // 主要操作函式
    std::pair<iterator, bool> insert(const Key& key);
    void erase(iterator pos);
    iterator find(const Key& key);
    size_t count(const Key& key) const;
    // 其他可能的成員函式
};

```

- **比較函式 (`Compare`)**：預設為 `std::less<Key>`，決定元素的有序性。
- **底層樹結構**：通常為紅黑樹，確保樹的平衡性和操作效率。

## **用法**

### 範例

```cpp
#include <set>
#include <iostream>

int main() {
    // 宣告一個整數集合
    std::set<int> s;

    // 添加元素
    s.insert(3); // s: [3]
    s.insert(1); // s: [1, 3]
    s.insert(2); // s: [1, 2, 3]

    // 嘗試插入重複元素
    auto result = s.insert(2); // 插入失敗，因為元素已存在

    if (!result.second) {
        std::cout << "元素 2 已存在於集合中。" << std::endl;
    }

    // 遍歷元素（自動排序）
    std::cout << "集合中的元素: ";
    for (int i : s) {
        std::cout << i << " "; // 輸出: 1 2 3
    }
    std::cout << std::endl;

    // 查找元素
    if (s.find(3) != s.end()) {
        std::cout << "元素 3 存在於集合中。" << std::endl;
    }

    return 0;
}

```

### **程式碼解釋**

- 使用 `insert` 添加元素，重複的元素會被忽略。
- 使用 `find` 查找元素是否存在。
- 集合中的元素自動按照順序排列。

## **底層實現**

### 底層容器

- **紅黑樹**：`set` 通常使用**自平衡二叉搜尋樹**（如紅黑樹）實現。
- **為什麼使用紅黑樹**：紅黑樹可以在 O(log n) 的時間內進行插入、刪除和查找，同時保持元素的有序性。

### **操作時間複雜度**

- **插入/刪除/查找**：O(log n)
    - **計算方式**：在紅黑樹中，這些操作需要經過樹的高度，紅黑樹的高度為 log n。

### **適用場景**

- **需要自動排序**：如需要按順序處理數據。
- **需要快速查找是否存在**：如判斷元素是否已出現過。
- **不允許重複元素**：如集合運算。

---

# Unordered_Set

## **介紹**

### 說明

`unordered_set` 是一種**無序**且**不允許重複元素**的容器。當需要快速查找，但不需要排序時，`unordered_set` 是理想的選擇。

### 結構

```cpp
template <class Key, class Hash = std::hash<Key>, class KeyEqual = std::equal_to<Key>,
          class Allocator = std::allocator<Key>>
class unordered_set {
protected:
    // 底層的哈希表結構
    // 包含桶（buckets）、元素鏈結等

public:
    // 主要操作函式
    std::pair<iterator, bool> insert(const Key& key);
    void erase(iterator pos);
    iterator find(const Key& key);
    size_t count(const Key& key) const;
    // 其他可能的成員函式
};

```

- **哈希函式 (`Hash`)**：預設為 `std::hash<Key>`，負責將鍵值轉換為哈希碼。
- **鍵比較函式 (`KeyEqual`)**：預設為 `std::equal_to<Key>`，用於比較鍵值的相等性。
- **桶（Buckets）**：哈希表中的桶數量會動態調整以維持負載因子（load factor）的合理範圍。

## **用法**

### 範例

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    // 宣告一個整數無序集合
    std::unordered_set<int> us;

    // 添加元素
    us.insert(3);
    us.insert(1);
    us.insert(2);

    // 遍歷元素（無序）
    std::cout << "無序集合中的元素: ";
    for (int i : us) {
        std::cout << i << " "; // 輸出順序不定
    }
    std::cout << std::endl;

    // 查找元素
    if (us.find(2) != us.end()) {
        std::cout << "元素 2 存在於無序集合中。" << std::endl;
    }

    return 0;
}

```

## **程式碼解釋**

- 使用 `insert` 添加元素。
- 使用 `find` 查找元素是否存在。
- 元素的存儲順序不定，因為是無序的。

## **底層實現**

### 底層容器

- **雜湊表（Hash Table）**：`unordered_set` 使用雜湊表實現。
- **哈希函數**：通過哈希函數將元素映射到不同的桶（bucket）中。
- **碰撞處理**：當多個元素映射到同一個桶時，使用鏈結串列（或其他方法）來儲存這些元素。
    - **鏈結串列法**：每個桶是一個鏈結串列，碰撞的元素被添加到鏈結串列中。
    - **開放定址法**：在碰撞時尋找下一個空位。

### **操作時間複雜度**

- **插入/刪除/查找**：O(1) 平均，O(n) 最壞情況
    - **計算方式**：在負載因子合理的情況下，哈希表的操作時間為常數。但當碰撞嚴重或負載因子過高時，時間複雜度會退化。

## **適用場景**

- **需要高效的查找**：如需要頻繁地判斷元素是否存在。
- **不需要排序**：元素的順序無關緊要。
- **處理大量數據**：哈希表在處理大量數據時效率高。

---

# Map 和 Unordered_Map

# Map

## **介紹**

### 說明

`map` 是一種**關聯式容器**，存儲**鍵值對**，鍵是唯一的，並自動排序。當需要按照鍵排序，並快速查找值時，`map` 是合適的選擇。

### 結構

```cpp
template <class Key, class T, class Compare = std::less<Key>, class Allocator = std::allocator<std::pair<const Key, T>>>
class map {
protected:
    // 底層的平衡二元搜尋樹結構
    // 例如，紅黑樹的節點結構，存儲鍵值對

public:
    // 主要操作函式
    std::pair<iterator, bool> insert(const std::pair<const Key, T>& value);
    void erase(iterator pos);
    iterator find(const Key& key);
    size_t count(const Key& key) const;
    // 其他可能的成員函式
};
```

- **鍵比較函式 (`Compare`)**：預設為 `std::less<Key>`，決定鍵的有序性。
- **底層樹結構**：通常為紅黑樹，確保樹的平衡性和操作效率。

## **用法**

### 範例

```cpp
#include <map>
#include <iostream>

int main() {
    // 宣告一個字串到整數的映射
    std::map<std::string, int> m;

    // 添加元素
    m["apple"] = 3;
    m["banana"] = 5;
    m.insert({"orange", 2});

    // 訪問元素
    std::cout << "蘋果的數量: " << m["apple"] << std::endl; // 輸出: 3

    // 遍歷元素（按照鍵排序）
    std::cout << "水果列表:" << std::endl;
    for (const auto& pair : m) {
        std::cout << pair.first << ": " << pair.second << std::endl;
        // 輸出順序: apple, banana, orange
    }

    // 查找元素
    auto it = m.find("banana");
    if (it != m.end()) {
        std::cout << "找到香蕉，數量為: " << it->second << std::endl; // 輸出: 5
    }

    return 0;
}

```

### **程式碼解釋**

- 使用 `operator[]` 和 `insert` 添加元素。
- 使用 `find` 查找鍵是否存在。
- 遍歷時，元素按照鍵的順序排列。

## **底層實現**

### 底層容器

- **紅黑樹**：`map` 使用紅黑樹（自平衡二叉搜尋樹）實現。
- **為什麼使用紅黑樹**：保持鍵的有序性，並在 O(log n) 的時間內進行操作。

### **操作時間複雜度**

- **插入/刪除/查找**：O(log n)
    - **計算方式**：操作需要經過樹的高度，紅黑樹的高度為 log n。

## **適用場景**

- **需要按鍵排序**：如需要以有序方式處理鍵值對。
- **需要快速查找值**：通過鍵快速獲取對應的值。
- **鍵的範圍較小且需要排序**：如統計字頻並按照字母順序輸出。

---

# Unordered_Map

## **介紹**

### 說明

`unordered_map` 是一種**無序**的關聯式容器，使用雜湊表存儲鍵值對。當需要快速查找值，但不需要鍵的排序時，`unordered_map` 是理想的選擇。

### 結構

```cpp
template <class Key, class T, class Hash = std::hash<Key>, class KeyEqual = std::equal_to<Key>,
          class Allocator = std::allocator<std::pair<const Key, T>>>
class unordered_map {
protected:
    // 底層的哈希表結構
    // 包含桶（buckets）、元素鏈結等

public:
    // 主要操作函式
    std::pair<iterator, bool> insert(const std::pair<const Key, T>& value);
    void erase(iterator pos);
    iterator find(const Key& key);
    size_t count(const Key& key) const;
    // 其他可能的成員函式
};

```

- **哈希函式 (`Hash`)**：預設為 `std::hash<Key>`，負責將鍵值轉換為哈希碼。
- **鍵比較函式 (`KeyEqual`)**：預設為 `std::equal_to<Key>`，用於比較鍵值的相等性。
- **桶（Buckets）**：哈希表中的桶數量會動態調整以維持負載因子（load factor）的合理範圍。

## **用法**

```cpp
#include <unordered_map>
#include <iostream>

int main() {
    // 宣告一個字串到整數的無序映射
    std::unordered_map<std::string, int> um;

    // 添加元素
    um["apple"] = 3;
    um["banana"] = 5;
    um.insert({"orange", 2});

    // 訪問元素
    std::cout << "蘋果的數量: " << um["apple"] << std::endl; // 輸出: 3

    // 遍歷元素（無序）
    std::cout << "水果列表:" << std::endl;
    for (const auto& pair : um) {
        std::cout << pair.first << ": " << pair.second << std::endl;
        // 輸出順序不定
    }

    // 查找元素
    auto it = um.find("banana");
    if (it != um.end()) {
        std::cout << "找到香蕉，數量為: " << it->second << std::endl; // 輸出: 5
    }

    return 0;
}

```

### **程式碼解釋**

- 使用 `operator[]` 和 `insert` 添加元素。
- 使用 `find` 查找鍵是否存在。
- 元素的存儲順序不定。

## **底層實現**

### 底層容器

- **雜湊表（Hash Table）**：`unordered_map` 使用哈希表實現。
- **哈希函數**：將鍵通過哈希函數映射到桶中。
- **碰撞處理**：與 `unordered_set` 相同，使用鏈結串列或其他方法。
    - **鏈結串列法**：每個桶是一個鏈結串列，碰撞的元素被添加到鏈結串列中。
    - **開放定址法**：在碰撞時尋找下一個空位。
- **為什麼使用哈希表**：在平均情況下，可以在常數時間內進行插入、刪除和查找。

### **操作時間複雜度**

- **插入/刪除/查找**：O(1) 平均，O(n) 最壞情況
    - **計算方式**：取決於哈希函數的效率和負載因子。

## **適用場景**

- **需要高效的鍵值查找**：如計算字頻、統計數據。
- **不需要鍵的排序**：僅關心鍵與值的映射關係。
- **處理大量數據**：哈希表在處理大規模數據時效率高。

---

# String

## **介紹**

### 說明

`string` 是專門用於處理字串的容器，通常基於 **動態陣列（Dynamic Array）** 的實作，實際上是一種特殊的 `vector<char>`（總而言之，就是一種專屬於char的vector）。當需要處理文本、字符序列時，`string` 是首選。

這個專屬於char的vector，會依照char和string的特性做特別優化，提高性能和效率。

### 結構

```cpp
template <class CharT, class Traits = std::char_traits<CharT>, class Allocator = std::allocator<CharT>>
class basic_string {
protected:
    CharT* data_;        // 指向字符陣列的指標
    size_t size_;        // 當前字符串的長度
    size_t capacity_;    // 分配的內存容量

public:
    // 建構函式、解構函式等

    size_t length() const;
    size_t size() const;
    void push_back(CharT c);
    void pop_back();
    basic_string& append(const basic_string& str);
    // 其他可能的成員函式
};
```

## **用法**

### 範例

```cpp
#include <string>
#include <iostream>

int main() {
    // 宣告一個字串
    std::string str = "Hello";

    // 拼接字串
    str += " World"; // str: "Hello World"

    // 訪問字符
    std::cout << "第一個字符: " << str[0] << std::endl; // 輸出: H

    // 修改字符
    str[6] = 'w'; // str: "Hello world"

    // 子字串
    std::string substr = str.substr(0, 5); // substr: "Hello"

    // 查找字符或子字串
    size_t pos = str.find("world");
    if (pos != std::string::npos) {
        std::cout << "找到 'world'，位置在: " << pos << std::endl; // 輸出: 6
    }

    // 字串長度
    std::cout << "字串的長度: " << str.length() << std::endl; // 輸出: 11

    return 0;
}

```

### **程式碼解釋**

- 使用 `+` 或 `+=` 拼接字串。
- 使用 `operator[]` 訪問和修改字符。
- 使用 `substr` 獲取子字串。
- 使用 `find` 查找子字串的位置。

## **底層實現**

### 底層容器

- **連續的字符陣列**：`string` 使用連續的字符陣列儲存字元，類似於 `vector<char>`。
- **動態記憶體管理**：使用指標和動態記憶體來管理字串的容量和大小。
- **小字串優化（SSO）**：為了提高效率，某些實作會對短字串進行優化，將其儲存在內部緩衝區中，避免動態記憶體配置。

### **操作時間複雜度**

- **訪問字符**：O(1)
    - **計算方式**：連續記憶體，通過索引直接訪問。
- **拼接**：O(n)
    - **計算方式**：需要拷貝原有字串和新字串。
- **查找**：O(n)
    - **計算方式**：需要遍歷字串來查找匹配。

## **適用場景**

- **文本處理**：如讀取、修改、分析文本。
- **字符串操作**：如拼接、分割、替換。
- **輸入輸出**：與用戶進行字串交互。

---

# 容器之間的比較

## **功能比較**

| 容器 | 有序性 | 重複元素 | 底層實現 | 隨機訪問 |
| --- | --- | --- | --- | --- |
| `vector` | 是 | 允許 | 動態陣列（連續記憶體） | 是 |
| `deque` | 是 | 允許 | 分段連續記憶體 | 是 |
| `list` | 是 | 允許 | 雙向連結串列 | 否 |
| `set` | 是 | 不允許 | 紅黑樹 | 否 |
| `unordered_set` | 否 | 不允許 | 雜湊表 | 否 |
| `map` | 是 | 鍵不允許 | 紅黑樹 | 否 |
| `unordered_map` | 否 | 鍵不允許 | 雜湊表 | 否 |
| `stack` | N/A | 允許 | 預設使用 `deque` | 否 |
| `queue` | N/A | 允許 | 預設使用 `deque` | 否 |
| `priority_queue` | N/A | 允許 | Heap （使用 `vector`） | 否 |
| `string` | 是 | 允許 | 動態字符陣列 | 是 |

## **時間複雜度比較**

| 操作 | `vector` | `deque` | `list` | `set` | `unordered_set` | `map` | `unordered_map` |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 插入（尾部） | O(1)* | O(1) | O(1) | O(log n) | O(1)* | O(log n) | O(1)* |
| 插入（中間） | O(n) | O(n) | O(1) | O(log n) | O(1)* | O(log n) | O(1)* |
| 刪除（尾部） | O(1) | O(1) | O(1) | O(log n) | O(1)* | O(log n) | O(1)* |
| 刪除（中間） | O(n) | O(n) | O(1) | O(log n) | O(1)* | O(log n) | O(1)* |
| 訪問（隨機） | O(1) | O(1) | O(n) | O(n) | O(n) | O(n) | O(n) |
| 查找 | O(n) | O(n) | O(n) | O(log n) | O(1)* | O(log n) | O(1)* |

均攤時間複雜度；在最壞情況下，操作時間可能較高。

# 結論

- **`vector`**：
    - 適用於需要高效隨機訪問和在尾部進行插入刪除的情況。如動態數組、需要頻繁讀取或更新特定位置的元素。
- **`list`**：
    - 適用於需要在任意位置頻繁插入刪除，但不需要隨機訪問的情況。如實現 LRU 緩存、任務隊列。
- **`deque`**：
    - 適用於需要在兩端頻繁插入刪除，且需要隨機訪問的情況。如實現雙向佇列、滑動窗口。
- **`set`** 和 **`map`**：
    - 兩個都是用**紅黑數**實作，才有自動排序和快速查找的功能。
    - 適用於需要自動排序和快速查找的情況。
    - 如統計排序後的數據、需要有序遍歷鍵值對。
    - **`set`**單純儲存元素，**`map`**儲存key和value。
- **`unordered_set`** 和 **`unordered_map`**：
    - 適用於需要高效查找，但不需要排序的情況。如統計元素頻率、快速檢查存在性。
    - **`unordered_set`**單純儲存元素，**`unordered_map`**儲存key和value。
- **`stack`** 和 **`queue`**：
    - 兩個都是Container Adapter。
    - `stack`用`std::vector`或`std::deque`實作（針對其中一端做操作），`queue`用`std::deque`實作（針對兩端做操作）。
    - 適用於先進後出（LIFO）或先進先出（FIFO）的場景。
    - 例如BFS (queue)和DFS (stack)。
- **`priority_queue`**：
    - 適用於需要按優先級處理任務的情況。如任務調度、資料壓縮。
- **`string`**：
    - 適用於處理文本和字符序列。如文本編輯、輸入輸出處理。