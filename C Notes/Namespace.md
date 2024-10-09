# Namespace

### Namespace 的定義與作用

- **定義**：`namespace` 用於創建命名空間，將標識符（類、函數、變量等）組織在一起，防止命名衝突。
- **作用**：
    - 組織代碼，增強可讀性。
    - 避免不同庫或模塊之間的命名衝突。

### 如何避免命名衝突

- **使用命名空間**：
    - 將代碼包裹在 `namespace` 中。
    - 使用時指定命名空間。
    - **範例**：
        
        ```cpp
        namespace Graphics {
            void draw() { /*...*/ }
        }
        
        namespace Physics {
            void draw() { /*...*/ }
        }
        
        Graphics::draw();
        Physics::draw();
        
        ```
        
- **匿名命名空間**：
    - 用於限制作用域在文件內，相當於 `static` 作用。
    - **範例**：
        
        ```cpp
        namespace {
            int internalVar = 0;
        }
        
        ```
        

### 底層實作與範例

- **底層實作**：
    - 編譯器通過名稱修飾（Name Mangling）將命名空間信息添加到符號中，實現區分。
- **範例**：
    
    ```cpp
    namespace MyNamespace {
        class MyClass {
        public:
            void myFunction();
        };
    }
    
    void MyNamespace::MyClass::myFunction() {
        // 函數實現
    }
    
    ```