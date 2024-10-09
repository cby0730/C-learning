# Big Endian & Little Endian

# **Big Endian 與 Little Endian 的定義、解釋與判別**

## **定義**

### **Big Endian（大端序）**

- 高位元組（MSB，Most Significant Byte）存放在低位記憶體位址。
- 低位元組（LSB，Least Significant Byte）存放在高位記憶體位址。

### **Little Endian（小端序）**

- 低位元組（LSB）存放在低位記憶體位址。
- 高位元組（MSB）存放在高位記憶體位址。

## **白話**

想像一個4位元組的整數 `0x12345678`，需要存入記憶體中。端序決定了這四個位元組在記憶體中的存放順序。

### **Big Endian**

```
位址   數據
0x00   0x12
0x01   0x34
0x02   0x56
0x03   0x78
```

### **Little Endian**

```
位址   數據
0x00   0x78
0x01   0x56
0x02   0x34
0x03   0x12
```

簡而言之，Big Endian 像是我們平常閱讀數字的順序，而 Little Endian 則是反過來存放。

### **如何辨別兩者**

可以通過檢查多位元組數據在記憶體中的位元組順序來判斷系統的端序。

## **C++ 函數判別**

```cpp
#include <iostream>

bool isLittleEndian() {
    unsigned int num = 1;
    char *ptr = (char*)&num;
    return (*ptr == 1);
}

int main() {
    if (isLittleEndian()) {
        std::cout << "此系統為 Little Endian（小端序）" << std::endl;
    } else {
        std::cout << "此系統為 Big Endian（大端序）" << std::endl;
    }
    return 0;
}

```

### **說明：**

- 我們定義了一個 `unsigned int`，數值為 `1`。
- 然後將其位址轉換為 `char*`，以便逐位元組訪問。
- 如果第一個位元組的值為 `1`，表示最低有效位元組在低位址，則為 Little Endian。