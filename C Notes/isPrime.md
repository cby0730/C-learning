# 質數判斷

### 質數的定義與判斷方法

- **定義**：大於1，且只有1和自身兩個正因數的自然數。
- **判斷方法**：
    - **簡單方法**：檢查從2到 n-1 是否有因數。
    - **優化方法**：檢查到平方根就好，只需檢查可能的質數因數。

### 高效算法與實作範例

- **程式碼**：
    
    ```cpp
    bool isPrime(int n) {
        if (n <= 1) return false;
        if (n <= 3) return true;
        if (n % 2 == 0 || n % 3 == 0) return false;
        for (int i = 5; i * i <= n; i += 6) {
            if (n % i == 0 || n % (i + 2) == 0) return false;
        }
        return true;
    }
    
    ```