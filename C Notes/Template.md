# Template

# Template 的底層實作與使用

**Template** 使得函數和類別可以在編譯時根據具體的資料類型生成相應的代碼。這種泛型編程的方式提高了代碼的重用性和靈活性。

- **函數模板**：
    
    ```cpp
    template <typename T>
    T add(T a, T b) {
        return a + b;
    }
    
    ```
    
    - **使用情境**：需要對不同資料類型執行相同操作時，如數字的加法。
- **類模板**：
    
    ```cpp
    template <typename T>
    class MyClass {
    public:
        MyClass(T value) : data(value) {}
        T getData() const { return data; }
    private:
        T data;
    };
    
    ```
    
    - **使用情境**：當需要創建能處理多種資料類型的類別時，例如通用的容器或資料結構。
    - **底層實作**：編譯器在編譯期間會根據實際使用的類型生成對應的類別代碼。