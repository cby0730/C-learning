# 進位制轉換（短除法程式碼）

## Decimal, Binary, Hex 程式碼實現短除法

### 各進制的定義與轉換方法

- **十進制（Decimal）**：基數為10。
- **二進制（Binary）**：基數為2。
- **十六進制（Hex）**：基數為16，使用0-9和A-F。

### 短除法的實作方式與範例代碼

- **原理**：反覆除以目標進制，取餘數作為下一位。
- **十進制轉二進制**：
    
    ```cpp
    std::string decimalToBinary(int num) {
        std::string binary;
        while (num > 0) {
            binary = std::to_string(num % 2) + binary;
            num /= 2;
        }
        return binary;
    }
    ```
    
- **十進制轉十六進制**：
    
    ```cpp
    std::string decimalToHex(int num) {
        const char hexDigits[] = "0123456789ABCDEF";
        std::string hex;
        while (num > 0) {
            hex = hexDigits[num % 16] + hex;
            num /= 16;
        }
        return hex;
    }
    ```
    
- 二進位轉十進位：
    
    ```cpp
    long long binaryToDecimal(const std::string& binary) {
        long long decimal = 0;
        unsigned int power = 0;
        int length = binary.length();
    
        for (int i = length - 1; i >= 0; --i) {
            if (binary[i] == '1') {
                decimal += static_cast<long long>(pow(2, power));
            } // if 
            else if (binary[i] != '0') {
    	        std::cerr << "Invalid char" << std::endl;
    	      } // else
    	      
    	      ++ power;
        }
    
        return decimal;
    }
    ```