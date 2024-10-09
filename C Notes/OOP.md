# 物件導向程式設計 (OOP)

### OOP 的基本概念（封裝、繼承、多態、抽象）

1. **封裝（Encapsulation）**：
    - **定義**：將數據和操作數據的方法封裝在一起，隱藏內部細節。
    - **實現**：使用類和訪問修飾符（private, protected, public）。
2. **繼承（Inheritance）**：
    - **定義**：新類別從已有類別獲得屬性和方法，實現代碼重用。
    - **實現**：使用 `:` 和繼承方式（public, protected, private）。
3. **多態（Polymorphism）**：
    - **定義**：同一接口，不同實現。
    - **實現**：虛函數、函數重載、運算符重載。
4. **抽象（Abstraction）**：
    - **定義**：關注對象的必要特性，忽略細節。
    - **實現**：純虛函數、抽象類。

### 與程序導向的區別

- **物件導向**：
    - 強調對象和類的概念。
    - 提高代碼的可維護性和可重用性。
- **程序導向**：
    - 強調過程和函數。
    - 更適合小型、簡單的程序。

### 底層實作與範例

- **虛函數表（V-Table）**：
    - 編譯器為含有虛函數的類創建虛函數表。
    - 對象包含指向虛函數表的指標，實現動態綁定。
- **範例**：
    
    ```cpp
    class Base {
    public:
        virtual void func() { std::cout << "Base" << std::endl; }
    };
    
    class Derived : public Base {
    public:
        void func() override { std::cout << "Derived" << std::endl; }
    };
    
    Base* obj = new Derived();
    obj->func(); // 輸出 "Derived"
    
    ```